# Module 1: Welcome to Claude Code - Your First Steps

**Goal:** Get comfortable with Claude Code and understand what it can do

**Estimated Time:** 20-30 minutes

---

## What You'll Learn

This module covers the basics: what Claude Code actually is, how to install it, how to talk to it, and the jargon you'll run into along the way. We'll also build a small Python script together so you can see the whole workflow in action.

---

## Lesson 1: What is "AI Pair Programming"?

### The Short Version

AI pair programming means you describe what you want, and an AI helps you build it. You're still in the driver's seat -- you decide what to build and how -- but you've got a knowledgeable partner that can write code, read your files, run commands, and explain what's happening.

Here's the contrast:

**The traditional way:** You write every line yourself, dig through docs, manually hunt for bugs, and keep all the syntax and APIs in your head. A small typo can cost you an hour.

**With Claude Code:** You say "create an HTTP server on port 3000" and it writes the code, creates the file, and can even run it for you. When something breaks, you debug it together.

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

Claude Code creates the file, writes the code, explains what it did, and can run it. That's it.

---

## Lesson 2: Installing Claude Code

### What You Need

Before installing:
- A computer running macOS 13+, Windows 10+, or Linux (Ubuntu 20.04+, Debian 10+)
- Internet connection
- Terminal/command-line access
- A Claude account (see Step 1 below)
- **Windows only:** [Git for Windows](https://git-scm.com/downloads/win) must be installed first

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

To make it permanent, add that line to your shell config file (`.bashrc`, `.zshrc`, `.profile`, etc.).

#### Step 4: Verify Installation

```bash
claude --version
```

You should see a version number. If you get an error, restart your terminal and try again. You can also run `claude doctor` to check your setup.

---

## Lesson 3: Understanding the Claude Code Interface

### What's a CLI?

A CLI (Command Line Interface) is a text-based way to interact with software. Instead of clicking buttons, you type commands. Claude Code's CLI is conversational though -- you talk to it in plain English.

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

The `>` is where you type. That's your conversation interface.

### What's on Screen

- **Header** -- version info, your current working directory, status
- **Prompt (`>`)** -- where you type your requests, in plain English
- **Response area** -- Claude's answers, tool usage, and output appear above the prompt

### Try It Out

Type this at the prompt:

```
Hello! Can you explain what you can do?
```

Hit Enter. You'll get a friendly rundown of capabilities. Claude Code is conversational -- just talk to it like you'd talk to a coworker.

---

## Lesson 4: Basic Terminology

Here are the key terms you'll encounter. You don't need to memorize these -- just come back here when you see a word you don't recognize.

**Session** -- One conversation with Claude Code, from start to finish. You start Claude Code, do your work, and exit. That's a session.

**Working Directory** -- The folder Claude Code is operating in. If you start Claude Code in `/home/user/my-project`, that's where it reads and writes files.

**Tool** -- A specific capability Claude Code has. Reading files, running commands, editing code -- each of these is a "tool." You'll see Claude mention which tool it's using as it works.

**Prompt** -- Your message or request. "Create a Python script that prints Hello World" is a prompt.

**Tool Use / Tool Call** -- When Claude actually does something with one of its tools. You'll see this in the output -- something like "Using Write tool" or "Using Bash tool."

**Agent** -- A specialized helper for complex tasks. Think of it as delegating a specific job to an expert. The "Explore" agent, for instance, helps you understand large codebases.

**Context** -- What Claude knows about your project so far. Every file it reads, every command it runs -- that all becomes context for the conversation.

**MCP Server** -- An extension that gives Claude Code new capabilities, like connecting to databases or external services. Think plugins.

**CLAUDE.md** -- A special file you can put in your project root that tells Claude Code how to work with your project. Think of it as a briefing document -- coding conventions, preferred patterns, project-specific instructions. We'll cover this in detail in later modules.

---

## Hands-On Practice: Your First Claude Code Session

Let's build something. It'll be simple -- the point is to get you comfortable with the workflow.

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

You'll see Claude Code acknowledge your request, use the Write tool to create the file, and confirm it's done.

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

Claude Code uses the Bash tool to run `python hello.py`. The script asks for your name, you type it, and you see the greeting.

#### Step 6: Make a Change

Type:

```
Make the greeting more enthusiastic with exclamation marks
```

Watch Claude Code use the Edit tool, show you what changed, and update the file. Only the greeting changes -- everything else stays put.

You just created a file, ran a program, and modified code, all through conversation.

---

## Understanding What Just Happened

Here's the pattern you'll use for basically everything in Claude Code:

**Creating the script:** Claude understood your request, chose the Write tool, created the file, and confirmed.

**Viewing the file:** Claude used the Read tool to open the file and display its contents.

**Running it:** Claude used the Bash tool to execute the command and showed you the output.

**Editing it:** Claude used the Edit tool to make a precise change, showing you the diff. It only touched what needed changing.

This workflow -- describe what you want, watch the tools do their thing, review the results -- is the core loop. Everything else builds on it.

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
No, but some basic understanding helps you guide Claude Code better. You'll pick things up as you go.

**Is Claude Code free?**
Claude Code itself is free to install. To use it, you need either a Claude Pro/Max subscription ($20-$100/month) or an Anthropic API key (pay-per-use). The free Claude.ai plan does not include Claude Code access.

**What if I make a mistake in my request?**
Just clarify, ask to undo changes, or make a new request. Nothing is permanent.

**What languages does it support?**
Python, JavaScript, TypeScript, Java, Go, Rust, and many more.

**What if Claude Code does something wrong?**
You're in control. Review changes before accepting them, and you can always ask to fix or revert things.

**How do I exit?**
Type `/exit` or press Ctrl+C. Your files are already saved.

---

## Tips

1. **Be conversational** -- talk to Claude Code like a colleague, not a search engine
2. **Be specific** -- more detail gets better results
3. **Ask questions** -- if you don't understand something, ask for an explanation
4. **Review changes** -- always look at what changed so you actually learn
5. **Experiment** -- try different requests. You can't break anything.
6. **Read the output** -- Claude Code explains what it's doing, and that's how you learn
7. **Create a CLAUDE.md early** -- even a simple one with your preferred language and conventions helps Claude Code give you better results from the start

---

## Troubleshooting

**"Command not found: claude"** -- Make sure the install script finished successfully and restart your terminal.

**"API key not found"** -- Either log in with your Claude subscription (`claude` opens a browser), or set `ANTHROPIC_API_KEY` as an environment variable.

**"Permission denied"** -- On macOS/Linux, try `curl -fsSL https://claude.ai/install.sh | sudo bash`. On Windows, make sure you have [Git for Windows](https://git-scm.com/downloads/win) installed.

**Claude Code seems slow** -- Normal. AI processing takes time, especially for complex requests.

**No output** -- Make sure you pressed Enter. Check your internet connection.

---

> **Curious what full projects built with Claude Code look like?** The [Real Projects Pack](https://payhip.com/b/dFXWO) walks you through 11 complete builds -- from todo apps to full-stack SaaS.

*Next up: Module 2 -- different ways to start projects with Claude Code.*
