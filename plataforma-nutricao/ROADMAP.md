# Roteiro de planejamento — Plataforma de Nutrição

> **Status:** planejamento do produto; nenhuma etapa representa implementação concluída do software.
> **Data de referência:** 31/10/2026
> **Etapas de planejamento:** 10
> **Concluídas (definições funcionais principais):** 3
> **Em andamento:** etapa 4 — Base de alimentos e cálculos nutricionais
> **Restantes:** 7, incluindo a etapa 4

## Convenções

- **Concluída:** decisões principais de escopo e comportamento registradas; detalhes técnicos e riscos transversais podem permanecer para etapas próprias.
- **Em andamento:** etapa em discussão.
- **Pendente:** etapa ainda não detalhada.
- Este documento não substitui [IDEA.md](./IDEA.md), que reúne a visão do produto, nem as especificações por módulo.
- Requisitos e decisões podem ser revisados quando identificados riscos reais, inconsistências ou necessidades de produto.

## Dez etapas do planejamento

| Etapa | Assunto | Estado | Entregas / foco |
| --- | --- | --- | --- |
| 1 | Visão de produto e público-alvo | Concluída | Dois públicos, proposta de valor, monetização inicial, MVP adulto |
| 2 | Modelo de dietas e refeições | Concluída | Plano versus diário, versionamento, bibliotecas, horários/observações, composição fixa |
| 3 | Criação e atribuição de dietas | Concluída | Fluxo profissional, atribuição, cardápio padrão com personalizações semanais, categorias, vigência imediata/programada, visualização antecipada, cancelamento/reagendamento |
| 4 | Base de alimentos e cálculos nutricionais | Em andamento | Alimentos personalizados privados para usuários e nutricionistas confirmados, com cópia informativa no plano do paciente; pendem uso/licença da TBCA, fontes, unidades/porções, dados ausentes e importação |
| 5 | Diário alimentar, metas e indicadores | Pendente | Registro do consumo real, histórico, metas datadas, saldo diário/semanal/mensal, peso e IMC |
| 6 | Gestão de pacientes e permissões | Pendente | Convites, vínculo único, revogação, acesso aos dados, histórico profissional, validação CRN |
| 7 | Monetização e assinaturas | Pendente | Recursos gratuitos e pagos, planos, limites, cobrança, cancelamento e continuidade do acesso |
| 8 | Requisitos não funcionais, segurança e privacidade | Pendente | LGPD, controle de acesso, riscos, retenção, portabilidade, observabilidade, desempenho, backup |
| 9 | Arquitetura, banco e integrações | Pendente | Modelo de dados, TypeScript, APIs, integridade, histórico, automações e infraestrutura |
| 10 | Escopo final do MVP, cronograma e documentos para agentes de IA | Pendente | Priorização, critérios de aceite, plano de implementação, `PLAN.md`, `AGENTS.md` e `ARCHITECTURE.md` |

## Conclusão da etapa 3

Decisões confirmadas:
- Profissional cria refeição com alimentos e quantidades; a aplicação calcula nutrientes.
- Biblioteca profissional de refeições reutilizáveis entre pacientes.
- Dietas com cardápio diário padrão e personalizações por dia da semana.
- Horários sugeridos e observações opcionais, sem alertas por atraso.
- Várias refeições podem compartilhar a mesma categoria no mesmo dia.
- Categorias padrão: Café da manhã, Lanche da manhã, Almoço, Lanche da tarde, Jantar e Ceia.
- Nutricionista pode criar categorias personalizadas em sua área profissional.
- Planos têm versões preservadas. Apenas uma dieta profissional vigente e uma futura programada por paciente.
- Publicação imediata ou programada da primeira dieta ou de versões subsequentes; futura visível antes da vigência.
- Cancelamento e reagendamento de versão futura antes da ativação; operações auditadas.
- Dietas pessoais também têm histórico de versões, sem agendamento no MVP.
- Não há seletor de alimentos alternativos para o paciente.
- Diário alimentar permanece independente do plano.

Ver: [DIETAS.md](./DIETAS.md).

### Pendências não bloqueantes da etapa 3

- Se usuários comuns terão categorias personalizadas além das seis categorias padrão.
- Se bibliotecas terão favoritos/etiquetas no MVP.
- Se o registro de refeição guardará referência opcional ao modelo de origem.
- Detalhes de correção do histórico: tratar na etapa 5 e na arquitetura.

Estas pendências ficam registradas, não são decisões tomadas por omissão.

## Critério para avançar na etapa 4

**Confirmado para o MVP:** usuários comuns e nutricionistas poderão cadastrar alimentos personalizados, inclusive industrializados com dados declarados de rótulos. Ver [ALIMENTOS.md](./ALIMENTOS.md).

**Visibilidade confirmada:** alimentos personalizados são privados ao criador; pacientes veem apenas dados incorporados a suas dietas profissionais, sem acesso ao catálogo original. Não haverá catálogo público colaborativo no MVP.

**Pendente:** confirmar cobertura/licenciamento comercial e formato da TBCA, fontes complementares, porções e medidas caseiras, valores nutricionais ausentes e regras de validação.

**Importante:** conclusão das 10 etapas encerra o planejamento, não significa que o software foi desenvolvido, validado ou colocado em produção.
