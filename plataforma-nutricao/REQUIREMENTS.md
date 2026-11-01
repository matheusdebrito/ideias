# Requisitos e regras de negócio — MVP

> Consolidado em 01/11/2026. Status: aprovado em planejamento, não implementado. Este documento prevalece sobre textos anteriores **apenas quando registrar decisão mais recente**.

## 1. Contas e acesso
- Cadastro restrito a maiores de 18 anos; uma conta serve tanto para usuário pessoal quanto para gestor e acompanhado.
- E-mail único, sem alteração de e-mail no MVP; verificação por link de uso único (24 h); redefinição de senha por link de uso único (30 min). Respostas de recuperação não revelam existência de conta.
- Login por senha e Google OIDC, com vinculação de identidades segura. Sessões opacas em cookie HttpOnly/Secure/SameSite; sessões armazenadas no PostgreSQL como hashes. Não usar JWT nem localStorage para autenticação.
- Proteções contra enumeração, força bruta, CSRF e acesso cruzado entre usuários. Papel administrativo separado, sem acesso geral ao diário dos usuários.

## 2. Catálogo nutricional
- TBCA é fonte **prioritária condicionada à confirmação de licença e direito de uso**; catálogo local independente da origem; importação manual, validada e versionada, não sincronizada automaticamente.
- Procurar com PostgreSQL: busca incremental, sem acentos/case, ordenada por relevância e limitada/paginada; priorizar alimentos preparados. Alimentos consumidos crus naturalmente continuam permitidos.
- Referências nutricionais por quantidade e unidade explícitas; gramas ou mililitros padrão; equivalências caseiras apenas por alimento/preparo quando comprovadas. Não converter peso cru em cozido nem massa em volume sem densidade confiável.
- Alimentos importados **podem ser usados mesmo quando nutrientes estiverem ausentes**; marcar cobertura parcial por nutriente nos totais e nunca interpretar ausência como zero.
- Alimento personalizado privado: **nome, quantidade de referência, proteína, carboidrato e gordura são obrigatórios**; zero é válido para macros, vazio não. Calorias opcionais: se ausentes, estimar pela fórmula 4P + 4C + 9G, rotulando o valor como estimado; se declaradas, priorizar declarado. Fibras, vitaminas e minerais opcionais.
- Validar quantidade positiva, macros não negativos, valores finitos, unidade coerente, e impossibilidades físicas somente quando comparáveis e fundamentadas; diferenças normais entre kcal e macros são aceitas.
- Usuário pode editar/excluir alimento privado. Alimentos já referenciados são desativados no catálogo, preservando snapshots usados; nunca usados podem ser eliminados. Sem compartilhamento público. Gestor pode usar alimento privado em plano de acompanhado por snapshot autorizado, sem abrir seu catálogo particular.
- Favoritos e recentes por usuário; refeições salvas/repetidas são distintas de favoritos de alimentos.

## 3. Cálculos, consistência e histórico
- Prévia nutricional imediata no frontend, sem requisição por alteração de quantidade; backend ignora totais do cliente, valida, recalcula e persiste snapshot histórico.
- Precisão interna consistente e apresentação arredondada; não arredondar etapas intermediárias. Testar paridade entre prévia e backend.
- Totais incompletos indicam explicitamente o nutriente com cobertura parcial; não apresentar soma conhecida como total exato.
- Mudança de catálogo, plano ou porção padronizada não altera retroativamente refeições já registradas.

## 4. Diário, metas e evolução
- Usuário pode criar, editar e excluir refeições, inclusive retroativamente, com data/hora real de consumo e fuso correto. Totais e gráficos são recalculados no dia do consumo (inclusive ao alterar a data). Edição transacional; sem histórico completo de versões por refeição no MVP.
- Refeição pode vir de sugestão de plano, de modelo salvo ou ser avulsa. O usuário pode alterar livremente alimentos/quantidades consumidos. Registrar não altera o plano.
- Gestor autorizado visualiza diário e evolução, mas **não edita nem exclui registros de consumo**.
- Dashboard: kcal e proteínas/carboidratos/gorduras consumidos versus metas opcionais, refeições e hidratação. Visão detalhada da data com vitaminas/minerais **apenas quantidades**, sem metas nem inferências clínicas.
- Histórico semanal/mensal e evolução em 7/30/90 dias, incluindo distinção entre dia sem registro e consumo igual a zero. Metas têm vigência temporal, são editáveis pelo titular com confirmação quando definidas pelo gestor.
- Água registrada separadamente por data, com meta opcional; medidas de peso e circunferências por data, com gráfico histórico e IMC adulto informativo, sem diagnóstico.

## 5. Planos alimentares e gestores
- Um plano alimentar atual editável por titular; versões preservam referências anteriores. Planejamento é sugestão, não consumo automático.
- Gestor monta refeições com alimentos e quantidades, horários apenas sugestivos, observações opcionais, e pode usar biblioteca privada de modelos de planos. Copiar modelo não cria dependência mutável entre plano individual e biblioteca.
- Vinculação por convite aceito voluntariamente; um gestor ativo por titular. Troca de gestor exibe aviso claro identificando gestor anterior/novo e revoga acesso anterior atomicamente, sem apagar dados.
- Convites por busca interna (nome com exposição limitada ou e-mail exato) ou link individual de WhatsApp de uso único; para pessoas sem cadastro, exigir entrar/cadastrar antes de aceitar. Validade de 7 dias, rejeição não prolonga validade, idempotência e limites antiabuso. Convites pendentes não contam na capacidade futura.
- Gestor pode encerrar vínculo; titular pode encerrar ou trocar. Histórico administrativo mínimo, sem acesso do ex-gestor aos dados nutricionais.
- Notificações in-app com consulta periódica HTTP; sem chat interno, push, WebSocket ou convite por e-mail.

## 6. Beta e modelo comercial
- Durante beta público inicial, todas as funcionalidades de uso pessoal e gestão disponíveis sem pagamento; aviso transparente de gratuidade temporária e mudanças futuras com antecedência. Não cobrar automaticamente.
- Posteriormente, recursos pessoais continuam gratuitos; gestão de terceiros e biblioteca profissional poderão exigir plano pago. Preço, limites por gestor, gateway e economia unitária **a decidir depois de estimar custos**, antes da monetização.
- Ao perder elegibilidade futura para gestão, preservar conta e dados; nunca excluir automaticamente vínculos ou registros. Definir claramente políticas de suspensão de recursos.

## Critérios de aceitação transversais
1. Usuário A não acessa dados, favoritos, alimentos privados, planos ou convites de usuário B sem vínculo/permissão explícitos.
2. Repetir requisições críticas e operar simultaneamente não gera vínculos/consumos duplicados nem saldos incorretos.
3. Um alimento sem dado de gordura não é mostrado como 0 g nem faz o total parecer completo.
4. Alterar um alimento cadastrado não muda refeição histórica.
5. Corrigir refeição de outra data modifica os agregados dos dias afetados, não o dia da edição.
6. A troca de gestor nunca deixa dois gestores ativos e retira leitura nutricional do antigo.
7. Erros de validação identificam campos, não expõem dados internos e preservam a edição do usuário.
