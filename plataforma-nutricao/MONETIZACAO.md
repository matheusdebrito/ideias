# Monetização e assinaturas — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 7 de 10 — Monetização e assinaturas (em andamento)
> **Versão:** 0.1
> **Data:** 31/10/2026
> **Referências:** [IDEA.md](./IDEA.md), [ROADMAP.md](./ROADMAP.md), [PACIENTES.md](./PACIENTES.md), [DIARIO.md](./DIARIO.md), [DIETAS.md](./DIETAS.md)

## 1. Objetivo

Definir como a plataforma de nutrição se sustenta financeiramente **sem cobrar do paciente pelo uso da própria conta ou pelo acompanhamento dentro da plataforma**, preservando a simplicidade para paciente e nutricionista.

## 2. Modelo aprovado para o MVP

1. **Usuários comuns e pacientes não pagam assinatura.** Adultos podem usar gratuitamente as ferramentas pessoais mesmo sem nutricionista: diário alimentar, registro de refeições, metas próprias, indicadores e gráficos, histórico de consumo, registro de peso/IMC e planos alimentares pessoais definidos no MVP.
2. **Paciente acompanhado também não paga para utilizar a plataforma.** Pode aceitar ou recusar convite, consultar dieta profissional atribuída, registrar refeições, acompanhar indicadores e manter seus próprios dados, sem assinatura pessoal.
3. **O nutricionista é o assinante pagante.** Uma **assinatura mensal** habilita as ferramentas profissionais de gestão e acompanhamento, sujeitas à validação do CRN e às demais permissões: envio de convites, administração de vínculos, consulta aos dados de pacientes autorizados, publicação de dietas e metas profissionais, biblioteca e histórico de trabalho.
4. **Separar recursos pessoais e profissionais.** Uma conta de nutricionista pode continuar usando os recursos pessoais normais sem assinatura profissional; a aprovação do CRN é requisito distinto da assinatura.
5. **Dados do paciente não dependem da cobrança do nutricionista.** Diário, metas, histórico, medições e versões de dietas publicadas para o paciente não podem ser apagados, sequestrados ou indisponibilizados ao titular devido a cancelamento ou falha de pagamento do profissional.
6. **Pagamento não equivale a consentimento nem validação profissional.** A assinatura não dá acesso a dados de pacientes sem vínculo ativo e autorização, nem libera funções profissionais antes da aprovação do registro.
7. **Sem monetização adicional presumida:** anúncios, cobranças por paciente, funcionalidades pessoais pagas e comissão sobre consultas não foram aprovados para este MVP.

## 3. Experiência de uso aprovada

- **Paciente:** não encontra etapa de checkout para criar conta, usar os recursos pessoais, aceitar convite ou consultar um plano que lhe foi publicado. O acompanhamento dentro da plataforma é gratuito para ele.
- **Nutricionista:** identifica com clareza que as funcionalidades profissionais são oferecidas por assinatura mensal, sem misturar pagamento e dados de pacientes. Detalhes da oferta e do fluxo de assinatura ainda serão decididos.
- **Simplicidade:** manter um modelo comercial inteligível; não criar camadas de planos, cobrança ou controles de recurso antes da decisão explícita do produto.

## 4. Questões a decidir na etapa 7

- **Estrutura da oferta:** um único plano profissional mensal ou vários planos/tierings.
- **Limites do plano:** pacientes ativos, armazenamento, solicitações e eventuais regras de uso justo, sem inventar limites antes da análise.
- **Preço e condições comerciais:** moeda, valor, período de teste, descontos ou promoção, se houver.
- **Pagamento:** provedor, renovação recorrente, confirmação, comprovantes e comunicação de falhas.
- **Cancelamento/inadimplência:** comportamento exato do acesso profissional, vínculo com pacientes e continuidade de dietas, sempre preservando dados do paciente e sem fingir que o acompanhamento foi encerrado.
- **Estados de assinatura:** definição do ciclo de vida comercial e mensagens adequadas para o nutricionista.

Regras técnicas de cobrança, idempotência, webhooks, segurança e integração serão detalhadas na etapa 9. Proteção de dados, retenção e responsabilidades na etapa 8.

## 5. Critérios de aceitação iniciais

| Cenário | Resultado esperado |
| --- | --- |
| Adulto sem nutricionista cria conta | Consegue usar diário, metas, gráficos, histórico e planos pessoais sem pagar |
| Paciente gratuito recebe convite válido de nutricionista | Pode aceitar e acompanhar dieta/indicadores sem checkout |
| Nutricionista deseja usar ferramentas profissionais | A oferta comercial é uma assinatura mensal, condicionada também à validação do CRN |
| Nutricionista paga assinatura mas CRN ainda não foi validado | Pagamento não dispensa validação profissional nem concede acesso a dados de pacientes |
| Nutricionista tem assinatura mas não possui vínculo autorizado com um paciente | API bloqueia acesso aos dados desse paciente |
| Nutricionista cancela assinatura ou falha pagamento | Dados pessoais do paciente permanecem preservados; restrições operacionais serão definidas em decisão própria |
| Paciente encerra vínculo com nutricionista | Mantém conta e dados independentemente do estado comercial do profissional |

**Próxima decisão:** definir se haverá um único plano profissional mensal no lançamento ou múltiplos planos.

**Observação:** este arquivo registra requisitos aprovados; não representa implementação, testes ou cobrança em produção.
