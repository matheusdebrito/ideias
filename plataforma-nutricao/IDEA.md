# Plataforma de Nutrição — Documento de ideia

> **Status:** descoberta do produto  
> **Versão:** 0.2  
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

Usuário que autoriza um nutricionista a acessar os dados da sua conta durante um vínculo profissional ativo. O produto permitirá um único nutricionista ativo por paciente de cada vez.

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
- Consultar dados que os pacientes autorizaram compartilhar.
- Acompanhar refeições, consumo energético, macronutrientes, peso e histórico.
- Elaborar planos alimentares e atribuí-los a pacientes.
- Atualizar planos e consultar versões anteriores.
- Acompanhar a diferença entre o plano prescrito e a alimentação registrada pelo paciente.

A validação das credenciais profissionais e os requisitos específicos para o perfil de nutricionista ainda precisam ser definidos.

## 3. Módulos funcionais propostos

### 3.1. Perfil e dados antropométricos

- Cadastro de idade/data de nascimento, sexo e altura.
- Registro de peso com data da medição.
- Histórico de peso, sem sobrescrever medições anteriores.
- Cálculo e apresentação do IMC.
- Exibição do contexto e das limitações do IMC.

O produto deverá atender adultos, crianças e adolescentes. Para menores de idade, a experiência precisa considerar responsáveis legais, proteção reforçada de dados e fluxos apropriados à idade. O IMC adulto não deve ser aplicado indiscriminadamente a crianças e adolescentes: a avaliação deve utilizar referências por idade e sexo e metodologia validada para a faixa etária. O escopo para bebês e primeira infância também deverá ser explicitamente definido.

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

- Cadastro de refeições com data e horário.
- Organização por momentos de alimentação, como café da manhã, almoço, jantar e lanches.
- Inclusão de alimentos e quantidades consumidas.
- Cálculo nutricional de cada item.
- Totalização por refeição e por dia.
- Consulta e edição do histórico, respeitando a preservação dos dados necessários para auditoria e histórico.

O diário deve representar **o que a pessoa efetivamente consumiu**, e não o que estava planejado em sua dieta.

### 3.4. Metas e indicadores de consumo

- Permitir cadastrar uma meta calórica diária.
- Comparar o consumo registrado com a meta.
- Mostrar se o consumo está abaixo, dentro ou acima da meta, sem transformar essa comparação em diagnóstico de saúde.
- Exibir calorias, carboidratos, proteínas e gorduras por refeição e no total diário.
- Consultar períodos anteriores, mantendo a meta que era válida em cada período.
- Apresentar resumos semanais e mensais.

A definição matemática de saldo semanal e mensal ainda está em aberto. A interface deverá diferenciar consumo total, média diária e diferença em relação à meta, evitando interpretações equivocadas de compensação entre dias.

A meta energética será estimada automaticamente e poderá ser ajustada manualmente, com validações. O cálculo não poderá usar uma única equação para todas as idades: deverá selecionar métodos de referência adequados à faixa etária, sexo, medidas e demais dados necessários. Para crianças e adolescentes, a estimativa precisa considerar crescimento e desenvolvimento, e não deve ser apresentada como prescrição ou recomendação de emagrecimento. Se não houver dados suficientes ou método validado para um perfil, a aplicação deverá informar que não pode estimar a meta em vez de inventar um valor. As fórmulas, limites e mensagens deverão ser validados por nutricionistas antes da disponibilização.

### 3.5. Planos alimentares e dietas

A plataforma deverá distinguir dois conceitos:

**Plano pessoal:** organização alimentar criada livremente pelo próprio usuário, com seleção de refeições, alimentos e quantidades. O sistema calcula a composição nutricional com base no catálogo, mas não deve confundir esse recurso com prescrição clínica.

**Plano profissional:** plano elaborado por nutricionista no contexto do acompanhamento de um paciente.

Regras para planos pessoais e profissionais:
- O usuário pode criar e atualizar seus próprios planos pessoais.
- O nutricionista cria e atribui planos profissionais aos pacientes.
- O paciente pode consultar o plano profissional vigente, mas não alterar seu conteúdo prescrito.
- Atualizar um plano publicado significa criar uma nova versão, não sobrescrever a versão anterior.
- Cada versão publicada deve preservar uma fotografia imutável dos alimentos, quantidades, refeições e orientações daquela versão.
- Registrar autor da alteração, data/hora, motivo ou resumo das mudanças e período de vigência.
- Manter uma única versão vigente por plano; versões anteriores permanecem no histórico.
- Permitir rascunhos editáveis antes da publicação, sem modificar versões já publicadas.
- O diário alimentar permanece independente do plano, registrando o que foi realmente consumido.

Uma atualização não pode alterar retroativamente a dieta que estava vigente em um período anterior. O modelo de versionamento deverá ser aplicado também aos planos pessoais, caso o histórico seja relevante para o usuário.

### 3.6. Gestão de pacientes

- Convite e aceite para vincular paciente e nutricionista, com informação clara sobre os dados acessíveis.
- Apenas um nutricionista ativo por paciente em determinado momento.
- Fluxo explícito para encerrar o vínculo ou transferir o paciente para outro nutricionista.
- Histórico dos vínculos anteriores, sem manter acesso ativo após o encerramento.
- Lista de pacientes vinculados.
- Visualização dos dados autorizados pelo paciente.
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

1. **Separar planejamento de registro:** plano alimentar e diário alimentar são conceitos diferentes.
2. **Dois públicos centrais:** a experiência pessoal gratuita e a experiência profissional paga devem ser planejadas desde o início.
3. **Versionar planos:** uma atualização publicada cria nova versão e preserva as anteriores.
4. **Preservar histórico:** mudanças de peso, metas e planos não devem apagar o estado anterior.
5. **Rastrear a origem nutricional:** todo valor nutricional deve ter fonte e unidade identificáveis.
6. **Permissões explícitas:** um nutricionista só acessa dados de pacientes com vínculo ativo e autorização apropriada.
7. **Dados do usuário não podem ser reféns da assinatura profissional:** o paciente mantém acesso aos seus dados pessoais.
8. **Segurança e privacidade desde o início:** dados de saúde exigem tratamento cuidadoso e conformidade com a LGPD.
9. **Proteção de menores:** o produto deve respeitar o melhor interesse de crianças e adolescentes e implementar fluxos apropriados para responsáveis e dados sensíveis.
10. **Não confundir indicador com diagnóstico:** IMC e comparação de calorias não representam avaliação nutricional completa.
11. **Cálculos por faixa etária:** metas energéticas e avaliação antropométrica precisam utilizar métodos adequados à idade e validados por profissionais.
12. **Não automatizar prescrição profissional sem validação adequada:** recursos de recomendação devem ter escopo, limites e revisão apropriados.

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

- **Segurança e privacidade:** dados pessoais e informações de saúde podem ser dados pessoais sensíveis pela LGPD.
- **Menores de idade:** atender crianças e adolescentes amplia significativamente a responsabilidade de produto, privacidade e segurança. Será necessário definir quem cria e administra o perfil, como o responsável comprova sua autoridade, quais dados são coletados, como o menor participa e como funciona o acesso do nutricionista. A LGPD exige que o tratamento observe o melhor interesse de crianças e adolescentes; a base legal aplicável deve ser avaliada juridicamente para cada finalidade.
- **Cálculos pediátricos:** estimativas de energia e indicadores de crescimento exigem métodos próprios por idade e sexo, dados de entrada confiáveis e validação de nutricionistas. Não reutilizar automaticamente fórmulas ou faixas de IMC para adultos.
- **Limites profissionais:** prescrição dietética e assistência nutricional têm regras próprias no Brasil. A experiência de auto-organização alimentar precisa ser diferenciada da prescrição profissional.
- **Qualidade da base:** fontes podem ter cobertura, idade, métodos e nutrientes diferentes.
- **Unidades e preparo:** erros em porções, densidade, estado cru/cozido e conversões comprometem os totais.
- **Histórico:** alterações em metas, alimentos e dietas não podem modificar indevidamente resultados passados.
- **Custos do modelo gratuito:** é necessário estimar infraestrutura e suporte para usuários não pagantes.
- **Adoção profissional:** o produto precisa resolver um problema real do fluxo de trabalho do nutricionista, não apenas oferecer uma tela de cadastro de pacientes.

## 7. Questões em aberto para a descoberta

1. Como será a estrutura de conta para menores: perfil dependente criado pelo responsável, conta própria do menor vinculada ao responsável ou outro modelo?
2. Qual idade mínima será permitida para login próprio? Bebês e crianças pequenas terão apenas perfis dependentes?
3. Quais dados e ações poderão ser administrados pelo responsável e quais poderão ser acessados pelo próprio adolescente?
4. Como validar o registro profissional do nutricionista?
5. Como será definido o saldo semanal e mensal?
6. Quais recursos serão gratuitos e quais farão parte da assinatura profissional?
7. Quais dados serão mantidos disponíveis ao paciente após o encerramento do vínculo com o nutricionista?
8. Qual será o fluxo de exportação, portabilidade, retenção e exclusão dos dados?
9. Quais campos compõem um plano alimentar: horários, refeições, alimentos, quantidades, substituições e orientações?
10. Como será o fluxo de transferência de um paciente de um nutricionista para outro?

## 8. Referências iniciais

- [TBCA — Tabela Brasileira de Composição de Alimentos](https://tbca.net.br/)
- [TACO — 4ª edição, NEPA/UNICAMP](https://nepa.unicamp.br/wp-content/uploads/sites/27/2023/10/taco_4_edicao_ampliada_e_revisada.pdf)
- [Lei nº 8.234/1991 — Regulamentação da profissão de nutricionista](https://www.planalto.gov.br/ccivil_03/leis/1989_1994/l8234.htm)
- [CFN — Nota Técnica nº 82/2023 sobre prescrição dietética](https://www.cfn.org.br/wp-content/uploads/2023/11/AMPLIADA_PRESCRI%C3%87%C3%83O-DIET%C3%89TICA-COMO-ATIVIDADE-PRIVATIVA-DO-NUTRICIONISTA.pdf)
- [LGPD — Lei nº 13.709/2018, art. 14 (crianças e adolescentes)](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
- [ANPD — Enunciado CD/ANPD nº 1/2023 sobre bases legais e melhor interesse](https://www.gov.br/anpd/pt-br/assuntos/noticias/anpd-divulga-enunciado-sobre-o-tratamento-de-dados-pessoais-de-criancas-e-adolescentes)
- [Ministério da Saúde — Orientações para avaliação antropométrica](https://www.gov.br/saude/pt-br/composicao/saps/vigilancia-alimentar-e-nutricional/arquivos/orientacoes-para-a-coleta-e-analise-de-dados-antropometricos-em-servicos-de-saude)
- [National Academies — Dietary Reference Intakes for Energy (2023)](https://www.nationalacademies.org/read/26818/chapter/2) — referência técnica que demonstra a necessidade de equações distintas por idade, sexo e atividade; não deve ser adotada automaticamente como padrão clínico brasileiro.

---

**Próximo passo:** resolver as questões de produto em aberto e, a partir delas, produzir os documentos complementares de requisitos, arquitetura, segurança, decisões técnicas e instruções para agentes de IA.