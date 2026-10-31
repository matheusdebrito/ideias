# Plataforma de Nutrição — Documento de ideia

> **Status:** descoberta do produto  
> **Versão:** 0.3  
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
- Crianças e adolescentes poderão ter acompanhamento profissional dentro do produto, mas a aplicação não interpretará automaticamente o IMC deles.

**Decisão de produto:** não criar perfis dependentes nem exigir uma conta de responsável como funcionalidade de produto. Para menores admitidos na plataforma, considerar contas individuais. O produto não tem como público prioritário crianças muito pequenas; a idade mínima e o modo de ingresso de menores ainda precisam ser definidos.

**Ressalva obrigatória:** a opção por não criar perfis dependentes não dispensa a análise jurídica de proteção de crianças e adolescentes, mecanismos proporcionais de aferição de idade e eventuais medidas de participação/autorização de responsáveis. Essas obrigações precisam ser verificadas antes do lançamento, considerando LGPD e ECA Digital.

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

### 3.4. Metas e indicadores de consumo

- Cadastro de metas calóricas e acompanhamento do consumo.
- Comparação do consumo registrado com a meta válida naquele dia, sem tratar isso como diagnóstico.
- Exibição de calorias e macronutrientes por refeição e total diário.
- Consulta de períodos anteriores, preservando as metas vigentes à época.
- Resumos semanais e mensais. Definições de saldo, média e tolerância em aberto.

**Adultos:** a aplicação poderá estimar uma meta energética por método validado e permitir ajustes manuais. O cálculo requer dados suficientes, explicação dos limites e revisão de profissionais antes do lançamento.

**Crianças e adolescentes:** a aplicação **não calcula nem recomenda automaticamente necessidades energéticas ou metas calóricas**, nem interpreta IMC infantil. As metas, quando necessárias, serão definidas pelo nutricionista; o app poderá apresentar o acompanhamento dos valores informados pelo profissional. Não sugerir emagrecimento por conta própria para menores.

### 3.5. Planos alimentares e dietas

**Plano pessoal:** o usuário pode montar seus próprios planos com refeições, alimentos e quantidades. Isso é organização alimentar, não prescrição clínica automática.

**Plano profissional:** o nutricionista elabora e atribui planos a pacientes. O paciente pode consultar o plano profissional, mas não alterar sua prescrição.

**Separação entre planejamento e consumo:**
- Refeições previstas em uma dieta representam **o plano**.
- Refeições registradas no diário representam **o consumo real**.
- O diário é independente das dietas e de suas versões.
- A comparação entre plano e diário poderá identificar diferenças, sem alterar nenhum dos registros.

**Versionamento do plano:**
- Um rascunho pode ser editado antes da publicação.
- Ao atualizar um plano publicado, criar uma nova versão com data de início de vigência.
- Cada versão deve preservar suas refeições planejadas, alimentos, quantidades, orientações e composição nutricional de referência.
- Registrar autor, data/hora e resumo ou motivo da atualização.
- Cada plano possui uma única versão vigente por vez; versões antigas permanecem consultáveis, respeitando retenção de dados.
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

O modelo de múltiplos nutricionistas simultâneos foi descartado: haverá um único vínculo profissional ativo por paciente. Ainda precisam ser definidos os detalhes de autorização, a transferência entre profissionais, o acesso a dados históricos após o encerramento e os fluxos para pacientes menores de idade.

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
9. **Proteção de dados e menores:** LGPD e ECA Digital considerados desde o projeto; sem criar perfis dependentes por padrão.
10. **Cálculos pediátricos fora da automação:** metas e avaliação infantil serão realizadas por nutricionistas.
11. **Sem diagnósticos automatizados:** indicadores não substituem avaliação profissional.

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
- **Menores:** não haverá perfis dependentes, mas obrigações legais sobre idade, privacidade, segurança e responsável devem ser avaliadas. A exigência de vinculação de conta de até 16 anos no ECA Digital está no capítulo de redes sociais; não presumir aplicação automática de todas essas regras a um diário nutricional sem análise.
- **Público pediátrico:** a aplicação não calculará automaticamente metas ou recomendações para menores, que ficarão a cargo de nutricionistas. Definir faixa etária mínima e forma de ingresso.
- **Qualidade nutricional:** fontes podem conter dados ausentes, unidades diferentes e informações específicas de preparo.
- **Histórico:** atualizações em TBCA, dietas e metas não podem alterar retrospectivamente informações antigas.
- **Custos do gratuito:** estimar armazenamento e uso médio antes do lançamento.
- **Viabilidade comercial:** validar se o profissional percebe valor suficiente na gestão e no acompanhamento de pacientes para pagar mensalidade.

## 7. Questões em aberto para a descoberta

1. Qual idade mínima será admitida na aplicação? Menores ingressarão apenas mediante convite de nutricionista?
2. Como satisfazer exigências aplicáveis de aferição de idade e participação/autorização de responsáveis **sem perfis dependentes**? Requer validação jurídica.
3. Como validar o registro profissional do nutricionista?
4. Como calcular o saldo semanal/mensal, as médias e a tolerância de meta?
5. Quais recursos compõem o plano profissional pago? Haverá limites de pacientes?
6. Quais dados ficam disponíveis depois do encerramento do vínculo profissional?
7. Qual será o fluxo de exportação, portabilidade, retenção e exclusão dos dados?
8. Quais campos e alternativas estarão disponíveis em um plano: horários, substituições, orientações?
9. Como será o processo de troca de nutricionista?
10. O versionamento dos planos pessoais deverá ter o mesmo detalhamento do profissional?

## 8. Referências iniciais

- [TBCA — Tabela Brasileira de Composição de Alimentos](https://tbca.net.br/)
- [TACO — 4ª edição, NEPA/UNICAMP](https://nepa.unicamp.br/wp-content/uploads/sites/27/2023/10/taco_4_edicao_ampliada_e_revisada.pdf)
- [Lei nº 8.234/1991 — Regulamentação da profissão de nutricionista](https://www.planalto.gov.br/ccivil_03/leis/1989_1994/l8234.htm)
- [CFN — Nota Técnica nº 82/2023 sobre prescrição dietética](https://www.cfn.org.br/wp-content/uploads/2023/11/AMPLIADA_PRESCRI%C3%87%C3%83O-DIET%C3%89TICA-COMO-ATIVIDADE-PRIVATIVA-DO-NUTRICIONISTA.pdf)
- [LGPD — Lei nº 13.709/2018, art. 14 (crianças e adolescentes)](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
- [ECA Digital — Lei nº 15.211/2025](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2025/lei/l15211.htm)
- [ANPD — Informações oficiais sobre o ECA Digital](https://www.gov.br/anpd/pt-br/assuntos/noticias/eca-digital)
- [ANPD — Enunciado CD/ANPD nº 1/2023 sobre bases legais e melhor interesse](https://www.gov.br/anpd/pt-br/assuntos/noticias/anpd-divulga-enunciado-sobre-o-tratamento-de-dados-pessoais-de-criancas-e-adolescentes)
- [Ministério da Saúde — Orientações para avaliação antropométrica](https://www.gov.br/saude/pt-br/composicao/saps/vigilancia-alimentar-e-nutricional/arquivos/orientacoes-para-a-coleta-e-analise-de-dados-antropometricos-em-servicos-de-saude)
- [National Academies — Dietary Reference Intakes for Energy (2023)](https://www.nationalacademies.org/read/26818/chapter/2) — referência técnica que demonstra a necessidade de equações distintas por idade, sexo e atividade; não deve ser adotada automaticamente como padrão clínico brasileiro.

---

**Próximo passo:** resolver as questões de produto em aberto e, a partir delas, produzir os documentos complementares de requisitos, arquitetura, segurança, decisões técnicas e instruções para agentes de IA.