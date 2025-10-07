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