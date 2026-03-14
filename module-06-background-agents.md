# Module 6: Using Background Agents

**Goal:** Learn to use specialized agents for complex, multi-step tasks

**Estimated Time:** 35-45 minutes

---

## What You'll Learn

This module covers agents -- specialized AI assistants that can work autonomously on tasks too big or tedious to guide step by step. You'll learn when to reach for them, how to use the Explore and Plan agents effectively, and how to run multiple agents in parallel when you've got a lot of ground to cover.

---

## Lesson 1: What Are Agents?

### Understanding Agents

Agents are specialized AI assistants that work autonomously to complete complex tasks. You give them a goal, they figure out the steps, and they report back when they're done.

Here's how they differ from regular Claude Code conversation:

**Regular Claude Code:**
- You guide each step
- Interactive back-and-forth
- You make decisions

**Agent Mode:**
- Works autonomously
- Makes its own decisions
- Completes multi-step tasks independently
- Reports back when done

The short version: regular mode is a conversation, agent mode is delegation.

---

### When to Use Agents

Agents shine when the task involves multiple steps, exploration, or decision-making. Think: exploring an unfamiliar codebase, planning a feature implementation, doing research that spans many files, or tackling anything where you'd otherwise be saying "now do this... okay now do that..." over and over.

Stick with regular conversation for simple, single-step tasks, quick modifications, or situations where you actually want to see each tool call happen -- like when you're learning how Claude Code works under the hood.

---

## Lesson 2: The Explore Agent

### What is the Explore Agent?

The Explore agent is built for navigating and understanding codebases. It's your go-to when you need to figure out how a project works, find where something is implemented, or get a feel for the architecture. It reads files, traces connections, and comes back with a coherent explanation instead of making you piece it together yourself.

---

### Using the Explore Agent

**Basic usage:**
```
Use the Explore agent to understand how authentication works in this codebase
```

When you send that, the agent searches for auth-related files, reads the relevant code, analyzes the implementation, traces the flow, and returns a comprehensive explanation. All without you lifting a finger.

---

### Explore Agent Examples

**Example 1: Understanding a Feature**
```
Use the Explore agent to explain how the payment processing works.
I need to know:
- What payment providers are supported
- How transactions are stored
- What security measures are in place
- The complete flow from checkout to confirmation
```

The agent finds all payment-related files, reads configuration, traces the payment flow, analyzes security implementations, and gives you a detailed report.

---

**Example 2: Finding API Endpoints**
```
Explore the codebase and list all API endpoints with:
- HTTP method
- Route path
- What each endpoint does
- Required parameters
```

You'll get back something like:
```
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
```
Use the Explore agent to analyze this codebase and explain:
- Overall architecture pattern (MVC, layered, etc.)
- How different parts communicate
- Database structure
- External dependencies
```

---

### Specifying Thoroughness

You can control how deep the agent digs. This matters more than you think -- a quick scan and a thorough analysis produce very different results.

**Quick exploration:**
```
Use the Explore agent with quick thoroughness to give me an overview of this project
```

**Medium exploration:**
```
Use the Explore agent with medium thoroughness to understand the authentication system
```

**Thorough exploration:**
```
Use the Explore agent with very thorough analysis to document the entire API
```

---

## Lesson 3: The Plan Agent

### What is the Plan Agent?

The Plan agent designs implementation strategies before you write a single line of code. Use it for planning new features, designing system architecture, thinking through complex changes, and catching problems before they become expensive.

Trust me on this -- spending five minutes with the Plan agent before a big change can save you hours of backtracking.

---

### Using the Plan Agent

**Basic usage:**
```
Use the Plan agent to create an implementation plan for adding user roles and permissions
```

The agent analyzes your current codebase, identifies what needs to change, plans the steps, considers edge cases, and returns a detailed plan you can follow -- or push back on.

---

### Plan Agent Examples

**Example 1: Adding a Feature**
```
I want to add email notifications when users receive messages.

Use the Plan agent to create an implementation plan including:
- Database changes needed
- New code to write
- Existing code to modify
- Testing strategy
- Potential issues to watch for
```

Here's the kind of plan you'll get back:
```
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
```
Use the Plan agent to plan refactoring the database queries from callbacks to async/await
```

---

**Example 3: Performance Optimization**
```
The app is slow when loading the dashboard with lots of data.

Use the Plan agent to create an optimization strategy.
```

---

## Lesson 4: General-Purpose Agent

### What is the General-Purpose Agent?

This one handles complex, multi-step tasks that don't fit neatly into "explore" or "plan." It's best for tasks that combine research and implementation, require web search, involve complex debugging, or need comprehensive updates across a codebase.

---

### General-Purpose Agent Examples

**Example 1: Research and Implement**
```
Use an agent to:
1. Research current best practices for rate limiting APIs
2. Implement rate limiting on our API endpoints
3. Add tests for the rate limiting
4. Document how it works
```

The agent searches the web for best practices, reads your current code, implements rate limiting, creates tests, and writes documentation -- all autonomously.

---

**Example 2: Find and Fix Issues**
```
Use an agent to:
- Find all potential security vulnerabilities in the authentication system
- Fix each issue found
- Add tests to prevent regression
- Document the changes
```

---

**Example 3: Update Dependencies**
```
Use an agent to:
1. Check which dependencies are outdated
2. Research breaking changes for major updates
3. Update dependencies safely
4. Fix any code that breaks
5. Run tests to verify everything works
```

---

## Lesson 5: Running Agents in Parallel

### Why Use Parallel Agents?

Sometimes you've got three independent questions and no reason to ask them one at a time. Parallel agents let multiple tasks run simultaneously, so you get all your answers at once instead of waiting in sequence.

---

### How to Run Agents in Parallel

**Request parallel execution:**
```
Please run these tasks in parallel using agents:

Agent 1: Explore the authentication system
Agent 2: Explore the payment processing
Agent 3: Create a list of all API endpoints
```

**Or:**
```
I need to understand three parts of this codebase simultaneously.
Use parallel agents to:
- Explore how data validation works
- Explore how errors are handled
- Explore how logging is implemented
```

---

### Practical Parallel Use Cases

**Planning multiple features at once:**
```
Run in parallel:
1. Agent to plan implementing user roles
2. Agent to plan implementing email notifications
3. Agent to plan implementing audit logging
```

**Research across topics:**
```
Research in parallel:
1. Best practices for REST API design
2. Best practices for database indexing
3. Best practices for caching strategies

Then summarize findings
```

---

### Real-World Agent Roster

In production projects, teams often maintain a roster of specialized agents, each tuned for a specific job. Here's what a mature agent setup looks like:

| Agent | Purpose | Model Tier | Why This Tier |
|-------|---------|-----------|---------------|
| `planner` | Architecture decisions | Opus | Needs deep reasoning |
| `implementer` | Write feature code | Sonnet | Good balance of speed and quality |
| `test-writer` | Generate test suites | Sonnet | Reliable pattern matching |
| `reviewer` | Code review and feedback | Opus | Catches subtle issues |
| `doc-writer` | Generate documentation | Haiku | Fast, straightforward task |
| `explorer` | Codebase navigation | Haiku | Speed matters most |
| `debugger` | Trace and fix bugs | Sonnet | Needs code understanding |
| `refactorer` | Clean up code | Sonnet | Balances quality and cost |
| `security-scanner` | Find vulnerabilities | Opus | Catches subtle security issues |
| `migrator` | Database migrations | Sonnet | Reliable schema work |
| `api-designer` | Design API contracts | Opus | Architectural decisions |
| `dependency-checker` | Audit dependencies | Haiku | Fast scanning task |
| `formatter` | Code style fixes | Haiku | Simple, mechanical task |
| `changelog-writer` | Generate changelogs | Haiku | Summarization task |
| `perf-analyzer` | Performance profiling | Sonnet | Needs analytical depth |
| `error-handler` | Add error handling | Sonnet | Pattern-aware changes |

**The pattern:** Opus for decisions that require deep thinking. Sonnet for tasks that need good judgment and code understanding. Haiku for fast, mechanical, or straightforward tasks. Matching the model to the task saves money and time without sacrificing quality where it matters.

**Using this in practice:**
```
Run these agents in parallel:
- Haiku agent: Scan for unused dependencies
- Sonnet agent: Review the authentication flow for bugs
- Opus agent: Plan the migration from REST to GraphQL
```

Each agent gets the model tier that matches the complexity of its job.

---

## Lesson 6: Working with Agent Results

### Understanding Agent Output

Agents return comprehensive reports -- detailed explanations, file locations, code snippets, recommendations, and warnings about potential issues. The output is meant to be actionable, not just informational.

---

### Acting on Agent Plans

Once an agent creates a plan, you've got options.

**Implement it:**
```
That plan looks good. Let's implement step 1: database migrations
```

**Modify it:**
```
The plan looks good, but instead of using a background job,
let's send emails synchronously for now. Update the plan.
```

**Ask for clarification:**
```
I don't understand step 3. Can you explain in more detail?
```

---

### Using Explore Results

After an agent explores your code, put that knowledge to work.

**Make informed changes:**
```
Now that you've explained how auth works,
add two-factor authentication following the same patterns
```

**Dig deeper:**
```
You mentioned the auth tokens expire after 24 hours.
Where is that configured?
```

---

## Lesson 7: Agent Best Practices

### Be Specific About What You Want

This is the single biggest factor in getting good results from agents. Vague prompts produce vague output.

❌ **Vague:**
```
Explore the codebase
```

✅ **Specific:**
```
Explore the codebase to find and explain all database queries,
focusing on performance and potential N+1 query problems
```

---

### Set Clear Goals

❌ **Unclear:**
```
Plan some improvements
```

✅ **Clear:**
```
Create a plan to add caching to the API to reduce database load,
targeting the 5 most frequently called endpoints
```

---

### Specify Constraints

Real projects have real limitations. Tell the agent about yours upfront:
```
Plan implementing real-time notifications, but:
- Cannot use WebSockets (firewall restrictions)
- Must work with current database (SQLite)
- Should be simple to deploy
```

---

### Ask for Different Perspectives

When you're weighing options, let the agent do the comparison work:
```
Use the Plan agent to create 3 different approaches for adding search:
1. Simple SQL LIKE queries
2. Full-text search with database
3. External search service (like Elasticsearch)

For each, list pros, cons, and complexity
```

---

## Hands-On Practice

### Exercise 1: Explore a Codebase

**Task:** Use Explore agent to understand a project

Find an open source project (or use one of yours) and try this:

```
Use the Explore agent to:
1. Explain what this project does
2. Identify the main components
3. Show me the data flow
4. List potential areas for improvement
```

Then follow up on something that caught your eye:
```
You mentioned [component]. Can you show me where it's defined?
```

---

### Exercise 2: Plan a Feature

**Task:** Use Plan agent to design an implementation

In a project:

```
I want to add user profile pictures.

Use the Plan agent to create an implementation plan for:
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
```
The plan suggests local storage, but I prefer cloud storage.
Update the plan to use AWS S3.
```

---

### Exercise 3: Parallel Research

**Task:** Research multiple topics simultaneously

```
I'm deciding how to implement search functionality.

Use parallel agents to research:
1. PostgreSQL full-text search capabilities and setup
2. Elasticsearch integration with Node.js
3. Simple LIKE query performance at scale

For each, provide:
- Setup complexity
- Performance characteristics
- Maintenance requirements
- Costs
```

---

### Exercise 4: Complex Autonomous Task

**Task:** Let an agent complete a complex task

```
Use an agent to:
1. Find all console.log statements in the codebase
2. Replace them with a proper logging library (Winston)
3. Set up log levels (error, warn, info, debug)
4. Configure log output to file and console
5. Update all existing logs to use appropriate levels
6. Test that logging works
```

This would take you hours to do manually. An agent handles it in minutes.

---

## Module 6 Checklist

Before moving to Module 7, make sure you understand:

- [ ] What agents are and when to use them
- [ ] How to use the Explore agent to navigate codebases
- [ ] How to use the Plan agent to design implementations
- [ ] When to use general-purpose agents
- [ ] How to run agents in parallel
- [ ] How to interpret and act on agent results
- [ ] Best practices for working with agents

---

## Common Questions (FAQ)

**Q: How long do agents take?**
Depends on the task. A simple exploration might take 30 seconds; complex planning can run 1-2 minutes.

**Q: Can I stop an agent mid-task?**
Yes -- press Ctrl+C or use the /tasks command to manage running agents.

**Q: Do agents make changes to my code?**
Only if you ask them to implement something. Explore and Plan agents just analyze and suggest.

**Q: Can agents make mistakes?**
Absolutely. Like any AI, they can get things wrong. Always review agent output before acting on it.

**Q: Should I use agents for simple tasks?**
No. Agents add overhead. For quick, straightforward tasks, regular conversation is faster.

**Q: Can multiple agents work on the same files?**
Fair warning: if agents modify the same files, you can end up with conflicts. Keep parallel agents pointed at different parts of the codebase.

---

## Agent Comparison Table

| Agent Type | Best For | Speed | Autonomy |
|------------|----------|-------|----------|
| Explore | Understanding code | Fast | High |
| Plan | Designing implementations | Medium | High |
| General | Complex multi-step tasks | Varies | Very High |
| Specialized | Domain-specific jobs (security, docs, testing) | Varies | High |
| Regular Chat | Simple tasks, learning | Fastest | Low |

---

## Pro Tips

1. **Use Explore before modifying** -- understand the code first
2. **Use Plan for complex features** -- think before coding
3. **Be specific** -- clear goals produce better results
4. **Review agent output** -- verify, don't blindly trust
5. **Parallel for independent tasks** -- save time when tasks don't overlap
6. **Ask follow-up questions** -- agents can clarify their own findings
7. **Iterate on plans** -- refine until you're confident

---

Next up: Module 7 -- where you'll learn to handle Git operations and version control through Claude Code.

---

*Module 6 Complete*
