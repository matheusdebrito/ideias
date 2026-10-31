# Roteiro de planejamento — Plataforma de Nutrição

> **Status:** planejamento do produto; nenhuma etapa representa implementação concluída do software.
> **Data de referência:** 31/10/2026
> **Etapas de planejamento:** 10
> **Concluídas (definições funcionais principais):** 4
> **Em andamento:** etapa 5 — Diário alimentar, metas e indicadores
> **Restantes:** 6, incluindo a etapa 5

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
| 4 | Base de alimentos e cálculos nutricionais | Concluída | Cadastro, unidades, cálculos, dados incompletos, pesagem pós-preparo, marcas e aviso de duplicidade definidos; fonte TBCA/TACO/JSON adiada para etapa 9; retenção para etapas 8/9 |
| 5 | Diário alimentar, metas e indicadores | Em andamento | Gráficos por dia/nutriente e contagem semanal/mensal de dias acima, abaixo ou na meta aprovados; períodos sem dados/sem meta separados; resta decidir critério de na meta e outras pendências |
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

## Encerramento da etapa 4 e próximos passos

**Confirmado para o MVP:** usuários comuns e nutricionistas poderão cadastrar alimentos personalizados, inclusive industrializados com dados declarados de rótulos. Ver [ALIMENTOS.md](./ALIMENTOS.md).

**Visibilidade confirmada:** alimentos personalizados são privados ao criador; pacientes veem apenas dados incorporados a suas dietas profissionais, sem acesso ao catálogo original. Não haverá catálogo público colaborativo no MVP.

**Cadastro manual definido:** nome, referência quantitativa e unidade, kcal, proteínas, carboidratos e gorduras totais obrigatórios; demais nutrientes opcionais. Valores ausentes não são zero; relatórios de micronutrientes com dados faltantes indicam incompletude.

**Porções confirmadas:** composição nutricional por 100 g, 100 mL ou porção declarada; medidas caseiras apenas com equivalências conhecidas por alimento. Não converter massa/volume/unidade sem fator específico e rastreável. Ver [ALIMENTOS.md](./ALIMENTOS.md).

**Completude dos dados institucionais confirmada:** permitir busca/consulta de alimentos com valores essenciais incompletos, mas bloquear seleção para dietas/modelos/diário até completar energia, proteínas, carboidratos e gorduras totais. Micronutrientes ausentes não bloqueiam o uso, mas relatórios sinalizam cobertura incompleta.

**Precisão confirmada:** aplicar conversões e somas sem arredondamento intermediário; arredondar somente na apresentação (kcal geralmente 0 ou 1 casa, macros 1 casa, micros conforme mg/µg e precisão da fonte). Não apresentar falsa exatidão. Ver [ALIMENTOS.md](./ALIMENTOS.md).

**Risco de fonte verificado em 31/10/2026:** a TBCA oficial restringe reprodução/comercialização e requer contato com os coordenadores para fins comerciais ([site oficial](https://www.tbca.net.br/index.html)). A disponibilidade pública da TACO ([NEPA/UNICAMP](https://nepa.unicamp.br/tabela-brasileira-de-composicao-de-alimentos-4a-edicao/)) não comprova permissão para distribuição em aplicativo pago. A autorização e as condições de cada base precisam ser verificadas antes de importar para o produto comercial.

**Decisão deliberadamente adiada (31/10/2026):** escolher entre TBCA, TACO e arquivo JSON próprio **na etapa 9 ou antes da implementação do importador**, sem bloquear a conclusão funcional da etapa 4. JSON é formato, não origem nutricional: verificar procedência, direitos de uso e qualidade dos dados, mesmo se mantidos em arquivo local. Nenhuma fonte está escolhida.

**Preparo definido:** o plano e o diário priorizam alimentos na forma pronta para consumo e peso após preparo (para arroz, carnes, massas etc.); alimentos de consumo cru seguem disponíveis; receitas por ingredientes crus são possibilidade futura, sem conversão genérica de peso cru/cozido. Ver [ALIMENTOS.md](./ALIMENTOS.md).

**Marcas confirmadas:** alimentos comuns não precisam ter marca; produtos industrializados podem ser diferenciados por marca/variante relevante, sem unificar composição nutricional de produtos diferentes.

**Duplicidade confirmada:** no cadastro personalizado, exibir candidatos semelhantes encontrados no catálogo privado do próprio criador, sem bloqueio de salvamento e sem fusão automática.

**Etapa 4 concluída funcionalmente:** as regras aprovadas estão detalhadas em [ALIMENTOS.md](./ALIMENTOS.md). A escolha da fonte (TBCA/TACO/JSON), licença/autorização aplicável, cobertura, importação, algoritmo de similaridade e aritmética decimal/modo técnico de arredondamento ficam na etapa 9. Regras de exclusão/retenção e preservação de dados pessoais serão detalhadas nas etapas 8 e 9, sem retroceder o planejamento funcional.

**Etapa 5 em andamento:** aprovado editar/excluir refeições e manter histórico discreto de correções. O paciente pode ajustar a meta efetiva originalmente definida por nutricionista mediante aviso e confirmação, sem alterar a prescrição. **Visibilidade profissional confirmada:** nutricionista com vínculo ativo/autorizado poderá ver meta original, meta ajustada, autoria e data/hora no painel do paciente. Aviso discreto no painel profissional aprovado, sem e-mail/push/mensagem externa por esse evento no MVP. Nova meta profissional entra automaticamente em vigor sem aceite do paciente, informando a alteração de modo discreto e preservando histórico; ele pode ajustar novamente se quiser. Registro retroativo aprovado: data/hora atuais sugeridas, mas consumo efetivo editável; contabilização no dia consumido e meta histórica dessa data. Fuso aprovado: detectar configuração do navegador sem geolocalização, permitir correção manual por refeição e preservar a data local/fuso do consumo, sem alterar históricos após viagens. Painel diário aprovado: mostrar kcal consumidas, meta efetiva, diferença restante/excedente e totais de proteínas, carboidratos e gorduras, atualizando após alterações no diário e respeitando a meta histórica de cada data. Resumos semanais/mensais aprovados: totais registrados de kcal e macronutrientes, médias divididas somente pelo número de dias com refeições registradas e quantidade desses dias; ausência de lançamentos não equivale a ingestão zero, nem dia registrado implica cobertura completa. **Peso e IMC aprovados:** registro de pesagens em qualquer data sem frequência obrigatória, gráfico da evolução, correção de medições sem afetar outras e IMC atual calculado com peso mais recente e altura válida do perfil, com explicitação de limites (não é avaliação completa da composição corporal). Metas opcionais aprovadas: qualquer adulto, inclusive usuário gratuito sem nutricionista, pode definir kcal, proteínas, carboidratos e gorduras independentemente; o profissional também pode definir metas, com ajustes do paciente sujeitos ao aviso/confirmação já acordados. Gráficos de consumo aprovados: visão semanal/mensal diária de calorias, com alternância para proteínas, carboidratos e gorduras; meta efetiva da data como referência e dias sem registros como sem dados, com alternativa textual acessível. Comparação por período confirmada: para calorias e cada macro selecionado, mostrar dias e contar quantos ficaram abaixo, acima ou na meta, usando a meta válida de cada data; não classificar dias sem registros nem dias sem meta. A leitura é sobre consumo **registrado**, que pode estar parcial. Próxima decisão: se **na meta** será igualdade exata ou uma faixa de tolerância. Saldos agregados e convenção de semana continuam pendentes. Detalhes técnicos de horário de verão, altura histórica e privacidade ficam para etapas 8/9. Privacidade, retenção e acesso à trilha ficam nas etapas 8/9. Ver [DIARIO.md](./DIARIO.md).

**Importante:** conclusão das 10 etapas encerra o planejamento, não significa que o software foi desenvolvido, validado ou colocado em produção.
