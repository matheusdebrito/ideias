# Gestão de pacientes, vínculos e permissões — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 6 de 10 — Gestão de pacientes e permissões (em andamento)
> **Versão:** 0.2
> **Data:** 31/10/2026
> **Referências:** [IDEA.md](./IDEA.md), [DIETAS.md](./DIETAS.md), [DIARIO.md](./DIARIO.md)

## 1. Objetivo

Definir vínculos e autorização entre pacientes adultos e nutricionistas, preservando a titularidade dos dados pessoais e o histórico do acompanhamento durante mudanças de profissional.

## 2. Decisão aprovada para o MVP — vínculo profissional único

1. **No máximo um nutricionista ativo por paciente**, a qualquer instante. Um paciente pode ficar **sem nutricionista** e continuar utilizando os recursos pessoais gratuitos e o próprio diário.
2. **Troca permitida:** o paciente pode encerrar seu vínculo e ter um novo nutricionista sem perder o próprio diário alimentar, histórico de refeições, medições de peso, metas pessoais, indicadores ou acesso à conta.
3. **Histórico preservado:** dietas e prescrições profissionais anteriores permanecem identificadas pelo autor e por sua vigência, sujeitas às regras legais de retenção/eliminação da etapa 8. Não reatribuir uma dieta antiga ao novo nutricionista e não sobrescrever histórico nutricional.
4. **Sem acesso ativo do ex-profissional:** depois que o vínculo ou a autorização terminar, o ex-nutricionista não tem mais acesso aos dados atuais ou históricos do paciente **pelo painel de acompanhamento**. Eventual guarda obrigatória de documentação profissional é um assunto jurídico separado, não uma autorização contínua de consulta à conta.
5. **Nova autorização independente:** qualquer acesso de um novo nutricionista a dados privados requer vínculo ativo e autorização válida; não herdar permissões ou compartilhamento do profissional anterior.
6. **Controle efetivo:** servidor e banco devem impedir mais de um vínculo ativo simultâneo, inclusive em tentativas concorrentes. RLS/autorização e integridade da transição serão projetadas na etapa 9. O cliente não é fonte de verdade para permissões.
7. **Escopo adulto:** somente usuários com 18 anos completos ou mais podem estabelecer vínculos no MVP.

## 3. Convite enviado pelo profissional e aceite do paciente (**confirmado no MVP**)

1. **Convite iniciado pelo nutricionista:** o profissional convida o paciente para acompanhamento; o convite pendente não cria vínculo ativo nem permite acesso ao diário, peso, metas, dietas ou dados privados.
2. **Aceite e autorização expressos:** o paciente destinatário deve estar autenticado, saber quem convida, o objetivo do acompanhamento e quais categorias de dados serão compartilhadas, e **aceitar e autorizar explicitamente** para ativar o vínculo. Receber o convite, criar conta ou apenas visualizá-lo não constitui autorização.
3. **Recusa e ausência de resposta:** o paciente pode recusar ou não responder, sem ativar vínculo nem conceder acesso ao profissional.
4. **Validação efetiva:** identidade do destinatário, aceite e autorização devem ser verificados no servidor. Convites não podem conceder acesso a terceiros nem ser usados por outra conta. Detalhes técnicos de identidade, tokens e expiração serão tratados na etapa 9.
5. **Sem substituição automática:** se o paciente tiver outro nutricionista ativo, o convite não substitui o vínculo existente nem libera acesso. O fluxo de troca será definido separadamente.
6. **Rastreabilidade proporcional:** registrar autoria do convite, aceite e autorização, quando ocorreram e início do vínculo, sujeitos às definições de retenção da etapa 8.
7. **Em aberto:** canal do convite (e-mail, link ou aviso interno), identificação do destinatário, prazo de validade e granularidade dos dados autorizáveis. Esta decisão confirma o ator que convida e a condição de ativação do vínculo, não o canal de entrega.

## 4. Critérios de aceitação inicial

| Cenário | Comportamento esperado |
| --- | --- |
| Paciente está sem nutricionista | Usa recursos pessoais e seu histórico normalmente |
| Paciente já possui nutricionista ativo e outro tenta criar vínculo ativo | Segundo vínculo simultâneo não é autorizado |
| Paciente troca de nutricionista | Mantém diário, medições, metas pessoais e histórico de dietas, com autoria original |
| Profissional anterior tenta abrir o painel de dados do paciente depois do encerramento | Consulta de acompanhamento é negada no servidor |
| Novo profissional tenta acessar o paciente sem vínculo e autorização válida | Acesso negado; não herda compartilhamento anterior |
| Duas tentativas concorrentes procuram ativar profissionais diferentes para o mesmo paciente | No máximo um vínculo se torna ativo, com garantia de integridade do sistema |
| Nutricionista convida paciente sem vínculo | Convite pendente, nenhum acesso aos dados do paciente |
| Convite é visualizado ou ignorado | Não ocorre aceite tácito, vínculo nem compartilhamento |
| Paciente recusa convite | Nenhum vínculo ou acesso é criado |
| Paciente autenticado aceita e autoriza categorias informadas | Vínculo ativado somente após checagem do destinatário e exclusividade; permissões delimitam o acesso |
| Nutricionista tenta consultar dados antes do aceite | Servidor nega acesso |
| Paciente já tem nutricionista ativo e recebe segundo convite | Não substitui profissional nem concede acesso automaticamente |

## 5. Decisões ainda em aberto na etapa 6

- **Entrega dos convites:** iniciativa do nutricionista e aceite/autorização do paciente já definidos. Pendente escolher o canal (e-mail, link ou área interna), identificar destinatário, expiração e granularidade da autorização. Não expor se determinada pessoa possui conta a usuários não autorizados.
- **Troca:** quem pode iniciar, quando o vínculo anterior termina e como evitar janela com dois profissionais simultâneos.
- **Planos profissionais antigos:** se uma dieta profissional vigente do nutricionista anterior deve encerrar automaticamente ou continuar apenas para consulta após a troca. Não alterar essa regra por suposição.
- **Permissões específicas:** quais informações de diário, peso, metas e planos podem ser acessadas pelo profissional autorizado; revogação pelo paciente.
- **Registro do nutricionista:** verificação do CRN e restrições de acesso antes da validação.
- **Dados profissionais após encerramento:** guarda obrigatória, eliminação, portabilidade e outros requisitos da LGPD na etapa 8.

**Observação:** esta especificação registra apenas decisões funcionais; não há implementação do sistema nesta fase.
