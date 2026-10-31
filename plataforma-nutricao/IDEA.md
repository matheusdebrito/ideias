# Plataforma de Nutrição — Documento de ideia

> **Status:** descoberta do produto  
> **Versão:** 0.72  
> **Última atualização:** 31/10/2026  
> **Este documento registra a visão inicial. Não é uma especificação pronta para implementação.** Decisões ainda não validadas estão marcadas como questões em aberto.

## 1. Visão do produto

Criar uma plataforma de nutrição com duas experiências integradas:

1. **Autonomia alimentar:** permitir que pessoas registrem o que comem, acompanhem a composição nutricional da alimentação, organizem metas pessoais e consultem seu histórico.
2. **Ferramenta profissional:** permitir que nutricionistas gerenciem pacientes, acompanhem informações compartilhadas por eles e elaborem planos alimentares individualizados.

O mesmo usuário deve poder utilizar os recursos pessoais e, se também for nutricionista, os recursos profissionais sem precisar manter contas desconectadas.

A plataforma é uma ferramenta de organização e acompanhamento alimentar, não uma substituta do nutricionista. O produto atende dois públicos centrais: pessoas que desejam organizar a própria alimentação e nutricionistas que precisam de ferramentas para gerir pacientes, elaborar planos e acompanhar resultados. O usuário comum poderá montar livremente seus próprios planos alimentares. Isso deve ser diferenciado de uma prescrição dietética profissional ou de recomendações clínicas para tratar condições de saúde.

## 2. Públicos e perfis

**Terminologia do MVP — confirmado:** a plataforma tem **um único tipo de usuário para o uso pessoal**, com ou sem vínculo com nutricionista. **“Paciente” não é outra conta, outro perfil, outro papel obrigatório nem um plano distinto:** é apenas a denominação contextual usada **pelo nutricionista em sua área de gestão** (ex.: **Meus pacientes**) para identificar os usuários que ele acompanha ou acompanhou conforme permissões e histórico. Um convite aceito cria um **vínculo autorizado** entre usuário e nutricionista; não converte a conta em um cadastro especial de paciente.

**Linguagem de interface:** para a pessoa que usa o próprio diário e os recursos pessoais, usar **usuário**, **minha conta**, **minha alimentação**, **meus dados** ou **Meu nutricionista**, não exibir “paciente” como uma categoria de conta ou identificação própria. O termo “paciente” permanece adequado na **visão do nutricionista sobre as pessoas que gerencia**, sem ampliar acesso às contas. O profissional pode também utilizar os recursos pessoais na mesma conta, com capacidades profissionais condicionadas a CRN, assinatura e autorização pertinentes.

### 2.1. Usuário que acompanha a própria alimentação

Pessoa que deseja registrar refeições, consultar informações nutricionais, acompanhar peso, organizar seus próprios planos alimentares e observar o consumo em relação a metas estimadas ou definidas por ela.

Possíveis necessidades:
- Consultar alimentos e seus nutrientes.
- Registrar refeições e quantidades consumidas.
- Visualizar calorias e macronutrientes por refeição e por dia.
- Consultar históricos diário, semanal e mensal.
- Registrar peso ao longo do tempo.
- Consultar IMC, com explicação dos limites desse indicador.
- Montar e atualizar livremente planos alimentares pessoais, com alimentos, porções e refeições.
- Consultar estimativas nutricionais e energéticas com explicação de suas limitações.

### 2.2. Usuário com vínculo de acompanhamento (mesma conta pessoal)

É **o mesmo usuário da seção 2.1**, que opcionalmente aceita um convite e autoriza um nutricionista a consultar **somente as categorias de dados aprovadas** durante um vínculo profissional ativo. Não existe cadastro adicional, tipo de conta “paciente” ou mudança de identidade para receber acompanhamento. O nutricionista poderá chamar essa pessoa de **paciente** em sua própria área de gestão. No MVP, cada usuário poderá ter no máximo um nutricionista ativo; o acesso exige autorização vigente e cessa com o fim do vínculo.

Possíveis necessidades:
- Vincular-se a um nutricionista por convite e aceite explícito.
- Consultar o plano alimentar profissional vigente.
- Registrar a alimentação realmente consumida.
- Acompanhar a própria evolução.
- Saber quais informações estão sendo compartilhadas com o profissional.
- Encerrar o vínculo e revogar acessos conforme as regras do produto.

### 2.3. Nutricionista

Profissional que utiliza a plataforma para organizar o acompanhamento de seus pacientes.

Possíveis necessidades:
- Manter perfil profissional e informações de registro.
- Convidar pacientes e administrar vínculos.
- Consultar todos os dados disponíveis do paciente, enquanto houver vínculo ativo e autorização vigente.
- Acompanhar refeições, consumo energético, macronutrientes, peso e histórico.
- Elaborar planos alimentares e atribuí-los a pacientes.
- Atualizar planos e consultar versões anteriores.
- Acompanhar a diferença entre o plano prescrito e a alimentação registrada pelo paciente.

**Validação profissional confirmada no MVP:** no cadastro profissional solicitar **nome completo, número do CRN e Conselho Regional/região do registro**. A conta pode ser criada com status **Em validação**, mas convites, criação de vínculos, acesso profissional aos dados de pacientes, publicação de dietas e definição de metas profissionais só serão liberados após conferência da **titularidade e regularidade do CRN**. O usuário poderá usar os recursos pessoais enquanto aguarda. Meio de conferência, revalidação e tratamento de irregularidades serão especificados nas etapas 8/9; não presumir integração automática oficial.

## 3. Módulos funcionais propostos

### 3.1. Perfil e dados antropométricos

- Cadastro de data de nascimento, sexo e altura, conforme a necessidade dos cálculos e do acompanhamento.
- **Acompanhamento de peso no MVP (confirmado):** o usuário adulto pode registrar seu peso em kg quando quiser, informando a data da medição, sem frequência mínima ou obrigação de lançamento. Cada medição permanece como registro independente no histórico e aparece em um **gráfico de evolução de peso ao longo do tempo**.
- **Correção de peso histórico (confirmado):** permitir corrigir o valor ou a data de uma medição anterior sem sobrescrever nem apagar as demais medições; alterações explícitas devem atualizar o gráfico de evolução. Regras de auditoria/retenção de correções de dados pessoais serão detalhadas nas etapas 8 e 9.
- **IMC para adultos (confirmado):** calcular o IMC atual a partir do **peso mais recente registrado** e da **altura informada no perfil**: IMC = peso (kg) ÷ [altura (m)]². Somente calcular com dados válidos, positivos e disponíveis; se faltar peso ou altura, mostrar indicador indisponível, não zero.
- **Limites do IMC:** apresentar como indicador geral, **não diagnóstico**, medida isolada de composição corporal, ou substituto de avaliação nutricional individualizada. Evitar linguagem de julgamento ou conclusões clínicas automatizadas.
- Caso a altura do perfil seja alterada, a regra técnica para eventuais IMCs históricos deve considerar a altura aplicável a cada data, sem reescrever medições de peso originais; detalhamento na etapa 9.
- O cadastro, o acompanhamento e o cálculo do IMC serão disponibilizados apenas para pessoas com **18 anos completos ou mais** no MVP.

**Decisão de produto — escopo adulto:** a plataforma será exclusiva para maiores de idade na primeira versão. Não haverá cadastro de pacientes menores de 18 anos, inclusive por nutricionistas, nem perfis dependentes, nem recursos pediátricos. A data de nascimento informada deverá ser validada no servidor com base na data corrente; contas abaixo de 18 anos devem ter cadastro rejeitado.

**Ressalva jurídica:** informar que o produto é 18+ e coletar data de nascimento não transfere automaticamente ao usuário toda a responsabilidade caso ele forneça dados falsos. Avaliar os riscos de acesso provável por menores conforme ECA Digital, Decreto nº 12.880/2026 e orientações vigentes da ANPD. Definir controles proporcionais e tratamento de contas identificadas como pertencentes a menores. Isso não implica exigir documentos de todos os usuários sem necessidade.

### 3.2. Base de alimentos e composição nutricional

Manter um catálogo pesquisável de alimentos com, quando disponíveis:

- Energia (kcal).
- Carboidratos.
- Proteínas.
- Gorduras totais e, quando disponíveis, subdivisões.
- Fibras.
- Vitaminas.
- Minerais.
- Outros componentes nutricionais que sejam relevantes.

Requisitos importantes para a qualidade dos dados:
- Armazenar a composição por uma base de referência definida, preferencialmente por 100 g de parte comestível.
- Distinguir valores iguais a zero de nutrientes não analisados ou sem informação.
- Identificar o **estado real do alimento no momento de consumo** (cozido, assado, grelhado, frito ou naturalmente consumido cru), quando afetar a composição nutricional.
- **Padrão do MVP — peso pronto para consumo:** dietas e diário orientarão a pesar a porção **depois do preparo**, para alimentos que exigem preparo (arroz, feijão, carnes, massas etc.). Buscar e selecionar preferencialmente a entrada nutricional correspondente ao alimento preparado: arroz cozido, frango grelhado etc.
- **Não excluir todos os alimentos crus:** frutas, saladas, castanhas e outros alimentos efetivamente consumidos crus continuam válidos e calculados no estado de consumo.
- Para alimentos normalmente preparados, a variante crua não será a opção padrão para montagem da refeição. Se a fonte a contiver, ela poderá ser preservada como dado de referência, sem oferecê-la como alternativa principal de porção pronta; uso de ingredientes crus para cálculo de receitas fica fora do fluxo padrão do MVP e é possibilidade futura.
- **Não usar valores de alimento cru para calcular o peso do prato pronto.** A absorção/perda de água, óleo e outras mudanças no preparo precisam estar refletidas na entrada nutricional correspondente. Não converter automaticamente peso cru em peso cozido com fator genérico.
- **Marca (confirmado no MVP):** alimentos comuns e genéricos, como arroz branco cozido ou banana, podem existir sem marca. Produtos industrializados podem ter marca identificada e exibida junto ao nome, especialmente quando ela diferencia a composição nutricional (por exemplo, diferentes iogurtes, pães ou suplementos).
- A busca deve privilegiar descrições simples para alimentos comuns e diferenciar produtos industrializados por **nome, marca, apresentação/variante relevante e composição nutricional** quando disponíveis. Não misturar produtos de marcas diferentes como se tivessem os mesmos nutrientes; marca ausente não deve impedir o uso de alimento genérico.
- A presença da marca é informação descritiva do alimento, não um campo universalmente obrigatório.
- **Aviso de possíveis duplicidades no cadastro de alimentos personalizados (definido no MVP):** ao criar um alimento, verificar possíveis nomes semelhantes apenas **no catálogo privado do próprio criador**. Exibir candidatos existentes antes de confirmar o salvamento para que ele possa conferir ou selecionar um já cadastrado.
- **Aviso, não bloqueio:** permitir que o criador prossiga e salve um registro distinto mesmo com nome semelhante; não fundir, substituir ou atualizar automaticamente alimentos já existentes. Marca, variante, preparo e composição podem justificar registros diferentes.
- A busca por possíveis duplicados nunca expõe cadastros privados de outras pessoas e não altera registros de dieta ou diário já salvos.
- Registrar a fonte, a versão e a data de referência dos dados.
- Não misturar valores de fontes diferentes sem identificar sua procedência.
- Permitir conversão de quantidades e medidas caseiras com regras explícitas.
- **Porções e unidades (confirmado no MVP):** aceitar composição nutricional com base em **100 g, 100 mL ou uma porção declarada**, com quantidade e unidade de referência explícitas. Uma porção declarada pode ter equivalência documentada em massa ou volume, quando conhecida; sem equivalência, somente cálculos proporcionais na própria unidade de porção.
- Os cálculos são proporcionais à quantidade informada, respeitando a dimensão e a referência do alimento. Exemplo: um rótulo com 120 kcal em 30 g equivale a 240 kcal em 60 g.
- Permitir medidas caseiras (colher, xícara, unidade etc.) **apenas quando houver equivalência conhecida para o alimento e sua forma de preparo**, com fonte registrada.
- Não converter g em mL, ou unidades/medidas caseiras em massa ou volume, sem uma densidade ou equivalência explícita e aplicável àquele alimento. Nunca inventar equivalências universais.
- Preservar a quantidade original, unidade, referência nutricional e fator de conversão aplicado nas versões publicadas e nos registros de consumo.
- Preservar a rastreabilidade dos dados usados nos cálculos.
- **Precisão e arredondamento (confirmado para o MVP):** usar os valores nutricionais de referência e fatores de porção/conversão sem arredondamentos intermediários; somar valores não arredondados ao calcular item, refeição e dia e **arredondar somente para apresentação**.
- Exibir calorias geralmente sem casas decimais ou com uma casa quando o contexto justificar; macronutrientes em gramas com uma casa decimal; micronutrientes com precisão adequada à unidade (mg/µg) e aos dados originais, evitando apresentar precisão artificial.
- Preservar os valores e a precisão disponíveis na fonte, de modo que cálculo numérico mais preciso **não signifique maior exatidão nutricional**. O total calculado a partir dos valores integrais poderá diferir em alguns décimos da soma de parcelas já arredondadas na tela.
- Adotar regras numéricas e de formatação consistentes entre biblioteca, dietas, diário e indicadores, inclusive ao preservar retratos históricos.

**Escolha da base adiada por decisão do produto (31/10/2026):** o catálogo inicial poderá ser abastecido por **TBCA, TACO ou um arquivo JSON próprio**; nenhuma dessas alternativas foi aprovada ainda. A decisão ocorrerá na etapa 9 (arquitetura e integrações) ou antes da implementação efetiva do importador, considerando viabilidade comercial, cobertura e qualidade.
- **JSON é o formato do arquivo, não a origem nutricional**: mesmo nesse caso, registrar procedência, versão, unidades e direitos de uso dos dados que compõem o arquivo.
- Até essa decisão, especificar regras funcionais de catálogo, cálculo, porção, histórico e acesso de forma **independente da fonte**; nenhuma implementação ou importação de base institucional está autorizada por esta definição.
- Se for escolhida uma base de terceiros, confirmar condições de uso e eventual autorização antes da integração comercial.

**Risco de licenciamento identificado na pesquisa de 31/10/2026 (não é autorização de uso):**
- A [página oficial da TBCA](https://www.tbca.net.br/index.html) informa que é necessário entrar em contato com os coordenadores para fins comerciais e apresenta restrições à reprodução e comercialização do material. Portanto, **não importar, replicar ou distribuir os dados TBCA comercialmente** com base apenas no acesso público à página. Buscar autorização expressa e definir exatamente as modalidades permitidas (armazenamento, API, exibição, cálculos, atualizações e atribuição).
- A [TACO disponibilizada pelo NEPA/UNICAMP](https://nepa.unicamp.br/tabela-brasileira-de-composicao-de-alimentos-4a-edicao/) pode ser alternativa ou complemento, mas os termos específicos de reutilização e distribuição em aplicativo pago **ainda não foram confirmados** e requerem análise/contato com os responsáveis.
- Até a regularização, o desenho do produto deve admitir troca de fonte e não pressupor acesso livre comercial à TBCA. A liberação jurídica é **condição para integrar qualquer base de terceiros ao produto comercial**; esse levantamento não substitui orientação jurídica.

**Alimentos personalizados (confirmado para o MVP):**
- Usuários comuns e nutricionistas poderão **cadastrar alimentos que não encontrem no catálogo de referência**, inclusive alimentos industrializados.
- O cadastro poderá usar informações declaradas pelo próprio usuário, como valores do rótulo nutricional, com identificação clara da origem declarada; esses dados não devem aparecer falsamente como validados pela TBCA.
- **Campos obrigatórios para salvar alimentos personalizados no MVP:** nome, quantidade de referência maior que zero, unidade de referência, energia (kcal), proteínas (g), carboidratos (g) e gorduras totais (g). Os quatro valores nutricionais essenciais devem ser explicitamente preenchidos, aceitando zero quando informado; não aceitar valores ausentes ou negativos.
- Fibras, vitaminas, minerais e outros micronutrientes/subdivisões são opcionais no cadastro manual. Ausência não significa zero e relatórios que dependam desses nutrientes devem indicar cobertura incompleta, não apresentar o total parcial como completo.
- Validar unidades, quantidades e inconsistências evidentes, sem exigir igualdade exata entre energia informada e a soma simplificada derivada de macros (rótulos, fibras, fatores de conversão e arredondamentos podem diferir).
- **Alimentos importados de bases institucionais (confirmado no MVP):** podem continuar **pesquisáveis e consultáveis** mesmo quando faltar algum dos quatro dados essenciais (energia, proteínas, carboidratos, gorduras totais), porém não poderão ser selecionados para dietas, modelos de refeição ou diário alimentar enquanto estiverem incompletos. A interface identificará os campos ausentes e o motivo do bloqueio.
- Alimentos de fontes institucionais que tenham os quatro dados essenciais completos poderão ser utilizados mesmo com fibras, vitaminas ou minerais ausentes; relatórios desses nutrientes indicarão cobertura incompleta.
- Não completar lacunas nutricionais das tabelas institucionais por suposição nem tratar ausência como zero; a eventual revisão dos dados deve ser rastreável por fonte e versão.
- A regra de **cadastro manual personalizado** permanece mais exigente no salvamento: não permitir gravar um alimento privado sem os quatro dados essenciais.
- O alimento personalizado poderá ser selecionado na criação de refeições, dietas e registros de consumo conforme as permissões do seu proprietário.
- Quantidades, unidade-base e valores informados devem ser explicitados para calcular a porção corretamente. Nutriente sem valor informado deve permanecer **desconhecido**, não assumir zero.
- Versões de dietas publicadas e refeições consumidas preservam um retrato dos valores utilizados e não são alteradas quando o alimento personalizado for editado.
- **Privacidade e visibilidade (confirmado no MVP):** cada alimento personalizado é **privado ao seu criador** (usuário comum ou nutricionista). Não existe catálogo comunitário de alimentos personalizados no MVP.
- Se um nutricionista usar seu alimento personalizado em um plano profissional publicado, o paciente poderá **consultar a descrição e os valores nutricionais incorporados àquela dieta**, sem ganhar acesso ao cadastro original no catálogo privado do profissional, nem permissão para editá-lo ou reutilizá-lo como alimento próprio.
- Caso o paciente registre uma refeição a partir do plano, será criada uma cópia independente dos valores necessários para registrar o consumo, sem criar acesso ao alimento privado do nutricionista.
- O controle de acesso deve ser aplicado pelo servidor; conhecer o identificador de um alimento privado não concede permissão de leitura ou edição.

Detalhamento funcional e questões da etapa 4 em [ALIMENTOS.md](./ALIMENTOS.md).

### 3.3. Diário alimentar

- Registro de refeições com data e horário e organização por café da manhã, almoço, jantar, lanches etc.
- **Lançamento retroativo (confirmado no MVP):** permitir adicionar hoje uma refeição consumida em um dia anterior, escolhendo a data e o horário efetivos de consumo. O formulário sugere a data/hora atuais para evitar trabalho adicional, mas permite substituí-las antes de confirmar.
- O lançamento deve compor os totais nutricionais, indicadores e consultas **do dia do consumo informado**, e não do dia em que o usuário digitou ou salvou a informação. Registrar separadamente data/hora declarada de consumo e instante de criação para rastreabilidade.
- Para lançamentos retroativos, comparar o consumo à meta que estava efetivamente vigente **na data de consumo**, sem aplicar retroativamente a meta atual nem alterar prescrições e históricos anteriores. Recalcular agregados das datas atingidas quando o registro for criado, corrigido ou excluído.
- **Fuso horário do diário (confirmado para o MVP):** ao abrir o registro, usar como padrão o fuso configurado no navegador/dispositivo, obtido por funcionalidade nativa (por exemplo, `Intl.DateTimeFormat().resolvedOptions().timeZone`); **não solicitar GPS, geolocalização nem serviço externo** para determinar o fuso.
- Permitir **corrigir manualmente** o fuso associado a cada refeição, sobretudo em lançamentos retroativos ou em viagem. O navegador informa o fuso configurado no dispositivo, não comprova a localização física do usuário; quando não houver fuso válido, oferecer escolha manual.
- Preservar junto ao lançamento a **data/hora local declarada de consumo, o fuso IANA selecionado, o instante equivalente em UTC (quando determinado de forma inequívoca) e a data/hora independente de criação do registro**. O dia nutricional é a data local declarada no fuso do consumo, inclusive perto da meia-noite; mudar depois o fuso do aparelho não deve deslocar registros antigos para outro dia.
- Novas refeições usam o fuso atual detectado; registros antigos retêm seu contexto original. Mudanças explícitas de data/hora ou fuso na edição atualizam os indicadores dos dias afetados, com histórico discreto de correção.
- **Pendente de arquitetura (etapa 9):** validação de identificadores IANA, representação de horários ambíguos/inexistentes por horário de verão, persistência dos instantes e conversões no backend. Não inferir fuso passado apenas pelo local de acesso atual.
- Inclusão de alimentos, quantidades e unidades realmente consumidas.
- Cálculo de calorias, proteínas, carboidratos, gorduras e outros nutrientes disponíveis.
- Totalização por refeição e dia; consulta, correção explícita e histórico.
- **Correções do diário (definido no MVP):** o dono do diário pode **editar ou excluir uma refeição já registrada**, incluindo corrigir alimentos, quantidades, unidades e data/horário de consumo, quando necessário. Não é necessário excluir e cadastrar toda a refeição novamente para corrigir uma quantidade.
- Ao corrigir, o sistema recalcula nutrientes da refeição a partir dos dados corrigidos e atualiza os totais/indicadores das datas afetadas; se a refeição for excluída, deixa de compor os totais de consumo. Nenhuma dessas ações edita a dieta profissional/pessoal ou modelos de refeição.
- **Histórico discreto de correções:** registrar operação, autor e instante da edição/exclusão e referências à alteração anterior/posterior na medida necessária, sem exigir justificativa e sem poluir a interface. Política de retenção, eliminação efetiva e quem pode consultar a trilha serão definidos nas etapas 8 e 9; exclusão no diário não é promessa de guarda indefinida de dados pessoais.
- Diferenciar atualização expressamente solicitada pelo usuário de atualização externa no catálogo/dieta: alterações externas nunca reescrevem o diário, mas correção autorizada do registro de consumo pode mudar seus nutrientes históricos de maneira auditável.

**Regra fundamental:** o diário registra **o que a pessoa efetivamente comeu**, independentemente de ter um plano alimentar ativo. Atualizar, encerrar ou trocar uma dieta não modifica registros anteriores de refeições consumidas.

Cada item consumido deve preservar quantidade, unidade, data/hora, composição nutricional calculada no momento do registro e referência/fonte do alimento. Isso evita que uma atualização futura do catálogo nutricional, independentemente de sua fonte, altere retroativamente os totais históricos.

### 3.3.1. Registro a partir de refeições e modelos (MVP)

O usuário poderá registrar seu consumo de três maneiras:
- **Escolher uma refeição da dieta vigente:** copiar alimentos e quantidades planejadas para um registro de consumo editável.
- **Escolher uma refeição da biblioteca pessoal:** usar modelo de refeição previamente salvo, sem modificá-lo.
- **Criar uma refeição avulsa:** cadastrar alimentos e quantidades manualmente; o registro entra no diário, mas só se torna modelo reutilizável se o usuário escolher explicitamente **salvar para reutilizar**.

Em qualquer fluxo, o usuário poderá ajustar a refeição antes de confirmá-la. A confirmação salva uma **cópia independente** no diário; alterar a dieta ou o modelo não altera o histórico de consumo. Evitar duplicação de registro se a mesma confirmação for reenviada.

**Biblioteca profissional no MVP:** o nutricionista cria refeições adicionando alimentos do catálogo e informando quantidades, sem digitar manualmente calorias ou macros. A aplicação calcula automaticamente a composição nutricional a partir dos dados da fonte nutricional escolhida (ainda não definida). O nutricionista poderá criar, consultar, editar, excluir e reutilizar seus próprios modelos de refeições ao montar dietas de diferentes pacientes.

O nutricionista poderá selecionar uma refeição da biblioteca e adicioná-la à dieta de um paciente. A aplicação copia os itens e quantidades para o rascunho do plano escolhido; o profissional poderá ajustar essa cópia para o paciente. A refeição só será apresentada ao paciente quando a versão do plano for publicada. Também será possível criar uma refeição diretamente no rascunho e optar por salvá-la na biblioteca profissional.

O nutricionista define uma composição fixa para cada refeição: alimentos e quantidades determinados. O paciente não recebe listas de alimentos alternativos para escolher dentro da refeição no MVP. Se o nutricionista quiser oferecer outra combinação, deverá criar outra refeição fixa no plano ou atualizar a dieta por meio de uma nova versão. O paciente registra no diário o que realmente consumiu, mesmo que seja diferente do plano; isso não altera a prescrição.

**Especificação funcional:** [DIETAS.md](./DIETAS.md).

### 3.4. Metas e indicadores de consumo

- **Metas próprias sem nutricionista (confirmado para o MVP):** qualquer usuário adulto, inclusive na modalidade pessoal gratuita e sem vínculo profissional, pode cadastrar, consultar e alterar livremente suas metas de calorias (kcal), proteínas (g), carboidratos (g) e gorduras totais (g). Não exigir nutricionista, dieta ativa nem assinatura profissional para gerenciá-las.
- As metas são **opcionais e independentes por nutriente**: o usuário escolhe quais deseja preencher, sem obrigatoriedade de definir os quatro valores em conjunto. Não inventar metas para campos não definidos e não transformar metas pessoais em prescrições clínicas.
- Nutricionistas com autorização poderão definir metas profissionais para pacientes; o paciente pode ajustá-las seguindo o fluxo de confirmação já aprovado, preservando sempre a referência profissional original. Metas pessoais sem nutricionista não exigem o aviso específico de alteração de uma meta prescrita. **Após o fim ou a troca do vínculo, a última meta efetiva continua como referência pessoal**, com autoria histórica preservada e sem ser rotulada como prescrição vigente do ex-profissional.
- Exibir, para cada nutriente com meta vigente, consumo registrado e diferença em relação à meta aplicável à data consultada. Sem meta definida para determinado nutriente, apresentar apenas seu consumo registrado, sem saldo ou rótulo de desvio.
- Persistir autor/origem e início de vigência das metas para consulta histórica. Mudanças posteriores, pessoais ou profissionais, não reescrevem metas efetivas dos dias passados.
- A consistência entre a meta energética e a soma simplificada calculada dos macros pode ser informativa, mas não deve bloquear o cadastro automaticamente; fontes, fibras, arredondamentos e objetivos individualizados podem gerar diferenças.
- Comparação do consumo registrado com a meta válida naquele dia, sem tratar isso como diagnóstico.
- Exibição de calorias e macronutrientes por refeição e total diário.
- **Painel diário confirmado no MVP:** mostrar **calorias consumidas**, **meta calórica efetiva do dia**, **calorias restantes até a meta** e, caso a meta seja ultrapassada, **quanto o consumo ficou acima dela**. Exibir também **proteínas, carboidratos e gorduras totais consumidos** (em gramas) naquele dia.
- A diferença é calculada entre meta calórica efetiva **da data consultada** e total consumido: quando o consumo for menor ou igual à meta, apresentar valor restante não negativo; quando for maior, apresentar o excedente como **acima da meta**, sem confundir o usuário com um valor negativo de calorias restantes. Se a data não tiver meta válida, apresentar consumo normalmente e indicar **sem meta definida**, sem inventar saldo nem assumir meta zero.
- Atualizar o painel quando refeições forem criadas, corrigidas ou excluídas, inclusive lançamentos retroativos, usando a data local de consumo do registro e os valores nutricionais preservados em cada item. **Não arredondar antes de somar**; formatar valores na apresentação conforme as regras nutricionais já definidas.
- **Metas opcionais de macronutrientes aprovadas:** proteína, carboidrato e gordura podem ter objetivos próprios definidos pelo usuário ou nutricionista autorizado. O painel mostra os totais consumidos mesmo sem objetivos, e apresenta a comparação somente onde houver meta válida. Os gráficos semanais/mensais, contagem estrita de dias acima/abaixo/na meta e diferença acumulada do consumo registrado contra metas diárias também foram aprovados, sempre com indicação de cobertura parcial.
- Consulta de períodos anteriores, preservando as metas vigentes à época.
- **Alteração de meta definida pelo nutricionista (confirmado no MVP):** o paciente pode alterar a meta que utiliza para acompanhar calorias e macronutrientes, mesmo quando a referência atual veio de seu nutricionista. Antes de aplicar a alteração, a interface deve avisar explicitamente que a meta foi definida pelo profissional e pedir confirmação inequívoca; cancelar mantém a meta atual.
- A confirmação cria uma **meta de acompanhamento ajustada pelo paciente**, com autor/origem e vigência identificados, que se torna a referência efetiva dos indicadores a partir da alteração. A meta original prescrita pelo nutricionista **permanece preservada e consultável**, não é modificada e não pode ser falsamente atribuída ao profissional.
- Sem ajuste pessoal posterior, uma meta profissional vigente pode ser a referência padrão no acompanhamento; sem meta profissional vigente, usar a meta pessoal disponível. Comparações históricas respeitam a meta efetiva e a origem aplicáveis ao período, sem reescrever metas ou prescrição anteriores.
- **Visibilidade ao nutricionista confirmada para o MVP:** enquanto houver vínculo profissional ativo **e autorização vigente**, o nutricionista poderá consultar no painel do paciente (a) a meta originalmente prescrita, (b) a meta de acompanhamento efetivamente ajustada pelo paciente e (c) a data/hora e origem do ajuste. A interface deve distinguir claramente prescrição profissional e escolha pessoal, sem tratar a última como prescrição editada.
- O acesso a esses dados deve ser autorizado pelo servidor e cessar quando o vínculo ou a permissão terminar, ressalvadas obrigações legais de retenção a definir na etapa 8.
- **Nova meta profissional, sem trabalho adicional para o paciente (definido para o MVP):** quando uma nova meta de calorias ou macronutrientes definida pelo nutricionista entrar em vigor, ela se torna **automaticamente** a referência efetiva dos indicadores, mesmo se o paciente tiver ajustado a meta anterior. Não pedir aceitação ou confirmação do paciente para essa atualização profissional.
- Caso a vigência seja futura, manter a referência atual até o instante programado; alterar automaticamente quando chegar a data/hora definida, sem reescrever metas nem indicadores históricos.
- Apresentar apenas uma informação discreta e não bloqueante de que a meta foi atualizada pelo nutricionista. O paciente poderá ajustá-la novamente por iniciativa própria, com o aviso e a confirmação definidos anteriormente, sem alterar a prescrição original. Cada mudança preserva origem, vigência e autoria.
- **Aviso profissional dentro do painel (confirmado no MVP):** quando o paciente alterar sua meta de acompanhamento, o nutricionista poderá identificar a mudança por um **indicador visual discreto, não bloqueante**, no painel do paciente, apresentando meta profissional, meta ajustada, autoria e data/hora. Não exigir que o profissional confirme leitura para continuar trabalhando.
- **Sem notificações externas no MVP por esse evento:** não enviar e-mail, mensagem ou push automático ao nutricionista apenas porque o paciente modificou sua meta. O aviso é restrito à interface da aplicação e exige vínculo profissional ativo e autorização vigente para ser visualizado.
- Preservar a separação entre ajuste do paciente e prescrição profissional, evitando comunicar o evento como se o profissional tivesse atualizado a dieta.
- **Resumos semanais e mensais (confirmado no MVP):** mostrar o **total registrado** de calorias, proteínas, carboidratos e gorduras, a **média diária desses nutrientes apenas sobre os dias que têm ao menos uma refeição registrada** e a **quantidade de dias com registros** no período selecionado.
- **Dia sem registros não é dia com consumo zero**: excluir dias sem lançamentos do divisor da média, não declarar que houve jejum ou consumo nulo e não apresentar os totais registrados como retrato completo do consumo real. Dias com registros podem estar incompletos, por isso identificar as médias como **médias dos dias registrados**.
- **Cálculo:** total do período = soma dos valores nutricionais das refeições registradas no período; média por dia registrado = total / quantidade de datas locais distintas que possuem ao menos uma refeição no período. Se nenhum dia tiver registros, exibir **sem dados registrados**, sem média zero nem divisão por zero.
- Adicionar/editar/excluir refeições, inclusive lançamentos retroativos ou alteração de data/fuso, atualiza o período e sua quantidade de dias registrados. Agregar pelo dia local de consumo já preservado; não fazer arredondamento intermediário.
- **Gráfico semanal/mensal de consumo (confirmado no MVP):** exibir a evolução **diária** do consumo registrado no período, inicialmente de calorias (kcal), com opção de alternar a visualização para proteínas, carboidratos ou gorduras (g). A escolha do indicador não mistura unidades diferentes no mesmo eixo.
- Quando houver meta válida para o nutriente em um dia, apresentá-la como **referência visual** nessa data, usando a meta efetivamente vigente naquele dia, e não a meta atual aplicada retroativamente. Se o valor ou a origem da meta mudar durante a semana ou mês, a referência também muda na data correspondente; sem meta naquele dia, não inventar referência zero.
- **Dias sem refeições registradas aparecem como _sem dados_**, e não como ingestão zero, tanto no gráfico quanto no detalhe acessível. Não traçar uma evolução contínua de consumo através desses dias de modo que pareçam medições reais. Dias com registros mostram apenas os nutrientes efetivamente cadastrados, sem presumir diário completo.
- Incluir data e valores de consumo e meta (quando disponível) em descrição/consulta textual acessível; o gráfico deve funcionar em telas pequenas. Alterações e exclusões no diário, inclusive retroativas, atualizam as datas afetadas.
- **Contagem semanal/mensal de dias em relação à meta (confirmada):** no gráfico diário de cada nutriente (kcal, proteínas, carboidratos e gorduras), permitir identificar quais **dias com registros e meta válida** ficaram **abaixo**, **acima** ou **na meta**, e mostrar quantos dias pertencem a cada categoria dentro da semana/mês.
- **Contagens por nutriente selecionado:** os totais das categorias devem ser calculados separadamente para calorias e para cada macronutriente, sempre usando a **meta efetiva histórica do respectivo dia**. Uma troca de meta no meio do período não altera a referência de dias anteriores.
- **Sem dados não é abaixo da meta:** dias sem refeições não entram nessas contagens; dias com registros, mas **sem meta** para o nutriente selecionado, ficam identificados como **sem meta**, fora das três categorias. Informar claramente o número de dias comparáveis e que registros parciais podem classificar um dia como abaixo sem comprovar ingestão insuficiente.
- **Classificação estrita, sem faixa de tolerância (confirmado para o MVP):** para calorias ou qualquer macro selecionado, comparar o consumo registrado da data à meta efetiva daquela data: **abaixo** se consumo < meta; **na meta** somente se consumo = meta; **acima** se consumo > meta. Não considerar 5%, 100 kcal ou qualquer diferença positiva/negativa como cumprimento da meta.
- A comparação deve utilizar números decimais consistentes (sem artefatos de ponto flutuante e sem arredondamento intermediário); apresentar o consumo e a diferença com precisão suficiente para explicar a classificação. Valores que parecem iguais somente por arredondamento visual não podem gerar uma indicação enganosa de **na meta**.
- Não converter as contagens em **saldo acumulado de nutrientes ou calorias** do período nem tratar **acima da meta alimentar** como sinônimo obrigatório de superávit energético frente ao gasto corporal. O valor nutricional de um alimento e a absorção energética real são estimativas, mesmo com pesagem precisa das porções; evitar rotular dias como sucesso/fracasso clínico.
- **Semana de acompanhamento (confirmado no MVP):** para todos os resumos, gráficos e contagens semanais do diário, considerar **segunda-feira a domingo**, com sete datas locais consecutivas, independentemente do dia em que o usuário começou a usar a aplicação. Semanas que atravessam mês/ano permanecem um mesmo período de segunda a domingo.
- Usar a **data local do consumo já preservada** para atribuir cada refeição à semana correspondente, não a data UTC do cadastro nem a mudança posterior do fuso do dispositivo. Lançamentos retroativos e correções recalculam a semana afetada; a semana incompleta em andamento mostra os registros existentes sem inventar consumo dos dias futuros.
- **Diferença acumulada semanal/mensal (confirmada para o MVP):** para o nutriente selecionado (calorias em kcal ou proteínas/carboidratos/gorduras em g), somar **(consumo registrado no dia − meta efetiva do dia)** para cada dia do período que tenha **ao menos uma refeição registrada e uma meta válida para esse nutriente**. A referência da meta é histórica, respeitando mudanças pessoais e profissionais durante o período.
- Exibir o resultado com sinal e descrição explícita: positivo = **acima da soma das metas nos dias comparáveis**, negativo = **abaixo da soma das metas nos dias comparáveis**, zero = **diferença acumulada igual a zero**. O resultado zero não significa que todos os dias atingiram a meta: dias acima e abaixo podem se compensar, e os contadores diários continuam disponíveis.
- Mostrar junto à diferença o número de **dias comparáveis** e identificar separadamente **dias sem registros** e **dias com registros mas sem meta para o nutriente**. Não imputar consumo zero nem meta artificial em dias ausentes; mesmo com registros, o diário pode estar incompleto, portanto rotular como **diferença parcial do consumo registrado em relação às metas**, não como saldo energético corporal ou ingestão total.
- Corrigir/adicionar/excluir refeições, inclusive retroativas e com mudança de data/fuso, atualiza o agregado e os contadores dos períodos afetados. Somar com precisão numérica consistente sem arredondar antes de agregar. Sem dias comparáveis, exibir **comparação indisponível**, em vez de saldo zero.
- **Vitaminas e minerais no acompanhamento (confirmado para o MVP):** disponibilizar no diário e em seus resumos os micronutrientes **efetivamente informados** nas fontes nutricionais dos alimentos consumidos, incluindo, quando disponíveis, ferro, cálcio, vitamina C e vitamina B12. Não criar valores para nutrientes ausentes nem exigir que todos os alimentos possuam dados de todos os micronutrientes.
- **Cobertura nutricional explícita:** para cada micronutriente, distinguir (a) valor conhecido, inclusive **zero informado**, (b) valor **desconhecido** em um ou mais itens e (c) ausência de qualquer dado conhecido no período. Se algum item consumido não tiver informação do micronutriente, qualquer soma dos valores conhecidos deve ser rotulada **total parcial / dados incompletos**, não total exato; se todos os itens estiverem sem dados, apresentar **sem dados para este nutriente**, não zero.
- O total de micronutrientes deve considerar as quantidades registradas e as unidades de cada fonte (mg/µg, conforme aplicável), sem somar unidades incompatíveis nem arredondar durante o cálculo. A precisão mostrada deve respeitar a qualidade dos dados originais; corrigir/adicionar/excluir refeições recalcula os totais parciais e seus indicadores de cobertura.
- **Sem diagnóstico automático:** a existência de uma soma parcial não permite concluir que houve ingestão adequada ou deficiência; dias sem refeições cadastradas não são dias com consumo zero. **Confirmado: no MVP, micronutrientes não terão metas individuais, percentuais de adequação ou comparações com valores nutricionais de referência**. Essas possibilidades ficam para versão posterior, com critérios validados e sem recomendações clínicas presumidas.
- **Organização do painel nutricional (confirmada para o MVP):** manter calorias, proteínas, carboidratos e gorduras em destaque na visão principal do diário e dos resumos. Disponibilizar vitaminas, minerais e outros micronutrientes informados na fonte em seção secundária **Detalhes nutricionais**, expansível/consultável sem sobrecarregar o painel principal.
- **Explicação de dados incompletos:** ao consultar um micronutriente identificado como **total parcial / dados incompletos**, permitir abrir seus detalhes e ver **quais alimentos efetivamente registrados não têm valor informado para aquele nutriente**, com identificação clara do item/refeição de origem. Não sugerir que o alimento contém 0 do nutriente nem inventar valor; distinguir zero explicitamente informado de ausência.
- Exibir quantidade conhecida com unidade apropriada e a indicação de cobertura; quando todos os itens estiverem sem dados, mostrar **sem dados disponíveis**, permitindo consultar quais registros não têm informação. A navegação deve funcionar em dispositivo móvel e ser acessível, sem depender exclusivamente de cores.
- Os detalhes se referem aos alimentos do diário do titular, sem revelar dados de terceiros. As consultas por período respeitam os registros e as datas locais de consumo; editar, excluir ou cadastrar refeições recalcula informações e lista de lacunas.
- **Adiado para depois do MVP:** comparações de vitaminas/minerais com metas ou valores de referência e classificações de adequação. **Pendências técnicas para etapa 9:** precisão e unidades nas apresentações, conversões validadas e cobertura dos dados de origem. Para calorias/macros, **na meta** continua sendo igualdade estrita, sem tolerância.

**Adultos:** a aplicação poderá estimar uma meta energética por método validado e permitir ajustes manuais. O cálculo requer dados suficientes, explicação dos limites e revisão de profissionais antes do lançamento.

**Faixa etária:** o MVP atende apenas adultos (18 anos ou mais), sem cálculo, prescrição, metas ou acompanhamento pediátrico, inclusive nos painéis profissionais. O cálculo automático de metas energéticas para menores fica fora do escopo.

### 3.5. Planos alimentares e dietas

**Plano pessoal:** o usuário pode montar seus próprios planos com refeições, alimentos e quantidades. Isso é organização alimentar, não prescrição clínica automática. Planos pessoais salvos terão histórico de versões: ao salvar alterações em um plano existente, a aplicação preserva o estado anterior e registra uma nova versão. No MVP, dietas pessoais não precisam de agendamento de vigência.

**Plano profissional:** o nutricionista elabora e atribui planos a pacientes. O paciente pode consultar o plano profissional, mas não alterar sua prescrição. No MVP, cada paciente poderá ter **uma única dieta profissional vigente por vez**. Alterações na prescrição geram novas versões do mesmo plano lógico, mantendo as versões anteriores no histórico.

**Horários sugeridos (confirmado para o MVP):**
- O horário de uma refeição planejada é opcional.
- Quando informado pelo nutricionista ou usuário, representa apenas uma sugestão de organização.
- Não é prazo, obrigação ou critério de validação da refeição.
- Registrar ou consumir a refeição em horário diferente não gera erro, alerta, aviso de atraso ou penalização.
- O horário planejado e o horário real de consumo são informações distintas: o primeiro pertence ao plano; o segundo, ao diário alimentar.

**Instruções e observações (confirmadas para o MVP):**
- O nutricionista ou usuário poderá adicionar uma observação opcional à refeição inteira.
- Também poderá adicionar uma observação opcional a cada alimento/item da refeição.
- As observações são instruções em texto, separadas dos alimentos, quantidades e cálculos nutricionais; não substituem nem alteram esses dados.
- A interface deverá mostrar observações somente quando existirem e usar apresentação discreta, sem criar blocos vazios ou poluir a tela.
- No editor, os campos de observação poderão ficar recolhidos/ocultos até o usuário escolher adicioná-los. Na visualização, a observação da refeição aparece como texto secundário junto ao título; a observação do alimento aparece abaixo do nome do alimento, em estilo discreto.
- Em telas pequenas, priorizar expansão sob demanda e não adicionar colunas permanentes apenas para observações.

**Categorias de refeições (confirmado para o MVP):**
- Categorias padrão: **Café da manhã, Lanche da manhã, Almoço, Lanche da tarde, Jantar e Ceia**.
- O nutricionista pode criar categorias personalizadas para organizar sua biblioteca profissional e os planos que elabora para seus pacientes.
- Uma categoria é apenas uma forma de classificação/exibição e **não limita a uma refeição por dia**. Por exemplo, segunda-feira pode conter dois ou mais cafés da manhã.
- Refeições têm identidade própria e poderão ter nome específico além da categoria.
- Alterar ou arquivar uma categoria personalizada não reescreve nem apaga a classificação existente em versões publicadas de dietas ou no histórico alimentar.
- A possibilidade de categorias personalizadas para usuários comuns será avaliada separadamente; no MVP está confirmada para o nutricionista.

**Organização do cardápio (confirmada para o MVP):**
- Todo plano começa com um **cardápio diário padrão**, repetido nos sete dias da semana.
- Um mesmo dia pode conter **várias refeições planejadas da mesma categoria** (por exemplo, mais de um café da manhã ou lanche), cada uma como registro independente, com seus próprios alimentos, quantidades, observações e horário sugerido opcional.
- O usuário ou nutricionista pode personalizar refeições específicas para **dias da semana** sem precisar refazer todo o cardápio.
- As refeições não personalizadas em determinado dia continuam herdando o padrão diário.
- Substituir, adicionar ou retirar uma refeição específica de um dia não altera as outras refeições, mesmo que tenham a mesma categoria, nem os outros dias.
- É possível restaurar o padrão de uma refeição removendo sua personalização. Retirar intencionalmente uma refeição daquele dia é diferente de restaurar o padrão.
- Os totais nutricionais diários consideram as personalizações aplicáveis. Padrão e personalizações pertencem à versão publicada do plano, preservando seu histórico.
- Exceções para **datas isoladas**, além dos dias da semana, continuam fora do escopo confirmado.

Regras detalhadas e critérios de aceitação em [DIETAS.md](./DIETAS.md).

**Separação entre planejamento, modelos reutilizáveis e consumo:**
- Refeições previstas em uma dieta representam **o plano**.
- Modelos pessoais e profissionais representam **refeições reutilizáveis** na criação de registros ou de planos.
- Refeições registradas no diário representam **o consumo real**.
- O diário é independente das dietas e de suas versões.
- A comparação entre plano e diário poderá identificar diferenças, sem alterar nenhum dos registros.

**Versionamento do plano:**
- Um rascunho pode ser editado antes da publicação.
- Ao atualizar um plano profissional publicado, criar uma nova versão e permitir escolher entre **vigência imediata** (ativa ao publicar) e **vigência programada** (ativa automaticamente na data futura escolhida).
- Quando uma nova versão profissional for programada para uma data futura, a versão atual continua vigente até a transição; a versão futura não substitui a atual antes da data definida.
- O paciente poderá visualizar antecipadamente a próxima dieta publicada em uma área claramente identificada como **Próxima dieta**, com sua data/hora de início. Essa visualização é somente informativa; a dieta futura não será confundida com a prescrição vigente nem usada como padrão para registrar o consumo antes de começar a valer.
- Cada paciente poderá ter **no máximo uma dieta profissional futura programada** por vez. Para programar outra, a anterior deverá ser cancelada primeiro.
- A primeira dieta profissional de um paciente também poderá ter vigência imediata ou programada. Se for programada, o paciente verá apenas a seção **Próxima dieta** até o início da vigência, sem uma dieta vigente exibida.
- Antes do início da vigência, o nutricionista poderá cancelar a publicação programada ou reagendar sua data/hora de início, mantendo o conteúdo da versão publicada. Essas ações serão registradas no histórico. Se a vigência já tiver começado, não será permitido cancelar ou reagendar aquela versão; uma alteração deverá gerar nova versão.
- No início da vigência programada, a nova versão passa a ser a única vigente e a anterior permanece no histórico, com seu período de vigência encerrado.
- A transição deve ocorrer automaticamente, sem exigir que o nutricionista ou paciente esteja conectado.
- Cada versão deve preservar suas refeições planejadas, alimentos, quantidades, observações/instruções e composição nutricional de referência.
- Registrar autor, data/hora e resumo ou motivo da atualização.
- Cada plano possui uma única versão vigente por vez; versões antigas permanecem consultáveis, respeitando retenção de dados. O versionamento também se aplica a planos pessoais, com fluxo simples de salvar alterações e preservar a versão anterior, sem agendamento de vigência no MVP. Uma versão futura publicada fica em estado programado, não vigente, até sua data de início.
- No MVP, um paciente não pode ter dois planos profissionais vigentes simultaneamente. A atualização da dieta vigente gera uma nova versão; o plano anterior permanece no histórico, sem coexistência de prescrições profissionais ativas.
- Não substituir a versão anterior, pois precisamos reconstruir **qual plano estava vigente em determinado dia**.
- Refeições do diário e seus nutrientes nunca são alterados por atualização de versão do plano.

**Modelo conceitual proposto (não é ainda um esquema físico de banco):**
- `PlanoAlimentar`: identidade, titular, tipo pessoal/profissional e nutricionista responsável, quando aplicável.
- `VersaoPlano`: identificador do plano, número da versão, vigência, autor e estado.
- `RefeicaoPlanejada` / `ItemPlanejado`: estrutura nutricional vinculada a uma versão específica.
- `RefeicaoConsumida` / `ItemConsumido`: diário vinculado ao usuário e ao momento de consumo, **sem dependência obrigatória** de plano ou versão.
- `MetaHistorica`: metas com vigência e origem (usuário, estimativa ou nutricionista).

A independência do diário preserva **o que a pessoa comeu**; o versionamento preserva **o que estava prescrito/planejado**. Uma coisa não substitui a outra.

### 3.6. Gestão de pacientes

- **Convite e aceite (confirmados para o MVP):** o nutricionista inicia o convite. A ativação do vínculo e a autorização de acesso dependem de o paciente **autenticado aceitar e autorizar expressamente**, após visualizar quem o convida e quais categorias de dados serão compartilhadas. Convites pendentes, ignorados ou recusados não concedem acesso ao nutricionista.
- **Convites por e-mail confirmados para o MVP:** o nutricionista informa o e-mail do paciente. Quem já possui conta entra na plataforma; quem ainda não possui pode criar uma conta de adulto, verificar o endereço convidado e então decidir sobre o vínculo. O e-mail não inclui informações alimentares ou dados sensíveis e **não autoriza acesso por si só**.
- **Prazo e reenvio dos convites confirmados para o MVP:** cada convite por e-mail vale **7 dias corridos (168 horas desde a emissão)**. Expirado esse prazo, o convite não pode ser aceito. O nutricionista pode **reenviar um convite pendente ou expirado**, criando outro convite válido por 7 dias e invalidando o anterior; jamais reutilizar o link antigo. Um convite aceito não pode ser reenviado como pendente.
- **Retorno profissional confirmado:** no painel do nutricionista que convidou, apresentar o estado **pendente, aceito, recusado, expirado ou substituído por reenvio**, sem confundir ausência de resposta com recusa. O aceite somente é confirmado depois de autenticação, autorização e ativação efetiva do vínculo. Na recusa, não exigir justificativa e não liberar dados privados do destinatário.
- **Notificação de resultado do convite confirmada para o MVP:** quando o paciente **aceitar e o vínculo for efetivamente ativado** ou **recusar**, a plataforma enviará um **e-mail ao nutricionista que convidou**, informando o resultado e remetendo à sua área de convites. A mensagem não conterá dados nutricionais, medições, dietas ou motivos de recusa. O resultado no painel profissional permanece disponível mesmo se o e-mail falhar; entrega da mensagem não condiciona o aceite/recusa.
- Receber ou visualizar convite ou criar conta não equivale a aceitar. A autorização é validada no servidor, exige identidade correspondente ao e-mail convidado e é específica para o vínculo; um novo convite não substitui automaticamente um nutricionista ativo. **Ainda a definir:** tratamento técnico de falhas/abusos, retentativas e detalhes do compartilhamento.
- **Um único nutricionista ativo por paciente (confirmado no MVP):** um paciente adulto pode possuir **no máximo um vínculo profissional ativo por vez**. Não é permitido criar vínculos profissionais simultâneos para o mesmo paciente.
- **Troca de nutricionista (confirmado no MVP):** o paciente pode trocar de profissional sem perder **seu histórico alimentar, medições corporais, metas pessoais ou acesso à conta**. Dietas profissionais publicadas e vínculos encerrados permanecem identificados historicamente, sem converter prescrições do antigo profissional em prescrições do novo. Ao encerrar o vínculo anterior, sua dieta vigente perde a vigência e suas versões futuras deixam de ser ativadas, preservando a consulta histórica; o fluxo de troca direta é aprovado mediante aceite e confirmação única, com transição atômica no servidor.
- **Troca direta por convite (confirmada no MVP):** paciente já vinculado que aceita convite válido de outro nutricionista vê quem sai e quem entra, os dados que autoriza e o efeito sobre dieta e agendamentos antigos. Com **uma única confirmação Aceitar e trocar de nutricionista**, servidor encerra autorização/vínculo anterior, desativa dieta antiga, cancela versões futuras e ativa somente o novo vínculo autorizado, de modo **atômico**. Falha preserva integralmente vínculo e dieta anteriores; não há dois profissionais simultâneos. Histórico do paciente é preservado, sem transferir biblioteca privada nem autoria das prescrições; dieta nova exige posterior publicação pelo novo nutricionista.
- **Encerramento do vínculo pelo paciente confirmado no MVP:** na área **Meu nutricionista**, o titular escolhe **Encerrar acompanhamento** e confirma a decisão em uma etapa simples, a qualquer momento, sem solicitar aprovação do nutricionista nem acionar suporte. O texto informa que o acesso profissional será revogado, seu histórico pessoal preservado e poderá se vincular a outro profissional posteriormente.
- Após confirmação e processamento no servidor, o nutricionista anterior perde **imediatamente** a autorização de acompanhamento aos dados novos e históricos, inclusive em requisições de sessões abertas; não é suficiente ocultar itens da interface. O paciente mantém diário, indicadores, medições, metas pessoais, conta e registros históricos. O histórico do vínculo é preservado sujeito a regras de retenção da etapa 8, sem conceder acesso continuado ao ex-profissional.
- **Planos profissionais após encerrar vínculo (confirmado no MVP):** a dieta profissional vigente deixa de ser a prescrição atual e permanece consultável no histórico do paciente, sem perder autoria, conteúdo e vigência histórica. Qualquer versão futura programada pelo nutricionista anterior tem sua ativação **cancelada por encerramento do vínculo**, permanecendo registrada, sem excluir o histórico ou modificar o diário alimentar. A aplicação informará isso brevemente na confirmação de encerramento, sem etapas extras. O bloqueio de ativações tardias será garantido no servidor e a regra é diferente do cancelamento manual de agendamento durante um vínculo ativo.
- **Continuidade da meta efetiva após encerramento ou troca (confirmada no MVP):** manter a última meta efetiva de cada nutriente definido (kcal e macronutrientes) nos indicadores pessoais do paciente; se a meta tinha origem profissional, manter sua autoria e valor original no histórico, mas sem apresentá-la como prescrição profissional vigente. O paciente pode ajustar livremente a referência pessoal; quando houver novo nutricionista autorizado, a referência continua até ajuste pessoal ou publicação efetiva de nova meta profissional. O encerramento não apaga diário, medições, dietas antigas nem metas. **Aviso ao ex-profissional confirmado:** enviar e-mail ao nutricionista anterior, incluindo **nome do paciente** e confirmação de encerramento após término ou troca de vínculo, sem motivo nem dados nutricionais.
- **Notificação de encerramento ou troca (confirmada no MVP):** quando o paciente encerrar o acompanhamento ou a troca para outro nutricionista for concluída, o **nutricionista anterior** será avisado no histórico profissional e por **e-mail contendo o nome do paciente que encerrou o vínculo**. Não enviar justificativa, dados alimentares, peso, metas, dietas ou informações sobre o novo profissional. Envio ao endereço verificado da conta do ex-profissional, apenas após conclusão da operação. Falha no envio não desfaz a desvinculação nem devolve acesso ao paciente.
- **Proteção de convites contra abuso (confirmada no MVP):** aplicar limites automáticos de **envio e reenvio por profissional/destinatário** e proteção contra volume anormal e tentativas de contorno, sem exigir CAPTCHA ou passos adicionais no uso normal. Bloqueios devem ter feedback simples, preservar convites anteriores e não expor se uma conta de paciente existe; não apresentar envio como concluído quando não ocorreu. Regras e números configuráveis, rate limiting em API, idempotência e tratamento de filas/provedor ficam para a etapa 9.
- **Validação de nutricionista para o MVP:** cadastro simples com nome completo, número e região do **CRN**. Permitir conta **Em validação**, mas não enviar convites, ativar vínculos, acessar dados privados de pacientes como profissional, definir metas profissionais ou publicar/programar dietas antes da **aprovação da conferência da titularidade e regularidade do registro**. Situação pendente, validada ou não aprovada deve ter feedback claro, sem bloquear uso pessoal comum. Validar privilégios no servidor e persistência, não por campos editáveis do frontend; vínculo e consentimento do paciente continuam obrigatórios mesmo após aprovação.
- **Autorização para novo profissional:** o vínculo com o novo nutricionista e o acesso aos dados do paciente exigem autorização válida; não transferir automaticamente o acesso concedido ao profissional anterior. O novo vínculo pode ser ativado por confirmação única de troca, em operação atômica com encerramento do anterior, sem autorização herdada. Detalhes técnicos ficam para a etapa 9.
- **Garantia de integridade:** a regra de vínculo único deverá ser aplicada no servidor e banco, inclusive contra convites aceitos simultaneamente; detalhes de transação/constraint ficam para a etapa 9.
- Lista de pacientes vinculados.
- **Compartilhamento simples e explícito (confirmado no MVP):** na aceitação do convite, o paciente autorizará em **uma tela objetiva** o acesso às categorias relevantes: **diário alimentar e nutrientes consumidos, metas de calorias/macronutrientes, gráficos e indicadores, histórico de peso/IMC e planos alimentares profissionais associados ao acompanhamento**. A tela identifica o nutricionista, resume os dados e apresenta **Aceitar e autorizar** e **Recusar convite**, com detalhes opcionais; não obrigar o paciente a configurar uma sequência de permissões separadas.
- O nutricionista só terá acesso às **categorias aprovadas**, com vínculo ativo, autorização vigente e validação no servidor. Ter permissão de consulta não permite alterar registros de consumo do paciente. Novos usos ou categorias não são autorizados por omissão; o paciente conserva seus dados e poderá encerrar o vínculo conforme fluxo a detalhar.
- **Histórico anterior ao vínculo confirmado no MVP:** o aceite informado em tela única também permite ao nutricionista com vínculo ativo consultar **refeições/diário, metas, indicadores, pesagens e IMC registrados antes do início do acompanhamento**, sem segundo consentimento ou upload manual de relatórios. A tela informa explicitamente esse acesso histórico. O profissional não recebe automaticamente acesso a prontuários ou bibliotecas privadas de nutricionistas anteriores, e a autoria das prescrições preservadas não muda. O término do vínculo revoga o acesso de acompanhamento tanto a dados antigos quanto novos.
- Acesso ao diário alimentar e aos indicadores por período.
- Consulta ao histórico de peso.
- Criação e atribuição de planos alimentares.
- Histórico de alterações nos planos.

O modelo de múltiplos nutricionistas simultâneos está **descartado no MVP**. O paciente permanece titular de seus registros pessoais ao trocar de nutricionista, e o acesso ativo do antigo profissional é encerrado com o vínculo. **Etapa 6 concluída funcionalmente:** prevenção de abuso em convites aprovada com controles automáticos de frequência e reenvio, sem fricção no uso normal. Cadastro com nome/CRN/regional e bloqueio das funcionalidades profissionais até a verificação efetiva também aprovados. Método de validação, rechecagem, números do rate limit, segurança, entrega e filas ficam para as etapas 8/9. **Aviso de encerramento/troca ao ex-profissional com o nome do paciente, por e-mail e no histórico restrito, confirmado para o MVP**. **Permanência da última meta efetiva e dos dados do paciente após o fim do vínculo já confirmada**, com origem profissional histórica preservada. **Troca direta com confirmação única e transição atômica, fim da dieta antiga e cancelamento de futuras versões** já aprovados, preservando histórico; retenção e documentos ficam para etapa 8. Ver [PACIENTES.md](./PACIENTES.md).

### 3.7. Assinaturas e monetização

**Terminologia comercial:** “usuário comum” e “paciente” não são dois grupos de contas. Todo usuário possui a experiência pessoal gratuita; ser acompanhado por nutricionista só estabelece um vínculo, não altera o tipo de conta nem cria cobrança pessoal.

**Modelo comercial confirmado para o MVP — etapa 7:**

- **Usuários gratuitos, com ou sem nutricionista:** toda conta pessoal adulta acessa sem cobrança diário alimentar, registro de consumo, metas próprias, indicadores, gráficos, peso/IMC, histórico e planos pessoais. A conta e o acesso aos próprios dados não dependem de acompanhamento ou assinatura do nutricionista.
- **Plano profissional único no MVP (confirmado):** o nutricionista terá **uma única opção de assinatura mensal** reunindo todos os recursos profissionais previstos: convites/vínculos, painel e indicadores de usuários autorizados, criação/atribuição de dietas e metas profissionais, histórico e biblioteca profissional. **Preço e existência/quantidade de limite de pacientes ainda serão decididos**, sem concluir que o plano será ilimitado.
- **Sem cobrança da plataforma ao paciente para ser acompanhado:** o paciente não precisa pagar assinatura para aceitar convite, visualizar dieta profissional atribuída nem registrar sua alimentação durante o acompanhamento.
- **Contratação profissional somente depois de CRN aprovado (confirmada para o MVP):** o nutricionista cria conta pessoal gratuita e informa CRN/região. **Enquanto o registro estiver Em validação ou Não aprovado, não permitir checkout, criação de assinatura profissional paga ou primeira cobrança**, inclusive via API. Após aprovação efetiva da titularidade e regularidade do CRN, oferecer a opção de contratar explicitamente. **A aprovação não inicia cobranças nem assinaturas automáticas**. Regras comerciais para CRN que se torne irregular após contratação serão decididas separadamente.
- **Papéis e controles continuam independentes da cobrança:** pagamento não substitui aprovação do CRN, vínculo ativo, autorização do usuário ou verificação de permissões no servidor. Os recursos pessoais continuam disponíveis ao nutricionista em sua própria conta mesmo sem plano profissional habilitado.
- **Cancelamento voluntário da assinatura aprovado:** ao cancelar, o nutricionista **interrompe a renovação automática, mas conserva o acesso profissional até a data/hora final do período já pago**, desde que também permaneçam válidos CRN e as autorizações exigidas. Depois desse prazo, o uso das funções profissionais pagas será restringido, preservando os recursos pessoais da própria conta. A interface informa o prazo restante e não confunde cancelamento programado com fim efetivo de acesso. **Não encerrar automaticamente vínculos, excluir dados ou dietas publicados, alterar metas ou apresentar a assinatura como vínculo com paciente.** **Acesso após vencimento aprovado:** o nutricionista mantém conta pessoal, área de assinatura e **lista administrativa mínima de Meus pacientes**, com nome e estado do vínculo, mas **não pode consultar diários, dietas, metas, indicadores ou outros dados nutricionais dos usuários, nem convidar, editar, programar ou publicar dietas/metas profissionais**. Essas restrições valem no servidor, inclusive para sessões anteriormente abertas. **Vínculos e dados não são encerrados ou excluídos por inadimplência comercial**. Reativação só restitui capacidades com assinatura, CRN e autorizações ainda válidas; não restaura vínculo encerrado. **Ativação futura após vencimento comercial confirmada:** versões de dietas publicadas e agendadas anteriormente entram em vigor na data prevista, mesmo com assinatura expirada, **somente se o mesmo vínculo e a autorização do usuário permanecerem válidos**. Encerramento/troca de vínculo cancela o agendamento como já aprovado. Profissional com assinatura vencida não pode criar, alterar nem reagendar prescrições. **Falha na renovação da assinatura — confirmada no MVP:** oferecer **7 dias corridos (168 horas) de tolerância** a partir do vencimento do ciclo de uma assinatura anteriormente paga quando a renovação falhar, com aviso ao nutricionista e tentativas seguras de recuperação. Durante a tolerância, manter funcionalidades profissionais e autorização do usuário normalmente, sujeitas a CRN/vínculo. Se regularizar, o acesso continua; após 7 dias sem pagamento, aplicar a restrição comercial já definida, sem apagar vínculos, dietas, metas ou dados pessoais. Primeira contratação exige pagamento confirmado: **a tolerância não é uma primeira assinatura grátis**. Cancelamento voluntário não recebe tolerância adicional. Evitar cobranças duplicadas em retentativas e eventos de pagamento; tecnologia na etapa 9.

**Estrutura comercial já aprovada:** somente **um plano profissional mensal** no lançamento. **Cancelamento voluntário aprovado:** renovação é desativada, acesso profissional mantido até o fim do período pago e restringido depois, sem encerrar vínculos automaticamente nem apagar dados. **Decisões adiadas:** preço e **se haverá ou não limite de pacientes** (e eventual quantidade), sem inferir oferta ilimitada. **Acesso após expiração já aprovado:** conta pessoal, assinatura e lista administrativa de vínculos com nome e estado, sem dados nutricionais; ações profissionais bloqueadas no servidor. **Ativação de dietas previamente agendadas após vencimento já aprovada**, condicionada a vínculo e autorização ainda válidos, sem liberar nova atuação profissional. **Falha na renovação aprovada:** tolerância de 7 dias apenas para assinatura anteriormente paga, com aviso, manutenção temporária de acesso e recuperação de cobrança sem duplicações; suspender funções profissionais após o prazo sem regularização. **Ainda a decidir na etapa 7:** período de teste gratuito, provedor/fluxo de cobrança e demais questões comerciais adiadas.

**Ponto de retorno obrigatório:** não decidir agora preço, teste gratuito (nem supostos 14/30 dias) nem existência/quantidade de limite de pacientes. Na **etapa 9**, estimar custos operacionais com cenários de escala; na **etapa 10**, retomar e registrar expressamente as três decisões antes de concluir o planejamento. Pendência rastreável em [MONETIZACAO.md](./MONETIZACAO.md) e [ROADMAP.md](./ROADMAP.md).

**Especificação comercial em desenvolvimento:** [MONETIZACAO.md](./MONETIZACAO.md).

## 4. Princípios de produto

1. **Dois públicos centrais:** experiências pessoal gratuita e profissional paga desenhadas em conjunto.
2. **Planejamento e consumo independentes:** plano alimentar não é o diário alimentar.
3. **Histórico preservado:** metas, medidas, planos e registros alimentares não podem ser alterados retroativamente por mudanças em outros dados.
4. **Planos versionados:** atualizar plano publicado cria nova versão; refeições planejadas pertencem à versão.
5. **Valores históricos estáveis:** registros de consumo mantêm o valor nutricional e sua origem na época do lançamento.
6. **Dados rastreáveis:** fonte, unidade e contexto de preparo dos alimentos devem estar identificáveis.
7. **Permissões explícitas:** nutricionista visualiza dados autorizados somente durante vínculo ativo, com exceções legais de retenção a avaliar.
8. **Direitos dos pacientes:** dados pessoais e diário alimentar não ficam condicionados à assinatura do nutricionista.
9. **Escopo 18+:** apenas adultos podem se cadastrar ou ser acompanhados por nutricionistas na primeira versão; sem perfis dependentes ou módulos pediátricos.
10. **Proteção proporcional:** a restrição 18+ não é uma dispensa automática das obrigações da LGPD e do ECA Digital quando aplicáveis; revisar risco de acesso por menores e aferição de idade antes do lançamento.
11. **Sem diagnósticos automatizados:** indicadores não substituem avaliação profissional.
12. **Bibliotecas independentes:** modelos pessoais e profissionais não alteram registros históricos de consumo ou dietas publicadas.
13. **Padrão semanal previsível:** personalizações de refeições para dias da semana não modificam o cardápio dos demais dias nem as versões anteriores.

## 5. Escopo futuro — não priorizar no primeiro lançamento

- Rede social de nutrição.
- Curtidas, comentários e compartilhamentos.
- Perfis públicos.
- Biblioteca pública de dietas criadas por usuários.
- Recomendações avançadas ou geração automática de dietas.
- Reconhecimento de alimentos por imagem.
- Leitura de códigos de barras.
- Integrações com dispositivos e aplicativos de saúde.

Essas funcionalidades poderão ser avaliadas após validar o valor do diário alimentar e da gestão profissional. Uma rede social também exigirá moderação, denúncia de conteúdo, proteção contra desinformação e controles adicionais de privacidade.

## 6. Riscos e pontos de atenção

- **Privacidade:** dados alimentares e de saúde exigem minimização, proteção e autorização apropriada.
- **Acesso de profissionais:** vínculo com um único nutricionista ativo e autorização não dispensam controle servidor/banco contra acesso indevido a dados de outros pacientes.
- **Restrição etária:** o produto será 18+, com informação clara no cadastro e nos termos. A idade deve ser validada no backend, sem confiar apenas no frontend. A autodeclaração não garante automaticamente que não haverá acesso por menores; revisar o enquadramento de acesso provável, riscos e controles proporcionais à luz de orientações da ANPD, em especial o guia publicado em 28/10/2026.
- **Entrada indevida de menores:** planejar um fluxo seguro para denúncia/identificação, restrição ou encerramento de contas que violem a idade mínima, com atenção à proteção dos dados e direitos aplicáveis. A política não é uma transferência automática de responsabilidade para o usuário.
- **Qualidade nutricional:** fontes podem conter dados ausentes, unidades diferentes e informações específicas de preparo.
- **Histórico:** atualizações no catálogo nutricional, nas dietas e nas metas não podem alterar retrospectivamente informações antigas.
- **Custos do gratuito:** estimar armazenamento e uso médio antes do lançamento.
- **Viabilidade comercial:** validar se o profissional percebe valor suficiente na gestão e no acompanhamento de pacientes para pagar mensalidade.

## 7. Questões em aberto para a descoberta

1. Quais medidas proporcionais de aferição de idade se aplicam ao aplicativo 18+, conforme análise jurídica antes do lançamento?
2. Como lidar com uma conta identificada como pertencente a menor sem reter ou expor indevidamente dados?
3. Como validar o registro profissional de nutricionista?
4. **Decidido para o MVP:** micronutrientes mostram somente quantidades conhecidas e detalhes de cobertura; metas individuais, percentuais e comparação com referências ficam para depois do MVP. As regras técnicas de precisão, fonte e conversão serão detalhadas na etapa 9.
5. Quais recursos compõem o plano profissional pago? Haverá limites de pacientes?
6. Quais dados ficam disponíveis depois do encerramento do vínculo profissional?
7. Qual será o fluxo de exportação, portabilidade, retenção e exclusão dos dados?
8. Como será a troca de nutricionista?
9. O usuário comum poderá criar suas próprias categorias personalizadas de refeições?
10. Quais campos adicionais realmente agregam valor ao cadastro das refeições, sem poluir a interface?
11. Quais regras adicionais de correção e retenção aplicar aos alimentos personalizados e seus históricos sem expor registros privados?

**Definições funcionais centrais do módulo de dietas concluídas para planejamento:** criação e atribuição, categorias, composição fixa, horários opcionais, observações, cardápio semanal, múltiplas refeições da mesma categoria, biblioteca, histórico/versionamento e agendamentos. Detalhes de implementação e questões transversais serão tratados nas demais etapas.

## 8. Referências iniciais

- [TBCA — Tabela Brasileira de Composição de Alimentos](https://tbca.net.br/)
- [TACO — 4ª edição, NEPA/UNICAMP](https://nepa.unicamp.br/wp-content/uploads/sites/27/2023/10/taco_4_edicao_ampliada_e_revisada.pdf)
- [Lei nº 8.234/1991 — Regulamentação da profissão de nutricionista](https://www.planalto.gov.br/ccivil_03/leis/1989_1994/l8234.htm)
- [CFN — Nota Técnica nº 82/2023 sobre prescrição dietética](https://www.cfn.org.br/wp-content/uploads/2023/11/AMPLIADA_PRESCRI%C3%87%C3%83O-DIET%C3%89TICA-COMO-ATIVIDADE-PRIVATIVA-DO-NUTRICIONISTA.pdf)
- [LGPD — Lei nº 13.709/2018, art. 14 (crianças e adolescentes)](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
- [ECA Digital — Lei nº 15.211/2025](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2025/lei/l15211.htm)
- [ANPD — Informações oficiais sobre o ECA Digital](https://www.gov.br/anpd/pt-br/assuntos/noticias/eca-digital)
- [ANPD — Guia sobre escopo e obrigações gerais do ECA Digital, publicado em 28/10/2026](https://www.gov.br/anpd/pt-br/assuntos/noticias/anpd-publica-guia-que-esclarece-quem-deve-cumprir-e-quais-sao-as-responsabilidades-das-plataformas-na-protecao-de-criancas-e-adolescentes)
- [Decreto nº 12.880/2026 — Regulamentação do ECA Digital](https://www2.camara.leg.br/legin/fed/decret/2026/decreto-12880-18-marco-2026-798813-publicacaooriginal-178481-pe.html)
- [ANPD — Enunciado CD/ANPD nº 1/2023 sobre bases legais e melhor interesse](https://www.gov.br/anpd/pt-br/assuntos/noticias/anpd-divulga-enunciado-sobre-o-tratamento-de-dados-pessoais-de-criancas-e-adolescentes)
- [Ministério da Saúde — Orientações para avaliação antropométrica](https://www.gov.br/saude/pt-br/composicao/saps/vigilancia-alimentar-e-nutricional/arquivos/orientacoes-para-a-coleta-e-analise-de-dados-antropometricos-em-servicos-de-saude)
- [National Academies — Dietary Reference Intakes for Energy (2023)](https://www.nationalacademies.org/read/26818/chapter/2) — referência técnica que demonstra a necessidade de equações distintas por idade, sexo e atividade; não deve ser adotada automaticamente como padrão clínico brasileiro.

---

**Próximo passo:** resolver as questões de produto em aberto e, a partir delas, produzir os documentos complementares de requisitos, arquitetura, segurança, decisões técnicas e instruções para agentes de IA.