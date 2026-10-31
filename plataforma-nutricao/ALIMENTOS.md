# Alimentos e cálculos nutricionais — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Módulo:** MVP
> **Etapa:** 4 de 10 — Base de alimentos e cálculos nutricionais (em andamento)
> **Versão:** 0.2
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
- Valores nutricionais não informados **não podem ser interpretados como zero**.
- O alimento selecionado terá quantidade e unidade para o cálculo proporcional.
- Alterações posteriores no cadastro de um alimento não podem reescrever o histórico de refeições consumidas ou dietas publicadas. Os registros de destino preservam a composição utilizada no momento da gravação.
- TBCA é a **fonte principal candidata**, com TACO como possível complemento. A incorporação comercial, atribuição e redistribuição dependem de validação das condições aplicáveis; não há licença presumida.

## 3. Proposta de experiência (a validar)

1. Buscar um alimento existente.
2. Caso não exista, oferecer a ação **Cadastrar alimento**.
3. Solicitar nome, dados de referência da porção/unidade e valores disponíveis na fonte escolhida, como o rótulo.
4. Mostrar a origem declarada, em contraste com alimentos de fontes de composição nutricional institucional.
5. Salvar alimento e disponibilizá-lo para seleção segundo as permissões de acesso.

**Cuidados técnicos propostos:** validar números e unidades; evitar divisão por zero; não inferir nutrientes ausentes; preservar origem, quantidade de referência, data e valores de cada cálculo; evitar duplicar alimento no diário ao repetir uma requisição.

## 4. Decisões em aberto — não presumir definição

1. Quais campos devem ser obrigatórios no cadastro inicial: nome, porção/medida de referência, energia, macros? Como tratar rótulos com informação parcial?
2. Como representar produtos com porção em g, mL, unidade e medidas caseiras sem conversões falsas?
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
| Dieta publicada usa alimento que depois é editado | Dieta publicada mantém seu retrato histórico |
| Diário registra alimento que depois é editado | Registro de consumo mantém seus valores originais |
| Um alimento tem origem e porção de referência distintas de outro | Cálculo respeita cada origem e sua unidade, sem converter silenciosamente |

## 6. Dependências da etapa

Validar a viabilidade jurídica e técnica da base principal antes de definir integração/importação. **Privacidade do catálogo personalizado definida:** dados privados do criador e somente retratos de nutrientes/nome/porção em dietas publicadas, com autorização no servidor. Não existe catálogo público colaborativo no MVP; eventual mudança exigirá nova decisão de produto e revisão de qualidade e privacidade.

---

Este é um documento de planejamento; não há funcionalidade implementada no repositório de ideias.
