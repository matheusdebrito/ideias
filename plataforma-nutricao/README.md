# Plataforma de Nutrição — consolidação do MVP

> Status: planejamento; **nenhum software foi implementado**. Consolidação em 01/11/2026.
>
> Em caso de conflito com documentos anteriores nesta pasta, **as decisões explícitas de `REQUIREMENTS.md` prevalecem para o MVP**. Itens em aberto não devem ser convertidos em requisitos silenciosamente.

## Visão
Aplicação web para adultos (18+) registrarem alimentação, metas, hidratação e evolução corporal; também permite que pessoas autorizadas organizem planos alimentares de outros usuários. **A plataforma não presta diagnóstico nem prescrição clínica.** Gerir outro usuário não exige CRN. Nutricionistas poderão ter selo opcional de CRN verificado.

## Princípios
- Uma conta pode usar recursos pessoais e gerenciar outros usuários.
- Cada titular tem no máximo um gestor ativo, por consentimento expresso, e mantém a propriedade de seus dados.
- Diário registra consumo efetivo; plano alimentar é uma sugestão independente.
- Primeiro lançamento: beta gratuito por tempo limitado, com funcionalidades abertas, sem cobrança automática.
- Desenvolvimento inteiramente local até o MVP estar validado; VPS e implantação pública depois.
- Simplicidade: monólito modular TypeScript, PostgreSQL, REST, Docker Compose; sem chat, WebSockets, Redis ou microsserviços no MVP.

## Mapa da documentação
- [REQUIREMENTS.md](REQUIREMENTS.md): regras do produto e critérios de aceitação.
- [ARCHITECTURE.md](ARCHITECTURE.md): componentes, segurança e operação.
- [DATABASE.md](DATABASE.md): modelo conceitual e invariantes.
- [PLAN.md](PLAN.md): fases de execução, testes e bloqueios.
- [AGENTS.md](AGENTS.md): orientações para agentes e implementação.
- Documentos preexistentes `IDEA.md`, `ALIMENTOS.md`, `DIARIO.md`, `DIETAS.md`, `PACIENTES.md`, `MONETIZACAO.md`, `ROADMAP.md`: contexto detalhado histórico, **a conciliar antes de implementação de pontos divergentes**.

## Fora do lançamento inicial
Pagamentos reais, cobrança recorrente, pesquisa pública de profissionais, chat interno, e-mails de convite, receitas, compartilhamento público de alimentos, rastreamento de exercícios, fotos e app mobile.

## Pendências bloqueantes para lançamento público
Confirmar licença/uso comercial da TBCA; revisão jurídica/LGPD e consentimentos; critérios operacionais de verificação CRN; política de retenção e eliminação; backup **fora da VPS** automatizado com restauração testada; estimativas de custos e estratégia da futura monetização. A integração de pagamento não integra o beta.
