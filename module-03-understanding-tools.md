# Module 3: Understanding Claude Code's Tools

**Goal:** Master the different capabilities and tools Claude Code has access to

**Estimated Time:** 40-50 minutes

---

## What You'll Learn

This module covers the tools Claude Code uses behind the scenes -- what they are, when it reaches for each one, and how they chain together to get real work done. You'll also learn how to steer Claude Code toward specific tools when you want more control.

---

## Lesson 1: What Are Tools?

### Understanding Tools

Tools are the specific capabilities Claude Code has for interacting with your system and codebase. Each tool does one thing well:

- **Reading** -- looking at a document
- **Writing** -- creating a new document
- **Editing** -- making changes to an existing document
- **Running commands** -- performing actions in the terminal

That's really all there is to it. Claude Code picks the right tool (or combination of tools) based on what you ask it to do.

### How Claude Code Uses Tools

When you make a request, Claude Code goes through a straightforward process:

1. Understands your request
2. Decides which tools to use
3. Uses them -- you'll see this happening in real time
4. Shows you the results
5. Explains what it did

Here's what that looks like in practice:

```
You: "Create a Python file that prints Hello World"

Claude Code thinks:
- I need to create a new file → Use Write tool
- The file doesn't exist yet → Write is the right choice
- I'll write Python code for printing

Claude Code does:
- Uses Write tool to create hello.py
- Confirms it's done
```

---

## Lesson 2: File Operation Tools

### The Read Tool

**Purpose:** Read and view files

Claude Code uses this when:
- You ask to see a file
- It needs to understand existing code
- Before making modifications
- To verify changes were made correctly

**Example requests:**
```
Show me the contents of app.js
What's in the package.json file?
Can you read the README and summarize it?
```

**What you'll see:**
```
Reading app.js...

[File contents displayed with line numbers]
```

Read is completely non-destructive -- it never changes anything. It's always safe.

---

### The Write Tool

**Purpose:** Create new files

Claude Code uses this when:
- Creating files that don't exist
- Generating configuration files
- Writing documentation
- Creating test files

**Example requests:**
```
Create a new file called config.js with database settings
Write a README.md for this project
Generate a .gitignore file for Node.js
```

**What you'll see:**
```
Writing config.js...

Created config.js with:
- Database configuration
- Port settings
- Environment variables
```

One thing to know: Write creates NEW files. If a file already exists, Claude Code will use Edit instead.

---

### The Edit Tool

**Purpose:** Make precise changes to existing files

Claude Code uses this when:
- Modifying existing code
- Fixing bugs
- Adding features to existing files
- Refactoring code

**Example requests:**
```
Add error handling to the login function
Change the port from 3000 to 8080
Add a new route to the API
```

**What you'll see:**
```
Editing server.js...

Old:
const PORT = 3000;

New:
const PORT = 8080;
```

This matters more than you think -- the Edit tool shows you exactly what changed, only touches what's needed, and leaves the rest of your code alone. You get a clear before/after view every time.

Get in the habit of reviewing Edit tool outputs. Trust me on this.

---

### The Glob Tool

**Purpose:** Find files by pattern

Claude Code uses this when:
- Finding all files of a certain type
- Locating files in your project
- Discovering project structure
- Finding files by name pattern

**Example searches:**
```
*.js → All JavaScript files
src/**/*.py → All Python files in src and subdirectories
**/*test* → All files with "test" in the name
components/**/*.tsx → All TypeScript React components
```

**Example requests:**
```
Find all Python files in this project
Show me all the test files
List all configuration files
```

**What you'll see:**
```
Finding *.py files...

Found:
- src/main.py
- src/utils.py
- tests/test_main.py
- config/database.py
```

---

### The Grep Tool

**Purpose:** Search for text/code inside files

Claude Code uses this when:
- Finding where something is used
- Searching for function definitions
- Looking for specific patterns
- Finding TODO comments

**Example searches:**
```
function login → Find the login function
TODO → Find all TODO comments
import.*express → Find Express imports
class.*User → Find User class definitions
```

**Example requests:**
```
Find all places where the User model is imported
Search for TODO comments
Find the login function
Show me where the database is connected
```

**What you'll see:**
```
Searching for "function login"...

Found in auth.js:15
function login(username, password) {

Found in auth.test.js:23
describe('function login', () => {
```

Grep also supports regular expressions, which makes it surprisingly powerful for tracking down patterns across a large codebase.

---

## Lesson 3: Command Execution Tools

### The Bash Tool

**Purpose:** Run terminal/shell commands

Claude Code uses this for:
- Installing dependencies (`npm install`)
- Running your application
- Git operations
- Building projects
- Running tests
- Really, any terminal command

**Example requests:**
```
Install the dependencies
Run the server
Execute the tests
Check the git status
Build the project
```

**What you'll see:**
```
Running: npm install

[Installation progress...]

✓ Dependencies installed successfully
```

**Commands Claude Code commonly runs:**
```bash
npm install          # Install Node.js dependencies
pip install -r requirements.txt  # Install Python dependencies
npm run dev          # Start development server
npm test             # Run tests
git status           # Check Git status
docker build .       # Build Docker image
```

Claude Code will explain commands before running them, so you won't be caught off guard.

---

### Background Processes

**Purpose:** Run long-running commands without blocking

This is useful for:
- Development servers
- Watch mode for tests
- Database servers
- Any long-running process

**Example:**
```
You: "Start the dev server"

Claude Code:
- Runs npm run dev in background
- You can continue working
- Server keeps running
- You can stop it later
```

**Managing background processes:**
```
Start the server in the background
Stop the background server
Show me running processes
```

---

## Lesson 4: AI-Powered Tools

### The Task Tool (Specialized Agents)

**Purpose:** Handle complex, multi-step tasks autonomously

Claude Code uses this for:
- Exploring large codebases
- Planning implementations
- Multi-step workflows
- Research tasks
- Complex analysis

There are a few different agent types worth knowing about:

#### 1. Explore Agent
```
Purpose: Understand and navigate codebases
Use for:
- "Explain how this codebase works"
- "Find all API endpoints"
- "How is authentication implemented?"
```

#### 2. Plan Agent
```
Purpose: Design implementation strategies
Use for:
- "Plan how to add user authentication"
- "Design the database schema for this feature"
- "Create an implementation plan for [feature]"
```

#### 3. General-Purpose Agent
```
Purpose: Complex multi-step tasks
Use for:
- "Research and implement best practices for error handling"
- "Find and fix all security vulnerabilities"
- "Optimize this codebase for performance"
```

**Example:**
```
You: "Help me understand how the authentication works in this codebase"

Claude Code:
- Launches Explore agent
- Agent searches for auth-related files
- Reads relevant code
- Analyzes the flow
- Returns comprehensive explanation
```

The key advantage of agents is that they work autonomously -- they handle multiple steps, make decisions along the way, and come back with thorough results. You don't need to hold their hand through each step.

---

### The WebSearch Tool

**Purpose:** Search the internet for information

Claude Code uses this when:
- Finding documentation
- Looking up error messages
- Researching best practices
- Finding package information
- Checking compatibility

**Example requests:**
```
Search for the latest React best practices
Look up this error message
Find documentation for the Stripe API
Check if this package is still maintained
```

**What you'll see:**
```
Searching: "React useEffect best practices 2025"

Found:
- React documentation on useEffect
- Common pitfalls and solutions
- Performance optimization tips
```

---

### The WebFetch Tool

**Purpose:** Fetch content from specific URLs

Claude Code uses this when:
- Reading documentation pages
- Fetching API docs
- Accessing GitHub pages
- Reading specific articles

**Example requests:**
```
Fetch the documentation from [URL]
Read this GitHub README: [URL]
Get the API docs from [URL]
```

---

### The LSP Tool (Language Server Protocol)

**Purpose:** Deep code intelligence

Claude Code uses this when:
- Finding function definitions
- Checking where code is used
- Understanding type information
- Navigating code relationships

**Capabilities:**
```
goToDefinition → Find where something is defined
findReferences → Find all uses of a symbol
hover → Get type/documentation info
documentSymbol → List all symbols in a file
```

**Example:**
```
You: "Find all places where the User class is used"

Claude Code:
- Uses LSP findReferences
- Shows all imports and usages
- Provides file and line numbers
```

---

## Lesson 5: How Tools Work Together

### Real-World Example: Adding a New Feature

**Request:** "Add a new /users endpoint to the API that returns all users from the database"

Here's what Claude Code actually does behind the scenes:

**Step 1: Understand the codebase**
```
Tool: Glob
Action: Find all route files
Result: Located routes/index.js
```

**Step 2: Read existing code**
```
Tool: Read
Action: Read routes/index.js
Result: Understands current route structure
```

**Step 3: Find similar patterns**
```
Tool: Grep
Action: Search for other endpoint examples
Result: Finds pattern to follow
```

**Step 4: Make the changes**
```
Tool: Edit
Action: Add new route to routes/index.js
Result: Route added following existing patterns
```

**Step 5: Test it**
```
Tool: Bash
Action: Restart server and test endpoint
Result: Confirms it works
```

### Another Example: Debugging an Error

**Request:** "The app is crashing with 'Cannot find module'. Help me fix it"

**Step 1: Check the error**
```
Tool: Bash
Action: Run the app to see full error
Result: Gets complete stack trace
```

**Step 2: Find the problem**
```
Tool: Grep
Action: Search for the missing import
Result: Finds which file has the issue
```

**Step 3: Read the file**
```
Tool: Read
Action: Read the problematic file
Result: Sees the incorrect import
```

**Step 4: Fix it**
```
Tool: Edit
Action: Correct the import statement
Result: Fixed!
```

**Step 5: Verify**
```
Tool: Bash
Action: Run app again
Result: No more error!
```

---

## Lesson 6: Requesting Specific Tool Usage

### You Can Guide Claude Code

Claude Code chooses tools automatically, but you can absolutely steer it.

**Be general** -- let Claude Code decide:
```
Fix the bug in app.js
```

**Be specific about approach:**
```
Search for all places where the database is connected, then show me the config file
```

**Request specific tools:**
```
Use grep to find all TODO comments
Read all test files
Run the linter
```

### When to Be Specific

Here's the short version: be specific when you know what you want, and let Claude Code decide when you're not sure.

Be specific when:
- You know exactly what you need
- Claude Code's approach isn't working
- You want to learn a specific tool
- You're following a specific workflow

Let Claude Code decide when:
- You're unsure of the best approach
- The task is complex
- You want the optimal solution

---

## Hands-On Practice: Tool Mastery

### Practice 1: File Operations

**Task:** Master Read, Write, and Edit

**Steps:**
1. **Write:** "Create a JavaScript file called calculator.js with add and subtract functions"
2. **Read:** "Show me the calculator.js file"
3. **Edit:** "Add multiply and divide functions to calculator.js"
4. **Read:** "Show me the updated file"

This gives you a feel for how Write creates files, Read displays them, and Edit makes targeted changes.

---

### Practice 2: Search and Find

**Task:** Master Glob and Grep

Navigate to any project directory, then try:

1. **Glob:** "Find all JavaScript files in this project"
2. **Glob:** "Find all test files"
3. **Grep:** "Search for all function definitions"
4. **Grep:** "Find all TODO comments"
5. **Grep:** "Search for 'import' statements"

You'll quickly see the difference -- Glob finds files by name, Grep finds content inside files.

---

### Practice 3: Command Execution

**Task:** Master Bash tool

In a Node.js project:

1. "Initialize a new npm project"
2. "Install express as a dependency"
3. "Create a package.json script for starting the server"
4. "Run npm install"
5. "Show me the package.json"

---

### Practice 4: Complex Task with Agents

**Task:** Use the Explore agent

In a larger codebase:

```
Please explore this codebase and answer:
1. What is the overall architecture?
2. What are the main components?
3. How is the data flow organized?
4. Where should I start if I want to add a new feature?
```

This is where agents really shine -- they'll autonomously dig through the codebase instead of needing you to point them at individual files.

---

## Module 3 Checklist

Before moving to Module 4, make sure you understand:

- [ ] What tools are and why Claude Code uses them
- [ ] The difference between Read, Write, and Edit
- [ ] When to use Glob vs Grep
- [ ] How the Bash tool executes commands
- [ ] What specialized agents do
- [ ] How tools work together
- [ ] How to request specific tool usage
- [ ] When to let Claude Code choose vs when to be specific

---

## Common Questions (FAQ)

### Q: Can I see which tools Claude Code is using?
**A:** Yes -- Claude Code shows you each tool use in real-time.

### Q: Can I prevent Claude Code from using certain tools?
**A:** Yes. Just be explicit: "Please explain the fix but don't modify any files yet."

### Q: Why does Claude Code use multiple tools for one task?
**A:** Complex tasks need multiple steps -- reading code, understanding it, then modifying it. One tool rarely covers the whole job.

### Q: What if a tool fails?
**A:** Claude Code will explain the error and try alternative approaches or ask for your guidance.

### Q: Can I suggest which tool to use?
**A:** Absolutely. "Use grep to find..." or "Read the file first, then..." both work.

---

## What's Next?

You've now got a solid mental model of Claude Code's toolbox -- what each tool does, when it gets used, and how to request specific ones. That foundation matters for everything that comes next.

Next up: Module 4 -- working with files and code. We'll cover reading codebases, writing clean code, and making effective edits.

---

## Pro Tips

1. **Watch the tools** -- Pay attention to which tools Claude Code reaches for. You'll start to internalize the workflow.

2. **Review edits carefully** -- Always check Edit tool outputs. This is how you catch mistakes early.

3. **Use Grep for code search** -- It's faster than reading through multiple files yourself.

4. **Let agents handle complexity** -- If you find yourself micromanaging a task across many files, that's a sign you should let an agent handle it.

5. **Think in tool sequences** -- Before asking, consider what combination of tools the task probably needs. This helps you write better prompts.

6. **Ask "why did you use that tool?"** -- Seriously, it's one of the fastest ways to learn how Claude Code thinks.

---

*Module 3 Complete*
