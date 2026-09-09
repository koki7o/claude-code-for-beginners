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

The habits that separate a throwaway script from production code. Task management, code review, documentation, error handling, logging, code organization, security, performance. It's a lot - but these are the things that make your code something you're actually proud of six months from now, instead of something you're scared to open.

---

## Lesson 1: Task Management with TodoWrite

### What is TodoWrite?

Hand Claude Code a complex, multi-step task and it builds itself an internal todo list to track its own progress. You'll see a checklist appear - what it plans to do, what it's on now, what's done. You don't manage this list; you just give Claude a clear breakdown of what you need and it takes care of the rest.

---

### When to Use TodoWrite

Use it for multi-step features (3+ steps), complex tasks that need planning, requests that bundle several things at once, or any time you just want a clear record of progress.

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

Claude Code marks a task in_progress when it starts and completed when it finishes, on its own. Only one task is in_progress at a time, so you always know exactly where things stand.

You can ask things like:
```text
What's left to do?
What have you completed so far?
What's the next step?
```

---

## Lesson 2: Code Review Best Practices

### Review Before Committing

Always review changes before committing. It's the single easiest way to catch a mistake before it becomes permanent.

```text
Before I commit, show me:
1. All files that changed
2. What changed in each file
3. Any potential issues
4. Suggestions for improvement
```

---

### Self-Review Checklist

Have Claude Code walk the usual pitfalls:

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

Reviewing a pull request? Claude Code gets you up to speed fast:

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

Here's the rule: good comments explain *why*, not *what*. The code already says what's happening. The comment's job is the reasoning behind it.

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

If a comment just restates the code in English, delete it.

---

### Function Documentation

Have Claude Code add JSDoc across a whole file:

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

A solid README is the front door to your project. Have Claude Code scaffold one:

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

This one earns its keep. Unhandled errors are how apps fall over in production at 2am.

```text
Add error handling to the database queries in userService.js:
- Try/catch blocks for all async operations
- Specific error messages for different failures
- Proper error types (not just Error)
- Logging of errors
- Don't expose internal errors to users
```

That last point matters: your users should get a clean "something went wrong," not a raw stack trace with your database schema in it.

---

### Error Types

Custom error classes make your handling far more precise:

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

Keep error responses consistent. Pick a shape and use it on every endpoint:

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

Log the things worth logging:
- Important operations -- user login, data changes
- Errors and exceptions
- Performance metrics
- Security events
- External API calls

And keep these out:
- Passwords or sensitive data (seriously, never)
- Excessive debug info in production
- Personal identifiable information (PII)

Getting that balance right is an underrated skill.

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

Structured logs - JSON - are far easier to search and filter than plain text, especially once you're in production:

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

Organizing by feature instead of by type pays off big as a project grows. When everything about "users" lives in one folder, you're not hopping across five directories to understand one feature.

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

Duplication is how bugs multiply. Fix it in one place, forget the other three copies, and now your behavior is inconsistent.

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

If a file is doing too many jobs, split it up:

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

Instead of leaning on memory or hoping everyone follows the same conventions, encode your project's standards into rules files that Claude Code follows automatically.

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

**Why this matters:** rules get enforced automatically every time Claude Code touches a matching file. No more "we forgot to add error handling" - it's baked into the workflow. Especially powerful on a team where several people drive Claude Code on the same codebase.

---

## Lesson 7: Security Best Practices

### Input Validation

Fair warning: this is one of those areas where cutting a corner comes back to bite you.

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

Hardcoded secrets in source are one of the most common - and most preventable - security mistakes there is.

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

Slow queries are the most common bottleneck in a web app, and usually the easiest to fix:

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

Cache invalidation is famously one of the hard problems in computer science, so don't feel bad if it takes some thought.

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

What a professional feature cycle looks like, end to end:

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

Feels like overhead at first. But it catches problems early and keeps your git history clean - and future you notices the difference.

---

### Advanced Workflow: The Automated Pipeline

Once the manual flow above is second nature, you can automate chunks of it. Here's a professional pipeline that handles the quality checks for you:

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

Researching before coding heads off the classic mistake of reimplementing something that already exists, or breaking a pattern the rest of the codebase follows.

**Step 3: Automated quality gate**
```text
After implementing, run the full quality check:
1. Run the linter -- fix any issues
2. Run the test suite -- fix any failures
3. Check test coverage -- add tests if coverage dropped
4. Review the diff for security issues
5. Generate a summary of all changes
```

**The key insight:** the best workflows aren't about following more steps - they're about automating the steps so quality becomes effortless. Set it up once, and every feature you build runs the same rigorous process for free.

---

## Lesson 10: Session Management

These commands and shortcuts save real time and real tokens. Learn them early.

### Essential Commands

| Command | What It Does | When to Use It |
|---------|-------------|----------------|
| `/clear` | Reset context completely | Between unrelated tasks - stale context wastes tokens |
| `/compact [focus]` | Summarize context | At ~50% context usage. Add focus: `/compact Focus on API changes` |
| `/cost` | Show token usage | Anytime you want to check spending |
| `/btw` | Side question in overlay | Quick questions that don't need to stay in history |
| `/effort low\|high\|max` | Adjust reasoning depth | `low` for simple lookups, `max` for architecture decisions |
| `/rewind` | Open checkpoint menu | Undo a wrong turn - restore conversation, code, or both |
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
| `Ctrl+B` | Send current task to background - you can keep working |
| `Alt+T` | Toggle thinking on/off |

### The `/clear` Habit

The single easiest way to cut token usage. Finish a task, start something unrelated, type `/clear`. Otherwise Claude Code drags the whole previous task forward - every file it read, every command it ran - and re-processes it on every message. That adds up fast.

### Proactive `/compact`

Don't wait for auto-compact at 95%. By then Claude may have lost details that mattered. Compact on purpose around 50%, with instructions about what to keep:

```text
/compact Focus on the database migration changes and the test failures
```

One thing to remember: CLAUDE.md survives compaction (it's re-read from disk), but anything you only said in conversation gets summarized and can lose detail. If it's important enough to survive compaction, put it in CLAUDE.md.

### Recovering with Checkpoints and Rewind

Claude Code automatically saves a **checkpoint** before it makes changes - a snapshot of your conversation and your files at that moment. So when something goes wrong - a bad turn, the wrong file edited, or you just changed your mind - you don't have to untangle it by hand.

Press **`Esc` twice** (or run **`/rewind`**) to open the checkpoint menu. You can restore:

- **Conversation only** - rewind the chat, keep the code. For when the discussion went off track but the files are fine.
- **Code only** - undo the file changes, keep the conversation. For when the edits were wrong but you want to keep talking it through.
- **Both** - go fully back to how things were at that checkpoint.

Think of it as an undo button for the *whole session*, not just the last edit. It's the safety net that makes it fine to let Claude try things: if an approach doesn't pan out, rewind to a clean point and go again.

Two things to know:

- Checkpoints cover the changes *Claude* made through its tools - a **within-session** safety net, not long-term history. They don't replace git, so still commit your real milestones (Module 7).
- Rewinding the conversation also rewinds context, so anything learned *after* that checkpoint is gone too. If there's a detail worth keeping, note it (or put it in CLAUDE.md) before you rewind.

### Watching Your Cost

Claude Code isn't free to run. On a subscription (Pro/Max) or an API key, every message spends tokens - and tokens are either money or usage-limit budget. A few habits keep the number low without you thinking about it.

Check where you stand anytime:

- **`/cost`** - token usage and spend for the current session, including your prompt-cache hit ratio (a low ratio means context is being re-processed instead of reused).
- **`/usage`** - a broader breakdown, including per-loop usage so you can see which repeated actions burn the most.
- **`/context`** - a visual of what's filling your context window right now.

The levers that actually move the number, roughly by impact:

1. **`/clear` between unrelated tasks** - the single biggest win. Stale context is re-processed on *every* message.
2. **Match the tier to the task** - fast tier for mechanical work; save the most-capable tier for genuinely hard problems (Module 6). Running everything on the most-capable tier is the most common way people overspend.
3. **`/compact` proactively** - summarize a long session instead of dragging every detail forward.
4. **`/effort low` for simple lookups** - less reasoning means fewer tokens on tasks that don't need the depth.

None of this needs babysitting. Set the habits - `/clear` when you switch tasks, the right tier for the job - and glance at `/cost` now and then to confirm nothing's leaking.

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

Write it right the first time. Tests save time in the long run - even when it doesn't feel like it in the moment. Documentation helps future you. Security is never optional.

### Communication

Clear commit messages, comprehensive PR descriptions, good docs, meaningful logs. Code is a team sport, even when the team is just present-you and future-you.

### Continuous Improvement

Read your own code critically. Learn from the mistakes. Keep up with best practices. Refactor when it's warranted - not constantly, but when the code is clearly fighting you.

---

## What's Next?

That wraps the 10 core modules. You've got a solid foundation in Claude Code, you can build complete applications, and you know the professional practices that keep projects maintainable.

Want to keep going? There are advanced modules ahead:
- Module 11: MCP Servers
- Module 12: Skills and Hooks
- Module 13: Multiple Languages/Frameworks
- Module 14: API Integration
- Module 15: Production Deployment

Or just start building. Point these skills at real projects, contribute to open source, grow your portfolio. The fastest way to get better is to ship things.

---

## Final Pro Tips

1. **Plan before coding** -- A few minutes of planning saves hours of debugging.

2. **Test as you build** -- Don't leave testing for the end. The end is where it gets skipped.

3. **Commit often** -- Small, frequent commits beat one massive commit every time.

4. **Document while fresh** -- Write the docs while you still remember why you decided what you decided.

5. **Security first** -- Much harder to bolt on later.

6. **Performance matters** -- But don't optimize prematurely. Measure first.

7. **Code is read more than written** -- Optimize for readability.

8. **Ask for help** -- Claude Code is right there whenever you want a second pair of eyes.

---

*Module 10 Complete -- Core Course Finished!*

Next up: if you're ready for more, the advanced modules start with Module 11 -- MCP Servers, which opens a whole new layer of what Claude Code can do.

> **Ready to go professional?** The [Advanced Modules](https://payhip.com/b/8E107) cover production deployment at scale, enterprise integration, performance optimization, and custom agent orchestration. Or grab the [Real Projects Pack](https://payhip.com/b/dFXWO) to practice these workflows on 14 real builds. [Bundle both and save $10.](https://payhip.com/b/S8nU1)

---

<div align="center">

[← Previous Module](module-09-real-world-project.md) · [🏠 Home](README.md) · [Next Module →](module-11-mcp-servers.md)

</div>
