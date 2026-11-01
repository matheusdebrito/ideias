# Roadmap atual — Plataforma de Nutrição

> Revisado em 01/11/2026. Planejamento consolidado; software ainda não implementado. Detalhamento operacional em [PLAN.md](./PLAN.md).

| Sequência | Etapa | Situação |
|---|---|---|
| 1 | Visão, público adulto e regras de gestão | Decisões principais consolidadas |
| 2 | Diário, alimentos, planos, metas e evolução | Decisões principais consolidadas |
| 3 | Stack, dados, segurança e desenvolvimento local | Decisões principais consolidadas |
| 4 | Documentos-guia e reconciliação das regras legadas | Documentado; revisar requisitos sensíveis antes do código |
| 5 | Infra local e testes automatizados | Não iniciado |
| 6 | Autenticação e isolamento | Não iniciado |
| 7 | Catálogo, cálculos híbridos e alimentos privados | Não iniciado |
| 8 | Diário e indicadores | Não iniciado |
| 9 | Planos, acompanhamento e convites | Não iniciado |
| 10 | Testes do MVP e publicação manual na VPS | Não iniciado |

## Escolhas atuais que anulam planos antigos
- **Beta inicial gratuito para todos os recursos**; não construir checkout nem impor assinatura antes da monetização.
- **TBCA prioritária, sujeita à licença**, não decisão indiferente entre TBCA/TACO.
- **Kcal opcionais para cadastro privado**, estimar 4/4/9 quando não informadas; macros, nome e porção obrigatórios.
- **Alimento institucional incompleto pode ser usado**, com indicador de nutrientes parcialmente conhecidos.
- **Sem histórico completo de versões das refeições**; preservar registros de consumo e correções por data, com auditoria mínima quando necessária.
- **Plano atual editável e versionado**, sem múltiplos planos programados complexos no MVP.
- **Convites internos e via WhatsApp**, não por e-mail; sem chat.
- **Desenvolvimento no computador local**, GitHub Actions para verificação e VPS apenas quando estiver pronto, com deploy manual.

## Riscos que impedem publicação pública
Confirmação documental dos termos da base nutricional; avaliação LGPD, direitos dos titulares e retenção; segurança de sessões, vínculos e controle de acesso; critérios responsáveis para CRN verificado; backup externo automatizado e restauração testada; HTTPS e monitoramento.

## Decisões adiadas
Preço, planos e limites de acompanhados, gateway, custos por cenário e duração do beta. A análise de custos e a decisão de monetização são tarefas posteriores ao MVP, não podem ser silenciosamente substituídas por hipóteses anteriores.

## Regras de passagem
Nenhuma etapa é considerada implementada sem testes e critérios de aceite. Não implantar na VPS antes da validação local. Não realizar migrações de produção nem alterar políticas de segurança para contornar testes.
