# Module 9: Real-World Project - Building a Complete Application

**Goal:** Apply everything you've learned by building a complete, production-ready application

**Estimated Time:** 3-6 hours (can be done over multiple sessions)

---

## What You'll Learn

This is where it all comes together. You're going to build a real application from scratch -- authentication, database, tests, error handling, documentation, version control, and deployment. It's a lot, but you've already practiced every piece of this individually. Now you're just wiring it all up into one cohesive project.

---

## Project Options

Choose one project that interests you:

### Option 1: Task Management API
A REST API for managing tasks and projects

**Features:**
- User authentication (register/login)
- Create, read, update, delete tasks
- Organize tasks into projects
- Assign tasks to users
- Mark tasks as complete
- Filter and search tasks

**Technologies:** Node.js, Express, SQLite/PostgreSQL, JWT

---

### Option 2: Blog Platform
A complete blogging system with frontend and backend

**Features:**
- User accounts
- Create/edit/delete blog posts
- Rich text editor
- Comments on posts
- Tags and categories
- Search functionality

**Technologies:** React/Next.js, Node.js, Express, Database

---

### Option 3: URL Shortener Service
A service for creating short URLs

**Features:**
- Shorten long URLs
- Custom short codes
- Track click analytics
- Expiration dates for links
- User accounts for managing links
- API for programmatic access

**Technologies:** Node.js, Express, Redis/Database

---

### Option 4: Weather Dashboard
A weather information dashboard

**Features:**
- Search weather by city
- 5-day forecast
- Save favorite locations
- Weather alerts
- Historical data visualization
- Mobile-responsive design

**Technologies:** React, External Weather API, Node.js backend

---

### Option 5: CLI Todo Application
A powerful command-line task manager

Features range from basic CRUD to categories, tags, due dates, priorities, search, filtering, data persistence, and a colorful interactive CLI.

**Technologies:** Node.js, Commander.js, Chalk, SQLite

---

## We'll Build: Task Management API

For this module, we'll walk through Option 1 step by step. The process applies to any of the projects above -- pick whichever one excites you and adapt the prompts accordingly.

---

## Phase 1: Planning and Setup

### Step 1: Define Requirements

```
I want to build a Task Management API.

Help me create a complete requirements document including:
- Feature list
- API endpoints needed
- Database schema
- Authentication approach
- Technology stack recommendations
```

---

### Step 2: Project Structure Planning

```
For this Task Management API, help me plan:
- Project folder structure
- File organization
- Separation of concerns (routes, controllers, services, models)
- Configuration management
- Where tests should go
```

---

### Step 3: Initialize Project

```
Initialize a new Node.js project for the Task Management API with:
- package.json with appropriate metadata
- Express.js
- SQLite with better-sqlite3
- JWT for authentication
- bcrypt for password hashing
- Jest for testing
- ESLint and Prettier for code quality
- nodemon for development
```

---

### Step 4: Set Up Project Structure

```
Create the following folder structure:
src/
  ├── routes/       (API routes)
  ├── controllers/  (Request handlers)
  ├── services/     (Business logic)
  ├── models/       (Database models)
  ├── middleware/   (Auth, validation, etc.)
  ├── utils/        (Helper functions)
  └── config/       (Configuration)
tests/
  ├── unit/
  └── integration/
.env.example
.gitignore
README.md
```

---

### Step 4b: Create a CLAUDE.md

Before diving into implementation, set up a CLAUDE.md so Claude Code follows your project's conventions from the start:

```
Create a CLAUDE.md for this Task Management API that includes:

## Quick Commands
- npm run dev — Start development server
- npm test — Run all tests
- npm run lint — Run ESLint

## Architecture
- Express.js REST API with SQLite
- Routes → Controllers → Services → Models pattern
- All async functions use try/catch

## Code Style
- Use async/await, never callbacks
- Named exports, not default
- Error responses: { error: { status, message, details } }

## Testing
- Jest for unit and integration tests
- Tests go in tests/ directory mirroring src/ structure
- Mock database for unit tests, test database for integration

## Don't
- Don't use console.log — use the logger utility
- Don't skip input validation on any endpoint
- Don't commit .env files
```

This file gets loaded automatically every time you start Claude Code in this project. Instead of reminding Claude about your conventions in every prompt, you write them once and they stick. As the project grows, update CLAUDE.md to reflect what you've learned.

---

## Phase 2: Database Setup

### Step 5: Design Database Schema

```
Design the database schema for:

Users table:
- id, email, password_hash, name, created_at

Projects table:
- id, name, description, owner_id, created_at

Tasks table:
- id, title, description, project_id, assigned_to, status, priority, due_date, created_at, completed_at

Create the SQL schema with:
- Proper data types
- Foreign keys and constraints
- Indexes for performance
```

---

### Step 6: Create Database Connection

```
Create a database connection module at src/config/database.js that:
- Connects to SQLite database
- Creates tables if they don't exist
- Exports database instance
- Includes error handling
- Has a method to run migrations
```

---

### Step 7: Create Database Models

```
Create model files for User, Project, and Task with methods:

User model:
- create(email, password, name)
- findByEmail(email)
- findById(id)
- verifyPassword(email, password)

Project model:
- create(name, description, ownerId)
- findById(id)
- findByOwner(ownerId)
- update(id, data)
- delete(id)

Task model:
- create(taskData)
- findById(id)
- findByProject(projectId)
- update(id, data)
- delete(id)
- markComplete(id)
```

---

## Phase 3: Authentication

### Step 8: Create Authentication Middleware

```
Create authentication middleware at src/middleware/auth.js that:
- Extracts JWT token from Authorization header
- Verifies the token
- Adds user info to request object
- Returns 401 if invalid/missing token
```

---

### Step 9: Create Auth Routes

```
Create authentication routes at src/routes/auth.js:

POST /api/auth/register
- Validates input (email, password, name)
- Hashes password
- Creates user
- Returns JWT token

POST /api/auth/login
- Validates credentials
- Verifies password
- Returns JWT token

GET /api/auth/me
- Requires authentication
- Returns current user info
```

---

### Step 10: Implement Password Hashing

```
Create a utility module for password hashing at src/utils/password.js with:
- hash(password) - returns hashed password
- verify(password, hash) - returns boolean
Use bcrypt with salt rounds of 10
```

---

## Phase 4: Core Features

### Step 11: Project Routes

```
Create project routes at src/routes/projects.js:

GET /api/projects
- List all projects for authenticated user
- Optional filtering by name

POST /api/projects
- Create new project
- Requires: name, description (optional)
- Sets owner_id to current user

GET /api/projects/:id
- Get single project
- Check user owns the project

PUT /api/projects/:id
- Update project
- Check user owns the project

DELETE /api/projects/:id
- Delete project and all its tasks
- Check user owns the project
```

---

### Step 12: Task Routes

```
Create task routes at src/routes/tasks.js:

GET /api/tasks
- List all tasks for authenticated user
- Optional filters: project_id, status, priority
- Optional sorting

POST /api/tasks
- Create new task
- Requires: title, project_id
- Optional: description, due_date, priority, assigned_to

GET /api/tasks/:id
- Get single task
- Check user has access

PUT /api/tasks/:id
- Update task
- Check user has access

PATCH /api/tasks/:id/complete
- Mark task as complete
- Set completed_at timestamp

DELETE /api/tasks/:id
- Delete task
- Check user has access
```

---

### Step 13: Add Input Validation

```
Create validation middleware using express-validator for:
- Email format
- Password strength (min 8 chars)
- Required fields
- Data types
- String lengths

Add validation to all routes
Return clear error messages
```

---

## Phase 5: Error Handling and Logging

### Step 14: Global Error Handler

This matters more than you think. A good error handler is the difference between an app that's debuggable and one that's a nightmare when something goes wrong in production.

```
Create error handling middleware at src/middleware/errorHandler.js that:
- Catches all errors
- Logs errors to console (with stack trace in development)
- Returns appropriate status codes
- Doesn't leak sensitive information
- Handles different error types:
  * Validation errors → 400
  * Authentication errors → 401
  * Authorization errors → 403
  * Not found errors → 404
  * Database errors → 500
```

---

### Step 15: Add Logging

```
Set up logging using a simple logger at src/utils/logger.js:
- Log requests (method, path, status, duration)
- Log errors with stack traces
- Different log levels (info, warn, error)
- Optionally log to file in production
```

---

## Phase 6: Testing

### Step 16: Unit Tests

```
Create unit tests for:

src/utils/password.test.js
- Test password hashing
- Test password verification

src/models/user.test.js
- Test user creation
- Test finding users
- Test password verification

src/models/task.test.js
- Test task CRUD operations
- Test task completion
```

---

### Step 17: Integration Tests

```
Create integration tests for all API endpoints:

tests/integration/auth.test.js
- Test registration flow
- Test login flow
- Test protected routes
- Test invalid credentials

tests/integration/projects.test.js
- Test creating projects
- Test listing projects
- Test updating projects
- Test deleting projects
- Test authorization

tests/integration/tasks.test.js
- Test creating tasks
- Test listing with filters
- Test completing tasks
- Test deleting tasks
```

---

### Step 18: Run Tests and Fix Issues

```
Run all tests with coverage:
npm test -- --coverage

Fix any failing tests
Aim for at least 80% coverage
Add tests for any uncovered critical paths
```

Fair warning: you'll almost certainly have failing tests on the first run. That's normal. Read the error messages carefully -- Claude Code can help you fix them, but understanding *why* they fail is where the real learning happens.

---

## Phase 7: Documentation

### Step 19: API Documentation

```
Create comprehensive API documentation in README.md:

For each endpoint, document:
- HTTP method and path
- Authentication requirements
- Request body/parameters
- Response format
- Example requests and responses
- Possible error codes
```

---

### Step 20: Setup Instructions

```
Add to README.md:
- Project description
- Prerequisites (Node.js version, etc.)
- Installation steps
- Environment variables needed
- How to run in development
- How to run tests
- How to build for production
```

---

## Phase 8: Version Control

### Step 21: Initialize Git

```
Initialize git repository
Create a good .gitignore file for Node.js
Create initial commit with project structure
```

---

### Step 22: Commit Your Progress

Trust me on this -- small, meaningful commits will save you later. If you break something, you can roll back to a working state instead of starting over.

```
Create meaningful commits for each major feature:
- "Add database schema and models"
- "Implement authentication"
- "Add project CRUD endpoints"
- "Add task management features"
- "Add input validation"
- "Add error handling and logging"
- "Add comprehensive tests"
- "Add documentation"
```

---

### Step 23: Create Repository on GitHub

```
Create a new repository on GitHub
Add remote origin
Push your code
Create a good README with badges
```

---

## Phase 9: Deployment Preparation

### Step 24: Environment Configuration

```
Create proper environment variable handling:
- .env.example with all required variables
- config/env.js to load and validate env vars
- Different configs for dev/test/production
```

---

### Step 25: Production Readiness

```
Add production optimizations:
- Compression middleware
- Rate limiting
- CORS configuration
- Security headers (helmet.js)
- Request size limits
- Proper HTTPS setup instructions
```

---

### Step 26: Create Start Scripts

```
Add scripts to package.json:
- "start": Production server
- "dev": Development with nodemon
- "test": Run all tests
- "test:watch": Run tests in watch mode
- "lint": Run ESLint
- "format": Run Prettier
```

---

## Phase 10: Optional Enhancements

### Enhancement Ideas

If you want to keep going, here are some directions to take it:

**More features:**
```
- Task due date reminders
- Task comments
- File attachments to tasks
- Task priority sorting
- Search functionality
- Pagination for lists
- Task history/audit log
- Team collaboration features
```

**Code quality improvements:**
```
- Add TypeScript
- Add API versioning
- Add request caching
- Add database query optimization
- Add comprehensive logging
- Add monitoring and metrics
```

---

## Final Checklist

Before considering your project complete:

**Functionality:**
- [ ] All core features work
- [ ] Authentication is secure
- [ ] Database operations are correct
- [ ] Validation works properly
- [ ] Error handling is comprehensive

**Code Quality:**
- [ ] Code is well-organized
- [ ] Functions are small and focused
- [ ] Names are descriptive
- [ ] No code duplication
- [ ] ESLint passes with no errors

**Testing:**
- [ ] Unit tests for models and utilities
- [ ] Integration tests for all endpoints
- [ ] Tests cover edge cases
- [ ] All tests pass
- [ ] Coverage is 80%+

**Documentation:**
- [ ] README is comprehensive
- [ ] API endpoints are documented
- [ ] Setup instructions are clear
- [ ] Code has helpful comments
- [ ] Examples are provided

**Version Control:**
- [ ] Git history is clean
- [ ] Commits are meaningful
- [ ] Code is on GitHub
- [ ] .gitignore is proper
- [ ] No secrets in repository

**Production Ready:**
- [ ] Environment variables are used
- [ ] Security best practices followed
- [ ] Error handling doesn't leak info
- [ ] Logging is adequate
- [ ] Performance is acceptable

---

## Deploying Your Project

### Option 1: Deploy to Heroku

```
Help me deploy this application to Heroku:
1. Create Heroku app
2. Add PostgreSQL database
3. Set environment variables
4. Configure build scripts
5. Deploy and test
```

---

### Option 2: Deploy with Docker

```
Create a Dockerfile for this application:
- Use Node.js Alpine image
- Copy and install dependencies
- Copy source code
- Expose port
- Set start command

Also create docker-compose.yml for local development
```

---

### Option 3: Deploy to VPS

```
Create deployment guide for Ubuntu VPS:
- Install Node.js
- Set up process manager (PM2)
- Configure Nginx reverse proxy
- Set up SSL with Let's Encrypt
- Configure firewall
- Set up automatic restarts
```

---

## What You've Built

Here's what you walked away with from this module:

- A complete, working API with secure authentication
- A proper database schema with models
- Comprehensive tests (unit and integration)
- Solid error handling and logging
- Real documentation that someone else could follow
- A professional Git history
- Production-ready configuration

This is a genuine portfolio piece. It's the kind of project that shows you can build something end to end, not just follow a tutorial.

---

## Going Further

A few ideas if you want to keep building on this:

1. **Add a frontend** -- build a React app that consumes your API
2. **Implement the enhancements** from Phase 10
3. **Actually deploy it** -- getting code running on a real server teaches you things nothing else will
4. **Ask for code review** -- have someone else read your code and give feedback
5. **Contribute to open source** -- you've got the workflow down now

---

Next up: Module 10 -- professional workflow and best practices that'll make everything you build from here on cleaner and faster.

---

*Module 9 Complete*
