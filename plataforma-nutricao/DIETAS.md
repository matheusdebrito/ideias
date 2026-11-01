# Planejamento alimentar — especificação atual

> Revisado em 01/11/2026. Regras de [REQUIREMENTS.md](./REQUIREMENTS.md) prevalecem. Sem agendamento avançado ou múltiplos planos ativos no MVP.

## Conceitos
Plano é sugestão de alimentação composta por refeições e alimentos/porções. Diário é consumo real e **nunca se preenche automaticamente** a partir do plano. Usuário pode registrar livremente refeições fora do planejado, copiar/ajustar refeição sugerida ou criar refeição avulsa.

## Plano individual
- Um plano **atual editável** por titular. Registrar versões das atualizações relevantes para preservar referências anteriores; não implementar calendário de versões futuras complexas nem múltiplos planos concorrentes.
- Titular pode montar e alterar plano próprio, sem depender de gestor.
- Gestor autorizado pode organizar plano do acompanhado; alteração de plano não reescreve diário passado, metas históricas ou refeições já registradas.
- Refeições de plano contêm **alimentos e quantidades**, não apenas macronutrientes. Nome identifica refeição; horários são sugestões opcionais; observações opcionais. Pode haver várias refeições de um mesmo período.
- Ao editar concorrentemente, detectar conflito de versão e exigir atualização/revisão, não sobrescrever silenciosamente.

## Biblioteca e modelos
Usuário pode salvar refeições privadas para reutilização. Gestores poderão ter biblioteca privada de planos/modelos no beta gratuito e, depois, conforme recursos do plano pago. Usar modelo cria cópia independente para o titular; edições no original não alteram cópias/diários.

## Alimentos e visibilidade
Refeições com alimento institucional incompleto podem ser planejadas, mas indicadores de nutrientes ausentes precisam sinalizar incompletude. Alimentos personalizados de gestor podem entrar em planos do acompanhado via **snapshot**, sem dar ao acompanhado acesso ao catálogo privado do gestor.

## Permissões
Gestor de terceiro exige vínculo vigente e consentimento; CRN não é pré-requisito. No beta, permissões de gestão estão abertas a todos os adultos, mediante vínculo. Após monetização, verificar elegibilidade no backend, preservando dados quando houver perda da assinatura. O titular é o único responsável por editar/excluir os próprios registros de **consumo** no diário.

## Não implementar no MVP
Múltiplas dietas vigentes, agenda de versões futuras com reagendamento, mecanismos de receitas, lista de substituição automática de alimentos, prescrição clínica automática, chat ou notificações por horário atrasado.
