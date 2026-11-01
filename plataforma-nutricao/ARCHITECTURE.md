# Arquitetura e operação — proposta técnica aprovada

> 01/11/2026. Arquitetura planejada; dependências/versões exatas serão validadas na implementação.

## Tecnologias
- Frontend: React + TypeScript + Vite, React Router, TanStack Query, React Hook Form; Fetch nativo; sem Redux inicialmente.
- Backend: Node.js + TypeScript + Express 5, REST JSON, Zod, Prisma.
- Persistência: PostgreSQL; jobs assíncronos duráveis via Graphile Worker no mesmo banco, em processo separado.
- Testes: Vitest unitário e integração com rotas reais e PostgreSQL isolado, Playwright para fluxos essenciais; GitHub Actions valida PRs.
- Infra local: backend/frontend/worker em modo desenvolvimento no computador, PostgreSQL no Docker Compose. Futuro deploy manual na VPS com containers Docker Compose.

## Estrutura pretendida
```text
plataforma-nutricao/
  frontend/
  backend/
    src/
      modules/       # auth, users, foods, diary, plans, relationships, goals, metrics, admin
      jobs/
      shared/        # infraestrutura transversal, sem regras de domínio arbitrárias
    prisma/
  docker-compose.yml
  .env.example
  README.md
```
A pasta atual contém documentos de planejamento; criar código somente após consolidar especificações. Não adotar monorepo-tooling complexo sem necessidade real.

## Limites de confiança e segurança
- Endpoints validam identidade, objeto pertencente a quem pede, vínculo atual e capacidade da operação; nunca confiar em role, preço, gestor_id, calorias ou permissões vindos do navegador.
- Cookie de sessão opaco, seguro, HttpOnly e SameSite; token de sessão **hasheado** no banco. Proteção Origin/CSRF e regras de revogação.
- Senhas Argon2id; sessão normal expira por inatividade em 24 h e por idade absoluta em 30 dias; opção manter conectado 30/90 dias; redefinição revoga sessões anteriores.
- Google OIDC verifica assinatura, audience, issuer e proteção de estado; vinculação de conta deliberada, não automática só por e-mail.
- Todos os dados nutricionais são pessoais; acesso mínimo, auditoria administrativa, logs redigidos, políticas LGPD, exclusão e exportação a especificar antes do beta público.
- Admin é papel separado; não conceder leitura irrestrita do diário.

## Consistência e desempenho
- Mudança de relacionamento, aceite de convite e plano com efeitos múltiplos: transação no banco + índices/constraints; proteção idempotente e concorrência.
- Refeições persistem snapshot nutricional detalhado de alimento/quantidade/referência/resultado; totais por data devem ser derivados ou atualizados transacionalmente com procedimento de reconstrução, sem divergência silenciosa.
- Frontend calcula prévias apenas para UX; backend recalcula a partir dos registros autorizados.
- Busca SQL indexada, paginação, limites e debounce de interface; medir antes de adicionar buscador externo/cache.
- API estável e agnóstica de cliente para permitir app mobile no futuro, sem implementar mobile agora.

## Desenvolvimento, deploy e confiabilidade
- PRs: TypeScript, lint, testes, integração e build no GitHub Actions. Não implantar automaticamente.
- VPS só após MVP validado: HTTPS, migrations controladas, health checks, logs e rollback avaliado segundo migração.
- PostgreSQL não exposto à internet; imagens sem segredos; separar configurações/credenciais por ambiente.
- Backups diários e testes reais de restauração. Cópia apenas no mesmo VPS + transferência manual não basta como proteção de desastre para lançamento público: definir backup externo automático criptografado antes de abrir ao público.
- Monitoramento simples de health, reinícios, disco, conexões, jobs e backups, sem Prometheus/Grafana inicialmente.

## Integrações
- E-mails só para verificação, recuperação e eventos críticos de segurança; interface de provedor substituível, Gmail inicial sujeito a limites e políticas.
- TBCA prioritária apenas depois de verificação documental da licença e atribuição/redistribuição. Importação manual e auditável; JSON é formato, não prova de licença.
