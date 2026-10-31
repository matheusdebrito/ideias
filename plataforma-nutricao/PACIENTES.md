# Gestão de usuários acompanhados, vínculos e permissões — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 6 de 10 — Gestão de usuários acompanhados e permissões (concluída funcionalmente)
> **Versão:** 1.6
> **Data:** 31/10/2026
> **Referências:** [IDEA.md](./IDEA.md), [DIETAS.md](./DIETAS.md), [DIARIO.md](./DIARIO.md)

## 1. Objetivo

**Regra atualizada na etapa 7:** esta especificação usa nomes antigos de “profissional” e “paciente” em alguns fluxos, mas **as regras valem para qualquer gestor adulto assinante e qualquer usuário acompanhado**. Uma conta é única; CRN não é requisito para gestão. O rótulo padrão é **Meus acompanhados**, e **Meus pacientes** fica restrito à interface de gestores com CRN verificado. Os fluxos documentados não conferem habilitação para serviços clínicos. Esta decisão substitui qualquer exigência antiga de CRN como permissão para gerir terceiros.

Definir vínculos e autorização entre **usuários adultos e gestores**, preservando a titularidade dos dados pessoais e o histórico do acompanhamento durante mudanças de profissional.

**Vocabulário obrigatório do MVP:** **usuário acompanhado não é um tipo de conta nem um cadastro separado**. Todo adulto tem uma **conta de usuário** e utiliza a experiência pessoal, tenha ou não gestor. **“Usuário acompanhado” é o termo usado apenas na visão de gestão do gestor** (como “Meus acompanhados”) para referir-se ao usuário com quem existe ou existiu vínculo, respeitando o acesso permitido. Para o próprio titular, usar “usuário”, “minha conta” e “Meu acompanhamento”. Aceitar, encerrar ou trocar vínculos altera as autorizações, **não o tipo de conta, os dados pessoais nem o direito ao acesso gratuito**.

## 2. Decisão aprovada para o MVP — vínculo profissional único

**Modelo relacional:** uma mesma conta de usuário pode estar **sem vínculo** ou **com vínculo ativo autorizado** de acompanhamento, sem uma migração de conta ou perfil para “usuário acompanhado”. No painel profissional, “Meus acompanhados” mostra apenas os usuários com relação e permissões aplicáveis; vínculos anteriores podem constar de histórico restrito sem manter acesso ao diário. 

1. **No máximo um gestor ativo por usuário acompanhado**, a qualquer instante. Um usuário acompanhado pode ficar **sem gestor** e continuar utilizando os recursos pessoais gratuitos e o próprio diário.
2. **Os dados de acompanhamento permanecem com o usuário acompanhado:** gestor trabalha nas dietas, metas e acompanhamento do usuário, mas o fim do vínculo não apaga seu diário, refeições, pesagens, indicadores, metas efetivas ou versões de dietas publicadas para ele. Autoria profissional e vigência de planos alimentares publicados continuam identificadas.
3. **Histórico preservado:** dietas e planos alimentares compartilhados anteriores permanecem identificadas pelo autor e por sua vigência, sujeitas às regras legais de retenção/eliminação da etapa 8. Não reatribuir uma dieta antiga ao novo gestor e não sobrescrever histórico nutricional.
4. **Sem acesso ativo do ex-profissional:** depois que o vínculo ou a autorização terminar, o ex-gestor não tem mais acesso aos dados atuais ou históricos do usuário acompanhado **pelo painel de acompanhamento**. Eventual guarda obrigatória de documentação profissional é um assunto jurídico separado, não uma autorização contínua de consulta à conta.
5. **Nova autorização independente:** qualquer acesso de um novo gestor a dados privados requer vínculo ativo e autorização válida; não herdar permissões ou compartilhamento do profissional anterior.
6. **Controle efetivo:** servidor e banco devem impedir mais de um vínculo ativo simultâneo, inclusive em tentativas concorrentes. RLS/autorização e integridade da transição serão projetadas na etapa 9. O cliente não é fonte de verdade para permissões.
7. **Escopo adulto:** somente usuários com 18 anos completos ou mais podem estabelecer vínculos no MVP.

## 3. Convite enviado pelo profissional e aceite do usuário acompanhado (**confirmado no MVP**)

1. **Convite iniciado pelo gestor:** o profissional convida o usuário acompanhado para acompanhamento; o convite pendente não cria vínculo ativo nem permite acesso ao diário, peso, metas, dietas ou dados privados.
2. **Aceite e autorização expressos:** o usuário acompanhado destinatário deve estar autenticado, saber quem convida, o objetivo do acompanhamento e quais categorias de dados serão compartilhadas, e **aceitar e autorizar explicitamente** para ativar o vínculo. Receber o convite, criar conta ou apenas visualizá-lo não constitui autorização.
3. **Recusa e ausência de resposta:** o usuário acompanhado pode recusar ou não responder, sem ativar vínculo nem conceder acesso ao profissional.
4. **Validação efetiva:** identidade do destinatário, aceite e autorização devem ser verificados no servidor. Convites não podem conceder acesso a terceiros nem ser usados por outra conta. Detalhes técnicos de identidade, tokens e expiração serão tratados na etapa 9.
5. **Sem substituição implícita:** um convite recebido ou aberto não substitui um vínculo ativo nem libera dados. A troca direta só ocorre após o usuário acompanhado autenticado **aceitar, autorizar e confirmar expressamente a troca** em uma única etapa, conforme seção 9.
6. **Rastreabilidade proporcional:** registrar autoria do convite, aceite e autorização, quando ocorreram e início do vínculo, sujeitos às definições de retenção da etapa 8.
7. **Canal confirmado no MVP: e-mail.** O gestor informa o **e-mail do usuário acompanhado**; a plataforma envia uma mensagem com acesso ao fluxo de convite, sem incluir diário, peso, metas, dietas nem outros dados sensíveis no conteúdo do e-mail.
8. **Usuário acompanhado com conta:** ao acessar o convite, precisa fazer login e ter o endereço destinatário vinculado e verificado na conta. Só após visualizar quem convida e os dados solicitados poderá aceitar e autorizar explicitamente.
9. **Usuário acompanhado sem conta:** o convite permite seguir para o cadastro de usuário adulto, com verificação do endereço destinatário, e depois retornar à análise do convite; cadastrar-se **não equivale a aceitar**.
10. **Proteção de identidade:** o servidor valida que a conta autenticada corresponde ao endereço destinatário verificado. Ter posse do link ou visualizar o e-mail, isoladamente, não concede permissão sobre dados pessoais nem acesso profissional. Respostas de criação/envio não devem revelar indevidamente se o destinatário já possui conta.
11. **Validade aprovada no MVP:** um convite por e-mail pode ser aceito ou recusado durante **7 dias corridos (168 horas) a partir de sua emissão**, conforme instante registrado no servidor. Após o prazo, o convite fica **expirado**, não pode ativar vínculo e não concede acesso.
12. **Reenvio aprovado no MVP:** o gestor pode reenviar um convite ainda pendente ou expirado, gerando **um novo convite com novo prazo de 7 dias** e invalidando o link/convite anterior de forma atômica. Links antigos, expirados ou substituídos não podem ser aceitos. Um convite já aceito não pode ser reenviado como se o vínculo estivesse pendente; nova solicitação após recusa é outro convite, sujeito a controles contra abuso.
13. **Retorno ao gestor aprovado:** o profissional que enviou o convite poderá consultar no **painel profissional** seu estado atual: **pendente**, **aceito**, **recusado**, **expirado** ou **substituído por reenvio**, com data/hora da decisão quando houver. Aceite corresponde à ativação efetiva do vínculo, após verificação de identidade, autorização e exclusividade; falhas de ativação não podem aparecer como aceitas.
14. **Privacidade da recusa:** o profissional pode ver que o convite foi recusado, **sem justificativa obrigatória**, sem acesso a dados privados do usuário acompanhado e sem saber se o destinatário possui conta apenas pelo ato de enviar o convite. Não confundir estado enviado com entrega confirmada do e-mail.
15. **Notificação por e-mail ao gestor confirmada para o MVP:** após a decisão **aceito ou recusado** ser registrada, enviar uma mensagem ao e-mail verificado da conta profissional **que originou o convite**, com resultado e acesso à área de convites no painel. A mensagem não deve conter valores nutricionais, peso, metas, histórico de saúde ou justificativa de recusa. O status no painel permanece a fonte de verdade.
16. **Consistência e falhas:** a notificação só pode refletir um estado efetivamente confirmado no servidor: **aceito** exige autorização e vínculo ativado; **recusado** exige recusa real. Falha no envio não reverte a decisão, não impede o usuário acompanhado de concluir o fluxo e não apresenta sucesso de entrega não confirmado. Processamento assíncrono, retries e prevenção de e-mails duplicados serão especificados na etapa 9.
17. **Proteção contra abuso aprovada:** limitar automaticamente envios e reenvios excessivos por profissional e destinatário, além de padrões suspeitos, sem criar etapas extras na utilização normal. Estados de convites, trocas, continuidade de metas e notificação de desligamento já estão definidos. **Ficam para a etapa 9:** limites numéricos, mecanismos técnicos, provedor de e-mail e retentativas; não ampliar notificações para eventos não aprovados.

## 4. Convite por e-mail para usuários acompanhados com ou sem conta (**confirmado no MVP**)

- O gestor informa o e-mail do usuário acompanhado para iniciar o convite. Não há necessidade de o usuário acompanhado já estar cadastrado.
- A mensagem conduz à plataforma para login ou cadastro, sem transmitir dados nutricionais ou de saúde no e-mail.
- **Conta existente:** usuário entra com a conta que possui o endereço de e-mail destinatário verificado, consulta o convite e decide se aceita/autoriza.
- **Conta inexistente:** usuário conclui cadastro 18+, valida o e-mail convidado e só então consulta e decide sobre o convite.
- O link do convite é um acesso ao fluxo de decisão, **não uma credencial suficiente para ler dados do usuário acompanhado nem um aceite automático**.
- A API verifica destinatário e autorização no momento de ativar vínculo; status da conta do destinatário não deve ser exposto indevidamente ao convidador.
- O prazo de **7 dias** e o reenvio com invalidação do link antigo já estão definidos; limites automáticos contra abuso também foram aprovados. Números, janelas e mecanismos de envio/retentativas ficam para a etapa 9.

## 5. Validade, reenvio e acompanhamento do convite (**confirmado no MVP**)

- Cada convite criado tem **7 dias corridos / 168 horas de validade a contar da emissão**. Um link expirado é recusado pelo servidor, ainda que o usuário consiga abrir a página.
- Ao reenviar um convite pendente ou expirado, gerar **um novo identificador/link e uma nova validade de sete dias**, invalidando o anterior. O reenvio não reaproveita o link antigo.
- O gestor vê no próprio painel, para convites enviados por ele, os estados **Pendente**, **Aceito**, **Recusado**, **Expirado** e **Substituído** (este último para convites invalidados por reenvio), e o momento de atualização quando aplicável. Estados devem corresponder ao estado real no servidor.
- O estado **Aceito** só é apresentado se o usuário acompanhado correto tiver aceitado/autorizado e o vínculo estiver efetivamente ativado. A recusa mostra **Recusado**, sem exigir nem mostrar motivo e sem dar acesso a dados privados.
- Convites antigos não podem alterar um estado terminal nem criar vínculo depois de expirar ou ser substituídos. Registrar transições apenas uma vez e proteger contra aceite/recusa concorrentes.
- **Aviso adicional por e-mail aprovado:** após **aceite com vínculo efetivamente ativado** ou **recusa registrada**, enviar ao gestor que convidou uma mensagem transacional com o resultado e referência ao painel profissional. O e-mail não deve expor dados nutricionais ou de saúde, conteúdo do diário nem motivos da recusa.
- **Fonte de verdade:** a resposta no painel é mantida mesmo se o serviço de e-mail falhar. Envios e retentativas não podem criar novo vínculo nem modificar o estado do convite.

## 6. Escopo do acompanhamento e aceite simples (**confirmado no MVP**)

**Dados apresentados para autorização do usuário acompanhado ao aceitar o convite:**
- **Diário alimentar:** refeições registradas e composição nutricional consumida.
- **Metas alimentares:** metas de calorias e macronutrientes, incluindo as referências profissionais e ajustes efetivos relevantes ao acompanhamento.
- **Gráficos e indicadores:** resumos, totais e evolução do consumo registrado.
- **Medidas corporais:** peso registrado, evolução do peso e IMC quando calculável.
- **Planos alimentares profissionais:** dietas profissionais vinculadas ao acompanhamento autorizado, com identificação da autoria e versões pertinentes.

**Experiência de autorização:** uma tela curta com nome do gestor e resumo legível dessas categorias, ações inequívocas **Aceitar e autorizar** e **Recusar convite**, e acesso opcional a **Ver detalhes**. Sem sequência extensa de formulários nem chaves individuais para cada categoria neste MVP. A ausência de chaves separadas não autoriza ampliar a lista de dados compartilhados: mudanças materiais do escopo exigem informação clara e nova autorização quando aplicável. O aceite não pode vir pré-selecionado ou ser inferido da navegação.

**Acesso e proteção:** o profissional autenticado só consulta os dados aprovados de usuário acompanhado com **vínculo ativo e autorização vigente**. A API/banco rejeitam consulta de outras categorias e de outros usuários acompanhados, inclusive tentativas diretas a endpoints; o usuário acompanhado não perde propriedade ou permissão de editar seus próprios registros. A autorização de acesso **não permite ao gestor editar refeições consumidas ou medidas do usuário acompanhado por padrão**; suas ações de plano alimentar publicado seguem permissões próprias já planejadas. O usuário acompanhado deve conseguir identificar o profissional vinculado e encontrar o caminho para encerrar/revogar o vínculo; detalhes desse fluxo serão definidos nesta etapa.

**Histórico anterior ao vínculo (confirmado no MVP):** o mesmo aceite explícito em tela única autoriza também consultar **o diário alimentar e consumo registrado, metas e indicadores relacionados, histórico de peso e IMC já existentes antes da ativação do vínculo**, sem exigir um segundo fluxo. A tela deve informar de forma simples: **"Seu gestor também poderá consultar seus registros anteriores ao início do acompanhamento"**, com detalhes opcionais. O usuário acompanhado não precisa enviar relatórios ou recadastrar registros.

- A consulta inclui dados registrados pelo próprio usuário acompanhado nas categorias autorizadas, independentemente de terem sido criados antes ou depois da ativação do vínculo, inclusive lançamentos retroativos. Não estabelecer recorte arbitrário que prejudique a evolução histórica, sujeito à conservação efetiva de registros e à exclusão/retificação pelo titular.
- **Não compartilhar automaticamente acervos privados de outros profissionais**: prontuários próprios, bibliotecas privadas, documentos ou materiais não pertencentes à conta do usuário acompanhado continuam protegidos. Os planos e planos alimentares compartilhados históricos do usuário acompanhado mantêm suas autorias e sua visibilidade específica, a ser detalhada com o tratamento de dietas anteriores; não reatribuir os planos ao novo gestor.
- A autorização é sempre condicionada a **vínculo ativo e permissão vigente**, verificada pelo servidor também em consultas históricas. Ao terminar o vínculo, o profissional deixa de poder consultar tanto dados novos quanto antigos pelo acompanhamento. Regras jurídicas de dados e eventuais obrigações de retenção ficam para a etapa 8.
- O usuário acompanhado segue titular do próprio histórico, independentemente da relação profissional. Histórico compartilhado não confere permissão de edição ao gestor.
- **Encerramento pelo usuário acompanhado confirmado no MVP:** disponível na área **Meu acompanhamento**, sem aprovação do gestor ou contato com suporte; requer confirmação simples. **Ao encerrar o vínculo, termina também a vigência da plano alimentar compartilhado atual e são cancelados agendamentos futuros do profissional anterior**, sem perder os planos publicados no histórico.

## 7. Encerramento de acompanhamento pelo usuário acompanhado (**confirmado no MVP**)

- O usuário acompanhado adulto autenticado pode entrar na seção **Meu acompanhamento** e escolher **Encerrar acompanhamento** a qualquer momento, sem precisar justificar, pedir anuência ao profissional ou procurar suporte.
- Uma única confirmação clara deve explicar que **o gestor perderá o acesso de acompanhamento aos dados do usuário acompanhado**, que **diário, medições, metas pessoais e histórico do titular serão preservados** e que será possível vincular outro profissional futuramente. A ação é cancelável até a confirmação e não exige múltiplas telas ou formulários.
- **Efeito imediato após a confirmação no servidor:** encerrar o vínculo e a autorização para consulta de dados **atuais e históricos**, inclusive por sessões previamente abertas ou chamadas diretas à API. O servidor/banco valida vínculo e permissão em **cada operação**, independentemente de haver páginas abertas ou dados em cache na interface; não basta remover o usuário acompanhado da lista visível.
- O usuário acompanhado mantém sua conta, seu histórico e seus recursos pessoais. A revogação **não exclui** registros alimentares, pesagens nem versões históricas de dietas. Eventuais responsabilidades legais de guarda de documentos profissionais serão analisadas na etapa 8, sem manter acesso ao painel de acompanhamento do ex-profissional por isso.
- Após o encerramento, o usuário acompanhado pode aceitar novo convite seguindo as regras existentes; o histórico pessoal não é automaticamente transferido para um profissional sem autorização própria. O sistema deve preservar quem era o gestor anterior e quando o vínculo se encerrou, com retenção proporcional.
- **Planos profissionais após o encerramento (confirmado no MVP):** a dieta profissional que estava vigente deixa de ser apresentada como plano alimentar publicado vigente e fica para consulta no histórico, com autor e período de vigência original preservados. Qualquer versão profissional publicada com ativação futura **não será ativada**, pois seu agendamento será cancelado por encerramento de vínculo; conteúdo e histórico do agendamento permanecem consultáveis. O eventual e-mail de aviso de encerramento ao gestor ainda não foi definido. **A última meta efetiva de kcal e de cada macronutriente definido permanece como referência pessoal dos indicadores**, preservando autoria e histórico, mesmo que tenha sido originalmente planejada por gestor. Não continua rotulada como plano alimentar publicado vigente do ex-profissional.
- **Técnica para etapa 9:** encerramento idempotente e atômico, prevenção de acesso residual no backend, coerência com convites pendentes e testes de autorização concorrente. O fluxo do MVP deve permanecer simples para o usuário acompanhado.

## 8. Dietas profissionais após o término do vínculo (**confirmado no MVP**)

- No momento do encerramento do vínculo profissional, **a plano alimentar publicado vigente deixa de ser vigente**. A versão publicada, suas refeições e quantidades permanecem imutáveis e disponíveis **somente para consulta no histórico do usuário acompanhado**, identificadas pelo gestor autor e pelas datas reais de vigência.
- **Versões profissionais futuras programadas** pelo gestor anterior têm sua ativação cancelada automaticamente em decorrência da revogação do vínculo; registrar o motivo e o momento do cancelamento no histórico. **Não apagar o conteúdo publicado nem agendamentos anteriores**, nem permitir que processos em segundo plano ativem uma versão após o encerramento.
- Trata-se de **uma consequência do encerramento pelo próprio usuário acompanhado**, e não da regra de cancelamento manual de agendamento por profissional. Por isso, é uma exceção explícita à regra da etapa 3 que proíbe o cancelamento automático de uma versão futura quando um gestor publica outra.
- Não converter automaticamente a plano alimentar compartilhado anterior em plano pessoal do usuário acompanhado e **não atribuí-la a um novo gestor**. O usuário acompanhado segue podendo consultar o histórico; planos pessoais existentes e registros de consumo independentes não são modificados.
- A confirmação de **Encerrar acompanhamento** deve explicar de forma breve que o plano profissional atual deixará de ser vigente e que a versão ficará disponível no histórico. Evitar múltiplas confirmações.
- Na etapa 9, especificar transição atômica e idempotente entre vínculo, autorização, fim de vigência e cancelamento da versão futura; resolver concorrência com processadores de ativação agendada. Nunca exibir uma versão como ativa após o fim do vínculo.
- **Continuidade das metas já aprovada:** a última meta efetiva por nutriente continua como referência pessoal dos indicadores após o encerramento, preservando origem e histórico, sem ser plano alimentar compartilhado vigente. Ver a seção 11 e [DIARIO.md](./DIARIO.md).

## 9. Troca direta de gestor por convite (**confirmado no MVP**)

1. **Fluxo sem etapa extra:** usuário acompanhado com gestor ativo pode receber convite válido de outro gestor e escolher **Aceitar e trocar de gestor**, sem precisar encerrar seu vínculo manualmente antes. Até sua confirmação, o convite é pendente e não autoriza o novo gestor.
2. **Uma única confirmação clara:** mostrar quem é o profissional atual, quem será o novo, as categorias de dados e histórico pessoal compartilhados e as consequências: revogação do acesso anterior, término da dieta profissional vigente anterior, cancelamento de futuras versões programadas e preservação dos dados/históricos do usuário acompanhado. Permitir voltar ou recusar. Não criar um fluxo de várias telas ou confirmações.
3. **Consentimento e validação:** usuário acompanhado autenticado e adulto, e-mail destinatário verificado, convite não expirado e autorização independente para o novo profissional. Nem o recebimento do e-mail nem o clique em link fazem a troca.
4. **Atomicidade obrigatória:** servidor e banco realizam em uma única transação lógica: encerrar vínculo antigo e suas autorizações, encerrar a vigência da dieta profissional antiga, cancelar agendamentos futuros da plano alimentar publicado antiga, registrar o aceite do convite e ativar o novo vínculo com autorização própria. **Jamais podem coexistir dois gestores ativos.** Se houver erro, reverter tudo e manter vínculo, permissões e dieta anteriores; o convite não aparece como Aceito.
5. **Histórico e limites:** o usuário acompanhado mantém diário, medições, metas pessoais, histórico de dietas e conta. Os planos antigos continuam consultáveis com autoria original; não os converter em plano pessoal ou atribuir ao novo profissional. A nova autorização não inclui bibliotecas privadas do gestor antigo nem publica automaticamente uma nova dieta.
6. **Concorrência e estado desatualizado:** impedir confirmação simultânea de convites diferentes ou aceite de convite vencido. Se o vínculo ativo mudar desde a abertura da tela, pedir para revisar a situação atual antes de confirmar; não substituir silenciosamente um terceiro profissional.
7. **Notificações:** somente confirmar **Aceito** no painel e e-mail ao gestor convidador depois de ativação real. **Quando a troca for concluída, o profissional anterior receberá aviso por e-mail com o nome do usuário acompanhado que saiu e o estado encerrado**, sem motivo ou dados nutricionais. A meta efetiva pessoal continua conforme a seção 11. Detalhes de transações, restrições e envio ficam na etapa 9.

## 10. Critérios de aceitação inicial

| Cenário | Comportamento esperado |
| --- | --- |
| Usuário nunca teve gestor | Tem conta e recursos pessoais gratuitos, sem precisar ser cadastrado como “usuário acompanhado” |
| Usuário aceita, encerra ou troca vínculo com gestor | Mantém a mesma identidade e conta; só vínculos, autorizações e planos alimentares publicados vigentes mudam |
| Usuário acompanhado está sem gestor | Usa recursos pessoais e seu histórico normalmente |
| Usuário acompanhado já possui gestor ativo e outro tenta criar vínculo ativo | Segundo vínculo simultâneo não é autorizado |
| Usuário acompanhado troca de gestor | Mantém diário, medições, metas pessoais e histórico de dietas, com autoria original |
| Profissional anterior tenta abrir o painel de dados do usuário acompanhado depois do encerramento | Consulta de acompanhamento é negada no servidor |
| Novo profissional tenta acessar o usuário acompanhado sem vínculo e autorização válida | Acesso negado; não herda compartilhamento anterior |
| Duas tentativas concorrentes procuram ativar profissionais diferentes para o mesmo usuário acompanhado | No máximo um vínculo se torna ativo, com garantia de integridade do sistema |
| Gestor convida usuário acompanhado sem vínculo | Convite pendente, nenhum acesso aos dados do usuário acompanhado |
| Gestor informa o e-mail de pessoa que ainda não tem conta | Enviar convite para que ela crie e verifique a conta antes de autorizar, sem criar vínculo antecipadamente |
| Usuário acompanhado já possui conta com o e-mail destinatário verificado | Pode entrar na plataforma, consultar quem convidou e aceitar ou recusar |
| Pessoa abre o convite sem login ou com conta de outro e-mail | Não ativa vínculo e não concede acesso a dados; identidade do destinatário é exigida |
| Pessoa cria conta após receber convite | Cadastro não aceita o convite automaticamente; exige confirmação expressa após autenticação |
| Convite é visualizado ou ignorado | Não ocorre aceite tácito, vínculo nem compartilhamento |
| Usuário acompanhado recusa convite | Nenhum vínculo ou acesso é criado |
| Usuário acompanhado autenticado aceita e autoriza categorias informadas | Vínculo ativado somente após checagem do destinatário e exclusividade; permissões delimitam o acesso |
| Gestor tenta consultar dados antes do aceite | Servidor nega acesso |
| Usuário acompanhado já tem gestor ativo e recebe segundo convite | O recebimento não altera vínculo nem dados; troca exige confirmação explícita em uma etapa |
| Usuário acompanhado tenta aceitar convite após 168 horas de sua emissão | Servidor impede ativação e registra estado expirado |
| Gestor reenvia convite pendente | Novo convite com novo prazo é criado; link anterior é invalidado |
| Usuário acompanhado tenta aceitar o link anterior depois do reenvio | Servidor recusa o convite substituído, sem criar vínculo |
| Usuário acompanhado autenticado aceita e autoriza validamente | Convite exibe **Aceito** no painel profissional somente após ativação do vínculo |
| Usuário acompanhado recusa o convite | Painel do gestor mostra **Recusado** sem exigir justificativa nem liberar dados privados |
| Usuário acompanhado não responde e prazo termina | Painel mostra **Expirado**, sem presumir recusa |
| Aceite e recusa são enviados concorrentemente | Um único resultado terminal prevalece, sem vínculo inconsistente ou dupla ativação |
| Usuário acompanhado aceita convite com sucesso e vínculo é ativado | Gestor convidador recebe e-mail informando aceite e encontra status Aceito no painel |
| Usuário acompanhado recusa convite | Gestor convidador recebe e-mail informando recusa, sem justificativa ou dados de saúde |
| Convite expira sem resposta | Não disparar e-mail de aceite/recusa; status Expirado permanece no painel |
| Falha na entrega de notificação por e-mail | Decisão registrada permanece válida e consultável no painel; falha pode ser tratada por processo de reenvio |
| Requisição de aceite ou recusa é repetida | Não duplicar eventos de decisão ou criar notificações contraditórias; regras de idempotência na etapa 9 |
| Usuário acompanhado recebe convite na plataforma | Uma tela informa nome do profissional, categorias de dados e ações claras Aceitar e autorizar / Recusar, com detalhes opcionais |
| Usuário acompanhado autoriza acompanhamento | Gestor vinculado pode consultar diário, metas, indicadores, peso/IMC e planos profissionais autorizados, respeitando período e permissões a definir |
| Gestor tenta consultar dados de usuário sem vínculo autorizado | Servidor nega mesmo que o cliente forneça o ID do usuário acompanhado |
| Gestor tenta editar refeição consumida pelo usuário acompanhado só por possuir acesso de acompanhamento | Operação negada, pois leitura não concede edição automática |
| Profissional tenta acessar dado fora das cinco categorias autorizadas | Acesso negado mesmo com vínculo ativo |
| Usuário acompanhado não visualiza detalhes opcionais e escolhe Recusar | Nenhum compartilhamento é criado; não obrigar leitura de telas adicionais para recusar |
| Usuário acompanhado possui registros alimentares anteriores ao convite e aceita | Gestor autorizado visualiza esses registros e seus indicadores históricos, sem nova solicitação de acesso |
| Usuário acompanhado possui histórico de pesagens anterior ao vínculo | Histórico de peso/IMC é consultável pelo gestor autorizado no acompanhamento |
| Novo gestor tenta abrir biblioteca privada de gestor anterior | Acesso negado, apesar da autorização para histórico do usuário acompanhado |
| Vínculo profissional é encerrado | Gestor anterior não pode consultar nem os registros históricos nem os novos no painel, sem excluir o histórico pessoal |
| Usuário acompanhado abre Meu acompanhamento e escolhe Encerrar acompanhamento, mas cancela | Vínculo e autorizações permanecem inalterados |
| Usuário acompanhado confirma Encerrar acompanhamento | Servidor encerra imediatamente vínculo e autorização, preservando diário, medidas, metas e histórico pessoal |
| Gestor mantém aba antiga aberta após o encerramento | Qualquer nova tentativa de carregar/editar dados pela API é negada pelo servidor |
| Usuário acompanhado acessa o próprio diário após encerrar o acompanhamento | Conta e histórico continuam disponíveis, sem exigir assinatura profissional |
| Usuário acompanhado confirma encerramento duas vezes | Operação não causa novos efeitos ou exclusão de registros; fluxo idempotente |
| Ex-gestor tenta consultar registros criados antes do vínculo | Acesso de acompanhamento negado, tal como para registros recentes |
| Usuário acompanhado encerra vínculo com dieta profissional vigente | Versão deixa de ser vigente e aparece para consulta no histórico, com autoria, conteúdo e período preservados |
| Usuário acompanhado encerra vínculo com próxima dieta profissional programada | Agendamento futuro é cancelado com motivo de encerramento; versão publicada permanece no histórico |
| Usuário acompanhado encerra vínculo tendo plano pessoal próprio | Plano pessoal e diário alimentar continuam inalterados |
| Agendador tenta ativar dieta do profissional depois do encerramento | Ativação negada, sem restabelecer vínculo ou plano alimentar publicado |
| Agendamento e encerramento ocorrem ao mesmo tempo | Registros convergem para ausência de plano alimentar compartilhado vigente, mantendo trilha cronológica |
| Usuário acompanhado consulta dieta do gestor anterior após término | Pode ler a versão histórica, sem reativá-la nem editá-la como plano alimentar compartilhado |
| Usuário acompanhado recebe convite de outro profissional e não confirma troca | Vínculo, acesso e dieta antigos permanecem, sem conceder acesso ao convidador |
| Usuário acompanhado confirma Aceitar e trocar de gestor | Encerra vínculo antigo e dieta vigente, cancela versões programadas, ativa novo vínculo autorizado, preservando histórico em transição única |
| Troca direta falha durante a operação | Reversão completa: vínculo, acesso e dieta antigos permanecem, convite não é Aceito |
| Dois convites diferentes são aceitos concorrentemente | No máximo um vínculo ativo; não há compartilhamento simultâneo |
| Profissional anterior mantém sessão aberta depois da troca | API nega consultas posteriores aos dados do usuário acompanhado |
| Vínculo ativo mudou enquanto convite estava aberto | Solicitar revisão da situação atual antes de confirmar, sem substituição silenciosa |
| Usuário acompanhado encerra vínculo com meta profissional de 2.200 kcal e sem ajuste posterior | Mantém 2.200 kcal como referência pessoal e autoria profissional histórica |
| Usuário acompanhado havia ajustado uma meta profissional de 2.200 para 2.000 kcal | Continua usando 2.000 kcal após encerramento, com plano alimentar publicado original preservada |
| Usuário acompanhado sem meta para proteínas encerra acompanhamento | Não cria nem preenche meta de proteínas |
| Usuário acompanhado troca de gestor e novo profissional não definiu metas | Última meta efetiva persiste até ajuste pessoal ou nova meta profissional válida |
| Ex-profissional tem mudança de meta agendada para após desligamento | Alteração futura não entra em vigor sem vínculo ativo |
| Usuário acompanhado edita meta pessoal preservada após desvinculação | Edição livre, sem aviso de plano alimentar publicado vigente do ex-profissional, preservando histórico |
| Usuário acompanhado encerra vínculo diretamente | Enviar e-mail ao ex-gestor com nome do usuário acompanhado e confirmação de encerramento, sem motivo ou dados de saúde |
| Usuário acompanhado confirma troca por convite de outro gestor | Após transação concluída, ex-gestor recebe e-mail identificando apenas pelo nome o usuário acompanhado desvinculado; novo profissional recebe notificação de aceite |
| Troca é recusada, cancelada ou falha | Não enviar e-mail de vínculo encerrado ao profissional anterior |
| Envio do e-mail de encerramento falha | Encerramento permanece válido e aparece no histórico profissional restrito; acesso a dados do usuário acompanhado segue revogado |
| Gestor anterior abre link do e-mail após desvinculação | Visualiza apenas o evento de encerramento, nunca o painel ou dados privados do usuário acompanhado |
| Gestor contrata gestão sem CRN | Assinatura pode ser contratada; poderá convidar/gerir terceiros somente com vínculo e autorização válidos |
| Gestor informa CRN mas credencial não foi verificada | Não exibir selo CRN verificado; assinatura e permissões de gestão não mudam |
| Profissional com registro verificado e aprovado envia convite | Convite permitido, sujeito às regras de autorização, prazo e controles contra abuso |
| Dados do CRN são divergentes ou pertencem a outra pessoa | Negar selo CRN verificado; permitir correção sem restringir assinatura de gestão já autorizada |
| Conta Em validação tenta chamar API de plano alimentar publicado diretamente | Servidor bloqueia ação sem depender da interface |
| Gestor com CRN em validação já possui assinatura | Recursos pessoais e de gestão seguem disponíveis conforme assinatura e consentimentos; selo continua pendente |
| Gestor tenta reenviar convites várias vezes ao mesmo e-mail em curto período | Servidor aplica limite por destinatário e por emissor, evitando spam; não criar convites extras |
| Profissional válido envia convites moderados a usuários acompanhados diferentes | Uso legítimo permitido sem desafios adicionais ou bloqueio indiscriminado |
| Convite ou reenvio é bloqueado por frequência excessiva | Apresentar mensagem simples de limite temporário, sem informar se destinatário tem conta e sem afirmar envio |
| Convite ativo existe e tentativa de reenvio é bloqueada | Convite/link e prazo anterior permanecem inalterados |
| Profissional usa requisições diretas ou múltiplos cliques para contornar limite | API rejeita o excesso mesmo fora da interface, sem duplicar efeitos |

## 11. Notificação de encerramento ao gestor anterior (**confirmada no MVP**)

- **Eventos:** enviar e-mail ao gestor anterior quando o usuário acompanhado confirmar **Encerrar acompanhamento** ou concluir **Aceitar e trocar de gestor**. Mostrar também o encerramento na área profissional de histórico de vínculos, sem manter acesso ao painel de acompanhamento daquele usuário acompanhado.
- **Identificação mínima útil:** a mensagem conterá **o nome do usuário acompanhado que teve seu vínculo encerrado**, já conhecido do profissional por sua relação anterior, e o resultado **acompanhamento encerrado**; poderá indicar a data/hora e o acesso à área de vínculos encerrados na plataforma. Não exigir justificativa do usuário acompanhado nem informar o motivo; **não incluir valores nutricionais, refeições, peso, metas, dietas, prontuário, nome do novo profissional ou seus dados**.
- **Destinatário restrito:** enviar **somente ao e-mail verificado associado à conta do gestor que possuía o vínculo imediatamente anterior**, e apenas quando essa relação realmente existia. Não notificar outros gestores, convidados sem vínculo nem endereços fornecidos por terceiros.
- **Consistência e privacidade:** criar o evento de notificação **somente depois da confirmação persistida do encerramento ou troca**. Falha de entrega não reabre vínculo, não devolve acesso e não impede a operação concluída; o estado permanece disponível no histórico profissional restrito. Não abrir acesso aos dados privados do usuário acompanhado por meio de link de notificação.
- **Fluxo simples:** usuário acompanhado não precisa escrever motivo nem decidir se o gestor será avisado; essa consequência poderá ser apresentada discretamente na confirmação única já aprovada. A etapa 9 tratará entrega assíncrona, idempotência de eventos, duplicidade e falhas.

## 12. Continuidade das metas e propriedade dos dados (**confirmado no MVP**)

- **Regra geral:** o usuário acompanhado não perde seus registros de acompanhamento ao encerrar ou trocar de gestor: diário, pesagens, metas, indicadores e conteúdo das versões de dietas profissionais publicadas para ele permanecem consultáveis em sua conta, sujeitos a direitos de exclusão/retificação e obrigações de guarda (etapa 8). Isso não inclui automaticamente bibliotecas internas e prontuários privados do profissional.
- **Metas sem interrupção:** conservar a última meta efetiva para cada nutriente que já possua valor (calorias, proteínas, carboidratos, gorduras). Tanto a meta definida pelo profissional quanto um ajuste posterior do usuário acompanhado podem ser a última referência. Não zerar, apagar, inventar nova meta ou alterar indicadores de períodos anteriores.
- **Exemplo:** gestor definiu 2.200 kcal, mas usuário acompanhado ajustou sua meta efetiva para 2.000 kcal: após a desvinculação **2.000 kcal continuam nos indicadores**, e a plano alimentar publicado original de 2.200 kcal fica no histórico. Sem ajuste do usuário acompanhado, mantém 2.200 kcal como referência pessoal, não como plano alimentar compartilhado atual.
- **Livre edição posterior:** a meta mantida passa a servir como referência pessoal; usuário pode ajustá-la sem o aviso específico aplicável a uma plano alimentar publicado de profissional ainda vinculado. A origem e autoria da referência original continuam visíveis no histórico.
- **Troca direta:** a referência atual persiste até ser alterada pelo usuário acompanhado ou substituída, quando entrar em vigor, por nova meta profissional de gestor atualmente vinculado e autorizado. O novo profissional não herda autoria antiga.
- **Sem comandos futuros do ex-profissional:** atualização de meta agendada pelo gestor anterior não pode modificar a referência após revogação do vínculo. A etapa 9 detalhará garantias de transação e concorrência.
- **Dieta preservada, vigência encerrada:** o usuário acompanhado conserva o conteúdo publicado e pode consultá-lo, mas conforme aprovação anterior a plano alimentar compartilhado deixa de ser vigente e suas futuras ativações são canceladas. Preservar dados não significa manter uma plano alimentar publicado como ativa.

## 13. Gestor sem CRN obrigatório; credencial verificada opcional (**novo modelo aprovado**)

- **Qualquer usuário adulto** pode contratar a assinatura mensal única e, com vínculo autorizado, utilizar as ferramentas de gestão de outros usuários. **Não exigir CRN** para conta, checkout, assinatura, convites, vínculos, leitura consentida de dados, compartilhamento de planos/metas e uso da biblioteca.
- **Credencial opcional:** nutricionistas podem declarar número/região de CRN e solicitar comprovação de titularidade e regularidade. Somente após conferência confiável exibir **CRN verificado**. Declaração sem verificação não autoriza selo.
- **Nomenclatura:** área genérica **Meus acompanhados**; apenas gestor identificado como nutricionista com CRN verificado pode usar a denominação contextual **Meus pacientes**, sem tipo de conta adicional ou privilégio funcional.
- **Permissões reais:** assinatura válida, vínculo ativo, aceite informado e autorização vigente do titular. CRN não libera dados, não substitui assinatura e não dá prioridade de autorização.
- **Credencial desatualizada:** remover/retirar selo após identificação de irregularidade sem cancelar automaticamente vínculos ou assinatura; revisão da fonte, método, rechecagem, apresentação e responsabilidades nas etapas 8/9.
- **Separação legal:** plataforma oferece organização e comunicação de dados, não presta serviço próprio de consulta ou prescrição nutricional. A utilização da ferramenta não comprova habilitação do gestor para atos regulados. Publicidade, termos e responsabilidade externa serão revisados na etapa 8.

## 14. Proteção automática contra abuso de convites (**confirmada no MVP**)

- **Envio e reenvio com limites automáticos:** proteger convites por e-mail contra solicitações repetidas, spam e disparos em massa, incluindo **múltiplos reenvios ao mesmo destinatário**, sem prejudicar o profissional que convida diversos usuários acompanhados legítimos.
- **Regras de uso sem fricção:** em uso normal não exigir CAPTCHA, etapas extras de confirmação, justificativa de convite nem bloqueios excessivos. Aplicar controles no servidor considerando, entre outros, **identidade do gestor assinante e autorizado**, **endereço destinatário**, **frequência de reenvios**, padrões de volume e tentativas de contornar limites.
- **Separação por escopo:** um limite temporário para reenvios a um destinatário não deve impedir automaticamente convites legítimos a outras pessoas. Proteger também contra novos convites usados para contornar uma restrição do mesmo par gestor/destinatário. Limites gerais por emissor e proteções em nível de infraestrutura podem complementar a regra específica.
- **Feedback simples e sem vazamento de dados:** se o limite for alcançado, informar discretamente que o convite não pôde ser enviado agora e poderá ser tentado mais tarde, com orientação de aguardar. **Não afirmar envio bem-sucedido quando o servidor não autorizou nem enfileirou o envio**. Não revelar se o e-mail pertence a uma conta, se o destinatário está cadastrado ou dados privados do usuário acompanhado.
- **Estados preservados:** tentativa bloqueada não cria convite válido, não muda status de convite anterior nem cancela vínculo existente. Em reenvio autorizado, manter regra já aprovada de novo convite/link e invalidação atômica do link anterior. Falhas de entrega posteriores não se confundem com bloqueio de limite.
- **Garantias técnicas adiadas à etapa 9:** definir **valores numéricos, janelas de tempo, cooldown por destinatário, limites por emissor, medidas contra contorno por contas/IP, idempotência e observabilidade**, considerando limites do provedor de e-mail. Não fixar valores arbitrários como requisito de produto nesta etapa. Adicionar testes contra múltiplos cliques, concorrência e explosão de reenvios; não depender do botão desabilitado no frontend.

## 15. Pendências técnicas e jurídicas encaminhadas a outras etapas

- **Convites definidos funcionalmente:** convite por e-mail, validade de **7 dias**, reenvio invalidando o anterior, retorno ao profissional, escopo de cinco categorias, aceite simples e **limites automáticos de envio e reenvio sem etapas extras**. **Etapa 9:** parâmetros numéricos, segurança dos endpoints, idempotência, provedor de e-mail, entrega e retentativas; sem revelar a existência de conta do destinatário.
- **Troca direta aprovada:** usuário acompanhado pode aceitar convite de outro profissional com uma única confirmação, encerrando o vínculo antigo e ativando o novo atomicamente, sem dois profissionais simultâneos.
- **Planos profissionais antigos — decidido:** ao encerrar vínculo, a dieta profissional vigente vai para consulta no histórico e eventuais versões futuras programadas são canceladas, preservando conteúdo e autoria. Troca direta por convite já definida; **última meta efetiva preservada como referência pessoal após desvinculação**, com origem profissional registrada.
- **Revogação e encerramento confirmados:** o histórico pessoal anterior ao vínculo será acessível ao profissional autorizado; o usuário acompanhado poderá encerrar o acompanhamento em **Meu acompanhamento**, com confirmação simples, revogação imediata do acesso, encerramento da dieta profissional vigente e cancelamento das versões futuras. **Aviso com nome do usuário acompanhado ao ex-profissional por e-mail e no histórico de vínculos já aprovado**; não inclui motivo nem dados nutricionais, e não restabelece acesso. A autorização não inclui bibliotecas privadas anteriores.
- **Selo CRN opcional — decidido:** solicitar registro somente de quem desejar obter a identificação CRN verificado; esta verificação nunca condiciona a assinatura e a gestão. Etapas 8/9: forma de conferência, rechecagem e apresentação da credencial.
- **Dados profissionais após encerramento:** guarda obrigatória, eliminação, portabilidade e outros requisitos da LGPD na etapa 8.

**Revisão transversal aprovada na etapa 7:** qualquer adulto com assinatura válida pode atuar como gestor; o selo CRN é opcional e separado das permissões. Os fluxos de vínculo e autorização da etapa 6 permanecem válidos. **Encerramento funcional da etapa 6:** convites, autorização, compartilhamento, encerramento/troca de gestor, preservação dos dados/metas/dietas, aviso ao ex-profissional, verificação opcional do CRN para selo e proteção contra abuso estão definidos para o MVP. Requisitos técnicos e jurídicos identificados serão detalhados nas etapas 8/9. Esta especificação não representa software implementado ou testado.
