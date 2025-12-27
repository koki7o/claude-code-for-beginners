# Module 3: Understanding Claude Code's Tools

**Goal:** Master the different capabilities and tools Claude Code has access to

**Estimated Time:** 40-50 minutes

---

## What You'll Learn in This Module

By the end of this module, you will:
- Understand what tools are and how Claude Code uses them
- Know the purpose of each major tool
- Learn when to use specific tools
- Understand how tools work together
- Be able to request specific tool usage

---

## Lesson 1: What Are Tools?

### Understanding Tools

**Tools** are specific capabilities that Claude Code has to interact with your system and codebase. Think of them as different skills or abilities.

**Human Analogy:**
- **Reading** = Looking at a document
- **Writing** = Creating a new document
- **Editing** = Making changes to an existing document
- **Running commands** = Performing actions

Claude Code works the same way!

### How Claude Code Uses Tools

When you make a request, Claude Code:
1. **Understands your request**
2. **Decides which tools to use**
3. **Uses the tools** (you'll see this happening)
4. **Shows you the results**
5. **Explains what it did**

**Example:**
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

**When Claude Code uses it:**
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

**Pro Tip:** Read is non-destructive - it never changes anything!

---

### The Write Tool

**Purpose:** Create new files

**When Claude Code uses it:**
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

**Important:** Write creates NEW files. If a file exists, Claude Code will use Edit instead.

---

### The Edit Tool

**Purpose:** Make precise changes to existing files

**When Claude Code uses it:**
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

**Why it's powerful:**
- Shows you exactly what changed
- Only modifies what's needed
- Preserves the rest of your code
- You can see before/after

**Best Practice:** Always review Edit tool outputs to understand changes!

---

### The Glob Tool

**Purpose:** Find files by pattern

**When Claude Code uses it:**
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

**When Claude Code uses it:**
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

**Advanced:** Grep supports regular expressions for powerful searches!

---

## Lesson 3: Command Execution Tools

### The Bash Tool

**Purpose:** Run terminal/shell commands

**When Claude Code uses it:**
- Installing dependencies (`npm install`)
- Running your application
- Git operations
- Building projects
- Running tests
- Any terminal command

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

**Safety Note:** Claude Code will explain commands before running them!

---

### Background Processes

**Purpose:** Run long-running commands without blocking

**When to use:**
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

**When Claude Code uses it:**
- Exploring large codebases
- Planning implementations
- Multi-step workflows
- Research tasks
- Complex analysis

**Agent Types:**

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

**Why use agents:**
- They work autonomously
- Handle multiple steps
- Can make decisions
- Provide thorough results

---

### The WebSearch Tool

**Purpose:** Search the internet for information

**When Claude Code uses it:**
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

**When Claude Code uses it:**
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

**When Claude Code uses it:**
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

**Claude Code's workflow:**

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

While Claude Code chooses tools automatically, you can request specific approaches:

**Be general (Claude Code decides):**
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

**Be specific when:**
- You know exactly what you need
- Claude Code's approach isn't working
- You want to learn a specific tool
- You're following a specific workflow

**Let Claude Code decide when:**
- You're unsure of the best approach
- The task is complex
- You want the optimal solution
- You're learning

---

## Hands-On Practice: Tool Mastery

### Practice 1: File Operations

**Task:** Master Read, Write, and Edit

**Steps:**
1. **Write:** "Create a JavaScript file called calculator.js with add and subtract functions"
2. **Read:** "Show me the calculator.js file"
3. **Edit:** "Add multiply and divide functions to calculator.js"
4. **Read:** "Show me the updated file"

**What you'll learn:**
- How Write creates new files
- How Read displays contents
- How Edit makes precise changes
- How to verify changes

---

### Practice 2: Search and Find

**Task:** Master Glob and Grep

**Navigate to any project directory, then:**

1. **Glob:** "Find all JavaScript files in this project"
2. **Glob:** "Find all test files"
3. **Grep:** "Search for all function definitions"
4. **Grep:** "Find all TODO comments"
5. **Grep:** "Search for 'import' statements"

**What you'll learn:**
- Finding files by pattern
- Searching within files
- Using patterns effectively

---

### Practice 3: Command Execution

**Task:** Master Bash tool

**In a Node.js project:**

1. "Initialize a new npm project"
2. "Install express as a dependency"
3. "Create a package.json script for starting the server"
4. "Run npm install"
5. "Show me the package.json"

**What you'll learn:**
- Running commands
- Managing dependencies
- Understanding output

---

### Practice 4: Complex Task with Agents

**Task:** Use the Explore agent

**In a larger codebase:**

```
Please explore this codebase and answer:
1. What is the overall architecture?
2. What are the main components?
3. How is the data flow organized?
4. Where should I start if I want to add a new feature?
```

**What you'll learn:**
- When to use agents
- How agents work differently
- Understanding complex codebases

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
**A:** Yes! Claude Code shows you each tool use in real-time.

### Q: Can I prevent Claude Code from using certain tools?
**A:** Yes, you can be explicit: "Please explain the fix but don't modify any files yet"

### Q: Why does Claude Code use multiple tools for one task?
**A:** Complex tasks often need multiple steps - reading code, understanding it, then modifying it.

### Q: What if a tool fails?
**A:** Claude Code will explain the error and try alternative approaches or ask for guidance.

### Q: Can I suggest which tool to use?
**A:** Yes! "Use grep to find..." or "Read the file first, then..."

---

## What's Next?

Great job! You now understand:
- All major tools Claude Code has
- When each tool is used
- How tools work together
- How to guide tool usage

**Ready for Module 4?** In the next module, we'll dive deep into working with files and code - reading codebases, writing clean code, and making effective edits!

---

## Pro Tips

1. **Watch the tools** - Pay attention to which tools Claude Code uses. This teaches you the workflow.

2. **Review edits carefully** - Always check Edit tool outputs to understand changes.

3. **Use Grep for code search** - It's faster than reading multiple files.

4. **Let agents handle complexity** - Don't micromanage complex tasks.

5. **Combine tools mentally** - Think about what sequence of tools you'd need.

6. **Ask for explanations** - "Why did you use that tool?" helps you learn.

---

*Module 3 Complete!*
