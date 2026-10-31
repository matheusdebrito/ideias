# Alimentos e cálculos nutricionais — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Módulo:** MVP
> **Etapa:** 4 de 10 — Base de alimentos e cálculos nutricionais (em andamento)
> **Versão:** 0.4
> **Data:** 31/10/2026
> **Fonte da visão geral:** [IDEA.md](./IDEA.md)
> **Decisões confirmadas** são distintas de **propostas a validar**.

## 1. Objetivo

Permitir selecionar alimentos de um catálogo nutricional rastreável e registrar alimentos não encontrados, mantendo unidades de porção, fonte dos dados e histórico coerentes.

## 2. Decisões confirmadas para o MVP

- Usuários comuns e nutricionistas poderão criar **alimentos personalizados**.
- Isso inclui alimentos industrializados e produtos cujos dados sejam informados com base na tabela nutricional do rótulo.
- **Cada alimento personalizado é privado ao criador** (usuário comum ou nutricionista), sem publicação ou descoberta em um catálogo comunitário no MVP.
- O criador pode utilizar o alimento em suas refeições, dietas e diário alimentar, conforme suas demais permissões de domínio.
- Quando o nutricionista utiliza um alimento privado em uma dieta profissional publicada para um paciente, este vê **os dados preservados na refeição planejada**, incluindo nome, porção, origem declarada e nutrientes disponíveis. Isso não compartilha o registro do alimento no catálogo nem permite acesso direto ao cadastro original.
- Uma refeição copiada da dieta para o diário alimentar cria novos dados de consumo independentes; não transforma o alimento privado do profissional em alimento pessoal do paciente.
- Um usuário não pode consultar, alterar, listar ou excluir alimentos privados de outro usuário pelo ID ou pela API. A autorização precisa ser garantida no servidor.
- Distinguir alimento de catálogo (com sua referência e versão de origem) de alimento personalizado com dados declarados pelo usuário. O fato de ser cadastrado não implica validação por entidade oficial.
- **No cadastro manual de alimento personalizado são obrigatórios:** nome, quantidade de referência (positiva), unidade de referência, energia (kcal), proteínas (g), carboidratos (g) e gorduras totais (g). É preciso informar explicitamente os quatro nutrientes essenciais; valor zero é permitido quando informado e coerente, mas ausência não é zero.
- Macronutrientes essenciais e energia não podem ficar em branco no alimento personalizado salvo; impedir salvamento e apontar precisamente os campos ausentes ou inválidos.
- Fibras, vitaminas, minerais e outros componentes são opcionais no cadastro manual. Nutriente opcional não informado deve permanecer **desconhecido**, não zero.
- Para totais de micronutrientes no plano ou diário, se qualquer item relevante não possuir o nutriente, indicar cobertura incompleta e não apresentar a soma parcial como total exato. Evitar reportar como zero valores desconhecidos.
- Verificar valores numéricos não negativos, quantidade de referência positiva e unidade válida. Pode-se sinalizar inconsistências relevantes entre energia e macros, mas **não exigir igualdade exata** entre kcal informada e kcal estimada a partir dos macronutrientes, considerando arredondamentos, fibra e fatores nutricionais.
- A obrigatoriedade dos quatro valores essenciais é uma regra do **cadastro manual personalizado**. Alimentos importados de fontes institucionais podem ter dados não analisados/ausentes e requerem critérios próprios de seleção e apresentação, a definir; não inventar valores para completá-los.
- O alimento selecionado terá quantidade e unidade para o cálculo proporcional.
- **Referências nutricionais aceitas no cadastro (definido para o MVP):** 100 g, 100 mL ou porção declarada, sempre com quantidade de referência positiva e unidade explícita. Para uma porção declarada, armazenar seu significado (por exemplo, porção de 30 g ou uma unidade), sem presumir que toda porção seja igual.
- Os nutrientes de uma quantidade consumida são proporcionais aos nutrientes da quantidade de referência, após uma conversão autorizada. Exemplo: 120 kcal para uma porção de 30 g implicam 240 kcal para 60 g.
- Conversões na mesma grandeza física podem usar unidades equivalentes (por exemplo, g e kg; mL e L). Conversão entre massa e volume exige densidade conhecida para o alimento; entre unidade/colher/xícara e g/mL exige fator específico e rastreável para aquele alimento e preparação.
- Medidas caseiras são opcionais e só devem ser oferecidas como seleção se a conversão para a referência nutricional for conhecida. Na ausência da conversão, impedir cálculo nessa medida, mostrar explicação e oferecer uma unidade compatível.
- Registrar os valores de referência, origem da equivalência e fator de conversão usados no cálculo e preservar o retrato no histórico; mudanças posteriores em porções/equivalências não alteram dieta publicada nem consumo registrado.
- Alterações posteriores no cadastro de um alimento não podem reescrever o histórico de refeições consumidas ou dietas publicadas. Os registros de destino preservam a composição utilizada no momento da gravação.
- TBCA é a **fonte principal candidata**, com TACO como possível complemento. A incorporação comercial, atribuição e redistribuição dependem de validação das condições aplicáveis; não há licença presumida.

## 3. Proposta de experiência (a validar)

1. Buscar um alimento existente.
2. Caso não exista, oferecer a ação **Cadastrar alimento**.
3. Solicitar nome, quantidade e unidade de referência (100 g, 100 mL ou porção declarada com dimensão definida), kcal, proteínas, carboidratos e gorduras totais; oferecer campos opcionais para fibras, vitaminas, minerais e demais componentes, conforme o rótulo.
4. Mostrar a origem declarada, em contraste com alimentos de fontes de composição nutricional institucional.
5. Salvar alimento e disponibilizá-lo para seleção segundo as permissões de acesso.

**Cuidados técnicos propostos:** validar números e unidades; evitar divisão por zero; não inferir nutrientes ausentes; preservar origem, quantidade de referência, data e valores de cada cálculo; evitar duplicar alimento no diário ao repetir uma requisição.

## 4. Decisões em aberto — não presumir definição

1. Como lidar com rótulos com campos essenciais faltantes (impedem cadastro manual) e alimentos institucionais com nutrientes ausentes (política própria)?
3. Como sinalizar alimentos semelhantes, marcas, preparação (cru/cozido) e possíveis duplicidades?
4. Qual é a base nutricional legalmente e operacionalmente disponível para uso comercial? Verificar cobertura da TBCA, formato, direitos de uso/redistribuição e possibilidade de TACO.
5. Quais campos e regras de arredondamento serão usados na exibição, cálculos e comparação de nutrientes?
6. Como tratar a permanência dos dados já publicados quando o criador apagar um alimento privado ou quando o vínculo profissional for encerrado?

## 5. Critérios de aceitação propostos

| Cenário | Resultado esperado |
| --- | --- |
| Usuário ou nutricionista não encontra alimento no catálogo | Pode cadastrar um alimento próprio |
| Usuário cadastra produto conforme rótulo | Fonte aparece como declarada pelo usuário, não como TBCA |
| Usuário tenta consultar via API alimento privado de outra pessoa | Backend nega acesso |
| Nutricionista usa alimento privado em dieta publicada | Paciente vê os dados incorporados na dieta, mas não obtém acesso ao catálogo privado do nutricionista |
| Paciente registra no diário refeição de dieta com alimento privado do nutricionista | Diário recebe cópia independente sem dar acesso ao cadastro privado original |
| Cadastro não informa determinado micronutriente | A ausência é preservada e não aparece como valor zero |
| Usuário tenta salvar alimento personalizado sem kcal, proteínas, carboidratos ou gorduras totais | Sistema não salva e indica os campos essenciais faltantes |
| Usuário informa valor zero em proteína, carboidrato ou gordura | Zero informado pode ser aceito; não confundir com campo vazio |
| Alimento tem micronutrientes desconhecidos e entra em refeição | Refeição pode ser utilizada; agregados desse nutriente são explicitamente incompletos |
| Energia informada difere levemente do cálculo simplificado 4-4-9 | Não bloquear por diferença exata: validar apenas inconsistências evidentes, respeitando arredondamentos e fatores de conversão |
| Dieta publicada usa alimento que depois é editado | Dieta publicada mantém seu retrato histórico |
| Diário registra alimento que depois é editado | Registro de consumo mantém seus valores originais |
| Um alimento tem origem e porção de referência distintas de outro | Cálculo respeita cada origem e sua unidade, sem converter silenciosamente |
| Produto tem 120 kcal em uma porção de 30 g e o consumo é de 60 g | Cálculo proporcional retorna 240 kcal e aplica o mesmo fator aos nutrientes declarados |
| Cadastro usa 100 mL, mas usuário informa 50 g sem densidade do alimento | Conversão é recusada com orientação; não se presume 1 g = 1 mL |
| Usuário tenta selecionar colher ou xícara sem equivalência específica | A medida não é oferecida como conversível; app sugere medida compatível |
| Um fator de equivalência muda após registrar consumo | O histórico conserva o fator usado originalmente e seus valores calculados |

## 6. Dependências da etapa

Validar a viabilidade jurídica e técnica da base principal antes de definir integração/importação. **Privacidade do catálogo personalizado definida:** dados privados do criador e somente retratos de nutrientes/nome/porção em dietas publicadas, com autorização no servidor. Não existe catálogo público colaborativo no MVP; eventual mudança exigirá nova decisão de produto e revisão de qualidade e privacidade.

---

Este é um documento de planejamento; não há funcionalidade implementada no repositório de ideias.
