# Plano incremental de implementação do MVP

> 01/11/2026. Nenhuma fase foi implementada; desenvolvimento inicial **somente local**. Avançar por critérios de aceite, não por pressão de cronograma.

## Fase 0 — Fechar bloqueios de produto e engenharia
- Revisar divergências dos documentos anteriores versus `REQUIREMENTS.md` (ver abaixo).
- Verificar licença/redistribuição comercial TBCA antes de copiar dados.
- Definir política mínima de privacidade, consentimento para vínculo, retenção e tratamento de exclusão.
- Desenhar schema inicial e contratos REST com exemplos de erro, autorização e datas/fusos.
- Definir a primeira fatia vertical: cadastro/login -> buscar alimento -> registrar refeição -> visualizar totais.

**Aceite:** decisões bloqueantes documentadas; nenhuma importação com licença incerta.

## Fase 1 — Fundamentos locais
Criar estrutura TS React/Express/Postgres/Prisma, Docker Compose local, configuração de teste, migrações iniciais, Vitest, Playwright mínimo, GitHub Actions (typecheck/test/build). Sem VPS.

**Aceite:** build reproduzível, testes conectam apenas ao Postgres isolado, sem segredos versionados.

## Fase 2 — Contas e segurança
Cadastro 18+, senha/verificação, recuperação, sessões, Google OIDC se compatível com escopo/custos, perfis, isolamento entre usuários.

**Aceite:** request specs cobrem acesso indevido, CSRF, login/logout, tokens de uso único, expirados e duplicados.

## Fase 3 — Catálogo e cálculos
Importação TBCA versionada **após autorização de uso**; pesquisa SQL, alimentos privados, favoritos/recentes, medidas confiáveis, cálculos híbridos, snapshots, cobertura de nutrientes incompletos.

**Aceite:** cálculo consistente nas duas camadas; usuário não acessa alimento privado de outro; valores ausentes não viram zero; duplicatas de importação não proliferam.

## Fase 4 — Diário e indicadores
Registros livres, edição e exclusão retroativa, refeições salvas, água, metas por vigência, resumo diário/semana/mês e evolução de peso/circunferências.

**Aceite:** editar data corrige ambos os dias; total é idempotente mesmo sob reenvio; histórico preserva snapshots e fuso.

## Fase 5 — Planos e gestão
Plano editável/versionado, templates privados, convites in-app e WhatsApp, vínculo único, troca segura, visualização restrita do gestor, notificações.

**Aceite:** teste concorrente nunca cria dois gestores ativos; gestor anterior perde acesso; gestor não pode editar diário.

## Fase 6 — Beta local validado
Testes E2E dos fluxos principais, concorrência, performance de busca, acessibilidade/responsividade, segurança, observabilidade, testes de restauração e regressão. Preparar instruções de configuração e operação.

**Aceite:** critérios de `REQUIREMENTS.md` atendidos e lacunas documentadas.

## Fase 7 — Preparar VPS e publicação
Somente após o MVP passar na validação: Docker Compose de produção, proxy HTTPS, backups externos automáticos e restauração testada, migrações controladas, logs, health, monitoramento, checagem LGPD/licenças. Deploy manual.

**Aceite:** smoke test real, rollback documentado, backup recuperável, sem banco aberto publicamente.

## Pendências comerciais
Durante beta: todos os recursos gratuitos, sem cobrança. Antes de monetizar: medir custos e retenção (7/30 dias), definir preço/limite, provedor de cobrança, política de redução/suspensão, comunicação prévia; sem inserir pagamento no MVP por antecipação.

## Divergências identificadas ao iniciar consolidação (documentos legados)
1. `ALIMENTOS.md` exige kcal no alimento manual; **decisão recente: kcal opcional com estimativa 4/4/9**.
2. `ALIMENTOS.md` bloqueia registro de alimentos importados sem os quatro nutrientes essenciais; **decisão recente: permitir uso com total incompleto explicitamente sinalizado**.
3. `ROADMAP.md`/textos anteriores descrevem assinatura mensal e condições de pagamento como escopo inicial; **decisão recente: beta gratuito, monetização adiada**.
4. `DIARIO.md` propõe histórico discreto de correções; **decisão recente: não manter histórico completo de versões**, mas auditoria mínima/obrigações legais ainda devem ser especificadas.
5. `ROADMAP.md` e `DIETAS.md` contêm regras mais complexas de agendamento, dietas futuras e gestão comercial; revisar caso a caso e excluir do MVP o que conflitar com plano atual editável simples.
6. `ALIMENTOS.md` dizia que origem TBCA/TACO/JSON estava em aberto; **decisão recente: TBCA prioritária**, condicionado à licença.

Não atualizar os legados em massa sem revisão; manter rastreabilidade da origem das decisões.
