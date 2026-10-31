# Gestão de pacientes, vínculos e permissões — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 6 de 10 — Gestão de pacientes e permissões (em andamento)
> **Versão:** 0.3
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
7. **Canal confirmado no MVP: e-mail.** O nutricionista informa o **e-mail do paciente**; a plataforma envia uma mensagem com acesso ao fluxo de convite, sem incluir diário, peso, metas, dietas nem outros dados sensíveis no conteúdo do e-mail.
8. **Paciente com conta:** ao acessar o convite, precisa fazer login e ter o endereço destinatário vinculado e verificado na conta. Só após visualizar quem convida e os dados solicitados poderá aceitar e autorizar explicitamente.
9. **Paciente sem conta:** o convite permite seguir para o cadastro de usuário adulto, com verificação do endereço destinatário, e depois retornar à análise do convite; cadastrar-se **não equivale a aceitar**.
10. **Proteção de identidade:** o servidor valida que a conta autenticada corresponde ao endereço destinatário verificado. Ter posse do link ou visualizar o e-mail, isoladamente, não concede permissão sobre dados pessoais nem acesso profissional. Respostas de criação/envio não devem revelar indevidamente se o destinatário já possui conta.
11. **Em aberto:** prazo de validade e renovação de convites, tratamento de envio falho, limites contra abuso e detalhes da verificação de e-mail/tokens; desenho técnico na etapa 9. Permanecem pendentes a granularidade da autorização e as regras de troca entre profissionais.

## 4. Convite por e-mail para pacientes com ou sem conta (**confirmado no MVP**)

- O nutricionista informa o e-mail do paciente para iniciar o convite. Não há necessidade de o paciente já estar cadastrado.
- A mensagem conduz à plataforma para login ou cadastro, sem transmitir dados nutricionais ou de saúde no e-mail.
- **Conta existente:** usuário entra com a conta que possui o endereço de e-mail destinatário verificado, consulta o convite e decide se aceita/autoriza.
- **Conta inexistente:** usuário conclui cadastro 18+, valida o e-mail convidado e só então consulta e decide sobre o convite.
- O link do convite é um acesso ao fluxo de decisão, **não uma credencial suficiente para ler dados do paciente nem um aceite automático**.
- A API verifica destinatário e autorização no momento de ativar vínculo; status da conta do destinatário não deve ser exposto indevidamente ao convidador.
- Prazo de validade, reenvio, política de tentativas e detalhes do serviço de entrega serão definidos separadamente; a decisão presente não fixa um prazo.

## 5. Critérios de aceitação inicial

| Cenário | Comportamento esperado |
| --- | --- |
| Paciente está sem nutricionista | Usa recursos pessoais e seu histórico normalmente |
| Paciente já possui nutricionista ativo e outro tenta criar vínculo ativo | Segundo vínculo simultâneo não é autorizado |
| Paciente troca de nutricionista | Mantém diário, medições, metas pessoais e histórico de dietas, com autoria original |
| Profissional anterior tenta abrir o painel de dados do paciente depois do encerramento | Consulta de acompanhamento é negada no servidor |
| Novo profissional tenta acessar o paciente sem vínculo e autorização válida | Acesso negado; não herda compartilhamento anterior |
| Duas tentativas concorrentes procuram ativar profissionais diferentes para o mesmo paciente | No máximo um vínculo se torna ativo, com garantia de integridade do sistema |
| Nutricionista convida paciente sem vínculo | Convite pendente, nenhum acesso aos dados do paciente |
| Nutricionista informa o e-mail de pessoa que ainda não tem conta | Enviar convite para que ela crie e verifique a conta antes de autorizar, sem criar vínculo antecipadamente |
| Paciente já possui conta com o e-mail destinatário verificado | Pode entrar na plataforma, consultar quem convidou e aceitar ou recusar |
| Pessoa abre o convite sem login ou com conta de outro e-mail | Não ativa vínculo e não concede acesso a dados; identidade do destinatário é exigida |
| Pessoa cria conta após receber convite | Cadastro não aceita o convite automaticamente; exige confirmação expressa após autenticação |
| Convite é visualizado ou ignorado | Não ocorre aceite tácito, vínculo nem compartilhamento |
| Paciente recusa convite | Nenhum vínculo ou acesso é criado |
| Paciente autenticado aceita e autoriza categorias informadas | Vínculo ativado somente após checagem do destinatário e exclusividade; permissões delimitam o acesso |
| Nutricionista tenta consultar dados antes do aceite | Servidor nega acesso |
| Paciente já tem nutricionista ativo e recebe segundo convite | Não substitui profissional nem concede acesso automaticamente |

## 6. Decisões ainda em aberto na etapa 6

- **Entrega dos convites:** canal por **e-mail** e destinatário identificado pelo endereço, com login ou cadastro e confirmação da identidade antes do aceite, já confirmados. Pendem expiração, reenvio, limites contra abuso e granularidade da autorização. Não expor se determinada pessoa possui conta a usuários não autorizados.
- **Troca:** quem pode iniciar, quando o vínculo anterior termina e como evitar janela com dois profissionais simultâneos.
- **Planos profissionais antigos:** se uma dieta profissional vigente do nutricionista anterior deve encerrar automaticamente ou continuar apenas para consulta após a troca. Não alterar essa regra por suposição.
- **Permissões específicas:** quais informações de diário, peso, metas e planos podem ser acessadas pelo profissional autorizado; revogação pelo paciente.
- **Registro do nutricionista:** verificação do CRN e restrições de acesso antes da validação.
- **Dados profissionais após encerramento:** guarda obrigatória, eliminação, portabilidade e outros requisitos da LGPD na etapa 8.

**Observação:** esta especificação registra apenas decisões funcionais; não há implementação do sistema nesta fase.
