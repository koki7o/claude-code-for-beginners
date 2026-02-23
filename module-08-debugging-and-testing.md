# Module 8: Debugging and Testing

**Goal:** Learn how to find and fix problems with AI assistance and write comprehensive tests

**Estimated Time:** 45-55 minutes

---

## What You'll Learn

This module covers the two skills that'll save you the most headaches as a developer: debugging and testing. You'll work through reading error messages, systematic debugging strategies, writing unit and integration tests, test-driven development, and handling performance issues. Trust me on this -- getting comfortable with these early pays off more than almost anything else.

---

## Lesson 1: Reading and Understanding Error Messages

### Anatomy of an Error Message

Error messages tell you four things:
- *What* went wrong
- *Where* it happened (file and line)
- *Why* it happened (sometimes)
- The *stack trace* -- the path the code took to reach the error

---

### Example Error Message

```
TypeError: Cannot read property 'name' of undefined
    at getUser (/app/services/userService.js:15:24)
    at /app/routes/users.js:42:18
    at Layer.handle [as handle_request] (/app/node_modules/express/lib/router/layer.js:95:5)
```

Here's how to read it:

**Error Type:** `TypeError` -- the category of error.

**Error Message:** `Cannot read property 'name' of undefined` -- you're trying to access `.name` on something that's `undefined`.

**Stack Trace:**
- Line 1: Error occurred in `userService.js` at line 15
- Line 2: Called from `users.js` at line 42
- Line 3: Part of Express routing (framework code -- you can usually ignore these lines)

---

### Getting Help from Claude Code

Paste the error directly into Claude Code and ask about it:

```
I'm getting this error:
TypeError: Cannot read property 'name' of undefined
    at getUser (/app/services/userService.js:15:24)

Can you explain what this means and how to fix it?
```

Claude Code will explain the error in plain English, show you the problematic code, suggest fixes, and explain why the error happened in the first place.

---

## Lesson 2: Systematic Debugging Strategies

### The Debugging Process

There's a temptation to just start changing things at random when something breaks. Resist that. Follow a process instead.

**1. Reproduce the Error**
```
The app crashes sometimes when users log in.
Help me create a reliable way to reproduce this error.
```

**2. Isolate the Problem**
```
The error happens in the login flow.
Show me the login function and let's trace through it step by step.
```

**3. Identify the Cause**
```
Read the login function and tell me what could cause it to crash.
```

**4. Fix the Issue**
```
Add validation to check if the user object exists before accessing properties.
```

**5. Verify the Fix**
```
Run the tests to make sure the fix works and doesn't break anything else.
```

---

### Using Console Logs for Debugging

This is the oldest trick in the book, and it still works great. Ask Claude Code to add debug logs:

```
Add console.log statements to the login function to show:
- When the function is called
- What the input parameters are
- What the database query returns
- Any intermediate values
```

And when you're done debugging, clean up after yourself:

```
Remove all the debug console.log statements I added
```

---

### Using Debugger Statements

For more control, you can drop a `debugger` statement into your code:

```
Add a debugger statement in the calculateTotal function
right before it calculates the tax.
```

Then run with:
```bash
node inspect app.js
```

---

## Lesson 3: Writing Unit Tests

### What are Unit Tests?

Unit tests verify that individual functions and components work correctly in isolation. They catch bugs early, serve as documentation for how your code should behave, make refactoring much safer, and generally push you toward better code. If you're not writing them yet, this is the module where that changes.

---

### Writing Your First Test with Claude Code

Tell Claude Code what you want to test and what cases matter:

```
Write unit tests for the calculateTotal function using Jest.

Test cases:
- Calculates correctly with positive numbers
- Returns 0 for empty cart
- Handles discounts correctly
- Throws error for negative prices
```

Claude Code will create something like this:

```javascript
// calculateTotal.test.js
const { calculateTotal } = require('./calculateTotal');

describe('calculateTotal', () => {
  test('calculates correctly with positive numbers', () => {
    const items = [
      { price: 10, quantity: 2 },
      { price: 5, quantity: 1 }
    ];
    expect(calculateTotal(items)).toBe(25);
  });

  test('returns 0 for empty cart', () => {
    expect(calculateTotal([])).toBe(0);
  });

  test('handles discounts correctly', () => {
    const items = [{ price: 100, quantity: 1 }];
    const discount = 0.1; // 10% off
    expect(calculateTotal(items, discount)).toBe(90);
  });

  test('throws error for negative prices', () => {
    const items = [{ price: -10, quantity: 1 }];
    expect(() => calculateTotal(items)).toThrow('Price cannot be negative');
  });
});
```

---

### Running Tests

```
Run the Jest tests
```

Claude Code runs:
```bash
npm test
```

And shows you the results:
```
PASS  ./calculateTotal.test.js
  calculateTotal
    ✓ calculates correctly with positive numbers (3ms)
    ✓ returns 0 for empty cart (1ms)
    ✓ handles discounts correctly (2ms)
    ✓ throws error for negative prices (4ms)

Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
```

---

### Testing Edge Cases

The happy path is easy. Edge cases are where bugs hide. Ask for them explicitly:

```
Add tests for edge cases to the calculateTotal tests:
- Very large numbers
- Floating point precision
- null or undefined inputs
- Missing quantity field
- Zero quantity
```

---

## Lesson 4: Integration Tests

### What are Integration Tests?

Integration tests verify that different parts of your application work together correctly -- things like API endpoints, database operations, authentication flows, and third-party API calls. Unit tests check the pieces; integration tests check the wiring between them.

---

### Writing API Integration Tests

```
Create integration tests for the /api/users endpoint using Supertest.

Test:
- GET /api/users returns list of users
- POST /api/users creates a new user
- POST /api/users validates required fields
- GET /api/users/:id returns specific user
- GET /api/users/:id returns 404 for non-existent user
- PUT /api/users/:id updates user
- DELETE /api/users/:id deletes user
```

Claude Code creates:

```javascript
// users.test.js
const request = require('supertest');
const app = require('../app');
const db = require('../db');

describe('Users API', () => {
  beforeEach(async () => {
    // Clear database before each test
    await db.clear('users');
  });

  describe('GET /api/users', () => {
    test('returns list of users', async () => {
      const response = await request(app)
        .get('/api/users')
        .expect(200);

      expect(Array.isArray(response.body)).toBe(true);
    });
  });

  describe('POST /api/users', () => {
    test('creates a new user', async () => {
      const newUser = {
        name: 'John Doe',
        email: 'john@example.com'
      };

      const response = await request(app)
        .post('/api/users')
        .send(newUser)
        .expect(201);

      expect(response.body).toMatchObject(newUser);
      expect(response.body).toHaveProperty('id');
    });

    test('validates required fields', async () => {
      const response = await request(app)
        .post('/api/users')
        .send({ name: 'John' }) // missing email
        .expect(400);

      expect(response.body).toHaveProperty('error');
    });
  });

  // More tests...
});
```

---

## Lesson 5: Test-Driven Development (TDD)

### What is TDD?

The short version: write tests BEFORE writing code.

The process is a tight loop:
1. Write a failing test
2. Write the minimum code to make it pass
3. Refactor if needed
4. Repeat

It feels backwards at first. Stick with it -- it gets natural fast, and it forces you to think about what your code should actually do before you start typing.

---

### TDD Example with Claude Code

**Step 1: Write the test first**
```
I want to create a password validator function.
First, write tests for it that check:
- Password is at least 8 characters
- Contains at least one uppercase letter
- Contains at least one number
- Contains at least one special character

Don't write the function yet, just the tests.
```

**Step 2: Watch it fail**
```
Run the tests
```

Tests fail because the function doesn't exist yet. That's exactly what you want.

**Step 3: Implement**
```
Now create the validatePassword function that makes all the tests pass.
```

**Step 4: Verify**
```
Run the tests again
```

Tests pass.

**Step 5: Refactor**
```
The validatePassword function works but is hard to read.
Refactor it to be more clear while keeping tests passing.
```

---

## Lesson 6: Debugging Common Scenarios

### Scenario 1: Null/Undefined Errors

```
TypeError: Cannot read property 'x' of undefined
```

You'll see this one constantly. Debug it with Claude Code:

```
I'm getting "Cannot read property 'name' of undefined" in the getUser function.
Help me:
1. Find where the undefined value comes from
2. Add proper null checking
3. Handle the case gracefully
```

---

### Scenario 2: Async/Await Issues

```
Promise rejection: Database query failed
UnhandledPromiseRejectionWarning
```

These can be tricky because the error often surfaces far from where the actual problem is. Try:

```
I have an unhandled promise rejection in the database query.
Show me:
1. Where the promise is rejected
2. Why it's not being caught
3. How to add proper error handling with try/catch
```

---

### Scenario 3: Logic Errors

Code runs without crashing but gives wrong results. These are the sneaky ones -- no error message to guide you.

```
The shopping cart total is calculating incorrectly.
It should be $25 but shows $30.

Help me debug the calculateTotal function:
1. Add logging to show each step
2. Trace through with example data
3. Find where the calculation goes wrong
```

---

### Scenario 4: Performance Issues

Code works, but it's painfully slow.

```
The user dashboard takes 5 seconds to load.

Help me:
1. Find which database queries are slow
2. Check for N+1 query problems
3. Add logging to measure each step
4. Suggest optimizations
```

Claude Code might come back with something like:

```
Performance Analysis:

Found issues:
1. N+1 query: Loading user's posts one at a time (100 queries!)
   Fix: Use JOIN to load in 1 query

2. Missing database index on user_id
   Fix: Add index for faster lookups

3. Loading all posts instead of paginating
   Fix: Add pagination (limit 20 per page)

Estimated improvement: 5s → 0.5s
```

---

## Lesson 7: Test Coverage and Quality

### Checking Test Coverage

```
Run tests with coverage report
```

Claude Code runs:
```bash
npm test -- --coverage
```

And shows coverage:
```
File             | % Stmts | % Branch | % Funcs | % Lines |
-----------------|---------|----------|---------|---------|
userService.js   |   85.71 |    75.00 |   100.0 |   85.71 |
authService.js   |   60.00 |    50.00 |    66.67|   60.00 |
```

---

### Improving Coverage

```
The authService.js only has 60% coverage.
Write additional tests to cover:
- The error handling branches
- The password reset function
- Edge cases in token generation
```

---

### Mocking External Dependencies

You don't want your tests actually sending emails or hitting third-party APIs. That's where mocking comes in.

```
Create tests for the sendEmail function, but mock the actual email sending.
The tests should verify the function is called with correct parameters
without actually sending emails.
```

Claude Code creates:

```javascript
jest.mock('../services/emailProvider');
const { sendEmail } = require('../services/emailService');
const emailProvider = require('../services/emailProvider');

test('sends welcome email with correct content', async () => {
  const user = {
    email: 'test@example.com',
    name: 'Test User'
  };

  await sendEmail.welcome(user);

  expect(emailProvider.send).toHaveBeenCalledWith({
    to: 'test@example.com',
    subject: 'Welcome!',
    body: expect.stringContaining('Test User')
  });
});
```

---

## Hands-On Practice

### Exercise 1: Debug a Broken Function

**Task:** Find and fix bugs

```
Create a broken calculateShipping function that has 3 bugs:
1. Doesn't handle zero weight
2. Calculates wrong total for international shipping
3. Returns string instead of number

Then help me debug it step by step.
```

---

### Exercise 2: Write Tests for Existing Code

**Task:** Add test coverage

```
I have an existing authentication module with no tests.
Create comprehensive unit tests for:
- login function
- register function
- validateToken function
- changePassword function

Aim for 100% coverage.
```

---

### Exercise 3: TDD New Feature

**Task:** Build feature test-first

```
Using TDD, create a shopping cart module.

Step 1: Write tests for addItem, removeItem, getTotal, applyDiscount
Step 2: Implement minimal code to pass tests
Step 3: Refactor for better code quality
```

---

### Exercise 4: Debug Performance Issue

**Task:** Find and fix slow code

```
Create a slow function that loads user data inefficiently.
Then help me:
1. Measure performance
2. Identify bottlenecks
3. Optimize the code
4. Verify improvement
```

---

## Module 8 Checklist

Before moving to Module 9, make sure you can:

- [ ] Read and understand error messages
- [ ] Use systematic debugging strategies
- [ ] Write unit tests for functions
- [ ] Create integration tests for APIs
- [ ] Practice test-driven development
- [ ] Debug common error scenarios
- [ ] Improve test coverage
- [ ] Use mocking for external dependencies

---

## Best Practices

### Debugging

- Read error messages carefully -- the answer is often right there
- Reproduce issues reliably before trying to fix them
- Use logging strategically
- Test your fixes thoroughly
- Document what caused the bug so you don't hit it again
- Add tests to prevent regressions

### Testing

- Write tests for new code as you go, not "later"
- Test edge cases and error paths, not just the happy path
- Keep tests simple and focused on one thing
- Use descriptive test names -- future you will thank you
- Mock external dependencies so tests stay fast and reliable
- Aim for good coverage, but don't obsess over hitting exactly 100%

---

## Common Questions (FAQ)

**Q: How much test coverage is enough?**
Aim for 80%+, but focus on critical paths. 100% coverage doesn't mean zero bugs -- it just means every line ran at least once.

**Q: Should I test everything?**
Focus on business logic, edge cases, and error handling. Don't bother testing framework code or trivial getters/setters.

**Q: How do I debug intermittent bugs?**
Add extensive logging, try to reproduce consistently, and check for race conditions. These are genuinely hard -- fair warning.

**Q: What's the difference between unit and integration tests?**
Unit tests check functions in isolation. Integration tests check how pieces work together. You need both.

**Q: Should I write tests for old code that has none?**
Add tests when you modify that code. Don't spend weeks retroactively testing things that already work.

---

## Pro Tips

1. **Read errors carefully** -- the answer is often in the message itself
2. **Add tests as you code** -- leaving testing for later means it won't happen
3. **Use TDD for complex logic** -- it saves time in the long run, even though it feels slower at first
4. **Debug systematically** -- randomly changing things is not debugging
5. **Keep tests fast** -- slow test suites don't get run
6. **Mock wisely** -- mock I/O and external services, not business logic
7. **Test behavior, not implementation** -- your tests should survive a refactor

---

Next up: Module 9 -- where you'll pull everything together and build a complete, real-world project from scratch.
