# Module 10: Development Workflow Best Practices

**Goal:** Learn professional development workflows to maximize productivity with Claude Code

**Estimated Time:** 35-45 minutes

---

## What You'll Learn in This Module

By the end of this module, you will:
- Use task management effectively with TodoWrite
- Follow code review best practices
- Write clear, useful documentation
- Implement robust error handling
- Add meaningful logging
- Organize code for maintainability
- Apply security best practices
- Optimize for performance

---

## Lesson 1: Task Management with TodoWrite

### What is TodoWrite?

**TodoWrite** is Claude Code's built-in task management system that helps you:
- Track what you're working on
- See progress at a glance
- Stay organized on complex tasks
- Communicate progress to others

---

### When to Use TodoWrite

**Use TodoWrite for:**
- Multi-step features (3+ steps)
- Complex tasks requiring planning
- When user requests multiple things
- Tracking implementation progress

**Don't use for:**
- Single, simple tasks
- Trivial changes
- Quick fixes

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

**Claude Code creates:**
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

**Claude Code automatically:**
- Marks tasks in_progress when starting
- Marks completed when done
- Shows current status
- Only one task in_progress at a time

**You can ask:**
```
Show me the current todo list
What's left to do?
Mark task 3 as completed
```

---

## Lesson 2: Code Review Best Practices

### Review Before Committing

**Always review changes before committing:**

```
Before I commit, show me:
1. All files that changed
2. What changed in each file
3. Any potential issues
4. Suggestions for improvement
```

---

### Self-Review Checklist

**Ask Claude Code to check:**

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

**When reviewing pull requests:**

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

**When to add comments:**

**Good comments explain WHY:**
```javascript
// Use exponential backoff to avoid overwhelming the API
// when it's experiencing issues
const delay = Math.pow(2, retryCount) * 1000;
```

**Bad comments explain WHAT (code should be self-explanatory):**
```javascript
// Multiply 2 to the power of retryCount times 1000
const delay = Math.pow(2, retryCount) * 1000;
```

---

### Function Documentation

**Request JSDoc comments:**

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

**Create comprehensive READMEs:**

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

**Ask for proper error handling:**

```
Add error handling to the database queries in userService.js:
- Try/catch blocks for all async operations
- Specific error messages for different failures
- Proper error types (not just Error)
- Logging of errors
- Don't expose internal errors to users
```

---

### Error Types

**Create custom error classes:**

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

**Consistent error format:**

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

**What to log:**
- Important operations (user login, data changes)
- Errors and exceptions
- Performance metrics
- Security events
- External API calls

**What NOT to log:**
- Passwords or sensitive data
- Excessive debug info in production
- Personal identifiable information (PII)

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

**Request structured logs:**

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

**Organize by feature:**

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

**Find and eliminate duplication:**

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

**Adding a new feature professionally:**

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

---

## Hands-On Practice

### Exercise 1: Refactor for Quality

**Task:** Improve code quality

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

**Task:** Find and fix security issues

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

**Task:** Speed up slow code

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

- Write it right the first time
- Tests save time in the long run
- Documentation helps future you
- Security is not optional

### Communication

- Clear commit messages
- Comprehensive PR descriptions
- Good documentation
- Meaningful logs

### Continuous Improvement

- Review your own code critically
- Learn from mistakes
- Stay updated on best practices
- Refactor when appropriate

---

## What's Next?

Congratulations! You've completed all **10 core modules**!

You now have:
- ✅ Strong foundation in Claude Code
- ✅ Ability to build complete applications
- ✅ Professional development skills
- ✅ Best practices knowledge

**Optional: Advanced Modules**
- Module 11: MCP Servers
- Module 12: Skills and Hooks
- Module 13: Multiple Languages/Frameworks
- Module 14: API Integration
- Module 15: Production Deployment

**Or start building!**
- Apply these skills to real projects
- Contribute to open source
- Build your portfolio
- Help others learn

---

## Final Pro Tips

1. **Plan before coding** - A few minutes planning saves hours debugging

2. **Test as you build** - Don't leave testing for later

3. **Commit often** - Small, frequent commits are better

4. **Document while fresh** - Write docs while you remember why

5. **Security first** - It's harder to add later

6. **Performance matters** - But don't optimize prematurely

7. **Code is read more than written** - Optimize for readability

8. **Ask for help** - Claude Code is always there to assist

---

*Module 10 Complete - Core Course Finished!*

**You're now ready to build professional applications with Claude Code!**
