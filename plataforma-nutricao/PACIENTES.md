# Gestão de usuários acompanhados, convites e permissões

> Revisado em 01/11/2026. A palavra “paciente” é contextual; **usuário acompanhado** é uma conta comum. Fonte: [REQUIREMENTS.md](./REQUIREMENTS.md).

## Contas e elegibilidade
Uma conta de adulto pode usar recursos pessoais, ser acompanhada e gerir terceiros. CRN não é obrigatório para gerir. CRN verificado é selo informativo de nutricionista, sem privilégios adicionais. No beta gratuito, funcionalidades de gestão ficam disponíveis com consentimento do titular; no futuro a elegibilidade comercial será definida em regras do backend.

## Vínculo
- Cada titular pode ter **um único gestor ativo**. Um gestor pode acompanhar vários titulares conforme elegibilidade futura.
- Convite nunca dá acesso automaticamente: titular deve aceitar autenticado e entender os dados compartilhados.
- Aceitar convite de novo gestor com vínculo existente mostra aviso com gestor anterior/novo e revogação de acesso anterior. Atualizar vínculo **atomicamente** em PostgreSQL, sem apagar histórico nutricional do titular.
- Ambas as partes podem encerrar vínculo. Revogação remove acesso aos dados imediatamente; ex-gestor retém, no máximo, metadados administrativos mínimos necessários e legalmente autorizados.

## Convites
- Dentro da aplicação, busca por nome limitada/paginada (sem exposição pública de e-mail) ou busca por **e-mail exato** para usuários cadastrados.
- Link individual de WhatsApp de uso único, inclusive para novos usuários que devem criar conta ou entrar antes de aceitar.
- Prazo de validade **7 dias**; rejeitar não estende validade; evitar convites pendentes duplicados, spam e enumeração de contas. Aceite e troca de gestor idempotentes e seguros sob concorrência.
- Convites pendentes não ocupam vagas/limites comerciais futuros.
- Não enviar convite por e-mail, não implementar chat interno.

## Acesso autorizado
Gestor ativo pode ver diário, peso, medidas, metas e evolução conforme consentimento e elaborar planos. Gestor **não pode editar/apagar consumo real no diário do titular**. Consultas sempre checam vínculo atual, operação autorizada e titularidade no backend — UUID não substitui autorização.

## Notificações
Criar notificações in-app persistidas e consultadas periodicamente por HTTP para convites, alteração de plano/metas, encerramento/troca e eventos relevantes. Não implementar WebSocket, push ou mensagens diretas no MVP.

## Após beta
Planos de gestão e limites serão definidos depois de estimar custo e analisar uso. Se capacidade futura cair abaixo da quantidade acompanhada, solicitar que gestor encerre vínculos excedentes, sem escolher/excluir pessoas automaticamente. Não excluir dados do titular se gestor cancelar assinatura.

## Critérios de teste
Autorização entre contas, restrições de enumeração, aceite duplicado, duas aceitações concorrentes, revogação de acesso de ex-gestor, gestores sem CRN, logout/sessão expirada, convites expirados e link consumido.
