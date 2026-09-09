<div align="center">

![Module 1](https://img.shields.io/badge/Module_1-32CD32?style=for-the-badge&labelColor=1a1a2e)
![Time](https://img.shields.io/badge/⏱_20_min-555555?style=for-the-badge&labelColor=1a1a2e)
![Difficulty](https://img.shields.io/badge/Beginner-32CD32?style=for-the-badge&labelColor=1a1a2e)

# Welcome to Claude Code

**Get comfortable with Claude Code and understand what it can do**

[Home](README.md) · [Next →](module-02-starting-your-first-project.md)

</div>

---

## What You'll Learn

By the end of this you'll know what Claude Code actually is, have it installed, and have talked it into writing a small Python script for you. About 20 minutes.

You'll need Python for the exercise at the end. Most Macs and Linux machines already have it. If yours doesn't, grab it from [python.org](https://python.org) and come back.

---

## Lesson 1: What is "AI Pair Programming"?

### The Short Version

You describe what you want. The AI builds it.

You're still the one driving - you decide what to make and whether it's any good - but now you've got a partner that can write code, read your files, run commands, and explain what it's doing while it does it.

Here's the difference.

**The old way:** you write every line yourself, dig through docs, hunt bugs by hand, and keep all the syntax and APIs in your head. One small typo, one hour gone.

**With Claude Code:** you say "create an HTTP server on port 3000" and it writes the code, creates the file, and runs it if you ask. When something breaks, you fix it together.

### A Quick Example

Instead of this:
```bash
# Search Google for "how to create HTTP server in Node.js"
# Read documentation
# Copy-paste examples
# Debug why it's not working
# Fix syntax errors
# Test manually
```

You type:
> "Create a simple HTTP server in Node.js that returns 'Hello World' on port 3000"

It creates the file, writes the code, tells you what it did, and can run it. That's the whole job.

---

## Lesson 2: Installing Claude Code

### What You Need

Before you install:
- A computer running macOS 13+, Windows 10+, or Linux (Ubuntu 20.04+, Debian 10+)
- Internet connection
- Terminal/command-line access (on macOS: open **Terminal** from Applications > Utilities. On Windows: open **PowerShell** from the Start menu. On Linux: look for **Terminal** in your applications menu)
- A Claude account (see Step 1 below)
- **Windows only:** [Git for Windows](https://git-scm.com/downloads/win) must be installed first -- this provides the terminal environment that Claude Code needs on Windows
- **For this module's exercises:** [Python](https://python.org/downloads) (most macOS and Linux systems have it pre-installed; Windows users may need to install it)

### Step-by-Step Installation

#### Step 1: Get a Claude Account

You need one of these to use Claude Code:

- **Claude Pro or Max subscription** ($20/month or $100/month at [claude.ai](https://claude.ai)) -- the easiest option. Subscribe, and you're ready.
- **Claude for Teams or Enterprise** -- if your company has a plan, ask your admin for an invite.
- **Anthropic API key** -- pay-per-use via [console.anthropic.com](https://console.anthropic.com). Sign up, create an API key (starts with `sk-ant-...`), and keep it safe.

> **Note:** The free Claude.ai plan does **not** include Claude Code access. You need at least a Pro subscription or an API key.

#### Step 2: Install Claude Code

**On macOS or Linux:**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**On Windows (PowerShell):**
```powershell
irm https://claude.ai/install.ps1 | iex
```

**On Windows (CMD):**
```batch
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

> **What does this command do?** `curl` downloads the install script from the internet, and `| bash` (or `| iex` on Windows) runs it. This is the standard way to install command-line tools.

The install script handles everything -- no Node.js required. It downloads Claude Code and sets it up on your system.

> **Alternative install methods:** You can also install via `brew install --cask claude-code` (macOS/Linux) or `winget install Anthropic.ClaudeCode` (Windows).

#### Step 3: Log In

Start Claude Code for the first time:

```bash
claude
```

**If you have a Claude subscription** (Pro/Max/Teams/Enterprise): A browser window opens automatically. Log in with your Claude account and you're done.

**If you're using an API key:** Set it as an environment variable before starting:

On macOS/Linux:
```bash
export ANTHROPIC_API_KEY="your-api-key-here"
```

On Windows (PowerShell):
```powershell
$env:ANTHROPIC_API_KEY="your-api-key-here"
```

> **Note:** These commands only last until you close your terminal. To make the key permanent, just ask Claude Code itself: say "Help me set up my API key permanently" and it'll walk you through it for your exact system.

#### Step 4: Verify Installation

```bash
claude --version
```

You should see a version number. If you get an error, restart your terminal and try again. You can also run `claude doctor` to check your setup.

---

## Lesson 3: Understanding the Claude Code Interface

### What's a CLI?

A CLI - command-line interface - is a text-based way to use software. Instead of clicking buttons, you type. Claude Code's is conversational though: you talk to it in plain English, not cryptic commands.

### Your First Look

Start Claude Code by typing:

```bash
claude
```

You'll see something like:
```
╭───────────────────────────────────────────────────╮
│                                                   │
│   Claude Code v1.0.0                             │
│   AI-powered development assistant               │
│                                                   │
╰───────────────────────────────────────────────────╯

Working directory: /home/user/projects

How can I help you today?
>
```

The `>` is where you type. That's the whole interface - a conversation.

### What's on Screen

- **Header** -- version info, your current working directory, status
- **Prompt (`>`)** -- where you type your requests, in plain English
- **Response area** -- Claude's answers, tool usage, and output appear above the prompt

### Try It Out

Type this at the prompt:

```
Hello! Can you explain what you can do?
```

Hit Enter. You'll get a friendly rundown of what it can do. Talk to it like you'd talk to a coworker, not a search box.

---

## Lesson 4: Basic Terminology

A few terms you'll keep running into. Don't memorize them - just come back here when a word trips you up.

**Session** -- one conversation with Claude Code, start to finish. You open it, do your work, exit. That's a session.

**Working Directory** -- the folder Claude Code is working in. Start it in `/home/user/my-project` and that's where it reads and writes.

**Tool** -- one specific thing Claude Code can do. Reading a file, running a command, editing code - each is a "tool," and you'll see it name the one it's using as it works.

**Prompt** -- your message. "Create a Python script that prints Hello World" is a prompt.

**Tool Use / Tool Call** -- the moment Claude actually reaches for a tool. You'll see it in the output, something like "Using Write tool" or "Using Bash tool."

**Agent** -- a specialized helper for a bigger job. Think of it as handing a specific task to an expert. Claude Code can spin up a sub-agent to go research part of your codebase while the main conversation keeps going.

**Context** -- everything Claude knows about your project so far. Every file it reads, every command it runs, all of it feeds the conversation.

**MCP (Model Context Protocol) Server** -- an extension that gives Claude Code new powers, like talking to a database or an external service. Think plugins. More in Module 11.

**CLAUDE.md** -- a file you drop in your project's main folder that tells Claude Code how to work with it. A briefing document, basically - conventions, patterns, project-specific rules. We go deep on this later.

---

## Hands-On Practice: Your First Claude Code Session

Let's build something. It's deliberately tiny - the point isn't the program, it's getting the workflow into your hands.

### Practice Project: A Simple Python Script

#### Step 1: Create a Project Directory

In your terminal (before starting Claude Code):

```bash
mkdir hello-claude
cd hello-claude
```

`mkdir` creates a folder, `cd` moves into it.

#### Step 2: Start Claude Code

```bash
claude
```

You're in a session now.

#### Step 3: Your First Request

Type:

```
Create a Python script called hello.py that asks for the user's name and greets them
```

Hit Enter.

You'll watch Claude Code take your request, reach for the Write tool to create the file, and confirm it's done.

#### Step 4: Check What Was Created

Type:

```
Can you show me what's in hello.py?
```

Claude Code uses the Read tool and shows you the contents.

#### Step 5: Run the Program

Type:

```
Run the hello.py script
```

Claude Code uses the Bash tool to run it (usually `python hello.py` or `python3 hello.py`, depending on your system). It asks for your name, you type it, you get the greeting.

#### Step 6: Make a Change

Type:

```
Make the greeting more enthusiastic with exclamation marks
```

Watch it reach for the Edit tool, show you what changed, and update the file. Only the greeting moves - everything else stays put.

You just created a file, ran a program, and modified code, all by talking.

---

## Understanding What Just Happened

That's the pattern behind basically everything you'll do in Claude Code:

**Creating the script:** Claude read your request, picked the Write tool, made the file, confirmed.

**Viewing the file:** Claude used the Read tool to open it and show you the contents.

**Running it:** Claude used the Bash tool to run the command and showed you the output.

**Editing it:** Claude used the Edit tool to make one precise change, showing you the diff. It touched only what needed touching.

Describe what you want, watch the tools work, check the result. That's the core loop. Everything else in this course is a variation on it.

---

## Module 1 Checklist

Before moving on, make sure you can:

- [ ] Explain what AI pair programming is in your own words
- [ ] Install Claude Code on your system
- [ ] Start a Claude Code session in your terminal
- [ ] Understand the basic terms (tool, prompt, session, context)
- [ ] Create a simple file using Claude Code
- [ ] Make changes to a file
- [ ] Run a program with Claude Code's help

---

## Common Questions

**Do I need to know how to code?**
No, but a little understanding helps you steer better. You'll pick things up as you go.

**Is Claude Code free?**
The tool itself is free to install. To use it you need either a Claude Pro/Max subscription ($20-$100/month) or an Anthropic API key (pay-per-use). The free Claude.ai plan doesn't include Claude Code access.

**What if I make a mistake in my request?**
Just clarify, ask it to undo the change, or make a new request. Nothing here is permanent.

**What languages does it support?**
Python, JavaScript, TypeScript, Java, Go, Rust, and plenty more.

**What if Claude Code does something wrong?**
You're in control. Review changes before you accept them, and you can always ask it to fix or revert.

**How do I exit?**
Type `/exit` or press Ctrl+C. Your files are already saved.

---

## Tips

1. **Be conversational** -- talk to it like a colleague, not a search engine
2. **Be specific** -- more detail, better results
3. **Ask questions** -- if you don't get something, ask it to explain
4. **Review changes** -- always look at what changed, that's how you actually learn
5. **Experiment** -- try things. You can't break anything.
6. **Read the output** -- Claude Code explains what it's doing, and that's the free lesson
7. **Create a CLAUDE.md early** -- even a simple one helps. Try: "Create a CLAUDE.md file that says I prefer Python and clear variable names." We go deep on this later.

---

## Troubleshooting

**"Command not found: claude"** -- Make sure the install script finished, then restart your terminal.

**"API key not found"** -- Either log in with your Claude subscription (`claude` opens a browser), or set `ANTHROPIC_API_KEY` as an environment variable.

**"Permission denied"** -- On macOS/Linux, try `curl -fsSL https://claude.ai/install.sh | sudo bash`. On Windows, make sure you've got [Git for Windows](https://git-scm.com/downloads/win) installed.

**Claude Code seems slow** -- Normal. AI processing takes a moment, especially on bigger requests.

**No output** -- Check you actually pressed Enter, and check your internet connection.

---

> **Curious what full projects built with Claude Code look like?** The [Real Projects Pack](https://payhip.com/b/dFXWO) walks you through 14 complete builds -- from todo apps to full-stack SaaS.

---

<div align="center">

[🏠 Home](README.md) · [Next Module →](module-02-starting-your-first-project.md)

</div>
