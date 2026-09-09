<div align="center">

![Module 5](https://img.shields.io/badge/Module_5-32CD32?style=for-the-badge&labelColor=1a1a2e)
![Time](https://img.shields.io/badge/⏱_35_min-555555?style=for-the-badge&labelColor=1a1a2e)
![Difficulty](https://img.shields.io/badge/Beginner-32CD32?style=for-the-badge&labelColor=1a1a2e)

# Effective Prompting for Coding Tasks

**Master the art of communicating coding tasks to AI**

[← Previous](module-04-working-with-files.md) · [Home](README.md) · [Next →](module-06-background-agents.md)

</div>

---

## What You'll Learn

This is the one that moves the needle most: writing good prompts. What you get back is almost entirely a function of what you put in. We'll cover being specific, giving useful context, building in layers, and a handful of prompt patterns that just work for common coding jobs.

---

## Lesson 1: The Anatomy of a Good Prompt

### What Makes a Good Coding Prompt?

A good prompt is specific - Claude Code knows exactly what you want. Contextual - you've given it what it needs to know. Actionable - there's a clear thing to do. And testable - you can check the result actually works.

Sounds obvious. It's also easy to underrate. So let's look at real examples.

### Bad vs Good Prompts

#### Example 1: Creating a Function

**Bad:**
```text
Make a function
```

**Better:**
```text
Create a JavaScript function that adds two numbers
```

**Best:**
```text
Create a JavaScript function called addNumbers that:
- Takes two parameters (a and b)
- Returns their sum
- Includes input validation to ensure both are numbers
- Has JSDoc comments
```

#### Example 2: Fixing a Bug

**Bad:**
```text
Fix the bug
```

**Better:**
```text
Fix the login bug
```

**Best:**
```text
The login function in auth.js is not handling empty passwords correctly.
When a user submits an empty password, the app crashes with "Cannot read property 'length' of undefined".
Please add validation to check for empty passwords and return an appropriate error message.
```

"Fix the bug" versus that last one is night and day. The more you say about what's happening, what you expected, and where to look, the faster you get to a fix.

---

## Lesson 2: Being Specific About Technical Requirements

### Specify Technologies

**Vague:**
```text
Create a web server
```

**Specific:**
```text
Create an Express.js web server using TypeScript
```

**Very Specific:**
```text
Create an Express.js web server using TypeScript with:
- CORS enabled
- JSON body parsing
- Request logging with Morgan
- Error handling middleware
- Port 3000
```

### Specify Architecture

**Vague:**
```text
Add a database
```

**Specific:**
```text
Add SQLite database using better-sqlite3
```

**Very Specific:**
```text
Add SQLite database with:
- better-sqlite3 library
- Database file at ./data/app.db
- Create users table with id, username, email, created_at
- Add connection helper in /db/connection.js
- Include migration script
```

### Specify Code Style

**Basic:**
```text
Write a React component
```

**With Style Preferences:**
```text
Write a React functional component using:
- Hooks (useState, useEffect)
- TypeScript for prop types
- Arrow function syntax
- Named export
- Tailwind CSS for styling
```

Worth the extra seconds. Leave it out and Claude Code makes reasonable choices - they just might not be your project's choices. A little specificity now saves a rewrite later.

---

## Lesson 3: Providing Context

### Project Context

On an existing project, context is the whole game:

**Without Context:**
```text
Add a new route
```

**With Context:**
```text
This is an Express.js API with routes in /routes folder.
Each route file exports a router with the route handlers.
Add a new route for /api/users that returns all users from the database.
Follow the pattern used in /routes/posts.js
```

That last line - "Follow the pattern used in /routes/posts.js" - is quietly one of the most powerful things you can write. It tells Claude Code to match your style instead of inventing its own.

### Code Context

When you're changing specific code:

**Without Context:**
```text
Add error handling
```

**With Context:**
```text
In the login function (auth.js, line 45),
add try-catch error handling that:
- Catches any database errors
- Returns 500 status with generic error message
- Logs the actual error to console
- Doesn't expose internal errors to users
```

### Business Context

For features that lean on domain knowledge:

**Without Context:**
```text
Add validation to the form
```

**With Context:**
```text
Add validation to the user registration form:
- Email must be valid format and not already exist
- Password must be 8+ characters with 1 number and 1 special char
- Username must be 3-20 characters, alphanumeric only
- Age must be 18 or older (we're a financial services app)
Show errors inline below each field, not in an alert
```

That parenthetical - "we're a financial services app" - changes how Claude Code thinks. It might reach for stricter validation or compliance checks you hadn't asked for but definitely wanted.

---

## Lesson 4: Iterative Development

### Start Simple, Add Complexity

Cramming an entire feature into one giant prompt rarely goes well. Build it in layers instead.

**Step 1: Basic Version**
```text
Create a simple todo list CLI that can:
- Add a task
- List all tasks
- Use an in-memory array
```

**Step 2: Add Features**
```text
Now add the ability to:
- Mark tasks as complete
- Delete tasks
```

**Step 3: Persist Data**
```text
Now save tasks to a JSON file so they persist between runs
```

**Step 4: Improve UX**
```text
Now add:
- Colors for completed vs incomplete tasks
- Numbered list for easy reference
- Clear screen between operations
```

### Why Iterate?

Each step is small enough to understand, test, and fix on its own. When something breaks, you already know which change did it. And you actually learn what each piece does - instead of squinting at a wall of generated code wondering how any of it connects.

---

## Lesson 5: Common Prompt Patterns

### Pattern 1: The Scaffold Pattern

For starting a new project.

**Template:**
```text
Create a [project type] with this structure:
- [folder/file 1]
- [folder/file 2]
- [folder/file 3]

Use [technologies]
Include [configurations]
```

**Example:**
```text
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

For adding new functionality.

**Template:**
```text
Add a [feature name] feature that:
- [Capability 1]
- [Capability 2]
- [Capability 3]

It should work by [describe behavior]
```

**Example:**
```text
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

For debugging.

**Template:**
```text
[Symptom/Error message]
This happens when [steps to reproduce]
The expected behavior is [what should happen]
Please [investigate/fix/explain]
```

**Example:**
```text
Error: "Cannot POST /api/login"
This happens when I submit the login form
The expected behavior is a 200 response with a token
Please investigate why the route isn't working
```

---

### Pattern 4: The Refactor Pattern

For improving existing code.

**Template:**
```text
Refactor [code/file/function] to:
- [Improvement 1]
- [Improvement 2]
- [Improvement 3]

Maintain current functionality
```

**Example:**
```text
Refactor the user validation functions to:
- Reduce code duplication
- Make them more reusable
- Add better error messages
- Use a validation library like Joi

Maintain all current validation rules
```

That last line is the one people forget. Always say what shouldn't change - otherwise Claude Code might "improve" something you were perfectly happy with.

---

### Pattern 5: The Test Pattern

For writing tests.

**Template:**
```text
Create tests for [function/component/feature] that verify:
- [Test case 1]
- [Test case 2]
- [Error cases]
- [Edge cases]

Use [testing framework]
```

**Example:**
```text
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

For when you want Claude Code to remember your preferences across the whole project.

**Template:**
```text
Create a CLAUDE.md for this project that encodes:
- How to build, test, and lint
- Code style preferences
- Architecture patterns to follow
- Things to avoid
```

**Example:**
```text
Create a CLAUDE.md that says:
- Use async/await, never callbacks
- All API responses use { data: T } or { error: { status, message } }
- Tests go next to source files, named *.test.ts
- Use Zod for input validation, not manual checks
- Never use console.log -- use the logger utility
- Run tests with: npm test
- Run linter with: npm run lint
```

**Why this works:** CLAUDE.md loads automatically every session when it sits in your project root. Write your preferences once and Claude Code just follows them - no more repeating yourself. As projects grow, this is one of the highest-leverage moves you've got.

---

### Pattern 7: The Interview Pattern

Instead of guessing what Claude Code needs to know, let it ask you.

**Template:**
```text
Interview me about [feature/requirement]. Ask me questions one at a time
to understand the requirements fully. When you have enough information,
write a spec to SPEC.md.
```

**Example:**
```text
Interview me about the notification system I want to build. Ask about
user preferences, delivery channels, retry logic, and anything else
you need. Then write a spec.
```

**Why this works:** You don't always know which details matter. Claude Code will poke at edge cases, error handling, and constraints you hadn't considered - and the spec that comes out is far more complete than one you'd write cold.

---

### Pattern 8: The Verification Target Pattern

The single highest-leverage prompting tip: always tell Claude Code how to check its own work.

**Template:**
```text
[Your request]

Verify by: [how to check it worked]
```

**Example:**
```text
Add input validation to the registration form.

Verify by:
- Run the existing tests (npm test)
- Try submitting with an empty email - should show an error
- Try submitting with a password under 8 characters - should show an error
- All existing tests should still pass
```

**Why this works:** With no verification target, Claude Code has to guess when it's done. Give it one and "done" becomes concrete - it keeps working until the criteria are actually met.

---

## Lesson 6: Asking Claude Code for Clarification

### When to Ask Questions

Don't guess - ask. Claude Code is genuinely good at helping you think a decision through before you commit to it.

**Scenario 1: Technology Choice**
```text
I need to add real-time notifications to my app.
What would you recommend: WebSockets, Server-Sent Events, or polling?
What are the tradeoffs?
```

**Scenario 2: Architecture Decision**
```text
I'm building a blog platform.
Should I use a SQL or NoSQL database?
What would work best for storing posts, comments, and user data?
```

**Scenario 3: Best Practices**
```text
I'm about to add user authentication.
What are the current best practices for:
- Password storage
- Session management
- JWT tokens
```

### Let Claude Code Ask YOU Questions

One of my favorite moves. Try this:

```text
Create a user dashboard for my app.
Ask me any questions you need to fully understand what I want.
```

Claude Code might come back with:
- What data should the dashboard show?
- Who are the users?
- What actions can they take?
- What's the visual style?
- Mobile responsive?

Surprisingly effective. It surfaces requirements you hadn't thought about, cuts the back-and-forth, and the result lands much closer to what you actually meant.

---

## Lesson 7: Advanced Prompting Techniques

### Chaining Prompts

For complex work, don't do it all in one shot. Chain simple prompts:

**Prompt 1:**
```text
Analyze this codebase and tell me how the routing is structured
```

**Prompt 2 (after understanding):**
```text
Now add a new route for user profiles following the same pattern
```

**Prompt 3:**
```text
Add validation middleware to that route
```

**Prompt 4:**
```text
Write tests for the new route
```

Each one builds on the last. Claude Code keeps context within a conversation, so it remembers what it did a few steps back.

### Providing Examples

Showing beats telling:

```text
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

Sometimes what you don't want matters just as much:

```text
Add user authentication but:
- Don't use any external libraries (implement from scratch)
- Don't store passwords in plain text
- Don't expose sensitive error messages
```

### Constraints and Requirements

Spell out the non-functional stuff - it's the part that quietly gets missed:

```text
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

Leave those out and you'll likely get a script that reads the whole file into memory. Fine for a small file - a crash waiting to happen on a big one.

---

## Hands-On Practice: Prompt Engineering

### Exercise 1: Improve These Prompts

**Bad Prompt 1:**
```text
Make an API
```

**Your improved version:**
```text
[Your answer here]
```

<details>
<summary>Suggested Answer</summary>

```text
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
```text
[Your answer here]
```

<details>
<summary>Suggested Answer</summary>

```text
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
```text
[Your answer here]
```

<details>
<summary>Suggested Answer</summary>

```text
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

**Mistake 1: Too Vague** -- Claude Code ends up guessing. Fix it by being specific about technologies, structure, and behavior.

**Mistake 2: Everything at Once** -- One giant prompt for a complex feature is hard to verify and hard to fix when it goes sideways. Break it up and build iteratively.

**Mistake 3: No Context** -- Claude Code doesn't magically know your project structure or conventions. Explain the patterns, show an example.

**Mistake 4: Assuming Knowledge** -- "Add the usual validation" means nothing. Whose usual? Say what you mean.

**Mistake 5: Not Testing Between Steps** -- Stacking a dozen changes without checking each one is a fast route to confusion. Request, test, then move on.

---

## Pro Tips

1. **Start conversations with context** -- "I'm working on a React app with TypeScript..." sets the stage for everything after.

2. **Reference your own code** -- "Following the pattern in auth.js..." is one of the most useful things you can say.

3. **State constraints up front** -- "Without using external libraries..." saves wasted effort.

4. **Ask for an explanation first** -- "Please explain your approach before implementing" can stop a wrong turn before it happens.

5. **Build in public** -- "Let's start with X, then add Y" makes your plan legible.

6. **Verify understanding** -- "Can you summarize what you're going to build?" catches a misread early.

7. **Watch how Claude Code reads your prompts.** When it misunderstands, that's free feedback on how to write the next one.

8. **Course-correct early** -- If Claude goes down the wrong path, don't keep piling on corrections. After two failed attempts, use `/clear` and start fresh with a better prompt. Faster than fighting accumulated context.

9. **Use @file references** -- Instead of "find the auth file", point straight at it: "Explain the logic in @src/utils/auth.js". The `@` prefix pulls the file in without Claude having to go looking.

---

## What's Next?

That's prompt engineering - honestly the skill that decides how productive you'll be with Claude Code. Time spent writing a clear prompt is never wasted. Every minute you put in, you get back several.

> **Take your prompting further.** The [Advanced Modules](https://payhip.com/b/8E107) cover professional prompt workflows including the RPI methodology (Research-Plan-Implement) and token-efficient session management (Module 23).

---

<div align="center">

[← Previous Module](module-04-working-with-files.md) · [🏠 Home](README.md) · [Next Module →](module-06-background-agents.md)

</div>
