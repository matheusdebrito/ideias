# Monetização e assinaturas — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 7 de 10 — Monetização e assinaturas (em andamento)
> **Versão:** 0.6
> **Data:** 31/10/2026
> **Referências:** [IDEA.md](./IDEA.md), [ROADMAP.md](./ROADMAP.md), [PACIENTES.md](./PACIENTES.md), [DIARIO.md](./DIARIO.md), [DIETAS.md](./DIETAS.md)

## 1. Objetivo

Definir como a plataforma de nutrição se sustenta financeiramente **sem cobrar do usuário pelo uso da própria conta ou por aceitar acompanhamento nutricional dentro da plataforma**, preservando a simplicidade para usuários e nutricionistas.

**Terminologia comercial aprovada:** “usuário comum” e “paciente” são **a mesma pessoa e a mesma conta**. “Paciente” é o nome usado **na área do nutricionista** para os usuários sob seu acompanhamento/gestão; não é tipo de conta, plano, cadastro ou papel comercial separado. Ter ou não ter vínculo de acompanhamento não altera a gratuidade dos recursos pessoais.

## 2. Modelo aprovado para o MVP

1. **Todos os usuários adultos usam gratuitamente os recursos pessoais**, estejam ou não vinculados a um nutricionista: diário alimentar, registro de refeições, metas próprias, indicadores e gráficos, histórico de consumo, peso/IMC e planos alimentares pessoais definidos no MVP.
2. **O mesmo usuário pode aceitar ou recusar convite sem mudar de conta ou pagar.** Se houver vínculo autorizado, pode consultar dieta profissional atribuída, registrar refeições, acompanhar indicadores e manter seus dados pessoais. Apenas na gestão do nutricionista esse usuário será denominado **paciente**.
3. **O nutricionista é o assinante pagante.** O MVP terá **um único plano profissional de assinatura mensal**, com acesso ao conjunto de funcionalidades profissionais aprovadas: convites, gestão de vínculos, dados de usuários autorizados na visão **Meus pacientes**, publicação de dietas e metas profissionais, biblioteca e histórico de trabalho. A validação do CRN e as autorizações específicas continuam obrigatórias. **O preço e a existência ou não de limite de pacientes permanecem sem decisão.**
4. **Separar recursos pessoais e profissionais.** Uma conta de nutricionista pode continuar usando os recursos pessoais normais sem assinatura profissional; a aprovação do CRN é requisito distinto da assinatura.
5. **Dados do paciente não dependem da cobrança do nutricionista.** Diário, metas, histórico, medições e versões de dietas publicadas para o paciente não podem ser apagados, sequestrados ou indisponibilizados ao titular devido a cancelamento ou falha de pagamento do profissional.
6. **Pagamento não equivale a consentimento nem validação profissional.** A assinatura não dá acesso a dados de pacientes sem vínculo ativo e autorização, nem libera funções profissionais antes da aprovação do registro.
7. **Sem monetização adicional presumida:** anúncios, cobranças por paciente, funcionalidades pessoais pagas e comissão sobre consultas não foram aprovados para este MVP.

## 3. Experiência de uso aprovada

- **Usuário, com ou sem acompanhamento:** usa a mesma conta e não encontra checkout para criar conta, usar recursos pessoais, aceitar convite ou consultar plano que lhe foi publicado. O acompanhamento dentro da plataforma é gratuito; a interface pessoal não apresenta “paciente” como um perfil separado.
- **Nutricionista:** identifica com clareza que as funcionalidades profissionais são oferecidas por assinatura mensal, sem misturar pagamento e dados de pacientes. Detalhes da oferta e do fluxo de assinatura ainda serão decididos.
- **Simplicidade confirmada:** apresentar **uma oferta mensal profissional única** com todas as ferramentas profissionais já aprovadas. Não introduzir níveis comerciais diferentes, nem mostrar preços/limites fictícios antes da decisão.

## 4. Cancelamento voluntário pelo nutricionista (**confirmado no MVP**)

- O nutricionista poderá **cancelar a renovação automática** de sua assinatura mensal. O cancelamento **não interrompe imediatamente** as funções profissionais: a assinatura permanece com direito de uso **até a data/hora de término do período já pago**.
- A interface deve exibir de forma clara a **data/hora até quando o acesso profissional permanecerá habilitado**, sem cobrar outro ciclo após a solicitação de cancelamento. O estado comercial deve distinguir **renovação cancelada, período pago ainda vigente** de **período encerrado**.
- Durante o período já pago, as funções profissionais permanecem disponíveis conforme as mesmas condições de **CRN validado, vínculo e autorização do usuário**. O cancelamento não concede direitos extras nem dispensa as verificações existentes.
- **Após terminar o período pago, aplicar o acesso restrito da seção 5:** manter conta pessoal, área de assinatura e lista administrativa mínima de vínculos, mas **bloquear acesso a dados nutricionais, novos convites e ações profissionais de dietas/metas**. **Versões já publicadas e programadas continuam podendo entrar em vigor na data prevista**, mesmo depois do vencimento comercial, desde que o vínculo e a autorização do usuário permaneçam válidos, conforme seção 6. Não equivale a autorizar nova publicação ou alteração pelo profissional.
- **Não confundir assinatura com vínculo:** o pedido de cancelamento ou o fim do período pago **não encerra automaticamente os vínculos de acompanhamento**, não comunica falsamente que o usuário encerrou o vínculo e **não exclui dietas publicadas, diário, metas, histórico ou outras informações do usuário**. O titular permanece com acesso aos próprios dados. Isso é diferente do encerramento de vínculo solicitado pelo usuário, que revoga permissões e encerra a vigência da prescrição conforme [PACIENTES.md](./PACIENTES.md).
- **Sem efeitos retroativos ou mudanças silenciosas:** cancelar a renovação por si só não despublica uma dieta vigente, não altera metas efetivas do usuário nem cancela agendamentos futuros de prescrição; o que deve acontecer **quando o período pago terminar** será decidido explicitamente, preservando integridade e histórico.
- A etapa 9 especificará verificação confiável de período pago no servidor, referência temporal inequívoca, idempotência de cancelamentos, eventos de cobrança e processamento de webhooks. Não confiar apenas na interface ou numa data fornecida pelo cliente.

## 5. Acesso após fim do período pago (**confirmado no MVP**)

- **Acesso mantido:** própria conta pessoal e seus recursos gratuitos; área de assinatura/pagamentos para visualizar e regularizar o estado comercial; listagem administrativa **Meus pacientes** com **nome e estado do vínculo** dos usuários vinculados, sem dados nutricionais, indicadores, detalhes das dietas ou acesso a perfil completo do usuário.
- **Acesso profissional bloqueado:** leitura de diário alimentar, calorias, macronutrientes, peso/IMC, metas, indicadores, histórico nutricional e prescrições de usuários; edição/publicação/programação de dietas e metas profissionais; envio de convites e ativação de novos vínculos; uso da biblioteca profissional paga. Deve bloquear também as respectivas chamadas da API, inclusive em sessões já abertas, sem depender de controles do frontend.
- **Não confundir assinatura e vínculo:** o fim do período pago **não encerra automaticamente os vínculos existentes**, nem dispara e-mail afirmando que o usuário encerrou o acompanhamento. Conteúdo das dietas publicadas, registros, metas e históricos continuam preservados e acessíveis ao próprio usuário.
- **Após reativar a assinatura:** as capacidades profissionais só retornam quando **CRN estiver validado** e cada vínculo e autorização do usuário ainda forem válidos. Não reabrir autorização revogada, relação encerrada nem prescrição que perdeu sua vigência por término do vínculo.
- **Dietas futuras publicadas antes da expiração — aprovado:** a execução automática de um agendamento publicado anteriormente **não é uma nova ação do nutricionista** e pode ocorrer mesmo com assinatura vencida, desde que vínculo e autorização do usuário ainda estejam válidos. A regra detalhada encontra-se na seção 6; o profissional continua impedido de criar ou alterar prescrições sem assinatura.
- **Verificação no servidor:** período pago, autorização, vínculo, CRN e papel devem ser validados antes de permitir ações profissionais; testes negativos, idempotência e invalidação de acesso/caches serão detalhados na etapa 9.

## 6. Ativação de dietas previamente programadas após vencimento (**confirmada no MVP**)

- Uma versão de dieta profissional **publicada e programada quando o nutricionista tinha permissão profissional válida** pode **entrar em vigor automaticamente na data/hora estabelecida**, mesmo se a assinatura mensal do autor tiver expirado após a publicação.
- **Condição obrigatória no momento da ativação:** deve existir **o mesmo vínculo de acompanhamento ainda ativo e autorização vigente do usuário**. Se o usuário encerrou o acompanhamento, trocou de profissional ou revogou a autorização, o agendamento anterior não pode ativar; vale a regra da etapa 6, que cancela programações do vínculo encerrado.
- **Separação das operações:** ativação de publicação antiga, realizada pelo processo programado, não é publicação nova nem edição pelo nutricionista. Durante a assinatura expirada, **não permitir** novos convites, novas prescrições/metas, edição, reagendamento, cancelamento manual profissional ou acesso aos dados nutricionais, conforme seção 5.
- **Histórico e integridade:** preservar autor, conteúdo e horário originalmente programado, encerrar a versão anterior sem sobreposição e registrar a ativação real. O usuário pode continuar consultando suas dietas sem depender da assinatura do profissional. Não criar versões ou alterar retrospectivamente dados do diário.
- **Concorrência e falhas:** a etapa 9 deverá prever agendador **idempotente**, autorização e vínculo conferidos na transação que efetiva a ativação, proteção contra disputa com encerramento/troca e verificação da situação profissional segundo as regras de CRN definidas nas etapas 8/9. A expiração comercial isolada **não** é motivo para cancelar o agendamento.

## 7. Questões a decidir na etapa 7

- **Estrutura da oferta — confirmada:** somente **um plano profissional mensal** no MVP, incluindo todas as ferramentas profissionais previstas; não criar níveis de assinatura diferentes no lançamento.
- **Limite de pacientes — decisão adiada explicitamente:** avaliar mais adiante **se existirá limite** de usuários que um nutricionista poderá acompanhar, antes de determinar qualquer quantidade. **Plano único não equivale a pacientes ilimitados.** Outros limites e regras de uso justo também não foram decididos.
- **Preço — decisão adiada explicitamente:** definir posteriormente valor da mensalidade e condições comerciais, considerando custos e validação de mercado. Não fixar preço, moeda, desconto ou promoção por suposição.
- **Período de teste — ainda sem decisão:** avaliar separadamente se existirá experiência gratuita por prazo limitado.
- **Pagamento:** provedor, renovação recorrente, confirmação, comprovantes e comunicação de falhas.
- **Cancelamento e acesso após vencimento — decididos:** renovação para, acesso profissional continua até o fim do período pago. Depois, ficam conta pessoal, assinatura e lista administrativa mínima; acesso nutricional e ações profissionais ficam bloqueados, preservando vínculos e dados.
- **Versões futuras já publicadas — decidido:** a ativação automática na data agendada continua possível depois do vencimento da assinatura, **somente se vínculo e autorização do usuário continuarem válidos**, sem permitir novas ações profissionais.
- **Inadimplência/falha de pagamento — decisão separada:** definir aviso, tentativas de cobrança, eventual período de tolerância e efeitos de suspensão, sem aplicar automaticamente a regra de cancelamento voluntário.
- **Estados de assinatura:** definição do ciclo de vida comercial e mensagens adequadas para o nutricionista.

Regras técnicas de cobrança, idempotência, webhooks, segurança e integração serão detalhadas na etapa 9. Proteção de dados, retenção e responsabilidades na etapa 8.

## 8. Critérios de aceitação iniciais

| Cenário | Resultado esperado |
| --- | --- |
| Adulto sem nutricionista cria conta | Consegue usar diário, metas, gráficos, histórico e planos pessoais sem pagar |
| Usuário gratuito recebe convite válido de nutricionista | Aceita na mesma conta e acompanha dieta/indicadores sem checkout nem mudança para “conta de paciente” |
| Usuário encerra acompanhamento e depois recebe novo convite | Mantém a mesma conta gratuita, dados e recursos pessoais; apenas o vínculo profissional muda |
| Nutricionista deseja usar ferramentas profissionais | Há um único plano mensal incluindo as ferramentas profissionais definidas, sujeito também à validação do CRN |
| Nutricionista compara ofertas de assinatura no MVP | Plataforma oferece somente um plano profissional, sem níveis comerciais múltiplos |
| Limite de pacientes ainda não foi decidido | Produto não presume número máximo nem promete pacientes ilimitados |
| Preço ainda não foi definido | Documentação e produto não inventam valor de assinatura nem condições de cobrança |
| Nutricionista paga assinatura mas CRN ainda não foi validado | Pagamento não dispensa validação profissional nem concede acesso a dados de pacientes |
| Nutricionista tem assinatura mas não possui vínculo autorizado com um paciente | API bloqueia acesso aos dados desse paciente |
| Nutricionista cancela voluntariamente a assinatura durante período pago | Renovação automática é interrompida, mas funcionalidades profissionais seguem disponíveis até o fim do período já pago |
| Nutricionista consulta vencimento após cancelar | Interface informa data/hora de término do acesso pago, distinguindo cancelamento agendado de período encerrado |
| Período pago termina após cancelamento | Nutricionista mantém conta pessoal, área de assinatura e lista administrativa de vínculos existentes, sem acesso nutricional nem ações profissionais |
| Assinatura expirada, nutricionista abre Meus pacientes | Somente nomes e estados dos vínculos, sem acesso aos diários, indicadores, dietas ou dados privados |
| Assinatura expirada, nutricionista tenta consultar dados por API ou criar nova dieta | Servidor bloqueia requisição mesmo com CRN verificado e vínculo anteriormente autorizado |
| Assinatura expirada, nutricionista tenta enviar novo convite | Ação é negada, sem criar convite nem vínculo |
| Assinatura reativada e vínculo/autorização ainda válidos | Acesso profissional pode voltar após conferir assinatura, CRN e permissões, sem reviver vínculos encerrados |
| Usuário encerrou o vínculo durante a suspensão comercial | A reativação de assinatura não restaura a autorização revogada nem a dieta profissional anteriormente encerrada |
| Nutricionista cancela assinatura com usuários vinculados | Vínculos não são encerrados automaticamente, nem enviada mensagem falsa de fim de acompanhamento; dados e prescrições publicadas permanecem preservados |
| Nutricionista cancela enquanto existe dieta profissional programada | Cancelar renovação não cancela por si só o agendamento; pode ativar após vencimento se vínculo e autorização continuarem válidos |
| Versão foi publicada e programada antes do vencimento, vínculo segue autorizado | Ativação automática ocorre na data agendada mesmo com assinatura expirada, sem nova publicação |
| Usuário encerra ou troca vínculo antes da data de ativação com assinatura expirada | Versão futura do profissional anterior não ativa, agendamento é cancelado pelas regras de encerramento do vínculo |
| Nutricionista com assinatura vencida tenta reagendar versão anterior | Servidor rejeita alteração, ainda que a ativação automática original continue autorizada |
| Vencimento comercial e ativação agendada coincidem | Processo de ativação aplica data programada e vínculo/autorização vigentes, sem confundir vencimento da assinatura com revogação de vínculo |
| Usuário acessa conta depois que assinatura do nutricionista terminou | Próprios dados, histórico, diário e metas permanecem acessíveis sem custo |
| Pagamento falha sem solicitação de cancelamento | Regras de inadimplência serão definidas separadamente; não tratar automaticamente como cancelamento voluntário |
| Paciente encerra vínculo com nutricionista | Mantém conta e dados independentemente do estado comercial do profissional |

**Próxima decisão:** como tratar uma **falha no pagamento da renovação mensal**: período de tolerância com avisos e tentativa de nova cobrança ou suspensão no vencimento. Diferenciar falha na renovação de cancelamento voluntário; não liberar o primeiro acesso profissional sem confirmação de cobrança. **Preço e existência/quantidade de limite de pacientes permanecem expressamente adiados.**

**Observação:** este arquivo registra requisitos aprovados; não representa implementação, testes ou cobrança em produção.
