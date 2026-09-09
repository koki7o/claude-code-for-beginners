<div align="center">

![Module 2](https://img.shields.io/badge/Module_2-32CD32?style=for-the-badge&labelColor=1a1a2e)
![Time](https://img.shields.io/badge/⏱_30_min-555555?style=for-the-badge&labelColor=1a1a2e)
![Difficulty](https://img.shields.io/badge/Beginner-32CD32?style=for-the-badge&labelColor=1a1a2e)

# Starting Your First Project

**Learn different ways to begin building with Claude Code**

[← Previous](module-01-welcome-to-claude-code.md) · [Home](README.md) · [Next →](module-03-understanding-tools.md)

</div>

---

## What You'll Learn

There's more than one way to start a project. Four, really: from an empty folder, from code that already exists, from a template, or from a cloned repo. This module walks all four and, more usefully, when to reach for each.

---

## Lesson 1: Starting from Scratch (The Most Common Way)

### What Does "From Scratch" Mean?

An empty directory and nothing else. No boilerplate, no starter code - just you, Claude Code, and a blank canvas you fill in together.

### When to Start from Scratch

- You're building something brand new
- You want to understand how every piece fits together
- You need something specific that no template covers
- You're prototyping an idea fast

### How to Start from Scratch

#### Step 1: Create Your Project Directory

Always give a project its own folder:

```bash
mkdir my-awesome-project
cd my-awesome-project
```

`mkdir` creates the folder; `cd` moves into it. (Both from Module 1.)

This keeps your code organized and walled off from everything else on your machine.

#### Step 2: Start Claude Code

```bash
claude
```

You're ready to build.

#### Step 3: Describe What You Want to Build

As specific or as loose as you like:

**Simple Approach:**
```
Create a simple todo list web app
```

**More Detailed Approach:**
```
Create a todo list web application with:
- React frontend
- Express backend with REST API
- SQLite database
- Features: add, complete, delete tasks
- Clean, modern UI
```

**Very Detailed Approach:**
```
Build a full-stack todo application with these specifications:

Frontend:
- React with TypeScript
- Tailwind CSS for styling
- React hooks for state management
- Responsive design

Backend:
- Node.js with Express
- RESTful API endpoints
- SQLite database with better-sqlite3
- Input validation

Features:
- Create tasks with title and description
- Mark tasks as complete
- Delete tasks
- Filter by status (all/active/completed)
- Task due dates

Please start with the project structure and package.json
```

Honestly, starting simple is almost always the right call. You can layer features on later, and building up beats debugging one giant opening prompt every time.

#### Step 4: Let Claude Code Build

Once you send your request, Claude Code will:
1. Ask clarifying questions if it needs to
2. Create the project structure
3. Set up configuration files (these describe your project and its dependencies)
4. Write initial code
5. Explain what it created

Watch which tools it reaches for:
- **Write** -- creating new files
- **Bash** -- running commands like `npm init`
- **Read** -- checking what it created

Don't sweat the tools yet - Module 3 covers them properly.

#### Step 5: Review the Structure

When it finishes, ask:

```
Can you show me the project structure?
```

You'll see something like:
```
my-awesome-project/
├── package.json          ← project info and dependencies
├── src/                  ← your source code
│   ├── index.js          ← entry point (where the app starts)
│   ├── app.js
│   └── routes/
│       └── todos.js
├── public/
│   └── index.html
└── README.md
```

Yours might look different depending on the tech Claude Code picked. That's fine.

#### Step 6: Test It

```
Can you run the application?
```

Claude Code will:
- Install the libraries your project needs (its "dependencies")
- Start the application
- Show you how to open it in your browser

#### Step 7: Set Up Project Memory

Once it's working, hand Claude Code some context about how you like to work:

```
Create a CLAUDE.md file for this project with:
- The language and framework we're using
- Our coding conventions
- How to run and test the project
```

This file lives in your project root and keeps Claude Code consistent across sessions. Optional this early, but it earns its keep as the project grows. We go deep on it in Module 12.

---

## Lesson 2: Working with Existing Codebases

### What is an Existing Codebase?

Code someone else wrote - or code past-you wrote and half-forgot - that you now need to change, fix, or just understand.

### When to Work with Existing Code

More often than you'd expect:

- Contributing to open source
- Joining a team project already in motion
- Maintaining legacy code nobody wants to touch
- Learning from real-world examples
- Debugging someone else's code

### How to Work with Existing Code

#### Step 1: Navigate to the Project

```bash
cd /path/to/existing-project
```

#### Step 2: Start Claude Code

```bash
claude
```

#### Step 3: Understand the Codebase

Before you change anything, have it look around:

```
Can you help me understand what this project does? Please explore the codebase and give me an overview.
```

Claude Code reads the key files, works out the structure, and comes back with a summary: what the project does, what it's built with, its main pieces, and how to run it. (More on the specific tools it uses in Module 3.)

#### Step 4: Find Specific Code

Need to find something? Ask:

```
Where is the user authentication logic?
```

Or:

```
Find all files that handle database operations
```

Claude Code searches your files and points you at the relevant code.

#### Step 5: Make Changes

Once you get the lay of the land, make your change:

```
Add a new endpoint to the API that returns user statistics
```

Claude Code finds the right file, drops the code in the right place, follows the patterns already there, and tells you what it did.

#### Step 6: Test Your Changes

```
Run the tests to make sure I didn't break anything
```

---

## Lesson 3: Using Templates and Scaffolding

### What are Templates?

Pre-built project structures that hand you a head start - blueprints that take care of the boring setup so you can get to the interesting part.

### Common Templates

- **Vite** -- React, Vue, and other frontend projects (recommended for new React apps)
- **express-generator** -- Express servers
- **nest-cli** -- NestJS applications
- **create-next-app** -- Next.js projects

Don't recognize these? No problem - they're popular tools in the JavaScript world, and Claude Code can help you pick the right one.

### How to Use Templates

#### Method 1: Ask Claude Code to Use a Template

```
Create a new React app using create-react-app
```

Claude Code will:
```bash
npx create-react-app my-app
```

Then help you customize it.

#### Method 2: Ask for a Custom Template

```
Set up a basic Express server with TypeScript, organized with:
- Controllers folder
- Routes folder
- Middleware folder
- Models folder
- Config folder
```

Claude Code builds the structure from scratch.

#### Method 3: Use Framework CLIs

```
Initialize a new Next.js project with TypeScript and Tailwind CSS
```

Claude Code will run:
```bash
npx create-next-app@latest --typescript --tailwind
```

### Customizing Templates

Once it's scaffolded, keep going:

```
Add authentication using JWT
Add a database connection using Prisma
Set up ESLint and Prettier
Add a Docker configuration
```

---

## Lesson 4: Cloning and Understanding Repositories

### What is Cloning?

Downloading a copy of a project from the internet. Projects usually live on hosting platforms like GitHub or GitLab. Don't worry about the Git details yet - Module 7 handles that. For now, "cloning" just means "pulling a project onto your computer."

### When to Clone

- Contributing to open source
- Learning from examples
- Using starter templates
- Following tutorials

### How to Clone with Claude Code

#### Step 1: Find a Repository

Say you want to clone: `https://github.com/user/awesome-project`

#### Step 2: Clone It

Do it by hand:

```bash
git clone https://github.com/user/awesome-project
cd awesome-project
claude
```

Or let Claude Code handle it:

```bash
claude
```

Then:
```
Clone the repository https://github.com/user/awesome-project and help me set it up
```

Claude Code runs `git clone`, reads the README, installs dependencies, and explains how to run it.

#### Step 3: Understand the Project

```
Please explore this codebase and explain:
1. What does this project do?
2. What technologies does it use?
3. How is it structured?
4. How do I run it locally?
```

#### Step 4: Set Up the Environment

```
Help me set up this project. Install dependencies and configure any necessary environment variables.
```

Claude Code checks for package.json, requirements.txt, and the like, installs dependencies, looks for .env.example files, and walks you through the config.

#### Step 5: Make Your First Contribution

```
I want to add a feature that [describe feature]. Where should I start?
```

Claude Code reads the codebase, points you at the right files, helps you build the feature, and walks you through testing it.

---

## Lesson 5: Choosing the Best Approach

### Decision Matrix

How to pick:

| Scenario | Best Approach | Why |
|----------|---------------|-----|
| Brand new project | **From scratch** | Full control, learn everything |
| Following a tutorial | **Clone & modify** | Start with working code |
| React/Next.js app | **Use template CLI** | Industry standard setup |
| Contributing to OSS | **Clone repository** | Work with existing code |
| Learning a framework | **From scratch** | Understand fundamentals |
| Quick prototype | **Template + customize** | Fast start, then iterate |
| Joining team project | **Clone repository** | Get existing codebase |

### Tips for Choosing

Start from scratch when you want to learn deeply, have unusual requirements, are building something custom, or following a specific architecture.

Use templates when you want to move fast, are on popular frameworks, need standard config, or are prototyping.

Clone repositories when you're contributing, learning from examples, following tutorials, or starting from someone's template.

There's no single right answer here, and with a bit of experience you'll just feel which one fits. When in doubt: templates are the safe default for common project types, and from-scratch wins when the whole point is to learn.

---

## Hands-On Practice: Try Each Method

### Practice 1: From Scratch -- Build a Simple CLI Tool

**Task:** Create a command-line weather app

**Your prompt:**
```
Create a Node.js CLI tool that:
- Takes a city name as argument
- Fetches weather data from a free API
- Displays temperature and conditions
- Use the wttr.in API (no key needed)

Example usage: node weather.js "New York"
```

**Steps:**
1. Create directory and move into it: `mkdir weather-cli && cd weather-cli` (the `&&` runs both commands in sequence)
2. Start Claude Code: `claude`
3. Send the prompt above
4. Test the application
5. Ask for improvements (add colors, better formatting)

---

### Practice 2: Template -- Create a React App

**Task:** Set up a React app with routing

**Your prompt:**
```
Create a new React application with:
- React Router for navigation
- Three pages: Home, About, Contact
- A navigation bar
- Tailwind CSS for styling

Use Vite as the build tool
```

**Steps:**
1. Create directory and move into it: `mkdir my-react-app && cd my-react-app` (the `&&` runs both commands in sequence)
2. Start Claude Code: `claude`
3. Send the prompt above
4. Explore the generated structure
5. Run the dev server
6. Make a small change (add a new page)

---

### Practice 3: Clone -- Work with an Open Source Project

**Task:** Clone and understand a popular repository

**Your prompt:**
```
Clone the repository https://github.com/simple-icons/simple-icons
Then help me:
1. Understand what this project does
2. Set up my development environment
3. Find where the icon data is stored
4. Explain how I could contribute a new icon
```

**Steps:**
1. Start in your projects folder
2. Start Claude Code: `claude`
3. Send the prompt above
4. Follow Claude Code's guidance
5. Ask questions about the codebase

---

### Practice 4: Existing Code -- Modify a Past Project

**Task:** Improve a project you built earlier

**Your prompt:**
```
Help me add error handling to this project.
Please:
1. Find all places that need error handling
2. Add try-catch blocks where appropriate
3. Create meaningful error messages
4. Add logging for errors
```

**Steps:**
1. Navigate to an old project
2. Start Claude Code: `claude`
3. Send the prompt above
4. Review the changes
5. Test error scenarios

---

## Module 2 Challenges

### Challenge 1: From Scratch -- Build a Note-Taking API (Beginner)

**Your Task:** Create a REST API for notes

**Requirements:**
- Express.js server
- In-memory storage (no database yet)
- Endpoints: GET, POST, PUT, DELETE
- Each note has: id, title, content, createdAt
- Include input validation

**Hints:**
- Start with basic structure
- Add one endpoint at a time
- Test as you build

**Check your solution:** See [Challenge Solutions](supplement-challenge-solutions.md#module-2-challenge-1)

---

### Challenge 2: Template -- Full-Stack App (Intermediate)

**Your Task:** Create a full-stack application using templates

**Requirements:**
- Next.js for frontend
- API routes for backend
- A simple contact form
- Form validation
- Success/error messages
- Responsive design

**Hints:**
- Use create-next-app
- Start with the form UI
- Then add API endpoint
- Finally add validation

**Check your solution:** See [Challenge Solutions](supplement-challenge-solutions.md#module-2-challenge-2)

---

### Challenge 3: Clone & Contribute -- Real World (Advanced)

**Your Task:**
1. Find an open source project with "good first issue" tags
2. Clone it
3. Set it up locally
4. Understand the codebase
5. Implement the issue
6. Test your changes

**Requirements:**
- Must be a real GitHub repository
- Must successfully run locally
- Your change must work
- Follow the project's contribution guidelines

**Hints:**
- Check CONTRIBUTING.md first
- Read existing code for patterns
- Test thoroughly
- Ask Claude Code for guidance

**Check your solution:** See [Challenge Solutions](supplement-challenge-solutions.md#module-2-challenge-3)

---

## Module 2 Checklist

Before moving to Module 3, make sure you can:

- [ ] Create a project from scratch with Claude Code
- [ ] Work with existing codebases
- [ ] Use templates and CLIs effectively
- [ ] Clone and set up repositories
- [ ] Choose the right approach for different scenarios
- [ ] Navigate and understand unfamiliar code
- [ ] Make modifications to existing projects

---

## Common Questions (FAQ)

### Q: Should I always start from scratch?
No. Use templates for common project types - they'll save you real time. Go from scratch when you're learning, or building something that doesn't fit a standard mold.

### Q: How do I know which template to use?
Ask Claude Code. Genuinely - say "What's the best way to start a [type of project]?" and it'll point you the right way.

### Q: What if I don't understand the cloned code?
Ask Claude Code to explain it. This is one of the things it's flat-out best at - walking you through an unfamiliar codebase piece by piece.

### Q: Can Claude Code help with non-JavaScript projects?
Absolutely. Python, Go, Rust, Java, plenty more. This module leaned on JavaScript because it's common for web work, but use any language you like - or have Claude Code pick one.

### Q: How detailed should my initial prompt be?
Start basic, add detail as you go. You can always refine, and iterating beats agonizing over the perfect first prompt.

---

## What's Next?

You've now got the full toolkit for starting projects - blank directories, cloned repos, and everything in between.

Next up: Module 3, where we get into Claude Code's tools - what each one does and when to reach for it.

---

## Pro Tips for Beginners

1. **Start simple, iterate** -- Build the basic version first, then add features. This matters more than it sounds.

2. Use templates for common patterns. No point reinventing the wheel when a good scaffold already exists.

3. **Read before modifying** -- Understand existing code before you change it. Skipping this is how bugs get born.

4. Ask for explanations as you go. Claude Code will teach you while it builds - take the free lesson.

5. Always read the README in a cloned repo. It's there for a reason.

6. Use Git from the start - Module 7 covers it, but the habit pays off early.

7. Test frequently. Run your code often and catch problems before they stack up.

8. **Set up a CLAUDE.md file** -- even a few lines about your project's conventions makes Claude Code more consistent across sessions

---

> **Looking for project ideas?** The [Real Projects Pack](https://payhip.com/b/dFXWO) has 14 ready-to-build projects -- from CLI tools to a full-stack SaaS boilerplate -- each with step-by-step Claude Code workflows.

---

<div align="center">

[← Previous Module](module-01-welcome-to-claude-code.md) · [🏠 Home](README.md) · [Next Module →](module-03-understanding-tools.md)

</div>
