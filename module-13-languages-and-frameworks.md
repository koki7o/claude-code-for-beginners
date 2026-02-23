# Module 13: Working with Different Languages and Frameworks

**Goal:** Use Claude Code across different tech stacks and languages

**Estimated Time:** 45-60 minutes

---

## What You'll Learn

Claude Code doesn't care what language you're writing in. Python, TypeScript, Go, Rust, Java -- it handles all of them, and it adapts to the idioms and conventions of each. This module walks you through using it across the major ecosystems, including projects that mix several languages at once.

---

## Lesson 1: Python Development

### Setting Up Python Projects

Here's a solid starting prompt for spinning up a new Python project from scratch:

```
Create a new Python project with:
- Virtual environment
- requirements.txt
- src/ directory structure
- pytest for testing
- black for formatting
- pylint for linting
- setup.py for package management
```

---

### Flask Web Application

```
Create a Flask API with:
- Application factory pattern
- Blueprints for routes
- SQLAlchemy for database
- Flask-JWT-Extended for auth
- pytest fixtures for testing
- Configuration management (dev/prod)
```

---

### Django Project

```
Create a Django project for a blog with:
- Custom user model
- Blog app with posts and comments
- Django REST framework for API
- Authentication and permissions
- Admin interface customization
- Tests for models, views, and API
```

---

### FastAPI Application

FastAPI is where Claude Code really shines with Python -- the type hints and Pydantic models give it a lot to work with.

```
Create a FastAPI application with:
- Automatic API documentation
- Pydantic models for validation
- Async database operations
- JWT authentication
- CORS configuration
- Docker setup
```

---

## Lesson 2: JavaScript/TypeScript

### Node.js Backend

Express with TypeScript is one of the most common stacks you'll encounter. Claude Code handles the boilerplate well, which is good because there's a lot of it.

```
Create an Express TypeScript API with:
- TypeScript strict mode
- Express routing with types
- TypeORM for database
- JWT authentication
- Validation with Zod
- Testing with Jest
- ESLint and Prettier
```

---

### React Frontend

```
Create a React app with:
- TypeScript
- React Router for navigation
- React Query for data fetching
- Context API for state management
- Tailwind CSS for styling
- Vitest for testing
- Component library structure
```

---

### Next.js Full-Stack

```
Create a Next.js application with:
- App Router (latest)
- Server Components
- API Routes
- Server Actions
- Authentication with NextAuth
- Database with Prisma
- Tailwind CSS
- Deployment configuration
```

---

## Lesson 3: Go Development

### Go CLI Application

```
Create a Go CLI tool that:
- Uses Cobra for commands
- Handles flags and arguments
- Reads configuration files
- Interacts with APIs
- Has proper error handling
- Includes tests
- Can be compiled for multiple platforms
```

---

### Go Web Server

```
Create a Go web server with:
- Chi router
- Middleware (logging, recovery, CORS)
- Database with sqlx or GORM
- JWT authentication
- Structured logging
- Graceful shutdown
- Docker containerization
```

---

## Lesson 4: Rust Development

### Rust CLI Tool

Fair warning: Rust projects involve more back-and-forth with Claude Code than most languages. The borrow checker is strict, and sometimes the generated code won't compile on the first pass. That's normal. Just feed the compiler errors back and Claude Code will sort it out.

```
Create a Rust CLI application with:
- Clap for argument parsing
- Error handling with thiserror or anyhow
- File I/O
- JSON/YAML parsing with serde
- Testing with built-in test framework
- Documentation with rustdoc
```

---

### Rust Web API

```
Create a Rust web API with Actix-web:
- RESTful routes
- Database with SQLx
- JWT authentication
- Request validation
- Error handling
- Tests
- OpenAPI documentation
```

---

## Lesson 5: Java and Spring Boot

### Spring Boot REST API

Spring Boot projects tend to be verbose. Claude Code is genuinely useful here because it'll generate all the annotation-heavy boilerplate that nobody wants to type by hand.

```
Create a Spring Boot application with:
- Maven or Gradle build
- Spring Data JPA
- Spring Security with JWT
- Validation
- Exception handling
- Swagger/OpenAPI docs
- JUnit tests
- Docker configuration
```

---

## Lesson 6: Polyglot Projects

### Working with Multiple Languages

This is where things get interesting. Real-world projects often mix languages -- a Python ML service talking to a Go gateway, fronted by a Node.js app. Claude Code can context-switch between them in the same session.

```
I have a project with:
- Python ML service (FastAPI)
- Go API gateway
- Node.js frontend (Next.js)
- Rust data processor

Help me:
1. Set up the monorepo structure
2. Configure Docker Compose for local development
3. Set up shared types/contracts
4. Create inter-service communication
5. Set up testing for each service
```

---

### Language-Specific Tasks

You can tell Claude Code to adjust its style based on what file you're working in. Trust me on this -- it makes polyglot work way smoother.

```
I'm working in a polyglot codebase.
When I ask you to add features:
- In .py files: Use Python best practices
- In .go files: Follow Go idioms
- In .ts files: Use TypeScript patterns
- In .rs files: Follow Rust conventions
```

---

## Lesson 7: Language-Specific Best Practices

Each language has its own set of conventions, and Claude Code can enforce them for you. Use these review prompts when you want a second pair of eyes on your code.

### Python Best Practices

```
Review this Python code and ensure it follows:
- PEP 8 style guide
- Type hints where appropriate
- Docstrings for functions/classes
- List comprehensions over loops (where readable)
- Context managers for resources
- Proper exception handling
```

---

### JavaScript/TypeScript Best Practices

```
Review this TypeScript code for:
- Proper type annotations
- Avoid 'any' types
- Use const/let, not var
- Async/await over callbacks
- Functional programming patterns
- Error boundaries in React
```

---

### Go Best Practices

```
Review this Go code for:
- Proper error handling
- Effective use of goroutines
- Channel usage
- Interface design
- Package structure
- Idiomatic Go patterns
```

---

### Rust Best Practices

```
Review this Rust code for:
- Ownership and borrowing
- Error handling with Result
- Use of traits
- Avoiding unnecessary clones
- Proper lifetimes
- Zero-cost abstractions
```

---

## Hands-On Practice

### Exercise 1: Python FastAPI Project

Build a complete FastAPI application from scratch:

```
Create a FastAPI task management API with:
- Users and authentication
- CRUD operations for tasks
- Database with SQLAlchemy
- Async operations
- Input validation with Pydantic
- Tests with pytest
- Documen in README
```

---

### Exercise 2: TypeScript Full-Stack

Build a Next.js + API application:

```
Create a Next.js blog application with:
- Server components for listing posts
- Client components for interactions
- API routes for CRUD
- Database with Prisma
- TypeScript throughout
- Tailwind for styling
```

---

### Exercise 3: Go Microservice

Build a production-style Go service:

```
Create a Go microservice that:
- Serves a REST API
- Connects to PostgreSQL
- Has health check endpoint
- Structured logging
- Graceful shutdown
- Docker container
- Kubernetes deployment config
```

---

### Exercise 4: Polyglot Integration

This one's the most ambitious -- connect services written in different languages into a working system:

```
Build a system with:
- Python ML model service
- Node.js API that calls the ML service
- Go service for data processing
- All communicating via HTTP/gRPC
- Docker Compose setup
- Shared API contracts
```

---

## Module 13 Checklist

- [ ] Can create Python projects (Flask, Django, FastAPI)
- [ ] Can build JavaScript/TypeScript apps (Node, React, Next.js)
- [ ] Can develop Go applications
- [ ] Can write Rust programs
- [ ] Can work with Java/Spring Boot
- [ ] Can handle polyglot projects
- [ ] Know language-specific best practices

---

## Language-Specific Commands

### Python
```
# Create virtual environment
python -m venv venv

# Activate (Linux/Mac)
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run tests
pytest
```

### Node.js/TypeScript
```
# Install dependencies
npm install

# Run dev server
npm run dev

# Run tests
npm test

# Build
npm run build
```

### Go
```
# Initialize module
go mod init

# Install dependencies
go mod tidy

# Run
go run main.go

# Test
go test ./...

# Build
go build
```

### Rust
```
# Create new project
cargo new myproject

# Build
cargo build

# Run
cargo run

# Test
cargo test
```

---

## What's Next?

Next up: Module 14 -- API integration and working with external services. You'll connect Claude Code to real-world APIs, which is where a lot of practical development happens.
