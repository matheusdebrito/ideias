# Alimentos e cálculos nutricionais — especificação atual

> Revisado em 01/11/2026. **Regras atuais** detalham [REQUIREMENTS.md](./REQUIREMENTS.md); não aplicar versões antigas.

## Catálogo institucional
- TBCA prioritária, **apenas depois de confirmação de licença, direitos de redistribuição e uso**; origem, versão, identificação externa estável, composição e unidades rastreáveis. Outras fontes futuras por importação, sem acoplamento ao diário.
- Importação por comando manual, validada e versionada; nunca altere snapshots históricos e não crie duplicação de registro externo.
- Energia, macros, fibras, vitaminas e minerais quando presentes; ausência de medida = desconhecido, **não zero**.
- **Alimentos institucionais incompletos podem ser adicionados a refeições/planos**. Nas somas, marcar explicitamente cada nutriente incompleto, inclusive calorias e macros; não afirmar que a soma parcial é um total exato.
- Busca indexada no PostgreSQL por nome, sem diferenciação de caixa ou acentos, relevância e paginação, priorizando preparados e prontos. Alimentos naturalmente consumidos crus permanecem disponíveis.
- Marca/variante opcional para diferenciar produtos e preparação; sem confundir variantes com mesmo nome.

## Cadastro privado de alimento
- **Obrigatórios:** nome não vazio; quantidade de referência positiva + unidade; proteína, carboidrato e gordura informados (inclusive zero quando aplicável).
- **Calorias opcionais**: usar kcal declaradas caso existam; caso contrário, calcular aproximação **4×proteína + 4×carboidrato + 9×gordura**, registrar indicador `estimated` e informar ao usuário que é estimativa.
- Fibras, vitaminas, minerais opcionais, sem preencher lacunas automaticamente com zero.
- Validar finitude numérica, valores não negativos, consistência de quantidade/unidade. Rejeitar impossibilidades físicas demonstráveis (comparar massa total de macros com massa de alimento quando unidades compatíveis e sem dupla contagem); **não comparar g diretamente com mL sem densidade nem exigir igualdade kcal/macros**.
- Cada alimento privado pertence a um usuário. Nunca consultável por outra conta por ID. Gestor poderá incorporar snapshot do alimento em um plano compartilhado com titular autorizado, sem publicar seu catálogo.
- Edição não muda snapshots antigos. Exclusão definitiva só quando nunca usado; quando já referenciado, desativação lógica e remoção dos resultados/favoritos. Sem compartilhamento público.

## Porções e conversões
Padrão de registro: gramas (sólidos) ou mililitros (líquidos), inclusive valores decimais válidos. Unidade, colher, fatia e xícara só com conversão conhecida e específica por alimento/preparo. Não converter cru/cozido automaticamente, nem massa/volume sem densidade confiável.

## Cálculo e exibição
`nutrienteConsumido = quantidadeNormalizada / quantidadeReferencia * nutrienteReferencia`.
Prévia calculada no frontend sem requisição por ajuste de quantidade. Backend recalcula e valida antes de salvar, ignorando calorias/macros de consumo enviados pelo cliente. Evitar arredondamento intermediário, conservar precisão de origem e formatar somente na apresentação. Snapshot por item consumido guarda valores, fonte/versão, porção e indicador de cobertura de nutrientes.

## Conveniência
Recentes do próprio usuário por último consumo; favoritos explícitos privados. Modelos de refeição guardam diversos alimentos e porções, separadamente dos favoritos. Nenhum sistema de recomendações ou buscador externo no MVP.

## Casos de teste indispensáveis
- Alimento personalizado exige macros, mas **não exige kcal**.
- Alimento TBCA sem proteína **pode ser registrado**; proteína do total diário vira cobertura parcial, não zero ou total exato.
- Mudar composição de alimento não altera consumo histórico.
- Dois usuários não enxergam alimentos/favoritos privados um do outro.
- Prévia frontend e cálculo backend concordam para porções e valores com decimais.
