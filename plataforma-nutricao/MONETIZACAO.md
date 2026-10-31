# Monetização e assinaturas — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 7 de 10 — Monetização e assinaturas (em andamento)
> **Versão:** 1.0
> **Data:** 31/10/2026
> **Referências:** [IDEA.md](./IDEA.md), [ROADMAP.md](./ROADMAP.md), [PACIENTES.md](./PACIENTES.md), [DIARIO.md](./DIARIO.md), [DIETAS.md](./DIETAS.md)

## 1. Objetivo

Definir como a plataforma de nutrição se sustenta financeiramente **sem cobrar do usuário pelo uso da própria conta ou por aceitar acompanhamento nutricional dentro da plataforma**, preservando a simplicidade para usuários e gestores.

**Terminologia comercial aprovada:** “usuário comum” e “usuário acompanhado” são **a mesma pessoa e a mesma conta**. “Usuário acompanhado” é o nome usado **na área do gestor** para os usuários sob seu acompanhamento/gestão; não é tipo de conta, plano, cadastro ou papel comercial separado. Ter ou não ter vínculo de acompanhamento não altera a gratuidade dos recursos pessoais.

## 2. Modelo aprovado para o MVP

1. **Todos os usuários adultos usam gratuitamente os recursos pessoais**, estejam ou não vinculados a um gestor: diário alimentar, registro de refeições, metas próprias, indicadores e gráficos, histórico de consumo, peso/IMC e planos alimentares pessoais definidos no MVP.
2. **O mesmo usuário pode aceitar ou recusar convite sem mudar de conta ou pagar.** Se houver vínculo autorizado, pode consultar dieta de gestão atribuída, registrar refeições, acompanhar indicadores e manter seus dados pessoais. Apenas na gestão do gestor esse usuário será denominado **usuário acompanhado**.
3. **Assinatura para gestão por qualquer usuário adulto.** O MVP mantém **um único plano mensal** para ferramentas de gestão: convites, vínculos, consulta de dados autorizados, criação/publicação de planos alimentares e metas de acompanhamento, biblioteca e histórico. **CRN não é requisito comercial nem funcional**, apenas credencial opcional com selo verificado. **Preço e eventual limite de usuários acompanhados seguem sem decisão.**
4. **Separar recursos pessoais e de gestão.** A própria conta continua com acesso gratuito às ferramentas pessoais sem assinatura de gestão. O CRN verificado é um atributo de credencial separado, não outro tipo de conta.
5. **Dados do usuário acompanhado não dependem da cobrança do gestor.** Diário, metas, histórico, medições e versões de dietas publicadas para o usuário acompanhado não podem ser apagados, sequestrados ou indisponibilizados ao titular devido a cancelamento ou falha de pagamento do de gestão.
6. **Pagamento não equivale a consentimento nem habilitação de gestão.** A assinatura nunca concede acesso a dados de outros usuários sem vínculo e autorização; o selo CRN opcional indica credencial conferida, mas não é necessário para assinar ou utilizar gestão.
7. **Sem monetização adicional presumida:** anúncios, cobranças por usuário acompanhado, funcionalidades pessoais pagas e comissão sobre consultas não foram aprovados para este MVP.

### 2.1. Assinatura para qualquer gestor e selo CRN opcional (**modelo aprovado**)

- **Uma conta de usuário adulto:** qualquer pessoa pode usar os recursos pessoais gratuitamente e, se desejar gerenciar outros usuários, contratar **o único plano mensal de gestão**. **CRN não é exigido** para criar conta, contratar, convidar, administrar vínculos, criar ou compartilhar planos, consultar dados consentidos ou manter biblioteca de refeições.
- **Verificação opcional de credencial:** quem afirmar ser nutricionista poderá solicitar conferência da titularidade e regularidade do CRN para exibir o selo **CRN verificado**. Sem registro verificado, a interface não identifica o usuário como nutricionista habilitado. Número declarado, ainda não aprovado, não recebe selo.
- **Permissões independentes do selo:** gerir dados de terceiros exige assinatura de gestão vigente, vínculo ativo e consentimento explícito do titular; o selo não abre acesso adicional, não substitui pagamento e não amplia escopo dos dados autorizados.
- **Contratação expressa:** o início da assinatura/cobrança depende da escolha explícita do titular e confirmação da operação comercial. Verificação de CRN não gera cobrança, isenção, plano especial ou assinatura automática.
- **Apresentação do produto:** ferramenta de organização, comunicação e compartilhamento de planos e dados alimentares, sem serviço da plataforma de diagnóstico ou prescrição clínica. O uso da ferramenta não concede habilitação profissional ao gestor. Direitos, publicidade de credenciais e responsabilidade pela atuação de terceiros serão revisados na etapa 8.
- **Atenção a legados:** as regras anteriores que exigiam CRN aprovado para checkout ou para usar gestão/convites **foram substituídas** por esta decisão.

## 3. Experiência de uso aprovada

- **Usuário, com ou sem acompanhamento:** usa a mesma conta e não encontra checkout para criar conta, usar recursos pessoais, aceitar convite ou consultar plano que lhe foi publicado. O acompanhamento dentro da plataforma é gratuito; a interface pessoal não apresenta “usuário acompanhado” como um perfil separado.
- **Gestor:** identifica com clareza que as funcionalidades gestores são oferecidas por assinatura mensal, sem misturar pagamento e dados de usuários acompanhados. Detalhes da oferta e do fluxo de assinatura ainda serão decididos.
- **Simplicidade confirmada:** apresentar **uma oferta mensal de gestão única** com todas as ferramentas gestores já aprovadas. Não introduzir níveis comerciais diferentes, nem mostrar preços/limites fictícios antes da decisão.

## 4. Cancelamento voluntário pelo gestor (**confirmado no MVP**)

- O gestor poderá **cancelar a renovação automática** de sua assinatura mensal. O cancelamento **não interrompe imediatamente** as funções gestores: a assinatura permanece com direito de uso **até a data/hora de término do período já pago**.
- A interface deve exibir de forma clara a **data/hora até quando o acesso de gestão permanecerá habilitado**, sem cobrar outro ciclo após a solicitação de cancelamento. O estado comercial deve distinguir **renovação cancelada, período pago ainda vigente** de **período encerrado**.
- Durante o período já pago, as funções gestores permanecem disponíveis conforme as mesmas condições de **CRN validado, vínculo e autorização do usuário**. O cancelamento não concede direitos extras nem dispensa as verificações existentes.
- **Após terminar o período pago, aplicar o acesso restrito da seção 5:** manter conta pessoal, área de assinatura e lista administrativa mínima de vínculos, mas **bloquear acesso a dados nutricionais, novos convites e ações gestores de dietas/metas**. **Versões já publicadas e programadas continuam podendo entrar em vigor na data prevista**, mesmo depois do vencimento comercial, desde que o vínculo e a autorização do usuário permaneçam válidos, conforme seção 6. Não equivale a autorizar nova publicação ou alteração pelo de gestão.
- **Não confundir assinatura com vínculo:** o pedido de cancelamento ou o fim do período pago **não encerra automaticamente os vínculos de acompanhamento**, não comunica falsamente que o usuário encerrou o vínculo e **não exclui dietas publicadas, diário, metas, histórico ou outras informações do usuário**. O titular permanece com acesso aos próprios dados. Isso é diferente do encerramento de vínculo solicitado pelo usuário, que revoga permissões e encerra a vigência da plano alimentar conforme [PACIENTES.md](./PACIENTES.md).
- **Sem efeitos retroativos ou mudanças silenciosas:** cancelar a renovação por si só não despublica uma dieta vigente, não altera metas efetivas do usuário nem cancela agendamentos futuros de plano alimentar; o que deve acontecer **quando o período pago terminar** será decidido explicitamente, preservando integridade e histórico.
- A etapa 9 especificará verificação confiável de período pago no servidor, referência temporal inequívoca, idempotência de cancelamentos, eventos de cobrança e processamento de webhooks. Não confiar apenas na interface ou numa data fornecida pelo cliente.

## 5. Acesso após fim do período pago (**confirmado no MVP**)

- **Acesso mantido:** própria conta pessoal e seus recursos gratuitos; área de assinatura/pagamentos para visualizar e regularizar o estado comercial; listagem administrativa **Meus acompanhados** com **nome e estado do vínculo** dos usuários vinculados, sem dados nutricionais, indicadores, detalhes das dietas ou acesso a perfil completo do usuário.
- **Acesso de gestão bloqueado:** leitura de diário alimentar, calorias, macronutrientes, peso/IMC, metas, indicadores, histórico nutricional e planos alimentares de usuários; edição/publicação/programação de dietas e metas gestores; envio de convites e ativação de novos vínculos; uso da biblioteca de gestão paga. Deve bloquear também as respectivas chamadas da API, inclusive em sessões já abertas, sem depender de controles do frontend.
- **Não confundir assinatura e vínculo:** o fim do período pago **não encerra automaticamente os vínculos existentes**, nem dispara e-mail afirmando que o usuário encerrou o acompanhamento. Conteúdo das dietas publicadas, registros, metas e históricos continuam preservados e acessíveis ao próprio usuário.
- **Após reativar a assinatura:** as capacidades gestores só retornam quando **CRN estiver validado** e cada vínculo e autorização do usuário ainda forem válidos. Não reabrir autorização revogada, relação encerrada nem plano alimentar que perdeu sua vigência por término do vínculo.
- **Dietas futuras publicadas antes da expiração — aprovado:** a execução automática de um agendamento publicado anteriormente **não é uma nova ação do gestor** e pode ocorrer mesmo com assinatura vencida, desde que vínculo e autorização do usuário ainda estejam válidos. A regra detalhada encontra-se na seção 6; o de gestão continua impedido de criar ou alterar planos alimentares sem assinatura.
- **Verificação no servidor:** período pago, autorização, vínculo, CRN e papel devem ser validados antes de permitir ações gestores; testes negativos, idempotência e invalidação de acesso/caches serão detalhados na etapa 9.

## 6. Ativação de dietas previamente programadas após vencimento (**confirmada no MVP**)

- Uma versão de dieta de gestão **publicada e programada quando o gestor tinha permissão de gestão válida** pode **entrar em vigor automaticamente na data/hora estabelecida**, mesmo se a assinatura mensal do autor tiver expirado após a publicação.
- **Condição obrigatória no momento da ativação:** deve existir **o mesmo vínculo de acompanhamento ainda ativo e autorização vigente do usuário**. Se o usuário encerrou o acompanhamento, trocou de de gestão ou revogou a autorização, o agendamento anterior não pode ativar; vale a regra da etapa 6, que cancela programações do vínculo encerrado.
- **Separação das operações:** ativação de publicação antiga, realizada pelo processo programado, não é publicação nova nem edição pelo gestor. Durante a assinatura expirada, **não permitir** novos convites, novas planos alimentares/metas, edição, reagendamento, cancelamento manual de gestão ou acesso aos dados nutricionais, conforme seção 5.
- **Histórico e integridade:** preservar autor, conteúdo e horário originalmente programado, encerrar a versão anterior sem sobreposição e registrar a ativação real. O usuário pode continuar consultando suas dietas sem depender da assinatura do de gestão. Não criar versões ou alterar retrospectivamente dados do diário.
- **Concorrência e falhas:** a etapa 9 deverá prever agendador **idempotente**, autorização e vínculo conferidos na transação que efetiva a ativação, proteção contra disputa com encerramento/troca e verificação da situação de gestão segundo as regras de CRN definidas nas etapas 8/9. A expiração comercial isolada **não** é motivo para cancelar o agendamento.

## 7. Falha na renovação mensal e tolerância de 7 dias (**confirmado no MVP**)

- **Aplicação restrita a renovações:** quando uma assinatura de gestão **já paga e ativa** chega à renovação mensal e a cobrança **falha**, iniciar **7 dias corridos (168 horas)** de tolerância a partir do vencimento original do ciclo. A ausência de confirmação de pagamento não pode ser registrada como pagamento aprovado.
- **Direitos durante a tolerância:** durante os sete dias, manter as funcionalidades gestores normalmente disponíveis, sujeitas à validação de CRN e às autorizações/vínculos dos usuários. Mostrar na área de assinatura um **aviso claro** de falha na renovação, data/hora final da tolerância e opção de regularizar o pagamento; notificação por e-mail transacional ao gestor sem dados de usuários acompanhados. Não confundir **em tolerância por falha de cobrança** com **cancelamento voluntário da renovação**.
- **Tentativas seguras de recuperar o pagamento:** a plataforma pode repetir a tentativa de cobrança conforme regras do provedor e permitir regularização pelo assinante, mas deve impedir **cobranças duplicadas e concessões duplicadas de período pago** em chamadas concorrentes, retentativas, webhooks e confirmações tardias. Detalhes de idempotência, respostas do gateway, frequência de tentativas e comunicação serão decididos na etapa 9.
- **Se regularizar durante os sete dias:** encerrar o estado de tolerância e manter a assinatura ativa, com continuidade do ciclo e sem perda de acesso. Registrar o pagamento confirmado sem inventar créditos ou ciclos adicionais.
- **Se não regularizar em até 168 horas:** terminado o prazo, **restringir as funcionalidades gestores** conforme seção 5: manter conta pessoal, área de assinatura e **listagem administrativa mínima de vínculos**; bloquear dados nutricionais, convites e ações gestores. O limite deve ser controlado pelo **servidor**, inclusive para sessões abertas. Vínculos, histórico e dados dos usuários permanecem preservados; dietas já publicadas e programadas seguem a regra da seção 6.
- **Primeira contratação sem tolerância:** a criação de uma nova assinatura não habilita recursos gestores apenas por tentar pagar; exigir **confirmação efetiva do primeiro pagamento** (e credencial opcional verificada). Período de teste promocional, se houver, será objeto de decisão separada e não deve ser tratado como pagamento confirmado.
- **Sem vínculo automático com cancelamento:** o pedido expresso de cancelamento segue sua regra específica da seção 4 (acesso até fim do período pago, **sem adicionar sete dias automaticamente**). A tolerância é exclusiva para falha real de **renovação** de assinatura previamente paga.
- **Estado e relógio verificáveis:** registrar vencimento do ciclo, falha, início/fim da tolerância, tentativas, confirmação de pagamento e mudança de acesso. A etapa 9 detalhará a transição idempotente de estados e concorrência entre pagamento confirmado no limite e processo de suspensão, evitando reativações/negações incorretas.

## 8. Questões a decidir na etapa 7

- **Estrutura da oferta — confirmada:** somente **um plano de gestão mensal** no MVP, incluindo todas as ferramentas gestores previstas; não criar níveis de assinatura diferentes no lançamento.
- **Limite de usuários acompanhados — decisão adiada explicitamente:** avaliar mais adiante **se existirá limite** de usuários que um gestor poderá acompanhar, antes de determinar qualquer quantidade. **Plano único não equivale a usuários acompanhados ilimitados.** Outros limites e regras de uso justo também não foram decididos.
- **Preço — decisão adiada explicitamente:** definir posteriormente valor da mensalidade e condições comerciais, considerando custos e validação de mercado. Não fixar preço, moeda, desconto ou promoção por suposição.
- **Período de teste — em aberto:** decidir após estimativa dos custos da etapa 9 se haverá teste gratuito e sua duração. Os prazos de 14 e 30 dias não foram aprovados.
- **Pagamento:** provedor, renovação recorrente, confirmação, comprovantes e comunicação de falhas.
- **Cancelamento e acesso após vencimento — decididos:** renovação para, acesso de gestão continua até o fim do período pago. Depois, ficam conta pessoal, assinatura e lista administrativa mínima; acesso nutricional e ações gestores ficam bloqueados, preservando vínculos e dados.
- **Versões futuras já publicadas — decidido:** a ativação automática na data agendada continua possível depois do vencimento da assinatura, **somente se vínculo e autorização do usuário continuarem válidos**, sem permitir novas ações gestores.
- **Falha na renovação — decidida:** conceder **7 dias corridos (168 horas) de tolerância** apenas para renovação de assinatura anteriormente paga, com aviso, funcionalidade de gestão mantida e tentativas de regularização sem cobranças duplicadas. Após o prazo sem pagamento, aplicar o acesso restrito da seção 5; a **primeira contratação requer pagamento confirmado**. Horários exatos de tentativas, integração e reconciliação ficam para a etapa 9.
- **Estados de assinatura:** definição do ciclo de vida comercial e mensagens adequadas para o gestor.

Regras técnicas de cobrança, idempotência, webhooks, segurança e integração serão detalhadas na etapa 9. Proteção de dados, retenção e responsabilidades na etapa 8.

## 9. Critérios de aceitação iniciais

| Cenário | Resultado esperado |
| --- | --- |
| Adulto sem gestor cria conta | Consegue usar diário, metas, gráficos, histórico e planos pessoais sem pagar |
| Usuário gratuito recebe convite válido de gestor | Aceita na mesma conta e acompanha dieta/indicadores sem checkout nem mudança para “conta de usuário acompanhado” |
| Usuário encerra acompanhamento e depois recebe novo convite | Mantém a mesma conta gratuita, dados e recursos pessoais; apenas o vínculo de gestão muda |
| Gestor deseja usar ferramentas gestores | Há um único plano mensal incluindo as ferramentas gestores definidas, sujeito também à validação do CRN |
| Gestor compara ofertas de assinatura no MVP | Plataforma oferece somente um plano de gestão, sem níveis comerciais múltiplos |
| Limite de usuários acompanhados ainda não foi decidido | Produto não presume número máximo nem promete usuários acompanhados ilimitados |
| Preço ainda não foi definido | Documentação e produto não inventam valor de assinatura nem condições de cobrança |
| Usuário sem CRN deseja contratar gestão | Pode contratar mediante escolha expressa e pagamento confirmado, sem apresentar-se como gestor verificado |
| Usuário com CRN em validação contrata gestão | Contratação e ferramentas de gestão não dependem da decisão sobre o selo |
| Gestor informa CRN sem verificação de titularidade/regularidade | Selo não aparece como verificado |
| Gestor obtém CRN validado após contratar | Selo CRN verificado pode ser exibido; permissões de gestão não são alteradas |
| CRN torna-se irregular após verificação | Selo deixa de ser exibido como válido até nova verificação, sem cancelar automaticamente assinatura ou vínculos |
| Gestor sem vínculo e autorização tenta consultar dados alheios | Servidor bloqueia independentemente de assinatura ou selo CRN |
| Gestor tem assinatura mas não possui vínculo autorizado com um usuário acompanhado | API bloqueia acesso aos dados desse usuário acompanhado |
| Gestor cancela voluntariamente a assinatura durante período pago | Renovação automática é interrompida, mas funcionalidades gestores seguem disponíveis até o fim do período já pago |
| Gestor consulta vencimento após cancelar | Interface informa data/hora de término do acesso pago, distinguindo cancelamento agendado de período encerrado |
| Período pago termina após cancelamento | Gestor mantém conta pessoal, área de assinatura e lista administrativa de vínculos existentes, sem acesso nutricional nem ações gestores |
| Assinatura expirada, gestor abre Meus acompanhados | Somente nomes e estados dos vínculos, sem acesso aos diários, indicadores, dietas ou dados privados |
| Assinatura expirada, gestor tenta consultar dados por API ou criar nova dieta | Servidor bloqueia requisição mesmo com CRN verificado e vínculo anteriormente autorizado |
| Assinatura expirada, gestor tenta enviar novo convite | Ação é negada, sem criar convite nem vínculo |
| Assinatura reativada e vínculo/autorização ainda válidos | Acesso de gestão pode voltar após conferir assinatura, CRN e permissões, sem reviver vínculos encerrados |
| Usuário encerrou o vínculo durante a suspensão comercial | A reativação de assinatura não restaura a autorização revogada nem a dieta de gestão anteriormente encerrada |
| Gestor cancela assinatura com usuários vinculados | Vínculos não são encerrados automaticamente, nem enviada mensagem falsa de fim de acompanhamento; dados e planos alimentares publicadas permanecem preservados |
| Gestor cancela enquanto existe dieta de gestão programada | Cancelar renovação não cancela por si só o agendamento; pode ativar após vencimento se vínculo e autorização continuarem válidos |
| Versão foi publicada e programada antes do vencimento, vínculo segue autorizado | Ativação automática ocorre na data agendada mesmo com assinatura expirada, sem nova publicação |
| Usuário encerra ou troca vínculo antes da data de ativação com assinatura expirada | Versão futura do de gestão anterior não ativa, agendamento é cancelado pelas regras de encerramento do vínculo |
| Gestor com assinatura vencida tenta reagendar versão anterior | Servidor rejeita alteração, ainda que a ativação automática original continue autorizada |
| Vencimento comercial e ativação agendada coincidem | Processo de ativação aplica data programada e vínculo/autorização vigentes, sem confundir vencimento da assinatura com revogação de vínculo |
| Usuário acessa conta depois que assinatura do gestor terminou | Próprios dados, histórico, diário e metas permanecem acessíveis sem custo |
| Renovação mensal de assinatura previamente paga falha | Inicia tolerância de 7 dias/168h com aviso e mantém funcionalidades gestores durante o prazo |
| Gestor regulariza a cobrança durante a tolerância | Assinatura volta ao estado normal sem interrupção das funções nem cobranças duplicadas |
| Pagamento de renovação permanece pendente por mais de 7 dias | Depois do prazo, servidor restringe acesso a dados nutricionais e ações gestores, preservando conta, listagem mínima e vínculos |
| Primeira tentativa de contratação tem pagamento não confirmado | Não libera funcionalidades gestores por tolerância; precisa de pagamento confirmado e credencial opcional verificada |
| Gestor cancelou voluntariamente a renovação | Não ganha automaticamente sete dias extras após fim do período pago; segue regra de cancelamento |
| Gateway reenvia o mesmo evento ou duas tentativas concorrem | Não duplicar cobrança, extensão de ciclo nem evento de concessão de acesso |
| Cobrança confirma no limite do prazo e suspensão é processada | Estado final deve refletir pagamento verificado de forma consistente, sem suspensão indevida |
| Dieta agendada antes de suspensão por falta de pagamento | Continua elegível à ativação automática se vínculo e autorização estiverem válidos, conforme seção 6 |
| Usuário acompanhado encerra vínculo com gestor | Mantém conta e dados independentemente do estado comercial do de gestão |

**Próxima decisão:** definir como o selo **CRN verificado** será apresentado e quando será revalidado, separadamente das permissões de gestão; detalhes operacionais da checagem ficam nas etapas 8/9. **Teste gratuito, preço e existência/quantidade de limite de usuários acompanhados continuam adiados para revisão nas etapas 9/10.**

**Pendência obrigatória:** na etapa 9, estimar custos operacionais. Antes de concluir a etapa 10, retomar e decidir explicitamente teste gratuito e duração, preço mensal e existência/quantidade de limite de usuários acompanhados. Ver o ponto de controle em [ROADMAP.md](./ROADMAP.md). Nenhuma dessas escolhas está aprovada.

**Observação:** este arquivo registra requisitos aprovados; não representa implementação, testes ou cobrança em produção.
