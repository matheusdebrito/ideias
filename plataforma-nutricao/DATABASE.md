# Modelo de dados conceitual — para implementação posterior

> 01/11/2026. Nomes de tabelas são sugestões, não migrations nem esquema Prisma final.

## Entidades e relacionamentos
| Entidade sugerida | Responsabilidade / invariantes |
| --- | --- |
| users | UUID estável, nome, email único normalizado, nascimento 18+, status |
| identities / credentials | Senha Argon2id e ligação OIDC; vínculo seguro |
| sessions | Hash do token opaco, usuário, expiração por inatividade e absoluta, revogação |
| email_tokens | Tokens de verificação/recuperação hasheados, tipo, validade, consumo único |
| nutritional_sources / import_batches | Fonte, licença/atestado de uso, versão, data, resultado de importação |
| foods / food_compositions | Catálogo institucional por código estável + versão; nutrientes NULL quando desconhecidos |
| private_foods | Dono, nome, quantidade e unidade de referência, macros obrigatórios, kcal declarada/estimada, desativação |
| food_portion_conversions | Conversões específicas por alimento e preparo, unidade, fator, origem |
| favorite_foods | Identidade composta por usuário e alimento; nunca expor favoritos alheios |
| meal_templates / meal_template_items | Refeições reutilizáveis privadas, sem efeito retroativo |
| diary_meals / diary_items | Titular, data e hora local de consumo, fuso IANA, instante UTC, criação, itens com snapshot |
| goals / goal_versions | Metas opcionais efetivas por data, autoria e origem; não sobrescrever metas antigas |
| plans / plan_versions / plan_meals | Plano atual + histórico, autoria, refeições planejadas com snapshots; independente do diário |
| relationships | Gestor e acompanhado, vigência/status; **no máximo um vínculo ativo de gestor por titular** |
| invitations | Token/link hasheado, alvo opcional, validade 7d, status, consumo idempotente |
| notifications | Destinatário, categoria, estado de leitura, origem do evento |
| hydration_entries / body_measurements | Registros do titular com data; medição de peso e circunferências |
| admin_audit_events | Ações administrativas mínimas, sem copiar conteúdo nutricional em log |
| worker_jobs | Estruturas do Graphile Worker para tarefas assíncronas |

## Regras estruturais
- Restringir dados privados com consultas scoped pelo usuário autenticado e autorização de vínculo ativo **no servidor**; IDs UUID não são controle de acesso.
- Criar índice único parcial para relacionamento ativo por usuário acompanhado; aceitar convite/trocar gestor em transação com bloqueio ou constraint apropriada.
- Convites não devem ser consumidos duas vezes, inclusive sob concorrência, e rejeição não renova prazo.
- Retratos de itens consumidos conservam nome, origem, referência, quantidade, nutrientes e cobertura/precisão para reprodutibilidade histórica.
- NULL significa nutriente indisponível; 0 é resultado observado/declarado; manter sinalizador de kcal estimada.
- Totais e métricas da data nutricional usam **dia local do consumo**, não data de cadastro; fuso do registro e metas históricas são preservados.
- Excluir alimento privado usado significa desativar; não apagar dados snapshots do diário.
- Excluir refeição remove seus efeitos nos totais; retenção/auditoria de exclusões deve ter prazo legal definido, não retenção infinita implícita.
- Índices prováveis: usuário+data de consumo; alimento+busca; convites pendentes; gestor+status; notificações usuário+status.
- Constraints para numéricos finitos, quantidades >0, macros >=0, unicidade das identidades e versões; evitar regras complexas dependentes de unidade apenas em CHECK SQL.
- Mudanças críticas devem ter migrations reversíveis quando possível, compatibilidade entre código e schema, e teste de rollback/restauração.

## Antes de criar migrations
Detalhar cardinalidade de refeições/planos, visibilidade de snapshots privados de gestor, resolução de horário de verão, modelo de nutrientes com unidades, revogação e retenção LGPD, algoritmos de concorrência e estratégia de agregação.
