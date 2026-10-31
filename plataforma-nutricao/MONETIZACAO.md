# Monetização e assinaturas — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 7 de 10 — Monetização e assinaturas (em andamento)
> **Versão:** 0.2
> **Data:** 31/10/2026
> **Referências:** [IDEA.md](./IDEA.md), [ROADMAP.md](./ROADMAP.md), [PACIENTES.md](./PACIENTES.md), [DIARIO.md](./DIARIO.md), [DIETAS.md](./DIETAS.md)

## 1. Objetivo

Definir como a plataforma de nutrição se sustenta financeiramente **sem cobrar do usuário pelo uso da própria conta ou por aceitar acompanhamento nutricional dentro da plataforma**, preservando a simplicidade para usuários e nutricionistas.

**Terminologia comercial aprovada:** “usuário comum” e “paciente” são **a mesma pessoa e a mesma conta**. “Paciente” é o nome usado **na área do nutricionista** para os usuários sob seu acompanhamento/gestão; não é tipo de conta, plano, cadastro ou papel comercial separado. Ter ou não ter vínculo de acompanhamento não altera a gratuidade dos recursos pessoais.

## 2. Modelo aprovado para o MVP

1. **Todos os usuários adultos usam gratuitamente os recursos pessoais**, estejam ou não vinculados a um nutricionista: diário alimentar, registro de refeições, metas próprias, indicadores e gráficos, histórico de consumo, peso/IMC e planos alimentares pessoais definidos no MVP.
2. **O mesmo usuário pode aceitar ou recusar convite sem mudar de conta ou pagar.** Se houver vínculo autorizado, pode consultar dieta profissional atribuída, registrar refeições, acompanhar indicadores e manter seus dados pessoais. Apenas na gestão do nutricionista esse usuário será denominado **paciente**.
3. **O nutricionista é o assinante pagante.** Uma **assinatura mensal** habilita as ferramentas profissionais de gestão e acompanhamento, sujeitas à validação do CRN e às demais permissões: envio de convites, administração de vínculos, consulta aos dados de pacientes autorizados, publicação de dietas e metas profissionais, biblioteca e histórico de trabalho.
4. **Separar recursos pessoais e profissionais.** Uma conta de nutricionista pode continuar usando os recursos pessoais normais sem assinatura profissional; a aprovação do CRN é requisito distinto da assinatura.
5. **Dados do paciente não dependem da cobrança do nutricionista.** Diário, metas, histórico, medições e versões de dietas publicadas para o paciente não podem ser apagados, sequestrados ou indisponibilizados ao titular devido a cancelamento ou falha de pagamento do profissional.
6. **Pagamento não equivale a consentimento nem validação profissional.** A assinatura não dá acesso a dados de pacientes sem vínculo ativo e autorização, nem libera funções profissionais antes da aprovação do registro.
7. **Sem monetização adicional presumida:** anúncios, cobranças por paciente, funcionalidades pessoais pagas e comissão sobre consultas não foram aprovados para este MVP.

## 3. Experiência de uso aprovada

- **Usuário, com ou sem acompanhamento:** usa a mesma conta e não encontra checkout para criar conta, usar recursos pessoais, aceitar convite ou consultar plano que lhe foi publicado. O acompanhamento dentro da plataforma é gratuito; a interface pessoal não apresenta “paciente” como um perfil separado.
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
| Usuário gratuito recebe convite válido de nutricionista | Aceita na mesma conta e acompanha dieta/indicadores sem checkout nem mudança para “conta de paciente” |
| Usuário encerra acompanhamento e depois recebe novo convite | Mantém a mesma conta gratuita, dados e recursos pessoais; apenas o vínculo profissional muda |
| Nutricionista deseja usar ferramentas profissionais | A oferta comercial é uma assinatura mensal, condicionada também à validação do CRN |
| Nutricionista paga assinatura mas CRN ainda não foi validado | Pagamento não dispensa validação profissional nem concede acesso a dados de pacientes |
| Nutricionista tem assinatura mas não possui vínculo autorizado com um paciente | API bloqueia acesso aos dados desse paciente |
| Nutricionista cancela assinatura ou falha pagamento | Dados pessoais do paciente permanecem preservados; restrições operacionais serão definidas em decisão própria |
| Paciente encerra vínculo com nutricionista | Mantém conta e dados independentemente do estado comercial do profissional |

**Próxima decisão:** definir se haverá um único plano profissional mensal no lançamento ou múltiplos planos.

**Observação:** este arquivo registra requisitos aprovados; não representa implementação, testes ou cobrança em produção.
