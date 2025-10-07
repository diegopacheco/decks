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

## .claudeignore Files

`.claudeignore` Files

Purpose
Similar to .gitignore, .claudeignore lets you exclude files/directories from Claude's context. This reduces:
- Token usage (saves money on API calls)
- Noise from irrelevant files
- Processing time for large codebases
- Risk of Claude reading sensitive files

Location
* Place `.claudeignore` in your project root (same directory as .git/)

Syntax
Same as .gitignore:
- One pattern per line
- `#` for comments
- `*` for wildcards
- `**` for recursive directories
- `!` to negate (include) patterns

Example `.claudeignore`
```
# Dependencies
node_modules/
vendor/
.pnp/
.pnp.js

# Build outputs
dist/
build/
out/
*.min.js
*.bundle.js
*.map

# Package manager lock files
package-lock.json
yarn.lock
pnpm-lock.yaml
Gemfile.lock
poetry.lock

# Environment and secrets
.env
.env.*
*.key
*.pem
credentials.json

# Test coverage and reports
coverage/
.nyc_output/
*.lcov
test-results/

# IDE and editor files
.vscode/
.idea/
*.swp
*.swo
.DS_Store

# Generated files
*.generated.ts
*_pb.js
*_pb.d.ts

# Large data files
*.csv
*.log
*.sql
*.dump
data/

# Documentation builds
docs/_build/
site/
.docusaurus/

# Cache directories
.cache/
.next/
.nuxt/
.turbo/

# But DO include specific config
!.env.example
!docs/architecture.md
```

Real-World Scenario

Before .claudeignore:
- Claude reads 50MB of node_modules searching for a function
- Costs 200K tokens
- Takes 30 seconds
- Returns irrelevant results from dependencies

After .claudeignore:
- Claude only reads your source code (2MB)
- Costs 8K tokens
- Takes 2 seconds
- Returns accurate results from your code

Strategic Exclusions - Always exclude:
- node_modules/, vendor/, venv/
- Build artifacts
- Lock files
- Large binary files
- Generated code

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
9. English Spealling and Grammar Checking - Review technical writing, comments, and documentation for clarity, conciseness.
10. API Evolution Planning - Analyze existing API usage patterns across consumers to design backward-compatible changes and generate migration guides.
11. Build Failure Forensics & Troubleshooting - Analyze build logs, recent commits, and dependency changes together to pinpoint root cause when CI mysteriously breaks.
12. Feature Flag Archaeology - Find stale feature flags by cross-referencing flag definitions with usage, creation
  dates, and A/B test completion to safely remove technical debt.
13. Security Audit Automation - Scan codebases for common security anti-patterns, outdated dependencies, and
  misconfigurations.