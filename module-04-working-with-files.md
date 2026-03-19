# Module 4: Working with Files and Code

**Goal:** Learn how to effectively read, write, and modify code with Claude Code

**Estimated Time:** 40-50 minutes

---

## What You'll Learn

This module covers the core of what you'll actually do day-to-day with Claude Code: reading existing code, writing new code, making edits without breaking things, refactoring, and navigating your way around a codebase. These skills matter more than you think -- they're the difference between flailing and being productive.

---

## Lesson 1: Reading and Understanding Code

### Why Reading Code Matters

Here's the deal: about 80% of programming is reading code, not writing it. You read code to understand how features work, to track down bugs, to figure out where to add something new, to pick up patterns, and to review what other people have written. Getting good at reading code -- with Claude Code's help -- will make everything else faster.

### Reading Single Files

Ask Claude Code to read and explain a file:

```
Read server.js and explain what it does
```

It'll show you the file contents, explain its main purpose, highlight key functions, and point out important patterns.

You can also zero in on specific parts:
```
Read the login function in auth.js and explain how it works
```

Or ask about structure:
```
Read package.json and tell me:
- What dependencies are used
- What scripts are available
- What's the entry point
```

---

### Reading Multiple Related Files

When you need to understand how a feature works across several files, just tell Claude Code which files to look at:

```
I want to understand how user authentication works.
Please read:
- routes/auth.js
- middleware/auth.js
- models/user.js
And explain the complete authentication flow
```

Claude Code will read all of them, trace the execution flow, explain how they connect, and show you how data moves between them.

---

### Understanding Project Structure

To get the big picture of a project:

```
Help me understand this project's structure.
Show me:
- The directory layout
- What each folder contains
- The entry point
- How the code is organized
```

Or use the Explore agent for a deeper dive:
```
Use the Explore agent to analyze this codebase and give me:
1. Overall architecture
2. Main components
3. Technology stack
4. How data flows through the system
```

---

### Reading Patterns and Conventions

This is one of the most underrated things you can do. Before you write anything, learn how the codebase already does things:

```
Show me how error handling is done in this project
Find examples of API endpoints and show me the pattern
How are database queries organized?
```

You'll learn the project's conventions, follow existing patterns, maintain consistency, and avoid reinventing solutions that already exist in the codebase.

---

## Lesson 2: Writing New Code

### Following Project Conventions

Before writing new code, understand the conventions first:

```
Before I add a new feature, show me:
- How routes are structured
- How controllers are organized
- Error handling patterns
- Naming conventions used
```

Then write code that follows those patterns:
```
Add a new route for /api/products following the same pattern as /api/users
```

---

### Writing Clean Code

Be explicit about what you want. The more specific your request, the better the output:

```
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

Ask for separation of concerns upfront:

```
Create a user service module that handles:
- Creating users
- Finding users
- Updating users
- Deleting users

Keep database logic separate from business logic
Export all functions
```

Result:
```
services/
  └── userService.js  (business logic)
database/
  └── userRepository.js  (database operations)
```

---

### Adding Documentation

Get in the habit of requesting docs as you write -- it's much easier than going back later:

```
Create a calculateTax function and include:
- JSDoc comments
- Parameter descriptions
- Return value description
- Example usage in comments
```

---

## Lesson 3: Editing Existing Code

### Making Precise Changes

The Edit tool is where Claude Code really shines for modifications.

A simple change:
```
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
```
In the login function, add validation to check if the password is at least 8 characters
```

Removing code:
```
Remove the console.log statements from auth.js
```

---

### Preserving Existing Patterns

Trust me on this -- always tell Claude Code to follow existing style:

```
Add a new endpoint for deleting users.
Follow the same pattern as the existing endpoints in routes/users.js
```

Claude Code will read the existing endpoints, match their structure, use the same error handling approach, and follow the naming conventions already in place.

---

### Making Safe Edits

Fair warning: you should review changes before they land. Ask Claude Code to show you the plan first:

```
Show me what you would change to add error handling to the database connection, but don't make the changes yet
```

Once you've looked it over:
```
That looks good, please apply the changes
```

---

### Editing Multiple Files

For changes that span several files:

```
Add TypeScript types for the User model.
Update:
- models/user.js (rename to user.ts)
- All files that import User
- Type definitions
```

Claude Code will make changes in the right order, update all imports, keep everything consistent, and check that nothing breaks.

---

## Lesson 4: Refactoring Code

### What is Refactoring?

Refactoring means improving code structure without changing what it does. The goals are straightforward: improve readability, reduce complexity, remove duplication, and make the code easier to maintain going forward.

---

### Removing Code Duplication

First, find the duplication:
```
Look for duplicate code in the routes files and suggest refactoring
```

Then refactor it:
```
I see the validation logic is duplicated in routes/users.js and routes/posts.js.
Extract it into a reusable validation module.
```

Result:
```
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

Long functions are hard to read and harder to debug. Ask Claude Code to break them apart:

```
The processOrder function is 150 lines long.
Refactor it into smaller, focused functions.
```

It'll identify logical sections, extract them into separate functions with clear names, and keep the behavior identical.

---

### Renaming for Clarity

```
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

If you're working with older code, ask Claude Code to bring it up to date:

```
Refactor auth.js to use:
- async/await instead of callbacks
- const/let instead of var
- Arrow functions where appropriate
- Template literals instead of concatenation
```

---

### Optimizing Performance

```
The search function is slow with large datasets.
Optimize it for better performance.
```

Claude Code might add caching, swap in a more efficient algorithm, suggest indexes, or implement pagination -- depending on what's actually causing the slowdown.

---

## Lesson 5: Navigating Large Codebases

### Finding Specific Code

Use Grep to search for code:

```
Find all places where the sendEmail function is called
```

```
Search for all TODO comments in the codebase
```

```
Find all database queries that use the users table
```

---

### Finding Files

Use Glob to find files by name or pattern:

```
Find all test files
```

```
Find all TypeScript files in the src directory
```

```
Show me all configuration files
```

---

### Understanding Code Flow

This can be tricky in large projects, but Claude Code handles it well. Ask it to trace execution:

```
Trace the execution flow when a user logs in.
Start from the API endpoint and show me each step.
```

It'll find the endpoint, show the middleware chain, follow function calls through to the database queries, and explain the complete flow.

---

### Finding Dependencies

Understanding what depends on what is crucial for safe changes:

```
What files depend on the User model?
```

```
Show me all components that import the API service
```

With the LSP tool:
```
Find all references to the authenticateUser function
```

---

## Lesson 6: Learning from Existing Code

### Pattern Recognition

One of the best ways to learn is by studying existing code:

```
Show me 3 examples of how API endpoints are structured in this project
```

Then apply what you've learned:
```
Now create a new endpoint following that same pattern
```

---

### Understanding Best Practices

```
Analyze the error handling in this codebase and explain:
- What patterns are used
- Why they work well
- How I should handle errors in my new code
```

---

### Code Review for Learning

```
Review the authentication module and teach me:
- What security measures are in place
- Why each is important
- What I should do in similar code
```

---

## Lesson 7: File Organization Strategies

### Creating Logical Structure

When starting a new project, ask Claude Code to set up the structure:

```
I'm building a REST API.
Create a project structure with:
- Clear separation of concerns
- Routes, controllers, services, models
- Test files organized with source files
- Configuration in separate folder
```

---

### Organizing by Feature

```
Reorganize this codebase from:
- Files by type (all controllers together)
To:
- Files by feature (user feature has its own folder with controller, service, model, tests)
```

---

### Managing Large Files

```
This server.js file is 500 lines.
Help me split it into logical modules.
```

---

## Hands-On Practice

### Exercise 1: Reading Code

**Task:** Understand an unfamiliar codebase

Choose any open source project and:

1. Clone it
2. Ask: "Explain what this project does and how it's structured"
3. Ask: "Show me the main entry point and explain the initialization"
4. Ask: "Find the core business logic and explain how it works"

---

### Exercise 2: Writing Clean Code

**Task:** Create a well-structured module

```
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

First, create messy code on purpose:
```
Create a function that calculates shipping cost.
Make it poorly structured with unclear variable names.
```

Then refactor it:
```
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
Ask Claude Code: "Review this code for clarity and suggest improvements." It's genuinely useful for this.

**Q: When should I refactor?**
When code is hard to understand, duplicated, or becoming painful to modify. If you dread touching a file, it probably needs refactoring.

**Q: How do I learn coding patterns?**
Read good code, ask Claude Code to explain the patterns you see, and practice implementing them yourself.

**Q: What if I break something while editing?**
This is why Git exists. Commit before making changes so you can always roll back.

**Q: How detailed should my edit requests be?**
Be specific about *what* to change, but let Claude Code handle the implementation details.

---

## Pro Tips

1. **Always read before modifying** -- understand the code first
2. **Follow the principle of least surprise** -- match existing patterns
3. **Refactor in small steps** -- don't try to perfect everything at once
4. **Use meaningful names** -- code is read far more often than it's written
5. **Ask Claude Code to explain** -- learn from the code you're working with
6. **Test incrementally** -- verify changes work before moving on
7. **Git commit frequently** -- it's your safety net for experiments
8. **Codify your standards** -- if you find yourself repeating the same conventions to Claude Code, put them in a CLAUDE.md file or in `.claude/rules/`. Once written, Claude Code follows them automatically in every session.

---

> **Want to build a real code analysis tool?** The [Real Projects Pack](https://payhip.com/b/dFXWO) includes a Code Review Tool and Bug Finder Assistant that put these file and code skills to work.

Next up: Module 5 -- where you'll learn prompt engineering techniques to communicate coding tasks more effectively and get better results from Claude Code.
