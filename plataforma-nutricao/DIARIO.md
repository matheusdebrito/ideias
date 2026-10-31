# Diário alimentar, metas e indicadores — Especificação funcional

> **Status:** planejamento funcional, não implementado
> **Etapa:** 5 de 10 — Diário alimentar, metas e indicadores (em andamento)
> **Versão:** 0.1
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

## 3. Metas e indicadores — confirmado em princípio

- Usuários adultos poderão acompanhar metas calóricas, consumo de calorias e macronutrientes.
- O consumo diário será comparado com a meta válida **na data correspondente**; metas posteriores não reescrevem o histórico.
- Resumos por semana e mês fazem parte do objetivo do MVP; fórmulas de saldo, médias, tolerâncias e visualização ainda precisam de definição.
- Medidas corporais (peso, altura e IMC, com limitações explicadas) integram a visão do produto; o detalhamento de edição e históricos será tratado nesta etapa.
- Metas e estimativas não são automaticamente prescrições clínicas.

## 4. Decisões em aberto da etapa 5

1. **Correções no diário:** que ações de editar, excluir e restaurar serão permitidas? Como preservar rastreabilidade sem complicar o uso nem conflitar com privacidade/retenção?
2. **Metas:** como permitir definir e alterar metas calóricas e de macronutrientes; o que prevalece entre metas pessoais e profissionais?
3. **Períodos e fusos:** como classificar refeições lançadas retroativamente ou perto da meia-noite e como tratar histórico após mudança de fuso?
4. **Indicadores:** quais totais, saldos, médias e critérios de cobertura de micronutrientes serão exibidos?
5. **Medidas corporais:** frequência, correção de registros e apresentação de peso/IMC ao longo do tempo.

## 5. Primeira proposta para avaliação (ainda não aprovada)

Permitir ao dono do diário corrigir ou excluir registros de consumo incorretos sem alterar o plano. Preservar uma trilha proporcional de correções (autor e momento, alteração anterior/posterior) para registros aos quais profissionais tenham acesso autorizado, respeitando regras futuras de privacidade, retenção e eliminação de dados. A interface deve ser simples; não exigir justificativa para cada correção.

## 6. Casos de aceitação de referência

| Cenário | Resultado esperado |
| --- | --- |
| Usuário sem dieta registra refeição avulsa | Consumo entra no diário normalmente |
| Usuário registra alimento diferente do prescrito | Diário reflete o consumo real; dieta permanece igual |
| Dieta é atualizada depois do consumo | Valores do diário já salvo permanecem os mesmos |
| Modelo pessoal é editado após o consumo | Registro antigo permanece independente |
| Duplo envio da mesma confirmação | Apenas uma refeição é registrada |
| Micronutriente não está disponível em alguns itens | Total do micronutriente é explicitamente sinalizado como incompleto |
| Usuário altera sua meta futura | Meta anterior permanece associada aos dias históricos em que vigorou |

---

A etapa 5 está em planejamento; **nenhuma regra sobre edição/exclusão de diário foi aprovada ainda**.
