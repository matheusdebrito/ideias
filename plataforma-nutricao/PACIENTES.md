# Gestão de pacientes, vínculos e permissões — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 6 de 10 — Gestão de pacientes e permissões (concluída funcionalmente)
> **Versão:** 1.5
> **Data:** 31/10/2026
> **Referências:** [IDEA.md](./IDEA.md), [DIETAS.md](./DIETAS.md), [DIARIO.md](./DIARIO.md)

## 1. Objetivo

Definir vínculos e autorização entre **usuários adultos e nutricionistas**, preservando a titularidade dos dados pessoais e o histórico do acompanhamento durante mudanças de profissional.

**Vocabulário obrigatório do MVP:** **paciente não é um tipo de conta nem um cadastro separado**. Todo adulto tem uma **conta de usuário** e utiliza a experiência pessoal, tenha ou não nutricionista. **“Paciente” é o termo usado apenas na visão de gestão do nutricionista** (como “Meus pacientes”) para referir-se ao usuário com quem existe ou existiu vínculo, respeitando o acesso permitido. Para o próprio titular, usar “usuário”, “minha conta” e “Meu nutricionista”. Aceitar, encerrar ou trocar vínculos altera as autorizações, **não o tipo de conta, os dados pessoais nem o direito ao acesso gratuito**.

## 2. Decisão aprovada para o MVP — vínculo profissional único

**Modelo relacional:** uma mesma conta de usuário pode estar **sem vínculo** ou **com vínculo ativo autorizado** de acompanhamento, sem uma migração de conta ou perfil para “paciente”. No painel profissional, “Meus pacientes” mostra apenas os usuários com relação e permissões aplicáveis; vínculos anteriores podem constar de histórico restrito sem manter acesso ao diário. 

1. **No máximo um nutricionista ativo por paciente**, a qualquer instante. Um paciente pode ficar **sem nutricionista** e continuar utilizando os recursos pessoais gratuitos e o próprio diário.
2. **Os dados de acompanhamento permanecem com o paciente:** nutricionista trabalha nas dietas, metas e acompanhamento do usuário, mas o fim do vínculo não apaga seu diário, refeições, pesagens, indicadores, metas efetivas ou versões de dietas publicadas para ele. Autoria profissional e vigência de prescrições continuam identificadas.
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
5. **Sem substituição implícita:** um convite recebido ou aberto não substitui um vínculo ativo nem libera dados. A troca direta só ocorre após o paciente autenticado **aceitar, autorizar e confirmar expressamente a troca** em uma única etapa, conforme seção 9.
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
17. **Proteção contra abuso aprovada:** limitar automaticamente envios e reenvios excessivos por profissional e destinatário, além de padrões suspeitos, sem criar etapas extras na utilização normal. Estados de convites, trocas, continuidade de metas e notificação de desligamento já estão definidos. **Ficam para a etapa 9:** limites numéricos, mecanismos técnicos, provedor de e-mail e retentativas; não ampliar notificações para eventos não aprovados.

## 4. Convite por e-mail para pacientes com ou sem conta (**confirmado no MVP**)

- O nutricionista informa o e-mail do paciente para iniciar o convite. Não há necessidade de o paciente já estar cadastrado.
- A mensagem conduz à plataforma para login ou cadastro, sem transmitir dados nutricionais ou de saúde no e-mail.
- **Conta existente:** usuário entra com a conta que possui o endereço de e-mail destinatário verificado, consulta o convite e decide se aceita/autoriza.
- **Conta inexistente:** usuário conclui cadastro 18+, valida o e-mail convidado e só então consulta e decide sobre o convite.
- O link do convite é um acesso ao fluxo de decisão, **não uma credencial suficiente para ler dados do paciente nem um aceite automático**.
- A API verifica destinatário e autorização no momento de ativar vínculo; status da conta do destinatário não deve ser exposto indevidamente ao convidador.
- O prazo de **7 dias** e o reenvio com invalidação do link antigo já estão definidos; limites automáticos contra abuso também foram aprovados. Números, janelas e mecanismos de envio/retentativas ficam para a etapa 9.

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
- **Encerramento pelo paciente confirmado no MVP:** disponível na área **Meu nutricionista**, sem aprovação do nutricionista ou contato com suporte; requer confirmação simples. **Ao encerrar o vínculo, termina também a vigência da prescrição profissional atual e são cancelados agendamentos futuros do profissional anterior**, sem perder os planos publicados no histórico.

## 7. Encerramento de acompanhamento pelo paciente (**confirmado no MVP**)

- O paciente adulto autenticado pode entrar na seção **Meu nutricionista** e escolher **Encerrar acompanhamento** a qualquer momento, sem precisar justificar, pedir anuência ao profissional ou procurar suporte.
- Uma única confirmação clara deve explicar que **o nutricionista perderá o acesso de acompanhamento aos dados do paciente**, que **diário, medições, metas pessoais e histórico do titular serão preservados** e que será possível vincular outro profissional futuramente. A ação é cancelável até a confirmação e não exige múltiplas telas ou formulários.
- **Efeito imediato após a confirmação no servidor:** encerrar o vínculo e a autorização para consulta de dados **atuais e históricos**, inclusive por sessões previamente abertas ou chamadas diretas à API. O servidor/banco valida vínculo e permissão em **cada operação**, independentemente de haver páginas abertas ou dados em cache na interface; não basta remover o paciente da lista visível.
- O paciente mantém sua conta, seu histórico e seus recursos pessoais. A revogação **não exclui** registros alimentares, pesagens nem versões históricas de dietas. Eventuais responsabilidades legais de guarda de documentos profissionais serão analisadas na etapa 8, sem manter acesso ao painel de acompanhamento do ex-profissional por isso.
- Após o encerramento, o paciente pode aceitar novo convite seguindo as regras existentes; o histórico pessoal não é automaticamente transferido para um profissional sem autorização própria. O sistema deve preservar quem era o nutricionista anterior e quando o vínculo se encerrou, com retenção proporcional.
- **Planos profissionais após o encerramento (confirmado no MVP):** a dieta profissional que estava vigente deixa de ser apresentada como prescrição vigente e fica para consulta no histórico, com autor e período de vigência original preservados. Qualquer versão profissional publicada com ativação futura **não será ativada**, pois seu agendamento será cancelado por encerramento de vínculo; conteúdo e histórico do agendamento permanecem consultáveis. O eventual e-mail de aviso de encerramento ao nutricionista ainda não foi definido. **A última meta efetiva de kcal e de cada macronutriente definido permanece como referência pessoal dos indicadores**, preservando autoria e histórico, mesmo que tenha sido originalmente prescrita por nutricionista. Não continua rotulada como prescrição vigente do ex-profissional.
- **Técnica para etapa 9:** encerramento idempotente e atômico, prevenção de acesso residual no backend, coerência com convites pendentes e testes de autorização concorrente. O fluxo do MVP deve permanecer simples para o paciente.

## 8. Dietas profissionais após o término do vínculo (**confirmado no MVP**)

- No momento do encerramento do vínculo profissional, **a prescrição vigente deixa de ser vigente**. A versão publicada, suas refeições e quantidades permanecem imutáveis e disponíveis **somente para consulta no histórico do paciente**, identificadas pelo nutricionista autor e pelas datas reais de vigência.
- **Versões profissionais futuras programadas** pelo nutricionista anterior têm sua ativação cancelada automaticamente em decorrência da revogação do vínculo; registrar o motivo e o momento do cancelamento no histórico. **Não apagar o conteúdo publicado nem agendamentos anteriores**, nem permitir que processos em segundo plano ativem uma versão após o encerramento.
- Trata-se de **uma consequência do encerramento pelo próprio paciente**, e não da regra de cancelamento manual de agendamento por profissional. Por isso, é uma exceção explícita à regra da etapa 3 que proíbe o cancelamento automático de uma versão futura quando um nutricionista publica outra.
- Não converter automaticamente a prescrição profissional anterior em plano pessoal do paciente e **não atribuí-la a um novo nutricionista**. O paciente segue podendo consultar o histórico; planos pessoais existentes e registros de consumo independentes não são modificados.
- A confirmação de **Encerrar acompanhamento** deve explicar de forma breve que o plano profissional atual deixará de ser vigente e que a versão ficará disponível no histórico. Evitar múltiplas confirmações.
- Na etapa 9, especificar transição atômica e idempotente entre vínculo, autorização, fim de vigência e cancelamento da versão futura; resolver concorrência com processadores de ativação agendada. Nunca exibir uma versão como ativa após o fim do vínculo.
- **Continuidade das metas já aprovada:** a última meta efetiva por nutriente continua como referência pessoal dos indicadores após o encerramento, preservando origem e histórico, sem ser prescrição profissional vigente. Ver a seção 11 e [DIARIO.md](./DIARIO.md).

## 9. Troca direta de nutricionista por convite (**confirmado no MVP**)

1. **Fluxo sem etapa extra:** paciente com nutricionista ativo pode receber convite válido de outro nutricionista e escolher **Aceitar e trocar de nutricionista**, sem precisar encerrar seu vínculo manualmente antes. Até sua confirmação, o convite é pendente e não autoriza o novo nutricionista.
2. **Uma única confirmação clara:** mostrar quem é o profissional atual, quem será o novo, as categorias de dados e histórico pessoal compartilhados e as consequências: revogação do acesso anterior, término da dieta profissional vigente anterior, cancelamento de futuras versões programadas e preservação dos dados/históricos do paciente. Permitir voltar ou recusar. Não criar um fluxo de várias telas ou confirmações.
3. **Consentimento e validação:** paciente autenticado e adulto, e-mail destinatário verificado, convite não expirado e autorização independente para o novo profissional. Nem o recebimento do e-mail nem o clique em link fazem a troca.
4. **Atomicidade obrigatória:** servidor e banco realizam em uma única transação lógica: encerrar vínculo antigo e suas autorizações, encerrar a vigência da dieta profissional antiga, cancelar agendamentos futuros da prescrição antiga, registrar o aceite do convite e ativar o novo vínculo com autorização própria. **Jamais podem coexistir dois nutricionistas ativos.** Se houver erro, reverter tudo e manter vínculo, permissões e dieta anteriores; o convite não aparece como Aceito.
5. **Histórico e limites:** o paciente mantém diário, medições, metas pessoais, histórico de dietas e conta. Os planos antigos continuam consultáveis com autoria original; não os converter em plano pessoal ou atribuir ao novo profissional. A nova autorização não inclui bibliotecas privadas do nutricionista antigo nem publica automaticamente uma nova dieta.
6. **Concorrência e estado desatualizado:** impedir confirmação simultânea de convites diferentes ou aceite de convite vencido. Se o vínculo ativo mudar desde a abertura da tela, pedir para revisar a situação atual antes de confirmar; não substituir silenciosamente um terceiro profissional.
7. **Notificações:** somente confirmar **Aceito** no painel e e-mail ao nutricionista convidador depois de ativação real. **Quando a troca for concluída, o profissional anterior receberá aviso por e-mail com o nome do paciente que saiu e o estado encerrado**, sem motivo ou dados nutricionais. A meta efetiva pessoal continua conforme a seção 11. Detalhes de transações, restrições e envio ficam na etapa 9.

## 10. Critérios de aceitação inicial

| Cenário | Comportamento esperado |
| --- | --- |
| Usuário nunca teve nutricionista | Tem conta e recursos pessoais gratuitos, sem precisar ser cadastrado como “paciente” |
| Usuário aceita, encerra ou troca vínculo com nutricionista | Mantém a mesma identidade e conta; só vínculos, autorizações e prescrições vigentes mudam |
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
| Paciente já tem nutricionista ativo e recebe segundo convite | O recebimento não altera vínculo nem dados; troca exige confirmação explícita em uma etapa |
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
| Paciente abre Meu nutricionista e escolhe Encerrar acompanhamento, mas cancela | Vínculo e autorizações permanecem inalterados |
| Paciente confirma Encerrar acompanhamento | Servidor encerra imediatamente vínculo e autorização, preservando diário, medidas, metas e histórico pessoal |
| Nutricionista mantém aba antiga aberta após o encerramento | Qualquer nova tentativa de carregar/editar dados pela API é negada pelo servidor |
| Paciente acessa o próprio diário após encerrar o acompanhamento | Conta e histórico continuam disponíveis, sem exigir assinatura profissional |
| Paciente confirma encerramento duas vezes | Operação não causa novos efeitos ou exclusão de registros; fluxo idempotente |
| Ex-nutricionista tenta consultar registros criados antes do vínculo | Acesso de acompanhamento negado, tal como para registros recentes |
| Paciente encerra vínculo com dieta profissional vigente | Versão deixa de ser vigente e aparece para consulta no histórico, com autoria, conteúdo e período preservados |
| Paciente encerra vínculo com próxima dieta profissional programada | Agendamento futuro é cancelado com motivo de encerramento; versão publicada permanece no histórico |
| Paciente encerra vínculo tendo plano pessoal próprio | Plano pessoal e diário alimentar continuam inalterados |
| Agendador tenta ativar dieta do profissional depois do encerramento | Ativação negada, sem restabelecer vínculo ou prescrição |
| Agendamento e encerramento ocorrem ao mesmo tempo | Registros convergem para ausência de prescrição profissional vigente, mantendo trilha cronológica |
| Paciente consulta dieta do nutricionista anterior após término | Pode ler a versão histórica, sem reativá-la nem editá-la como prescrição profissional |
| Paciente recebe convite de outro profissional e não confirma troca | Vínculo, acesso e dieta antigos permanecem, sem conceder acesso ao convidador |
| Paciente confirma Aceitar e trocar de nutricionista | Encerra vínculo antigo e dieta vigente, cancela versões programadas, ativa novo vínculo autorizado, preservando histórico em transição única |
| Troca direta falha durante a operação | Reversão completa: vínculo, acesso e dieta antigos permanecem, convite não é Aceito |
| Dois convites diferentes são aceitos concorrentemente | No máximo um vínculo ativo; não há compartilhamento simultâneo |
| Profissional anterior mantém sessão aberta depois da troca | API nega consultas posteriores aos dados do paciente |
| Vínculo ativo mudou enquanto convite estava aberto | Solicitar revisão da situação atual antes de confirmar, sem substituição silenciosa |
| Paciente encerra vínculo com meta profissional de 2.200 kcal e sem ajuste posterior | Mantém 2.200 kcal como referência pessoal e autoria profissional histórica |
| Paciente havia ajustado uma meta profissional de 2.200 para 2.000 kcal | Continua usando 2.000 kcal após encerramento, com prescrição original preservada |
| Paciente sem meta para proteínas encerra acompanhamento | Não cria nem preenche meta de proteínas |
| Paciente troca de nutricionista e novo profissional não definiu metas | Última meta efetiva persiste até ajuste pessoal ou nova meta profissional válida |
| Ex-profissional tem mudança de meta agendada para após desligamento | Alteração futura não entra em vigor sem vínculo ativo |
| Paciente edita meta pessoal preservada após desvinculação | Edição livre, sem aviso de prescrição vigente do ex-profissional, preservando histórico |
| Paciente encerra vínculo diretamente | Enviar e-mail ao ex-nutricionista com nome do paciente e confirmação de encerramento, sem motivo ou dados de saúde |
| Paciente confirma troca por convite de outro nutricionista | Após transação concluída, ex-nutricionista recebe e-mail identificando apenas pelo nome o paciente desvinculado; novo profissional recebe notificação de aceite |
| Troca é recusada, cancelada ou falha | Não enviar e-mail de vínculo encerrado ao profissional anterior |
| Envio do e-mail de encerramento falha | Encerramento permanece válido e aparece no histórico profissional restrito; acesso a dados do paciente segue revogado |
| Nutricionista anterior abre link do e-mail após desvinculação | Visualiza apenas o evento de encerramento, nunca o painel ou dados privados do paciente |
| Usuário cadastra nome e CRN ainda não conferido | Conta criada com estado Em validação, sem enviar convites, consultar pacientes ou publicar metas/dietas profissionais |
| Número do CRN tem formato válido mas não foi verificado | Permissões profissionais permanecem bloqueadas; não confiar no valor digitado |
| Profissional com registro verificado e aprovado envia convite | Convite permitido, sujeito às regras de autorização, prazo e controles contra abuso |
| Cadastro CRN tem número/regional divergente ou pertence a outra pessoa | Solicitação não aprovada e opção clara de corrigir os dados; não criar permissões de nutricionista |
| Conta Em validação tenta chamar API de prescrição diretamente | Servidor bloqueia ação sem depender da interface |
| Usuário com CRN em validação deseja usar diário e plano pessoal | Recursos pessoais continuam utilizáveis, sem capacidades profissionais |
| Nutricionista tenta reenviar convites várias vezes ao mesmo e-mail em curto período | Servidor aplica limite por destinatário e por emissor, evitando spam; não criar convites extras |
| Profissional válido envia convites moderados a pacientes diferentes | Uso legítimo permitido sem desafios adicionais ou bloqueio indiscriminado |
| Convite ou reenvio é bloqueado por frequência excessiva | Apresentar mensagem simples de limite temporário, sem informar se destinatário tem conta e sem afirmar envio |
| Convite ativo existe e tentativa de reenvio é bloqueada | Convite/link e prazo anterior permanecem inalterados |
| Profissional usa requisições diretas ou múltiplos cliques para contornar limite | API rejeita o excesso mesmo fora da interface, sem duplicar efeitos |

## 11. Notificação de encerramento ao nutricionista anterior (**confirmada no MVP**)

- **Eventos:** enviar e-mail ao nutricionista anterior quando o paciente confirmar **Encerrar acompanhamento** ou concluir **Aceitar e trocar de nutricionista**. Mostrar também o encerramento na área profissional de histórico de vínculos, sem manter acesso ao painel de acompanhamento daquele paciente.
- **Identificação mínima útil:** a mensagem conterá **o nome do paciente que teve seu vínculo encerrado**, já conhecido do profissional por sua relação anterior, e o resultado **acompanhamento encerrado**; poderá indicar a data/hora e o acesso à área de vínculos encerrados na plataforma. Não exigir justificativa do paciente nem informar o motivo; **não incluir valores nutricionais, refeições, peso, metas, dietas, prontuário, nome do novo profissional ou seus dados**.
- **Destinatário restrito:** enviar **somente ao e-mail verificado associado à conta do nutricionista que possuía o vínculo imediatamente anterior**, e apenas quando essa relação realmente existia. Não notificar outros nutricionistas, convidados sem vínculo nem endereços fornecidos por terceiros.
- **Consistência e privacidade:** criar o evento de notificação **somente depois da confirmação persistida do encerramento ou troca**. Falha de entrega não reabre vínculo, não devolve acesso e não impede a operação concluída; o estado permanece disponível no histórico profissional restrito. Não abrir acesso aos dados privados do paciente por meio de link de notificação.
- **Fluxo simples:** paciente não precisa escrever motivo nem decidir se o nutricionista será avisado; essa consequência poderá ser apresentada discretamente na confirmação única já aprovada. A etapa 9 tratará entrega assíncrona, idempotência de eventos, duplicidade e falhas.

## 12. Continuidade das metas e propriedade dos dados (**confirmado no MVP**)

- **Regra geral:** o paciente não perde seus registros de acompanhamento ao encerrar ou trocar de nutricionista: diário, pesagens, metas, indicadores e conteúdo das versões de dietas profissionais publicadas para ele permanecem consultáveis em sua conta, sujeitos a direitos de exclusão/retificação e obrigações de guarda (etapa 8). Isso não inclui automaticamente bibliotecas internas e prontuários privados do profissional.
- **Metas sem interrupção:** conservar a última meta efetiva para cada nutriente que já possua valor (calorias, proteínas, carboidratos, gorduras). Tanto a meta definida pelo profissional quanto um ajuste posterior do paciente podem ser a última referência. Não zerar, apagar, inventar nova meta ou alterar indicadores de períodos anteriores.
- **Exemplo:** nutricionista prescreveu 2.200 kcal, mas paciente ajustou sua meta efetiva para 2.000 kcal: após a desvinculação **2.000 kcal continuam nos indicadores**, e a prescrição original de 2.200 kcal fica no histórico. Sem ajuste do paciente, mantém 2.200 kcal como referência pessoal, não como prescrição profissional atual.
- **Livre edição posterior:** a meta mantida passa a servir como referência pessoal; usuário pode ajustá-la sem o aviso específico aplicável a uma prescrição de profissional ainda vinculado. A origem e autoria da referência original continuam visíveis no histórico.
- **Troca direta:** a referência atual persiste até ser alterada pelo paciente ou substituída, quando entrar em vigor, por nova meta profissional de nutricionista atualmente vinculado e autorizado. O novo profissional não herda autoria antiga.
- **Sem comandos futuros do ex-profissional:** atualização de meta agendada pelo nutricionista anterior não pode modificar a referência após revogação do vínculo. A etapa 9 detalhará garantias de transação e concorrência.
- **Dieta preservada, vigência encerrada:** o paciente conserva o conteúdo publicado e pode consultá-lo, mas conforme aprovação anterior a prescrição profissional deixa de ser vigente e suas futuras ativações são canceladas. Preservar dados não significa manter uma prescrição como ativa.

## 13. Cadastro profissional e validação do CRN (**confirmados no MVP**)

- **Dados simples no cadastro profissional:** solicitar **nome completo**, **número do CRN** e **região/Conselho Regional correspondente ao registro**. A criação da conta pode ser concluída mesmo sem validação profissional imediata; não exigir um processo longo nem documentos extras por padrão sem necessidade de verificação.
- **Conta criada não equivale a profissional habilitado:** até a confirmação da regularidade e titularidade do registro, a conta permanece **Em validação**, com uma explicação simples do estado. Informar um CRN com formato aparentemente válido não comprova que pertence ao usuário, está ativo ou autoriza exercício profissional.
- **Liberação apenas após aprovação:** só quando o registro tiver sido **verificado e aprovado** o profissional poderá **enviar convites de acompanhamento**, **ativar/aceitar novos vínculos profissionais**, **acessar dados privados de pacientes na função de nutricionista**, **publicar, editar ou programar prescrições profissionais** e **definir metas profissionais**. Um usuário não validado não pode burlar a restrição acessando APIs diretamente, alterando papel no cliente ou usando convite previamente criado.
- **Uso pessoal preservado:** um profissional com validação pendente poderá usar as funcionalidades pessoais comuns da própria conta, sem ganhar permissões profissionais por isso. O sistema pode permitir preencher/atualizar os dados cadastrais do registro e visualizar o status da solicitação.
- **Verificação confiável:** a checagem deve corresponder à **pessoa que fez o cadastro**, ao **registro e respectivo regional** e à **situação profissional aplicável**, por fonte confiável ou procedimento de conferência definido na etapa 9. **Não assumir que há API pública oficial**, que apenas consultar um número é suficiente ou que validação automática está disponível. Se não houver meio automatizado confiável, usar revisão controlada antes de liberar capacidades profissionais.
- **Estados e rechecagem:** manter estados distinguíveis de **Em validação**, **Validado** e **Não aprovado / requer correção**, com orientação clara para correção sem expor dados desnecessários. Como tratar posterior irregularidade, bloqueio ou revalidação do registro será detalhado nas etapas 8/9, com controle de privilégios baseado em situação efetivamente verificada.
- **Segurança e histórico:** a validação profissional é um requisito **adicional**, nunca substitui autenticação da conta, aceite do paciente, vínculo ativo e autorização específica de dados. Preservar trilha mínima da decisão de validação e sua autoria para auditoria, com retenção e acesso definidos na etapa 8.

## 14. Proteção automática contra abuso de convites (**confirmada no MVP**)

- **Envio e reenvio com limites automáticos:** proteger convites por e-mail contra solicitações repetidas, spam e disparos em massa, incluindo **múltiplos reenvios ao mesmo destinatário**, sem prejudicar o profissional que convida diversos pacientes legítimos.
- **Regras de uso sem fricção:** em uso normal não exigir CAPTCHA, etapas extras de confirmação, justificativa de convite nem bloqueios excessivos. Aplicar controles no servidor considerando, entre outros, **identidade do nutricionista validado**, **endereço destinatário**, **frequência de reenvios**, padrões de volume e tentativas de contornar limites.
- **Separação por escopo:** um limite temporário para reenvios a um destinatário não deve impedir automaticamente convites legítimos a outras pessoas. Proteger também contra novos convites usados para contornar uma restrição do mesmo par nutricionista/destinatário. Limites gerais por emissor e proteções em nível de infraestrutura podem complementar a regra específica.
- **Feedback simples e sem vazamento de dados:** se o limite for alcançado, informar discretamente que o convite não pôde ser enviado agora e poderá ser tentado mais tarde, com orientação de aguardar. **Não afirmar envio bem-sucedido quando o servidor não autorizou nem enfileirou o envio**. Não revelar se o e-mail pertence a uma conta, se o destinatário está cadastrado ou dados privados do paciente.
- **Estados preservados:** tentativa bloqueada não cria convite válido, não muda status de convite anterior nem cancela vínculo existente. Em reenvio autorizado, manter regra já aprovada de novo convite/link e invalidação atômica do link anterior. Falhas de entrega posteriores não se confundem com bloqueio de limite.
- **Garantias técnicas adiadas à etapa 9:** definir **valores numéricos, janelas de tempo, cooldown por destinatário, limites por emissor, medidas contra contorno por contas/IP, idempotência e observabilidade**, considerando limites do provedor de e-mail. Não fixar valores arbitrários como requisito de produto nesta etapa. Adicionar testes contra múltiplos cliques, concorrência e explosão de reenvios; não depender do botão desabilitado no frontend.

## 15. Pendências técnicas e jurídicas encaminhadas a outras etapas

- **Convites definidos funcionalmente:** convite por e-mail, validade de **7 dias**, reenvio invalidando o anterior, retorno ao profissional, escopo de cinco categorias, aceite simples e **limites automáticos de envio e reenvio sem etapas extras**. **Etapa 9:** parâmetros numéricos, segurança dos endpoints, idempotência, provedor de e-mail, entrega e retentativas; sem revelar a existência de conta do destinatário.
- **Troca direta aprovada:** paciente pode aceitar convite de outro profissional com uma única confirmação, encerrando o vínculo antigo e ativando o novo atomicamente, sem dois profissionais simultâneos.
- **Planos profissionais antigos — decidido:** ao encerrar vínculo, a dieta profissional vigente vai para consulta no histórico e eventuais versões futuras programadas são canceladas, preservando conteúdo e autoria. Troca direta por convite já definida; **última meta efetiva preservada como referência pessoal após desvinculação**, com origem profissional registrada.
- **Revogação e encerramento confirmados:** o histórico pessoal anterior ao vínculo será acessível ao profissional autorizado; o paciente poderá encerrar o acompanhamento em **Meu nutricionista**, com confirmação simples, revogação imediata do acesso, encerramento da dieta profissional vigente e cancelamento das versões futuras. **Aviso com nome do paciente ao ex-profissional por e-mail e no histórico de vínculos já aprovado**; não inclui motivo nem dados nutricionais, e não restabelece acesso. A autorização não inclui bibliotecas privadas anteriores.
- **Registro do nutricionista — decidido:** solicitar nome completo, número e regional do CRN; permitir cadastro em **Em validação**; liberar convites, vínculo e prescrições **somente após aprovação do registro e da titularidade**. **Em aberto para etapas 8/9:** fonte/método de consulta, evidências proporcionais, rechecagens, suspensão e integração técnica.
- **Dados profissionais após encerramento:** guarda obrigatória, eliminação, portabilidade e outros requisitos da LGPD na etapa 8.

**Encerramento funcional da etapa 6:** convites, autorização, compartilhamento, encerramento/troca de nutricionista, preservação dos dados/metas/dietas, aviso ao ex-profissional, verificação do CRN e proteção contra abuso estão definidos para o MVP. Requisitos técnicos e jurídicos identificados serão detalhados nas etapas 8/9. Esta especificação não representa software implementado ou testado.
