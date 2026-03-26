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

### Python Standards That Matter

Beyond just generating code, you want Claude Code to enforce patterns that actually matter in Python codebases. Here are the ones worth encoding into your workflow:

- **Always use type hints**, including return types. Not just for function signatures -- annotate variables when the type isn't obvious. This gives Claude Code (and your IDE) far more to work with.
- **Use `Protocol` for duck typing** instead of abstract base classes where possible. Protocols are more Pythonic and don't force inheritance hierarchies.
- **Context managers for resource cleanup.** If something opens, connects, or acquires -- it should use `with`. Tell Claude Code to wrap database connections, file handles, and locks in context managers every time.
- **List comprehensions over loops**, but only when readability isn't sacrificed. A comprehension that spans three lines is worse than a simple for-loop.
- **pytest fixtures and parametrized tests.** Don't let Claude Code generate `unittest.TestCase` classes. Fixtures for setup/teardown, `@pytest.mark.parametrize` for testing multiple inputs.
- **Django-specific conventions:**
  - ViewSets over function-based views -- they reduce boilerplate and play better with routers
  - Always use serializers for data validation, even for internal APIs
  - Management commands over standalone scripts -- they get Django's ORM and settings for free
- **FastAPI-specific:** Pydantic models for everything, dependency injection for shared resources, async endpoints by default.

You can make these standards persistent by encoding them into `.claude/rules/python/` files. Module 12 covers this in detail, but the short version: create a rule file with these conventions, and Claude Code will follow them automatically whenever it touches Python files.

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

### TypeScript Standards That Matter

TypeScript's type system is powerful, but only if you actually use it. These are the standards that separate clean TypeScript from "JavaScript with extra steps":

- **Strict mode, always.** Enable every strict flag in `tsconfig.json`. No exceptions. Claude Code should never generate code that requires loosening strictness.
- **No `any`.** Use `unknown` and narrow the type with type guards. If Claude Code reaches for `any`, push back. The whole point of TypeScript is the types.
- **Named exports over default exports.** Default exports make refactoring harder and auto-imports less reliable. The only exception is Next.js page components, where default exports are required by the framework.
- **`interface` for object shapes, `type` for unions and intersections.** This isn't just style -- interfaces can be extended and merged, types can't. Use each where it's strongest.
- **Barrel exports only at module boundaries.** A `src/components/index.ts` that re-exports everything is fine. A barrel file in every subdirectory creates circular dependency nightmares.
- **React-specific conventions:**
  - Server Components by default in Next.js App Router -- only add `'use client'` when the component genuinely needs browser APIs or interactivity
  - Props interfaces named `{ComponentName}Props`
  - Custom hooks for shared logic, not utility functions that secretly use React internals
  - Zod schemas for runtime validation at API boundaries

Encode these into `.claude/rules/typescript/` files so they apply automatically. See Module 12 for the full setup.

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

### Go Standards That Matter

Go's simplicity is deceptive. The language is small, but writing idiomatic Go requires discipline. These are the patterns Claude Code should always follow:

- **Always wrap errors with context.** Use `fmt.Errorf("failed to fetch user: %w", err)` -- never return a bare error, and never silently ignore one. The `%w` verb preserves the error chain for `errors.Is` and `errors.As`.
- **Table-driven tests as the default.** Every test function should start with a slice of test cases. This is the single most idiomatic Go testing pattern, and Claude Code should reach for it automatically.
- **Context propagation through all layers.** Every function that does I/O or could block should accept `context.Context` as its first parameter. No exceptions. This enables cancellation, timeouts, and tracing.
- **Goroutine safety: always know who owns the lifecycle.** Before launching a goroutine, answer: who waits for it to finish? What happens if it panics? Use `errgroup` for managing groups of goroutines with proper error collection.
- **Channel patterns: prefer closing channels over signaling.** A closed channel is readable by all receivers simultaneously -- a signal value only reaches one. Use `done` channels or context cancellation for shutdown coordination.
- **Go reviewer mindset.** When asking Claude Code to review Go code, have it check for: exported functions without doc comments, error returns that aren't checked, goroutines without lifecycle management, and missing `defer` for cleanup.

Encode these into `.claude/rules/golang/` files so they're enforced automatically. Module 12 has the details on setting up language-scoped rules.

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

## Lesson 5b: Swift and iOS Development

### SwiftUI Application

Swift and iOS development is another area where Claude Code pulls its weight. The framework APIs are extensive, and having Claude Code generate the boilerplate for views, data models, and persistence saves real time.

```
Create a SwiftUI application with:
- Core Data persistence layer
- async/await for all network calls
- MVVM architecture
- Navigation using NavigationStack
- @Observable macro for state management
- Unit tests with XCTest
```

---

### Swift Standards That Matter

iOS codebases accumulate complexity fast. These patterns keep things manageable:

- **Swift actors for state isolation.** Any shared mutable state should live inside an `actor`. This eliminates data races at compile time instead of debugging them at runtime.
- **Protocol-oriented design over class inheritance.** Define behavior through protocols, provide defaults via extensions. This is how the Swift standard library is built, and your code should follow the same pattern.
- **Dependency injection via protocols for testability.** Never instantiate services directly inside views or view models. Define a protocol, inject the concrete implementation, and swap in mocks for testing.
- **SwiftUI patterns:**
  - Use the `@Observable` macro (not `ObservableObject`) for new code -- it's more efficient and requires less boilerplate
  - Prefer view modifiers over wrapper views for styling and behavior
  - Use `NavigationStack` with typed navigation paths, not the deprecated `NavigationView`
- **Testing with XCTest:** Use `async` test methods for testing async code. Combine `XCTestExpectation` with structured concurrency for integration tests. Keep UI tests focused on critical user flows, not pixel-perfect layout.

Encode Swift-specific conventions into `.claude/rules/swift/` files to keep them consistent across your project. Module 12 covers the setup.

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

## Lesson 8: Setting Up Language-Specific Claude Code Configuration

Everything in this module -- the standards, the patterns, the idioms -- is only useful if Claude Code remembers it between sessions. That's what the `.claude/rules/` directory is for. You organize your conventions by language, and Claude Code loads the relevant ones based on what files you're working in.

Here's the recommended directory structure:

```
.claude/rules/
├── common/
│   ├── coding-style.md    # Universal: naming, file length, DRY
│   ├── testing.md         # 80% coverage, TDD-first, colocated tests
│   └── git-workflow.md    # Conventional commits, branch naming
├── python/
│   └── patterns.md        # Type hints, pytest, Django conventions
├── typescript/
│   └── patterns.md        # Strict mode, named exports, React patterns
└── golang/
    └── patterns.md        # Error wrapping, table-driven tests
```

Each rule file is a short Markdown document that states conventions directly. Here's what a real one looks like:

```markdown
---
description: Python coding standards
globs: ["**/*.py"]
---
- Always use type hints for function parameters and return types
- Use pytest for all tests, never unittest
- Use Protocol instead of ABC for duck typing
- Context managers for all resource cleanup (files, connections, locks)
- Django views: use ViewSets, never function-based views
```

The `path` frontmatter in the YAML header tells Claude Code when to load this rule. A pattern like `**/*.py` means it only activates when working on Python files. The `common/` rules use a broader path (or no path restriction) so they apply everywhere.

Language-specific rules only load when Claude Code is working on matching files. This keeps your context focused -- Go conventions don't clutter the prompt when you're editing TypeScript, and vice versa.

**Exercise:** Create a `.claude/rules/` directory for your primary language. Add one rule file with 5-10 conventions that matter most to your team. Use the path-scoped frontmatter to scope it to the right file types. Then start a Claude Code session and verify the rules are being followed.

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
- [ ] Know language-specific standards for Python, TypeScript, Go
- [ ] Can set up Claude Code rules for a specific language
- [ ] Understand Swift/iOS development with Claude Code

---

## Language-Specific Commands

### Python
```
# Create virtual environment
python -m venv venv

# Activate (Linux/Mac)
source venv/bin/activate

# Activate (Windows PowerShell)
.\venv\Scripts\Activate.ps1

# Activate (Windows CMD)
venv\Scripts\activate.bat

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

> **Build real projects in any language.** The [Real Projects Pack](https://payhip.com/b/dFXWO) includes multi-language projects -- from TypeScript microservices to Python-powered code analysis tools -- each with Claude Code workflows tailored to the language.

Next up: Module 14 -- API integration and working with external services. You'll connect Claude Code to real-world APIs, which is where a lot of practical development happens.
