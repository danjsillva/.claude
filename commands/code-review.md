# Code Review Command

Generate PR description and detailed code review analysis in Portuguese.

## Usage

- `/code-review <pr>` - Review PR (URL or number)

## Instructions

1. **Get PR diff**: Run `gh pr diff $ARGUMENTS`

2. **Generate PR description** based on the diff following these rules:
   - Write a short paragraph (maximum 3 lines) summarizing what the PR does
   - Write in Portuguese, keep technical terms in English (entity names, property names, schemas, tests)
   - Do not hallucinate - only describe changes that actually exist in the code
   - Generate a list of changes where each item starts with a verb in past participle, masculine singular, in Portuguese (Adicionado, Removido, Corrigido, Ajustado, Refatorado)
   - Be clear and technical - mention properties, entities, types, rules, schemas, tests
   - Use subitems (indented) only when necessary to clarify details or exceptions
   - Ignore irrelevant style-only changes (formatting, spacing, unused imports)
   - Group changes by file, entity, or domain when appropriate
   - If changes involve workflows or complex interactions, add a Mermaid sequence diagram

3. **Generate code review analysis** covering these sections:

   ### 1. Pontos Criticos

   List critical issues that MUST be addressed before merge:
   - Security vulnerabilities
   - Breaking changes
   - Data loss risks
   - Performance bottlenecks
   - Logic errors
     If none found, state: "Nenhum ponto critico identificado."

   ### 2. Pontos de Atencao

   List concerns that should be reviewed:
   - Missing error handling
   - Missing tests
   - Incomplete implementations
   - Code smells
   - Potential bugs
     If none found, state: "Nenhum ponto de atencao identificado."

   ### 3. Metricas

   Provide quantitative assessment:
   - Files changed: [number]
   - Lines added: [number]
   - Lines removed: [number]
   - Complexity: [Low/Medium/High]
   - Test coverage impact: [Improved/Maintained/Reduced/Unknown]
   - Risk level: [Low/Medium/High]

   ### 4. Sinais de Codigo IA

   Detect patterns typical of AI-generated code that often pass unnoticed in reviews:
   - **Unnecessary logs/comments**: forgotten console.log, obvious comments, generic TODO/FIXME
   - **Over-engineering**: premature abstractions, excessive error handling, overly defensive code
   - **Under-engineering**: happy path only, ignored edge cases, missing validations
   - **Pattern inconsistency**: naming conventions, patterns, import structure different from project
   - **Generic/tutorial code**: looks like docs example, hardcoded values, ignores project context
   - **Side effects**: changes affecting unrelated code, ignored performance, N+1 queries
   - **Unadapted copy-paste**: duplicated code without context adjustment, nonsensical names
   - **Shallow tests**: happy path only, mocks mirroring implementation, generic asserts
   - **Questionable dependencies**: libs for trivial tasks, outdated versions, duplication
   - **Missing business context**: technically correct but ignores business rules, wrong naming
     If detected, list with file:line and correction suggestion.
     If none found, state: "Nenhum sinal de codigo IA identificado."

## Output

```
## Descricao da PR

Essa PR implementa autenticacao de usuario com validacao via auth-api.

- Adicionado middleware de autenticacao no bossabox-api
  - Validacao de token JWT com auth-api
  - Cache de sessoes implementado com TTL de 1 hora

---

## Analise de Code Review

### 1. Pontos Criticos
Nenhum ponto critico identificado.

### 2. Pontos de Atencao
- Missing error handling em X
- Faltam testes para Y

### 3. Metricas
- Files changed: 5
- Lines added: 120
- Lines removed: 30
- Complexity: Medium
- Test coverage impact: Improved
- Risk level: Low

### 4. Sinais de Codigo IA
- **Over-engineering**: src/auth/middleware.ts:45 - try/catch desnecessario, AuthService ja lanca excecao tratada
- **Codigo generico**: src/auth/constants.ts:12 - TTL hardcoded, mover para env
```
