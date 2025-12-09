# Deep Review Command

Generate PR description and run CodeRabbit analysis.

## Usage

- `/deep-review <pr-url>` - Deep review PR (full GitHub URL)

## Instructions

1. **Parse PR URL**: Extract owner, repo, and PR number from `$ARGUMENTS`
   - Format: `https://github.com/{owner}/{repo}/pull/{number}`
   - Fail if URL format is invalid

2. **Get PR metadata**: Run `gh pr view $ARGUMENTS --json baseRefName,headRefName`
   - Extract base branch (baseRefName) and PR branch (headRefName)

3. **Checkout PR branch**: Run `gh pr checkout {number}`
   - If checkout fails, abort and report error

4. **Get diff locally**: Run `git diff {baseRefName}..HEAD`

5. **Generate and show PR description** based on the diff following these rules:
   - Write a short paragraph (maximum 3 lines) summarizing what the PR does
   - Write in Portuguese, keep technical terms in English (entity names, property names, schemas, tests)
   - Do not hallucinate - only describe changes that actually exist in the code
   - Generate a list of changes where each item starts with a verb in past participle, masculine singular, in Portuguese (Adicionado, Removido, Corrigido, Ajustado, Refatorado)
   - Be clear and technical - mention properties, entities, types, rules, schemas, tests
   - Use subitems (indented) only when necessary to clarify details or exceptions
   - Ignore irrelevant style-only changes (formatting, spacing, unused imports)
   - Group changes by file, entity, or domain when appropriate
   - If changes involve workflows or complex interactions, add a Mermaid sequence diagram
   - **Show the description immediately** so user can read while CodeRabbit runs

6. **Run CodeRabbit analysis** (synchronous):

   ```
   coderabbit review --plain --type committed --base {baseRefName}
   ```

   - Use timeout of 5 minutes
   - Display the **complete** output - do not summarize or truncate
   - The output contains the actual review findings with file paths, issues, and suggested fixes

7. **Return to previous branch**: Run `git checkout -`

8. **Delete local PR branch** with guardrails:
   - Get branch name from step 2 (headRefName)
   - **NEVER delete**: main, master, development, develop, dev, staging, stage, homologation, homolog
   - **ONLY delete local branch**: `git branch -d {headRefName}`
   - **NEVER use -D** (force delete) - if branch has unmerged changes, leave it
   - **NEVER delete remote**: no `git push origin --delete`
   - If delete fails, just warn and continue (don't abort)

## Output Format

```
## Descricao da PR

[Short paragraph summarizing the PR]

- Adicionado [change description]
- Removido [change description]
- Corrigido [change description]

---

## CodeRabbit Analysis

[Full CodeRabbit output]

---

Cleanup: Branch `{headRefName}` removed locally.
```

## Error Handling

- Invalid URL format: Show error with expected format
- Checkout fails: Abort with error message
- CodeRabbit fails: Show description anyway, report CodeRabbit error
- Branch delete fails: Warn but don't fail the command
