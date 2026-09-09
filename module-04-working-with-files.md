<div align="center">

![Module 4](https://img.shields.io/badge/Module_4-32CD32?style=for-the-badge&labelColor=1a1a2e)
![Time](https://img.shields.io/badge/⏱_40_min-555555?style=for-the-badge&labelColor=1a1a2e)
![Difficulty](https://img.shields.io/badge/Beginner-32CD32?style=for-the-badge&labelColor=1a1a2e)

# Working with Files and Code

**Learn how to effectively read, write, and modify code**

[← Previous](module-03-understanding-tools.md) · [Home](README.md) · [Next →](module-05-prompt-engineering.md)

</div>

---

## What You'll Learn

This is the day-to-day stuff - the things you'll actually do most with Claude Code. Reading existing code, writing new code, editing without breaking things, refactoring, and finding your way around a codebase. These are the skills that separate flailing from flying.

---

## Lesson 1: Reading and Understanding Code

### Why Reading Code Matters

Here's the thing nobody tells beginners: roughly 80% of programming is reading code, not writing it. You read to understand how a feature works, to hunt a bug, to figure out where new code goes, to pick up the local patterns, to review someone else's work. Get fast at reading - with Claude Code doing the heavy lifting - and everything else speeds up.

### Reading Single Files

Have Claude Code read a file and explain it:

```text
Read server.js and explain what it does
```

It shows you the contents, explains the main purpose, flags the key functions, and points out the patterns that matter.

You can also zero in:
```text
Read the login function in auth.js and explain how it works
```

Or ask about structure:
```text
Read package.json and tell me:
- What dependencies are used
- What scripts are available
- What's the entry point
```

---

### Reading Multiple Related Files

When a feature is spread across files, just name them:

```text
I want to understand how user authentication works.
Please read:
- routes/auth.js
- middleware/auth.js
- models/user.js
And explain the complete authentication flow
```

Claude Code reads all three, traces the flow, explains how they connect, and shows you how the data moves between them.

---

### Understanding Project Structure

For the big picture:

```text
Help me understand this project's structure.
Show me:
- The directory layout
- What each folder contains
- The entry point
- How the code is organized
```

Or send the Explore agent in for a deeper dig:
```text
Use the Explore agent to analyze this codebase and give me:
1. Overall architecture
2. Main components
3. Technology stack
4. How data flows through the system
```

---

### Reading Patterns and Conventions

This one's underrated. Before you write a single line, learn how the codebase already does things:

```text
Show me how error handling is done in this project
Find examples of API endpoints and show me the pattern
How are database queries organized?
```

You'll pick up the project's conventions, follow the patterns already there, keep things consistent, and stop reinventing wheels the codebase already has.

---

## Lesson 2: Writing New Code

### Following Project Conventions

Learn the conventions before you add anything:

```text
Before I add a new feature, show me:
- How routes are structured
- How controllers are organized
- Error handling patterns
- Naming conventions used
```

Then write code that fits right in:
```text
Add a new route for /api/products following the same pattern as /api/users
```

---

### Writing Clean Code

Say what you want. The more specific the ask, the better what comes back:

```text
Create a function to validate email addresses with:
- Clear variable names
- Input validation
- Error messages
- JSDoc comments
- Unit testable structure
```

Claude Code will write something like:
```javascript
/**
 * Validates an email address format
 * @param {string} email - The email address to validate
 * @returns {boolean} True if valid, false otherwise
 * @throws {TypeError} If email is not a string
 */
function validateEmail(email) {
  if (typeof email !== 'string') {
    throw new TypeError('Email must be a string');
  }

  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
}
```

---

### Writing Modular Code

Ask for separation of concerns up front:

```text
Create a user service module that handles:
- Creating users
- Finding users
- Updating users
- Deleting users

Keep database logic separate from business logic
Export all functions
```

Result:
```text
services/
  └── userService.js  (business logic)
database/
  └── userRepository.js  (database operations)
```

---

### Adding Documentation

Ask for docs as you write, not later. Later never comes.

```text
Create a calculateTax function and include:
- JSDoc comments
- Parameter descriptions
- Return value description
- Example usage in comments
```

---

## Lesson 3: Editing Existing Code

### Making Precise Changes

Modifications are where the Edit tool earns its name.

A simple change:
```text
In config.js, change the database port from 5432 to 3306
```

Claude Code shows you the diff:
```diff
Old:
const DB_PORT = 5432;

New:
const DB_PORT = 3306;
```

Adding to existing code:
```text
In the login function, add validation to check if the password is at least 8 characters
```

Removing code:
```text
Remove the console.log statements from auth.js
```

---

### Preserving Existing Patterns

Do this every time: tell Claude Code to follow the style that's already there.

```text
Add a new endpoint for deleting users.
Follow the same pattern as the existing endpoints in routes/users.js
```

Claude Code reads the existing endpoints, matches their shape, reuses the same error handling, and keeps the naming conventions intact.

---

### Making Safe Edits

Review changes before they land. Ask for the plan first:

```text
Show me what you would change to add error handling to the database connection, but don't make the changes yet
```

Once you've looked it over:
```text
That looks good, please apply the changes
```

---

### Editing Multiple Files

For a change that ripples across files:

```text
Add TypeScript types for the User model.
Update:
- models/user.js (rename to user.ts)
- All files that import User
- Type definitions
```

Claude Code makes the changes in the right order, updates every import, keeps it all consistent, and checks nothing broke.

---

## Lesson 4: Refactoring Code

### What is Refactoring?

Improving the structure of code without changing what it does. The goals are plain: easier to read, less complex, no duplication, and less painful to maintain later.

---

### Removing Code Duplication

Find the duplication first:
```text
Look for duplicate code in the routes files and suggest refactoring
```

Then clear it out:
```text
I see the validation logic is duplicated in routes/users.js and routes/posts.js.
Extract it into a reusable validation module.
```

Result:
```text
Before:
routes/users.js - validation logic
routes/posts.js - same validation logic

After:
middleware/validation.js - shared validation
routes/users.js - uses validation middleware
routes/posts.js - uses validation middleware
```

---

### Improving Function Structure

Long functions are hard to read and harder to debug. Ask Claude Code to break them up:

```text
The processOrder function is 150 lines long.
Refactor it into smaller, focused functions.
```

It spots the logical sections, pulls each into its own well-named function, and keeps the behavior identical.

---

### Renaming for Clarity

```text
The variable 'x' in calculateTotal is unclear.
Rename it to something descriptive.
```

```javascript
// Before
function calculateTotal(x) {
  return x * 1.08;
}

// After
function calculateTotal(subtotal) {
  const TAX_RATE = 1.08;
  return subtotal * TAX_RATE;
}
```

---

### Modernizing Code

Working with older code? Ask Claude Code to drag it into the present:

```text
Refactor auth.js to use:
- async/await instead of callbacks
- const/let instead of var
- Arrow functions where appropriate
- Template literals instead of concatenation
```

---

### Optimizing Performance

```text
The search function is slow with large datasets.
Optimize it for better performance.
```

Depending on what's actually causing the drag, Claude Code might add caching, swap in a better algorithm, suggest indexes, or paginate the results.

---

## Lesson 5: Navigating Large Codebases

### Finding Specific Code

Use Grep to search inside files:

```text
Find all places where the sendEmail function is called
```

```text
Search for all TODO comments in the codebase
```

```text
Find all database queries that use the users table
```

---

### Finding Files

Use Glob to find files by name or pattern:

```text
Find all test files
```

```text
Find all TypeScript files in the src directory
```

```text
Show me all configuration files
```

---

### Understanding Code Flow

Tricky in a big project, but Claude Code handles it well. Ask it to trace the path:

```text
Trace the execution flow when a user logs in.
Start from the API endpoint and show me each step.
```

It finds the endpoint, shows the middleware chain, follows the calls down to the database queries, and lays out the whole flow.

---

### Finding Dependencies

Knowing what depends on what is the difference between a safe change and a 2am incident:

```text
What files depend on the User model?
```

```text
Show me all components that import the API service
```

With the LSP tool:
```text
Find all references to the authenticateUser function
```

---

## Lesson 6: Learning from Existing Code

### Pattern Recognition

One of the best ways to learn is to study code that already works:

```text
Show me 3 examples of how API endpoints are structured in this project
```

Then put it to use:
```text
Now create a new endpoint following that same pattern
```

---

### Understanding Best Practices

```text
Analyze the error handling in this codebase and explain:
- What patterns are used
- Why they work well
- How I should handle errors in my new code
```

---

### Code Review for Learning

```text
Review the authentication module and teach me:
- What security measures are in place
- Why each is important
- What I should do in similar code
```

---

## Lesson 7: File Organization Strategies

### Creating Logical Structure

Starting fresh? Have Claude Code lay out the structure:

```text
I'm building a REST API.
Create a project structure with:
- Clear separation of concerns
- Routes, controllers, services, models
- Test files organized with source files
- Configuration in separate folder
```

---

### Organizing by Feature

```text
Reorganize this codebase from:
- Files by type (all controllers together)
To:
- Files by feature (user feature has its own folder with controller, service, model, tests)
```

---

### Managing Large Files

```text
This server.js file is 500 lines.
Help me split it into logical modules.
```

---

## Hands-On Practice

### Exercise 1: Reading Code

**Task:** Understand an unfamiliar codebase

Pick any open source project and:

1. Clone it
2. Ask: "Explain what this project does and how it's structured"
3. Ask: "Show me the main entry point and explain the initialization"
4. Ask: "Find the core business logic and explain how it works"

---

### Exercise 2: Writing Clean Code

**Task:** Create a well-structured module

```text
Create a blog post service module with:
- createPost(title, content, authorId)
- getPostById(id)
- updatePost(id, updates)
- deletePost(id)
- listPosts(options) with pagination

Requirements:
- Input validation for all functions
- Descriptive variable names
- JSDoc comments
- Error handling
- Return consistent response format
```

---

### Exercise 3: Refactoring

**Task:** Improve existing code

First, make a mess on purpose:
```text
Create a function that calculates shipping cost.
Make it poorly structured with unclear variable names.
```

Then clean it up:
```text
Refactor the calculateShipping function to:
- Use descriptive variable names
- Extract magic numbers to constants
- Add comments
- Improve readability
- Handle edge cases
```

---

### Exercise 4: Navigating Code

**Task:** Find specific functionality

In a project with multiple files:

1. "Find all database query functions"
2. "Show me where the User model is defined"
3. "Find all API routes"
4. "Trace what happens when POST /api/login is called"

---

## Module 4 Checklist

Before moving to Module 5, make sure you can:

- [ ] Read and understand existing code
- [ ] Write clean, well-structured new code
- [ ] Make precise edits using the Edit tool
- [ ] Refactor code to improve quality
- [ ] Navigate large codebases efficiently
- [ ] Find specific code using Grep and Glob
- [ ] Follow and apply existing code patterns
- [ ] Organize code logically

---

## Best Practices

### Reading Code
- Start with README and package.json
- Understand structure before details
- Trace execution flows
- Look for patterns
- Ask for explanations of unclear parts

### Writing Code
- Follow existing conventions
- Use descriptive names
- Add comments for complex logic
- Keep functions focused and small
- Validate inputs
- Handle errors properly
- Encode conventions in CLAUDE.md or rules files so Claude Code follows them automatically

### Editing Code
- Review changes before applying
- Make small, focused changes
- Test after each change
- Preserve existing patterns

### Refactoring
- Test before refactoring
- Make one change at a time
- Keep functionality identical
- Test after each refactor
- Commit working code before big refactors

---

## Common Questions (FAQ)

**Q: How do I know if my code is "clean"?**
Ask Claude Code: "Review this code for clarity and suggest improvements." It's genuinely good at this.

**Q: When should I refactor?**
When code is hard to follow, duplicated, or painful to change. If you dread opening a file, that file needs refactoring.

**Q: How do I learn coding patterns?**
Read good code, ask Claude Code to explain the patterns you spot, then implement them yourself. That third step is the one that sticks.

**Q: What if I break something while editing?**
This is exactly what Git is for. Commit before you change things and you can always roll back.

**Q: How detailed should my edit requests be?**
Be specific about *what* to change; let Claude Code handle *how*.

---

## Pro Tips

1. **Always read before modifying** -- understand the code first
2. **Follow the principle of least surprise** -- match existing patterns
3. **Refactor in small steps** -- don't try to perfect everything at once
4. **Use meaningful names** -- code is read far more often than it's written
5. **Ask Claude Code to explain** -- learn from the code you're working with
6. **Test incrementally** -- verify changes work before moving on
7. **Git commit frequently** -- it's your safety net for experiments
8. **Codify your standards** -- if you keep repeating the same conventions to Claude Code, put them in a CLAUDE.md file or in `.claude/rules/`. Once written, Claude Code follows them automatically, every session.

---

> **Want to build a real code analysis tool?** The [Real Projects Pack](https://payhip.com/b/dFXWO) includes a Code Review Tool and Bug Finder Assistant that put these file and code skills to work.

---

<div align="center">

[← Previous Module](module-03-understanding-tools.md) · [🏠 Home](README.md) · [Next Module →](module-05-prompt-engineering.md)

</div>
