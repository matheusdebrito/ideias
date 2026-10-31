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
| 4 | Base de alimentos e cálculos nutricionais | Em andamento | Regras de cadastro, unidades, cálculos e dados incompletos definidas; fonte TBCA/TACO/JSON adiada para etapa 9 sem bloquear planejamento funcional; pendem regras de identificação de alimentos e histórico |
| 5 | Diário alimentar, metas e indicadores | Pendente | Registro do consumo real, histórico, metas datadas, saldo diário/semanal/mensal, peso e IMC |
| 6 | Gestão de pacientes e permissões | Pendente | Convites, vínculo único, revogação, acesso aos dados, histórico profissional, validação CRN |
| 7 | Monetização e assinaturas | Pendente | Recursos gratuitos e pagos, planos, limites, cobrança, cancelamento e continuidade do acesso |
| 8 | Requisitos não funcionais, segurança e privacidade | Pendente | LGPD, controle de acesso, riscos, retenção, portabilidade, observabilidade, desempenho, backup |
| 9 | Arquitetura, banco e integrações | Pendente | Modelo de dados, TypeScript, APIs, integridade, histórico, automações, infraestrutura e escolha final da fonte de alimentos (TBCA/TACO/JSON) antes da integração |
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

**Cadastro manual definido:** nome, referência quantitativa e unidade, kcal, proteínas, carboidratos e gorduras totais obrigatórios; demais nutrientes opcionais. Valores ausentes não são zero; relatórios de micronutrientes com dados faltantes indicam incompletude.

**Porções confirmadas:** composição nutricional por 100 g, 100 mL ou porção declarada; medidas caseiras apenas com equivalências conhecidas por alimento. Não converter massa/volume/unidade sem fator específico e rastreável. Ver [ALIMENTOS.md](./ALIMENTOS.md).

**Completude dos dados institucionais confirmada:** permitir busca/consulta de alimentos com valores essenciais incompletos, mas bloquear seleção para dietas/modelos/diário até completar energia, proteínas, carboidratos e gorduras totais. Micronutrientes ausentes não bloqueiam o uso, mas relatórios sinalizam cobertura incompleta.

**Precisão confirmada:** aplicar conversões e somas sem arredondamento intermediário; arredondar somente na apresentação (kcal geralmente 0 ou 1 casa, macros 1 casa, micros conforme mg/µg e precisão da fonte). Não apresentar falsa exatidão. Ver [ALIMENTOS.md](./ALIMENTOS.md).

**Risco de fonte verificado em 31/10/2026:** a TBCA oficial restringe reprodução/comercialização e requer contato com os coordenadores para fins comerciais ([site oficial](https://www.tbca.net.br/index.html)). A disponibilidade pública da TACO ([NEPA/UNICAMP](https://nepa.unicamp.br/tabela-brasileira-de-composicao-de-alimentos-4a-edicao/)) não comprova permissão para distribuição em aplicativo pago. A autorização e as condições de cada base precisam ser verificadas antes de importar para o produto comercial.

**Decisão deliberadamente adiada (31/10/2026):** escolher entre TBCA, TACO e arquivo JSON próprio **na etapa 9 ou antes da implementação do importador**, sem bloquear a conclusão funcional da etapa 4. JSON é formato, não origem nutricional: verificar procedência, direitos de uso e qualidade dos dados, mesmo se mantidos em arquivo local. Nenhuma fonte está escolhida.

**Pendências atuais para concluir a etapa 4:** regras de identificação/duplicidade e preparo dos alimentos e preservação dos dados publicados. **Pendências para a etapa 9 e antes do lançamento:** fonte do catálogo, licença/autorização quando aplicável, cobertura, importação e aritmética decimal/modo de arredondamento técnico.

**Importante:** conclusão das 10 etapas encerra o planejamento, não significa que o software foi desenvolvido, validado ou colocado em produção.
