# Module 2: Starting Your First Project

**Goal:** Learn all the different ways to begin building with Claude Code

**Estimated Time:** 30-40 minutes

---

## What You'll Learn

This module covers the main ways to kick off a project with Claude Code -- starting from scratch, working with code that already exists, using templates, and cloning repos. You'll also get a feel for when each approach makes sense.

---

## Lesson 1: Starting from Scratch (The Most Common Way)

### What Does "From Scratch" Mean?

Starting from scratch means you begin with an empty directory and let Claude Code help you build everything from the ground up. No boilerplate, no starter code -- just you and a blank canvas.

### When to Start from Scratch

- You're building something brand new
- You want to understand how every piece fits together
- You need something very specific that templates won't cover
- You're prototyping an idea quickly

### How to Start from Scratch

#### Step 1: Create Your Project Directory

Always start by creating a dedicated folder for your project:

```bash
mkdir my-awesome-project
cd my-awesome-project
```

`mkdir` creates a new folder; `cd` moves into it. (These were covered in Module 1.)

This keeps your code organized and isolated from everything else on your machine.

#### Step 2: Start Claude Code

```bash
claude
```

You're now ready to build.

#### Step 3: Describe What You Want to Build

Be as specific or as general as you like:

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

Honestly, starting simple is almost always the right call. You can layer on features later, and it's way easier to build incrementally than to debug a huge initial prompt.

#### Step 4: Let Claude Code Build

After you send your request, Claude Code will:
1. Ask clarifying questions if needed
2. Create the project structure
3. Set up configuration files (these describe your project and its dependencies)
4. Write initial code
5. Explain what it created

Watch as it uses different tools:
- **Write** -- creating new files
- **Bash** -- running commands like `npm init`
- **Read** -- checking what it created

Don't worry about understanding these tools in detail yet -- Module 3 covers them thoroughly.

#### Step 5: Review the Structure

Once Claude Code finishes, ask:

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

Your structure may look different depending on the technologies Claude Code chooses. That's fine.

#### Step 6: Test It

```
Can you run the application?
```

Claude Code will:
- Install the libraries your project needs (its "dependencies")
- Start the application
- Show you how to open it in your browser

#### Step 7: Set Up Project Memory

Once your project is working, give Claude Code some context about your preferences:

```
Create a CLAUDE.md file for this project with:
- The language and framework we're using
- Our coding conventions
- How to run and test the project
```

This file lives in your project root and helps Claude Code maintain consistency across sessions. It's optional at this stage, but gets more valuable as your project grows. We'll go deep on this in Module 12.

---

## Lesson 2: Working with Existing Codebases

### What is an Existing Codebase?

An existing codebase is a project that someone else built -- or that you built a while back -- that you now want to modify, fix, or just understand.

### When to Work with Existing Code

This comes up more often than you'd think:

- Contributing to open source
- Joining a team project already in progress
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

Start by asking Claude Code to explore:

```
Can you help me understand what this project does? Please explore the codebase and give me an overview.
```

Claude Code will read key files, analyze the project structure, and come back with a summary covering what the project does, the technologies it uses, its main components, and how to run it. (You'll learn more about the specific tools it uses in Module 3.)

#### Step 4: Find Specific Code

Need to find something? Just ask:

```
Where is the user authentication logic?
```

Or:

```
Find all files that handle database operations
```

Claude Code will search through your files to find the relevant code.

#### Step 5: Make Changes

Once you understand the code, make modifications:

```
Add a new endpoint to the API that returns user statistics
```

Claude Code will find the right file, add the code in the correct place, follow existing patterns, and explain what it did.

#### Step 6: Test Your Changes

```
Run the tests to make sure I didn't break anything
```

---

## Lesson 3: Using Templates and Scaffolding

### What are Templates?

Templates are pre-built project structures that give you a head start -- basically blueprints that handle the boring setup so you can focus on the interesting parts.

### Common Templates

- **Vite** -- React, Vue, and other frontend projects (recommended for new React apps)
- **express-generator** -- Express servers
- **nest-cli** -- NestJS applications
- **create-next-app** -- Next.js projects

Don't worry if you don't recognize these names. They're popular tools in the JavaScript ecosystem. Claude Code can help you pick the right one for your project.

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

Claude Code will create the structure from scratch.

#### Method 3: Use Framework CLIs

```
Initialize a new Next.js project with TypeScript and Tailwind CSS
```

Claude Code will run:
```bash
npx create-next-app@latest --typescript --tailwind
```

### Customizing Templates

After scaffolding, you can keep going:

```
Add authentication using JWT
Add a database connection using Prisma
Set up ESLint and Prettier
Add a Docker configuration
```

---

## Lesson 4: Cloning and Understanding Repositories

### What is Cloning?

Cloning means downloading a copy of a project from the internet. Projects are typically stored on hosting platforms like GitHub or GitLab. Don't worry about the details of Git yet -- Module 7 covers that. For now, just think of cloning as "downloading a project to your computer."

### When to Clone

- Contributing to open source
- Learning from examples
- Using starter templates
- Following tutorials

### How to Clone with Claude Code

#### Step 1: Find a Repository

Say you want to clone: `https://github.com/user/awesome-project`

#### Step 2: Clone It

You can either clone manually:

```bash
git clone https://github.com/user/awesome-project
cd awesome-project
claude
```

Or ask Claude Code to do it:

```bash
claude
```

Then:
```
Clone the repository https://github.com/user/awesome-project and help me set it up
```

Claude Code will run `git clone`, read the README, install dependencies, and explain how to run the project.

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

Claude Code will check for package.json, requirements.txt, etc., install dependencies, look for .env.example files, and guide you through configuration.

#### Step 5: Make Your First Contribution

```
I want to add a feature that [describe feature]. Where should I start?
```

Claude Code will analyze the codebase, suggest the right files to modify, help you implement the feature, and guide you through testing.

---

## Lesson 5: Choosing the Best Approach

### Decision Matrix

Here's how to choose:

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

Start from scratch when you want to learn deeply, have unique requirements, are building something custom, or following a specific architecture.

Use templates when you want to move fast, are using popular frameworks, need standard configuration, or are prototyping.

Clone repositories when you're contributing to projects, learning from examples, following tutorials, or using starter templates.

Trust me on this -- there's no single "right" answer. As you get more experience, you'll develop instincts for which approach fits. When in doubt, templates are a safe default for most common project types, and starting from scratch is best when you really want to learn.

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
No. Use templates for common project types -- they'll save you a lot of time. Start from scratch when you're learning or building something that doesn't fit a standard mold.

### Q: How do I know which template to use?
Ask Claude Code. Seriously -- just say "What's the best way to start a [type of project]?" and it'll point you in the right direction.

### Q: What if I don't understand the cloned code?
Ask Claude Code to explain it. This is honestly one of the things it's best at -- walking you through unfamiliar codebases piece by piece.

### Q: Can Claude Code help with non-JavaScript projects?
Absolutely. It works with Python, Go, Rust, Java, and many other languages. This module used JavaScript examples because they're common for web projects, but you can use any language -- or ask Claude Code to pick one for you.

### Q: How detailed should my initial prompt be?
Start with the basics, then add details as you go. You can always refine -- and iterating is usually faster than trying to write the perfect prompt on your first try.

---

## What's Next?

You've now got the full toolkit for starting projects -- from blank directories to cloned repos and everything in between.

Next up: Module 3, where we dig into Claude Code's tools -- what each one does and when to reach for it.

---

## Pro Tips for Beginners

1. **Start simple, iterate** -- Build the basic version first, then add features. This matters more than you think.

2. Use templates for common patterns. No point reinventing the wheel when a good scaffold exists.

3. **Read before modifying** -- Understand existing code before changing it. Skipping this step is how bugs happen.

4. Ask for explanations as you go. Claude Code can teach you while it builds -- take advantage of that.

5. Always read README files in cloned repos. They're there for a reason.

6. Use Git from the start -- we'll cover this in Module 7, but getting into the habit early pays off.

7. Test frequently. Run your code often to catch issues before they pile up.

8. **Set up a CLAUDE.md file** -- even a few lines about your project's conventions will make Claude Code more consistent across sessions

---

> **Looking for project ideas?** The [Real Projects Pack](https://payhip.com/b/dFXWO) has 11 ready-to-build projects -- from CLI tools to a full-stack SaaS boilerplate -- each with step-by-step Claude Code workflows.

*Module 2 Complete*
