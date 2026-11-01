# Instruções para agentes de IA — Plataforma de Nutrição

> Leia este arquivo antes de propor, escrever ou modificar código.

## Fontes de verdade
1. `REQUIREMENTS.md`: comportamento de produto consolidado do MVP.
2. `ARCHITECTURE.md` e `DATABASE.md`: decisões e invariantes técnicos; esquemas conceituais não são código pronto.
3. `PLAN.md`: ordem de implementação, aceites e bloqueios.
4. `README.md`: visão e status.
5. Documentos temáticos (`IDEA.md`, `ALIMENTOS.md`, `DIARIO.md`, `DIETAS.md`, `PACIENTES.md`, `MONETIZACAO.md`, `ROADMAP.md`), revisados para refletir as mesmas decisões. **Se encontrar qualquer divergência remanescente, parar a funcionalidade afetada, apontar o conflito e não escolher silenciosamente uma versão.**

## Modo de trabalho
- Atuar como engenheiro sênior: questionar inconsistências com evidência; não concordar automaticamente.
- Trabalhar em pequenas fatias, com critérios de aceite e testes; não reescrever arquitetura fora do escopo.
- TypeScript no frontend e backend. Um único repositório; monólito modular; React/Vite, Express 5, Prisma/PostgreSQL, Graphile Worker.
- Priorizar desenvolvimento no computador local. **Não implantar na VPS e não executar migrações em produção sem solicitação específica.**
- Antes de modificar schema, API pública, segurança, dados históricos ou regras de pagamento, apresentar riscos/compatibilidade e validar as premissas.
- Nunca importar dados TBCA sem confirmar o direito de uso do conjunto específico.
- Não adicionar Redis, RabbitMQ, microservices, WebSockets, chat ou app mobile sem novo requisito.

## Segurança e integridade inegociáveis
- Não confiar em campos do frontend para autorização, cálculo nutricional ou titularidade.
- Sessões opacas em cookie HttpOnly e hashes no banco; sem JWT/localStorage para login.
- Acesso sempre scoped por titular e vínculo vigente; gestores só leem diário, não editam.
- Convite aceito/troca de gestor em transação, idempotente sob reenvio e concorrência.
- Snapshot nutricional ao salvar; dados ausentes permanecem desconhecidos; não reescrever o passado a partir do catálogo.
- Não executar testes contra produção. Nunca inserir segredos em código/logs.
- Respeitar idade 18+, privacidade, consentimento e direitos de exclusão.

## Validação mínima por mudança
- Testes Vitest unitários para domínio alterado.
- Testes de integração contra rotas reais e PostgreSQL isolado para persistência, permissões e concorrência.
- Playwright para jornadas principais e regressões visuais/interativas relevantes.
- Typecheck e build.
- Relatar claramente o que foi validado, o que não foi e os comandos executados. Não afirmar que passou sem executar.

## Como reportar
Descrever escopo, decisões aplicadas, arquivos modificados, riscos e evidências dos testes. Identificar explicitamente pendências em vez de inventar soluções ou tomar decisões comerciais/jurídicas em nome do usuário.
