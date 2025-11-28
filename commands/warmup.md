# Warmup Command

Load minimal project context.

## Usage

- `/warmup` - Load project overview and development preferences

## Instructions

1. **Read CLAUDE.md** from project root for development preferences
2. **List root directories** to find all applications
3. **Read `{app}/package.json`** for each directory that has one

Each package.json contains current technologies and packages, versions, and available scripts.

**IMPORTANT**: Read ONLY the top-level package.json files - do not read any other directories or files to preserve context window.

## Output

Output in a **4-line format**:

```
✓ Development preferences loaded
✓ Project overview loaded {X applications}
✓ {Readiness short message}

{Inspirational or funny quote}
```
