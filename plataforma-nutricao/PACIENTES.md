# Gestão de pacientes, vínculos e permissões — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 6 de 10 — Gestão de pacientes e permissões (em andamento)
> **Versão:** 0.7
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
11. **Validade aprovada no MVP:** um convite por e-mail pode ser aceito ou recusado durante **7 dias corridos (168 horas) a partir de sua emissão**, conforme instante registrado no servidor. Após o prazo, o convite fica **expirado**, não pode ativar vínculo e não concede acesso.
12. **Reenvio aprovado no MVP:** o nutricionista pode reenviar um convite ainda pendente ou expirado, gerando **um novo convite com novo prazo de 7 dias** e invalidando o link/convite anterior de forma atômica. Links antigos, expirados ou substituídos não podem ser aceitos. Um convite já aceito não pode ser reenviado como se o vínculo estivesse pendente; nova solicitação após recusa é outro convite, sujeito a controles contra abuso.
13. **Retorno ao nutricionista aprovado:** o profissional que enviou o convite poderá consultar no **painel profissional** seu estado atual: **pendente**, **aceito**, **recusado**, **expirado** ou **substituído por reenvio**, com data/hora da decisão quando houver. Aceite corresponde à ativação efetiva do vínculo, após verificação de identidade, autorização e exclusividade; falhas de ativação não podem aparecer como aceitas.
14. **Privacidade da recusa:** o profissional pode ver que o convite foi recusado, **sem justificativa obrigatória**, sem acesso a dados privados do paciente e sem saber se o destinatário possui conta apenas pelo ato de enviar o convite. Não confundir estado enviado com entrega confirmada do e-mail.
15. **Notificação por e-mail ao nutricionista confirmada para o MVP:** após a decisão **aceito ou recusado** ser registrada, enviar uma mensagem ao e-mail verificado da conta profissional **que originou o convite**, com resultado e acesso à área de convites no painel. A mensagem não deve conter valores nutricionais, peso, metas, histórico de saúde ou justificativa de recusa. O status no painel permanece a fonte de verdade.
16. **Consistência e falhas:** a notificação só pode refletir um estado efetivamente confirmado no servidor: **aceito** exige autorização e vínculo ativado; **recusado** exige recusa real. Falha no envio não reverte a decisão, não impede o paciente de concluir o fluxo e não apresenta sucesso de entrega não confirmado. Processamento assíncrono, retries e prevenção de e-mails duplicados serão especificados na etapa 9.
17. **Em aberto:** detalhes do provedor de e-mail, falhas de entrega, limites/prevenção de reenvios abusivos, granularidade da autorização e regras de troca. Não ampliar automaticamente o envio de e-mails para outros eventos (ex.: mudança de metas) fora das decisões já aprovadas.

## 4. Convite por e-mail para pacientes com ou sem conta (**confirmado no MVP**)

- O nutricionista informa o e-mail do paciente para iniciar o convite. Não há necessidade de o paciente já estar cadastrado.
- A mensagem conduz à plataforma para login ou cadastro, sem transmitir dados nutricionais ou de saúde no e-mail.
- **Conta existente:** usuário entra com a conta que possui o endereço de e-mail destinatário verificado, consulta o convite e decide se aceita/autoriza.
- **Conta inexistente:** usuário conclui cadastro 18+, valida o e-mail convidado e só então consulta e decide sobre o convite.
- O link do convite é um acesso ao fluxo de decisão, **não uma credencial suficiente para ler dados do paciente nem um aceite automático**.
- A API verifica destinatário e autorização no momento de ativar vínculo; status da conta do destinatário não deve ser exposto indevidamente ao convidador.
- Prazo de validade, reenvio, política de tentativas e detalhes do serviço de entrega serão definidos separadamente; a decisão presente não fixa um prazo.

## 5. Validade, reenvio e acompanhamento do convite (**confirmado no MVP**)

- Cada convite criado tem **7 dias corridos / 168 horas de validade a contar da emissão**. Um link expirado é recusado pelo servidor, ainda que o usuário consiga abrir a página.
- Ao reenviar um convite pendente ou expirado, gerar **um novo identificador/link e uma nova validade de sete dias**, invalidando o anterior. O reenvio não reaproveita o link antigo.
- O nutricionista vê no próprio painel, para convites enviados por ele, os estados **Pendente**, **Aceito**, **Recusado**, **Expirado** e **Substituído** (este último para convites invalidados por reenvio), e o momento de atualização quando aplicável. Estados devem corresponder ao estado real no servidor.
- O estado **Aceito** só é apresentado se o paciente correto tiver aceitado/autorizado e o vínculo estiver efetivamente ativado. A recusa mostra **Recusado**, sem exigir nem mostrar motivo e sem dar acesso a dados privados.
- Convites antigos não podem alterar um estado terminal nem criar vínculo depois de expirar ou ser substituídos. Registrar transições apenas uma vez e proteger contra aceite/recusa concorrentes.
- **Aviso adicional por e-mail aprovado:** após **aceite com vínculo efetivamente ativado** ou **recusa registrada**, enviar ao nutricionista que convidou uma mensagem transacional com o resultado e referência ao painel profissional. O e-mail não deve expor dados nutricionais ou de saúde, conteúdo do diário nem motivos da recusa.
- **Fonte de verdade:** a resposta no painel é mantida mesmo se o serviço de e-mail falhar. Envios e retentativas não podem criar novo vínculo nem modificar o estado do convite.

## 6. Escopo do acompanhamento e aceite simples (**confirmado no MVP**)

**Dados apresentados para autorização do paciente ao aceitar o convite:**
- **Diário alimentar:** refeições registradas e composição nutricional consumida.
- **Metas alimentares:** metas de calorias e macronutrientes, incluindo as referências profissionais e ajustes efetivos relevantes ao acompanhamento.
- **Gráficos e indicadores:** resumos, totais e evolução do consumo registrado.
- **Medidas corporais:** peso registrado, evolução do peso e IMC quando calculável.
- **Planos alimentares profissionais:** dietas profissionais vinculadas ao acompanhamento autorizado, com identificação da autoria e versões pertinentes.

**Experiência de autorização:** uma tela curta com nome do nutricionista e resumo legível dessas categorias, ações inequívocas **Aceitar e autorizar** e **Recusar convite**, e acesso opcional a **Ver detalhes**. Sem sequência extensa de formulários nem chaves individuais para cada categoria neste MVP. A ausência de chaves separadas não autoriza ampliar a lista de dados compartilhados: mudanças materiais do escopo exigem informação clara e nova autorização quando aplicável. O aceite não pode vir pré-selecionado ou ser inferido da navegação.

**Acesso e proteção:** o profissional autenticado só consulta os dados aprovados de paciente com **vínculo ativo e autorização vigente**. A API/banco rejeitam consulta de outras categorias e de outros pacientes, inclusive tentativas diretas a endpoints; o paciente não perde propriedade ou permissão de editar seus próprios registros. A autorização de acesso **não permite ao nutricionista editar refeições consumidas ou medidas do paciente por padrão**; suas ações de prescrição seguem permissões próprias já planejadas. O paciente deve conseguir identificar o profissional vinculado e encontrar o caminho para encerrar/revogar o vínculo; detalhes desse fluxo serão definidos nesta etapa.

**Histórico anterior ao vínculo (confirmado no MVP):** o mesmo aceite explícito em tela única autoriza também consultar **o diário alimentar e consumo registrado, metas e indicadores relacionados, histórico de peso e IMC já existentes antes da ativação do vínculo**, sem exigir um segundo fluxo. A tela deve informar de forma simples: **"Seu nutricionista também poderá consultar seus registros anteriores ao início do acompanhamento"**, com detalhes opcionais. O paciente não precisa enviar relatórios ou recadastrar registros.

- A consulta inclui dados registrados pelo próprio paciente nas categorias autorizadas, independentemente de terem sido criados antes ou depois da ativação do vínculo, inclusive lançamentos retroativos. Não estabelecer recorte arbitrário que prejudique a evolução histórica, sujeito à conservação efetiva de registros e à exclusão/retificação pelo titular.
- **Não compartilhar automaticamente acervos privados de outros profissionais**: prontuários próprios, bibliotecas privadas, documentos ou materiais não pertencentes à conta do paciente continuam protegidos. Os planos e prescrições profissionais históricos do paciente mantêm suas autorias e sua visibilidade específica, a ser detalhada com o tratamento de dietas anteriores; não reatribuir os planos ao novo nutricionista.
- A autorização é sempre condicionada a **vínculo ativo e permissão vigente**, verificada pelo servidor também em consultas históricas. Ao terminar o vínculo, o profissional deixa de poder consultar tanto dados novos quanto antigos pelo acompanhamento. Regras jurídicas de dados e eventuais obrigações de retenção ficam para a etapa 8.
- O paciente segue titular do próprio histórico, independentemente da relação profissional. Histórico compartilhado não confere permissão de edição ao nutricionista.
- A ação exata para revogar o vínculo com autonomia do paciente e o tratamento da dieta profissional vigente continuam em aberto nesta etapa.

## 7. Critérios de aceitação inicial

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
| Paciente tenta aceitar convite após 168 horas de sua emissão | Servidor impede ativação e registra estado expirado |
| Nutricionista reenvia convite pendente | Novo convite com novo prazo é criado; link anterior é invalidado |
| Paciente tenta aceitar o link anterior depois do reenvio | Servidor recusa o convite substituído, sem criar vínculo |
| Paciente autenticado aceita e autoriza validamente | Convite exibe **Aceito** no painel profissional somente após ativação do vínculo |
| Paciente recusa o convite | Painel do nutricionista mostra **Recusado** sem exigir justificativa nem liberar dados privados |
| Paciente não responde e prazo termina | Painel mostra **Expirado**, sem presumir recusa |
| Aceite e recusa são enviados concorrentemente | Um único resultado terminal prevalece, sem vínculo inconsistente ou dupla ativação |
| Paciente aceita convite com sucesso e vínculo é ativado | Nutricionista convidador recebe e-mail informando aceite e encontra status Aceito no painel |
| Paciente recusa convite | Nutricionista convidador recebe e-mail informando recusa, sem justificativa ou dados de saúde |
| Convite expira sem resposta | Não disparar e-mail de aceite/recusa; status Expirado permanece no painel |
| Falha na entrega de notificação por e-mail | Decisão registrada permanece válida e consultável no painel; falha pode ser tratada por processo de reenvio |
| Requisição de aceite ou recusa é repetida | Não duplicar eventos de decisão ou criar notificações contraditórias; regras de idempotência na etapa 9 |
| Paciente recebe convite na plataforma | Uma tela informa nome do profissional, categorias de dados e ações claras Aceitar e autorizar / Recusar, com detalhes opcionais |
| Paciente autoriza acompanhamento | Nutricionista vinculado pode consultar diário, metas, indicadores, peso/IMC e planos profissionais autorizados, respeitando período e permissões a definir |
| Nutricionista tenta consultar dados de usuário sem vínculo autorizado | Servidor nega mesmo que o cliente forneça o ID do paciente |
| Nutricionista tenta editar refeição consumida pelo paciente só por possuir acesso de acompanhamento | Operação negada, pois leitura não concede edição automática |
| Profissional tenta acessar dado fora das cinco categorias autorizadas | Acesso negado mesmo com vínculo ativo |
| Paciente não visualiza detalhes opcionais e escolhe Recusar | Nenhum compartilhamento é criado; não obrigar leitura de telas adicionais para recusar |
| Paciente possui registros alimentares anteriores ao convite e aceita | Nutricionista autorizado visualiza esses registros e seus indicadores históricos, sem nova solicitação de acesso |
| Paciente possui histórico de pesagens anterior ao vínculo | Histórico de peso/IMC é consultável pelo nutricionista autorizado no acompanhamento |
| Novo nutricionista tenta abrir biblioteca privada de nutricionista anterior | Acesso negado, apesar da autorização para histórico do paciente |
| Vínculo profissional é encerrado | Nutricionista anterior não pode consultar nem os registros históricos nem os novos no painel, sem excluir o histórico pessoal |

## 8. Decisões ainda em aberto na etapa 6

- **Entrega e retorno dos convites:** convite por e-mail, identificação do destinatário, validade de **7 dias**, reenvio invalidando o anterior e retorno no painel e por e-mail já confirmados. **Escopo de cinco categorias e aceite em tela única com resumo claro também confirmados**, sem múltiplos controles individuais no MVP. Pendem tratamento técnico de falha/repetição de envios e limites contra abuso. Não expor se determinada pessoa possui conta a usuários não autorizados.
- **Troca:** quem pode iniciar, quando o vínculo anterior termina e como evitar janela com dois profissionais simultâneos.
- **Planos profissionais antigos:** se uma dieta profissional vigente do nutricionista anterior deve encerrar automaticamente ou continuar apenas para consulta após a troca. Não alterar essa regra por suposição.
- **Revogação e encerramento:** o histórico pessoal **anterior ao início do vínculo será acessível ao profissional autorizado**, já confirmado com aviso claro no aceite. Falta definir o procedimento simples pelo qual o paciente encerra o vínculo e os efeitos sobre a dieta profissional vigente; acesso a dados privados de profissionais anteriores não decorre dessa autorização.
- **Registro do nutricionista:** verificação do CRN e restrições de acesso antes da validação.
- **Dados profissionais após encerramento:** guarda obrigatória, eliminação, portabilidade e outros requisitos da LGPD na etapa 8.

**Observação:** esta especificação registra apenas decisões funcionais; não há implementação do sistema nesta fase.
