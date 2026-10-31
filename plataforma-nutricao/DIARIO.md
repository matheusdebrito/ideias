# Diário alimentar, metas e indicadores — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 5 de 10 — Diário alimentar, metas e indicadores (em andamento)
> **Versão:** 0.5
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
- Resumos por semana e mês fazem parte do objetivo do MVP; fórmulas de saldo, médias, tolerâncias e visualização ainda precisam de definição.
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
- **Pendente:** decidir se haverá notificação automática ao nutricionista quando o paciente alterar uma meta; visualização no painel não implica envio de mensagens, alertas ou e-mails.

## 4. Decisões em aberto da etapa 5

1. **Metas:** ajuste pessoal com confirmação, visibilidade no painel profissional e aplicação automática de novas metas do nutricionista já definidos. Resta decidir se haverá notificação ativa ao nutricionista quando o paciente ajustar uma meta.
2. **Períodos e fusos:** como classificar refeições lançadas retroativamente ou perto da meia-noite e como tratar histórico após mudança de fuso?
3. **Indicadores:** quais totais, saldos, médias e critérios de cobertura de micronutrientes serão exibidos?
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

**Pendente para próxima decisão:** decidir sobre notificações ativas ao nutricionista quando o paciente ajustar sua meta.

## 6. Fluxo de correção do diário (**Definido para o MVP**)

1. O usuário seleciona uma refeição registrada e escolhe **Editar** ou **Excluir**.
2. Ao editar, o formulário apresenta os dados atuais; a pessoa pode alterar alimentos/porções e demais dados de consumo real sem alterar o plano ou modelos.
3. Ao salvar, validar alimentos e unidades, recalcular nutrientes e agregados, registrar a correção com autor e instante, e atualizar a apresentação.
4. Ao excluir, retirar a refeição dos resultados e totais do diário. Manter somente os dados de auditoria necessários enquanto a política de retenção futura permitir.
5. Não exigir justificativa para editar ou excluir. O acesso ao histórico de correções e suas regras de guarda/eliminação não estão decididos.

**Critério de UX:** permitir corrigir 200 g de arroz para 150 g dentro do mesmo registro, sem forçar exclusão e recadastro. As correções são explícitas; alterações futuras de catálogo, alimento ou dieta não são correções do diário.

## 7. Casos de aceitação de referência

| Cenário | Resultado esperado |
| --- | --- |
| Usuário sem dieta registra refeição avulsa | Consumo entra no diário normalmente |
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
| Paciente modifica meta de acompanhamento | A aplicação não promete ou envia notificação automática ao profissional sem decisão específica sobre alertas |
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

A etapa 5 continua em planejamento. **Edição/exclusão de refeições, histórico discreto de correções, ajustes de metas confirmados pelo paciente, visibilidade profissional autorizada e aplicação automática de novas metas do nutricionista foram aprovados para o MVP.** Notificações ativas ao profissional, retenção e acesso à trilha permanecem pendentes.
