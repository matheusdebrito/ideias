# Plataforma de Nutrição — Documento de ideia

> **Status:** descoberta do produto  
> **Versão:** 0.19  
> **Última atualização:** 30/10/2026  
> **Este documento registra a visão inicial. Não é uma especificação pronta para implementação.** Decisões ainda não validadas estão marcadas como questões em aberto.

## 1. Visão do produto

Criar uma plataforma de nutrição com duas experiências integradas:

1. **Autonomia alimentar:** permitir que pessoas registrem o que comem, acompanhem a composição nutricional da alimentação, organizem metas pessoais e consultem seu histórico.
2. **Ferramenta profissional:** permitir que nutricionistas gerenciem pacientes, acompanhem informações compartilhadas por eles e elaborem planos alimentares individualizados.

O mesmo usuário deve poder utilizar os recursos pessoais e, se também for nutricionista, os recursos profissionais sem precisar manter contas desconectadas.

A plataforma é uma ferramenta de organização e acompanhamento alimentar, não uma substituta do nutricionista. O produto atende dois públicos centrais: pessoas que desejam organizar a própria alimentação e nutricionistas que precisam de ferramentas para gerir pacientes, elaborar planos e acompanhar resultados. O usuário comum poderá montar livremente seus próprios planos alimentares. Isso deve ser diferenciado de uma prescrição dietética profissional ou de recomendações clínicas para tratar condições de saúde.

## 2. Públicos e perfis

### 2.1. Usuário que acompanha a própria alimentação

Pessoa que deseja registrar refeições, consultar informações nutricionais, acompanhar peso, organizar seus próprios planos alimentares e observar o consumo em relação a metas estimadas ou definidas por ela.

Possíveis necessidades:
- Consultar alimentos e seus nutrientes.
- Registrar refeições e quantidades consumidas.
- Visualizar calorias e macronutrientes por refeição e por dia.
- Consultar históricos diário, semanal e mensal.
- Registrar peso ao longo do tempo.
- Consultar IMC, com explicação dos limites desse indicador.
- Montar e atualizar livremente planos alimentares pessoais, com alimentos, porções e refeições.
- Consultar estimativas nutricionais e energéticas com explicação de suas limitações.

### 2.2. Paciente acompanhado por nutricionista

Usuário que autoriza um nutricionista a acessar todos os dados disponíveis na plataforma durante um vínculo profissional ativo. O produto permitirá um único nutricionista ativo por paciente de cada vez; o acesso depende de autorização vigente e deve cessar quando o vínculo ou a autorização terminar.

Possíveis necessidades:
- Vincular-se a um nutricionista por convite e aceite explícito.
- Consultar o plano alimentar profissional vigente.
- Registrar a alimentação realmente consumida.
- Acompanhar a própria evolução.
- Saber quais informações estão sendo compartilhadas com o profissional.
- Encerrar o vínculo e revogar acessos conforme as regras do produto.

### 2.3. Nutricionista

Profissional que utiliza a plataforma para organizar o acompanhamento de seus pacientes.

Possíveis necessidades:
- Manter perfil profissional e informações de registro.
- Convidar pacientes e administrar vínculos.
- Consultar todos os dados disponíveis do paciente, enquanto houver vínculo ativo e autorização vigente.
- Acompanhar refeições, consumo energético, macronutrientes, peso e histórico.
- Elaborar planos alimentares e atribuí-los a pacientes.
- Atualizar planos e consultar versões anteriores.
- Acompanhar a diferença entre o plano prescrito e a alimentação registrada pelo paciente.

A validação das credenciais profissionais e os requisitos específicos para o perfil de nutricionista ainda precisam ser definidos.

## 3. Módulos funcionais propostos

### 3.1. Perfil e dados antropométricos

- Cadastro de data de nascimento, sexo e altura, conforme a necessidade dos cálculos e do acompanhamento.
- Registro de peso com data e histórico sem sobrescrita de medições anteriores.
- Cálculo de IMC para adultos, acompanhado de explicação sobre suas limitações.
- O cadastro, o acompanhamento e o cálculo do IMC serão disponibilizados apenas para pessoas com **18 anos completos ou mais** no MVP.

**Decisão de produto — escopo adulto:** a plataforma será exclusiva para maiores de idade na primeira versão. Não haverá cadastro de pacientes menores de 18 anos, inclusive por nutricionistas, nem perfis dependentes, nem recursos pediátricos. A data de nascimento informada deverá ser validada no servidor com base na data corrente; contas abaixo de 18 anos devem ter cadastro rejeitado.

**Ressalva jurídica:** informar que o produto é 18+ e coletar data de nascimento não transfere automaticamente ao usuário toda a responsabilidade caso ele forneça dados falsos. Avaliar os riscos de acesso provável por menores conforme ECA Digital, Decreto nº 12.880/2026 e orientações vigentes da ANPD. Definir controles proporcionais e tratamento de contas identificadas como pertencentes a menores. Isso não implica exigir documentos de todos os usuários sem necessidade.

### 3.2. Base de alimentos e composição nutricional

Manter um catálogo pesquisável de alimentos com, quando disponíveis:

- Energia (kcal).
- Carboidratos.
- Proteínas.
- Gorduras totais e, quando disponíveis, subdivisões.
- Fibras.
- Vitaminas.
- Minerais.
- Outros componentes nutricionais que sejam relevantes.

Requisitos importantes para a qualidade dos dados:
- Armazenar a composição por uma base de referência definida, preferencialmente por 100 g de parte comestível.
- Distinguir valores iguais a zero de nutrientes não analisados ou sem informação.
- Identificar alimento cru, cozido, assado, frito ou outra condição relevante.
- Registrar a fonte, a versão e a data de referência dos dados.
- Não misturar valores de fontes diferentes sem identificar sua procedência.
- Permitir conversão de quantidades e medidas caseiras com regras explícitas.
- Preservar a rastreabilidade dos dados usados nos cálculos.

**Fontes candidatas:** TBCA como fonte principal a ser avaliada e TACO como fonte complementar. A incorporação de dados em um produto comercial depende da confirmação das condições de uso, atribuição e redistribuição de cada fonte.

### 3.3. Diário alimentar

- Registro de refeições com data e horário e organização por café da manhã, almoço, jantar, lanches etc.
- Inclusão de alimentos, quantidades e unidades realmente consumidas.
- Cálculo de calorias, proteínas, carboidratos, gorduras e outros nutrientes disponíveis.
- Totalização por refeição e dia; consulta, correção explícita e histórico.

**Regra fundamental:** o diário registra **o que a pessoa efetivamente comeu**, independentemente de ter um plano alimentar ativo. Atualizar, encerrar ou trocar uma dieta não modifica registros anteriores de refeições consumidas.

Cada item consumido deve preservar quantidade, unidade, data/hora, composição nutricional calculada no momento do registro e referência/fonte do alimento. Isso evita que uma atualização futura do catálogo TBCA altere retroativamente os totais históricos.

### 3.3.1. Registro a partir de refeições e modelos (MVP)

O usuário poderá registrar seu consumo de três maneiras:
- **Escolher uma refeição da dieta vigente:** copiar alimentos e quantidades planejadas para um registro de consumo editável.
- **Escolher uma refeição da biblioteca pessoal:** usar modelo de refeição previamente salvo, sem modificá-lo.
- **Criar uma refeição avulsa:** cadastrar alimentos e quantidades manualmente; o registro entra no diário, mas só se torna modelo reutilizável se o usuário escolher explicitamente **salvar para reutilizar**.

Em qualquer fluxo, o usuário poderá ajustar a refeição antes de confirmá-la. A confirmação salva uma **cópia independente** no diário; alterar a dieta ou o modelo não altera o histórico de consumo. Evitar duplicação de registro se a mesma confirmação for reenviada.

**Biblioteca profissional no MVP:** o nutricionista cria refeições adicionando alimentos do catálogo e informando quantidades, sem digitar manualmente calorias ou macros. A aplicação calcula automaticamente a composição nutricional a partir dos dados da TBCA. O nutricionista poderá criar, consultar, editar, excluir e reutilizar seus próprios modelos de refeições ao montar dietas de diferentes pacientes.

O nutricionista poderá selecionar uma refeição da biblioteca e adicioná-la à dieta de um paciente. A aplicação copia os itens e quantidades para o rascunho do plano escolhido; o profissional poderá ajustar essa cópia para o paciente. A refeição só será apresentada ao paciente quando a versão do plano for publicada. Também será possível criar uma refeição diretamente no rascunho e optar por salvá-la na biblioteca profissional.

O nutricionista define uma composição fixa para cada refeição: alimentos e quantidades determinados. O paciente não recebe listas de alimentos alternativos para escolher dentro da refeição no MVP. Se o nutricionista quiser oferecer outra combinação, deverá criar outra refeição fixa no plano ou atualizar a dieta por meio de uma nova versão. O paciente registra no diário o que realmente consumiu, mesmo que seja diferente do plano; isso não altera a prescrição.

**Especificação funcional:** [DIETAS.md](./DIETAS.md).

### 3.4. Metas e indicadores de consumo

- Cadastro de metas calóricas e acompanhamento do consumo.
- Comparação do consumo registrado com a meta válida naquele dia, sem tratar isso como diagnóstico.
- Exibição de calorias e macronutrientes por refeição e total diário.
- Consulta de períodos anteriores, preservando as metas vigentes à época.
- Resumos semanais e mensais. Definições de saldo, média e tolerância em aberto.

**Adultos:** a aplicação poderá estimar uma meta energética por método validado e permitir ajustes manuais. O cálculo requer dados suficientes, explicação dos limites e revisão de profissionais antes do lançamento.

**Faixa etária:** o MVP atende apenas adultos (18 anos ou mais), sem cálculo, prescrição, metas ou acompanhamento pediátrico, inclusive nos painéis profissionais. O cálculo automático de metas energéticas para menores fica fora do escopo.

### 3.5. Planos alimentares e dietas

**Plano pessoal:** o usuário pode montar seus próprios planos com refeições, alimentos e quantidades. Isso é organização alimentar, não prescrição clínica automática. Planos pessoais salvos terão histórico de versões: ao salvar alterações em um plano existente, a aplicação preserva o estado anterior e registra uma nova versão. No MVP, dietas pessoais não precisam de agendamento de vigência.

**Plano profissional:** o nutricionista elabora e atribui planos a pacientes. O paciente pode consultar o plano profissional, mas não alterar sua prescrição. No MVP, cada paciente poderá ter **uma única dieta profissional vigente por vez**. Alterações na prescrição geram novas versões do mesmo plano lógico, mantendo as versões anteriores no histórico.

**Horários sugeridos (confirmado para o MVP):**
- O horário de uma refeição planejada é opcional.
- Quando informado pelo nutricionista ou usuário, representa apenas uma sugestão de organização.
- Não é prazo, obrigação ou critério de validação da refeição.
- Registrar ou consumir a refeição em horário diferente não gera erro, alerta, aviso de atraso ou penalização.
- O horário planejado e o horário real de consumo são informações distintas: o primeiro pertence ao plano; o segundo, ao diário alimentar.

**Instruções e observações (confirmadas para o MVP):**
- O nutricionista ou usuário poderá adicionar uma observação opcional à refeição inteira.
- Também poderá adicionar uma observação opcional a cada alimento/item da refeição.
- As observações são instruções em texto, separadas dos alimentos, quantidades e cálculos nutricionais; não substituem nem alteram esses dados.
- A interface deverá mostrar observações somente quando existirem e usar apresentação discreta, sem criar blocos vazios ou poluir a tela.
- No editor, os campos de observação poderão ficar recolhidos/ocultos até o usuário escolher adicioná-los. Na visualização, a observação da refeição aparece como texto secundário junto ao título; a observação do alimento aparece abaixo do nome do alimento, em estilo discreto.
- Em telas pequenas, priorizar expansão sob demanda e não adicionar colunas permanentes apenas para observações.

**Categorias de refeições (confirmado para o MVP):**
- Categorias padrão: **Café da manhã, Lanche da manhã, Almoço, Lanche da tarde, Jantar e Ceia**.
- O nutricionista pode criar categorias personalizadas para organizar sua biblioteca profissional e os planos que elabora para seus pacientes.
- Uma categoria é apenas uma forma de classificação/exibição e **não limita a uma refeição por dia**. Por exemplo, segunda-feira pode conter dois ou mais cafés da manhã.
- Refeições têm identidade própria e poderão ter nome específico além da categoria.
- Alterar ou arquivar uma categoria personalizada não reescreve nem apaga a classificação existente em versões publicadas de dietas ou no histórico alimentar.
- A possibilidade de categorias personalizadas para usuários comuns será avaliada separadamente; no MVP está confirmada para o nutricionista.

**Organização do cardápio (confirmada para o MVP):**
- Todo plano começa com um **cardápio diário padrão**, repetido nos sete dias da semana.
- Um mesmo dia pode conter **várias refeições planejadas da mesma categoria** (por exemplo, mais de um café da manhã ou lanche), cada uma como registro independente, com seus próprios alimentos, quantidades, observações e horário sugerido opcional.
- O usuário ou nutricionista pode personalizar refeições específicas para **dias da semana** sem precisar refazer todo o cardápio.
- As refeições não personalizadas em determinado dia continuam herdando o padrão diário.
- Substituir, adicionar ou retirar uma refeição específica de um dia não altera as outras refeições, mesmo que tenham a mesma categoria, nem os outros dias.
- É possível restaurar o padrão de uma refeição removendo sua personalização. Retirar intencionalmente uma refeição daquele dia é diferente de restaurar o padrão.
- Os totais nutricionais diários consideram as personalizações aplicáveis. Padrão e personalizações pertencem à versão publicada do plano, preservando seu histórico.
- Exceções para **datas isoladas**, além dos dias da semana, continuam fora do escopo confirmado.

Regras detalhadas e critérios de aceitação em [DIETAS.md](./DIETAS.md).

**Separação entre planejamento, modelos reutilizáveis e consumo:**
- Refeições previstas em uma dieta representam **o plano**.
- Modelos pessoais e profissionais representam **refeições reutilizáveis** na criação de registros ou de planos.
- Refeições registradas no diário representam **o consumo real**.
- O diário é independente das dietas e de suas versões.
- A comparação entre plano e diário poderá identificar diferenças, sem alterar nenhum dos registros.

**Versionamento do plano:**
- Um rascunho pode ser editado antes da publicação.
- Ao atualizar um plano profissional publicado, criar uma nova versão e permitir escolher entre **vigência imediata** (ativa ao publicar) e **vigência programada** (ativa automaticamente na data futura escolhida).
- Quando uma nova versão profissional for programada para uma data futura, a versão atual continua vigente até a transição; a versão futura não substitui a atual antes da data definida.
- O paciente poderá visualizar antecipadamente a próxima dieta publicada em uma área claramente identificada como **Próxima dieta**, com sua data/hora de início. Essa visualização é somente informativa; a dieta futura não será confundida com a prescrição vigente nem usada como padrão para registrar o consumo antes de começar a valer.
- Cada paciente poderá ter **no máximo uma dieta profissional futura programada** por vez. Para programar outra, a anterior deverá ser cancelada primeiro.
- A primeira dieta profissional de um paciente também poderá ter vigência imediata ou programada. Se for programada, o paciente verá apenas a seção **Próxima dieta** até o início da vigência, sem uma dieta vigente exibida.
- Antes do início da vigência, o nutricionista poderá cancelar a publicação programada ou reagendar sua data/hora de início, mantendo o conteúdo da versão publicada. Essas ações serão registradas no histórico. Se a vigência já tiver começado, não será permitido cancelar ou reagendar aquela versão; uma alteração deverá gerar nova versão.
- No início da vigência programada, a nova versão passa a ser a única vigente e a anterior permanece no histórico, com seu período de vigência encerrado.
- A transição deve ocorrer automaticamente, sem exigir que o nutricionista ou paciente esteja conectado.
- Cada versão deve preservar suas refeições planejadas, alimentos, quantidades, observações/instruções e composição nutricional de referência.
- Registrar autor, data/hora e resumo ou motivo da atualização.
- Cada plano possui uma única versão vigente por vez; versões antigas permanecem consultáveis, respeitando retenção de dados. O versionamento também se aplica a planos pessoais, com fluxo simples de salvar alterações e preservar a versão anterior, sem agendamento de vigência no MVP. Uma versão futura publicada fica em estado programado, não vigente, até sua data de início.
- No MVP, um paciente não pode ter dois planos profissionais vigentes simultaneamente. A atualização da dieta vigente gera uma nova versão; o plano anterior permanece no histórico, sem coexistência de prescrições profissionais ativas.
- Não substituir a versão anterior, pois precisamos reconstruir **qual plano estava vigente em determinado dia**.
- Refeições do diário e seus nutrientes nunca são alterados por atualização de versão do plano.

**Modelo conceitual proposto (não é ainda um esquema físico de banco):**
- `PlanoAlimentar`: identidade, titular, tipo pessoal/profissional e nutricionista responsável, quando aplicável.
- `VersaoPlano`: identificador do plano, número da versão, vigência, autor e estado.
- `RefeicaoPlanejada` / `ItemPlanejado`: estrutura nutricional vinculada a uma versão específica.
- `RefeicaoConsumida` / `ItemConsumido`: diário vinculado ao usuário e ao momento de consumo, **sem dependência obrigatória** de plano ou versão.
- `MetaHistorica`: metas com vigência e origem (usuário, estimativa ou nutricionista).

A independência do diário preserva **o que a pessoa comeu**; o versionamento preserva **o que estava prescrito/planejado**. Uma coisa não substitui a outra.

### 3.6. Gestão de pacientes

- Convite e aceite para vincular paciente e nutricionista, com informação clara sobre os dados acessíveis.
- Apenas um nutricionista ativo por paciente em determinado momento.
- Fluxo explícito para encerrar o vínculo ou transferir o paciente para outro nutricionista.
- Histórico dos vínculos anteriores, sem manter acesso ativo após o encerramento.
- Lista de pacientes vinculados.
- Visualização de todos os dados disponíveis do paciente, condicionada ao vínculo ativo e à autorização vigente.
- Acesso ao diário alimentar e aos indicadores por período.
- Consulta ao histórico de peso.
- Criação e atribuição de planos alimentares.
- Histórico de alterações nos planos.

O modelo de múltiplos nutricionistas simultâneos foi descartado: haverá um único vínculo profissional ativo por paciente. Apenas pacientes adultos poderão ser vinculados no MVP. Ainda precisam ser definidos os detalhes de autorização, a transferência entre profissionais e o acesso a dados históricos após o encerramento.

### 3.7. Assinaturas e monetização

Hipótese comercial inicial:

- Usuários comuns utilizam gratuitamente as funcionalidades pessoais.
- Nutricionistas pagam uma mensalidade para acessar as ferramentas profissionais de gestão e acompanhamento de pacientes.
- O acesso do paciente aos próprios dados e ao diário não deve depender da assinatura ativa do nutricionista.

Ainda precisam ser definidos: limites de pacientes, planos comerciais, período de teste, cobrança, inadimplência, cancelamento e regras de continuidade dos pacientes.

## 4. Princípios de produto

1. **Dois públicos centrais:** experiências pessoal gratuita e profissional paga desenhadas em conjunto.
2. **Planejamento e consumo independentes:** plano alimentar não é o diário alimentar.
3. **Histórico preservado:** metas, medidas, planos e registros alimentares não podem ser alterados retroativamente por mudanças em outros dados.
4. **Planos versionados:** atualizar plano publicado cria nova versão; refeições planejadas pertencem à versão.
5. **Valores históricos estáveis:** registros de consumo mantêm o valor nutricional e sua origem na época do lançamento.
6. **Dados rastreáveis:** fonte, unidade e contexto de preparo dos alimentos devem estar identificáveis.
7. **Permissões explícitas:** nutricionista visualiza dados autorizados somente durante vínculo ativo, com exceções legais de retenção a avaliar.
8. **Direitos dos pacientes:** dados pessoais e diário alimentar não ficam condicionados à assinatura do nutricionista.
9. **Escopo 18+:** apenas adultos podem se cadastrar ou ser acompanhados por nutricionistas na primeira versão; sem perfis dependentes ou módulos pediátricos.
10. **Proteção proporcional:** a restrição 18+ não é uma dispensa automática das obrigações da LGPD e do ECA Digital quando aplicáveis; revisar risco de acesso por menores e aferição de idade antes do lançamento.
11. **Sem diagnósticos automatizados:** indicadores não substituem avaliação profissional.
12. **Bibliotecas independentes:** modelos pessoais e profissionais não alteram registros históricos de consumo ou dietas publicadas.
13. **Padrão semanal previsível:** personalizações de refeições para dias da semana não modificam o cardápio dos demais dias nem as versões anteriores.

## 5. Escopo futuro — não priorizar no primeiro lançamento

- Rede social de nutrição.
- Curtidas, comentários e compartilhamentos.
- Perfis públicos.
- Biblioteca pública de dietas criadas por usuários.
- Recomendações avançadas ou geração automática de dietas.
- Reconhecimento de alimentos por imagem.
- Leitura de códigos de barras.
- Integrações com dispositivos e aplicativos de saúde.

Essas funcionalidades poderão ser avaliadas após validar o valor do diário alimentar e da gestão profissional. Uma rede social também exigirá moderação, denúncia de conteúdo, proteção contra desinformação e controles adicionais de privacidade.

## 6. Riscos e pontos de atenção

- **Privacidade:** dados alimentares e de saúde exigem minimização, proteção e autorização apropriada.
- **Acesso de profissionais:** vínculo com um único nutricionista ativo e autorização não dispensam controle servidor/banco contra acesso indevido a dados de outros pacientes.
- **Restrição etária:** o produto será 18+, com informação clara no cadastro e nos termos. A idade deve ser validada no backend, sem confiar apenas no frontend. A autodeclaração não garante automaticamente que não haverá acesso por menores; revisar o enquadramento de acesso provável, riscos e controles proporcionais à luz de orientações da ANPD, em especial o guia publicado em 28/10/2026.
- **Entrada indevida de menores:** planejar um fluxo seguro para denúncia/identificação, restrição ou encerramento de contas que violem a idade mínima, com atenção à proteção dos dados e direitos aplicáveis. A política não é uma transferência automática de responsabilidade para o usuário.
- **Qualidade nutricional:** fontes podem conter dados ausentes, unidades diferentes e informações específicas de preparo.
- **Histórico:** atualizações em TBCA, dietas e metas não podem alterar retrospectivamente informações antigas.
- **Custos do gratuito:** estimar armazenamento e uso médio antes do lançamento.
- **Viabilidade comercial:** validar se o profissional percebe valor suficiente na gestão e no acompanhamento de pacientes para pagar mensalidade.

## 7. Questões em aberto para a descoberta

1. Quais medidas proporcionais de aferição de idade se aplicam ao aplicativo 18+, conforme análise jurídica antes do lançamento?
2. Como lidar com uma conta identificada como pertencente a menor sem reter ou expor indevidamente dados?
3. Como validar o registro profissional de nutricionista?
4. Como calcular o saldo semanal/mensal, as médias e a tolerância de meta?
5. Quais recursos compõem o plano profissional pago? Haverá limites de pacientes?
6. Quais dados ficam disponíveis depois do encerramento do vínculo profissional?
7. Qual será o fluxo de exportação, portabilidade, retenção e exclusão dos dados?
8. Como será a troca de nutricionista?
9. O usuário comum poderá criar suas próprias categorias personalizadas de refeições?
10. Quais campos adicionais realmente agregam valor ao cadastro das refeições, sem poluir a interface?

**Definições funcionais centrais do módulo de dietas concluídas para planejamento:** criação e atribuição, categorias, composição fixa, horários opcionais, observações, cardápio semanal, múltiplas refeições da mesma categoria, biblioteca, histórico/versionamento e agendamentos. Detalhes de implementação e questões transversais serão tratados nas demais etapas.

## 8. Referências iniciais

- [TBCA — Tabela Brasileira de Composição de Alimentos](https://tbca.net.br/)
- [TACO — 4ª edição, NEPA/UNICAMP](https://nepa.unicamp.br/wp-content/uploads/sites/27/2023/10/taco_4_edicao_ampliada_e_revisada.pdf)
- [Lei nº 8.234/1991 — Regulamentação da profissão de nutricionista](https://www.planalto.gov.br/ccivil_03/leis/1989_1994/l8234.htm)
- [CFN — Nota Técnica nº 82/2023 sobre prescrição dietética](https://www.cfn.org.br/wp-content/uploads/2023/11/AMPLIADA_PRESCRI%C3%87%C3%83O-DIET%C3%89TICA-COMO-ATIVIDADE-PRIVATIVA-DO-NUTRICIONISTA.pdf)
- [LGPD — Lei nº 13.709/2018, art. 14 (crianças e adolescentes)](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
- [ECA Digital — Lei nº 15.211/2025](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2025/lei/l15211.htm)
- [ANPD — Informações oficiais sobre o ECA Digital](https://www.gov.br/anpd/pt-br/assuntos/noticias/eca-digital)
- [ANPD — Guia sobre escopo e obrigações gerais do ECA Digital, publicado em 28/10/2026](https://www.gov.br/anpd/pt-br/assuntos/noticias/anpd-publica-guia-que-esclarece-quem-deve-cumprir-e-quais-sao-as-responsabilidades-das-plataformas-na-protecao-de-criancas-e-adolescentes)
- [Decreto nº 12.880/2026 — Regulamentação do ECA Digital](https://www2.camara.leg.br/legin/fed/decret/2026/decreto-12880-18-marco-2026-798813-publicacaooriginal-178481-pe.html)
- [ANPD — Enunciado CD/ANPD nº 1/2023 sobre bases legais e melhor interesse](https://www.gov.br/anpd/pt-br/assuntos/noticias/anpd-divulga-enunciado-sobre-o-tratamento-de-dados-pessoais-de-criancas-e-adolescentes)
- [Ministério da Saúde — Orientações para avaliação antropométrica](https://www.gov.br/saude/pt-br/composicao/saps/vigilancia-alimentar-e-nutricional/arquivos/orientacoes-para-a-coleta-e-analise-de-dados-antropometricos-em-servicos-de-saude)
- [National Academies — Dietary Reference Intakes for Energy (2023)](https://www.nationalacademies.org/read/26818/chapter/2) — referência técnica que demonstra a necessidade de equações distintas por idade, sexo e atividade; não deve ser adotada automaticamente como padrão clínico brasileiro.

---

**Próximo passo:** resolver as questões de produto em aberto e, a partir delas, produzir os documentos complementares de requisitos, arquitetura, segurança, decisões técnicas e instruções para agentes de IA.