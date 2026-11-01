# Monetização e beta — decisões e pendências

> Revisado em 01/11/2026. **Não há pagamentos implementados nem preços definidos.** Ver [REQUIREMENTS.md](./REQUIREMENTS.md).

## Primeiro lançamento: beta gratuito
Durante o beta, funcionalidades pessoais e de gestão de terceiros ficam acessíveis sem pagamento. Apresentar aviso honesto de **gratuidade por tempo limitado**, sem anunciar prazo de encerramento não definido. Não coletar cartão para cobrança futura nem iniciar cobranças automáticas. Informar antecipadamente quando novas condições de uso forem definidas.

## Modelo futuro em avaliação
- Recursos pessoais de diário, metas, hidratação, históricos e planos próprios devem permanecer disponíveis gratuitamente.
- Gestão de terceiros e biblioteca de modelos para gestores são possíveis recursos pagos, sem exigir CRN. Selo CRN verificado é independente de assinatura.
- Possíveis planos por número de usuários acompanhados, com preço, limites e regras de elegibilidade **a decidir após análise de custos e métricas reais**.
- Ao reduzir/encerrar assinatura futura, preservar dados e vínculos, impedir funções pagas conforme regras explícitas; jamais escolher automaticamente pessoas para excluir. Se houver excesso de capacidade, solicitar ao gestor regularizar sua lista.
- Evitar cobrança automática sem consentimento expresso e comunicação clara; cobranças, reembolsos, webhooks e idempotência só serão desenhados quando entrarem no escopo de monetização.

## Métricas para decidir
Cadastros, atividade em 7/30 dias, refeições registradas, pessoas ativamente acompanhadas, planos usados, retenção dias 7/30, custos efetivos de hospedagem, e-mail e banco, suporte e custos variáveis por usuário/gestor. Registrar apenas o necessário, sem expor dados nutricionais em telemetria genérica.

## Pendências obrigatórias antes de monetizar
Custos operacionais por cenário; definição de preços, quantidade máxima de acompanhados, política de transição do beta, cobrança/reembolsos, consentimento e comunicados. **Nenhuma pendência comercial bloqueia iniciar desenvolvimento local ou beta gratuito**, mas LGPD e segurança bloqueiam acesso público quando não atendidas.

## Não incluir agora
Gateway de pagamentos, checkout, assinaturas ativas, ciclos de renovação, tolerância de cobrança de 7 dias, desconto e plano de teste com duração fixa. Esses eram cenários anteriores, não decisões vigentes.
