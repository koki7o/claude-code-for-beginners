<div align="center">

![Module 6](https://img.shields.io/badge/Module_6-0066FF?style=for-the-badge&labelColor=1a1a2e)
![Time](https://img.shields.io/badge/⏱_35_min-555555?style=for-the-badge&labelColor=1a1a2e)
![Difficulty](https://img.shields.io/badge/Intermediate-0066FF?style=for-the-badge&labelColor=1a1a2e)

# Using Background Agents

**Learn to use specialized agents for complex tasks**

[← Previous](module-05-custom-instructions.md) · [🏠 Home](README.md) · [Next →](module-07-git-operations.md)

</div>

---

## What You'll Learn

This module covers agentic tasks -- prompts where you delegate complex, multi-step work to Claude Code and let it operate autonomously. You'll learn when to reach for this approach, how to write effective exploration and planning prompts, and how to run multiple Claude Code sessions in parallel when you've got a lot of ground to cover.

---

## Lesson 1: What Are Agentic Tasks?

### Understanding Agentic Tasks

Claude Code can work autonomously to complete complex tasks. You give it a goal, it figures out the steps, and it reports back when it's done. There aren't separate "agents" to select -- you just write a prompt that delegates the work, and Claude Code handles the rest.

Here's how agentic delegation differs from regular Claude Code conversation:

**Regular Claude Code:**
- You guide each step
- Interactive back-and-forth
- You make decisions

**Agentic Delegation:**
- Works autonomously
- Makes its own decisions
- Completes multi-step tasks independently
- Reports back when done

The short version: regular mode is a conversation, agentic mode is delegation.

---

### When to Delegate

Agentic prompts shine when the task involves multiple steps, exploration, or decision-making. Think: exploring an unfamiliar codebase, planning a feature implementation, doing research that spans many files, or tackling anything where you'd otherwise be saying "now do this... okay now do that..." over and over.

Stick with regular conversation for simple, single-step tasks, quick modifications, or situations where you actually want to see each tool call happen -- like when you're learning how Claude Code works under the hood.

---

## Lesson 2: Exploration Tasks

### When to Use Exploration Prompts

Exploration prompts are your go-to when you need to figure out how a project works, find where something is implemented, or get a feel for the architecture. Claude Code reads files, traces connections, and comes back with a coherent explanation instead of making you piece it together yourself.

---

### Writing Exploration Prompts

**Basic usage:**
```text
Explore this codebase and explain how authentication works
```

When you send that, Claude Code searches for auth-related files, reads the relevant code, analyzes the implementation, traces the flow, and returns a comprehensive explanation. All without you lifting a finger.

---

### Exploration Examples

**Example 1: Understanding a Feature**
```text
Explore how the payment processing works in this codebase.
I need to know:
- What payment providers are supported
- How transactions are stored
- What security measures are in place
- The complete flow from checkout to confirmation
```

Claude Code finds all payment-related files, reads configuration, traces the payment flow, analyzes security implementations, and gives you a detailed report.

---

**Example 2: Finding API Endpoints**
```text
Explore the codebase and list all API endpoints with:
- HTTP method
- Route path
- What each endpoint does
- Required parameters
```

You'll get back something like:
```text
API Endpoints Found:

POST /api/auth/login
- Authenticates user with email/password
- Requires: email, password
- Returns: JWT token

GET /api/users
- Lists all users
- Requires: Admin authentication
- Returns: Array of user objects

POST /api/users
- Creates new user
- Requires: name, email, password
- Returns: Created user object

... (and so on)
```

---

**Example 3: Understanding Architecture**
```text
Analyze this codebase and explain:
- Overall architecture pattern (MVC, layered, etc.)
- How different parts communicate
- Database structure
- External dependencies
```

---

### Controlling Depth Through Your Prompt

The depth of Claude Code's analysis depends on how you phrase your request. There aren't formal "thoroughness settings" -- it's about how specific and demanding your prompt is.

**Quick pass -- ask for an overview:**
```text
Give me a high-level overview of this project
```

**Moderate depth -- ask about a specific system:**
```text
Walk me through how the authentication system works in this codebase
```

**Deep analysis -- ask for exhaustive detail:**
```text
Do a thorough analysis of the entire API. Document every endpoint, trace each request through the full stack, and flag any inconsistencies or missing error handling.
```

---

## Lesson 3: Planning Tasks

### When to Ask Claude Code to Plan

Asking Claude Code to plan gets you implementation strategies before you write a single line of code. Use planning prompts for new features, system architecture, complex changes, and catching problems before they become expensive.

Trust me on this -- spending five minutes asking Claude Code to plan before a big change can save you hours of backtracking.

---

### Writing Planning Prompts

**Basic usage:**
```text
Create an implementation plan for adding user roles and permissions to this project
```

Claude Code analyzes your current codebase, identifies what needs to change, plans the steps, considers edge cases, and returns a detailed plan you can follow -- or push back on.

---

### Planning Examples

**Example 1: Adding a Feature**
```text
I want to add email notifications when users receive messages.

Create an implementation plan including:
- Database changes needed
- New code to write
- Existing code to modify
- Testing strategy
- Potential issues to watch for
```

Here's the kind of plan you'll get back:
```text
Implementation Plan: Email Notifications for Messages

1. Database Schema Changes
   - Add notifications table:
     * id, user_id, message_id, sent_at, status
   - Add email_notifications_enabled to users table

2. New Code to Create
   - services/emailService.js
     * sendMessageNotification(user, message)
     * Uses configured email provider
   - jobs/emailNotificationJob.js
     * Background job for sending emails
     * Handles retries and failures

3. Existing Code to Modify
   - routes/messages.js
     * After creating message, queue notification
   - models/User.js
     * Add preference methods

4. Configuration
   - Add email provider settings to config
   - Environment variables for email credentials

5. Testing Strategy
   - Unit tests for emailService
   - Integration tests for notification flow
   - Test email preferences
   - Test failure scenarios

6. Potential Issues
   - Email provider rate limits
   - Handling bounced emails
   - User privacy concerns
   - Performance impact on message creation

7. Implementation Order
   Step 1: Database migrations
   Step 2: Email service (isolated)
   Step 3: Background job
   Step 4: Integration with messages
   Step 5: Tests
   Step 6: Configuration and deployment

Estimated complexity: Medium
Estimated time: 4-6 hours for basic implementation
```

---

**Example 2: Refactoring**
```text
Plan out a refactor of the database queries from callbacks to async/await
```

---

**Example 3: Performance Optimization**
```text
The app is slow when loading the dashboard with lots of data.

Create an optimization strategy for improving dashboard load times.
```

---

## Lesson 4: Multi-Step Tasks

### Combining Research and Implementation

Some tasks don't fit neatly into "explore" or "plan" -- they combine research, implementation, testing, and documentation in a single prompt. Claude Code handles these multi-step workflows autonomously when you lay out the steps clearly.

---

### Multi-Step Task Examples

**Example 1: Research and Implement**
```text
1. Research current best practices for rate limiting APIs
2. Implement rate limiting on our API endpoints
3. Add tests for the rate limiting
4. Document how it works
```

Claude Code searches the web for best practices, reads your current code, implements rate limiting, creates tests, and writes documentation -- all autonomously.

---

**Example 2: Find and Fix Issues**
```text
Find all potential security vulnerabilities in the authentication system,
fix each issue, add tests to prevent regression, and document the changes.
```

---

**Example 3: Update Dependencies**
```text
1. Check which dependencies are outdated
2. Research breaking changes for major updates
3. Update dependencies safely
4. Fix any code that breaks
5. Run tests to verify everything works
```

---

## Lesson 5: Running Tasks in Parallel

### Why Work in Parallel?

Sometimes you've got three independent questions and no reason to ask them one at a time. Running parallel Claude Code sessions lets multiple tasks proceed simultaneously, so you get all your answers at once instead of waiting in sequence.

**Important:** A single Claude Code session handles one conversation at a time. To run tasks in parallel, you need to open multiple sessions -- either multiple terminal tabs or use `claude -p "task"` in separate terminals.

---

### How to Run Tasks in Parallel

**Open multiple terminal tabs and run separate commands:**
```bash
# Terminal 1
claude -p "Explore how the authentication system works in this codebase"

# Terminal 2
claude -p "Explore how the payment processing works in this codebase"

# Terminal 3
claude -p "List all API endpoints with their HTTP methods and parameters"
```

**Or open multiple interactive sessions:**
Each terminal tab runs its own `claude` session, working on a different part of the codebase at the same time.

---

### Practical Parallel Use Cases

**Planning multiple features at once (each in its own terminal):**
```bash
# Terminal 1
claude -p "Create an implementation plan for adding user roles to this project"

# Terminal 2
claude -p "Create an implementation plan for adding email notifications"

# Terminal 3
claude -p "Create an implementation plan for adding audit logging"
```

**Research across topics:**
```bash
# Terminal 1
claude -p "Research best practices for REST API design and summarize"

# Terminal 2
claude -p "Research best practices for database indexing and summarize"

# Terminal 3
claude -p "Research best practices for caching strategies and summarize"
```

---

### Choosing the Right Model for the Task

You can switch which model Claude Code uses with the `/model` command inside a session, or by passing the `--model` flag when starting a session. Different models suit different tasks:

| Task Type | Suggested Model | Why |
|-----------|----------------|-----|
| Architecture decisions | Opus | Needs deep reasoning |
| Writing feature code | Sonnet | Good balance of speed and quality |
| Generating test suites | Sonnet | Reliable pattern matching |
| Code review | Opus | Catches subtle issues |
| Generating documentation | Haiku | Fast, straightforward task |
| Codebase navigation | Haiku | Speed matters most |
| Tracing and fixing bugs | Sonnet | Needs code understanding |
| Cleaning up code | Sonnet | Balances quality and cost |
| Finding vulnerabilities | Opus | Catches subtle security issues |
| Database migrations | Sonnet | Reliable schema work |
| Designing API contracts | Opus | Architectural decisions |
| Auditing dependencies | Haiku | Fast scanning task |
| Code style fixes | Haiku | Simple, mechanical task |
| Generating changelogs | Haiku | Summarization task |
| Performance profiling | Sonnet | Needs analytical depth |
| Adding error handling | Sonnet | Pattern-aware changes |

**The pattern:** Opus for decisions that require deep thinking. Sonnet for tasks that need good judgment and code understanding. Haiku for fast, mechanical, or straightforward tasks. Matching the model to the task saves money and time without sacrificing quality where it matters.

**Note:** You can't assign different models to different tasks within a single session. To use different models in parallel, run separate sessions:

```bash
# Terminal 1 -- fast scan with Haiku
claude --model haiku -p "Scan for unused dependencies in this project"

# Terminal 2 -- thorough review with Sonnet
claude --model sonnet -p "Review the authentication flow for bugs"

# Terminal 3 -- deep planning with Opus
claude --model opus -p "Plan the migration from REST to GraphQL"
```

---

## Lesson 6: Working with Results

### Understanding the Output

Agentic tasks return comprehensive reports -- detailed explanations, file locations, code snippets, recommendations, and warnings about potential issues. The output is meant to be actionable, not just informational.

---

### Acting on Plans

Once Claude Code creates a plan, you've got options.

**Implement it:**
```text
That plan looks good. Let's implement step 1: database migrations
```

**Modify it:**
```text
The plan looks good, but instead of using a background job,
let's send emails synchronously for now. Update the plan.
```

**Ask for clarification:**
```text
I don't understand step 3. Can you explain in more detail?
```

---

### Using Exploration Results

After Claude Code explores your code, put that knowledge to work.

**Make informed changes:**
```text
Now that you've explained how auth works,
add two-factor authentication following the same patterns
```

**Dig deeper:**
```text
You mentioned the auth tokens expire after 24 hours.
Where is that configured?
```

---

## Lesson 7: Best Practices for Agentic Prompts

### Be Specific About What You Want

This is the single biggest factor in getting good results. Vague prompts produce vague output.

❌ **Vague:**
```text
Explore the codebase
```

✅ **Specific:**
```text
Explore the codebase to find and explain all database queries,
focusing on performance and potential N+1 query problems
```

---

### Set Clear Goals

❌ **Unclear:**
```text
Plan some improvements
```

✅ **Clear:**
```text
Create a plan to add caching to the API to reduce database load,
targeting the 5 most frequently called endpoints
```

---

### Specify Constraints

Real projects have real limitations. State your constraints upfront:
```text
Plan implementing real-time notifications, but:
- Cannot use WebSockets (firewall restrictions)
- Must work with current database (SQLite)
- Should be simple to deploy
```

---

### Ask for Different Perspectives

When you're weighing options, let Claude Code do the comparison work:
```text
Create 3 different approaches for adding search to this project:
1. Simple SQL LIKE queries
2. Full-text search with database
3. External search service (like Elasticsearch)

For each, list pros, cons, and complexity
```

---

## Hands-On Practice

### Exercise 1: Explore a Codebase

**Task:** Use an exploration prompt to understand a project

Find an open source project (or use one of yours) and try this:

```text
Explore this project and:
1. Explain what it does
2. Identify the main components
3. Show me the data flow
4. List potential areas for improvement
```

Then follow up on something that caught your eye:
```text
You mentioned [component]. Can you show me where it's defined?
```

---

### Exercise 2: Plan a Feature

**Task:** Use a planning prompt to design an implementation

In a project:

```text
I want to add user profile pictures.

Create an implementation plan for:
- Uploading images
- Storing images
- Serving images
- Updating user profiles
- Image validation and security

Consider:
- File size limits
- Image formats allowed
- Where to store (local vs cloud)
- Performance implications
```

Then refine based on what comes back:
```text
The plan suggests local storage, but I prefer cloud storage.
Update the plan to use AWS S3.
```

---

### Exercise 3: Parallel Research

**Task:** Research multiple topics simultaneously using separate sessions

Open three terminal tabs and run one in each:

```bash
# Terminal 1
claude -p "Research PostgreSQL full-text search: setup complexity, performance, maintenance, and costs"

# Terminal 2
claude -p "Research Elasticsearch integration with Node.js: setup complexity, performance, maintenance, and costs"

# Terminal 3
claude -p "Research simple SQL LIKE query performance at scale: setup complexity, performance, maintenance, and costs"
```

---

### Exercise 4: Complex Autonomous Task

**Task:** Delegate a complex multi-step task

```text
1. Find all console.log statements in the codebase
2. Replace them with a proper logging library (Winston)
3. Set up log levels (error, warn, info, debug)
4. Configure log output to file and console
5. Update all existing logs to use appropriate levels
6. Test that logging works
```

This would take you hours to do manually. Claude Code handles it in minutes.

---

## Module 6 Checklist

Before moving to Module 7, make sure you understand:

- [ ] What agentic tasks are and when to use them
- [ ] How to write exploration prompts to navigate codebases
- [ ] How to write planning prompts to design implementations
- [ ] How to delegate complex multi-step tasks
- [ ] How to run tasks in parallel using multiple sessions
- [ ] How to interpret and act on results
- [ ] Best practices for agentic prompts

---

## Common Questions (FAQ)

**Q: How long do these tasks take?**
Depends on the task. A simple exploration might take 30 seconds; complex planning can run 1-2 minutes.

**Q: Can I stop a task mid-run?**
Yes -- press Ctrl+C. You can also use /tasks to see background task status.

**Q: Does Claude Code make changes to my code during these tasks?**
Only if you ask it to implement something. Exploration and planning prompts just analyze and suggest.

**Q: Can Claude Code make mistakes on these tasks?**
Absolutely. Like any AI, it can get things wrong. Always review the output before acting on it.

**Q: Should I use agentic prompts for simple tasks?**
No. They add overhead. For quick, straightforward tasks, regular conversation is faster.

**Q: Can multiple parallel sessions work on the same files?**
Fair warning: if parallel sessions modify the same files, you can end up with conflicts. Keep parallel sessions pointed at different parts of the codebase.

---

## Prompting Approach Comparison

| Approach | Best For | Speed | Autonomy |
|----------|----------|-------|----------|
| Exploration prompts | Understanding code | Fast | High |
| Planning prompts | Designing implementations | Medium | High |
| Multi-step prompts | Complex tasks combining research and implementation | Varies | Very High |
| Specialized prompts | Domain-specific jobs (security, docs, testing) | Varies | High |
| Regular conversation | Simple tasks, learning | Fastest | Low |

---

## Pro Tips

1. **Explore before modifying** -- understand the code first
2. **Plan before complex features** -- think before coding
3. **Be specific** -- clear goals produce better results
4. **Review the output** -- verify, don't blindly trust
5. **Parallel sessions for independent tasks** -- save time when tasks don't overlap
6. **Ask follow-up questions** -- Claude Code can clarify its own findings
7. **Iterate on plans** -- refine until you're confident

---

Next up: Module 7 -- where you'll learn to handle Git operations and version control through Claude Code.

> **Agentic workflows are just the beginning.** The [Advanced Modules](https://payhip.com/b/8E107) cover multi-session orchestration (Module 17) and building custom agentic workflows with autonomous loops and safety safeguards (Module 21).

---

*Module 6 Complete*

---

<div align="center">

[← Previous Module](module-05-custom-instructions.md) · [🏠 Home](README.md) · [Next Module →](module-07-git-operations.md)

</div>
