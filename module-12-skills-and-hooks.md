# Module 12: Customizing Claude Code - Making It Yours

**Goal:** Learn every way to customize Claude Code's behavior, from project memory to custom skills and automation hooks

**Estimated Time:** 45-60 minutes

---

## What You'll Learn

This is the module where Claude Code goes from a useful tool to *your* tool. We'll cover CLAUDE.md files (Claude's persistent memory), modular rules, the settings hierarchy, custom skills, commands, hooks, and a first look at custom agents. By the end you'll know how to control basically every aspect of how Claude behaves in your projects.

---

## Lesson 1: CLAUDE.md & Memory

### The Most Important File in Your Project

Here's the deal: the single most impactful thing you can do with Claude Code is set up a good `CLAUDE.md`. It's Claude's persistent memory -- loaded automatically at the start of every session. Without one, Claude starts every conversation knowing nothing about your project conventions, your preferred commands, or how your codebase is organized.

You can bootstrap one quickly:

```text
/init
```

Or create it manually in your project root.

---

### Where CLAUDE.md Files Live

There are several locations, each with a different scope:

**Project memory** (shared with your team via version control):
```text
./CLAUDE.md              # Project root
./.claude/CLAUDE.md      # Alternative location
```

**Project memory (personal)** -- automatically gitignored:
```text
./CLAUDE.local.md        # Your personal project preferences
```

**User memory** (applies to all your projects):
```text
~/.claude/CLAUDE.md      # Global personal preferences
```

**Managed policy** (organization-wide, admin-deployed):
```text
# macOS: /Library/Application Support/ClaudeCode/CLAUDE.md
# Linux: /etc/claude-code/CLAUDE.md
```

More specific files take precedence over broader ones. So your project `CLAUDE.md` overrides your global one for that project.

---

### What Goes in a Good CLAUDE.md

Keep it under 150 lines. Seriously. Claude loads this into context every session, so bloated files waste tokens and dilute the important stuff.

A solid CLAUDE.md includes something like:

```markdown
# Project: My App

## Architecture
- Next.js frontend in /src/app
- Express API in /src/api
- PostgreSQL database, Prisma ORM

## Common Commands
- Build: `npm run build`
- Test: `npm test`
- Lint: `npm run lint`
- Dev server: `npm run dev`

## Coding Standards
- TypeScript strict mode, no `any` types
- Use named exports, not default exports
- Error handling: always use custom AppError class
- Tests: colocate test files next to source (*.test.ts)

## Commit Format
- feat: new feature
- fix: bug fix
- refactor: code change that neither fixes a bug nor adds a feature

## Key Files
- /src/api/middleware/auth.ts - Authentication middleware
- /src/lib/db.ts - Database connection and helpers
- /prisma/schema.prisma - Database schema
```

Don't put everything in one file. Use `@imports` to pull in detailed references:

```markdown
See @README.md for project overview and @docs/api-reference.md for API details.
```

Both relative and absolute paths work. Relative paths resolve from the file containing the import. You can even import from your home directory so all worktrees share the same personal instructions:

```markdown
# My personal preferences
- @~/.claude/my-project-notes.md
```

---

### Production CLAUDE.md Examples

What does a mature CLAUDE.md look like for real projects? Here are condensed examples across different stacks:

**SaaS Next.js (Supabase + Stripe + RLS):**
- Architecture split: server components use `createServerClient()`, client components use `createBrowserClient()` -- never mix them
- All database queries go through RLS policies; direct table access is forbidden outside migrations
- Stripe webhook handlers live in `src/app/api/webhooks/stripe/` and must be idempotent
- Test commands: `npm test`, `npm run test:e2e`, `npx supabase db reset` for clean DB state

**Go Microservice (gRPC + PostgreSQL):**
- Clean architecture layers: `internal/domain/`, `internal/service/`, `internal/repository/`, `internal/transport/`
- All errors wrap with `fmt.Errorf("operation: %w", err)` -- never discard error context
- Proto files in `proto/`, generate with `make proto`; never hand-edit generated `.pb.go` files
- Build: `make build`, Test: `make test`, Lint: `golangci-lint run ./...`

**Django API (DRF + Celery):**
- Serializers in `serializers.py`, views in `views.py`, business logic in `services.py` -- views must stay thin
- Background jobs use Celery tasks in `tasks.py`; never call external APIs synchronously in request handlers
- Management commands for data operations: `python manage.py <command>`, test with `pytest --reuse-db`

**Rust API (Axum + SQLx):**
- Handler functions in `src/handlers/`, extractors in `src/extractors/`, SQLx queries use compile-time checked `sqlx::query!` macro
- All database migrations in `migrations/` managed by `sqlx migrate run`
- Build: `cargo build`, Test: `cargo test`, Lint: `cargo clippy -- -D warnings`

---

### How Claude Looks Up Memory

Claude reads CLAUDE.md files recursively up the directory tree from your current working directory. If you're working in `packages/frontend/`, Claude reads:
- `packages/frontend/CLAUDE.md`
- `packages/CLAUDE.md`
- `CLAUDE.md` (project root)

CLAUDE.md files in subdirectories below your cwd are loaded lazily -- only when Claude actually reads files in those directories.

---

### Auto Memory

Claude Code also has auto memory -- notes Claude writes for itself as it works. These live at `~/.claude/projects/<project>/memory/` and include things like debugging insights, project patterns, and architecture notes.

The first 200 lines of `MEMORY.md` in that directory are loaded every session. Use `/memory` to view and edit these files.

---

## Lesson 2: Rules

### Modular Instructions with `.claude/rules/`

For larger projects, one CLAUDE.md file isn't enough. Rules let you organize instructions into focused files:

```text
your-project/
├── .claude/
│   ├── CLAUDE.md
│   └── rules/
│       ├── code-style.md
│       ├── testing.md
│       └── security.md
```

Every `.md` file in `.claude/rules/` is automatically loaded as project memory. You can also create personal rules at `~/.claude/rules/` that apply to all your projects.

---

### Path-Scoped Rules

This is where rules get interesting. You can scope them to specific files using YAML frontmatter:

```markdown
---
description: API development standards
globs: ["src/api/**/*.ts"]
---

# API Development Rules

- All API endpoints must include input validation
- Use the standard error response format from src/lib/errors.ts
- Include OpenAPI documentation comments
```

Rules without a `globs` field load unconditionally. Rules with globs only activate when Claude is working with matching files.

Supported glob patterns:

| Pattern | Matches |
|---------|---------|
| `**/*.ts` | All TypeScript files in any directory |
| `src/**/*` | All files under `src/` |
| `*.md` | Markdown files in project root only |
| `src/**/*.{ts,tsx}` | TypeScript and TSX files under src |

Organize rules into subdirectories if you want -- they're discovered recursively:

```text
.claude/rules/
├── frontend/
│   ├── react.md
│   └── styles.md
├── backend/
│   ├── api.md
│   └── database.md
└── general.md
```

For polyglot projects -- where you have multiple languages in one repo -- organizing rules by language keeps things clean and avoids cross-contamination of conventions:

```text
.claude/rules/
├── common/
│   ├── coding-style.md
│   ├── testing.md
│   ├── security.md
│   └── git-workflow.md
├── typescript/
│   ├── strict-mode.md
│   └── react-patterns.md
├── python/
│   ├── type-hints.md
│   └── django-patterns.md
└── golang/
    ├── error-handling.md
    └── concurrency.md
```

The `common/` directory holds rules that apply regardless of language -- things like commit message format, testing philosophy, and security requirements. Language-specific directories contain rules scoped to their respective file types using `globs` frontmatter (e.g., `globs: ["**/*.ts", "**/*.tsx"]` for the TypeScript rules). Language-specific rules can reference common ones with `@imports`, so your Go error-handling rules can point to `@../common/coding-style.md` for the shared conventions they build on. This structure mirrors how your codebase is actually organized and makes it obvious where new rules belong.

---

## Lesson 3: Settings

### The 5-Tier Settings Hierarchy

If you've ever wondered "why isn't my setting working?" -- it's almost always a precedence problem. Claude Code settings follow a specific order, and higher tiers override lower ones:

| Priority | Location | Scope |
|----------|----------|-------|
| 1 (highest) | CLI flags (`--model`, `--allowedTools`) | Current session |
| 2 | `.claude/settings.local.json` | Project, personal (gitignored) |
| 3 | `.claude/settings.json` | Project, shared via VCS |
| 4 | `~/.claude/settings.local.json` | User, local machine |
| 5 (lowest) | `~/.claude/settings.json` | User, global |

Plus: `managed-settings.json` deployed by admins has the absolute highest priority and can't be overridden by anything.

---

### Basic Permission Settings

The most useful setting you'll configure is permissions. Here's a practical example:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git commit *)",
      "Edit(/src/**)"
    ],
    "deny": [
      "Bash(git push --force *)",
      "Read(.env)"
    ]
  }
}
```

A few key rules to keep in mind:
- Deny always wins over allow
- `Bash(npm run *)` -- the space before `*` means "npm run" must be a complete prefix followed by a space
- `Edit(/src/**)` -- gitignore-style patterns, `**` matches recursively
- `WebFetch(domain:example.com)` -- restrict web fetches by domain

Put shared team rules in `.claude/settings.json`. Put your personal overrides in `.claude/settings.local.json`.

---

### Permission Modes

You can also set a default mode for how Claude handles permissions overall. This is separate from individual allow/deny rules -- think of it as the baseline posture:

| Mode | What It Does |
|------|-------------|
| `default` | Prompts for permission on first use |
| `acceptEdits` | Auto-accepts file edits |
| `plan` | Read-only, no modifications |
| `dontAsk` | Auto-denies unless pre-approved |
| `bypassPermissions` | Skips all checks (containers only!) |

Most people stick with `default` and use allow/deny rules for fine-tuning. The `bypassPermissions` mode is strictly for disposable container environments -- do not use it on your actual machine.

---

## Lesson 4: Skills

### The Real Skill Format

Skills are custom slash commands that extend what Claude can do. Forget TypeScript files in `~/.config/` -- that's not how it works. Skills are Markdown files with YAML frontmatter.

Every skill needs:
```text
.claude/skills/<skill-name>/SKILL.md
```

That's it. A directory with a `SKILL.md` file inside.

---

### Creating Your First Skill

Here's a skill that reviews code:

```bash
mkdir -p .claude/skills/review-code
```

Create `.claude/skills/review-code/SKILL.md`:

```yaml
---
description: Reviews code for quality, security, and best practices. Use when the user asks for a code review or after significant changes.
---

When reviewing code:

1. Run `git diff` to see recent changes
2. Focus on modified files
3. Check for:
   - Security issues (exposed secrets, injection vulnerabilities)
   - Performance problems (unnecessary loops, missing indexes)
   - Code style violations against project conventions
   - Missing error handling
   - Missing tests for new functionality

Provide feedback organized by priority:
- **Critical** (must fix before merge)
- **Warning** (should fix)
- **Suggestion** (nice to have)
```

Use it:
```text
/review-code
```

Or just ask Claude something that matches the description -- Claude will load the skill automatically.

---

### Where Skills Live

| Location | Path | Applies To |
|----------|------|-----------|
| Personal | `~/.claude/skills/<name>/SKILL.md` | All your projects |
| Project | `.claude/skills/<name>/SKILL.md` | This project only |
| Plugin | `<plugin>/skills/<name>/SKILL.md` | Where plugin is enabled |

When skills share the same name, higher priority wins: enterprise > personal > project.

Skills in subdirectories of your project are also discovered automatically. Working in `packages/frontend/`? Claude picks up skills from `packages/frontend/.claude/skills/` too.

---

### Frontmatter Reference

Here's the full menu of frontmatter fields. All fields are optional, but `description` is strongly recommended -- without it, Claude has no idea when your skill is relevant:

| Field | What It Does |
|-------|-------------|
| `name` | Display name, becomes the `/slash-command`. Lowercase, hyphens. Max 64 chars |
| `description` | When to use this skill. Claude uses this to decide when to load it |
| `argument-hint` | Autocomplete hint, e.g. `[filename]` or `[issue-number]` |
| `disable-model-invocation` | `true` = only YOU can invoke it (not Claude). Use for `/deploy`, `/ship` |
| `user-invocable` | `false` = hidden from `/` menu. Background knowledge only |
| `allowed-tools` | Tools Claude can use without asking when skill is active |
| `model` | Model override when skill is active |
| `context` | `fork` = run in isolated subagent context |
| `agent` | Which subagent type when `context: fork` (Explore, Plan, general-purpose, or custom) |
| `hooks` | Lifecycle hooks scoped to this skill |

---

### Dynamic Substitutions

Skills would be pretty limited if you couldn't pass data into them. These placeholders get replaced at runtime:

| Variable | What It Gets Replaced With |
|----------|---------------------------|
| `$ARGUMENTS` | Everything passed after `/skill-name` |
| `$ARGUMENTS[0]`, `$0` | First argument |
| `$ARGUMENTS[1]`, `$1` | Second argument |
| `${CLAUDE_SESSION_ID}` | Current session ID |

Example:
```yaml
---
description: Fix a GitHub issue
disable-model-invocation: true
---

Fix GitHub issue #$ARGUMENTS following our coding standards.

1. Read the issue description with `gh issue view $ARGUMENTS`
2. Understand the requirements
3. Implement the fix
4. Write tests
5. Create a commit
```

Run it: `/fix-issue 123`

---

### Injecting Dynamic Context

The `` !`command` `` syntax runs shell commands before the skill content reaches Claude:

```yaml
---
description: Summarize the current pull request
context: fork
agent: Explore
---

## Current PR context
- Diff: !`gh pr diff`
- Comments: !`gh pr view --comments`
- Changed files: !`gh pr diff --name-only`

Summarize this pull request. Focus on what changed and why.
```

The commands execute first, their output replaces the placeholders, and Claude only sees the final rendered content.

---

### Supporting Files

Skills can include extra files in their directory:

```text
review-code/
├── SKILL.md           # Main instructions (required)
├── checklist.md       # Detailed review checklist
├── examples/
│   └── good-review.md # Example of expected output
└── scripts/
    └── lint-check.sh  # Script Claude can execute
```

Reference them from SKILL.md so Claude knows they exist:

```markdown
For the detailed checklist, see [checklist.md](checklist.md).
For an example of a good review, see [examples/good-review.md](examples/good-review.md).
```

---

### Real-World Skills Gallery

Once you understand the format, skills become your primary way to encode workflows. Here are the categories of skills that show up in mature setups:

**Development Workflow:**
- `tdd-workflow` -- Structured red-green-refactor cycle. Frontmatter: `allowed-tools: Bash, Edit, Read` and `disable-model-invocation: true`. The skill walks Claude through writing a failing test first, implementing the minimum code to pass, then refactoring.
- `e2e-testing` -- End-to-end test generation using Playwright Page Object Model. Frontmatter: `argument-hint: [page-or-feature]`. Includes supporting files with POM templates and selector conventions.

**Quality:**
- `security-review` -- Vulnerability scanning against common patterns (SQL injection, XSS, secrets in code). Frontmatter: `context: fork`, `agent: Explore`, `model: sonnet`. Runs in isolation so findings don't pollute the main conversation.
- `coding-standards` -- Per-language enforcement with auto-fix suggestions. Frontmatter: `user-invocable: false` (background knowledge). Claude loads this automatically when editing code.

**Operations:**
- `deployment-patterns` -- Docker build verification, CI/CD pipeline checks, environment variable validation. Frontmatter: `disable-model-invocation: true` (only run when you explicitly ask). Includes checklists as supporting files.
- `database-migrations` -- Safe schema change workflow: generate migration, review SQL, check for destructive operations, test rollback. Frontmatter: `allowed-tools: Bash, Read, Grep`.

**Learning:**
- `continuous-learning` -- Extracts patterns from the current session and records them with a confidence score. Frontmatter: `hooks: { "Stop": [...] }` to trigger at session end. Over time, builds a knowledge base of project-specific insights that feed back into CLAUDE.md.

Each of these follows the same SKILL.md format you already know -- the difference is just in how they combine frontmatter fields, supporting files, and dynamic context injection. You'll build your own in the exercises below.

---

## Lesson 5: Commands

### The Original Slash Commands

Before skills existed, there were commands. They still work and they're simpler:

```text
.claude/commands/review.md
```

That file becomes `/review`. The content is the prompt Claude receives.

A command file is just a markdown file with optional template variables:

```markdown
Review the code changes in this project.

The project directory is: ${CLAUDE_PROJECT_DIR}

User's request: ${ARGUMENTS}

Focus on:
- Code quality
- Security issues
- Test coverage
```

Template variables:
- `${ARGUMENTS}` -- whatever the user types after the command name
- `${CLAUDE_PROJECT_DIR}` -- the project root directory

---

### Commands vs Skills -- When to Use Which

The short version: skills are the future, commands are the legacy format. A file at `.claude/commands/review.md` and a skill at `.claude/skills/review/SKILL.md` both create `/review` and work the same way. If both exist with the same name, the skill wins.

Use skills when you need the extra power -- supporting files, frontmatter control (model, tools, invocation settings), Claude auto-invoking based on context, or running in a forked subagent.

Use commands when you just want something quick. One markdown file, done. No directory structure needed. Good for prototyping before you need the extra features.

In practice, most people start with commands and graduate to skills once they need more.

---

### What a Full Command Library Looks Like

Power users end up with 30-40+ commands organized by category. Here's what a mature command library looks like:

| Category | Commands | Purpose |
|----------|----------|---------|
| Core Workflow | `/plan`, `/tdd`, `/code-review`, `/build-fix` | Day-to-day development loop |
| Quality | `/verify`, `/quality-gate`, `/test-coverage` | Pre-merge validation checks |
| Documentation | `/update-docs`, `/update-codemaps` | Keep documentation current with code changes |
| Session | `/sessions`, `/checkpoint`, `/compact` | Context and session management |
| Multi-Agent | `/orchestrate`, `/multi-plan` | Coordinated workflows across multiple agents |

You don't need to build all of these on day one. Most teams start with 3-5 commands that address their biggest pain points -- usually something like `/plan`, `/review`, and `/test`. As patterns emerge in your daily work, you'll notice things you keep asking Claude to do repeatedly. Those repetitive prompts are your next commands. The library grows organically from actual usage, not from trying to anticipate every possible need upfront.

---

## Lesson 6: Hooks

### Automating Everything

Hooks are shell commands that run automatically at specific points in Claude Code's lifecycle. Unlike skills -- which Claude decides when to use -- hooks are deterministic. They run every time their trigger event fires.

---

### The Hook Events

Claude Code has 17 hook events:

| Event | When It Fires | Can Block? |
|-------|--------------|------------|
| `SessionStart` | Session begins or resumes | No |
| `UserPromptSubmit` | You submit a prompt, before Claude processes it | Yes |
| `PreToolUse` | Before a tool call executes | Yes |
| `PermissionRequest` | When a permission dialog appears | Yes |
| `PostToolUse` | After a tool call succeeds | No (already ran) |
| `PostToolUseFailure` | After a tool call fails | No |
| `Notification` | When Claude sends a notification | No |
| `SubagentStart` | When a subagent is spawned | No |
| `SubagentStop` | When a subagent finishes | Yes |
| `Stop` | When Claude finishes responding | Yes |
| `TeammateIdle` | When a team member is about to go idle | Yes |
| `TaskCompleted` | When a task is marked completed | Yes |
| `ConfigChange` | When config files change during session | Yes |
| `WorktreeCreate` | When a worktree is being created | Yes |
| `WorktreeRemove` | When a worktree is being removed | No |
| `PreCompact` | Before context compaction | No |
| `SessionEnd` | Session terminates | No |

---

### Exit Codes

Your hook script communicates with Claude Code through exit codes:

- Exit 0 -- Success. Action proceeds. Stdout is parsed for optional JSON output.
- Exit 2 -- Block. The action is prevented. Stderr is fed back to Claude as an error.
- Any other code -- Non-blocking error. Logged but execution continues.

---

### Configuring Hooks

Hooks go in your settings.json file. Here's the structure:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/auto-format.sh"
          }
        ]
      }
    ]
  }
}
```

Three levels: event, then matcher group (filter by tool name), then hook handlers (what to run).

The `matcher` is a regex. `"Write|Edit"` matches either tool. `"Bash"` matches only Bash. Omit the matcher or use `"*"` to match everything.

---

### Practical Hook Examples

Auto-format after file changes:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/format.sh"
          }
        ]
      }
    ]
  }
}
```

The script (`.claude/hooks/format.sh`):
```bash
#!/bin/bash
# Read the JSON input from stdin
INPUT=$(cat)
FILE_PATH=$(echo "$INPUT" | jq -r '.tool_input.file_path // empty')

# Only format JS/TS files
if [[ "$FILE_PATH" == *.js ]] || [[ "$FILE_PATH" == *.ts ]] || [[ "$FILE_PATH" == *.tsx ]]; then
  npx prettier --write "$FILE_PATH" 2>/dev/null
fi

exit 0
```

Block dangerous commands:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/block-dangerous.sh"
          }
        ]
      }
    ]
  }
}
```

The script:
```bash
#!/bin/bash
COMMAND=$(cat | jq -r '.tool_input.command')

if echo "$COMMAND" | grep -q 'rm -rf'; then
  echo "Blocked: rm -rf commands are not allowed" >&2
  exit 2
fi

exit 0
```

---

### Hook Types

Shell commands cover most use cases, but sometimes you want something smarter. Hooks support three types:

- `type: "command"` -- runs a shell command. All the examples above use this.
- `type: "prompt"` -- sends hook context to an LLM for evaluation. The model returns `{"ok": true/false}`, which works well when the decision requires judgment rather than a simple regex check.
- `type: "agent"` -- spawns a full subagent that can use tools like Read, Grep, and Glob to investigate before deciding. This is the heavy option -- use it when verification requires actually reading files or running tests.

Hooks can also run in the background with `"async": true`, which is useful for running tests after file changes without blocking Claude.

---

### Session Management Hooks

The most practical hook pattern is memory persistence across sessions. Claude's context resets between sessions, but hooks let you bridge that gap by saving and restoring state automatically.

The pattern uses three hooks working together:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "node \"$CLAUDE_PROJECT_DIR/.claude/hooks/scripts/session-start.js\""
          }
        ]
      }
    ],
    "PreCompact": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "node \"$CLAUDE_PROJECT_DIR/.claude/hooks/scripts/pre-compact.js\""
          }
        ]
      }
    ],
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "node \"$CLAUDE_PROJECT_DIR/.claude/hooks/scripts/session-end.js\""
          }
        ]
      }
    ]
  }
}
```

- **`SessionStart`** loads saved context from a state file (e.g., `.claude/state/last-session.json`) and outputs it as JSON so Claude picks up where it left off -- recent decisions, open tasks, known issues.
- **`PreCompact`** saves the current working state before Claude compacts the conversation. This is critical because compaction throws away detail; the hook preserves what matters.
- **`SessionEnd`** extracts patterns and insights from the session and persists them for next time.

Notice the scripts use Node.js instead of bash. This is deliberate -- Node.js scripts in a `scripts/` directory work identically on macOS, Linux, and Windows (via WSL), making your hooks portable across your team. Keep the scripts in `.claude/hooks/scripts/` and they'll be versioned alongside the rest of your configuration.

---

## Lesson 7: Custom Agents (Introduction)

### What Are Custom Agents?

Custom agents are specialized AI assistants with their own context window, tools, and personality. When Claude encounters a task matching an agent's description, it delegates to that agent.

Agent files live in `.claude/agents/`:

```markdown
---
description: Reviews code for quality and best practices. Use proactively after code changes.
tools: Read, Grep, Glob, Bash
model: sonnet
---

You are a senior code reviewer. When invoked, analyze the code and provide
specific, actionable feedback on quality, security, and best practices.

Focus on:
1. Run git diff to see recent changes
2. Review modified files
3. Provide feedback by priority (critical, warning, suggestion)
```

The frontmatter gives you fine-grained control over what the agent can do:

- `name` -- unique identifier
- `description` -- tells Claude when to delegate to this agent. Write it well and Claude will route the right tasks here automatically.
- `tools` -- what tools the agent can use (inherits everything if omitted)
- `model` -- `sonnet`, `opus`, `haiku`, or `inherit`
- `permissionMode` -- `default`, `acceptEdits`, `plan`, `dontAsk`, or `bypassPermissions`
- `memory` -- `user`, `project`, or `local` for persistent cross-session memory. This is how agents get smarter over time.
- `skills` -- preload specific skills into the agent's context
- `isolation` -- `"worktree"` for isolated git worktree
- `background` -- `true` to run without blocking

Where agents live:

| Location | Scope |
|----------|-------|
| `.claude/agents/` | This project |
| `~/.claude/agents/` | All your projects |

Use `/agents` to manage them interactively, or create the files manually.

We'll go much deeper on agents in the advanced modules. For now, just know they exist and how the basic file format works.

---

### Real-World Agent Examples

To make agents concrete, here are three production agents with different design tradeoffs. Pay attention to *why* each choice was made -- model selection and tool restrictions aren't arbitrary.

**1. Planner Agent** -- deep reasoning, zero side effects:
```yaml
---
description: Analyzes codebases and creates detailed implementation plans. Use when the user needs architecture decisions or multi-step plans.
tools: Read, Grep, Glob
model: opus
---

You are a senior architect. Analyze the codebase and produce a detailed plan.
Never suggest changes directly -- only produce plans with reasoning.
```
Why opus? Planning requires deep reasoning and long-horizon thinking. Why only Read/Grep/Glob? A planner must never modify code -- it only reads and reasons. Restricting tools makes this guarantee structural, not behavioral.

**2. Security Reviewer Agent** -- specialized checklist, moderate cost:
```yaml
---
description: Reviews code for security vulnerabilities using OWASP guidelines. Use after code changes that touch auth, input handling, or data access.
tools: Read, Grep, Glob, Bash
model: sonnet
---

You are a security specialist. Review code against OWASP Top 10.
Check for: SQL injection, XSS, CSRF, broken auth, secrets in code,
insecure deserialization, and missing input validation.
Run `grep -r` for patterns like API keys, passwords, and tokens.
```
Why sonnet? Security review needs good reasoning but runs frequently -- opus would be too expensive for every PR. Bash access is included so the agent can run `grep` for secret patterns and `npm audit` or similar tools.

**3. Documentation Updater Agent** -- routine work, high volume:
```yaml
---
description: Updates documentation to reflect code changes. Use after features are merged.
tools: Read, Grep, Glob, Edit, Write
model: haiku
---

You update documentation files to match current code. Read the changed files,
then update relevant docs. Keep the existing style and tone.
```
Why haiku? Doc updates are straightforward -- read what changed, update the matching docs. This runs often and the work is routine, so the cheapest capable model keeps costs down. Edit and Write access is needed because this agent actually modifies files.

Module 21 goes deep into agent architecture, including multi-agent coordination, worktree isolation, and agent-to-agent communication patterns.

---

## Hands-On Practice

### Exercise 1: Set Up Project Memory

**Task:** Create a CLAUDE.md for a project you're working on (or a sample project)

```text
1. Create CLAUDE.md in your project root
2. Add: project description, architecture overview
3. Add: common commands (build, test, lint)
4. Add: coding standards and conventions
5. Add: key file paths
6. Keep it under 150 lines
7. Create CLAUDE.local.md with your personal preferences
8. Test it: start a new Claude Code session and ask about project conventions
```

---

### Exercise 2: Create a Custom Skill

**Task:** Build a skill that generates a changelog entry

```text
1. Create .claude/skills/changelog/SKILL.md
2. Add frontmatter: name, description, disable-model-invocation: true
3. Add instructions to:
   - Read recent git commits since last tag
   - Categorize changes (features, fixes, breaking changes)
   - Generate formatted changelog entry
4. Test with: /changelog
```

---

### Exercise 3: Write a Basic Hook

**Task:** Create a PostToolUse hook that runs your linter after file edits

```text
1. Create .claude/hooks/lint-check.sh
2. Make it executable (chmod +x)
3. Read tool_input.file_path from stdin JSON
4. Run your linter on that file
5. Exit 0 (non-blocking, just informational)
6. Add the hook config to .claude/settings.json under PostToolUse
7. Test by asking Claude to edit a file
```

---

## Module 12 Checklist

- [ ] Created a CLAUDE.md file with project conventions
- [ ] Understand the memory hierarchy (project, personal, user, managed)
- [ ] Know how to create path-scoped rules in `.claude/rules/`
- [ ] Understand the 5-tier settings hierarchy
- [ ] Can configure basic permission rules (allow/deny)
- [ ] Created a custom skill with SKILL.md and frontmatter
- [ ] Know the difference between skills and commands
- [ ] Understand hook events and exit codes
- [ ] Can configure a basic hook in settings.json
- [ ] Know what custom agents are and where they live

---

## Best Practices

A few hard-won lessons that'll save you time:

CLAUDE.md is the foundation of everything. Keep it under 150 lines -- every line goes into context every session, so bloat costs you real tokens. Use `@imports` for detailed references instead of cramming everything inline. Update it as your project evolves, and use CLAUDE.local.md for personal preferences that your team doesn't need to see.

Skills work best when they have clear, specific descriptions so Claude knows when to reach for them. Use `disable-model-invocation: true` for anything with side effects -- deploy, ship, anything destructive. You do not want Claude deciding on its own that it's time to deploy. Keep SKILL.md under 500 lines and put details in supporting files.

Hooks are powerful but easy to get wrong. Fair warning: always quote shell variables (`"$VAR"` not `$VAR`), and use `"$CLAUDE_PROJECT_DIR"` for project-relative paths. The biggest pitfall is slow hooks -- they block Claude's execution, so keep them fast. Use `async: true` for long-running tasks like test suites.

Settings have a natural split: put shared team config in `.claude/settings.json` and personal overrides in `.claude/settings.local.json`. Remember that deny rules always win over allow rules -- use them for hard safety boundaries you never want crossed.

---

## What's Next?

You now know how to customize every aspect of Claude Code. Between CLAUDE.md, rules, settings, skills, commands, hooks, and agents, you've got complete control over how Claude behaves in your projects. The real payoff comes over time as your configuration accumulates -- a well-tuned setup means Claude starts every session already knowing how you work.

Next up: Module 13 -- using Claude Code with different programming languages and frameworks.

Want to go deeper? The [Advanced Modules](https://payhip.com/b/8E107) cover custom agents in depth (Module 21), sandbox and plugins (Module 22), and professional development workflows (Module 23).

---

*Module 12 Complete*
