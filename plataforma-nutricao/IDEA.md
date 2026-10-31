# Plataforma de Nutrição — Documento de ideia

> **Status:** descoberta do produto  
> **Versão:** 0.1  
> **Última atualização:** 30/10/2026  
> **Este documento registra a visão inicial. Não é uma especificação pronta para implementação.** Decisões ainda não validadas estão marcadas como questões em aberto.

## 1. Visão do produto

Criar uma plataforma de nutrição com duas experiências integradas:

1. **Autonomia alimentar:** permitir que pessoas registrem o que comem, acompanhem a composição nutricional da alimentação, organizem metas pessoais e consultem seu histórico.
2. **Ferramenta profissional:** permitir que nutricionistas gerenciem pacientes, acompanhem informações compartilhadas por eles e elaborem planos alimentares individualizados.

O mesmo usuário deve poder utilizar os recursos pessoais e, se também for nutricionista, os recursos profissionais sem precisar manter contas desconectadas.

A plataforma deve ser apresentada como ferramenta de organização e acompanhamento alimentar. Ela não deve ser posicionada como substituta de avaliação ou acompanhamento profissional, nem realizar por conta própria prescrição dietética para tratar condições de saúde.

## 2. Públicos e perfis

### 2.1. Usuário que acompanha a própria alimentação

Pessoa que deseja registrar refeições, consultar informações nutricionais, acompanhar peso e observar o consumo em relação a metas definidas por ela.

Possíveis necessidades:
- Consultar alimentos e seus nutrientes.
- Registrar refeições e quantidades consumidas.
- Visualizar calorias e macronutrientes por refeição e por dia.
- Consultar históricos diário, semanal e mensal.
- Registrar peso ao longo do tempo.
- Consultar IMC, com explicação dos limites desse indicador.
- Organizar um plano alimentar pessoal, conforme o escopo que ainda será definido.

### 2.2. Paciente acompanhado por nutricionista

Usuário que autoriza um nutricionista a acessar determinados dados da sua conta.

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

As regras de interpretação do IMC dependem da faixa etária. O público inicial (adultos ou também crianças e adolescentes) ainda precisa ser definido.

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

O cálculo automático de metas energéticas não está confirmado no escopo inicial.

### 3.5. Planos alimentares e dietas

A plataforma deverá distinguir dois conceitos:

**Plano pessoal:** organização alimentar criada pelo próprio usuário, dentro dos limites de funcionalidade que serão definidos.

**Plano profissional:** plano elaborado por nutricionista no contexto do acompanhamento de um paciente.

Regras propostas para o plano profissional:
- O nutricionista cria e atribui o plano ao paciente.
- O paciente pode consultar o plano, mas não alterar sua prescrição.
- O nutricionista pode atualizar o plano a qualquer momento.
- Cada atualização gera uma nova versão.
- As versões anteriores permanecem consultáveis pelo profissional, conforme as regras de retenção.
- O diário alimentar do paciente permanece independente do plano, para registrar o que foi realmente consumido.

A edição de um plano pelo nutricionista não deve alterar retroativamente o histórico de versões anteriores.

### 3.6. Gestão de pacientes

- Convite e aceite explícito para vincular paciente e nutricionista.
- Lista de pacientes vinculados.
- Visualização dos dados compartilhados pelo paciente.
- Acesso ao diário alimentar e aos indicadores por período.
- Consulta ao histórico de peso.
- Criação e atribuição de planos alimentares.
- Histórico de alterações nos planos.

O modelo de múltiplos nutricionistas por paciente, os níveis de permissão e o procedimento de encerramento de vínculo ainda precisam ser definidos.

### 3.7. Assinaturas e monetização

Hipótese comercial inicial:

- Usuários comuns utilizam gratuitamente as funcionalidades pessoais.
- Nutricionistas pagam uma mensalidade para acessar as ferramentas profissionais de gestão e acompanhamento de pacientes.
- O acesso do paciente aos próprios dados e ao diário não deve depender da assinatura ativa do nutricionista.

Ainda precisam ser definidos: limites de pacientes, planos comerciais, período de teste, cobrança, inadimplência, cancelamento e regras de continuidade dos pacientes.

## 4. Princípios de produto

1. **Separar planejamento de registro:** plano alimentar e diário alimentar são conceitos diferentes.
2. **Preservar histórico:** mudanças de peso, metas e planos não devem apagar o estado anterior.
3. **Rastrear a origem nutricional:** todo valor nutricional deve ter fonte e unidade identificáveis.
4. **Permissões explícitas:** um nutricionista só acessa dados de pacientes com vínculo válido e autorização apropriada.
5. **Dados do usuário não podem ser reféns da assinatura profissional:** o paciente mantém acesso aos seus dados pessoais.
6. **Segurança e privacidade desde o início:** dados de saúde exigem tratamento cuidadoso e conformidade com a LGPD.
7. **Não confundir indicador com diagnóstico:** IMC e comparação de calorias não representam avaliação nutricional completa.
8. **Não automatizar prescrição profissional sem validação adequada:** recursos de recomendação devem ter escopo, limites e revisão apropriados.

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
- **Limites profissionais:** prescrição dietética e assistência nutricional têm regras próprias no Brasil. A experiência de auto-organização alimentar precisa ser diferenciada da prescrição profissional.
- **Qualidade da base:** fontes podem ter cobertura, idade, métodos e nutrientes diferentes.
- **Unidades e preparo:** erros em porções, densidade, estado cru/cozido e conversões comprometem os totais.
- **Histórico:** alterações em metas, alimentos e dietas não podem modificar indevidamente resultados passados.
- **Custos do modelo gratuito:** é necessário estimar infraestrutura e suporte para usuários não pagantes.
- **Adoção profissional:** o produto precisa resolver um problema real do fluxo de trabalho do nutricionista, não apenas oferecer uma tela de cadastro de pacientes.

## 7. Questões em aberto para a descoberta

1. O usuário poderá apenas montar um plano pessoal com alimentos e quantidades escolhidos por ele ou a plataforma também calculará/recomendará quantidades e metas nutricionais?
2. O público inicial será exclusivamente adulto?
3. A meta calórica será sempre informada pelo usuário/profissional ou haverá cálculo estimado?
4. Um paciente poderá estar vinculado a mais de um nutricionista simultaneamente?
5. Quais informações o nutricionista poderá consultar e quais dependerão de autorização específica?
6. O plano alimentar terá horários, refeições, alimentos, quantidades, substituições e orientações?
7. Como será definido o saldo semanal e mensal?
8. Quais recursos serão gratuitos e quais farão parte da assinatura profissional?
9. Como validar o registro profissional do nutricionista?
10. Qual será o fluxo de exportação, portabilidade, retenção e exclusão dos dados?

## 8. Referências iniciais

- [TBCA — Tabela Brasileira de Composição de Alimentos](https://tbca.net.br/)
- [TACO — 4ª edição, NEPA/UNICAMP](https://nepa.unicamp.br/wp-content/uploads/sites/27/2023/10/taco_4_edicao_ampliada_e_revisada.pdf)
- [Lei nº 8.234/1991 — Regulamentação da profissão de nutricionista](https://www.planalto.gov.br/ccivil_03/leis/1989_1994/l8234.htm)
- [CFN — Nota Técnica nº 82/2023 sobre prescrição dietética](https://www.cfn.org.br/wp-content/uploads/2023/11/AMPLIADA_PRESCRI%C3%87%C3%83O-DIET%C3%89TICA-COMO-ATIVIDADE-PRIVATIVA-DO-NUTRICIONISTA.pdf)
- [ANPD — Perguntas frequentes sobre LGPD](https://www.gov.br/anpd/pt-br/acesso-a-informacao/perguntas-frequentes)

---

**Próximo passo:** resolver as questões de produto em aberto e, a partir delas, produzir os documentos complementares de requisitos, arquitetura, segurança, decisões técnicas e instruções para agentes de IA.