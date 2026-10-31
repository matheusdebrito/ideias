# Diário alimentar, metas e indicadores — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 5 de 10 — Diário alimentar, metas e indicadores (em andamento)
> **Versão:** 0.9
> **Data:** 31/10/2026
> **Fonte principal da visão:** [IDEA.md](./IDEA.md) e [DIETAS.md](./DIETAS.md)
> **Convenção:** regras confirmadas estão separadas das decisões em aberto.

## 1. Objetivo

Registrar o que a pessoa **realmente consumiu** e apresentar acompanhamento alimentar independente das prescrições e versões de dieta.

## 2. Decisões já confirmadas

- O diário alimentar é de titularidade do usuário/paciente e funciona **sem dieta ativa**.
- O registro de consumo não modifica uma dieta planejada e não depende de uma versão de plano continuar vigente.
- Existem três formas de lançar uma refeição:
  1. Copiar uma refeição da dieta vigente, ajustando alimentos e porções para refletir o que foi consumido.
  2. Copiar uma refeição da biblioteca pessoal salva, com ajustes opcionais.
  3. Registrar uma refeição avulsa.
- A ação **Registrar consumo** é separada da ação **Salvar refeição como modelo**. Lançamento avulso não cria modelo reutilizável automaticamente.
- Data/hora real do consumo são dados do diário; horário sugerido pelo plano não vira automaticamente horário efetivo.
- **Registro retroativo (confirmado no MVP):** o usuário pode lançar refeições consumidas em datas anteriores à data do cadastro, informando a data/hora reais do consumo, sem depender de dieta ativa.
- O formulário oferece a data e a hora atuais como valores iniciais para minimizar a digitação, mas ambos podem ser editados antes da confirmação.
- Preservar distintamente o instante do **consumo declarado** e o instante do **cadastro no sistema**; nenhum deve sobrescrever o outro.
- Nutrientes, indicadores e totais devem ser contabilizados no **dia do consumo**, inclusive quando o registro foi feito depois, recalculando os agregados dos períodos afetados. O consumo retroativo deve ser comparado com a meta efetiva naquela data, e não com a meta vigente no dia do cadastro.
- **Fuso horário por refeição (confirmado para o MVP):** sugerir automaticamente o fuso IANA configurado no navegador/dispositivo no momento de abrir o formulário; não usar GPS, localização física ou API externa.
- O usuário pode **corrigir manualmente o fuso** para a refeição (inclusive ao lançar consumo retroativo), caso a configuração do dispositivo seja incorreta ou o consumo tenha ocorrido em outro fuso. Se o navegador não fornecer fuso válido, solicitar escolha manual em vez de adivinhar.
- Preservar data/hora **locais de consumo**, fuso IANA escolhido, instante UTC equivalente (quando inequívoco) e instante de cadastro em campos conceitualmente separados. A data nutricional pertence ao calendário **local do consumo informado**; perto da meia-noite não deve ser determinada pelo UTC do cadastro.
- Em viagens ou alterações do fuso do dispositivo, novos registros usam o fuso atual, mas os anteriores conservam seu fuso e dia originais. Somente uma edição explícita autorizada altera data, horário ou fuso do registro antigo, com recálculo das datas afetadas e histórico de correção.
- **Etapa 9:** especificar formato de persistência e conversão, validação IANA e tratamento de instantes ambíguos/inexistentes durante transições de horário de verão; não deduzir fuso histórico a partir do aparelho atual.
- Cada refeição/ingrediente consumido preserva nome, quantidade, unidade, nutrientes calculados, origem e referências aplicáveis ao momento do registro (retrato histórico).
- Recalcular ou substituir os dados de uma dieta, modelo ou alimento não pode reescrever o consumo histórico.
- Usar o alimento no estado em que foi consumido; para preparações, a quantidade é normalmente pesada após o preparo.
- Os valores nutricionais são calculados sem arredondamento intermediário; a apresentação segue as regras de [ALIMENTOS.md](./ALIMENTOS.md).
- Repetir a mesma confirmação de registro por falha/reenvio de rede não deve criar consumo duplicado.
- O diário apresentará totais nutricionais por refeição e por dia e consulta a períodos históricos.
- **Edição e exclusão (confirmado para o MVP):** o dono do diário pode editar uma refeição já registrada (inclusive seus alimentos, quantidades, unidades e data/hora) ou excluí-la da visualização e dos indicadores de consumo.
- Uma edição não cria uma nova refeição duplicada; recalcula seus valores nutricionais com os dados corrigidos e recalcula os totais das datas afetadas. Se a data mudar, atualizar os indicadores tanto do dia anterior quanto do novo dia.
- A exclusão retira o registro dos totais e do diário corrente, mediante ação explícita, sem alterar a prescrição nem a biblioteca de modelos.
- Registrar um **histórico discreto de correções**, com tipo da ação, autor, data/hora e dados necessários para identificar o antes/depois, sem exigir justificativa para cada ajuste. Proteger essa trilha com autorização no servidor.
- O período, a visibilidade e o comportamento de retenção/eliminação desse histórico serão definidos nas etapas 8 e 9 em conformidade com o tratamento de dados pessoais; **não presumir retenção indefinida** nem possibilidade de restauração após exclusão. A ação de excluir no diário deve ser descrita com precisão ao usuário.
- Distinguir uma correção feita pelo dono do diário de atualizações posteriores de alimentos ou planos: apenas a primeira pode alterar explicitamente um registro de consumo existente.

## 3. Metas e indicadores — confirmado em princípio

- Usuários adultos poderão acompanhar metas calóricas, consumo de calorias e macronutrientes.
- O consumo diário será comparado com a meta válida **na data correspondente**; metas posteriores não reescrevem o histórico.
- **Indicadores diários confirmados para o MVP:** apresentar energia total consumida (kcal), meta calórica efetiva da data, diferença em kcal **restantes** enquanto o consumo não ultrapassar a meta, ou **acima da meta** quando ultrapassar; apresentar também o total diário consumido de proteínas, carboidratos e gorduras totais (g).
- Para uma data com meta positiva válida, usar a diferença entre a meta e o consumo sem arredondamento intermediário: **restantes = máximo(0, meta − consumo)** e **acima = máximo(0, consumo − meta)**. A interface privilegia o indicador contextual correto, em vez de exibir um saldo negativo como se fossem calorias restantes.
- Na ausência de uma meta válida naquele dia, exibir calorias e macros consumidos e informar **sem meta definida**; não calcular restante/excedente sobre zero nem utilizar meta atual para datas passadas.
- Atualizar cálculos e visualização após criar, editar ou excluir refeições, inclusive retroativas; agrupar pelo dia local do consumo e utilizar a meta efetiva daquela data, sem sobrescrever referências históricas.
- Totais obtidos de nutrientes não arredondados e valores só formatados na apresentação, com kcal geralmente inteiras ou com uma casa conforme o contexto e macronutrientes com uma casa decimal, respeitando [ALIMENTOS.md](./ALIMENTOS.md).
- **Ainda não definido:** metas individuais de proteína/carboidrato/gordura e visualizações de sua distribuição; resumos por semana e mês fazem parte do objetivo do MVP, mas suas fórmulas de saldo, médias, tolerâncias e visualização continuam em discussão.
- Medidas corporais (peso, altura e IMC, com limitações explicadas) integram a visão do produto; o detalhamento de edição e históricos será tratado nesta etapa.
- Metas e estimativas não são automaticamente prescrições clínicas.
- **Mudança de meta profissional pelo paciente (confirmado no MVP):** permitir que o paciente altere a meta efetiva de acompanhamento de calorias e/ou macronutrientes, mesmo quando ela foi definida pelo nutricionista. Antes de aplicar, exibir mensagem explícita de origem profissional e solicitar confirmação. Cancelamento não modifica a meta.
- A alteração confirmada fica marcada como **ajuste do paciente**, com identificação de autoria, data e início de vigência; os indicadores futuros usam essa meta ajustada. Não sobrescrever, editar ou reatribuir a prescrição profissional original: ela continua acessível para comparação e o plano profissional publicado permanece inalterado.
- Se houver uma meta profissional vigente e nenhuma alteração pessoal posterior, ela pode servir como referência principal; na ausência de meta profissional vigente, utilizar a meta pessoal. O histórico dos indicadores compara cada data à meta efetivamente aplicada àquele período, incluindo origem, sem alterações retroativas.
- **Visibilidade profissional confirmada:** com vínculo ativo e autorização vigente, o nutricionista terá no painel do paciente acesso à meta originalmente definida pelo profissional, à meta efetiva ajustada pelo paciente e à data/hora da mudança, identificando a autoria. O ajuste pessoal não substitui a prescrição no histórico profissional.
- Consultas e APIs deverão respeitar a autorização do paciente e o vínculo ativo. O fim do vínculo revoga a visualização corrente; eventual retenção legal de dados segue política ainda pendente das etapas 8/9.
- **Nova meta profissional, atualização automática (definido para o MVP):** ao entrar em vigor uma meta definida/publicada posteriormente pelo nutricionista, essa meta passa a ser a referência efetiva dos indicadores, inclusive quando a referência anterior era um ajuste pessoal do paciente. Não exigir aceite, clique ou confirmação do paciente.
- Se a nova meta tiver vigência futura, ela só substitui a referência atual no início programado; a data efetiva, a autoria profissional e os períodos anteriores devem ser preservados no histórico. Informar a atualização de forma discreta e não bloqueante, sem alterar valores históricos.
- Após uma atualização profissional, o paciente ainda poderá ajustar a referência por conta própria mediante confirmação, sem modificar a prescrição. Para os indicadores, a meta vigente é a do último evento efetivo de atualização profissional ou ajuste pessoal confirmado, observando seus instantes de vigência.
- **Aviso ao nutricionista (confirmado no MVP):** quando o paciente confirmar uma alteração na meta, o painel do profissional exibirá um **aviso visual discreto**, não bloqueante, com a meta original, a ajustada, autoria e data/hora, apenas durante vínculo ativo e autorização válida.
- Esse evento não gera e-mail, push nem mensagens externas no MVP; também não exige confirmação de leitura pelo nutricionista. Os detalhes visuais do indicador serão resolvidos na etapa de UX/implementação, sem transformar a alteração em alerta clínico ou sinal de descumprimento.
- O aviso deve ter acesso protegido no servidor e não expor dados de pacientes sem vínculo/autorização. Prescrição original e meta efetiva ajustada continuam separadas.

## 4. Decisões em aberto da etapa 5

1. **Metas:** ajuste pessoal com confirmação, aviso discreto no painel profissional e aplicação automática de novas metas do nutricionista definidos. E-mail/push externos por ajuste de meta ficam fora do MVP; detalhes visuais do indicador vão para a etapa de implementação.
2. **Períodos e fusos:** detectação do fuso do dispositivo, correção manual e preservação do fuso de cada refeição já definidas. Restam detalhes técnicos de horário de verão/conversões (etapa 9) e o cálculo/agrupamento de indicadores por semana e mês.
3. **Indicadores:** painel diário de calorias consumidas, meta, restante/excedente e totais de proteínas, carboidratos e gorduras já confirmado. Restam resumos semanais/mensais, eventuais metas de macros e critérios de cobertura de micronutrientes.
4. **Medidas corporais:** frequência, correção de registros e apresentação de peso/IMC ao longo do tempo.
5. **Retenção e permissões da trilha de correções:** quais registros de auditoria podem persistir e quais atores poderão visualizá-los, respeitando privacidade e eliminação de dados? Resolver nas etapas 8 e 9.

## 5. Fluxo de alteração de meta profissional (**Definido para o MVP**)

1. O paciente abre seus indicadores e escolhe **Alterar meta**.
2. Quando a meta vigente tiver origem profissional, exibir mensagem como: "Esta meta foi definida pelo seu nutricionista. Deseja definir uma meta diferente para o seu acompanhamento? A meta original será preservada."
3. Oferecer **Cancelar** e **Confirmar alteração**; não aplicar a mudança sem confirmação explícita.
4. Ao confirmar, salvar nova referência efetiva com origem **Ajustada pelo paciente** e início de vigência. Não atualizar nem republicar a prescrição do nutricionista.
5. Exibir de forma distinguível a meta usada nos indicadores e a meta profissional original, para evitar atribuir o ajuste ao nutricionista.

**Visibilidade confirmada:** o painel profissional do paciente mostrará a meta original, a meta ajustada, a data/hora e a autoria do ajuste apenas com vínculo ativo e permissão vigente. O registro de prescrição permanece independente.

**Precedência confirmada:** uma nova meta do nutricionista se torna automaticamente a referência dos indicadores no início de sua vigência, sem aceite do paciente. O usuário é informado discretamente e pode ajustar depois se desejar. Atualizações futuras não alteram os indicadores históricos.

**Aviso confirmado:** o nutricionista autorizado verá um indicador discreto no painel do paciente, sem exigir ação e sem disparar e-mail, push ou mensagem externa. A próxima decisão funcional será sobre data/hora e lançamentos retroativos no diário.

## 6. Registro retroativo de refeições (**Definido para o MVP**)

1. Ao adicionar uma refeição, sugerir data e horário atuais, permitindo alterá-los antes de confirmar.
2. Se o consumo ocorreu ontem ou anteriormente, o usuário escolhe a data e hora do consumo efetivo.
3. Ao salvar, registrar separadamente quando a refeição foi consumida e quando foi cadastrada.
4. Contabilizar os nutrientes no dia do consumo declarado e recalcular o total e os indicadores dos períodos afetados, comparando-os à meta que vigorava nessa data.
5. Não criar uma segunda refeição pelo reenvio da mesma confirmação. Alterações no registro continuam seguindo as regras de edição e exclusão já definidas.

**Regra de fuso aprovada:** predefinir o fuso detectado do dispositivo, permitir ajuste manual por refeição e preservar fuso e data/hora locais do consumo no histórico. O dia de consumo é definido pela data local da refeição no fuso selecionado, não pela data do salvamento ou pelo fuso atual em consulta.

**Detalhe técnico postergado:** resolver ambiguidades de horário de verão e serialização UTC/IANA na etapa 9.

## 7. Fluxo de correção do diário (**Definido para o MVP**)

1. O usuário seleciona uma refeição registrada e escolhe **Editar** ou **Excluir**.
2. Ao editar, o formulário apresenta os dados atuais; a pessoa pode alterar alimentos/porções e demais dados de consumo real sem alterar o plano ou modelos.
3. Ao salvar, validar alimentos e unidades, recalcular nutrientes e agregados, registrar a correção com autor e instante, e atualizar a apresentação.
4. Ao excluir, retirar a refeição dos resultados e totais do diário. Manter somente os dados de auditoria necessários enquanto a política de retenção futura permitir.
5. Não exigir justificativa para editar ou excluir. O acesso ao histórico de correções e suas regras de guarda/eliminação não estão decididos.

**Critério de UX:** permitir corrigir 200 g de arroz para 150 g dentro do mesmo registro, sem forçar exclusão e recadastro. As correções são explícitas; alterações futuras de catálogo, alimento ou dieta não são correções do diário.

## 8. Painel diário de acompanhamento (**Definido para o MVP**)

Mostrar calorias consumidas, meta calórica efetiva da data e diferença contextual (restante ou excedente), além de proteínas, carboidratos e gorduras totais consumidos. Dados vêm do diário, não da dieta planejada; meta é a referência vigente para o dia analisado. Se não houver meta, não inventar um saldo.

O painel recalcula automaticamente após salvar, editar ou excluir refeições, inclusive quando a alteração muda o dia do consumo. A visualização não deve tratar o excesso como diagnóstico nem penalizar o usuário.

## 9. Casos de aceitação de referência

| Cenário | Resultado esperado |
| --- | --- |
| Usuário sem dieta registra refeição avulsa | Consumo entra no diário normalmente |
| Meta diária é 2.000 kcal e consumo registrado é 1.500 kcal | Exibir 1.500 kcal consumidas e 500 kcal restantes |
| Meta diária é 2.000 kcal e consumo registrado é 2.200 kcal | Exibir 2.200 kcal consumidas e 200 kcal acima da meta, sem saldo restante negativo |
| Meta diária é 2.000 kcal e consumo registrado é 2.000 kcal | Exibir meta atingida e diferença igual a zero, sem excedente |
| Não há meta calórica efetiva na data consultada | Exibir consumo e macros, sinalizar ausência de meta e não calcular saldo com meta zero |
| Usuário edita ou exclui uma refeição já registrada | Atualizar calorias, proteínas, carboidratos, gorduras e restante/excedente da data afetada |
| Dia anterior recebe lançamento retroativo | Recalcular indicadores do dia do consumo e comparar com meta vigente naquele dia |
| Usuário registra refeições com quantidades fracionárias | Somar nutrientes sem arredondamento intermediário; formatar apenas na interface |
| Usuário abre o cadastro para registrar o almoço de agora | Formulário sugere data e hora atuais, que podem ser editadas |
| Usuário registra hoje o jantar consumido ontem | Refeição entra no dia de ontem e atualiza os totais daquele dia, sem somar consumo extra no dia de hoje |
| Usuário lança uma refeição de semana anterior | Resumos da semana e do mês correspondentes são recalculados usando a data do consumo |
| Meta mudou entre o dia do consumo e o dia do lançamento | O consumo retroativo é comparado com a meta que vigorava no dia informado, preservando o histórico de metas |
| Consulta distingue consumo do instante de cadastro | Registro preserva os dois instantes para rastreabilidade |
| Usuário abre formulário no Brasil com fuso America/Sao_Paulo configurado | Fuso é sugerido automaticamente sem solicitar localização/GPS |
| Usuário lança hoje uma refeição feita em Lisboa em outra data | Pode selecionar Europe/Lisbon para o consumo retroativo, independentemente do fuso atual do aparelho |
| Pessoa viaja e o dispositivo muda de fuso | Novos registros usam o fuso detectado naquele momento; históricos não mudam de dia |
| Consumo ocorre perto da meia-noite local | Totalização usa a data local de consumo e o fuso selecionado, não a data UTC do cadastro |
| Dispositivo não fornece um fuso válido | Aplicação oferece seleção manual em vez de supor uma localização |
| Usuário corrige o fuso de uma refeição antiga | Salva correção explícita e recalcula dias envolvidos, preservando trilha autorizada |
| Usuário registra alimento diferente do prescrito | Diário reflete o consumo real; dieta permanece igual |
| Dieta é atualizada depois do consumo | Valores do diário já salvo permanecem os mesmos |
| Modelo pessoal é editado após o consumo | Registro antigo permanece independente |
| Duplo envio da mesma confirmação | Apenas uma refeição é registrada |
| Micronutriente não está disponível em alguns itens | Total do micronutriente é explicitamente sinalizado como incompleto |
| Usuário altera sua meta futura | Meta anterior permanece associada aos dias históricos em que vigorou |
| Paciente pede alteração de meta definida pelo nutricionista | Aplicação avisa que a meta é profissional e solicita confirmação expressa |
| Paciente cancela a confirmação da alteração | Meta efetiva permanece intacta |
| Paciente confirma uma meta diferente | Meta ajustada pelo paciente entra em vigor nos indicadores, com autoria explícita; meta profissional original não muda |
| Nutricionista consulta a prescrição publicada após ajuste do paciente | Valores da prescrição original permanecem imutáveis e corretamente atribuídos ao autor |
| Nutricionista com vínculo ativo e autorização consulta o painel após ajuste | Vê meta original prescrita, meta de acompanhamento ajustada, data/hora e autoria da alteração |
| Nutricionista sem vínculo ou autorização tenta consultar metas do paciente via API | Acesso negado pelo servidor; dados pessoais não são expostos |
| Paciente modifica meta de acompanhamento | O nutricionista com vínculo ativo vê aviso discreto no painel, com autoria, valores e data/hora; sem e-mail, push nem mensagem externa |
| Profissional acessa paciente sem vínculo ativo ou autorização | Não vê aviso nem detalhes da mudança de meta |
| Profissional consulta o painel após ajuste do paciente | Visualiza a alteração sem confirmação obrigatória de leitura |
| Paciente consulta indicadores de dias anteriores ao ajuste | Utiliza a meta efetiva vigente na época, sem reescrever histórico |
| Meta profissional de 2.200 kcal foi ajustada pelo paciente para 2.000 kcal; nutricionista publica nova meta de 2.100 kcal de início imediato | A referência dos indicadores muda automaticamente para 2.100 kcal, sem pedir confirmação ao paciente |
| Nutricionista agenda uma nova meta para data futura | A meta anterior continua efetiva até a data/hora programada; depois, a nova entra automaticamente |
| Paciente deseja alterar a nova meta profissional depois da atualização | A aplicação solicita confirmação do ajuste pessoal e preserva a meta profissional publicada |
| Paciente consulta um período anterior à nova meta | Utiliza metas efetivas daquela época, sem reescrever indicadores históricos |
| Usuário corrige porção de arroz de 200 g para 150 g | O registro é atualizado, nutrientes e totais são recalculados e ação é registrada discretamente |
| Usuário edita data de uma refeição | Totais e indicadores são atualizados nas duas datas afetadas |
| Usuário exclui refeição registrada por engano | Ela deixa de aparecer e de contar nos totais; trilha de ação respeita futura política de retenção |
| Usuário edita refeição já registrada | Prescrição e modelos que originaram o registro permanecem intactos |
| Sistema atualiza composição nutricional de alimento do catálogo | Não altera registros de consumo já salvos automaticamente |

---

A etapa 5 continua em planejamento. **Edição/exclusão de refeições, registros retroativos, fuso com correção manual, histórico discreto, ajustes confirmados de metas, aviso no painel profissional, aplicação automática de novas metas profissionais e indicadores diários de kcal/meta/restante/excedente e macronutrientes consumidos foram aprovados para o MVP.** Permanecem pendentes indicadores semanais/mensais, metas específicas de macros, detalhes técnicos de horário de verão, retenção e acesso à trilha.
