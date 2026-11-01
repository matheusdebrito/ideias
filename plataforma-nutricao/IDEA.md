# Plataforma de Nutrição — visão atual do produto

> Revisado em 01/11/2026. **Planejamento, não software implementado.** Fonte normativa de requisitos: [REQUIREMENTS.md](./REQUIREMENTS.md). Esta visão não estabelece regras adicionais incompatíveis.

## Propósito e público
Plataforma web para adultos (18+) organizarem sua alimentação, registrarem consumo e metas, acompanharem peso, medidas, água e evolução, e, se desejarem, permitirem a outro adulto organizar planos e consultar seus registros mediante autorização. **A plataforma é uma ferramenta de gestão, não um serviço de diagnóstico ou prescrição clínica.**

Qualquer adulto pode exercer a função de gestor independentemente de CRN. A verificação de CRN oferece selo informativo somente a nutricionista efetivamente verificado, não uma nova função, permissão ou requisito para gestão. Uma conta pode simultaneamente acompanhar a própria alimentação, ser acompanhada e gerenciar terceiros. Um titular tem apenas **um gestor ativo** por vez.

## Beta do MVP
Lançar uma fase de testes gratuita por tempo limitado, com funcionalidades pessoais e de gestão liberadas, sem cobrança nem cartão obrigatório. Não estabelecer prazo artificial antecipadamente. Comunicar claramente que as condições poderão mudar no futuro, sem cobrar automaticamente. Após analisar adesão, retenção e custos, decidir preços/limites e implementar pagamentos, fora do beta inicial.

## Jornada pessoal
O usuário verifica seu e-mail, entra, encontra alimentos por busca PostgreSQL ou por recentes/favoritos, informa quantidade consumida, visualiza prévias nutricionais instantâneas, registra refeições livres ou copiadas do plano/modelo salvo, consulta indicadores e faz ajustes retroativos. Pode registrar água, peso e circunferências. Planejamentos são sugestões; o diário sempre representa consumo declarado, não prescrito nem automático.

## Jornada de gestão
Um gestor cria refeições/planos com alimentos, pode usar biblioteca própria, envia convite interno ou link de WhatsApp de uso único; o titular aceita e consente. Gestor ativo pode ler diário e evolução do titular e elaborar planos, **mas não pode alterar ou excluir registros reais de consumo**. Encerramento ou troca revoga o acesso anterior sem remover os dados pessoais do titular.

## Dados nutricionais
TBCA é fonte prioritária condicionada aos direitos de uso; importação manual versionada. Catálogo local com busca textual; variantes preparadas priorizadas. Cadastro privado de alimento com nome, quantidade, proteínas, carboidratos e gorduras obrigatórios; kcal opcionais, estimadas pela regra 4/4/9 se ausentes, com marca de estimativa. Alimentos importados podem ter lacunas; quando registrados, o indicador correspondente deve mostrar cobertura incompleta. Frontend calcula prévia; backend valida e salva snapshot.

## Escopo de engenharia
Monólito modular, TypeScript, React/Vite, Express 5, Prisma/PostgreSQL, Graphile Worker para tarefas duráveis e REST. Desenvolvimento no computador local, Vitest/Playwright e GitHub Actions para validação; somente após validação implantar manualmente na VPS com Docker Compose, HTTPS, backups externos e restauração testada.

## Limites
Sem chat interno, WebSockets, app mobile, motores de receitas, produtos pagos ou infraestrutura distribuída no MVP. Rever LGPD, licença TBCA, política de dados e critérios CRN antes de lançamento.

Referências: [REQUIREMENTS](./REQUIREMENTS.md) · [ARCHITECTURE](./ARCHITECTURE.md) · [DATABASE](./DATABASE.md) · [PLAN](./PLAN.md).
