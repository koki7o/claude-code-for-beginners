# Troubleshooting Guide

Things break. Here's how to fix them.

---

## Installation Issues

### Problem: "command not found: claude"

**Symptoms:**
```bash
$ claude
zsh: command not found: claude
```

**Causes:**
- Claude Code not installed or install script didn't finish
- Terminal not restarted after installation
- Install path not in PATH

**Solutions:**

**Solution 1: Run the install script**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Solution 2: Restart your terminal**
Close and reopen your terminal application.

**Solution 3: Check your PATH**
The install script will tell you where Claude Code was installed. Make sure that directory is in your PATH. Add it to `.bashrc`, `.zshrc`, or similar if needed.

---

### Problem: "Permission denied" during installation

**Symptoms:**
```bash
curl -fsSL https://claude.ai/install.sh | bash
# Error: EACCES: permission denied
```

**Solutions:**

**Solution 1: Run with sudo**
```bash
curl -fsSL https://claude.ai/install.sh | sudo bash
```

**Solution 2: Check directory permissions**
Make sure you have write access to the install location. The install script will tell you where it's trying to write.

---

### Problem: Node.js version errors

Claude Code's native installer bundles its own runtime — you don't need Node.js installed separately. If you see a Node.js version error, you may be using the old npm-based install method.

**Solution:** Reinstall using the native installer:

```bash
# macOS/Linux
curl -fsSL https://claude.ai/install.sh | bash

# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
```

> **Note:** Node.js may still be needed for your own projects (running `npm`, MCP servers, etc.) — but Claude Code itself does not require it.

---

## Authentication Issues

### Problem: Can't log in or authenticate

Claude Code supports two authentication methods:

1. **Claude subscription** (Pro/Max/Teams/Enterprise) -- log in via browser
2. **Anthropic API key** -- set as environment variable

**If using a subscription:**

Run `claude` and it will open a browser window to log in. If the browser doesn't open, press `c` to copy the login URL. Make sure you have an active Pro, Max, Teams, or Enterprise plan — the free Claude.ai plan does **not** include Claude Code.

To log out and re-authenticate: type `/logout` in Claude Code.

**If using an API key:**

Set the environment variable before starting Claude Code:

```bash
# macOS/Linux (current session)
export ANTHROPIC_API_KEY="your-api-key-here"

# macOS/Linux (persistent — add to ~/.bashrc or ~/.zshrc)
echo 'export ANTHROPIC_API_KEY="your-api-key-here"' >> ~/.zshrc
source ~/.zshrc  # Reloads your shell config so the change takes effect

# Windows PowerShell
$env:ANTHROPIC_API_KEY="your-api-key-here"
```

Get your API key from [console.anthropic.com](https://console.anthropic.com) — it starts with `sk-ant-`.

---

### Problem: "Invalid API key"

**Symptoms:**
```
Error: Invalid API key
```

**Solutions:**

1. **Get a fresh API key** from [console.anthropic.com](https://console.anthropic.com)
2. **Check for typos:** keys start with `sk-ant-`, no spaces or extra quotes
3. **Verify it's set:** `echo $ANTHROPIC_API_KEY`
4. **Consider switching to subscription login** — run `unset ANTHROPIC_API_KEY` then `claude` to log in via browser instead

---

### Problem: "Insufficient credits" or usage limits

**If using an API key:**
- Check your balance at [console.anthropic.com](https://console.anthropic.com) under "Usage"
- Add a payment method and purchase credits

**If using a subscription:**
- Claude Pro has usage limits that reset over time
- Claude Max ($100/month) has significantly higher limits
- Check `/status` inside Claude Code to see your current usage

---

## Runtime Issues

### Problem: Claude Code freezes or hangs

**Symptoms:**
- No response after sending a prompt
- Cursor isn't blinking
- No output for minutes

**Solutions:**

**Solution 1: Check internet connection**
```bash
ping anthropic.com
```

**Solution 2: Increase timeout (if available)**
```bash
# In your request, specify longer timeout
claude --timeout 300
```

**Solution 3: Kill and restart**
Press `Ctrl+C` to stop, then start again.

**Solution 4: Check for background processes**
```bash
# List background tasks
/tasks

# Kill if needed
```

---

### Problem: "Rate limit exceeded"

**Symptoms:**
```
Error: Rate limit exceeded. Please try again later.
```

**Causes:**
- Too many requests in a short time
- API limits reached

**Solutions:**

1. **Wait and retry:**
   - Give it 60 seconds, then try again

2. **Reduce request frequency:**
   - Combine multiple small requests into larger ones
   - Fewer, bigger requests beat many small ones

3. **Check your usage:**
   - Visit console.anthropic.com
   - Review your usage limits

---

### Problem: Changes not being saved

**Symptoms:**
- Files show as modified but aren't actually changed
- Code disappears after closing

**Solutions:**

**Solution 1: Check file permissions**
```bash
ls -la [filename]
```

If you don't have write permission:
```bash
chmod u+w [filename]
```

**Solution 2: Verify working directory**
```
Show me the current working directory
List all files here
```

**Solution 3: Explicitly save**
```
Please write the changes to [filename]
Show me [filename] to verify
```

---

### Problem: Tools not working

**Symptoms:**
- Read/Write/Edit tools fail
- Bash commands don't execute
- Error: "Tool execution failed"

**Solutions:**

**For file tools (Read/Write/Edit):**
1. **Check file paths:**
   - Use absolute paths: `/home/user/project/file.js`
   - Or make sure you're in the right directory

2. **Check permissions:**
   ```bash
   ls -la
   ```

3. **Check disk space:**
   ```bash
   df -h
   ```

**For Bash tool:**
1. **Check command syntax:**
   ```
   Please explain this command before running it
   ```

2. **Test the command manually:**
   ```bash
   # Exit Claude Code and test
   [your command]
   ```

3. **Check for required programs:**
   ```bash
   which [program]
   ```

---

## Code Execution Issues

### Problem: "Module not found" errors

**Symptoms:**
```
Error: Cannot find module 'express'
```

**Solutions:**

**Solution 1: Install dependencies**
```
Install all dependencies from package.json
```

Or manually:
```bash
npm install
# or
pip install -r requirements.txt
```

**Solution 2: Check node_modules**
```bash
ls node_modules/
```

If it's empty or missing:
```bash
rm -rf node_modules package-lock.json
npm install
```

**Solution 3: Check import paths**
```
Check all import statements in [file]
Fix any incorrect paths
```

---

### Problem: Syntax errors after Claude Code changes

**Symptoms:**
```
SyntaxError: Unexpected token
```

**Solutions:**

**Solution 1: Review the changes**
```
Show me what you changed in [file]
```

**Solution 2: Revert changes**
```
Please undo the changes to [file]
```

Or with Git:
```bash
git checkout [file]
```

**Solution 3: Fix the syntax**
```
There's a syntax error in [file] at line [X]. Please fix it.
```

---

### Problem: Tests failing after changes

**Symptoms:**
- Tests passed before
- Tests fail after Claude Code modifications

**Solutions:**

**Solution 1: Check what changed**
```
Show me the git diff
What files did you modify?
```

**Solution 2: Review test failures**
```
Run the tests and show me the output
Explain why these tests are failing
```

**Solution 3: Fix the tests**
```
Fix the code to make the tests pass
```

Or if the new behavior is intentional:
```
Update the tests to match the new implementation
```

---

## Git Integration Issues

### Problem: Git commands failing

**Symptoms:**
```
Error: fatal: not a git repository
```

**Solutions:**

**Solution 1: Initialize Git**
```
Initialize a git repository in this directory
```

Or manually:
```bash
git init
```

**Solution 2: Check Git installation**
```bash
git --version
```

If it's not installed:
- **macOS:** `brew install git`
- **Ubuntu:** `sudo apt-get install git`
- **Windows:** Download from git-scm.com

**Solution 3: Verify you're in the right directory**
```bash
pwd
ls -la .git
```

---

### Problem: Cannot commit changes

**Symptoms:**
```
Error: Please tell me who you are
```

**Solution:**

Git needs to know who you are before it'll let you commit.

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Or ask Claude Code:
```
Configure git with my name and email
```

---

## Performance Issues

### Problem: Claude Code is slow

**Symptoms:**
- Long delays between request and response
- Tools take a long time to execute

**Solutions:**

**Solution 1: Check internet speed**
```bash
speedtest-cli
# or visit fast.com
```

**Solution 2: Simplify requests**
- Break big tasks into smaller ones
- Be more specific -- it cuts down on thinking time

**Solution 3: Use agents for multi-step tasks**
```
Use an agent to [complex task]
```
Agents can handle multi-step work more efficiently.

**Solution 4: Close other applications**
- Free up memory and CPU
- Close other Claude Code sessions if you have any running

---

### Problem: Large file operations are slow

**Symptoms:**
- Reading large files takes forever
- Editing large files is slow

**Solutions:**

**Solution 1: Read specific sections**
```
Read lines 100-200 of [file]
Show me just the [function/class] in [file]
```

**Solution 2: Use search instead**
```
Use grep to find [pattern] in [file]
```

**Solution 3: Split large files**
```
Help me refactor this large file into smaller modules
```

---

## MCP Server Issues

### Problem: MCP server not connecting

**Symptoms:**
```
Error: Failed to connect to MCP server
```

**Solutions:**

**Solution 1: Check server is running**
```bash
# Check if server process is running
ps aux | grep mcp
```

**Solution 2: Verify configuration**
Check your MCP config in `.mcp.json` (project-level) or `~/.claude/settings.json` (user-level):
```json
{
  "mcpServers": {
    "server-name": {
      "command": "path-to-server",
      "args": []
    }
  }
}
```

**Solution 3: Test server manually**
```bash
# Run the MCP server command manually
/path/to/mcp-server
```

**Solution 4: Check server logs**
Run `claude doctor` to check for issues, or look in `~/.claude/` for error logs.

---

## CLAUDE.md and Rules Issues

### Problem: CLAUDE.md not being followed

**Symptoms:**
- Claude Code ignores conventions you put in CLAUDE.md
- Code style doesn't match your preferences

**Solutions:**

**Solution 1: Check file location**
CLAUDE.md must be in the project root -- the same directory where you start Claude Code.
```bash
ls CLAUDE.md
```

**Solution 2: Check formatting**
Make sure your CLAUDE.md uses clear, imperative language:
```markdown
# Good
- Use async/await, never callbacks
- All API responses use { data: T } format

# Less effective
- It would be nice to use async/await
- Consider using a consistent response format
```

**Solution 3: Be specific, not vague**
"Write clean code" doesn't mean anything concrete. "Use named exports, camelCase for functions, PascalCase for classes" does.

---

### Problem: Rules not applying to files

**Symptoms:**
- Rules in `.claude/rules/` aren't being enforced

**Solutions:**

**Solution 1: Check glob patterns**
Make sure the `globs` field in your rule frontmatter matches the files you're working on:
```markdown
---
description: TypeScript rules
globs: ["src/**/*.ts", "src/**/*.tsx"]
---
```

**Solution 2: Verify file location**
Rules must be in `.claude/rules/` in your project root:
```bash
ls .claude/rules/
```

**Solution 3: Check frontmatter format**
The frontmatter must use valid YAML with `---` delimiters:
```markdown
---
description: Your description here
globs: ["pattern"]
---
```

---

## Getting More Help

### Ask Claude Code itself

You can just ask Claude Code to help you troubleshoot directly:

```
I'm getting this error: [error message]
Can you help me understand and fix it?
```

```
Something's not working correctly. Can you help me debug?
```

### Check the docs

- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [Claude API Docs](https://docs.anthropic.com)
- [GitHub Issues](https://github.com/anthropics/claude-code/issues)

### File a GitHub issue

If you're stuck, search the existing issues first. If nothing matches, open a new one with:
  - A clear problem description
  - Steps to reproduce
  - Error messages
  - System information (see below)

### System information to include

When reporting issues, grab this info:

```bash
# Claude Code version
claude --version

# Run diagnostics
claude doctor

# Operating system
uname -a  # macOS/Linux
# or: systeminfo  (Windows)
```

---

## Preventing Problems

1. **Use version control (Git).** Commit working code before making changes so you can revert if something breaks.

2. **Review changes before accepting.** Check Edit tool outputs and verify Bash commands before they run.

3. **Test after each change.** Don't wait until you've made ten changes to find out the first one broke something.

4. **Commit to Git regularly** and push to a remote. That's your safety net.

5. **Be specific in your requests.** Vague prompts lead to vague results.

6. **Ask for explanations.** If you don't understand what Claude Code did, ask. You'll learn to spot problems faster.

7. **Start simple, then add complexity.** Get the basics working before piling on features.
