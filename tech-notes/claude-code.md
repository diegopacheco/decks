# Claude Code 

## Global Instructions

Global instructions will work for all directories, all sessions and all projects you do you in your machine with Claude Code. If I may draw a parallel this feature is the same as `~/.bashrc` or `~/.zshrc` for Claude.

This file must be placed in `~/.claude/CLAUDE.md` it control Claude code guidelines for all projects. Them you can write in any structure or format - instructions you want Claude to follow.

## Prompt Advice

Be Explicit in Prompts

- Claude Code follows instructions literally
- "Fix the bug" vs "Fix the authentication timeout in login.ts:45"
- Reference specific files, functions, line numbers when possible
- Use @filename to reference files in slash commands
- Break large tasks into steps
- Claude tracks context across the session
- Easier to review and iterate on smaller changes

## Create custom commands

Use Slash Commands for Repeated Tasks

- Create .claude/commands/ in your project root
- Common use cases: code review, test generation, refactoring patterns
- Personal commands in ~/.claude/commands/ for all projects
- Name them short and memorable (e.g., /fix, /test, /deploy)

Examples:

Example 1: Code Review Command
```
File: .claude/commands/review.md
Review the changes in this file for:
- Bugs and edge cases
- Performance issues
- Security vulnerabilities
- Code style and best practices
- Missing error handling

Provide specific line numbers for issues found.

Usage: /review @src/auth.js
```

Example 2: Test Generation Command
```
File: .claude/commands/tests.md
Generate comprehensive tests for $1 covering:
- Happy path scenarios
- Edge cases
- Error conditions
- Boundary values

Use the existing test patterns in this project.

Usage: /tests src/utils/validator.js
```

Example 3: Refactor Command
```
File: ~/.claude/commands/clean.md
Refactor $ARGUMENTS to:
- Remove code duplication
- Improve naming and clarity
- Simplify complex logic
- Maintain exact same behavior

Run tests after refactoring to verify nothing broke.

Usage: /clean src/payment/processor.js
```

Example 4: Performance Audit
```
File: .claude/commands/perf.md
Analyze $1 for performance issues:
- Unnecessary loops or iterations
- Database query optimization
- Memory allocations
- Blocking operations
- Caching opportunities

Suggest specific optimizations with code snippets.

Usage: /perf @api/users.ts
```

The key: commands save you from retyping the same instructions repeatedly.
