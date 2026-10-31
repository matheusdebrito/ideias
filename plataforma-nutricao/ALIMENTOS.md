# Alimentos e cálculos nutricionais — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Módulo:** MVP
> **Etapa:** 4 de 10 — Base de alimentos e cálculos nutricionais (planejamento funcional concluído; fonte adiada para etapa 9)
> **Versão:** 1.2
> **Data:** 31/10/2026
> **Fonte da visão geral:** [IDEA.md](./IDEA.md)
> **Decisões confirmadas** são distintas de **propostas a validar**.

## 1. Objetivo

**Regra transversal atualizada:** qualquer usuário pode cadastrar alimentos privados; **qualquer gestor assinante**, mesmo sem CRN, pode incorporar alimentos autorizados em planos compartilhados com usuários vinculados. O selo **CRN verificado** é opcional e não altera acesso a alimentos ou bibliotecas. A aplicação organiza dados nutricionais e não valida condutas clínicas.

Permitir selecionar alimentos de um catálogo nutricional rastreável e registrar alimentos não encontrados, mantendo unidades de porção, fonte dos dados e histórico coerentes.

## 2. Decisões confirmadas para o MVP

- Usuários comuns e gestores poderão criar **alimentos personalizados**.
- Isso inclui alimentos industrializados e produtos cujos dados sejam informados com base na tabela nutricional do rótulo.
- **Cada alimento personalizado é privado ao criador** (usuário comum ou gestor), sem publicação ou descoberta em um catálogo comunitário no MVP.
- O criador pode utilizar o alimento em suas refeições, dietas e diário alimentar, conforme suas demais permissões de domínio.
- Quando o gestor utiliza um alimento privado em uma dieta compartilhada publicada para um usuário acompanhado, este vê **os dados preservados na refeição planejada**, incluindo nome, porção, origem declarada e nutrientes disponíveis. Isso não compartilha o registro do alimento no catálogo nem permite acesso direto ao cadastro original.
- Uma refeição copiada da dieta para o diário alimentar cria novos dados de consumo independentes; não transforma o alimento privado do profissional em alimento pessoal do usuário acompanhado.
- Um usuário não pode consultar, alterar, listar ou excluir alimentos privados de outro usuário pelo ID ou pela API. A autorização precisa ser garantida no servidor.
- Distinguir alimento de catálogo (com sua referência e versão de origem) de alimento personalizado com dados declarados pelo usuário. O fato de ser cadastrado não implica validação por entidade oficial.
- **No cadastro manual de alimento personalizado são obrigatórios:** nome, quantidade de referência (positiva), unidade de referência, energia (kcal), proteínas (g), carboidratos (g) e gorduras totais (g). É preciso informar explicitamente os quatro nutrientes essenciais; valor zero é permitido quando informado e coerente, mas ausência não é zero.
- Macronutrientes essenciais e energia não podem ficar em branco no alimento personalizado salvo; impedir salvamento e apontar precisamente os campos ausentes ou inválidos.
- Fibras, vitaminas, minerais e outros componentes são opcionais no cadastro manual. Nutriente opcional não informado deve permanecer **desconhecido**, não zero.
- Para totais de micronutrientes no plano ou diário, se qualquer item relevante não possuir o nutriente, indicar cobertura incompleta e não apresentar a soma parcial como total exato. Evitar reportar como zero valores desconhecidos.
- Verificar valores numéricos não negativos, quantidade de referência positiva e unidade válida. Pode-se sinalizar inconsistências relevantes entre energia e macros, mas **não exigir igualdade exata** entre kcal informada e kcal estimada a partir dos macronutrientes, considerando arredondamentos, fibra e fatores nutricionais.
- **Alimentos importados de fontes institucionais:** podem existir no catálogo e aparecer nos resultados de busca e na consulta nutricional mesmo se faltar energia, proteína, carboidrato ou gordura total, com esses campos explicitamente marcados como indisponíveis.
- **Bloqueio de uso:** se faltar algum dos quatro nutrientes essenciais no alimento importado, o sistema não permite adicioná-lo a dietas (pessoais ou profissionais), modelos de refeição ou registros de consumo no diário; a interface explica quais campos impedem o uso.
- Alimento institucional com energia e os três macronutrientes completos pode ser usado mesmo sem dados de fibra, vitaminas ou minerais. Quando um relatório agregar nutrientes desconhecidos, sinalizar que o total não é completo.
- A checagem de completude e o bloqueio devem ocorrer no backend, inclusive ao salvar uma refeição ou publicar uma dieta, não apenas na escolha visual. Não inferir nutrientes ausentes de outros componentes nem substituir lacuna por zero.
- **Distinção:** para alimentos privados cadastrados manualmente os quatro campos essenciais são obrigatórios já no salvamento; para alimentos importados incompletos, a visualização é permitida, mas o uso em cálculos de refeições é bloqueado.
- **Precisão e arredondamento dos cálculos (definido para o MVP):** usar valores de origem e razões de porção/conversão na precisão disponível, sem arredondar nutrientes a cada etapa da multiplicação ou soma. Aplicar arredondamento apenas para formatar resultados na interface; totais de refeição/dia devem ser obtidos dos valores não arredondados.
- **Apresentação:** energia (kcal) normalmente sem casas decimais ou com uma casa, dependendo do contexto; proteínas, carboidratos e gorduras (g) com uma casa decimal; micronutrientes com casas adequadas à unidade (mg ou µg), magnitude e precisão da fonte.
- Valores informados em rótulos ou tabelas têm precisão limitada; preservar valores/escala da fonte e não fabricar exatidão adicional. Divergência pequena entre a soma das linhas arredondadas visíveis e o total arredondado é admissível e não deve ser corrigida alterando os valores de origem.
- O algoritmo de cálculo deve ser determinístico e uniforme entre catálogo, refeições, dietas e diário. **Recomendação técnica ainda sujeita à arquitetura:** aritmética decimal ou representação fixa para evitar erros de ponto flutuante em cálculos repetidos; definir explicitamente o modo de arredondamento usado na apresentação.
- **Regra de consumo do MVP:** em dietas e diário, orientar pesagem e quantidade na **forma pronta para ser consumida**. Para alimentos preparados, apresentar por padrão as entradas nutricionais da forma efetivamente consumida, como arroz cozido, feijão cozido, frango grelhado/assado e massa cozida.
- A regra não proíbe alimentos ingeridos crus: fruta, salada, castanhas e similares devem continuar disponíveis com sua composição correspondente.
- Diferentes formas de preparo podem ter nutrientes por 100 g distintos; preservar a forma de preparo quando houver diferença nutricional relevante. A presença da versão crua de um alimento que normalmente é cozido na fonte não a torna a opção padrão no plano ou diário.
- Nunca calcular a porção de alimento pronto usando valores nutricionais por 100 g da versão crua, nem assumir transformação entre pesos crus e cozidos com equivalência universal. Preparação que incorpora óleo ou outros ingredientes precisa de composição adequada para aquele preparo; cálculo a partir de ingredientes e rendimentos é evolução futura a avaliar.
- Alimentos crus da base podem permanecer como dados de origem, sem aparecer como opções principais de consumo para alimentos que exigem cocção. Não excluir alimentos que efetivamente são consumidos crus.
- **Identificação por marca (confirmado no MVP):** alimentos comuns/genéricos podem ter marca ausente (arroz branco cozido, banana etc.); produtos industrializados podem identificar marca e variante/apresentação relevante, facilitando a escolha de composição nutricional correspondente.
- **Pesquisa:** mostrar nome legível e, quando houver, marca e variante, sem poluir os resultados para alimentos não industrializados. Produtos de marcas distintas não devem ser tratados como nutricionalmente equivalentes apenas porque compartilham um nome genérico.
- Marca é campo opcional no modelo de alimento; pode ser preenchida no cadastro manual de produtos industrializados. Não exigir marca de um alimento genérico nem presumir que uma marca, sozinha, identifique unicamente o produto.
- **Duplicidade de alimentos personalizados (definido para o MVP):** ao tentar cadastrar um alimento, pesquisar nomes semelhantes **somente entre os alimentos privados do criador** e exibir sugestões para revisão antes de salvar.
- A sugestão é **informativa e não bloqueante**: o usuário pode escolher um alimento já existente ou continuar o cadastro de outro registro. Nunca mesclar, sobrescrever ou apagar registros automaticamente.
- Nomes semelhantes não são prova de identidade; marca, apresentação/variante, forma de preparo e composição nutricional podem diferir.
- O mecanismo de comparação/sugestão deve respeitar o isolamento de dados: não pesquisar nem revelar alimentos privados cadastrados por outras contas. A técnica exata de similaridade ficará para a etapa de arquitetura, sem usar um nome normalizado como identificador único.
- O alimento selecionado terá quantidade e unidade para o cálculo proporcional.
- **Referências nutricionais aceitas no cadastro (definido para o MVP):** 100 g, 100 mL ou porção declarada, sempre com quantidade de referência positiva e unidade explícita. Para uma porção declarada, armazenar seu significado (por exemplo, porção de 30 g ou uma unidade), sem presumir que toda porção seja igual.
- Os nutrientes de uma quantidade consumida são proporcionais aos nutrientes da quantidade de referência, após uma conversão autorizada. Exemplo: 120 kcal para uma porção de 30 g implicam 240 kcal para 60 g.
- Conversões na mesma grandeza física podem usar unidades equivalentes (por exemplo, g e kg; mL e L). Conversão entre massa e volume exige densidade conhecida para o alimento; entre unidade/colher/xícara e g/mL exige fator específico e rastreável para aquele alimento e preparação.
- Medidas caseiras são opcionais e só devem ser oferecidas como seleção se a conversão para a referência nutricional for conhecida. Na ausência da conversão, impedir cálculo nessa medida, mostrar explicação e oferecer uma unidade compatível.
- Registrar os valores de referência, origem da equivalência e fator de conversão usados no cálculo e preservar o retrato no histórico; mudanças posteriores em porções/equivalências não alteram dieta publicada nem consumo registrado.
- Alterações posteriores no cadastro de um alimento não podem reescrever o histórico de refeições consumidas ou dietas publicadas. Os registros de destino preservam a composição utilizada no momento da gravação.
- **Decisão adiada pelo usuário (31/10/2026):** a origem/fornecimento da base do catálogo será escolhida em etapa posterior, entre **TBCA, TACO ou arquivo JSON próprio**. Nenhuma opção está confirmada ou hierarquizada neste momento.
- **JSON representa um formato de entrega/armazenamento, não garante origem própria ou licença livre**. Informar a fonte original de cada composição, sua versão, unidade e direitos de uso. A aplicação pode importar JSON mesmo que os dados nele venham de uma das tabelas, mas a licença da origem continua valendo.
- Projetar o catálogo e a interface de seleção de alimentos de modo independente do mecanismo de importação ou da fonte. **Retomar decisão na etapa 9 (arquitetura e integrações), antes de desenvolver/importar o catálogo definitivo**; a disponibilidade de fonte comercialmente utilizável continua sendo requisito de lançamento.
- **Risco confirmado por consulta aos portais oficiais em 31/10/2026:** a [TBCA/USP](https://www.tbca.net.br/index.html) orienta contatar seus coordenadores para fins comerciais e publica restrições de reprodução/comercialização. O acesso livre online não autoriza assumir que a base pode ser copiada, transformada ou distribuída dentro da plataforma paga.
- **Ação necessária antes de importar/redistribuir dados TBCA:** obter posição formal dos responsáveis sobre uso comercial, armazenamento próprio, API, disponibilização a assinantes, cálculo de nutrientes, requisitos de atribuição e condições financeiras, caso existam. A eventual dispensa de citar para o cálculo de informação nutricional não equivale a permissão ampla de uso comercial.
- **TACO/NEPA-UNICAMP:** a [publicação oficial da 4ª edição](https://nepa.unicamp.br/tabela-brasileira-de-composicao-de-alimentos-4a-edicao/) está disponível para consulta, mas ainda é necessário verificar as condições de uso dos dados em um serviço comercial; não confundir acesso público com licença de redistribuição.
- **Regra de governança:** não realizar importação definitiva/replicação para produção comercial antes da liberação jurídica específica da(s) fonte(s). Manter o módulo desacoplado de qualquer provedor único.

## 3. Proposta de experiência (a validar)

1. Buscar um alimento existente.
2. Caso não exista, oferecer a ação **Cadastrar alimento**.
3. Solicitar nome, quantidade e unidade de referência (100 g, 100 mL ou porção declarada com dimensão definida), kcal, proteínas, carboidratos e gorduras totais; oferecer campos opcionais para fibras, vitaminas, minerais e demais componentes, conforme o rótulo.
4. Mostrar a origem declarada, em contraste com alimentos de fontes de composição nutricional institucional.
5. Salvar alimento e disponibilizá-lo para seleção segundo as permissões de acesso.

**Cuidados técnicos propostos:** validar números e unidades; evitar divisão por zero; não inferir nutrientes ausentes; preservar origem, quantidade de referência, data e valores de cada cálculo; evitar duplicar alimento no diário ao repetir uma requisição.

## 4. Decisões em aberto — não presumir definição

1. **Decisão postergada para a etapa 9:** avaliar TBCA, TACO ou arquivo JSON próprio; confirmar origem, licença, cobertura, formato e estratégia de importação antes da implementação definitiva.
2. Qual algoritmo de similaridade, modo de arredondamento decimal e precisão interna serão usados na implementação (etapa 9)?
3. Como tratar a permanência dos dados já publicados quando o criador apagar um alimento privado ou quando o vínculo profissional for encerrado (etapas 8 e 9)?

## 5. Critérios de aceitação propostos

| Cenário | Resultado esperado |
| --- | --- |
| Usuário ou gestor não encontra alimento no catálogo | Pode cadastrar um alimento próprio |
| Usuário pesquisa arroz branco cozido | Encontra alimento genérico sem marca obrigatória |
| Usuário pesquisa iogurte industrializado | Resultados podem distinguir marca e variante/apresentação quando cadastradas |
| Dois iogurtes possuem nomes semelhantes e marcas diferentes | Mantêm registros e composições nutricionais independentes; não são agrupados automaticamente |
| Usuário cadastra alimento genérico sem marca | Cadastro não é rejeitado por ausência de marca |
| Usuário tenta cadastrar alimento com nome semelhante a outro de seu catálogo | Sugestão privada de possível duplicidade é mostrada antes da confirmação |
| Usuário confirma cadastro mesmo após alerta de possível duplicidade | O novo alimento é salvo sem alterar, apagar ou mesclar o registro anterior |
| Dois usuários diferentes cadastram alimentos com nomes semelhantes | Nenhum vê alimentos privados do outro durante a verificação |
| Alimentos têm nomes parecidos, mas marcas ou preparos diferentes | A sugestão não é tratada como prova de equivalência nutricional; cadastro distinto continua permitido |
| Usuário pesquisa arroz para plano publicado | Exibir arroz preparado/cozido como opção padrão; orientar pesagem após preparo |
| Usuário pesquisa frango para consumo | Oferecer formas de preparo correspondentes aos valores nutricionais; não calcular frango grelhado como se fosse cru |
| Usuário seleciona banana ou alface crua | Permitir normalmente, pois o alimento é consumido nessa forma |
| Fonte nutricional disponibiliza arroz cru | Manter como referência quando necessário, mas não sugerir como porção principal do prato pronto |
| Usuário informa 100 g de arroz cozido | Não utilizar os dados por 100 g de arroz cru nem inferir conversão sem referência específica |
| Usuário cadastra produto conforme rótulo | Fonte aparece como declarada pelo usuário, não como TBCA |
| Usuário tenta consultar via API alimento privado de outra pessoa | Backend nega acesso |
| Gestor usa alimento privado em dieta publicada | Usuário acompanhado vê os dados incorporados na dieta, mas não obtém acesso ao catálogo privado do gestor |
| Usuário acompanhado registra no diário refeição de dieta com alimento privado do gestor | Diário recebe cópia independente sem dar acesso ao cadastro privado original |
| Cadastro não informa determinado micronutriente | A ausência é preservada e não aparece como valor zero |
| Usuário tenta salvar alimento personalizado sem kcal, proteínas, carboidratos ou gorduras totais | Sistema não salva e indica os campos essenciais faltantes |
| Usuário informa valor zero em proteína, carboidrato ou gordura | Zero informado pode ser aceito; não confundir com campo vazio |
| Alimento tem micronutrientes desconhecidos e entra em refeição | Refeição pode ser utilizada; agregados desse nutriente são explicitamente incompletos |
| Alimento importado de tabela institucional não tem gordura total | Aparece na busca e pode ser consultado, com gordura total indisponível; seleção em dieta, modelo ou diário é bloqueada |
| Alimento importado contém kcal, proteínas, carboidratos e gorduras, mas não vitamina C | Pode ser selecionado; relatório de vitamina C sinaliza cobertura incompleta |
| Cliente tenta enviar via API refeição com alimento institucional sem um valor essencial | Backend rejeita a operação e indica o nutriente ausente |
| Energia informada difere levemente do cálculo simplificado 4-4-9 | Não bloquear por diferença exata: validar apenas inconsistências evidentes, respeitando arredondamentos e fatores de conversão |
| Dieta publicada usa alimento que depois é editado | Dieta publicada mantém seu retrato histórico |
| Diário registra alimento que depois é editado | Registro de consumo mantém seus valores originais |
| Um alimento tem origem e porção de referência distintas de outro | Cálculo respeita cada origem e sua unidade, sem converter silenciosamente |
| Produto tem 120 kcal em uma porção de 30 g e o consumo é de 60 g | Cálculo proporcional retorna 240 kcal e aplica o mesmo fator aos nutrientes declarados |
| Cadastro usa 100 mL, mas usuário informa 50 g sem densidade do alimento | Conversão é recusada com orientação; não se presume 1 g = 1 mL |
| Usuário tenta selecionar colher ou xícara sem equivalência específica | A medida não é oferecida como conversível; app sugere medida compatível |
| Um fator de equivalência muda após registrar consumo | O histórico conserva o fator usado originalmente e seus valores calculados |
| Três alimentos têm 12,46 g de proteínas cada | Total antes da apresentação é 37,38 g; interface mostra 37,4 g (não 37,5 g por somar parcelas arredondadas) |
| Soma visual de itens arredondados difere ligeiramente do total apresentado | Total representa a soma original calculada; não distorcer valores para forçar igualdade visual |
| Micronutriente vem com precisão limitada na fonte | Exibir apenas casas compatíveis com a unidade e a precisão do dado, sem falsa exatidão |
| Mesmos alimentos e quantidades são usados em biblioteca, dieta e diário | Regra de cálculo e formatação é consistente em todas as áreas |

## 6. Dependências da etapa

A escolha da fonte nutricional (TBCA, TACO ou JSON próprio) foi adiada; seguir o planejamento funcional sem eleger uma fonte ou travar a etapa 4 por isso. Antes da importação definitiva e do lançamento comercial, confirmar procedência, viabilidade jurídica e qualidade da fonte selecionada. **Privacidade do catálogo personalizado definida:** dados privados do criador e somente retratos de nutrientes/nome/porção em dietas publicadas, com autorização no servidor. Não existe catálogo público colaborativo no MVP; eventual mudança exigirá nova decisão de produto e revisão de qualidade e privacidade.

---

**Conclusão da etapa 4 (31/10/2026):** regras funcionais principais do catálogo e dos cálculos nutricionais aprovadas. A escolha da fonte do catálogo e os detalhes técnicos da importação permanecem para a etapa 9; exclusão e retenção ficam nas etapas 8 e 9. Isso não significa que a funcionalidade foi implementada.

Este é um documento de planejamento; não há funcionalidade implementada no repositório de ideias.
