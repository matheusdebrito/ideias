# Dietas e biblioteca de refeições — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Módulo:** MVP
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
- O paciente pode consultar o plano profissional, mas não alterar a prescrição de seu nutricionista.

## 2. Conceitos separados

| Conceito | Função | Titular / dono | Mutabilidade |
| --- | --- | --- | --- |
| Refeição planejada | Indica o que compõe o plano alimentar em uma determinada versão | Versão do plano pessoal ou profissional | Editável apenas em rascunho; publicada é histórica |
| Modelo pessoal de refeição | Atalho reutilizável para registrar consumo | Usuário | Editável e excluível sem alterar diário |
| Modelo profissional de refeição | Bloco reutilizável para criar dietas dos pacientes | Nutricionista | Editável e excluível sem alterar dietas já publicadas |
| Refeição consumida | Registro do que a pessoa efetivamente comeu em uma data | Usuário / paciente | Corrigível por ação explícita, sem depender de alterações nos modelos |
| Item alimentar | Alimento, quantidade, unidade, fonte e nutrientes | Composição de um desses registros | Deve manter referência e valores próprios ao contexto em que foi usado |

A mesma implementação de seleção de alimentos, medidas e cálculo nutricional pode ser reaproveitada, mas **não** deve acoplar o ciclo de vida dos registros.

## 3. Registro alimentar — três caminhos (**Definido**)

### 3.1. A partir do plano alimentar

1. Usuário abre o diário e escolhe adicionar uma refeição.
2. Seleciona uma das refeições da versão vigente do seu plano alimentar.
3. O sistema prepara uma **cópia independente** dos alimentos e das quantidades para revisão.
4. O usuário pode ajustar porções, remover ou adicionar alimentos segundo o consumo real.
5. Ao confirmar, a refeição é registrada no diário com data, horário, quantidades e nutrientes calculados.
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

O nutricionista poderá:
- Criar modelos reutilizáveis contendo nome, itens alimentares e quantidades.
- Pesquisar, consultar, editar e excluir seus modelos.
- Selecionar um ou mais modelos para compor as refeições planejadas ao preparar planos de diferentes pacientes.
- Ajustar livremente a cópia dentro do **rascunho** de cada plano, sem mudar o modelo da biblioteca ou outros pacientes.
- Ver os valores nutricionais calculados da refeição ao montá-la.

**Isolamento:**
- Modelos profissionais pertencem ao nutricionista que os criou.
- Um nutricionista não acessa a biblioteca privada de outro.
- A biblioteca profissional não é automaticamente compartilhada com pacientes, nem copiada para a biblioteca pessoal deles.
- Refeições consumidas por pacientes não entram automaticamente na biblioteca do nutricionista.
- Encerrar vínculo com um paciente não elimina registros históricos nem transfere propriedade da biblioteca profissional.

**Persistência:** ao adicionar um modelo profissional a um plano, criar dados próprios daquela versão/rascunho. Atualizar ou excluir o modelo original **não muda** nenhum plano publicado e não altera planos de outros pacientes.

## 5. Dietas e versionamento (**Definido**)

- O plano pessoal pertence ao usuário; o profissional é criado pelo nutricionista e atribuído a um paciente.
- Uma versão em rascunho pode ser editada antes da publicação.
- A publicação fixa alimentos, quantidades, orientações, valores de referência e início da vigência.
- Atualização de versão publicada cria nova versão do plano, preservando as anteriores.
- O histórico deve permitir identificar a versão vigente em determinada data.
- O paciente pode registrar consumo diferente do plano sem alterar a versão prescrita.
- Uma nova versão de dieta não substitui retroativamente valores registrados no diário.
- É proibido alterar automaticamente dietas de outros pacientes quando o nutricionista modifica um modelo em sua biblioteca.

## 6. Organização semanal: padrão diário com personalização (**Definido para o MVP**)

**Modelo escolhido:** cada plano possui um **cardápio diário padrão**, que se repete nos sete dias da semana. Quem monta o plano (usuário comum ou nutricionista) pode **personalizar refeições em dias específicos da semana**, sem precisar preencher sete cardápios completos.

### 6.1. Comportamento

1. Criar refeições no cardápio padrão (por exemplo, café da manhã, almoço e jantar).
2. Por padrão, essas refeições aparecem de segunda-feira a domingo.
3. Para um dia da semana, permitir substituir, acrescentar ou retirar **refeições planejadas específicas**.
4. Refeições não personalizadas naquele dia continuam seguindo o cardápio padrão.
5. Remover uma personalização restaura a refeição do padrão naquele dia; remover intencionalmente a refeição apenas naquele dia deve ser uma ação explícita, diferente de restaurar o padrão.
6. Alterar o padrão dentro de um **rascunho** atualiza os dias que ainda herdam aquela refeição; as personalizações explícitas ficam preservadas, sem reescrita silenciosa.
7. Uma vez publicada, a versão preserva o padrão e todas as personalizações; alterações posteriores exigem nova versão.
8. O sistema calcula o total nutricional **de cada dia da semana** conforme o cardápio efetivamente aplicável, e pode apresentar um resumo semanal.

**Exemplo:** almoço padrão com arroz, feijão e frango em todos os dias. No sábado, o usuário substitui apenas o almoço por outra combinação; café da manhã e jantar continuam herdados do padrão. No domingo, pode usar o padrão integral.

**Limite inicial:** personalizações recorrentes por **dia da semana**. Exceções para datas isoladas (feriados, viagens etc.) não estão confirmadas para o MVP.

### 6.2. Persistência conceitual

Uma versão de plano contém refeições planejadas padrão e eventuais regras de personalização por dia da semana. O que vale em uma data resulta de: (1) versão do plano vigente naquela data; (2) dia da semana; (3) personalizações explícitas, se houver; (4) refeições padrão para o restante.

As refeições do **diário** continuam sendo registros de consumo independentes. Registrar algo diferente do cardápio de terça-feira nunca altera nem a versão do plano nem o padrão de terça-feira.

### 6.3. Critérios de aceitação

- Sem personalizações, os sete dias mostram o mesmo cardápio padrão.
- Personalizar somente o almoço de sábado mantém as outras refeições e os outros dias inalterados.
- Alterar o cardápio padrão em um rascunho não apaga uma personalização explícita de sábado.
- Restaurar o padrão de sábado remove a personalização, sem apagar a refeição padrão.
- Publicar nova versão com mudanças semanais preserva a visualização de versões anteriores.
- Os totais nutricionais de sábado refletem a personalização, não a combinação padrão substituída.

## 7. Invariantes de domínio

1. Refeições consumidas e itens consumidos não dependem da existência de plano ativo ou modelo salvo.
2. Uma refeição planejada pertence a uma versão específica do plano. A versão publicada é imutável.
3. Exclusão ou edição de modelos pessoais/profissionais não reescreve consumos ou planos publicados.
4. Copiar uma refeição jamais significa compartilhar o mesmo registro mutável.
5. Valores e unidades de consumo são armazenados com referência à origem nutricional, para preservar histórico mesmo quando o catálogo for atualizado.
6. Vínculo e permissões são validados no servidor a cada operação; profissional não pode acessar ou alterar dados de outro profissional/paciente.
7. O salvamento opcional como modelo deve ser explícito e separado do registro alimentar.
8. Requisições repetidas de confirmação não devem registrar o mesmo consumo duas vezes sem intenção explícita.

## 8. Proposta de entidades (não é o esquema final)

- `PlanoAlimentar`
- `VersaoPlano`
- `RefeicaoPlanejada` e `ItemPlanejado`, com base diária padrão e personalizações recorrentes por dia da semana
- `ModeloRefeicao` e `ItemModelo` — com propriedade explicitamente diferenciada (usuário ou nutricionista)
- `RefeicaoConsumida` e `ItemConsumido`
- `Alimento` e dados nutricionais versionados / rastreáveis

**Proposta de implementação:** compartilhar componentes de edição e serviço de cálculo, mas não utilizar uma única tabela genérica de refeições para todos os contextos sem avaliar cuidadosamente as permissões, integridade e históricos.

## 9. Cenários de aceitação do MVP

| Cenário | Resultado esperado |
| --- | --- |
| Usuário registra uma refeição da dieta e reduz um alimento | Diário salva quantidade real; dieta permanece intacta |
| Usuário registra uma refeição avulsa sem marcar para reutilização | Registro no diário; nenhum modelo criado |
| Usuário registra uma refeição avulsa e opta por reutilização | Diário recebe o consumo e biblioteca pessoal recebe modelo independente |
| Usuário altera ou exclui modelo pessoal | Refeições já consumidas permanecem iguais |
| Nutricionista usa um modelo em dois planos de pacientes distintos | Cada plano recebe uma cópia independente |
| Nutricionista altera modelo profissional após publicar dietas | Versões publicadas de ambos os pacientes não mudam |
| Nutricionista atualiza dieta de um paciente | Uma nova versão entra em vigor; anterior fica no histórico |
| Paciente registra alimentação diferente do prescrito | O diário registra o ocorrido; plano publicado não muda |
| Nutricionista tenta obter biblioteca de outro nutricionista | Servidor nega acesso |
| Usuário envia duas confirmações iguais por erro de rede | Sistema evita consumo duplicado por repetição da mesma operação |

## 10. Decisões em aberto — não presumir definição

- Refeições programadas terão horário obrigatório, opcional ou apenas ordem?
- Substituições de alimentos farão parte do MVP? Se sim, serão cadastradas manualmente pelo nutricionista?
- Modelos pessoais e profissionais poderão ter categorias, favoritos ou tags desde o MVP?
- Planos pessoais terão o mesmo detalhamento de histórico/versionamento dos profissionais?
- Refeição registrada a partir de um modelo deverá armazenar um identificador de origem opcional (para análises) sem se tornar dependente do modelo?
- Quais ações de correção de histórico permitirão auditoria apropriada?

## 11. Critérios de priorização

No MVP, priorizar a criação e reutilização de modelos, registro de consumo, publicação de planos com histórico e isolamento de dados. Não incluir rede social, compartilhamento público ou recomendações automáticas de substituições sem validação nutricional.
