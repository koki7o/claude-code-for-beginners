# Module 7: Git Operations and Version Control

**Goal:** Master Git workflows with Claude Code's assistance

**Estimated Time:** 40-50 minutes

---

## What You'll Learn

This module covers how to use Git through Claude Code -- from basic commits and branches all the way to pull requests and merge conflict resolution. If you've ever written a commit message that just says "fixed stuff," this one's for you.

---

## Lesson 1: Why Version Control Matters

### What is Version Control?

Version control is a system that tracks changes to your code over time. Here's the short version of why it matters:

- **Unlimited undo** -- go back to any previous version of your code
- **Collaboration** -- work with other people without stepping on each other's changes
- **History** -- see what changed, when, and why
- **Safety net** -- experiment freely, knowing you can always roll back

### Git Basics

There are a few core concepts you'll need to know before we go further.

**Repository (Repo)** -- a project tracked by Git. It contains all your files and their entire change history. Repos can live on your machine (local) or on a server like GitHub or GitLab (remote).

**Commit** -- a snapshot of your code at a specific point in time. Every commit has a message describing what changed, and together they form a permanent record of your project's evolution.

**Branch** -- a parallel version of your code. Branches let you work on features in isolation without touching the main codebase. When you're done, you merge the branch back in.

**Remote** -- a copy of your repo hosted on a server, usually called "origin." This is how you back up your work and collaborate with others.

---

## Lesson 2: Basic Git Operations with Claude Code

### Checking Status

To see what's changed in your project:
```
Show me git status
What files have I modified?
```

Claude Code will run:
```bash
git status
```

The output tells you which files have been modified, which are new (untracked), which have been deleted, and which are already staged for a commit.

---

### Viewing Changes

To see the actual code differences:
```
Show me the git diff
What changes have I made to auth.js?
```

Claude Code will show lines added (green, with +), lines removed (red, with -), and some surrounding context so you can orient yourself.

**Example output:**
```diff
- const PORT = 3000;
+ const PORT = 8080;
```

---

### Creating Commits with Claude Code

This is one of the places where Claude Code saves you the most time.

**The old way:**
```bash
git add .
git commit -m "Updated the thing"  # Vague, unhelpful
```

**With Claude Code:**
```
Please create a commit with my current changes
```

When you ask for a commit, Claude Code runs `git status` and `git diff` to understand what changed, analyzes the differences, writes a descriptive commit message, and creates the commit with proper formatting.

**Example commit message Claude Code might write:**
```
Add user authentication with JWT tokens

- Implement login and register endpoints
- Add bcrypt for password hashing
- Create JWT middleware for protected routes
- Add user model and database schema
- Include input validation for auth requests

This provides secure user authentication for the API.
```

A lot better than "fixed stuff."

---

### Commit Message Best Practices

Claude Code follows standard conventions automatically, but it's worth knowing what good looks like:

**Structure:**
```
Brief summary (50 chars or less)

Longer description if needed:
- What changed
- Why it changed
- Any important details

Issue references: #123
```

**Good vs Bad:**

Bad:
```
- "fixed bug"
- "updated code"
- "changes"
- "asdfasdf"
```

Good:
```
- "Fix login error when password is empty"
- "Add email validation to user registration"
- "Refactor database connection for better error handling"
- "Update API endpoints to use async/await"
```

---

## Lesson 3: Working with Branches

### Why Use Branches?

Branches let you work on features without breaking the main codebase, try experiments safely, collaborate without stepping on each other, and keep different types of work organized. Trust me on this -- once you start using branches properly, you won't go back.

### Creating Branches with Claude Code

**Create a new branch:**
```
Create a new branch called feature/user-dashboard
```

Claude Code will:
```bash
git checkout -b feature/user-dashboard
```

You can also be more descriptive and let Claude Code pick a sensible name:
```
Create a feature branch for adding password reset
```

Claude Code might create:
```bash
git checkout -b feature/password-reset
```

### Branch Naming Conventions

**Common patterns:**
```
feature/feature-name    # New features
bugfix/bug-description  # Bug fixes
hotfix/critical-fix     # Urgent fixes
refactor/what-refactor  # Code improvements
docs/what-documentation # Documentation
```

**Examples:**
```
feature/user-authentication
bugfix/login-validation-error
hotfix/security-vulnerability
refactor/database-queries
docs/api-endpoints
```

### Switching Branches

**Switch to existing branch:**
```
Switch to the main branch
```

```bash
git checkout main
```

**Or:**
```
Switch back to my feature branch
```

---

### Merging Branches

**Merge feature into main:**
```
Merge the feature/user-dashboard branch into main
```

Claude Code will switch to main, pull the latest changes, merge the feature branch, and handle any conflicts -- which brings us to a later lesson.

---

## Lesson 4: Creating Pull Requests

### What is a Pull Request (PR)?

A pull request is how you propose merging your code into another branch. It's a request for review, a place for discussion about the changes, and a quality gate before code hits production.

### Creating PRs with Claude Code

This matters more than you think. Writing good PR descriptions is tedious, and Claude Code handles it really well.

**Simple request:**
```
Create a pull request for my changes
```

**What Claude Code does:**

1. Analyzes all commits in your branch
2. Reviews the code changes
3. Generates a comprehensive PR description -- summary, list of modifications, test plan, the works
4. Uses GitHub CLI to create the PR

**Example PR description Claude Code might generate:**

```markdown
## Summary

This PR adds user authentication to the application using JWT tokens.

## Changes

- ✨ Add login and registration endpoints
- 🔒 Implement JWT token generation and validation
- 🛡️ Add password hashing with bcrypt
- ✅ Create authentication middleware
- 📝 Add user model and database schema
- ✔️ Include comprehensive input validation

## Implementation Details

### Authentication Flow
1. User registers with email/password
2. Password is hashed using bcrypt (10 rounds)
3. User credentials stored in database
4. On login, credentials are verified
5. JWT token generated and returned
6. Token required for protected routes

### Security Measures
- Passwords hashed before storage
- JWT tokens expire after 24 hours
- Input validation on all auth endpoints
- SQL injection prevention with prepared statements

## Test Plan

- [ ] Test user registration with valid data
- [ ] Test registration with invalid email
- [ ] Test registration with weak password
- [ ] Test login with correct credentials
- [ ] Test login with wrong password
- [ ] Test protected routes without token
- [ ] Test protected routes with valid token
- [ ] Test protected routes with expired token

## Database Changes

Added `users` table:
```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

## Dependencies Added

- jsonwebtoken: JWT creation and verification
- bcrypt: Password hashing
- express-validator: Input validation

---

Generated with Claude Code
```

That would take you 20 minutes to write by hand. Claude Code does it in seconds.

---

### Customizing PR Creation

You can be specific about what you want:

```
Create a pull request with:
- Title: "Add user authentication"
- Target branch: develop (not main)
- Include screenshots from /screenshots folder
- Mark as draft
```

---

## Lesson 5: Code Review Before Committing

### Why Review First?

Reviewing your own changes before committing catches mistakes early, helps you understand exactly what you're shipping, and -- honestly -- it's just a good habit that'll save you from embarrassing "oops" commits.

### Review Workflow with Claude Code

**Step 1: See what changed**
```
Show me all the changes I'm about to commit
```

**Step 2: Review specific files**
```
Show me the diff for auth.js
Explain what changed in the database schema
```

**Step 3: Check for issues**
```
Review my changes and check for:
- Security vulnerabilities
- Code quality issues
- Missing error handling
- Inconsistent style
```

**Step 4: Make fixes if needed**
```
Fix the security issue you found in login.js
```

**Step 5: Commit when ready**
```
Now create a commit with these changes
```

---

## Lesson 6: Resolving Merge Conflicts

### What are Merge Conflicts?

Conflicts happen when two branches change the same line of code and Git doesn't know which version to keep. You have to decide. Fair warning: this can be tricky the first few times, but Claude Code makes it much less painful.

**Example conflict:**
```javascript
<<<<<<< HEAD
const PORT = 3000;
=======
const PORT = 8080;
>>>>>>> feature/new-port
```

### Resolving Conflicts with Claude Code

When you hit a conflict:
```
I have a merge conflict in server.js. Help me resolve it.
```

Claude Code will show you the conflict, explain both versions, ask which to keep -- or suggest combining them -- then resolve it and mark the file as resolved.

**Example dialogue:**
```
Claude Code: I see a conflict in server.js with the PORT value.
- HEAD (main branch): PORT = 3000
- feature/new-port: PORT = 8080

Which would you like to keep? Or should we make it configurable with an environment variable?

You: Make it configurable

Claude Code: I'll update it to use process.env.PORT with 8080 as default.
```

---

## Lesson 7: GitHub CLI Integration

### What is GitHub CLI (gh)?

`gh` is GitHub's official command-line tool, and Claude Code can use it directly. This gives you access to pull requests, issues, CI/CD status, releases, and more -- all without leaving your terminal.

### Common gh Operations

**View PRs:**
```
Show me all open pull requests
```

**Create issue:**
```
Create a GitHub issue for the bug I just found:
Title: Login fails with empty password
Description: [your description]
```

**Check PR status:**
```
What's the status of PR #42?
Has it passed all checks?
```

**Merge PR:**
```
Merge pull request #42
```

---

## Lesson 8: Complete Git Workflow Example

### Scenario: Adding a New Feature

Here's the full lifecycle of a feature, from branch to merge. This is the workflow you'll use over and over.

**Step 1: Create feature branch**
```
Create a new branch for adding password reset feature
```

**Step 2: Make changes**
```
Implement password reset functionality with:
- Request reset endpoint
- Reset token generation
- Password update endpoint
- Email sending (simulated)
```

**Step 3: Test your changes**
```
Run the tests
```

**Step 4: Review changes**
```
Show me all changes I made
Review for any security issues
```

**Step 5: Commit**
```
Create a commit for the password reset feature
```

**Step 6: Push to remote**
```
Push this branch to GitHub
```

**Step 7: Create PR**
```
Create a pull request for this feature
Target: main branch
Include:
- Summary of functionality
- Testing steps
- Security considerations
```

**Step 8: Address review comments**
```
The PR review asked to add rate limiting to prevent abuse.
Add rate limiting to the reset endpoints.
```

**Step 9: Update PR**
```
Add a commit with the rate limiting changes
Push the update
```

**Step 10: Merge**
```
Merge the pull request
```

**Step 11: Clean up**
```
Delete the feature branch locally and remotely
Switch back to main
Pull the latest changes
```

---

## Hands-On Practice

### Exercise 1: Basic Git Workflow

**Task:** Make a change and commit it properly

**Steps:**
1. Create a test file: "Create a file called test.js with a hello function"
2. Check status: "Show me git status"
3. Review changes: "Show me the diff"
4. Commit: "Create a commit with this change"
5. View history: "Show me the git log"

---

### Exercise 2: Feature Branch Workflow

**Task:** Create a feature on a branch

**Steps:**
1. "Create a new branch called feature/add-tests"
2. "Create a test file for the hello function"
3. "Show me what I've changed"
4. "Commit the test file"
5. "Switch back to main"
6. "Merge the feature branch"

---

### Exercise 3: Pull Request Workflow

**Task:** Create a proper pull request

**Steps:**
1. "Create a branch for adding documentation"
2. "Create a README.md for this project"
3. "Review my changes"
4. "Commit the README"
5. "Push to GitHub"
6. "Create a pull request"

---

## Module 7 Checklist

Before moving to Module 8, make sure you can:

- [ ] Check Git status and view changes
- [ ] Create meaningful commits with Claude Code
- [ ] Work with branches (create, switch, merge)
- [ ] Generate good commit messages
- [ ] Create pull requests with comprehensive descriptions
- [ ] Review code before committing
- [ ] Resolve merge conflicts
- [ ] Use GitHub CLI through Claude Code

---

## Git Safety Tips

### Do's

- **Commit working code** -- make sure it runs before you commit
- **Write clear messages** -- or better yet, let Claude Code write them
- **Review before committing** -- always know what you're shipping
- **Use branches** -- keep your main branch stable
- **Pull before push** -- get the latest changes first to avoid surprises
- **Push regularly** -- back up your work; your laptop isn't immortal

### Don'ts

- **Don't commit broken code** -- test first
- **Don't commit secrets** -- API keys, passwords, database credentials, none of it
- **Don't force push to main** -- this can destroy other people's work
- **Don't skip commit messages** -- future you will be grateful
- **Don't commit large binary files** -- use .gitignore for those
- **Don't rewrite history on shared branches** -- it creates a mess for everyone

---

## Common Questions (FAQ)

### Q: Should I commit after every small change?
**A:** Commit when you have a logical, working unit of functionality. Not after every line, but don't wait until you've built an entire feature either. Think of each commit as one coherent thought.

### Q: How often should I push to GitHub?
**A:** At least daily, and always when you've got working features. More frequent is better -- it's your backup.

### Q: Can Claude Code write commit messages for any project?
**A:** Yes. It analyzes your actual code changes, so it works regardless of the project or language.

### Q: What if I accidentally committed something I shouldn't have?
**A:** Tell Claude Code: "I accidentally committed [file]. Help me remove it from the last commit"

### Q: How do I see old commits?
**A:** "Show me the git log" or "Show me commits from the last week"

---

## Pro Tips

1. **Always review before committing** -- know what you're shipping.

2. **Let Claude Code write your commit messages** -- they'll be more descriptive than what most people write manually. No shame in that.

3. **Make small, focused commits** -- each commit should do one thing. It's easier to understand, easier to review, and easier to revert if something goes wrong.

4. **Branch for every feature** -- even if it feels like overkill for a small change. The habit pays off.

5. **Use descriptive branch names** -- `feature/add-password-reset` tells you a lot more than `my-branch`.

6. **Let Claude Code write PR descriptions** -- it's thorough and fast. You can always edit the result.

7. **Never commit broken code to a shared branch** -- run your tests first.

---

Next up: Module 8 -- debugging and testing with Claude Code, where things get really practical.
