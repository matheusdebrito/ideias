# Diário alimentar, metas e indicadores — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 5 de 10 — Diário alimentar, metas e indicadores (em andamento)
> **Versão:** 1.8
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
- **Resumo semanal e mensal (confirmado para o MVP):** apresentar o **total de calorias e de proteínas/carboidratos/gorduras registrados**, a **média diária de cada um desses nutrientes considerando somente dias com pelo menos uma refeição** e a **quantidade de dias com registros** dentro do período selecionado.
- Usar a **data local do consumo** associada a cada registro e contar **datas distintas** com consumo registrado no intervalo. O total registrado é a soma dos nutrientes das refeições desses dias; a média diária de cada nutriente é **total do período ÷ dias com registros**. Calcular com valores não arredondados e formatar somente na apresentação.
- **Dias sem registros são dados ausentes, não consumo zero**: não entram no divisor e não devem ser representados como dias de jejum. Dias com alguma refeição registrada também podem estar incompletos, portanto rotular as médias como **média por dia com registros**, sem afirmar cobertura alimentar completa.
- Se o período não tiver qualquer refeição, exibir **sem dados registrados**, incluindo o contador de dias igual a zero e sem calcular média numérica.
- Criar, editar, mover de data/fuso ou excluir registros deve recalcular totais, média e dias registrados de todos os períodos afetados, inclusive após lançamento retroativo.
- **Metas opcionais de macronutrientes (confirmado no MVP):** o titular adulto do diário pode definir e alterar metas pessoais de proteína (g), carboidratos (g), gordura total (g) e calorias (kcal) **sem nutricionista**, inclusive no uso pessoal gratuito. Não exigir plano ativo, vínculo ou pagamento profissional.
- Cada meta é opcional e independente: não há obrigação de preencher todos os nutrientes. Se um nutriente não tiver meta válida, mostrar somente seu consumo registrado, sem calcular restante/excedente ou presumir meta zero.
- Para nutrientes com metas efetivas válidas na data, mostrar consumo e diferença **restante ou acima da meta**, sem saldos negativos rotulados como restante. Usar dados registrados no diário, com soma sem arredondamento intermediário e apresentação em g (macros) ou kcal (energia).
- A origem da meta fica explícita: pessoal, profissional ou ajuste do paciente. Metas pessoais podem ser criadas e editadas sem profissional; ao ajustar meta de origem profissional, manter o aviso/confirmação aprovado e a prescrição intacta. Uma nova meta profissional vigente prevalece automaticamente, conforme regras já aprovadas.
- Preservar autoria e vigência de cada alteração, comparando datas históricas com suas metas efetivas. Não forçar equivalência energética exata entre kcal e macros nem gerar automaticamente valores de macros que o usuário não definiu.
- **Gráficos semanais e mensais (confirmado no MVP):** mostrar, por **dia local de consumo**, o total de calorias registradas (kcal), permitindo alternar para proteínas, carboidratos e gorduras (g), sem misturar unidades na mesma escala.
- Para cada dia/nutriente com meta válida, representar a **meta efetivamente vigente naquele dia** como referência. Metas pessoais e profissionais respeitam as regras de vigência e autoria; mudanças de meta não transformam uma meta atual em meta histórica de dias anteriores. Sem meta válida, não desenhar referência numérica presumida.
- Um dia sem refeição registrada deve aparecer como **sem dados**, não como ponto/barras de valor zero nem como uma linha interpolada de consumo real. Um dia com registros pode estar incompleto; identificar os valores como **consumo registrado**.
- Garantir alternativa textual consultável por data, com valores de consumo e referência quando presente, além de navegação utilizável em telas pequenas. Gráficos devem atualizar automaticamente após criar, corrigir ou excluir consumo, inclusive lançamentos retroativos e mudança de data/fuso.
- **Contadores de dias por relação à meta (confirmado para o MVP):** no resumo semanal/mensal, para o **nutriente selecionado no gráfico**, exibir quantos dias com consumo registrado e meta válida ficaram **abaixo**, **acima** ou **na meta**. O gráfico já mostra os valores diários e a referência correspondente, de modo que a contagem complementa a visualização sem exigir saldo acumulado do período.
- Calcular separadamente para calorias, proteínas, carboidratos e gorduras; classificar usando o **consumo registrado e a meta histórica efetiva daquela data**, considerando eventuais trocas pessoais/profissionais de meta.
- Dias **sem qualquer refeição registrada** permanecem **sem dados** e não contam como abaixo/na meta/acima. Dias com refeições, mas **sem meta válida para o nutriente**, permanecem **sem meta** e também não entram nas três contagens. Exibir, se necessário, essas quantidades para que a soma dos contadores seja interpretável.
- Dias com registros **podem estar incompletos**: descrever os estados como comparação entre **consumo registrado** e meta, não como constatação de cumprimento clínico/adesão. Uma refeição parcial pode gerar uma classificação abaixo da meta sem representar consumo total do dia.
- **Comparação estrita com a meta (confirmada para o MVP):** para qualquer nutriente selecionado, **abaixo** quando `consumoRegistrado < metaEfetiva`, **na meta** apenas quando `consumoRegistrado === metaEfetiva` em representação decimal consistente e **acima** quando `consumoRegistrado > metaEfetiva`. Não aplicar margens como ±5% ou ±100 kcal, nem transformar estar próximo da meta em meta atingida.
- Usar comparação numérica decimal com precisão definida no contrato de dados, evitando divergências artificiais por ponto flutuante. Não arredondar valores intermediários antes da classificação; exibir detalhe da diferença com precisão suficiente para explicar quando valores formatados aparentam igualdade, mas não são iguais.
- A meta alimentar é referência de acompanhamento; excedê-la não equivale necessariamente a superávit energético em relação ao gasto corporal. Pesagem melhora o controle de quantidades, mas composição dos alimentos e energia realmente absorvida continuam estimativas. As categorias descrevem **consumo registrado**, não avaliação clínica.
- O modelo gráfico exato permanece em aberto. **A semana foi definida de segunda-feira a domingo**; a contagem de dias com registros segue a data local do consumo. A diferença acumulada entre consumo registrado e metas dos dias comparáveis foi aprovada, sem supor que o diário represente toda a alimentação do usuário.
- **Registro de peso (confirmado no MVP):** permitir que o usuário adulto registre seu peso **quando quiser**, com data da medição e valor em kg, sem exigir frequência ou metas obrigatórias. Exibir a evolução em **gráfico cronológico**, preservando todas as medições distintas.
- **Correções (confirmado):** permitir editar a data ou o valor de uma medição já registrada; a correção atualiza a série temporal sem apagar ou sobrescrever outras medições. Política de exclusão, retenção e acesso ao histórico de correção pertence às etapas 8 e 9.
- **IMC atual (confirmado):** calcular com o **peso mais recente registrado** e a **altura válida do perfil**, por `IMC = pesoKg / (alturaMetros * alturaMetros)`. Sem peso ou altura válidos, exibir **IMC indisponível**, sem inventar valores. O MVP atende exclusivamente adultos (18+).
- **Apresentação responsável:** o IMC é apenas um indicador geral e **não avalia individualmente composição corporal nem define diagnóstico**; evitar alertas automáticos ou julgamentos baseados somente nele. Dados de peso são do usuário e o acesso profissional é condicionado às autorizações já previstas.
- **Pendente para arquitetura:** como apresentar IMC associado a medições antigas quando a altura cadastrada mudar; nunca reescrever os valores originais de peso nem simular alturas históricas não registradas.
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
2. **Períodos e fusos:** fuso por refeição, correção manual e agrupamento dos registros por data local já definidos; **semana de segunda-feira a domingo** e contagem de dias e média dos períodos semanal/mensal confirmados. Horário de verão/conversões continuam para a etapa 9.
3. **Indicadores:** painel diário, resumos, gráficos, contagens e diferença acumulada de kcal/macros confirmados, com comparação estrita e semana de segunda a domingo. Vitaminas e minerais disponíveis serão exibidos, com **total parcial identificado quando faltar dado em algum item**. Restam apresentação detalhada da cobertura e eventual uso de referências de micronutrientes.
4. **Medidas corporais:** registro livre de peso com data, correção de medições, gráfico histórico e IMC atual pelo peso mais recente/altura do perfil foram aprovados. Restam regras técnicas de altura histórica e retenção de medições (etapas 8 e 9).
5. **Retenção e permissões da trilha de correções:** quais registros de auditoria podem persistir e quais atores poderão visualizá-los, respeitando privacidade e eliminação de dados? Resolver nas etapas 8 e 9.

## 5. Fluxo de alteração de meta profissional (**Definido para o MVP**)

1. O paciente abre seus indicadores e escolhe **Alterar meta**.
2. Quando a meta vigente tiver origem profissional, exibir mensagem como: "Esta meta foi definida pelo seu nutricionista. Deseja definir uma meta diferente para o seu acompanhamento? A meta original será preservada."
3. Oferecer **Cancelar** e **Confirmar alteração**; não aplicar a mudança sem confirmação explícita.
4. Ao confirmar, salvar nova referência efetiva com origem **Ajustada pelo paciente** e início de vigência. Não atualizar nem republicar a prescrição do nutricionista.
5. Exibir de forma distinguível a meta usada nos indicadores e a meta profissional original, para evitar atribuir o ajuste ao nutricionista.

**Visibilidade confirmada:** o painel profissional do paciente mostrará a meta original, a meta ajustada, a data/hora e a autoria do ajuste apenas com vínculo ativo e permissão vigente. O registro de prescrição permanece independente.

**Precedência confirmada:** uma nova meta do nutricionista se torna automaticamente a referência dos indicadores no início de sua vigência, sem aceite do paciente. O usuário é informado discretamente e pode ajustar depois se desejar. Atualizações futuras não alteram os indicadores históricos.

**Aviso confirmado:** o nutricionista autorizado verá um indicador discreto no painel do paciente, sem exigir ação e sem disparar e-mail, push ou mensagem externa. Lançamentos retroativos e fusos do diário também já foram definidos nesta etapa.

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

## 9. Resumos semanais e mensais (**Definidos para o MVP**)

**Calendário semanal confirmado:** iniciar na **segunda-feira** e encerrar no **domingo**. Agrupar pelo dia local de consumo (não pelo instante UTC de cadastro), inclusive em viradas de mês/ano. A semana atual não presume refeições para dias ainda não transcorridos e lança retroativamente na semana do consumo.

O período mostrará totais **registrados** de energia e macronutrientes, médias **por dia com registros** desses mesmos valores e número de dias com registros. Um dia conta uma única vez quando possui uma ou mais refeições. As médias usam somente essas datas, sem representar dias vazios como consumo zero e sem presumir que dias com registros tenham todo o consumo informado.

**Fórmula:** para cada nutriente, `totalPeriodo = soma(nutrientesRegistrados)`; `diasComRegistro = quantidade de datas locais distintas com refeições no período`; `mediaPorDiaComRegistro = totalPeriodo / diasComRegistro`, apenas se `diasComRegistro > 0`. Com zero dias, indicar ausência de dados sem média numérica.

Atualizar esses resultados após inclusão, exclusão ou correção de registros, inclusive lançamentos retroativos e mudanças da data/fuso. Sem arredondamento intermediário.

## 10. Acompanhamento de peso e IMC (**Definido para o MVP**)

- **Peso:** registrar livremente em kg, com a data em que a medição foi feita. Não exigir pesagem diária ou semanal. Apresentar gráfico de evolução com registros ordenados pela data de medição, sem confundir data de registro com data da aferição.
- **Correção:** o dono da informação pode editar uma medição histórica e atualizar o gráfico, preservando os outros registros. Regras de retenção, trilha de correção e tratamento de exclusões serão analisadas na etapa de privacidade.
- **IMC atual:** usar a última medição de peso disponível, a altura atualmente informada no perfil e a fórmula peso (kg) dividido pela altura (m) ao quadrado. Validar peso e altura positivos e informar quando algum dado não estiver disponível.
- **Limites:** a interpretação do IMC é geral e não equivale a avaliação da composição corporal ou diagnóstico individual. Não deduzir IMC histórico automaticamente de uma altura atual como se fosse uma altura medida no passado; definir abordagem na etapa 9.

## 11. Configuração de metas pessoais e profissionais (**Definida para o MVP**)

1. O usuário adulto pode definir, no modo pessoal gratuito e **sem profissional vinculado**, meta de calorias e qualquer combinação de metas de proteínas, carboidratos e gorduras; nenhum desses valores é obrigatório.
2. Valores de meta são consultados junto dos valores de **consumo real** registrados, no dia efetivo de vigência; não gerar diferença para nutrientes sem meta.
3. Editar uma meta pessoal cria uma referência histórica a partir da nova vigência sem reescrever dias anteriores. Metas pessoais não são identificadas como prescrição profissional.
4. Se uma referência estiver definida pelo nutricionista, o paciente recebe o aviso e confirma antes de substituí-la em seus indicadores; a meta original fica preservada, visível ao profissional autorizado.
5. Uma nova meta profissional publicada passa a valer automaticamente na vigência definida, sem tarefa adicional ao paciente. O paciente conserva a possibilidade de ajustá-la depois.

## 12. Gráficos de consumo semanal e mensal (**Definidos para o MVP**)

- O gráfico apresenta **consumo registrado por dia** no período selecionado; padrão em **kcal**, com troca de indicador para proteína, carboidratos ou gorduras (g).
- A meta, quando existente, é referência para o mesmo nutriente na **data em que valia**, inclusive se ocorreram mudanças profissionais ou pessoais dentro do período. A referência fica ausente para datas sem meta válida.
- **Dia sem registros não pode aparecer como consumo zero**: sinalizar ausência de dados e não interpolar valores fictícios. Um dia com consumo lançado não é presumido completo.
- Atualizar a série ao editar/adicionar/excluir refeições ou alterar data/fuso, inclusive retroativamente. A visualização precisa oferecer alternativa textual acessível dos valores por data e se adaptar ao celular.

**Contagem por meta confirmada:** em cada visualização de calorias ou macronutriente, a interface contabilizará dias com consumo registrado e meta válida **abaixo**, **na meta** e **acima** da referência histórica daquele dia. Dias sem consumo registrado não serão classificados; dias com consumo mas sem meta ficam fora da contagem. A classificação refere-se somente aos **dados lançados**, não à alimentação inteira.

**Regra de igualdade estrita aprovada:** em cada dia comparável, `consumo < meta` é **abaixo**, `consumo = meta` é **na meta**, `consumo > meta` é **acima**, sem faixa de tolerância. Por exemplo, meta de 2.000 kcal e 2.100 kcal registrados contam **acima** em cada um dos sete dias, totalizando 700 kcal acima da **soma das metas**, mas isso não demonstra necessariamente superávit em relação ao gasto energético. Não comparar valores que foram arredondados apenas para exibição.

**Período semanal confirmado:** segunda-feira a domingo, organizado pela data local de consumo de cada refeição; semanas em curso podem ter dias futuros ainda sem lançamentos, que não são consumo zero. **Diferença acumulada confirmada:** por nutriente, usar somente dias com consumo registrado e meta vigente; mostrar o número de dias comparáveis e diferenciar ausência de registros e de meta. **Pendências:** tipo gráfico definitivo e critérios de apresentação da cobertura nutricional.

## 13. Diferença acumulada em períodos (**Definida para o MVP**)

Para cada semana (segunda a domingo) ou mês, e separadamente para calorias, proteínas, carboidratos e gorduras, usar somente os dias com **refeições registradas e meta válida para o nutriente**. Calcular `diferencaAcumulada = soma(consumoRegistradoDia - metaEfetivaDia)` sobre os dias comparáveis, preservando a meta válida em cada data e a precisão numérica dos cálculos.

- Resultado positivo: valor registrado **acima** da soma das metas nos dias comparáveis. Resultado negativo: **abaixo** dessa soma. Resultado zero: diferença agregada zero, **não** implica que todos os dias estiveram na meta; dias acima e abaixo podem se compensar.
- Exibir os **dias comparáveis**, dias **sem registros** e dias **com registros mas sem meta**. Sem dias comparáveis, apresentar **comparação indisponível**, sem diferença numérica.
- O resultado reflete somente o que foi **registrado**; até mesmo um dia com registros pode ter refeições faltantes. Não interpretar o saldo frente à meta como balanço energético fisiológico nem como diagnóstico.
- Atualizar a diferença, os dias comparáveis e os contadores acima/abaixo/na meta sempre que uma refeição for criada, editada, movida para outra data/fuso ou excluída, inclusive retroativamente. Não arredondar valores antes do cálculo.

## 14. Vitaminas, minerais e dados incompletos (**Definidos para o MVP**)

- Nos detalhes do diário e em resumos nutricionais, apresentar vitaminas e minerais que tenham dados conhecidos nos alimentos consumidos, como ferro, cálcio, vitamina C e vitamina B12, respeitando os nutrientes de fato disponibilizados pela fonte. Os dados de micronutrientes permanecem opcionais no cadastro de alimentos, conforme [ALIMENTOS.md](./ALIMENTOS.md).
- **Total com cobertura completa nos itens registrados:** quando todos os itens consumidos possuírem valor conhecido para o micronutriente selecionado, somar as contribuições ajustadas às quantidades informadas e apresentar o **total do consumo registrado**, sem afirmar que o diário contém toda a alimentação do usuário.
- **Total parcial:** se pelo menos um item tiver valor conhecido e outro não possuir o micronutriente, permitir mostrar a **soma dos dados conhecidos**, mas com destaque **parcial / dados incompletos**; não substituir o desconhecido por zero nem apresentar o resultado parcial como total nutricional exato.
- **Sem dados:** quando nenhum item consumido tiver informação sobre determinado micronutriente, exibir **sem dados disponíveis**, sem mostrar 0 mg/µg. Um zero fornecido explicitamente pela fonte deve continuar sendo um zero conhecido.
- Respeitar as unidades apropriadas (mg, µg etc.) e precisão dos dados-fonte, sem arredondamentos intermediários nem somas incompatíveis. Criar, editar, excluir ou registrar refeição retroativa deve atualizar os valores e a cobertura das datas e períodos afetados.
- O diário não conclui automaticamente deficiência, excesso ou adequação clínica a partir desses dados, nem compara vitaminas/minerais a metas genéricas não aprovadas.

**Em aberto:** modelo de interface para detalhar quais informações faltam e eventual uso de valores de referência para micronutrientes, sem presumir metas individuais.

## 15. Casos de aceitação de referência

| Cenário | Resultado esperado |
| --- | --- |
| Usuário sem dieta registra refeição avulsa | Consumo entra no diário normalmente |
| Usuário abre o resumo semanal | Gráfico exibe o consumo diário de calorias, com opção para visualizar proteínas, carboidratos e gorduras |
| Meta calórica 2.000 kcal e consumo 2.100 kcal em cada um dos sete dias registrados | Mostrar diferença acumulada +700 kcal sobre as metas e sete dias acima da meta, sem inferir superávit em relação ao gasto corporal |
| Três dias comparáveis têm diferenças +100, -40 e -60 kcal | Diferença acumulada 0 kcal, mas contadores de dias mostram 1 acima e 2 abaixo; não declarar três dias na meta |
| Semana tem 4 dias com registros/metas, 2 sem registros e 1 com registro mas sem meta | Diferença usa somente quatro dias comparáveis, enquanto apresenta 2 sem registros e 1 sem meta separadamente |
| Período não tem dias comparáveis | Mostrar comparação indisponível, não diferença zero |
| O usuário troca o gráfico de kcal para proteína | Diferença acumulada recalculada em gramas, usando apenas dias com meta de proteína |
| Uma refeição retroativa altera o total de um dia já registrado | Atualizar a diferença acumulada e os contadores dos períodos afetados |
| Semana contém 5 dias com calorias registradas e meta válida, 1 dia sem dados e 1 dia com registro mas sem meta | Contagens acima/abaixo/na meta somam 5, com 1 dia sem dados e 1 sem meta identificados separadamente |
| Usuário abre o resumo semanal em uma quarta-feira | Mostrar o intervalo completo de segunda a domingo, com dias futuros sem dados e sem classificações fictícias |
| Uma semana atravessa a virada de mês ou ano | Manter o mesmo período contínuo de segunda-feira a domingo, sem dividir em dois |
| Usuário registra no sábado uma refeição consumida na segunda-feira anterior | Atualizar a segunda-feira e os resumos da semana do consumo, não a semana do cadastro |
| Dia com registro parcial tem consumo abaixo da meta | Pode entrar como abaixo no comparativo de dados registrados, sem afirmar consumo insuficiente nem adesão à prescrição |
| Paciente altera a meta durante o mês | Contagem de cada dia considera o consumo registrado e a meta efetiva naquele dia |
| Usuário alterna gráfico de calorias para proteínas | Recalcular as contagens para a meta de proteínas, não reutilizar as classificações calóricas |
| Meta diária de 2.000 kcal e consumo registrado de 2.000 kcal | Classificar o dia como na meta |
| Meta diária de 2.000 kcal e consumo registrado de 2.001 kcal | Classificar como acima, mesmo sendo diferença pequena; sem tolerância |
| Meta diária de 2.000 kcal e consumo registrado de 1.999 kcal | Classificar como abaixo, mesmo sendo diferença pequena; sem tolerância |
| Meta diária de 2.000 kcal e consumo registrado de 2.100 kcal em sete dias | Sete dias acima da meta, e diferença somada de 700 kcal sobre as metas; não apresentar isso como prova de superávit energético sobre o gasto |
| Arredondamento visual mostra consumo e meta iguais, mas valores internos diferem | Não classificar erroneamente como na meta; permitir visualizar diferença com precisão apropriada |
| Há meta de proteína apenas em parte do mês | Gráfico de proteína mostra referência somente nos dias em que houve meta válida |
| Nutricionista altera meta de 2.000 para 2.100 kcal no meio da semana | Referência do gráfico acompanha os valores históricos corretos por data; não reescreve dias anteriores |
| Um dos sete dias da semana não tem nenhuma refeição registrada | Marcar o dia como sem dados, sem ponto de consumo zero ou interpolação |
| Usuário lança ou corrige refeição de período anterior | Gráfico semanal/mensal correspondente reflete os nutrientes e a data local corrigidos |
| Usuário consulta gráficos em celular ou sem recurso visual | Pode consultar as datas e seus valores por alternativa textual acessível |
| Usuário gratuito sem nutricionista define 120 g de proteína e 2.000 kcal | Salvar as metas pessoais normalmente, sem exigir vinculação profissional |
| Usuário decide definir somente 100 g de proteínas | Mostrar objetivo de proteína, mas apenas consumo registrado para kcal, carboidratos e gorduras, sem inventar outras metas |
| Usuário ainda não definiu meta de gordura | Exibir gordura consumida em gramas sem valor restante/excedente |
| Usuário sem nutricionista edita sua própria meta | Registrar nova vigência e preservar meta anterior no histórico sem exigir aviso de alteração profissional |
| Profissional define macros para paciente e paciente solicita ajuste | Mostrar origem profissional e pedir confirmação antes do ajuste, mantendo a prescrição original |
| Nutricionista publica nova meta de macro depois de ajuste pessoal | Aplicar a nova meta automaticamente a partir da vigência, com histórico preservado |
| Usuário registra 72 kg medidos em data anterior | Gráfico de peso inclui o ponto na data da medição, sem tratá-lo como pesagem do dia do cadastro |
| Usuário registra várias medições ao longo de semanas | Gráfico mostra a evolução das medições, sem exigir frequência mínima |
| Usuário corrige uma medição antiga de 75 para 74 kg | Corrige apenas a medição escolhida, preserva os demais pontos e atualiza o gráfico |
| Usuário tem 72 kg como peso mais recente e 1,80 m de altura válida no perfil | Exibir IMC atual aproximado de 22,2, contextualizado como indicador geral |
| Usuário não informou altura ou não possui peso registrado | Indicar IMC indisponível, sem mostrar zero ou inferir valores |
| Usuário modifica altura do perfil | Manter medições históricas de peso; regra dos IMCs históricos permanece para arquitetura |
| Semana contém 7 dias e apenas 3 têm refeições registradas, somando 4.500 kcal | Exibir 3 dias com registros, total registrado 4.500 kcal, média 1.500 kcal por dia registrado; não dividir por 7 |
| Um dia tem várias refeições registradas | Contar esse dia uma única vez no divisor, somando todas as refeições |
| Período não possui refeições registradas | Exibir sem dados registrados e nenhum valor de média, sem assumir ingestão zero |
| Dia contém somente uma refeição lançada | Contabilizar o dia entre os registrados, mas não afirmar que a alimentação do dia está completa |
| Uma refeição de ontem é excluída e não há outros registros naquele dia | Recalcular a média e diminuir a quantidade de dias registrados, se aplicável |
| Usuário cadastra retroativamente refeição de outro mês | Atualizar totais, médias e dias com registros do mês de consumo |
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
| Refeição contém alimento com ferro conhecido e outro sem dado de ferro | Exibir apenas a soma conhecida como **total parcial**, sinalizando informação ausente; não tratar dado faltante como 0 mg |
| Todos os alimentos da refeição possuem dado válido de vitamina C | Somar as quantidades informadas e exibir total do consumo registrado sem aviso de item faltante |
| Nenhum alimento consumido possui informação de vitamina B12 | Mostrar sem dados disponíveis em vez de 0 µg |
| Fonte informa explicitamente 0 mg de cálcio | Preservar valor zero conhecido e não confundir com nutriente sem informação |
| Micronutriente é fornecido em unidades compatíveis com conversão (mg e µg) | Normalizar corretamente para unidade escolhida e evitar soma de unidades incompatíveis |
| Usuário adiciona refeição retroativa com vitamina C conhecida | Recalcular o total de vitamina C e sua indicação de cobertura no dia e período correspondentes |
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

A etapa 5 continua em planejamento. **Edição/exclusão de refeições, registros retroativos, fuso com correção manual, histórico discreto, metas opcionais pessoais/profissionais, novos objetivos profissionais automáticos, indicadores diários, resumos e gráficos semanais/mensais com contagem estrita dos dias acima, abaixo ou exatamente na meta (sem tolerância) e diferença acumulada sobre metas nos dias comparáveis, acompanhamento de peso e IMC atual, além de exibição de vitaminas/minerais com indicação de totais parciais quando faltarem dados, foram aprovados para o MVP.** Permanecem pendentes visualização detalhada da cobertura, eventual referência de micronutrientes, horário de verão e altura histórica, retenção e acesso à trilha.
