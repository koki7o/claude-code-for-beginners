<div align="center">

![Module 10](https://img.shields.io/badge/Module_10-0066FF?style=for-the-badge&labelColor=1a1a2e)
![Time](https://img.shields.io/badge/⏱_40_min-555555?style=for-the-badge&labelColor=1a1a2e)
![Difficulty](https://img.shields.io/badge/Intermediate-0066FF?style=for-the-badge&labelColor=1a1a2e)

# Development Workflow Best Practices

**Learn professional development workflows with Claude Code**

[← Previous](module-09-real-world-project.md) · [🏠 Home](README.md) · [Next →](module-11-mcp-servers.md)

</div>

---

## What You'll Learn

This module covers the habits and practices that separate throwaway scripts from production-quality code. We're talking task management, code review, documentation, error handling, logging, code organization, security, and performance. It's a lot -- but these are the things that'll make your code something you're actually proud of six months from now.

---

## Lesson 1: Task Management with TodoWrite

### What is TodoWrite?

When you give Claude Code a complex, multi-step task, it automatically creates an internal todo list to track its own progress. You'll see a checklist appear showing what Claude plans to do, what it's working on, and what's done. You don't need to manage this list yourself -- just give Claude a clear breakdown of what you need.

---

### When to Use TodoWrite

Use it for multi-step features (3+ steps), complex tasks that need planning, situations where a user requests multiple things at once, or when you just want a clear record of implementation progress.

Skip it for single simple tasks, trivial changes, and quick fixes. Not everything needs a checklist.

---

### Creating a Todo List

```text
I need to add user authentication to the app.

Create a todo list for:
1. Set up database tables for users
2. Create registration endpoint
3. Create login endpoint
4. Add JWT token generation
5. Add authentication middleware
6. Protect existing routes
7. Add tests
8. Update documentation
```

Claude Code creates something like this:
```text
✓ Set up database tables for users
→ Create registration endpoint (in progress)
○ Create login endpoint
○ Add JWT token generation
○ Add authentication middleware
○ Protect existing routes
○ Add tests
○ Update documentation
```

---

### Tracking Progress

Claude Code automatically marks tasks as in_progress when it starts them and completed when it finishes. Only one task shows as in_progress at a time, so you always know exactly where things stand.

You can ask things like:
```text
What's left to do?
What have you completed so far?
What's the next step?
```

---

## Lesson 2: Code Review Best Practices

### Review Before Committing

Always review changes before committing. Trust me on this -- it's the single easiest way to catch mistakes before they become permanent.

```text
Before I commit, show me:
1. All files that changed
2. What changed in each file
3. Any potential issues
4. Suggestions for improvement
```

---

### Self-Review Checklist

Ask Claude Code to run through the common pitfalls:

```text
Review my changes and check for:
- Security vulnerabilities
- Performance issues
- Missing error handling
- Code duplication
- Unclear variable names
- Missing tests
- Incomplete documentation
```

---

### Reviewing Others' Code

When you're reviewing a pull request, Claude Code can help you get up to speed quickly:

```text
I need to review this pull request.
Help me:
1. Understand what it does
2. Check for bugs
3. Verify it follows our patterns
4. Suggest improvements
5. Check test coverage
```

---

## Lesson 3: Documentation

### Code Comments

Here's the deal: good comments explain *why*, not *what*. The code itself should tell you what's happening. Comments should tell you the reasoning behind it.

Good:
```javascript
// Use exponential backoff to avoid overwhelming the API
// when it's experiencing issues
const delay = Math.pow(2, retryCount) * 1000;
```

Bad:
```javascript
// Multiply 2 to the power of retryCount times 1000
const delay = Math.pow(2, retryCount) * 1000;
```

If your comment just restates the code in English, delete it.

---

### Function Documentation

You can ask Claude Code to add JSDoc comments across a whole file:

```text
Add JSDoc comments to all functions in userService.js with:
- Description of what the function does
- Parameter types and descriptions
- Return type and description
- Examples of usage
- Any exceptions thrown
```

---

### README Files

A solid README is the front door to your project. Ask Claude Code to scaffold one:

```text
Create a README.md for this project with:
- Project description
- Features list
- Prerequisites
- Installation instructions
- Usage examples
- API documentation
- Contributing guidelines
- License information
```

---

### API Documentation

```text
Generate API documentation for all endpoints in routes/:
- For each endpoint list:
  * HTTP method and path
  * Description
  * Authentication requirements
  * Request parameters/body
  * Response format with examples
  * Possible errors
```

---

## Lesson 4: Error Handling

### Comprehensive Error Handling

This matters more than you think. Unhandled errors are how apps crash in production at 2 AM.

```text
Add error handling to the database queries in userService.js:
- Try/catch blocks for all async operations
- Specific error messages for different failures
- Proper error types (not just Error)
- Logging of errors
- Don't expose internal errors to users
```

That last point is important -- your users should get a clean "something went wrong" message, not a raw stack trace with your database schema in it.

---

### Error Types

Custom error classes make your error handling way more precise:

```text
Create custom error classes for:
- ValidationError (400)
- AuthenticationError (401)
- AuthorizationError (403)
- NotFoundError (404)
- DatabaseError (500)

Each should:
- Extend Error
- Set appropriate status code
- Have meaningful message
```

---

### Error Responses

Keep your error responses consistent. Pick a format and stick with it across every endpoint:

```text
Create error response middleware that returns:
{
  "error": {
    "status": 400,
    "message": "Validation failed",
    "details": [
      "Email is required",
      "Password must be at least 8 characters"
    ]
  }
}
```

---

## Lesson 5: Logging

### Strategic Logging

Log the right things:
- Important operations -- user login, data changes
- Errors and exceptions
- Performance metrics
- Security events
- External API calls

And don't log these:
- Passwords or sensitive data (seriously, never)
- Excessive debug info in production
- Personal identifiable information (PII)

Getting this balance right is an underrated skill.

---

### Implement Logging

```text
Add logging throughout the application:

1. Create logger utility with levels:
   - error: Errors that need attention
   - warn: Warning conditions
   - info: Important events
   - debug: Detailed debugging (dev only)

2. Add logging to:
   - All error handlers
   - Authentication attempts
   - Database operations
   - External API calls

3. Include useful context:
   - Timestamp
   - User ID (if applicable)
   - Request ID
   - Operation being performed
```

---

### Log Format

Structured logs -- JSON format -- are far easier to search and filter than plain text, especially once you're running in production:

```text
Configure logger to output:
{
  "timestamp": "2025-01-15T10:30:00.000Z",
  "level": "info",
  "message": "User logged in",
  "userId": "123",
  "ip": "192.168.1.1",
  "requestId": "abc-def-123"
}
```

---

## Lesson 6: Code Organization

### File Structure

Organizing by feature instead of by type makes a huge difference as your project grows. When everything related to "users" lives in one folder, you don't have to jump between five different directories to understand one feature.

```text
Help me reorganize from:
src/
  ├── routes/       (all routes together)
  ├── controllers/  (all controllers together)
  └── models/       (all models together)

To:
src/
  ├── users/
  │   ├── user.model.js
  │   ├── user.controller.js
  │   ├── user.routes.js
  │   └── user.test.js
  ├── tasks/
  │   ├── task.model.js
  │   ├── task.controller.js
  │   ├── task.routes.js
  │   └── task.test.js
```

---

### DRY Principle (Don't Repeat Yourself)

Duplication is how bugs multiply. Fix it in one place, forget about the other three copies, and now you've got inconsistent behavior.

```text
Find duplicate code in this project and refactor:
1. Search for repeated logic
2. Extract into shared functions
3. Create utility modules
4. Update all uses
5. Test that nothing breaks
```

---

### Single Responsibility

If a file is doing too many things, it's time to split it up:

```text
The userController.js file is doing too much.
Refactor so that:
- Controller only handles HTTP requests/responses
- Service layer handles business logic
- Repository layer handles database
- Each layer has single responsibility
```

---

### Codifying Standards with Rules

Instead of relying on memory or hoping everyone follows the same conventions, you can encode your project's standards into rules files that Claude Code follows automatically.

**Create a rules directory:**
```text
.claude/rules/
├── code-style.md        # Formatting and naming conventions
├── error-handling.md    # How errors should be handled
├── testing.md           # Testing requirements
└── security.md          # Security requirements
```

**Example rule file (.claude/rules/error-handling.md):**
```markdown
---
description: Error handling standards for all application code
globs: ["src/**/*.ts", "src/**/*.js"]
---

## Error Handling Rules

- All async functions must use try/catch
- Never expose internal error details to API responses
- Use custom error classes (ValidationError, NotFoundError, etc.)
- Log all errors with context (userId, requestId, operation)
- Database errors must be wrapped in application-specific errors
- Always include a meaningful error message for debugging
```

**Why this matters:** Rules are enforced automatically every time Claude Code works on matching files. No more "we forgot to add error handling" -- it's baked into the workflow. This is especially powerful on teams where multiple people use Claude Code on the same codebase.

---

## Lesson 7: Security Best Practices

### Input Validation

Fair warning: this is one of those areas where cutting corners will come back to bite you.

```text
Add comprehensive input validation:
- Validate all user input
- Sanitize data before database queries
- Use parameterized queries (prevent SQL injection)
- Validate file uploads
- Check input length limits
- Whitelist allowed values
```

---

### Authentication & Authorization

```text
Review and improve authentication security:
- Use bcrypt for password hashing (salt rounds 10+)
- Implement rate limiting on login
- Add account lockout after failed attempts
- Use HTTPS only
- Secure cookie settings
- Validate JWT tokens properly
- Implement refresh tokens
```

---

### Secrets Management

Hardcoded secrets in source code are one of the most common -- and most preventable -- security mistakes.

```text
Ensure no secrets in code:
1. Find any hardcoded secrets
2. Move to environment variables
3. Add .env to .gitignore
4. Create .env.example template
5. Document required env vars
```

---

### Security Headers

```text
Add security headers using helmet.js:
- Content Security Policy
- X-Frame-Options
- X-Content-Type-Options
- Strict-Transport-Security
- X-XSS-Protection
```

---

## Lesson 8: Performance Optimization

### Database Optimization

Slow queries are the most common performance bottleneck in web apps, and they're usually the easiest to fix:

```text
Optimize database queries:
1. Add indexes on frequently queried columns
2. Fix N+1 query problems
3. Use joins instead of multiple queries
4. Implement pagination for large result sets
5. Add database query logging to find slow queries
```

---

### Caching

```text
Implement caching for expensive operations:
- Cache database queries that rarely change
- Cache external API responses
- Set appropriate TTL (time to live)
- Implement cache invalidation strategy
```

Cache invalidation is one of the famously hard problems in computer science, so don't feel bad if it takes some thought.

---

### Async Operations

```text
Optimize with async operations:
- Use Promise.all for parallel operations
- Use Promise.all when loop iterations are independent and can run in parallel
- Use for...of with await when operations must run sequentially
- Avoid forEach with async callbacks (it doesn't await properly)
- Move slow operations to background jobs
- Implement job queues for heavy tasks
```

---

## Lesson 9: Complete Workflow Example

### Feature Development Flow

Here's what a professional feature development cycle looks like end to end:

**Step 1: Plan**
```text
Create a todo list for adding password reset feature
```

**Step 2: Create branch**
```text
Create a new git branch: feature/password-reset
```

**Step 3: Write tests first (TDD)**
```text
Write tests for password reset:
- Request reset token
- Verify token
- Reset password
```

**Step 4: Implement**
```text
Implement password reset following the plan:
- Add database column for reset tokens
- Create request reset endpoint
- Create verify and reset endpoint
- Add proper validation
- Add error handling
- Add logging
```

**Step 5: Review**
```text
Review all changes before committing:
- Check for security issues
- Verify error handling
- Check test coverage
- Review code quality
```

**Step 6: Commit**
```text
Create a commit with meaningful message
```

**Step 7: Create PR**
```text
Create a pull request with:
- Summary of changes
- Testing steps
- Security considerations
```

This flow might feel like a lot of overhead at first, but it catches problems early and keeps your git history clean. Future you will appreciate it.

---

### Advanced Workflow: The Automated Pipeline

Once you're comfortable with the manual workflow above, you can automate chunks of it. Here's a professional pipeline that handles quality checks automatically:

**Step 1: Pre-flight checks**
```text
Before I start working on this feature, run these checks:
1. Are all tests passing?
2. Is the linter clean?
3. Are there any uncommitted changes?
4. Is my branch up to date with main?
```

**Step 2: Research-first development**
```text
Before writing any code for this feature:
1. Explore how similar features are implemented in this codebase
2. Check if there are existing utilities or patterns I should reuse
3. Identify which files will need to change
4. Create a plan, then implement
```

This "research before coding" approach prevents the common mistake of reimplementing something that already exists or breaking patterns the rest of the codebase follows.

**Step 3: Automated quality gate**
```text
After implementing, run the full quality check:
1. Run the linter -- fix any issues
2. Run the test suite -- fix any failures
3. Check test coverage -- add tests if coverage dropped
4. Review the diff for security issues
5. Generate a summary of all changes
```

**The key insight:** The best workflows aren't about following more steps -- they're about automating the steps so quality becomes effortless. Set up the automation once, and every feature you build goes through the same rigorous process without any extra effort.

---

## Lesson 10: Session Management

These commands and shortcuts will save you significant time and tokens. Learn them early.

### Essential Commands

| Command | What It Does | When to Use It |
|---------|-------------|----------------|
| `/clear` | Reset context completely | Between unrelated tasks — stale context wastes tokens |
| `/compact [focus]` | Summarize context | At ~50% context usage. Add focus: `/compact Focus on API changes` |
| `/cost` | Show token usage | Anytime you want to check spending |
| `/btw` | Side question in overlay | Quick questions that don't need to stay in history |
| `/effort low\|high\|max` | Adjust reasoning depth | `low` for simple lookups, `max` for architecture decisions |
| `/rewind` | Open checkpoint menu | Undo a wrong turn — restore conversation, code, or both |
| `/context` | Visualize context usage | See what's taking up space |
| `/rename` | Name the current session | Makes it easy to resume later with `claude --resume name` |

### Essential Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Esc` | Stop current generation |
| `Esc + Esc` | Open rewind/checkpoint menu |
| `Shift+Tab` | Cycle permission modes (Default → AcceptEdits → Plan → Auto) |
| `Ctrl+G` | Open plan in your external editor |
| `Ctrl+O` | Toggle verbose mode (see Claude's thinking) |
| `Ctrl+B` | Send current task to background — you can keep working |
| `Alt+T` | Toggle thinking on/off |

### The `/clear` Habit

This is the single easiest way to reduce token usage. When you finish a task and start something unrelated, type `/clear`. Otherwise, Claude Code carries forward all the context from the previous task -- every file it read, every command it ran -- and processes it on every subsequent message. That adds up fast.

### Proactive `/compact`

Don't wait for auto-compact at 95% context. By that point, Claude may lose important details. Instead, compact proactively around 50% with instructions about what to keep:

```text
/compact Focus on the database migration changes and the test failures
```

One thing to remember: CLAUDE.md survives compaction (it's re-read from disk), but anything you only said in conversation is summarized and may lose detail. If something is important enough to survive compaction, put it in CLAUDE.md.

---

## Hands-On Practice

### Exercise 1: Refactor for Quality

Take an existing messy file and clean it up:

```text
Take an existing messy file and refactor it:
1. Add proper error handling
2. Add logging
3. Split large functions
4. Remove duplication
5. Add documentation
6. Add tests
```

---

### Exercise 2: Security Audit

Find and fix security issues in an application:

```text
Review this application for security issues:
1. Check input validation
2. Check authentication
3. Look for SQL injection risks
4. Check for exposed secrets
5. Verify secure headers
6. Fix all issues found
```

---

### Exercise 3: Performance Optimization

Take a slow endpoint and make it faster:

```text
This endpoint is slow. Optimize it:
1. Find the bottleneck
2. Add database indexes
3. Implement caching
4. Optimize queries
5. Measure improvement
```

---

## Module 10 Checklist

Before completing this course, make sure you can:

- [ ] Use TodoWrite for task management
- [ ] Review code before committing
- [ ] Write clear documentation
- [ ] Implement proper error handling
- [ ] Add strategic logging
- [ ] Organize code maintainably
- [ ] Follow security best practices
- [ ] Optimize for performance
- [ ] Apply professional workflows

---

## The Professional Developer Mindset

### Quality Over Speed

Write it right the first time. Tests save time in the long run -- even when it doesn't feel like it in the moment. Documentation helps future you. And security is never optional.

### Communication

Clear commit messages, comprehensive PR descriptions, good documentation, meaningful logs. Code is a team sport, even when the team is just present-you and future-you.

### Continuous Improvement

Review your own code critically. Learn from mistakes. Stay updated on best practices. Refactor when appropriate -- not constantly, but when the code is clearly fighting you.

---

## What's Next?

That wraps up the 10 core modules. You've got a solid foundation in Claude Code, you can build complete applications, and you know the professional practices that keep projects maintainable.

If you want to keep going, there are advanced modules available:
- Module 11: MCP Servers
- Module 12: Skills and Hooks
- Module 13: Multiple Languages/Frameworks
- Module 14: API Integration
- Module 15: Production Deployment

Or just start building. Apply these skills to real projects, contribute to open source, build your portfolio. The best way to get better is to ship things.

---

## Final Pro Tips

1. **Plan before coding** -- A few minutes planning saves hours debugging.

2. **Test as you build** -- Don't leave testing for the end. It always gets skipped.

3. **Commit often** -- Small, frequent commits beat one massive commit every time.

4. **Document while fresh** -- Write docs while you still remember why you made the decisions you made.

5. **Security first** -- It's much harder to bolt on later.

6. **Performance matters** -- But don't optimize prematurely. Measure first.

7. **Code is read more than written** -- Optimize for readability.

8. **Ask for help** -- Claude Code is there whenever you need a second pair of eyes.

---

*Module 10 Complete -- Core Course Finished!*

Next up: if you're ready for more, the advanced modules start with Module 11 -- MCP Servers, which opens up a whole new layer of what Claude Code can do.

> **Ready to go professional?** The [Advanced Modules](https://payhip.com/b/8E107) cover production deployment at scale, enterprise integration, performance optimization, and custom agent orchestration. Or grab the [Real Projects Pack](https://payhip.com/b/dFXWO) to practice these workflows on 11 real builds. [Bundle both and save $10.](https://payhip.com/b/8E107+real-projects-bundle)

---

<div align="center">

[← Previous Module](module-09-real-world-project.md) · [🏠 Home](README.md) · [Next Module →](module-11-mcp-servers.md)

</div>
