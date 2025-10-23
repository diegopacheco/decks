# Claude Code 

## 1. Global Instructions

Global instructions will work for all directories, all sessions and all projects you do you in your machine with Claude Code. If I may draw a parallel this feature is the same as `~/.bashrc` or `~/.zshrc` for Claude.

This file must be placed in `~/.claude/CLAUDE.md` it control Claude code guidelines for all projects. Them you can write in any structure or format - instructions you want Claude to follow.

## 2. Prompt Advice

Be Explicit in Prompts

- Claude Code follows instructions literally
- "Fix the bug" vs "Fix the authentication timeout in login.ts:45"
- Reference specific files, functions, line numbers when possible
- Use @filename to reference files in slash commands
- Break large tasks into steps
- Claude tracks context across the session
- Easier to review and iterate on smaller changes

## 3. Create custom commands

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

## 4. Excluding Files from Claude Context

Claude Code uses `permissions.deny` in settings.json to exclude files/directories. This reduces:
- Token usage
- Noise from irrelevant files
- Processing time for large codebases
- Risk of Claude reading sensitive files

Global Exclusions: `~/.claude/settings.json`
Project Exclusions: `.claude/settings.json`

```json
{
  "permissions": {
    "deny": [
      "Read(./node_modules/**)",
      "Read(./vendor/**)",
      "Read(./venv/**)",
      "Read(./dist/**)",
      "Read(./build/**)",
      "Read(./out/**)",
      "Read(./**/*.min.js)",
      "Read(./**/*.bundle.js)",
      "Read(./**/*.map)",
      "Read(./package-lock.json)",
      "Read(./yarn.lock)",
      "Read(./pnpm-lock.yaml)",
      "Read(./.env)",
      "Read(./.env.*)",
      "Read(./**/*.key)",
      "Read(./**/*.pem)",
      "Read(./credentials.json)",
      "Read(./coverage/**)",
      "Read(./.nyc_output/**)",
      "Read(./.vscode/**)",
      "Read(./.idea/**)",
      "Read(./**/*.log)",
      "Read(./data/**)"
    ]
  }
}
```

## 5. Non-Obvious Use Cases for Claude Code

1. Git Archaeology - Investigate why code was written a certain way by analyzing git history, blame annotations, and
commit messages together with current code structure
2. Dependency Auditing - Scan package.json/requirements.txt/go.mod across multiple projects to identify version
inconsistencies, security vulnerabilities, and upgrade paths
3. Documentation Reverse Engineering - Generate accurate technical documentation by reading actual implementation
rather than outdated docs, including API contracts, state machines, and data flows
4. Codebase Onboarding Scripts - Create personalized learning paths by analyzing a new repository's structure,
identifying entry points, and generating reading orders for understanding the system
5. Test Data Generation - Analyze schema definitions, database models, and validation rules to generate realistic test
fixtures that respect all constraints and edge cases
6. Configuration Drift Detection - Compare config files across environments (dev/staging/prod) to identify
discrepancies, missing values, or environment-specific bugs
7. Refactoring Impact Analysis - Before making changes, analyze call graphs and dependency trees to predict blast
radius and identify all affected consumers
8. Performance Archaeology - Correlate git history with performance metrics to identify which commits introduced
regressions by analyzing algorithmic complexity changes
9. English Spelling and Grammar Checking - Review technical writing, comments, and documentation for clarity, conciseness.
10. API Evolution Planning - Analyze existing API usage patterns across consumers to design backward-compatible changes and generate migration guides.
11. Build Failure Forensics & Troubleshooting - Analyze build logs, recent commits, and dependency changes together to pinpoint root cause when CI mysteriously breaks.
12. Feature Flag Archaeology - Find stale feature flags by cross-referencing flag definitions with usage, creation
  dates, and A/B test completion to safely remove technical debt.
13. Security Audit Automation - Scan codebases for common security anti-patterns, outdated dependencies, and
  misconfigurations.

## 6. Advanced Pipelines

Here the idea is that you can create a script to automate complex flow, so you do not need to type a huge prompt all the time. This code could run in a jenkins job, or even dirrectly in your terminal.

pipeline.sh
```
#!/bin/bash

# Stage 1: Code generation
claude -p "Generate a REST API for user management in Python FastAPI" > api_draft.py

# Stage 2: Security review agent
claude -p "Review this code for security vulnerabilities and suggest fixes:
$(cat api_draft.py)" > security_review.md

# Stage 3: Apply fixes
claude -p "Here's the original code:
$(cat api_draft.py)

Here's the security review:
$(cat security_review.md)

Apply all security fixes and output the improved code." > api_secure.py

# Stage 4: Add tests
claude -p "Generate comprehensive pytest tests for this code:
$(cat api_secure.py)" > test_api.py
```

## 7. Bash on Asteroids

You can use Claude Code to help you write bash scripts for automating complex tasks. You can combine any program with bash + claude being a super powerful orchestrator.
```
#!/bin/bash

curl -s "wttr.in/London?format=3" | \
claude -p "Extract only the temperature number in Celsius from this text, output just the number" | tee weather_celsius.txt | \
xargs -I {} claude -p 'Is {} Celsius HOT or COLD? Output only HOT or COLD followed by the temperature value in Celsius' | tee weather_report.txt
```

Result:
```
COLD 10
```