# Task Description Generator

Generate structured task descriptions.

## Usage

- `/task-description` - Generate a formatted task
- `/task-description <brief description>` - Generate task with initial context

## Instructions

Help create a task following this template:

### Template

**[2-3 lines of context - no subtitle]**

**O que precisa ser feito:**

- [System] Objective action description
- [System] Objective action description
- ...

**Criterios de aceite:**

- User-focused impact/validation
- User-focused impact/validation
- ...

**Pontos de atencao:** _(only when really necessary)_

- Critical points or risks

### Important rules

1. **Avoid code in solutions** - focus on WHAT needs to be done, not HOW to implement
2. **User-focused criteria** - concentrate on end-user impacts (though other criteria may exist when necessary)
3. **Simplicity and clarity** - be intentional, avoid text that adds no value
4. **Item prefixes** - use prefixes based on affected system (e.g., [API], [Web], [Mobile], [Data], [Infra])

### Execution instructions

1. If user provided a description, use it as starting point
2. Ask objective questions to understand:
   - What is the current problem or need?
   - Which systems will be impacted?
   - What is the expected result for the user?
3. Generate the task following the template above
4. **DO NOT add** unrequested extra sections
5. **DO NOT include** code or implementation details
6. Keep the description **concise and objective**
