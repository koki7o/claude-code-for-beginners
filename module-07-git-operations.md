<div align="center">

![Module 7](https://img.shields.io/badge/Module_7-0066FF?style=for-the-badge&labelColor=1a1a2e)
![Time](https://img.shields.io/badge/⏱_40_min-555555?style=for-the-badge&labelColor=1a1a2e)
![Difficulty](https://img.shields.io/badge/Intermediate-0066FF?style=for-the-badge&labelColor=1a1a2e)

# Git Operations and Version Control

**Master Git workflows with Claude Code**

[← Previous](module-06-background-agents.md) · [🏠 Home](README.md) · [Next →](module-08-debugging-and-testing.md)

</div>

---

## What You'll Learn

Git through Claude Code - from basic commits and branches all the way to pull requests and merge conflicts. If you've ever pushed a commit that just says "fixed stuff," this one's for you.

---

## Lesson 1: Why Version Control Matters

### What is Version Control?

A system that tracks changes to your code over time. Why it's worth the trouble:

- **Unlimited undo** -- go back to any previous version of your code
- **Collaboration** -- work with other people without stepping on each other's changes
- **History** -- see what changed, when, and why
- **Safety net** -- experiment freely, knowing you can always roll back

### Git Basics

A few core ideas to have straight before we go further.

**Repository (Repo)** -- a project tracked by Git. It holds all your files and their entire change history. Repos live on your machine (local) or on a server like GitHub or GitLab (remote).

**Commit** -- a snapshot of your code at one point in time. Each commit carries a message describing what changed, and together they form a permanent record of the project's life.

**Branch** -- a parallel version of your code. Branches let you build a feature in isolation without touching the main codebase. When it's ready, you merge it back in.

**Remote** -- a copy of your repo on a server, usually called "origin." It's how you back up your work and collaborate.

---

## Lesson 2: Basic Git Operations with Claude Code

### Checking Status

To see what's changed:
```text
Show me git status
What files have I modified?
```

Claude Code will run:
```bash
git status
```

The output tells you what's been modified, what's new (untracked), what's deleted, and what's already staged for a commit.

---

### Viewing Changes

To see the actual code differences:
```text
Show me the git diff
What changes have I made to auth.js?
```

Claude Code shows lines added (green, with +), lines removed (red, with -), and enough surrounding context to orient you.

**Example output:**
```diff
- const PORT = 3000;
+ const PORT = 8080;
```

---

### Creating Commits with Claude Code

This is one of the spots where Claude Code buys you back the most time.

**The old way:**
```bash
git add .
git commit -m "Updated the thing"  # Vague, unhelpful
```

**With Claude Code:**
```text
Please create a commit with my current changes
```

Ask for a commit and Claude Code runs `git status` and `git diff` to see what actually changed, reads the diff, writes a message that describes it, and creates the commit cleanly formatted.

**Example commit message Claude Code might write:**
```text
Add user authentication with JWT tokens

- Implement login and register endpoints
- Add bcrypt for password hashing
- Create JWT middleware for protected routes
- Add user model and database schema
- Include input validation for auth requests

This provides secure user authentication for the API.
```

Beats "fixed stuff" by a mile.

---

### Commit Message Best Practices

Claude Code follows the standard conventions on its own, but it helps to know what good looks like:

**Structure:**
```text
Brief summary (50 chars or less)

Longer description if needed:
- What changed
- Why it changed
- Any important details

Issue references: #123
```

**Good vs Bad:**

Bad:
```text
- "fixed bug"
- "updated code"
- "changes"
- "asdfasdf"
```

Good:
```text
- "Fix login error when password is empty"
- "Add email validation to user registration"
- "Refactor database connection for better error handling"
- "Update API endpoints to use async/await"
```

---

## Lesson 3: Working with Branches

### Why Use Branches?

Branches let you build features without breaking main, run experiments safely, collaborate without collisions, and keep different kinds of work apart. Once you start branching properly, you won't go back.

### Creating Branches with Claude Code

**Create a new branch:**
```text
Create a new branch called feature/user-dashboard
```

Claude Code will:
```bash
git checkout -b feature/user-dashboard
```

Or describe it and let Claude Code name it sensibly:
```text
Create a feature branch for adding password reset
```

Claude Code might create:
```bash
git checkout -b feature/password-reset
```

### Branch Naming Conventions

**Common patterns:**
```text
feature/feature-name    # New features
bugfix/bug-description  # Bug fixes
hotfix/critical-fix     # Urgent fixes
refactor/what-refactor  # Code improvements
docs/what-documentation # Documentation
```

**Examples:**
```text
feature/user-authentication
bugfix/login-validation-error
hotfix/security-vulnerability
refactor/database-queries
docs/api-endpoints
```

### Switching Branches

**Switch to existing branch:**
```text
Switch to the main branch
```

```bash
git checkout main
```

**Or:**
```text
Switch back to my feature branch
```

---

### Merging Branches

**Merge feature into main:**
```text
Merge the feature/user-dashboard branch into main
```

Claude Code switches to main, pulls the latest, merges your feature branch, and handles any conflicts - which is exactly what a later lesson covers.

---

## Lesson 4: Creating Pull Requests

### What is a Pull Request (PR)?

A pull request is how you propose merging your code into another branch. It's a request for review, a place to discuss the change, and a quality gate before anything hits production.

### Creating PRs with Claude Code

Writing a good PR description is tedious work, and it's one Claude Code does genuinely well.

**Simple request:**
```text
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

That's 20 minutes of writing by hand. Claude Code does it in seconds.

---

### Customizing PR Creation

Be specific about what you want:

```text
Create a pull request with:
- Title: "Add user authentication"
- Target branch: develop (not main)
- Include screenshots from /screenshots folder
- Mark as draft
```

---

## Lesson 5: Code Review Before Committing

### Why Review First?

Reviewing your own diff before committing catches mistakes early, keeps you honest about exactly what you're shipping, and spares you the occasional embarrassing "oops" commit.

### Review Workflow with Claude Code

**Step 1: See what changed**
```text
Show me all the changes I'm about to commit
```

**Step 2: Review specific files**
```text
Show me the diff for auth.js
Explain what changed in the database schema
```

**Step 3: Check for issues**
```text
Review my changes and check for:
- Security vulnerabilities
- Code quality issues
- Missing error handling
- Inconsistent style
```

**Step 4: Make fixes if needed**
```text
Fix the security issue you found in login.js
```

**Step 5: Commit when ready**
```text
Now create a commit with these changes
```

---

## Lesson 6: Resolving Merge Conflicts

### What are Merge Conflicts?

A conflict happens when two branches change the same line and Git can't tell which version wins. You have to decide. It feels scary the first few times - Claude Code takes most of the sting out of it.

**Example conflict:**
```javascript
<<<<<<< HEAD
const PORT = 3000;
=======
const PORT = 8080;
>>>>>>> feature/new-port
```

### Resolving Conflicts with Claude Code

When you hit one:
```text
I have a merge conflict in server.js. Help me resolve it.
```

Claude Code shows you the conflict, explains both sides, asks which to keep - or suggests combining them - then resolves it and marks the file done.

**Example dialogue:**
```text
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

`gh` is GitHub's official command-line tool, and Claude Code can drive it directly. Pull requests, issues, CI/CD status, releases - all without leaving your terminal.

### Common gh Operations

**View PRs:**
```text
Show me all open pull requests
```

**Create issue:**
```text
Create a GitHub issue for the bug I just found:
Title: Login fails with empty password
Description: [your description]
```

**Check PR status:**
```text
What's the status of PR #42?
Has it passed all checks?
```

**Merge PR:**
```text
Merge pull request #42
```

---

## Lesson 8: Complete Git Workflow Example

### Scenario: Adding a New Feature

The full lifecycle of a feature, branch to merge. This is the loop you'll run over and over.

**Step 1: Create feature branch**
```text
Create a new branch for adding password reset feature
```

**Step 2: Make changes**
```text
Implement password reset functionality with:
- Request reset endpoint
- Reset token generation
- Password update endpoint
- Email sending (simulated)
```

**Step 3: Test your changes**
```text
Run the tests
```

**Step 4: Review changes**
```text
Show me all changes I made
Review for any security issues
```

**Step 5: Commit**
```text
Create a commit for the password reset feature
```

**Step 6: Push to remote**
```text
Push this branch to GitHub
```

**Step 7: Create PR**
```text
Create a pull request for this feature
Target: main branch
Include:
- Summary of functionality
- Testing steps
- Security considerations
```

**Step 8: Address review comments**
```text
The PR review asked to add rate limiting to prevent abuse.
Add rate limiting to the reset endpoints.
```

**Step 9: Update PR**
```text
Add a commit with the rate limiting changes
Push the update
```

**Step 10: Merge**
```text
Merge the pull request
```

**Step 11: Clean up**
```text
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
- **Pull before push** -- grab the latest first to avoid surprises
- **Push regularly** -- back up your work; your laptop isn't immortal

### Don'ts

- **Don't commit broken code** -- test first
- **Don't commit secrets** -- API keys, passwords, database credentials, none of it
- **Don't force push to main** -- you can wipe out other people's work
- **Don't skip commit messages** -- future you will thank present you
- **Don't commit large binary files** -- that's what .gitignore is for
- **Don't rewrite history on shared branches** -- it's a mess for everyone

---

## Common Questions (FAQ)

### Q: Should I commit after every small change?
**A:** Commit when you have a logical, working unit. Not after every line, but don't hoard a whole feature into one commit either. Think of each commit as a single coherent thought.

### Q: How often should I push to GitHub?
**A:** At least daily, and always when you've got working features. More often is better - it's your backup.

### Q: Can Claude Code write commit messages for any project?
**A:** Yes. It reads your actual diff, so it works regardless of project or language.

### Q: What if I accidentally committed something I shouldn't have?
**A:** Tell Claude Code: "I accidentally committed [file]. Help me remove it from the last commit"

### Q: How do I see old commits?
**A:** "Show me the git log" or "Show me commits from the last week"

---

## Pro Tips

1. **Always review before committing** -- know what you're shipping.

2. **Let Claude Code write your commit messages** -- they'll be more descriptive than what most people type by hand. No shame in that.

3. **Make small, focused commits** -- one thing per commit. Easier to read, easier to review, easier to revert when something goes sideways.

4. **Branch for every feature** -- even when it feels like overkill for a small change. The habit pays.

5. **Use descriptive branch names** -- `feature/add-password-reset` says a lot more than `my-branch`.

6. **Let Claude Code write PR descriptions** -- thorough and fast, and you can always edit the result.

7. **Never commit broken code to a shared branch** -- run the tests first.

8. **Automate quality checks with hooks** -- You can set up a hook that auto-lints your code every time Claude Code edits a file. Add this to `.claude/settings.json`:
   ```json
   {
     "hooks": {
       "PostToolUse": [{
         "matcher": "Edit|Write",
         "hooks": [{ "type": "command", "command": "npm run lint --quiet 2>/dev/null || true" }]
       }]
     }
   }
   ```
   Catches lint issues the moment they happen instead of at commit time. Full hooks coverage is in Module 12.

---

> **Level up your git workflow.** The [Advanced Modules](https://payhip.com/b/8E107) cover enterprise git patterns including automated PR pipelines, git worktree isolation, and monorepo strategies (Module 23).

Next up: Module 8 -- debugging and testing with Claude Code, where things get really practical.

---

<div align="center">

[← Previous Module](module-06-background-agents.md) · [🏠 Home](README.md) · [Next Module →](module-08-debugging-and-testing.md)

</div>
