# Module 5: Effective Prompting for Coding Tasks

**Goal:** Master the art of communicating coding tasks to AI effectively

**Estimated Time:** 30-40 minutes

---

## What You'll Learn

This module is about the single most important skill you'll develop with Claude Code: writing good prompts. The quality of what you get back is almost entirely determined by the quality of what you put in. We'll cover how to be specific, how to give useful context, how to build things incrementally, and a handful of prompt patterns that work well for common coding tasks.

---

## Lesson 1: The Anatomy of a Good Prompt

### What Makes a Good Coding Prompt?

A good prompt is specific -- Claude Code knows exactly what you want. It's contextual -- you've given it the information it needs. It's actionable -- there's a clear thing to do. And it's testable -- you can verify the result actually works.

That sounds obvious, but it's easy to underestimate. Let's look at some real examples.

### Bad vs Good Prompts

#### Example 1: Creating a Function

**Bad:**
```
Make a function
```

**Better:**
```
Create a JavaScript function that adds two numbers
```

**Best:**
```
Create a JavaScript function called addNumbers that:
- Takes two parameters (a and b)
- Returns their sum
- Includes input validation to ensure both are numbers
- Has JSDoc comments
```

#### Example 2: Fixing a Bug

**Bad:**
```
Fix the bug
```

**Better:**
```
Fix the login bug
```

**Best:**
```
The login function in auth.js is not handling empty passwords correctly.
When a user submits an empty password, the app crashes with "Cannot read property 'length' of undefined".
Please add validation to check for empty passwords and return an appropriate error message.
```

The difference between "fix the bug" and that last version is night and day. The more you tell Claude Code about what's happening, what you expected, and where to look, the faster it'll get you to a fix.

---

## Lesson 2: Being Specific About Technical Requirements

### Specify Technologies

**Vague:**
```
Create a web server
```

**Specific:**
```
Create an Express.js web server using TypeScript
```

**Very Specific:**
```
Create an Express.js web server using TypeScript with:
- CORS enabled
- JSON body parsing
- Request logging with Morgan
- Error handling middleware
- Port 3000
```

### Specify Architecture

**Vague:**
```
Add a database
```

**Specific:**
```
Add SQLite database using better-sqlite3
```

**Very Specific:**
```
Add SQLite database with:
- better-sqlite3 library
- Database file at ./data/app.db
- Create users table with id, username, email, created_at
- Add connection helper in /db/connection.js
- Include migration script
```

### Specify Code Style

**Basic:**
```
Write a React component
```

**With Style Preferences:**
```
Write a React functional component using:
- Hooks (useState, useEffect)
- TypeScript for prop types
- Arrow function syntax
- Named export
- Tailwind CSS for styling
```

This matters more than you think. If you don't specify, Claude Code will make reasonable choices -- but they might not match your project's conventions. A few extra seconds of specificity saves you from rewriting things later.

---

## Lesson 3: Providing Context

### Project Context

When you're working on an existing project, context is everything:

**Without Context:**
```
Add a new route
```

**With Context:**
```
This is an Express.js API with routes in /routes folder.
Each route file exports a router with the route handlers.
Add a new route for /api/users that returns all users from the database.
Follow the pattern used in /routes/posts.js
```

That last line -- "Follow the pattern used in /routes/posts.js" -- is especially powerful. It tells Claude Code to match your existing style instead of inventing its own.

### Code Context

When modifying specific code:

**Without Context:**
```
Add error handling
```

**With Context:**
```
In the login function (auth.js, line 45),
add try-catch error handling that:
- Catches any database errors
- Returns 500 status with generic error message
- Logs the actual error to console
- Doesn't expose internal errors to users
```

### Business Context

For features that need domain knowledge:

**Without Context:**
```
Add validation to the form
```

**With Context:**
```
Add validation to the user registration form:
- Email must be valid format and not already exist
- Password must be 8+ characters with 1 number and 1 special char
- Username must be 3-20 characters, alphanumeric only
- Age must be 18 or older (we're a financial services app)
Show errors inline below each field, not in an alert
```

The parenthetical about being a financial services app? That kind of detail changes how Claude Code thinks about the problem. It might suggest stricter validation or compliance-related checks you hadn't considered.

---

## Lesson 4: Iterative Development

### Start Simple, Add Complexity

Here's the deal: trying to describe an entire feature in one massive prompt usually doesn't work well. Build it up in layers instead.

**Step 1: Basic Version**
```
Create a simple todo list CLI that can:
- Add a task
- List all tasks
- Use an in-memory array
```

**Step 2: Add Features**
```
Now add the ability to:
- Mark tasks as complete
- Delete tasks
```

**Step 3: Persist Data**
```
Now save tasks to a JSON file so they persist between runs
```

**Step 4: Improve UX**
```
Now add:
- Colors for completed vs incomplete tasks
- Numbered list for easy reference
- Clear screen between operations
```

### Why Iterate?

Each step is small enough to understand, test, and fix on its own. When something breaks, you know it was the most recent change. And you actually learn what each piece does -- instead of staring at a huge blob of generated code wondering how it all fits together.

---

## Lesson 5: Common Prompt Patterns

### Pattern 1: The Scaffold Pattern

Use this when starting a new project.

**Template:**
```
Create a [project type] with this structure:
- [folder/file 1]
- [folder/file 2]
- [folder/file 3]

Use [technologies]
Include [configurations]
```

**Example:**
```
Create a React TypeScript project with this structure:
- src/components (for React components)
- src/hooks (for custom hooks)
- src/utils (for helper functions)
- src/types (for TypeScript types)

Use Vite as the build tool
Include ESLint and Prettier configs
Add Tailwind CSS
```

---

### Pattern 2: The Feature Pattern

Use this when adding new functionality.

**Template:**
```
Add a [feature name] feature that:
- [Capability 1]
- [Capability 2]
- [Capability 3]

It should work by [describe behavior]
```

**Example:**
```
Add a user authentication feature that:
- Allows users to register with email and password
- Allows users to login
- Creates a JWT token on successful login
- Includes middleware to protect routes

Use bcrypt for password hashing
Store users in the database
```

---

### Pattern 3: The Fix Pattern

Use this when debugging issues.

**Template:**
```
[Symptom/Error message]
This happens when [steps to reproduce]
The expected behavior is [what should happen]
Please [investigate/fix/explain]
```

**Example:**
```
Error: "Cannot POST /api/login"
This happens when I submit the login form
The expected behavior is a 200 response with a token
Please investigate why the route isn't working
```

---

### Pattern 4: The Refactor Pattern

Use this when improving existing code.

**Template:**
```
Refactor [code/file/function] to:
- [Improvement 1]
- [Improvement 2]
- [Improvement 3]

Maintain current functionality
```

**Example:**
```
Refactor the user validation functions to:
- Reduce code duplication
- Make them more reusable
- Add better error messages
- Use a validation library like Joi

Maintain all current validation rules
```

That last line is critical. Always tell Claude Code what shouldn't change -- otherwise it might "improve" things you didn't want touched.

---

### Pattern 5: The Test Pattern

Use this when writing tests.

**Template:**
```
Create tests for [function/component/feature] that verify:
- [Test case 1]
- [Test case 2]
- [Error cases]
- [Edge cases]

Use [testing framework]
```

**Example:**
```
Create tests for the calculateTotal function that verify:
- Correctly sums positive numbers
- Handles negative numbers
- Returns 0 for empty array
- Throws error for non-numeric inputs
- Handles floating point precision

Use Jest
Aim for 100% code coverage
```

---

### Pattern 6: The Persistence Pattern

Use this when you want Claude Code to remember your preferences across a whole project.

**Template:**
```
Create a CLAUDE.md for this project that encodes:
- How to build, test, and lint
- Code style preferences
- Architecture patterns to follow
- Things to avoid
```

**Example:**
```
Create a CLAUDE.md that says:
- Use async/await, never callbacks
- All API responses use { data: T } or { error: { status, message } }
- Tests go next to source files, named *.test.ts
- Use Zod for input validation, not manual checks
- Never use console.log -- use the logger utility
- Run tests with: npm test
- Run linter with: npm run lint
```

**Why this works:** CLAUDE.md is loaded automatically every session. Instead of repeating your preferences, you write them once and Claude Code follows them forever. This is one of the highest-leverage things you can do as your projects grow.

---

## Lesson 6: Asking Claude Code for Clarification

### When to Ask Questions

Instead of guessing, just ask. Seriously -- Claude Code is great at helping you think through decisions before you commit to them.

**Scenario 1: Technology Choice**
```
I need to add real-time notifications to my app.
What would you recommend: WebSockets, Server-Sent Events, or polling?
What are the tradeoffs?
```

**Scenario 2: Architecture Decision**
```
I'm building a blog platform.
Should I use a SQL or NoSQL database?
What would work best for storing posts, comments, and user data?
```

**Scenario 3: Best Practices**
```
I'm about to add user authentication.
What are the current best practices for:
- Password storage
- Session management
- JWT tokens
```

### Let Claude Code Ask YOU Questions

This is one of my favorite patterns. Try this:

```
Create a user dashboard for my app.
Ask me any questions you need to fully understand what I want.
```

Claude Code might ask things like:
- What data should the dashboard show?
- Who are the users?
- What actions can they take?
- What's the visual style?
- Mobile responsive?

This is surprisingly effective. It surfaces requirements you hadn't thought about, cuts down on back-and-forth, and the end result is usually much closer to what you actually wanted.

---

## Lesson 7: Advanced Prompting Techniques

### Chaining Prompts

For complex tasks, don't try to do it all at once. Chain simple prompts together:

**Prompt 1:**
```
Analyze this codebase and tell me how the routing is structured
```

**Prompt 2 (after understanding):**
```
Now add a new route for user profiles following the same pattern
```

**Prompt 3:**
```
Add validation middleware to that route
```

**Prompt 4:**
```
Write tests for the new route
```

Each prompt builds on the previous one. Claude Code retains context within a conversation, so it remembers what it did in earlier steps.

### Providing Examples

Showing is better than telling:

```
I want to add more API endpoints.
Here's the pattern I'm using:

router.get('/posts', async (req, res) => {
  const posts = await db.all('SELECT * FROM posts');
  res.json(posts);
});

Please create similar endpoints for:
- GET /comments
- POST /comments
- DELETE /comments/:id
```

### Negative Instructions

Sometimes it's just as important to say what you don't want:

```
Add user authentication but:
- Don't use any external libraries (implement from scratch)
- Don't store passwords in plain text
- Don't expose sensitive error messages
```

### Constraints and Requirements

Be explicit about non-functional requirements -- these are the things that often get missed:

```
Create a data processing script that:
- Reads a CSV file
- Transforms the data
- Writes to a database

Requirements:
- Must handle files up to 1GB
- Must be memory efficient (stream processing)
- Must validate each row
- Must continue on errors (log but don't stop)
- Must provide progress updates
```

Without those requirements, you'd probably get a script that reads the entire file into memory. Fine for small files, but it'll crash on anything large.

---

## Hands-On Practice: Prompt Engineering

### Exercise 1: Improve These Prompts

**Bad Prompt 1:**
```
Make an API
```

**Your improved version:**
```
[Your answer here]
```

<details>
<summary>Suggested Answer</summary>

```
Create a REST API using Express.js with TypeScript that manages a library system:

Endpoints:
- GET /books - List all books
- GET /books/:id - Get one book
- POST /books - Add new book
- PUT /books/:id - Update book
- DELETE /books/:id - Remove book

Each book has: id, title, author, isbn, publishedYear, available

Include:
- Input validation
- Error handling
- SQLite database
- API documentation comments
```
</details>

---

### Exercise 2: Write Prompts for These Scenarios

**Scenario 1:** You need to add password reset functionality to an existing authentication system

**Your prompt:**
```
[Your answer here]
```

<details>
<summary>Suggested Answer</summary>

```
Add password reset functionality to the existing authentication system:

Flow:
1. User requests password reset with email
2. System generates unique reset token
3. System emails reset link (just log to console for now)
4. User clicks link with token
5. User enters new password
6. System validates token and updates password

Requirements:
- Reset tokens expire after 1 hour
- Tokens can only be used once
- New password must meet existing validation rules
- Use the existing user table, add resetToken and resetExpiry columns
- Follow the pattern in auth.js for other auth functions

Security:
- Hash the reset token before storing
- Clear token after successful reset
- Don't reveal if email exists in system
```
</details>

---

### Exercise 3: Debug Prompt

You have an error: `TypeError: Cannot read property 'map' of undefined` in your React component

**Your debugging prompt:**
```
[Your answer here]
```

<details>
<summary>Suggested Answer</summary>

```
I'm getting this error in my UserList component:
"TypeError: Cannot read property 'map' of undefined"

It happens when:
- Component first renders
- Before the fetch request completes

The component tries to map over a users array from state.

Expected behavior:
- Show loading state while fetching
- Show user list when data arrives
- Handle case where fetch fails

Please:
1. Show me the component code
2. Explain why the error occurs
3. Fix it with proper loading state handling
4. Add error handling for failed fetch
```
</details>

---

## Module 5 Checklist

Before moving to Module 6, make sure you can:

- [ ] Write specific, detailed prompts
- [ ] Provide relevant context
- [ ] Use iterative development approach
- [ ] Apply common prompt patterns
- [ ] Know when to ask for clarification
- [ ] Debug by writing clear problem descriptions
- [ ] Chain prompts for complex tasks

---

## Common Prompting Mistakes to Avoid

**Mistake 1: Too Vague** -- Claude Code has to guess what you want. Fix this by being specific about technologies, structure, and behavior.

**Mistake 2: Everything at Once** -- One massive prompt for a complex feature is hard to verify and hard to fix when something goes wrong. Break it into steps and build iteratively.

**Mistake 3: No Context** -- Claude Code doesn't magically know your project structure or conventions. Explain existing patterns, show examples.

**Mistake 4: Assuming Knowledge** -- "Add the usual validation" doesn't mean anything. What's usual? Be explicit about what you mean.

**Mistake 5: Not Testing Between Steps** -- Requesting many changes without verifying each one works is a recipe for confusion. Request, test, then proceed.

---

## Pro Tips

1. **Start conversations with context** -- "I'm working on a React app with TypeScript..." sets the stage for everything that follows.

2. **Reference your own code** -- "Following the pattern in auth.js..." is one of the most useful things you can say.

3. **State constraints up front** -- "Without using external libraries..." avoids wasted effort.

4. **Ask for an explanation first** -- "Please explain your approach before implementing" can save you from going down the wrong path.

5. **Build in public** -- "Let's start with X, then add Y" makes your plan clear.

6. **Verify understanding** -- "Can you summarize what you're going to build?" catches misunderstandings early.

7. **Pay attention to how Claude Code interprets your prompts.** If it misunderstands something, that's feedback on how to write the next prompt better.

---

## What's Next?

That covers prompt engineering -- honestly the skill that makes the biggest difference in how productive you'll be with Claude Code. Trust me on this: time spent writing a clear prompt is never wasted.

> **Take your prompting further.** The [Advanced Modules](https://payhip.com/b/8E107) cover professional prompt workflows including the RPI methodology (Research-Plan-Implement) and token-efficient session management (Module 23).

Next up: Module 6 -- where we'll get into background agents for handling complex tasks and codebase exploration.
