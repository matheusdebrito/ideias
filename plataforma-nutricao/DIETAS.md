# Dietas e biblioteca de refeições — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Módulo:** MVP
> **Versão:** 3.4
> **Data:** 31/10/2026
> **Fonte da visão geral:** [IDEA.md](./IDEA.md)
> **Regra de trabalho:** decisões confirmadas estão marcadas como **Definido**; sugestões ainda não aprovadas estão marcadas como **Proposta**.

## 1. Objetivo

Permitir que usuários adultos organizem seus próprios planos alimentares e registrem refeições, enquanto nutricionistas criam planos para pacientes e reutilizam refeições padronizadas entre pacientes. Reduzir o retrabalho sem corromper histórico.

**Definido para o MVP:**
- Criar dietas pessoais e profissionais, com refeições e itens alimentares.
- O usuário pode registrar uma refeição planejada, usar um modelo próprio já salvo ou criar uma refeição avulsa.
- Uma refeição avulsa confirmada entra no diário, mas **não** passa automaticamente a integrar a biblioteca de modelos reutilizáveis. O usuário escolhe se deseja salvá-la para uso futuro.
- O nutricionista dispõe de uma **biblioteca de refeições profissionais**, reutilizável ao criar dietas de diferentes pacientes.
- Toda dieta publicada tem versões: novas publicações não sobrescrevem versões históricas.
- Diário alimentar, bibliotecas de refeições e planos versionados são conceitos independentes.
- O paciente pode consultar o plano profissional, mas não alterar a prescrição de seu nutricionista. **Exceção conceitual distinta:** o paciente pode, após aviso e confirmação, ajustar a **meta de acompanhamento usada nos seus próprios indicadores**, ainda que a meta original tenha sido definida pelo profissional. Esse ajuste não modifica a prescrição publicada nem atribui uma decisão do paciente ao nutricionista. O nutricionista com vínculo ativo e autorização vigente poderá visualizar no painel do paciente a meta original, a meta ajustada e data/hora/autoria da alteração, com aviso discreto no painel, sem exigir confirmação de leitura nem enviar e-mail, push ou mensagens externas no MVP. Quando o nutricionista publicar uma nova meta, ela passa a ser utilizada automaticamente nos indicadores na sua data de vigência, sem pedir aceite ao paciente; ajustes pessoais futuros seguem permitidos mediante confirmação e sem alterar a prescrição. Ver [DIARIO.md](./DIARIO.md).

## 2. Conceitos separados

| Conceito | Função | Titular / dono | Mutabilidade |
| --- | --- | --- | --- |
| Refeição planejada | Indica o que compõe o plano alimentar em uma determinada versão | Versão do plano pessoal ou profissional | Editável apenas em rascunho; publicada é histórica |
| Modelo pessoal de refeição | Atalho reutilizável para registrar consumo | Usuário | Editável e excluível sem alterar diário |
| Modelo profissional de refeição | Bloco reutilizável para criar dietas dos pacientes | Nutricionista | Editável e excluível sem alterar dietas já publicadas |
| Refeição consumida | Registro do que a pessoa efetivamente comeu em uma data | Usuário / paciente | Editável ou excluível pelo dono do diário, com recálculo de agregados e histórico discreto de correções, sem depender de alterações nos modelos |
| Item alimentar | Alimento, quantidade, unidade, fonte e nutrientes | Composição de um desses registros | Deve manter referência e valores próprios ao contexto em que foi usado |

A mesma implementação de seleção de alimentos, medidas e cálculo nutricional pode ser reaproveitada, mas **não** deve acoplar o ciclo de vida dos registros.

## 3. Registro alimentar — três caminhos (**Definido**)

### 3.1. A partir do plano alimentar

1. Usuário abre o diário e escolhe adicionar uma refeição.
2. Seleciona uma das refeições da versão vigente do seu plano alimentar.
3. O sistema prepara uma **cópia independente** dos alimentos e das quantidades para revisão.
4. O usuário pode ajustar porções, remover ou adicionar alimentos segundo o consumo real.
5. Ao confirmar, a refeição é registrada no diário com a data, o horário real do consumo (quando informado ou capturado pelo fluxo) e os nutrientes calculados. O horário sugerido no plano não é copiado como se fosse o horário real. **Lançamentos retroativos são permitidos:** sugerir data/hora atuais, mas permitir editar para o dia/hora efetivos da refeição; contabilizar no dia declarado de consumo, preservando também o instante separado de cadastro e a meta vigente na data do consumo. **Fuso:** o diário sugere o fuso IANA configurado no navegador, com correção manual por refeição; preservar data/hora local e fuso escolhidos, sem reclassificar refeições antigas quando o aparelho mudar de fuso.
6. Isso não edita a dieta e não cria um modelo pessoal automaticamente.

### 3.2. A partir de uma refeição pessoal salva

1. Usuário escolhe um modelo da sua biblioteca pessoal.
2. O sistema prepara uma cópia editável.
3. O usuário ajusta, se necessário, e confirma o consumo.
4. O modelo original continua inalterado e a entrada no diário é independente.

### 3.3. Refeição avulsa

1. Usuário informa nome da refeição, alimentos e quantidades.
2. Confirma o registro no diário.
3. Pode optar explicitamente por **salvar também como modelo pessoal** para reutilização futura.
4. Sem essa escolha, a refeição fica apenas no diário.

**Ponto de UX:** deixar claro que “Registrar consumo” e “Salvar para reutilizar” são ações diferentes. Evitar qualquer gravação de modelo feita silenciosamente.

## 4. Biblioteca de refeições do nutricionista (**Definido para o MVP**)

O nutricionista cria a refeição selecionando **alimentos do catálogo**, inclusive os alimentos personalizados aos quais tenha acesso, e informando as quantidades. Ele não cadastra manualmente calorias, proteínas, carboidratos, gorduras, vitaminas ou minerais **ao montar a refeição**: a aplicação calcula esses valores a partir da composição nutricional dos alimentos selecionados e das quantidades informadas. **Exceção distinta:** ao cadastrar um **novo alimento personalizado**, o criador informa os valores nutricionais essenciais do alimento (kcal, proteínas, carboidratos e gorduras totais) por quantidade de referência; a aplicação então calcula os valores proporcionais nas refeições.

O nutricionista poderá:
- Criar modelos reutilizáveis com nome, refeições/itens alimentares e quantidades.
- Adicionar uma **observação opcional à refeição inteira** (por exemplo, orientação geral de preparo ou consumo).
- Adicionar uma **observação opcional a cada alimento/item** (por exemplo, modo de preparo ou detalhe específico daquele alimento).
- As observações são instruções em texto e não substituem alimento, quantidade ou dados nutricionais.
- Pesquisar alimentos no catálogo e adicionar, remover ou ajustar suas quantidades.
- Consultar automaticamente os valores nutricionais calculados por alimento, por refeição e no total do plano.
- Pesquisar, consultar, editar e excluir seus modelos.
- Selecionar uma refeição da biblioteca e adicioná-la à dieta de um paciente.
- Escolher o paciente e a dieta de destino. Se não houver dieta, iniciar a criação de um plano; se já existir um plano publicado, criar/usar um rascunho para a próxima versão.
- Ajustar a cópia da refeição dentro do **rascunho** do plano daquele paciente, sem mudar o modelo da biblioteca ou planos de outros pacientes.
- Criar uma refeição diretamente dentro do rascunho de uma dieta, com opção de também salvá-la na biblioteca profissional para reutilização futura.

**Apresentação das observações — requisito de UX do MVP:**
- Não exibir campos, rótulos ou espaços vazios para observações não preenchidas.
- No editor, manter os campos de observação recolhidos por padrão e disponibilizar uma ação discreta como **Adicionar observação**; expandir o campo somente quando solicitado ou quando já houver conteúdo.
- Na visualização da dieta, mostrar a observação da refeição como texto secundário logo abaixo do nome da refeição, somente quando existir.
- Mostrar a observação de um alimento em linha secundária abaixo do nome do alimento, somente quando existir; não criar uma coluna extra permanente.
- Em dispositivos móveis, manter a hierarquia visual compacta e permitir expandir/recolher observações longas.
- Não misturar observações com valores nutricionais nem dar a elas destaque visual maior que o nome da refeição/alimento.

**Semântica e histórico das observações:**
- Observações de refeição e de alimento fazem parte do modelo ou da versão do plano em que foram cadastradas.
- Ao copiar uma refeição da biblioteca para um rascunho, copiar também suas observações; o nutricionista pode ajustá-las na cópia.
- A atualização do modelo na biblioteca não altera observações em dietas já publicadas.
- Ao exibir uma refeição planejada no fluxo de registro do diário, as observações podem ser mostradas como referência, mas não devem ser tratadas como prova de que o alimento foi preparado/consumido daquela forma nem gerar alertas automáticos.

**Composição fixa das refeições — decisão para o MVP:**
- Cada refeição planejada contém uma composição fixa de alimentos e quantidades definida pelo usuário (em plano pessoal) ou pelo nutricionista (em plano profissional).
- Não haverá funcionalidade para cadastrar alternativas de alimentos dentro de um mesmo item/refeição nem interface para o paciente escolher substituições alimentares.
- Se o nutricionista desejar mudar a composição, deverá editar a refeição no rascunho e publicar uma nova versão do plano (ou personalizar o cardápio de um dia específico conforme as regras semanais). Não apresentar várias combinações como alternativas para o paciente escolher.
- Se o paciente não consumir exatamente o que foi planejado, poderá registrar no diário os alimentos e quantidades que realmente consumiu. Isso não altera a dieta prescrita.
- Não sugerir substituições automáticas nem tratar a ausência de um alimento como autorização para o paciente escolher outro equivalente.
- A possibilidade de substituições orientadas pelo nutricionista poderá ser reavaliada no futuro, caso haja evidência de necessidade e critérios nutricionais claros.

**Cálculo nutricional:**
- O cálculo é derivado dos alimentos, quantidades, unidades e dados de composição disponíveis na fonte nutricional.
- A aplicação apresenta calorias, macronutrientes e vitaminas/minerais disponíveis na fonte nutricional selecionada posteriormente, sem pedir que o nutricionista digite macros.
- Conversões de unidades e medidas caseiras precisam usar fatores definidos e rastreáveis; não presumir equivalência de gramas e mililitros. São aceitas composições por 100 g, 100 mL ou porção declarada, desde que a referência seja explícita; medidas caseiras somente quando o alimento possuir equivalência documentada.
- Um alimento não pode ser selecionado em medida incompatível sem fator de conversão rastreável. Uma refeição planejada/consumida deve preservar a referência nutricional e o fator usados em seus totais históricos.
- **Forma pronta para consumo é o padrão da dieta e do diário.** Refeições com arroz, feijão, carnes, massas etc. devem ser planejadas e pesadas após o preparo, utilizando valores nutricionais correspondentes ao estado realmente consumido (cozido, grelhado, assado, frito etc.).
- Alimentos normalmente consumidos crus (frutas, saladas, castanhas etc.) continuam válidos. Entradas nutricionais de ingredientes crus que exigem cocção, caso existam na fonte, não serão opções principais no fluxo de prescrição/consumo.
- Não aplicar valores nutricionais de alimento cru à quantidade pesada após preparo; não converter automaticamente entre cru e cozido por fator genérico. O preparo pode modificar água, gordura e a composição por 100 g.
- Ao montar receitas a partir de ingredientes crus, poderão ser necessários rendimentos e retenção de nutrientes; esse fluxo não está confirmado para o MVP e permanece como evolução possível.
- O seletor de alimentos diferencia produtos industrializados por marca e variante relevantes quando disponíveis; alimentos genéricos comuns não exigem marca. Produtos de marcas distintas não devem ser combinados como se tivessem a mesma composição nutricional.
- Dados ausentes na fonte devem ser apresentados como não disponíveis, nunca como zero inventado. Alimentos personalizados cadastrados manualmente exigem kcal, proteínas, carboidratos e gorduras totais completos; micronutrientes podem permanecer desconhecidos, e relatórios agregados desses nutrientes devem sinalizar cobertura incompleta.
- Alimentos importados de bases nutricionais podem aparecer na consulta mesmo sem os quatro dados essenciais, mas **não podem ser adicionados** a uma dieta, modelo de refeição ou diário enquanto faltar energia, proteínas, carboidratos ou gorduras totais. O backend deve validar a completude antes de aceitar o registro; ausência apenas de micronutrientes não bloqueia o uso.
- O cálculo e o arredondamento devem ser consistentes entre biblioteca, dieta e diário. Para o MVP, **não arredondar valores intermediários**: aplicar fatores proporcionais, somar os nutrientes com a precisão disponível e arredondar somente na apresentação.
- Formatar calorias normalmente como número inteiro ou com uma casa conforme contexto, macronutrientes com uma casa decimal e micronutrientes com precisão adequada à unidade e à fonte; totais calculados de valores não arredondados podem apresentar diferença mínima da soma das parcelas arredondadas já visíveis.
- Calcular com precisão numérica estável sem alegar exatidão maior do que a fonte nutricional oferece.

**Isolamento:**
- Modelos profissionais pertencem ao nutricionista que os criou.
- Um nutricionista não acessa a biblioteca privada de outro.
- A biblioteca profissional não é automaticamente compartilhada com pacientes, nem copiada para a biblioteca pessoal deles.
- Alimentos personalizados usados em refeições profissionais são privados ao nutricionista que os cadastrou. O paciente vê apenas a cópia de nome, porção e informações nutricionais incluída na versão publicada de sua dieta, não o registro original no catálogo profissional.
- Registrar no diário uma refeição da dieta pode copiar os valores nutricionais da versão publicada para um novo registro independente; não autoriza acessar, editar ou incorporar o alimento privado do profissional ao catálogo pessoal.
- A leitura de registros privados deve ser autorizada no servidor, nunca apenas pela ocultação da interface.
- Refeições consumidas por pacientes não entram automaticamente na biblioteca do nutricionista.
- Encerrar vínculo com um paciente não elimina registros históricos nem transfere propriedade da biblioteca profissional.

**Fluxo de atribuição à dieta do paciente:**
1. Nutricionista cria ou abre um modelo na biblioteca profissional.
2. Seleciona **Adicionar à dieta de um paciente**.
3. Escolhe o paciente e a dieta de destino (ou inicia uma nova).
4. A aplicação copia a composição da refeição para o rascunho da dieta.
5. O nutricionista pode ajustar a cópia para aquele paciente.
6. A refeição só chega ao paciente quando a versão do plano for publicada.

**Persistência:** ao adicionar um modelo profissional a um plano, criar dados próprios daquela versão/rascunho. Atualizar ou excluir o modelo original **não muda** nenhum plano publicado e não altera planos de outros pacientes. Atribuir uma refeição a uma dieta não significa registrar consumo no diário do paciente.

### 4.1. Horários sugeridos das refeições (**Definido para o MVP**)

- O horário da refeição planejada é **opcional**.
- Se informado, é apenas uma sugestão de organização para o paciente, não uma obrigação.
- Atrasar ou adiantar uma refeição em relação ao horário sugerido não gera erro, alerta, notificação de atraso, penalidade ou indicação automática de descumprimento.
- O horário sugerido pertence à refeição planejada na versão do plano.
- O horário real do consumo pertence ao diário alimentar e deve ser registrado separadamente.
- A diferença entre horário sugerido e horário de consumo não será usada para classificar adesão ou gerar alertas no MVP.

**Exemplo:** refeição sugerida para 07:00 e registrada pelo usuário às 08:00. O diário salva o consumo normalmente, sem aviso de atraso.

## 5. Dietas e versionamento (**Definido**)

- O plano pessoal pertence ao usuário; o profissional é criado pelo nutricionista e atribuído a um paciente.
- **Planos pessoais também são versionados no MVP:** ao salvar alterações em um plano pessoal já salvo, criar uma nova versão e preservar a anterior para consulta histórica.
- O fluxo pessoal é mais simples que o profissional: não exige agendamento de início, cancelamento ou reagendamento de vigência. A nova versão pessoal passa a ser a versão atual ao ser salva.
- Permitir editar um rascunho sem criar uma versão publicada a cada alteração de campo; a nova versão é criada no momento de salvar/publicar a atualização.
- **Regra do MVP:** cada paciente pode ter no máximo uma dieta profissional vigente por vez. O sistema deve impedir a publicação de uma segunda dieta profissional ativa para o mesmo paciente.
- **Regra do MVP:** cada paciente pode ter no máximo uma versão profissional futura no estado `SCHEDULED` por vez. O backend e a persistência devem impedir um segundo agendamento enquanto houver um existente não cancelado ou ativado.
- A primeira dieta profissional atribuída ao paciente também pode ser publicada com vigência imediata ou programada; não é obrigatório existir uma versão anterior.
- Alterar a prescrição vigente cria uma nova versão do mesmo plano lógico, com início de vigência definido. A versão anterior é encerrada e permanece consultável no histórico.
- Não criar uma segunda dieta profissional paralela para contornar o versionamento. Se futuramente houver necessidade de planos paralelos (por exemplo, estratégias distintas), isso será uma evolução de produto fora do MVP.
- Uma versão em rascunho pode ser editada antes da publicação.
- Ao publicar uma versão profissional, o nutricionista escolhe uma das modalidades: **vigência imediata** ou **vigência programada** para data e horário futuros.
- Na vigência imediata, a versão passa a ser vigente no momento da publicação e encerra a vigência da versão anterior.
- Na vigência programada, a versão é publicada com estado `SCHEDULED`; a versão atualmente vigente permanece ativa até a data/hora programada.
- Na data/hora programada, a transição ocorre automaticamente: a versão programada passa a `ACTIVE` e a anterior tem sua vigência encerrada. Não depender de login, ação manual ou abertura do aplicativo.
- Datas/horários programados devem ser interpretados no fuso horário definido para o paciente e armazenados de forma inequívoca (por exemplo, instante UTC mais identificador IANA do fuso), evitando ativação no horário errado por diferenças de fuso ou horário de verão.
- A ativação automática é responsabilidade do backend, nunca de um temporizador no navegador ou de o paciente abrir o aplicativo. O processamento deve ser idempotente e transacional, suportar retomada após indisponibilidade e não ativar duas versões simultaneamente.
- A data/hora de vigência é a fonte de verdade. Mesmo que um processo em segundo plano execute com atraso, a leitura do plano vigente deve considerar o instante atual e a linha do tempo de vigência; o processamento em segundo plano não pode deixar o paciente vendo uma versão expirada como atual.
- A versão programada não pode ter início no passado. O backend valida a data/hora e a ausência de sobreposição de vigências antes de publicar.
- Se já existir uma versão `SCHEDULED` para o paciente, uma nova publicação programada deve ser bloqueada com mensagem clara e orientação para cancelar ou reagendar a versão pendente. Não substituir nem cancelar silenciosamente o agendamento anterior.
- Se o nutricionista precisar publicar uma versão imediatamente enquanto houver uma versão futura programada, deverá primeiro resolver explicitamente o agendamento pendente (por exemplo, cancelá-lo); o sistema não deve cancelar uma versão programada automaticamente **por causa da publicação de outra dieta**. A regra de **encerramento do vínculo pelo paciente** é uma exceção distinta, que cancela agendamentos futuros por perda de vínculo e autorização, sem substituir a prescrição.
- Antes do início da vigência, o nutricionista autorizado poderá **cancelar** uma versão programada. O estado passa para `CANCELLED`, a versão permanece no histórico de auditoria e a versão atual continua vigente sem interrupção.
- Antes do início, o nutricionista autorizado poderá **reagendar** a data/hora de início da versão programada. O conteúdo nutricional publicado permanece imutável; apenas os metadados de agendamento mudam.
- Cancelamento e reagendamento exigem validação no backend e registro de autor, data/hora, ação, data/hora anterior e nova data/hora (quando aplicável), e motivo opcional.
- Se a data/hora de início for alcançada antes de a solicitação de cancelamento/reagendamento ser confirmada, a operação deve ser recusada; após a ativação, qualquer alteração exige uma nova versão.
- Cancelar uma versão programada não cancela nem encerra a dieta atualmente vigente.
- A disputa entre o processo automático de ativação e uma ação manual de cancelar/reagendar deve ser resolvida de forma transacional e idempotente, impedindo que uma versão seja ativada e cancelada simultaneamente.
- A publicação fixa alimentos, quantidades, observações/instruções da refeição e dos itens, valores de referência e início da vigência.
- Uma versão profissional programada é uma publicação válida, mas não é a versão vigente até chegar sua data/hora de início.
- Por paciente, pode existir apenas uma próxima dieta programada. A versão vigente e uma única versão futura `SCHEDULED` podem coexistir temporalmente, desde que suas vigências não se sobreponham.
- O paciente pode consultar a versão futura publicada antes do início em uma seção claramente identificada como **Próxima dieta**, com data/hora prevista de início.
- Se for a primeira dieta profissional e ainda não houver versão vigente, a interface mostra somente **Próxima dieta**, com a data/hora de início. Não deve exibir uma dieta vigente vazia nem apresentar erro por ainda não existir plano ativo.
- Enquanto não houver dieta profissional vigente, o paciente continua podendo usar o diário alimentar normalmente: registrar refeições avulsas ou modelos pessoais, sem depender de uma dieta profissional. A dieta futura não será usada como origem de refeição no diário antes da ativação.
- A próxima dieta é apresentada em modo de consulta, sem substituir visualmente a dieta vigente e sem ser selecionada automaticamente no fluxo de registro de consumo antes de sua ativação.
- A versão futura não permite que o paciente edite a prescrição; dúvidas ou pedidos de alteração devem ser tratados com o nutricionista.
- O histórico precisa registrar autor, momento da publicação, modalidade de vigência, data/hora efetiva e fuso usado.
- O histórico de agendamento deve registrar cancelamentos e reagendamentos, incluindo autor, instante da ação, horário anterior e novo horário, além de motivo opcional.
- Atualização de versão profissional publicada cria nova versão do plano, preservando as anteriores.
- Atualização de plano pessoal salvo também cria uma nova versão, preservando a anterior; não é necessário agendamento de vigência para planos pessoais.
- A publicação imediata encerra a vigência da versão anterior no momento da publicação; a publicação programada mantém a versão anterior vigente até a data/hora futura definida.
- A transição programada encerra a vigência anterior e ativa a nova versão de forma atômica; não pode haver sobreposição de duas versões vigentes do mesmo plano.
- Para cada paciente, não pode haver dois planos profissionais vigentes ao mesmo tempo.
- O histórico deve permitir identificar a versão vigente em determinada data.
- O paciente pode registrar consumo diferente do plano sem alterar a versão prescrita.
- Uma nova versão de dieta não substitui retroativamente valores registrados no diário.
- É proibido alterar automaticamente dietas de outros pacientes quando o nutricionista modifica um modelo em sua biblioteca.

### 5.1. Ativação de publicação já programada após vencimento da assinatura profissional (**Confirmado no MVP**)

- A expiração da assinatura do nutricionista **não cancela, por si só**, uma dieta profissional que ele **já publicou e programou** enquanto possuía autorização profissional válida. A versão poderá ser ativada na data/hora marcada, desde que **vínculo com aquele mesmo nutricionista e autorização do usuário continuem válidos no instante da ativação**.
- A ativação agendada é execução de uma publicação já registrada, não uma nova edição ou publicação profissional. Assinatura vencida continua impedindo novas versões, edições, agendamentos e reagendamentos iniciados pelo profissional e o acesso a dados nutricionais, conforme [MONETIZACAO.md](./MONETIZACAO.md).
- **Prioridade do término de vínculo:** se o usuário encerrou acompanhamento, trocou de nutricionista ou revogou autorização, agendamentos antigos não podem ativar. O vínculo ativo e a autorização devem ser verificados junto com a atualização programada; a transição entre versões deve ser atômica e idempotente. A etapa 9 detalhará concorrência, tarefas e conferência de elegibilidade profissional independente da assinatura.
- Preservar o conteúdo publicado, a autoria, o histórico e o instante real de vigência; a expiração comercial não modifica o diário do usuário nem altera prescrições retroativamente.

### 5.2. Encerramento da vigência quando termina o acompanhamento (**Confirmado no MVP**)

- Quando o paciente **encerrar o vínculo ou confirmar a troca direta por convite de outro nutricionista**, **a versão profissional vigente do vínculo anterior deixa de ser vigente no instante do encerramento**. Preservar a prescrição publicada sem edições retroativas, com autor e intervalo de vigência real identificáveis. Ela passa a ficar acessível **apenas no histórico do paciente**, para consulta.
- **Agendamentos futuros do vínculo encerrado não podem mais entrar em vigor.** A versão programada é cancelada por encerramento do vínculo, mantendo conteúdo, autoria, programação original e motivo/data do cancelamento no histórico. Nenhum job de ativação pode ativá-la posteriormente.
- O **encerramento de vínculo é um evento do domínio diferente de cancelar uma publicação programada por decisão do profissional**. A proibição de cancelamento automático quando o nutricionista publica outra versão não se aplica ao encerramento de acompanhamento iniciado pelo paciente.
- **O paciente não perde o conteúdo da dieta:** todas as versões profissionais publicadas para ele permanecem disponíveis para consulta histórica, com refeições, quantidades, autoria e vigência original. Preservar também diário de consumo e demais dados sem alterações retroativas. **Não converter a dieta antiga automaticamente em plano pessoal vigente** nem atribuí-la ao nutricionista seguinte; a referência permanece identificada como **dieta anterior, não vigente**. A manutenção da última meta efetiva para indicadores pessoais é independente do status da dieta (ver [DIARIO.md](./DIARIO.md)).
- As dietas pessoais do paciente não são afetadas pelo término do vínculo profissional.
- A etapa 9 deverá especificar **operação atômica e idempotente** para fim do vínculo, revogação de acesso, fim de vigência e cancelamento dos agendamentos, mesmo quando concorrer com a transição agendada. Registros de histórico permitem reconstruir qual versão esteve vigente em cada instante.
- **Troca direta aprovada:** após aceite de convite válido e uma confirmação do paciente, encerrar dieta antiga, cancelar agendamentos futuros e ativar novo vínculo autorizado **na mesma transição atômica** que encerra o vínculo anterior. Falha reverte tudo, preservando dieta e vínculo anteriores. A dieta histórica não vira plano do novo profissional nem é automaticamente substituída por outra. **A última meta efetiva do paciente continua como referência pessoal** após desvinculação, com origem profissional histórica preservada; o ex-profissional recebe notificação de encerramento do vínculo com o nome do usuário, conforme já aprovado na etapa 6. Ver [PACIENTES.md](./PACIENTES.md).

## 6. Categorias de refeições (**Definido para o MVP**)

- Categorias padrão: **Café da manhã, Lanche da manhã, Almoço, Lanche da tarde, Jantar e Ceia**.
- O nutricionista poderá criar categorias personalizadas e utilizá-las na biblioteca profissional ou nas dietas dos seus pacientes.
- Cada refeição planejada tem um identificador independente da categoria; **não existe restrição de unicidade por dia + categoria**.
- Duas ou mais refeições podem utilizar a mesma categoria no mesmo dia. Nome, ordem de exibição e horário sugerido opcional podem diferenciá-las visualmente.
- A categoria serve para organização, não para limitar quantidade de refeições nem para aplicar regras nutricionais diferentes.
- Categorias personalizadas de um nutricionista não ficam disponíveis automaticamente na biblioteca de outro profissional.
- Renomear ou arquivar uma categoria personalizada não deve alterar nomes/classificações já preservados em versões publicadas de dietas; a versão mantém os dados necessários para exibição histórica.
- Excluir uma categoria em uso não pode causar desaparecimento de refeições ou quebra de planos já publicados. Preferir arquivamento em vez de exclusão destrutiva.
- A possibilidade de o usuário comum criar categorias personalizadas permanece em aberto; usuários comuns contam com as categorias padrão.

**Critérios de aceitação:**
1. Nutricionista cria uma categoria personalizada e a aplica a refeições de diferentes pacientes.
2. Uma segunda refeição com a categoria Café da manhã na segunda-feira é aceita.
3. A categoria personalizada de um nutricionista não aparece na biblioteca de outro.
4. Arquivar ou renomear a categoria não altera a classificação histórica das dietas já publicadas.

## 7. Organização semanal: padrão diário com personalização (**Definido para o MVP**)

**Modelo escolhido:** cada plano possui um **cardápio diário padrão**, que se repete nos sete dias da semana. Quem monta o plano (usuário comum ou nutricionista) pode **personalizar refeições em dias específicos da semana**, sem precisar preencher sete cardápios completos.

### 6.1. Comportamento

1. Criar refeições no cardápio padrão (por exemplo, café da manhã, almoço e jantar). Um mesmo dia pode conter várias refeições independentes com a mesma categoria, como dois registros de café da manhã.
2. Por padrão, essas refeições aparecem de segunda-feira a domingo.
3. Para um dia da semana, permitir substituir, acrescentar ou retirar **refeições planejadas específicas**. A operação identifica a refeição pelo seu registro próprio, não apenas pela categoria: remover um café da manhã não remove os demais cafés da manhã daquele dia.
4. Refeições não personalizadas naquele dia continuam seguindo o cardápio padrão.
5. Remover uma personalização restaura a refeição do padrão naquele dia; remover intencionalmente a refeição apenas naquele dia deve ser uma ação explícita, diferente de restaurar o padrão.
6. Alterar o padrão dentro de um **rascunho** atualiza os dias que ainda herdam aquela refeição; as personalizações explícitas ficam preservadas, sem reescrita silenciosa.
7. Uma vez publicada, a versão preserva o padrão e todas as personalizações; alterações posteriores exigem nova versão.
8. O sistema calcula o total nutricional **de cada dia da semana** conforme o cardápio efetivamente aplicável, e pode apresentar um resumo semanal.

**Exemplo:** o padrão pode conter dois registros independentes classificados como café da manhã, cada um com sua própria composição e horário sugerido. Na segunda-feira, o nutricionista pode acrescentar um terceiro café da manhã ou personalizar apenas um dos dois. Essa alteração não afeta o outro café da manhã nem os demais dias. O almoço padrão continua herdado normalmente.

**Limite inicial:** personalizações recorrentes por **dia da semana**. Exceções para datas isoladas (feriados, viagens etc.) não estão confirmadas para o MVP.

### 6.2. Persistência conceitual

Uma versão de plano contém várias refeições planejadas padrão, inclusive múltiplas refeições com a mesma categoria, e eventuais regras de personalização por dia da semana. Cada ocorrência planejada tem identidade própria; categoria (por exemplo, café da manhã), horário sugerido e posição de exibição não são identificadores únicos. O que vale em uma data resulta de: (1) versão do plano vigente naquela data; (2) dia da semana; (3) personalizações explícitas, se houver; (4) refeições padrão para o restante.

As refeições do **diário** continuam sendo registros de consumo independentes. Registrar algo diferente do cardápio de terça-feira nunca altera nem a versão do plano nem o padrão de terça-feira.

### 6.3. Critérios de aceitação

- Sem personalizações, os sete dias mostram o mesmo cardápio padrão, incluindo todas as ocorrências de cada categoria.
- Um plano aceita duas ou mais refeições com a categoria café da manhã no mesmo dia.
- Personalizar ou remover uma dessas refeições não altera as outras refeições com a mesma categoria.
- Personalizar somente o almoço de sábado mantém as outras refeições e os outros dias inalterados.
- Alterar o cardápio padrão em um rascunho não apaga uma personalização explícita de sábado.
- Restaurar o padrão de sábado remove a personalização, sem apagar a refeição padrão.
- Publicar nova versão com mudanças semanais preserva a visualização de versões anteriores.
- Os totais nutricionais de sábado refletem a personalização, não a combinação padrão substituída.

## 8. Invariantes de domínio

1. Refeições consumidas e itens consumidos não dependem da existência de plano ativo ou modelo salvo.
2. Um paciente tem no máximo um plano profissional vigente no MVP; a regra deve ser garantida no domínio e na persistência, não apenas escondendo botões na interface.
3. Um paciente tem no máximo uma versão profissional futura programada não cancelada; a regra também deve ser garantida no backend e na persistência.
2. Uma refeição planejada pertence a uma versão específica do plano. A versão publicada é imutável.
3. Exclusão ou edição de modelos pessoais/profissionais não reescreve consumos ou planos publicados.
4. Copiar uma refeição jamais significa compartilhar o mesmo registro mutável.
5. Observações de refeição e de alimento são opcionais e não interferem no cálculo nutricional.
6. Observações vazias não são exibidas na interface.
7. A apresentação das observações deve ser discreta e progressiva, sem aumentar desnecessariamente a densidade visual.
5. Nutricionistas informam alimentos e quantidades, não macros manualmente; nutrientes são calculados pela aplicação a partir da fonte nutricional.
7. Adicionar uma refeição a uma dieta de paciente modifica somente o rascunho/versionamento do plano escolhido; nunca registra consumo no diário.
5. Valores e unidades de consumo são armazenados com referência à origem nutricional, para preservar histórico mesmo quando o catálogo for atualizado.
6. Vínculo e permissões são validados no servidor a cada operação; profissional não pode acessar ou alterar dados de outro profissional/paciente.
7. O salvamento opcional como modelo deve ser explícito e separado do registro alimentar.
8. Requisições repetidas de confirmação não devem registrar o mesmo consumo duas vezes sem intenção explícita.

## 9. Proposta de entidades (não é o esquema final)

- `PlanoAlimentar`
- `VersaoPlano`
- `RefeicaoPlanejada` e `ItemPlanejado`, com base diária padrão e personalizações recorrentes por dia da semana; cada ocorrência planejada tem ID próprio e pode compartilhar categoria com outras refeições do mesmo dia
- `ModeloRefeicao` e `ItemModelo` — com propriedade explicitamente diferenciada (usuário ou nutricionista)
- `RefeicaoConsumida` e `ItemConsumido`
- `Alimento` e dados nutricionais versionados / rastreáveis, incluindo alimentos personalizados cadastrados por usuários comuns e nutricionistas com fonte declarada (ex.: rótulo), controle de acesso e histórico de composição

**Proposta de implementação:** compartilhar componentes de edição e serviço de cálculo, mas não utilizar uma única tabela genérica de refeições para todos os contextos sem avaliar cuidadosamente as permissões, integridade e históricos.

## 10. Cenários de aceitação do MVP

| Cenário | Resultado esperado |
| --- | --- |
| Usuário registra uma refeição da dieta e reduz um alimento | Diário salva quantidade real; dieta permanece intacta |
| Usuário registra uma refeição avulsa sem marcar para reutilização | Registro no diário; nenhum modelo criado |
| Usuário registra uma refeição avulsa e opta por reutilização | Diário recebe o consumo e biblioteca pessoal recebe modelo independente |
| Usuário altera ou exclui modelo pessoal | Refeições já consumidas permanecem iguais |
| Nutricionista usa um modelo em dois planos de pacientes distintos | Cada plano recebe uma cópia independente |
| Nutricionista cria refeição com alimentos e quantidades | Sistema calcula automaticamente os nutrientes; não solicita macros manuais |
| Nutricionista usa uma categoria personalizada | A categoria é aplicada à refeição sem impor unicidade por dia |
| Nutricionista renomeia categoria após publicação da dieta | A dieta publicada continua mostrando sua classificação histórica |
| Nutricionista adiciona observação à refeição | A observação aparece discretamente abaixo do nome da refeição |
| Nutricionista adiciona observação a um alimento | A observação aparece discretamente abaixo do nome do alimento |
| Refeição/alimento não possui observação | Nenhum espaço ou rótulo vazio é exibido |
| Refeição da biblioteca é copiada para um plano | Observações são copiadas; alterações no rascunho não modificam o modelo original |
| Nutricionista adiciona uma refeição da biblioteca à dieta de um paciente | A refeição entra no rascunho da dieta escolhida; não é registrada como consumo |
| Nutricionista ajusta a cópia para um paciente | Biblioteca e planos de outros pacientes permanecem inalterados |
| Dieta profissional apresenta uma refeição fixa | O paciente vê somente os alimentos e quantidades definidos na versão publicada, sem seletor de alternativas |
| Paciente não consome um dos alimentos prescritos | Pode registrar o consumo real no diário; a prescrição permanece inalterada |
| Nutricionista altera modelo profissional após publicar dietas | Versões publicadas de ambos os pacientes não mudam |
| Nutricionista publica nova versão com vigência imediata | A nova versão fica vigente imediatamente e a anterior é encerrada no histórico |
| Nutricionista publica nova versão com vigência futura | A versão atual continua vigente até a data/hora programada; a futura aparece como programada |
| Nutricionista publica a primeira dieta de um paciente com vigência futura | Paciente vê somente a seção Próxima dieta; não há dieta vigente nem erro; diário alimentar continua disponível |
| Paciente registra refeição avulsa antes da primeira dieta profissional entrar em vigor | Registro é salvo normalmente no diário, sem exigir dieta ativa |
| Nutricionista tenta criar um segundo agendamento para o paciente | Backend bloqueia a operação e orienta a cancelar ou reagendar a versão já programada |
| Paciente consulta a próxima dieta antes da vigência | Vê a versão futura em seção identificada, com início previsto, sem confundi-la com a atual nem usá-la automaticamente no diário |
| Nutricionista cancela uma versão programada antes do início | A versão recebe estado cancelado, permanece no histórico e a dieta atual continua vigente |
| Nutricionista reagenda uma versão programada antes do início | A data/hora é atualizada, o conteúdo publicado permanece igual e a alteração fica auditada |
| Solicitação de cancelamento/reagendamento chega após o início da vigência | Backend recusa a operação; mudanças exigem uma nova versão |
| Chega a data/hora de uma versão programada | A transição ocorre automaticamente, sem login ou ação manual; a versão anterior é encerrada |
| Nutricionista publicou versão para ativação futura, mas assinatura venceu após publicar | Agendamento ativa na data prevista se o vínculo/autorização ainda forem válidos; assinatura expirada não cancela publicação |
| Usuário encerra o vínculo depois da publicação e antes da ativação | Programação do profissional anterior não ativa, mesmo que sua assinatura estivesse vencida |
| Assinatura profissional expirada e nutricionista tenta reagendar versão futura | API nega alteração manual; publicação original segue sujeita à ativação conforme vínculo e autorização |
| Nutricionista tenta programar uma vigência que se sobrepõe a outra | Backend impede a publicação e informa o conflito |
| Nutricionista atualiza dieta de um paciente | Uma nova versão entra em vigor imediatamente ou na data programada; anterior fica no histórico |
| Usuário atualiza seu plano pessoal salvo | Nova versão pessoal é criada ao salvar; versão anterior continua disponível no histórico |
| Nutricionista tenta publicar uma segunda dieta profissional vigente para o mesmo paciente | Operação é bloqueada ou direcionada para atualizar o plano vigente; nunca ficam dois planos profissionais ativos |
| Paciente encerra acompanhamento com dieta profissional ativa | Vigência termina naquele instante; versão, autoria e período real continuam no histórico para consulta |
| Paciente encerra acompanhamento com versão profissional futura programada | Cancelar agendamento devido ao término do vínculo, preservar versão publicada e histórico; não ativar mais tarde |
| Paciente encerra acompanhamento com dieta pessoal existente | Dieta pessoal e diário não mudam |
| Ativação agendada concorre com o término do vínculo | Estado final sem dieta profissional vigente e sem ativação posterior, com cronologia consistente |
| Paciente troca diretamente por convite de outro nutricionista | Dieta antiga deixa de ser vigente e futuras programadas são canceladas atomicamente com a troca do vínculo |
| Troca direta não consegue ser concluída | Vínculo e dieta anteriores permanecem; convite não é aceito parcialmente |
| Novo nutricionista assume após confirmação válida | Sem dieta profissional vigente automática, sem transferência da autoria antiga |
| Paciente registra alimentação diferente do prescrito | O diário registra o ocorrido; plano publicado não muda |
| Nutricionista tenta obter biblioteca de outro nutricionista | Servidor nega acesso |
| Usuário envia duas confirmações iguais por erro de rede | Sistema evita consumo duplicado por repetição da mesma operação |

## 11. Decisões em aberto — não presumir definição

- Usuários comuns também poderão criar categorias personalizadas no MVP ou usarão apenas as categorias padrão?
- Favoritos e etiquetas adicionais (tags) nas bibliotecas pessoal e profissional agregam valor suficiente para o MVP?
- Refeição registrada a partir de um modelo deverá armazenar um identificador de origem opcional (para análises) sem se tornar dependente do modelo?
- Quais regras específicas de retenção, eliminação e visibilidade da trilha de correções do diário serão aprovadas nas etapas 8 e 9?

## 12. Critérios de priorização

No MVP, priorizar a criação e reutilização de modelos, registro de consumo, publicação de planos com histórico e isolamento de dados. Não incluir rede social, compartilhamento público, seletor de alimentos alternativos ou recomendações automáticas de substituições.
