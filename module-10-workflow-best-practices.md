# Module 10: Development Workflow Best Practices

**Goal:** Learn professional development workflows to maximize productivity with Claude Code

**Estimated Time:** 35-45 minutes

---

## What You'll Learn

This module covers the habits and practices that separate throwaway scripts from production-quality code. We're talking task management, code review, documentation, error handling, logging, code organization, security, and performance. It's a lot -- but these are the things that'll make your code something you're actually proud of six months from now.

---

## Lesson 1: Task Management with TodoWrite

### What is TodoWrite?

TodoWrite is Claude Code's built-in task management system. It tracks what you're working on, shows progress at a glance, and keeps you organized when a task has more moving parts than you can hold in your head. If you've ever lost track of where you were halfway through a feature, this is the fix.

---

### When to Use TodoWrite

Use it for multi-step features (3+ steps), complex tasks that need planning, situations where a user requests multiple things at once, or when you just want a clear record of implementation progress.

Skip it for single simple tasks, trivial changes, and quick fixes. Not everything needs a checklist.

---

### Creating a Todo List

```
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
```
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
```
Show me the current todo list
What's left to do?
Mark task 3 as completed
```

---

## Lesson 2: Code Review Best Practices

### Review Before Committing

Always review changes before committing. Trust me on this -- it's the single easiest way to catch mistakes before they become permanent.

```
Before I commit, show me:
1. All files that changed
2. What changed in each file
3. Any potential issues
4. Suggestions for improvement
```

---

### Self-Review Checklist

Ask Claude Code to run through the common pitfalls:

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
The userController.js file is doing too much.
Refactor so that:
- Controller only handles HTTP requests/responses
- Service layer handles business logic
- Repository layer handles database
- Each layer has single responsibility
```

---

## Lesson 7: Security Best Practices

### Input Validation

Fair warning: this is one of those areas where cutting corners will come back to bite you.

```
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

```
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

```
Ensure no secrets in code:
1. Find any hardcoded secrets
2. Move to environment variables
3. Add .env to .gitignore
4. Create .env.example template
5. Document required env vars
```

---

### Security Headers

```
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

```
Optimize database queries:
1. Add indexes on frequently queried columns
2. Fix N+1 query problems
3. Use joins instead of multiple queries
4. Implement pagination for large result sets
5. Add database query logging to find slow queries
```

---

### Caching

```
Implement caching for expensive operations:
- Cache database queries that rarely change
- Cache external API responses
- Set appropriate TTL (time to live)
- Implement cache invalidation strategy
```

Cache invalidation is one of the famously hard problems in computer science, so don't feel bad if it takes some thought.

---

### Async Operations

```
Optimize with async operations:
- Use Promise.all for parallel operations
- Don't await in loops (use Promise.all or for...of)
- Move slow operations to background jobs
- Implement job queues for heavy tasks
```

---

## Lesson 9: Complete Workflow Example

### Feature Development Flow

Here's what a professional feature development cycle looks like end to end:

**Step 1: Plan**
```
Create a todo list for adding password reset feature
```

**Step 2: Create branch**
```
Create a new git branch: feature/password-reset
```

**Step 3: Write tests first (TDD)**
```
Write tests for password reset:
- Request reset token
- Verify token
- Reset password
```

**Step 4: Implement**
```
Implement password reset following the plan:
- Add database column for reset tokens
- Create request reset endpoint
- Create verify and reset endpoint
- Add proper validation
- Add error handling
- Add logging
```

**Step 5: Review**
```
Review all changes before committing:
- Check for security issues
- Verify error handling
- Check test coverage
- Review code quality
```

**Step 6: Commit**
```
Create a commit with meaningful message
```

**Step 7: Create PR**
```
Create a pull request with:
- Summary of changes
- Testing steps
- Security considerations
```

This flow might feel like a lot of overhead at first, but it catches problems early and keeps your git history clean. Future you will appreciate it.

---

## Hands-On Practice

### Exercise 1: Refactor for Quality

Take an existing messy file and clean it up:

```
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

```
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

```
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
