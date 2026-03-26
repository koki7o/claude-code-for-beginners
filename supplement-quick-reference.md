# Quick Reference Guide

**The stuff you'll actually look up mid-session.**

---

## Starting and Stopping

### Start Claude Code
```bash
claude
```

### Start in a specific directory
```bash
cd /path/to/project
claude
```

### Exit Claude Code
```
/exit
```
Or press `Ctrl+C`

---

## Essential Commands

### Built-in Slash Commands

| Command | Description |
|---------|-------------|
| `/exit` | Exit Claude Code |
| `/help` | Show help information |
| `/clear` | Clear conversation history |
| `/tasks` | Show running background tasks |

---

## Common Prompts

### File Operations

**Create a new file:**
```
Create a file called [filename] that [does what]
```

**Read a file:**
```
Show me [filename]
What's in [filename]?
```

**Edit a file:**
```
In [filename], change [X] to [Y]
Add [feature] to [filename]
Fix the [issue] in [filename]
```

**Find files:**
```
Find all [type] files
Show me all files containing [text]
List all test files
```

---

### Code Operations

**Understand code:**
```
Explain what [filename/function] does
How does [feature] work in this codebase?
What does this code do?
```

**Write new code:**
```
Create a [function/class/component] that [does what]
Write a [type] function for [purpose]
```

**Fix issues:**
```
Fix the bug in [filename]
Debug why [issue] is happening
The app crashes when [scenario], help me fix it
```

**Refactor:**
```
Refactor [code/file] to [improvement]
Improve the structure of [file]
Make this code more readable
```

---

### Project Operations

**Initialize project:**
```
Create a new [project type] project
Set up a [framework] application with [features]
Initialize a [language] project with [dependencies]
```

**Install dependencies:**
```
Install [package name]
Add [library] to this project
Install all dependencies
```

**Run commands:**
```
Run the application
Start the development server
Execute the tests
Build the project
```

---

### Git Operations

**Status and changes:**
```
Show me git status
What files have changed?
Show me the diff
```

**Commit:**
```
Create a commit with these changes
Write a commit message for my changes
```

**Pull Request:**
```
Create a pull request for these changes
Write a PR description
```

**Branches:**
```
Create a new branch called [name]
Switch to [branch]
```

---

### Understanding Codebases

**Explore:**
```
Help me understand this codebase
Explain the project structure
What does this project do?
```

**Find code:**
```
Where is [feature] implemented?
Find all uses of [function/class]
Show me where [X] is defined
```

**Dependencies:**
```
What dependencies does this use?
What version of [package] is installed?
```

---

### Testing and Debugging

**Run tests:**
```
Run all tests
Run tests for [file/feature]
Create tests for [code]
```

**Debug:**
```
Help me debug [issue]
Why is [error] happening?
This code isn't working: [description]
```

**Errors:**
```
Explain this error message: [error]
Fix this error: [error]
```

---

### Documentation

**Create docs:**
```
Write a README for this project
Document this function
Add comments to this code
```

**Generate:**
```
Create API documentation
Write usage examples
Generate a changelog
```

---

## Tool-Specific Requests

You don't usually need to name tools directly -- Claude Code picks the right one. But if you want to steer it, here's how.

**Read tool:**
```
Read [filename] and show me [specific part]
```

**Write tool:**
```
Write a new file [filename] with [content]
```

**Edit tool:**
```
Edit [filename] to add [feature]
```

**Grep tool:**
```
Search for [text] in all files
Find all [pattern] occurrences
```

**Glob tool:**
```
Find all files matching [pattern]
List all *.js files
```

**Bash tool:**
```
Run [command]
Execute [script]
```

**Task/Agent:**
```
Explore this codebase thoroughly
Plan the implementation for [feature]
Research and implement [solution]
```

---

## Effective Prompting Patterns

These are reusable shapes for prompts. Fill in the blanks and you're good.

### The Specific Pattern
```
In [file], add a [feature] that [does X] when [condition]
```

### The Exploratory Pattern
```
Help me understand [concept/code], then [action]
```

### The Iterative Pattern
```
Create a basic [thing]
[After reviewing] Now add [enhancement]
[After testing] Fix [issue]
```

### The Context Pattern
```
I'm building [project]. I need to [goal].
The current structure is [description].
Please [specific request].
```

---

## Authentication

### Option 1: Claude Subscription (Pro/Max/Teams/Enterprise)
```bash
# Just run claude — it opens a browser to log in
claude
```

### Option 2: API Key
```bash
# macOS/Linux
export ANTHROPIC_API_KEY="your-key"

# Windows PowerShell
$env:ANTHROPIC_API_KEY="your-key"
```

### Check auth status
```bash
# Inside Claude Code, type:
/status
```

## Configuration

### Config file location
```
~/.claude/settings.json
```

### Set config values
```bash
claude config set [key] [value]
```

---

## CLAUDE.md and Rules

### Creating CLAUDE.md
```
Create a CLAUDE.md for this project with:
- Build/test/lint commands
- Code style preferences
- Architecture patterns to follow
- Common pitfalls to avoid
```

### Rules Directory
```
.claude/rules/
├── code-style.md        # Formatting conventions
├── error-handling.md    # Error handling standards
├── testing.md           # Testing requirements
└── security.md          # Security rules
```

### Rule File Format
```markdown
---
description: What this rule enforces
globs: ["src/**/*.ts"]
---

Your rules here as bullet points
```

### Skills and Hooks

**Create a skill:**
```
.claude/skills/skill-name/SKILL.md with frontmatter:
---
name: skill-name
description: What it does
---
```

**Common hooks** (add to `.claude/settings.json`):
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{ "type": "command", "command": "npm run lint --quiet 2>/dev/null || true" }]
      }
    ],
    "Stop": [
      {
        "hooks": [{ "type": "command", "command": "echo 'Session complete'" }]
      }
    ]
  }
}
```

---

## Model Routing

| Task Type | Best Model | Why |
|-----------|-----------|-----|
| Quick scans, formatting | Haiku | Fast, cheap |
| Daily coding, reviews | Sonnet | Good balance |
| Architecture, complex bugs | Opus | Deep reasoning |

```
Use a Haiku agent to scan for unused imports
Use a Sonnet agent to review the auth module
Use an Opus agent to plan the database migration
```

---

## Best Practices

### Do's

- Be specific about what you want
- Give context about your project
- Review changes before accepting
- Ask for explanations when you're learning
- Use version control (Git)
- Test changes incrementally
- Break big tasks into steps

### Don'ts

- Don't give vague instructions
- Don't skip testing after changes
- Don't blindly accept all changes
- Don't forget to commit working code
- Don't ask for too many changes at once
- Don't ignore error messages

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Exit Claude Code |
| `Ctrl+D` | End input |
| `Up Arrow` | Previous command |
| `Down Arrow` | Next command |

---

## Common File Patterns

### Glob Patterns

```
*.js                 # All .js files in current directory
**/*.py              # All .py files recursively
src/**/*.tsx         # All .tsx files in src/
**/test/**           # All files in test directories
**/*test*            # All files with 'test' in name
```

### Grep Patterns (Regex)

```
function.*login      # Functions containing 'login'
import.*from         # Import statements
TODO|FIXME           # TODO or FIXME comments
class\s+\w+          # Class definitions
```

---

## Troubleshooting Quick Fixes

### Claude Code won't start
```bash
# Check version
claude --version

# Run diagnostics
claude doctor

# Reinstall (macOS/Linux)
curl -fsSL https://claude.ai/install.sh | bash

# Reinstall (Windows PowerShell)
irm https://claude.ai/install.ps1 | iex
```

### Authentication not working
```bash
# If using subscription: log out and log back in
# Inside Claude Code, type: /logout
# Then restart: claude

# If using API key: set it again
export ANTHROPIC_API_KEY="your-key"

# Check current auth status inside Claude Code:
# /status
```

### Changes not applying
```
Please show me the file to verify the changes
```

### Command failed
```
Can you explain the error and try a different approach?
```

---

## Project-Specific Templates

### React Component
```
Create a React functional component called [Name] that:
- Accepts props: [list props]
- Displays [UI elements]
- Handles [events]
```

### API Endpoint
```
Add a new [HTTP method] endpoint at [path] that:
- Accepts [parameters]
- Returns [data]
- Handles errors
```

### Database Model
```
Create a [ORM] model for [entity] with fields:
- [field]: [type]
- [field]: [type]
Include validation and relationships.
```

### Test Suite
```
Create tests for [function/component] that verify:
- [test case 1]
- [test case 2]
- Edge cases and errors
```

---

## Language-Specific Patterns

### Python
```
Create a Python script that [purpose]
Use [libraries]
Include error handling and logging
```

### JavaScript/Node
```
Create a Node.js [type] using [framework]
Use ES6+ syntax
Include async/await for async operations
```

### TypeScript
```
Create a TypeScript [type] with proper types
Use interfaces for [data structures]
Enable strict mode
```

---

## Quick Debugging Checklist

When something's broken, work through this:

1. **Read the error:** `What does this error mean?`
2. **Check the file:** `Show me [filename] around line [X]`
3. **Find similar code:** `Find other places where [X] works`
4. **Check dependencies:** `Are all dependencies installed?`
5. **Test in isolation:** `Create a simple test for [feature]`
6. **Ask for help:** `Help me debug [specific issue]`

---

## Resources

### Official Documentation
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [Claude API Docs](https://docs.anthropic.com)

### This Course
- [Main README](README.md) - Course overview
- [Troubleshooting Guide](supplement-troubleshooting.md) - Detailed solutions
- [Challenge Solutions](supplement-challenge-solutions.md) - Practice answers
