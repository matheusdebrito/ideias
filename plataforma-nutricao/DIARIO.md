# Diário alimentar, metas e indicadores — especificação atual

> Revisado em 01/11/2026. Alinhado a [REQUIREMENTS.md](./REQUIREMENTS.md).

## Consumo
O titular registra qualquer quantidade de refeições por dia, com nome identificador, data e hora reais de consumo, alimentos e porções. Pode copiar sugestão do plano ou refeição salva, editar o resultado e registrar — copiar não altera o modelo original e registrar não altera o plano. Horário do consumo e data de cadastro são distintos; preservar fuso IANA e dia **local do consumo** para agregações.

## Edição e exclusão retroativa
O titular pode editar/excluir inclusive em dias passados. Edição corrige o mesmo registro; se a data mudar, recalcular totais dos dois dias e períodos afetados. Exclusão retira contribuição dos totais. Operações transacionais, autenticação e idempotência em registro/reenvio. **Sem histórico integral de versões por refeição no MVP**; metadados mínimos de auditoria somente quando necessários para segurança/compliance, sujeitos a retenção definida (não presumir trilha ilimitada).

Gestores com vínculo ativo e consentimento **podem visualizar, não alterar nem excluir refeições do titular**. Ex-gestor não pode visualizar. Um plano atualizado não reescreve refeições passadas.

## Nutrição
O backend recalcula e salva snapshot de cada alimento/porção e nutrientes conhecidos. Frontend apresenta prévias instantâneas. Ausência em alimento institucional significa desconhecido. Quando pelo menos um item não fornece determinado nutriente, o agregado desse nutriente deve ser rotulado **incompleto** e mostrar apenas o consumo conhecido, sem afirmar total exato. Alimento personalizado exige macros obrigatórios; kcal estimadas são rotuladas como tal.

## Metas e datas
Metas de kcal, proteína, carboidrato e gordura são opcionais e individualmente editáveis; gestor pode propor metas mediante vínculo. Titular pode alterar meta proposta pelo gestor com confirmação. Conservar autoria e vigência, comparando consumo histórico com **meta efetiva naquela data**, nunca com a meta corrente automaticamente.

## Interface e indicadores
- Dashboard «Meu dia»: kcal, proteínas, carboidratos, gorduras consumidos vs metas quando houver; refeições e hidratação.
- Ao expandir refeição: nutrientes por alimento quando disponíveis. Página de detalhes da data: vitaminas/minerais **somente quantidades**, sem metas, alertas de deficiência ou avaliação clínica.
- Semanal e mensal: totais, indicadores por dia e comparação com metas históricas válidas, distinguindo dia sem registros de dia registrado com consumo zero. Evolução de 7/30/90 dias.
- Refeições por horário real de consumo, não pelo horário recomendado no plano.
- Água registrada à parte, com meta opcional e edições retroativas. Peso e circunferências (cintura, quadril, braço, coxa, peito), com data e gráficos; IMC adulto informativo quando houver peso e altura, não diagnóstico. Gestor apenas visualiza.

## Verificações
Autorização por titular e vínculo, fuso horário e alterações de data, totais completos vs parciais, metas com vigência, ausência de duplicação por reenvio, concorrência de edição, comportamento sem registros, responsividade.
