# Challenge Solutions

**Suggested solutions and approaches for module challenges**

---

## How to Use This Guide

Fair warning: spoilers below. These are suggested solutions -- not the only way to do things. Your approach might look completely different and still be perfectly fine. If you're comparing your solution to what's here, the interesting part isn't whether they match, it's understanding why the differences exist.

---

## Module 1 Challenges

### Module 1 - Getting Started

**Challenge:** Successfully install Claude Code and create your first program

**Solution Approach:**

1. **Installation:**
   ```bash
   # Install Claude Code (macOS/Linux)
   curl -fsSL https://claude.ai/install.sh | bash
   # Windows PowerShell: irm https://claude.ai/install.ps1 | iex

   # Log in (opens browser for Pro/Max/Teams subscribers)
   claude

   # Or set API key for API-based usage
   export ANTHROPIC_API_KEY="your-key-here"

   # Verify installation
   claude --version
   ```

2. **First Program:**
   ```bash
   # Create project directory
   mkdir my-first-project
   cd my-first-project

   # Start Claude Code
   claude
   ```

   Then prompt:
   ```text
   Create a Python script called greeting.py that:
   - Asks for the user's name
   - Asks for their favorite color
   - Prints a personalized greeting with their color
   ```

3. **Test it:**
   ```text
   Run the greeting.py script
   ```

**What you'll get out of this:**
- Installing and configuring Claude Code
- Basic prompt structure
- Seeing tool usage in action (Write, Bash)

---

## Module 2 Challenges

### Module 2 Challenge 1: Build a Note-Taking API (Beginner)

**Challenge:** Create a REST API for notes using Express.js

**Solution Approach:**

**Step 1: Initial Prompt**
```text
Create a REST API for notes using Express.js with these requirements:
- In-memory storage (array of notes)
- Each note has: id, title, content, createdAt
- Endpoints: GET /notes, GET /notes/:id, POST /notes, PUT /notes/:id, DELETE /notes/:id
- Input validation for title and content
- Proper error handling and status codes
```

**Step 2: Project Structure**
Claude Code should create something like this:
```text
notes-api/
├── package.json
├── server.js
├── routes/
│   └── notes.js
├── middleware/
│   └── validation.js
└── README.md
```

**Step 3: Testing**
```text
Install dependencies and start the server
```

Then test with:
```bash
# Create a note
curl -X POST http://localhost:3000/notes \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","content":"Hello"}'

# Get all notes
curl http://localhost:3000/notes
```

**Another way to do it:**
Instead of one big prompt, you could build it up piece by piece -- start with the basic server, add endpoints one at a time, then layer in validation and error handling at the end. That's actually a great habit to develop.

**What you'll get out of this:**
- RESTful API design
- Express.js routing
- Input validation
- Error handling

---

### Module 2 Challenge 2: Full-Stack App with Next.js (Intermediate)

**Challenge:** Create a contact form with Next.js, validation, and API routes

**Solution Approach:**

**Step 1: Initialize Project**
```text
Create a new Next.js project with TypeScript and Tailwind CSS.
Name it contact-form-app.
```

**Step 2: Build UI**
```text
Create a contact form page at /contact with:
- Name field (required, min 2 characters)
- Email field (required, valid email)
- Message field (required, min 10 characters)
- Submit button
- Loading state during submission
- Success and error message display
- Use Tailwind for styling
```

**Step 3: Add API Route**
```text
Create an API route at /api/contact that:
- Validates the input
- Returns 400 for invalid data
- Returns 200 with success message for valid data
- For now, just log the data (we'll add email later)
```

**Step 4: Connect Form to API**
```text
Update the contact form to:
- Call the /api/contact endpoint on submit
- Show loading spinner during request
- Display success message on 200 response
- Display error message on error
- Clear form on success
```

**Expected File Structure:**
```text
contact-form-app/
├── pages/
│   ├── contact.tsx
│   └── api/
│       └── contact.ts
├── components/
│   └── ContactForm.tsx
├── styles/
│   └── globals.css
└── package.json
```

**Testing:**
```text
Start the development server
Open http://localhost:3000/contact
Test all validation cases
Test successful submission
```

**What you'll get out of this:**
- Next.js page and API routes
- Form validation on both client and server
- State management with hooks
- Error handling across the stack

---

### Module 2 Challenge 3: Clone & Contribute (Advanced)

**Challenge:** Contribute to a real open source project

**Solution Approach:**

**Step 1: Find a Project**
```bash
# Good beginner-friendly projects:
# - first-contributions
# - awesome lists
# - documentation projects
```

Example:
```text
Clone https://github.com/firstcontributions/first-contributions
and help me understand the project structure
```

**Step 2: Set Up**
```text
Help me:
1. Fork this repository to my GitHub
2. Clone my fork locally
3. Set up the upstream remote
4. Create a new branch for my contribution
```

**Step 3: Make Changes**
```text
I want to add my name to the Contributors.md file.
Help me:
1. Find the correct format
2. Add my entry alphabetically
3. Test that it follows the project standards
```

**Step 4: Commit and Push**
```text
Create a commit following this project's commit message conventions
Push to my fork
```

**Step 5: Create PR**
```text
Help me create a pull request with:
- A clear title
- Description of what I added
- Reference to any related issues
```

**What you'll get out of this:**
- Git workflow (fork, clone, branch)
- Following contribution guidelines
- Code review process
- Open source collaboration

---

## Module 3 Challenges

### Module 3 Challenge 1: Tool Mastery (Beginner)

**Challenge:** Use each tool correctly for specific tasks

**Solution Prompts:**

1. **Read Tool:**
   ```text
   Read the package.json file and tell me what dependencies are installed
   ```

2. **Write Tool:**
   ```text
   Create a new file called config.js that exports database configuration
   ```

3. **Edit Tool:**
   ```text
   In server.js, change the port from 3000 to 8080
   ```

4. **Glob Tool:**
   ```text
   Find all JavaScript files in the src directory
   ```

5. **Grep Tool:**
   ```text
   Search for all TODO comments in the codebase
   ```

6. **Bash Tool:**
   ```text
   Install the express package
   ```

**What you'll get out of this:**
- What each tool actually does
- Knowing when to reach for which tool
- Being specific in your requests

---

## Module 4 Challenges

### Module 4 Challenge 1: Prompt Engineering Mastery (Beginner)

**Challenge:** Write effective prompts for different scenarios

**Solution Approach:**

**Scenario 1: Bug Fix**
```text
I'm getting a TypeError in userService.js at line 45.
The error says "Cannot read property 'email' of undefined".
This happens when a user tries to log in with an email that doesn't exist in the database.
Please read the file, identify the issue, and fix it with proper null checking.
```

**Why this works:** It specifies the file, error, context, and desired outcome. Compare with the vague version: "fix the bug in my app."

**Scenario 2: New Feature**
```text
Add pagination to the GET /api/users endpoint:
- Accept `page` and `limit` query parameters
- Default to page 1, limit 20
- Return total count in response headers
- Return 400 if page or limit are invalid
```

**Why this works:** Clear requirements, specific parameters, defined edge cases.

**Scenario 3: Refactoring**
```text
Refactor the authentication middleware in middleware/auth.js:
- Extract token validation into a separate function
- Add proper error messages for expired vs invalid tokens
- Keep the existing behavior -- all current tests should still pass
```

**Why this works:** States what to change, how to change it, and the constraint (don't break existing tests).

**What you'll get out of this:**
- Structuring prompts with context, requirements, and constraints
- Understanding why specificity matters
- Seeing how prompt quality directly affects output quality

---

### Module 4 Challenge 2: Multi-Step Conversation (Intermediate)

**Challenge:** Build a feature through iterative conversation

**Solution Approach:**

**Step 1:** Start with the skeleton
```text
Create a basic Express middleware for rate limiting.
Just the structure -- no implementation yet.
```

**Step 2:** Add core logic
```text
Now implement the rate limiting logic:
- Track requests by IP address
- Allow 100 requests per 15-minute window
- Return 429 with retry-after header when exceeded
```

**Step 3:** Add persistence
```text
The rate limiter currently uses in-memory storage.
Add Redis support so it works across multiple server instances.
Keep the in-memory version as a fallback.
```

**Step 4:** Add tests
```text
Write tests for the rate limiter covering:
- Normal requests within limit
- Requests exceeding limit
- Window reset behavior
- Redis connection failure fallback
```

**What you'll get out of this:**
- Building incrementally through conversation
- Each step builds on the last
- Natural progression from simple to complex

---

## Module 5 Challenges

### Module 5 Challenge 1: CLAUDE.md Mastery (Beginner)

**Challenge:** Create an effective CLAUDE.md for a project

**Solution Approach:**

```text
Help me create a CLAUDE.md file for this project that includes:
- Project overview and architecture
- Key commands for building, testing, and linting
- Code style preferences
- Important patterns to follow
- Common pitfalls to avoid
```

**Example CLAUDE.md:**
```markdown
# Project: Task Manager API

## Quick Commands
- `npm run dev` — Start development server
- `npm test` — Run test suite
- `npm run lint` — Check code style

## Architecture
- Express.js REST API with PostgreSQL
- Routes in routes/, business logic in services/, data access in repositories/
- All async functions use try/catch with custom error classes

## Code Style
- Use async/await, never raw promises
- All functions must have JSDoc comments
- Error responses follow { error: { status, message, details } } format
- Use named exports, not default exports

## Patterns
- Repository pattern for database access
- Middleware for validation (see middleware/validate.js for examples)
- All dates stored as UTC, converted to local time only in responses

## Don't
- Don't use console.log for logging -- use the logger utility
- Don't write raw SQL -- use the query builder
- Don't skip input validation on any endpoint
```

**What you'll get out of this:**
- Understanding what belongs in CLAUDE.md
- Making Claude Code follow your project's conventions
- Saving time by not repeating instructions every session

---

### Module 5 Challenge 2: Memory and Context (Intermediate)

**Challenge:** Use memory features to maintain context across sessions

**Solution Approach:**

**Step 1:** Set up project memory
```text
Save to project memory: This project uses Express.js with TypeScript,
PostgreSQL for the database, and Jest for testing.
The main entry point is src/index.ts.
```

**Step 2:** Add coding conventions
```text
Save to project memory: Code conventions --
- All API responses use the ResponseWrapper class
- Database queries go through the repository layer
- Use zod for request validation schemas
```

**Step 3:** Verify it works
```text
What do you know about this project's conventions?
```

Claude Code should recall the saved context without you repeating it.

**What you'll get out of this:**
- Persistent project context across sessions
- Not repeating yourself every time you start a new conversation
- Building up institutional knowledge

---

## Module 6 Challenges

### Module 6 Challenge 1: Agent Exploration (Beginner)

**Challenge:** Use the Explore agent to understand a codebase

**Solution Approach:**

Pick any open-source project and try:
```text
Use the Explore agent with medium thoroughness to:
1. Explain the project architecture
2. Identify the main entry points
3. Map out the data flow for the primary feature
4. List all external dependencies and what they're used for
```

Then go deeper on something specific:
```text
The Explore agent mentioned [component]. Use it again to trace
exactly how [component] handles errors -- from where the error
occurs to where it's reported to the user.
```

**What you'll get out of this:**
- Navigating unfamiliar codebases quickly
- Knowing when to use quick vs thorough exploration
- Following up on agent findings

---

### Module 6 Challenge 2: Parallel Agent Research (Intermediate)

**Challenge:** Research three architectural approaches simultaneously

**Solution Approach:**

```text
I need to choose a database for a new project.
Run these agents in parallel:

Agent 1: Research PostgreSQL -- setup complexity, performance characteristics,
          best use cases, scaling story
Agent 2: Research MongoDB -- same criteria
Agent 3: Research SQLite -- same criteria

For each, include real-world examples of when it's the right choice.
```

After results come back:
```text
Based on the research, I'm building a multi-tenant SaaS app
with complex queries and strict data integrity requirements.
Which database fits best and why?
```

**What you'll get out of this:**
- Running multiple agents simultaneously
- Comparing results from parallel research
- Making informed decisions from agent output

---

## Module 7 Challenges

### Module 7 Challenge 1: Git Workflow (Beginner)

**Challenge:** Complete a full git workflow through Claude Code

**Solution Approach:**

```text
Help me with a complete git workflow:
1. Create a new branch called feature/add-search
2. Make changes (add a search endpoint to the API)
3. Stage the changes
4. Create a commit with a descriptive message
5. Push the branch
6. Create a pull request
```

**What you'll get out of this:**
- Full git workflow without leaving Claude Code
- Proper branch naming and commit messages
- PR creation with meaningful descriptions

---

### Module 7 Challenge 2: Handling Merge Conflicts (Intermediate)

**Challenge:** Resolve a merge conflict using Claude Code

**Solution Approach:**

**Step 1:** Create a conflict scenario
```text
Create two branches that modify the same file differently,
then try to merge them so we get a conflict.
```

**Step 2:** Resolve it
```text
I have a merge conflict in userService.js.
Show me what's conflicting and help me resolve it by:
- Keeping the new validation logic from feature-branch
- Keeping the error handling improvements from main
- Making sure both changes work together
```

**What you'll get out of this:**
- Understanding merge conflicts
- Using Claude Code to see both sides
- Making informed resolution decisions

---

## Module 8 Challenges

### Module 8 Challenge 1: Debug a Multi-Bug Module (Beginner)

**Challenge:** Find and fix multiple bugs in a single file

**Solution Approach:**

```text
Create a user authentication module with these 5 intentional bugs:
1. Password comparison using == instead of a proper hash check
2. Missing await on an async database call
3. JWT token never expires
4. Error message reveals whether email or password was wrong
5. No rate limiting on login attempts

Then walk me through finding and fixing each one.
```

**What you'll get out of this:**
- Systematic debugging -- finding multiple issues
- Security-aware debugging
- Understanding why each bug matters

---

### Module 8 Challenge 2: TDD a Complete Feature (Intermediate)

**Challenge:** Build a feature entirely test-first

**Solution Approach:**

```text
Using TDD, build a URL shortener module:

Round 1 -- Write tests for: createShortUrl(longUrl) returns a short code
Round 2 -- Write tests for: resolveShortUrl(code) returns original URL
Round 3 -- Write tests for: getUrlStats(code) returns click count
Round 4 -- Write tests for: edge cases (invalid URLs, expired links, duplicates)

For each round:
1. Write the tests first
2. Run them (they should fail)
3. Write minimum code to pass
4. Refactor if needed
5. Move to next round
```

**What you'll get out of this:**
- Full TDD workflow from start to finish
- Building confidence that tests catch real issues
- Incremental feature development

---

## Module 9 Challenges

### Module 9 Challenge 1: Build a Complete Project (Beginner)

**Challenge:** Build a task manager from start to finish

**Solution Approach:**

Break it into sessions:

**Session 1: Foundation**
```text
Create a task manager API with:
- Express.js with TypeScript
- SQLite database
- Basic CRUD endpoints for tasks
- Each task has: id, title, description, status, priority, createdAt, updatedAt
```

**Session 2: Features**
```text
Add to the task manager:
- Filter tasks by status and priority
- Sort by any field
- Pagination
- Search by title/description
```

**Session 3: Polish**
```text
Add production touches:
- Input validation with meaningful errors
- Request logging
- Error handling middleware
- API documentation
- Tests for all endpoints
```

**What you'll get out of this:**
- End-to-end project development
- Breaking a real project into manageable pieces
- Applying everything from modules 1-8

---

## Module 10 Challenges

### Module 10 Challenge 1: Professional Workflow (Beginner)

**Challenge:** Apply professional development practices to an existing project

**Solution Approach:**

```text
Review this project and help me bring it up to professional standards:

1. Add a proper .gitignore
2. Set up ESLint with recommended rules
3. Add Prettier for code formatting
4. Create a pre-commit hook that runs lint and format
5. Add a CI-ready test script
6. Create a meaningful README
7. Add environment variable validation on startup
```

**What you'll get out of this:**
- Setting up professional tooling
- Automated code quality checks
- Project documentation standards

---

### Module 10 Challenge 2: Security Audit and Fix (Intermediate)

**Challenge:** Find and fix security vulnerabilities

**Solution Approach:**

```text
Perform a security audit on this Express.js application:

1. Check for SQL injection vulnerabilities
2. Look for XSS risks in any rendered output
3. Verify authentication is implemented correctly
4. Check for exposed secrets or sensitive data
5. Review CORS configuration
6. Check dependency vulnerabilities with npm audit

For each issue found:
- Explain the risk
- Show the vulnerable code
- Implement the fix
- Add a test that verifies the fix
```

**What you'll get out of this:**
- Thinking about security proactively
- Understanding common vulnerability patterns
- Fixing real security issues

---

## Module 11 Challenges

### Module 11 Challenge 1: Set Up MCP Server (Beginner)

**Challenge:** Connect Claude Code to an external data source via MCP

**Solution Approach:**

```text
Help me set up an MCP server that connects Claude Code to a SQLite database.
I want to be able to:
- Query tables using natural language
- Insert and update records
- See the database schema
```

**Step 1:** Install and configure the SQLite MCP server
**Step 2:** Test it by asking Claude Code to query data
**Step 3:** Try more complex queries -- joins, aggregations, filters

**What you'll get out of this:**
- Understanding MCP server architecture
- Connecting Claude Code to external tools
- Querying data through natural language

---

## Module 12 Challenges

### Module 12 Challenge 1: Create a Custom Skill (Beginner)

**Challenge:** Build a skill that automates a repetitive task

**Solution Approach:**

Create a skill that generates boilerplate for a new API endpoint:

```text
Create a skill at .claude/skills/new-endpoint/SKILL.md that:
- Takes an entity name as input
- Generates route file, controller, service, and test
- Follows our project's existing patterns
- Adds the route to the main router
```

**Example skill file:**
```markdown
---
description: Generate boilerplate for a new API endpoint
---

Given an entity name, create:
1. routes/{entity}.routes.js with CRUD endpoints
2. controllers/{entity}.controller.js with request handlers
3. services/{entity}.service.js with business logic
4. tests/{entity}.test.js with basic test cases

Follow the patterns in the existing user module for consistency.
```

**What you'll get out of this:**
- Automating repetitive project setup
- Encoding project patterns into reusable skills
- Saving time on boilerplate

---

### Module 12 Challenge 2: Set Up Hooks (Intermediate)

**Challenge:** Add lifecycle hooks to your workflow

**Solution Approach:**

```text
Set up these hooks in .claude/settings.json:

1. PostToolUse hook on Edit|Write: Auto-lint after file changes
2. Notification hook: Alert when a permission prompt appears
3. Stop hook: Run cleanup when session ends
```

**Example configuration:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{
          "type": "command",
          "command": "npm run lint --quiet 2>/dev/null || true"
        }]
      }
    ],
    "Notification": [
      {
        "matcher": "permission_prompt",
        "hooks": [{
          "type": "command",
          "command": "echo 'Permission requested' >> ~/.claude/hook-log.txt"
        }]
      }
    ]
  }
}
```

**What you'll get out of this:**
- Automating quality checks
- Automating post-edit quality checks
- Custom workflow automation

---

## Module 13 Challenges

### Module 13 Challenge 1: Multi-Language Project (Beginner)

**Challenge:** Use Claude Code across different languages in one project

**Solution Approach:**

```text
Create a project with:
- A Python FastAPI backend
- A TypeScript React frontend
- A shared types definition

Help me:
1. Set up the project structure
2. Create the API with 3 endpoints
3. Create the React frontend that calls the API
4. Generate TypeScript types from the Python schemas
5. Add tests for both backend and frontend
```

**What you'll get out of this:**
- Working across language boundaries
- Keeping shared types in sync
- Testing multi-language projects

---

## Module 14 Challenges

### Module 14 Challenge 1: External API Integration (Beginner)

**Challenge:** Integrate with a third-party API

**Solution Approach:**

```text
Integrate the OpenWeatherMap API into our Express app:
1. Create a weather service that fetches current weather
2. Add error handling for API failures
3. Implement caching to avoid hitting rate limits
4. Add a fallback for when the API is down
5. Write tests using mocked API responses
```

**What you'll get out of this:**
- Working with external APIs
- Error handling and resilience
- Caching strategies
- Mocking external services in tests

---

## Module 15 Challenges

### Module 15 Challenge 1: Deploy an Application (Beginner)

**Challenge:** Take a project from local to deployed

**Solution Approach:**

```text
Help me deploy this Express.js app:
1. Add a Dockerfile
2. Create docker-compose.yml for local testing
3. Set up environment variable management
4. Add health check endpoint
5. Create a basic CI/CD pipeline (GitHub Actions)
6. Configure for production (compression, security headers, logging)
```

**What you'll get out of this:**
- Containerizing applications
- CI/CD pipeline basics
- Production configuration
- Deployment readiness

---

## Tips for Challenges

### General Approach

1. **Read the challenge carefully** -- make sure you understand all the requirements and note any specific technologies mentioned.

2. **Break it down** into steps and tackle them one at a time. Don't try to hold the whole thing in your head.

3. **Start simple.** Get the basic version working first, then add complexity.

4. **Test as you go.** It's way easier to catch problems early than to debug a whole finished project that doesn't work.

5. **Ask for help when you're stuck.** Use Claude Code to explain concepts or suggest different approaches -- that's what it's there for.

### Prompting Tips

**For bigger challenges:**
```text
I need to build [X]. Let's break it down:
1. First, create the basic structure
2. Then add [feature 1]
3. Then add [feature 2]
Let's start with step 1.
```

**When you're stuck:**
```text
I'm trying to [goal] but [problem].
Can you help me understand what's wrong and suggest a fix?
```

**When you want to actually learn what happened:**
```text
Please explain what this code does and why you chose this approach
```

### Evaluation Criteria

When comparing your solution to these, here's what to look at:

**Functionality** -- Does it work? Does it handle edge cases? Is there error handling?

**Code Quality** -- Is it readable and well-organized? Are names descriptive? Is it properly commented?

**Best Practices** -- Does it follow language conventions? Use appropriate patterns? Address security considerations?

**Understanding** -- Do you actually get how it works? Could you explain it to someone else? Could you modify it if you needed to?

---

## Common Pitfalls

### Pitfall 1: Too Vague
"Make an API" won't get you far. "Create a REST API with Express that has endpoints for CRUD operations on users" -- that's something Claude Code can work with.

### Pitfall 2: Too Much at Once
Asking for an entire application in one prompt usually doesn't go well. Build incrementally, feature by feature.

### Pitfall 3: Not Testing
Building everything and then testing at the end is a recipe for confusion. Test after each major change.

### Pitfall 4: Ignoring Errors
If something breaks, stop and fix it. Don't keep building on top of broken code.

### Pitfall 5: Guessing Instead of Asking
If you don't know how a technology works, ask Claude Code to explain it or look up the docs. Guessing wastes time.

---

## Additional Practice Ideas

If you want more to work on beyond the official challenges, here are some ideas:

### Beginner Projects
- Calculator CLI
- Todo list (command line)
- File organizer script
- Simple HTTP server
- Unit converter

### Intermediate Projects
- Blog API with database
- Authentication system
- File upload service
- Real-time chat (WebSocket)
- Task queue system

### Advanced Projects
- Full-stack application
- Microservices architecture
- CI/CD pipeline
- Monitoring dashboard
- Package/library development

---

## Getting the Most from Challenges

These challenges are ordered intentionally -- they build on each other, so it's worth doing them in sequence. Once you've finished a challenge, try tweaking it or adding features to explore further. And if you come back to an early challenge after finishing later modules, you'll probably spot things you'd do differently now. That's a good sign.
