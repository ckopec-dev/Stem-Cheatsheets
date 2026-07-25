# A Deep Dive: Using Cline Effectively for Advanced Python, Flask, and Docker Projects

For advanced projects, the biggest mistake is treating Cline as a faster autocomplete tool. Its real value is closer to a **software-development agent that can investigate a codebase, formulate a plan, modify files, run tools, test the result, and iterate**.

For a Python/Flask/Docker project, Cline can work across:

* Python application code
* Flask routes and application factories
* SQLAlchemy models and migrations
* REST APIs
* Celery or background workers
* Dockerfiles
* Docker Compose
* Database containers
* pytest
* linting and formatting
* browser testing
* CI/CD configuration
* cloud deployment configuration

The central principle is:

> **Do not ask Cline to “write a feature.” Ask it to manage a controlled engineering process that produces a feature.**

---

# 1. The Core Workflow: Plan → Act → Verify

The ideal workflow is:

```text
Goal
  ↓
Codebase exploration
  ↓
Architecture analysis
  ↓
Implementation plan
  ↓
Human review
  ↓
Incremental changes
  ↓
Docker build / pytest / linting
  ↓
Observe failures
  ↓
Diagnose
  ↓
Modify
  ↓
Re-test
  ↓
Review
  ↓
Commit
```

For a Flask project, that might look like:

```text
┌────────────────────┐
│ User requirement   │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│ PLAN MODE          │
│                    │
│ Explore Flask app  │
│ Inspect models     │
│ Inspect Docker     │
│ Analyze tests      │
│ Design solution    │
└─────────┬──────────┘
          ↓
      Human review
          ↓
┌────────────────────┐
│ ACT MODE           │
│                    │
│ Modify Python      │
│ Add tests          │
│ Build Docker image │
│ Run test suite     │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│ Review              │
│                    │
│ Diff                │
│ Security            │
│ Architecture       │
│ Deployment          │
└────────────────────┘
```

---

# 2. Make the Repository Explain Itself

A good Python/Flask project should contain explicit project knowledge.

For example:

```text
myproject/
├── .clinerules/
│   ├── 01-project-overview.md
│   ├── 02-architecture.md
│   ├── 03-python.md
│   ├── 04-flask.md
│   ├── 05-testing.md
│   ├── 06-database.md
│   ├── 07-docker.md
│   └── 08-security.md
│
├── docs/
│   ├── architecture/
│   ├── decisions/
│   ├── development/
│   └── operations/
│
├── app/
│   ├── __init__.py
│   ├── config.py
│   ├── extensions.py
│   ├── models/
│   ├── routes/
│   ├── services/
│   ├── repositories/
│   └── tasks/
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── migrations/
├── scripts/
├── Dockerfile
├── docker-compose.yml
├── pyproject.toml
├── pytest.ini
└── README.md
```

The exact structure is not mandatory. The important thing is that the structure and architectural rules are explicit.

---

# 3. Example `.clinerules` for Python and Flask

## `03-python.md`

```text
# Python Development Rules

- Use Python 3.12 unless the project explicitly specifies another version.
- Use type hints for public functions and methods.
- Prefer clear, explicit code over clever one-liners.
- Do not add dependencies without explaining why they are necessary.
- Use the project's existing dependency-management system.
- Do not silently change public APIs.
- Avoid global mutable state.
- Use context managers for resources that require cleanup.
- Do not catch broad Exception unless the exception is intentionally handled,
  translated, logged, and re-raised where appropriate.
```

## `04-flask.md`

```text
# Flask Architecture Rules

- Use the Flask application factory pattern.
- Routes should be thin.
- Business logic belongs in service modules.
- Database access belongs in repositories or dedicated persistence modules.
- Do not place substantial business logic directly in route functions.
- Do not access the database through global connection objects.
- Use Flask extensions through the existing application initialization pattern.
- Keep configuration environment-driven.
- Do not store secrets in source code.
```

## `05-testing.md`

```text
# Testing Rules

- Use pytest.
- New business behavior requires tests.
- Prefer unit tests for isolated business logic.
- Use integration tests for database behavior.
- Use Flask's test client for HTTP endpoint testing.
- Tests must not depend on production services.
- External APIs must be mocked or replaced with test doubles.
- Do not weaken tests merely to make an implementation pass.
```

## `07-docker.md`

```text
# Docker Rules

- The application must run reproducibly in Docker.
- Do not install development-only dependencies in the production image.
- Do not embed secrets in Dockerfiles.
- Use environment variables or secret-management mechanisms.
- Prefer multi-stage builds where appropriate.
- Containers should run as a non-root user where practical.
- Health checks should be defined for long-running services.
- Docker Compose should be used for local multi-service development where appropriate.
```

These rules eliminate the need to repeatedly tell Cline:

> “Please use an application factory.”

or:

> “Don't put all the business logic in the Flask route.”

---

# 4. Give Cline Tasks, Not Vague Objectives

Weak:

> Add authentication.

Strong:

```text
Implement authentication for the existing Flask application.

Requirements:
- Preserve the existing user model.
- Use the current database and migration system.
- Follow the existing application factory pattern.
- Support login, logout, and role-based authorization.
- Add unit tests for authentication services.
- Add integration tests for protected endpoints.
- Do not introduce a new authentication framework without first explaining why.
- Do not store credentials in source code.
- Begin in Plan mode.
- First inspect the current user model, application factory,
  configuration, routes, and authorization logic.
```

The second prompt gives Cline:

1. An objective
2. Constraints
3. Architectural boundaries
4. Testing requirements
5. A process

---

# 5. Use a Strong Prompt Structure

For large Python projects:

```text
## Objective

What should exist when finished?

## Current Situation

What currently exists?

## Constraints

What must not change?

## Requirements

What behavior is required?

## Architecture

Where should the changes live?

## Data

What database/schema changes are needed?

## Docker

What container or deployment changes are needed?

## Testing

What tests are required?

## Acceptance Criteria

How do we know this is complete?

## Process

How should Cline work?
```

For example:

```text
## Objective

Add a Salesforce synchronization subsystem to the Flask application.

## Current Situation

The application:
- Uses Flask
- Uses SQLAlchemy
- Runs in Docker
- Uses PostgreSQL
- Has existing background task infrastructure
- Already contains customer records

## Requirements

Implement:
- OAuth token management
- Salesforce API client
- Account synchronization
- Retry handling
- Incremental synchronization
- Synchronization history
- Error tracking

## Constraints

- Do not put Salesforce logic in Flask route functions.
- Do not expose Salesforce credentials to the browser.
- Follow the existing service and repository architecture.
- Use the existing HTTP client abstraction if one exists.
- Do not introduce a second database abstraction.

## Docker

- The development environment must run through Docker Compose.
- The application container must be able to communicate with the worker and database services.

## Testing

Add:
- Unit tests for token handling
- Unit tests for mapping
- Retry behavior tests
- Integration tests for synchronization
- API endpoint tests if administrative endpoints are added

## Acceptance Criteria

- pytest passes.
- The Docker image builds successfully.
- The application starts successfully using Docker Compose.
- Failed records are retryable.
- Synchronization activity is observable through logs and database records.

## Process

Begin in Plan mode.
Explore the existing architecture first.
Do not modify files until the implementation plan is approved.
```

---

# 6. Explore Before Building

For an unfamiliar Flask project, begin with:

```text
Do not modify anything.

Analyze this repository and produce an architectural map.

Identify:
- Flask application entry points
- Application factory
- Configuration system
- Blueprints
- Database initialization
- SQLAlchemy models
- Migration system
- Service layer
- Repository layer
- Background workers
- External integrations
- Authentication and authorization
- Logging
- Test projects
- Docker configuration
- Docker Compose services
- CI/CD configuration
- Architectural inconsistencies
- High-risk areas
```

This lets Cline understand the application before modifying it.

For example, a typical Flask architecture might look like:

```text
HTTP Request
     │
     ▼
Flask Blueprint
     │
     ▼
Route Handler
     │
     ▼
Service Layer
     │
     ├──────────────┐
     ▼              ▼
Repository      External API
     │
     ▼
SQLAlchemy
     │
     ▼
PostgreSQL
```

Cline should understand this flow before adding new features.

---

# 7. Use Subagents for Parallel Investigation

A useful Cline task:

```text
Use subagents to independently investigate:

1. Flask application initialization
2. Database and migrations
3. External API integrations
4. Background processing
5. Docker and deployment
6. Test architecture

Do not modify files.

For each area, report:
- Relevant files
- Main abstractions
- Data flow
- Important dependencies
- Potential architectural problems
- Recommendations
```

The main Cline agent can then synthesize the results into a plan.

This is especially useful for a large Python project because different concerns can often be investigated independently.

---

# 8. Context Management

Do not give Cline the entire repository every time.

A better sequence is:

```text
Phase 1:
Analyze repository structure

Phase 2:
Analyze Flask architecture

Phase 3:
Analyze the relevant service

Phase 4:
Analyze the relevant database models

Phase 5:
Implement the feature
```

For example:

```text
First understand the entire architecture.

Now focus specifically on:

app/routes/
app/services/
app/repositories/
app/models/
tests/

Now investigate only the synchronization architecture.

Now implement the first slice.
```

The goal is to provide:

> **Enough context to understand the problem, but not so much that the important details become buried.**

---

# 9. Break Large Features Into Vertical Slices

Suppose you are building:

```text
Salesforce Integration
```

Do not ask:

```text
Build the entire Salesforce integration.
```

Break it into:

```text
Slice 1: Configuration
Slice 2: OAuth token management
Slice 3: Salesforce API client
Slice 4: Account retrieval
Slice 5: Data mapping
Slice 6: Database persistence
Slice 7: Incremental synchronization
Slice 8: Retry handling
Slice 9: Observability
Slice 10: Administrative API
```

Each slice should follow:

```text
Requirements
     ↓
Implementation
     ↓
Tests
     ↓
Docker build/test
     ↓
Review
     ↓
Checkpoint
```

This creates small, recoverable units of work.

---

# 10. Docker Should Be Part of the Development Loop

For a serious Flask project, Cline should not only test Python code on the host machine.

It should also test the actual containerized environment.

A useful verification loop is:

```text
Edit Python code
      ↓
pytest
      ↓
ruff / mypy
      ↓
docker build
      ↓
docker compose up
      ↓
Health check
      ↓
Integration tests
      ↓
Browser/API testing
```

For example:

```text
After implementing this feature:

1. Run the relevant pytest tests.
2. Run the full pytest suite.
3. Run the project's linting tools.
4. Run type checking if configured.
5. Build the Docker image.
6. Start the Docker Compose environment.
7. Verify the application health endpoint.
8. Run integration tests against the containerized application.
9. Report every command and its result.
```

This catches problems such as:

* Missing Python dependencies
* Incorrect Docker paths
* Environment variable problems
* Import errors
* Network configuration problems
* Incorrect service names
* Database startup timing problems
* File permission problems
* Differences between local and container environments

---

# 11. Example Docker Project Structure

A production-oriented Flask project might look like:

```text
project/
├── app/
│   ├── __init__.py
│   ├── config.py
│   ├── extensions.py
│   ├── models/
│   ├── routes/
│   ├── services/
│   └── repositories/
│
├── tests/
├── migrations/
├── requirements/
│   ├── base.txt
│   ├── development.txt
│   └── production.txt
│
├── Dockerfile
├── docker-compose.yml
├── docker-compose.test.yml
├── pyproject.toml
└── wsgi.py
```

A typical multi-stage Docker workflow might be:

```text
Development
     │
     ▼
Docker Compose
     │
     ├── Flask application
     ├── PostgreSQL
     ├── Redis
     └── Worker
```

For a background-processing architecture:

```text
                  ┌──────────────┐
                  │    Flask     │
                  │   Web App    │
                  └──────┬───────┘
                         │
                         ▼
                    Redis Queue
                    /          \
                   ▼            ▼
             Celery Worker   Celery Beat
                   │
                   ▼
              PostgreSQL
```

Cline should understand the complete service topology before modifying Docker Compose.

---

# 12. Test-Driven Development Works Extremely Well

For complex Python logic:

## Step 1 — Plan

```text
Analyze how this feature should work.
Do not modify files.
```

## Step 2 — Define behavior

```text
Convert the requirements into concrete test cases.
```

## Step 3 — Write tests

```text
Implement the tests only.
Do not implement the production code.
```

## Step 4 — Confirm failure

```text
Run the tests and confirm that they fail for the expected reason.
```

## Step 5 — Implement

```text
Implement the minimum production code required to satisfy the tests.
```

## Step 6 — Refactor

```text
Improve the implementation without changing behavior.
Run all tests afterward.
```

This is especially useful for:

* Data transformations
* REST API behavior
* Authentication
* Authorization
* Retry logic
* Synchronization
* Scheduling
* Parsers
* State machines
* Complex algorithms

---

# 13. Flask Route Testing

For a Flask endpoint, ask Cline to test behavior rather than simply inspect code.

For example:

```text
Implement the customer creation endpoint.

Before implementation, define tests for:

1. Valid customer creation.
2. Missing required fields.
3. Invalid email address.
4. Duplicate customer.
5. Unauthorized request.
6. Database failure.
7. Unexpected service failure.

Use Flask's test client for HTTP behavior.
Keep business logic tests separate from route tests.
```

A typical test structure might be:

```text
tests/
├── unit/
│   ├── test_customer_service.py
│   └── test_customer_validation.py
│
├── integration/
│   ├── test_customer_repository.py
│   └── test_customer_database.py
│
└── api/
    └── test_customer_routes.py
```

The important principle is:

```text
Route test
    ↓
HTTP behavior

Service test
    ↓
Business behavior

Repository test
    ↓
Persistence behavior
```

---

# 14. Use Cline as a Debugging Agent

A strong debugging prompt is:

```text
This bug is reproducible.

Do not immediately modify the code.

First:

1. Reproduce the failure.
2. Trace the execution path.
3. Identify the first point where actual behavior diverges from expected behavior.
4. Form at least two hypotheses.
5. Test the hypotheses.
6. Explain the root cause.
7. Only then propose a fix.
8. Add a regression test.
```

For a Docker problem:

```text
The application works outside Docker but fails inside the container.

Do not immediately modify the Dockerfile.

First:
1. Reproduce the failure.
2. Inspect the container logs.
3. Inspect environment variables.
4. Inspect installed Python dependencies.
5. Compare host and container runtime assumptions.
6. Identify the root cause.
7. Propose the smallest appropriate fix.
```

This prevents random Dockerfile edits.

---

# 15. Use Browser and API Testing

For Flask applications, Cline can test:

```text
Browser
   ↓
Flask route
   ↓
Service layer
   ↓
Database
```

A useful workflow:

```text
Start Docker Compose
        ↓
Wait for health check
        ↓
Open application
        ↓
Log in
        ↓
Navigate to feature
        ↓
Submit form
        ↓
Verify result
        ↓
Inspect logs
        ↓
Run tests
```

For API-focused applications:

```text
Start containers
      ↓
POST /api/customers
      ↓
GET /api/customers/{id}
      ↓
Verify response
      ↓
Inspect database
      ↓
Test error cases
```

A good prompt:

```text
Start the application using Docker Compose.

Test the complete customer creation workflow through the API.

Verify:
- Authentication
- Request validation
- Database persistence
- Response format
- Error handling

If anything fails, diagnose the root cause and fix it.
```

---

# 16. Use MCP to Connect Cline to Your Development Environment

For a serious development workflow, Cline can potentially interact with:

```text
┌─────────────┐
│    Cline    │
└──────┬──────┘
       │
       ├── Files
       ├── Terminal
       ├── Browser
       ├── PostgreSQL
       ├── GitHub
       ├── Jira
       ├── Docker
       ├── CI/CD
       └── Cloud services
```

A useful MCP setup might allow Cline to:

```text
Inspect issue
      ↓
Inspect repository
      ↓
Inspect database schema
      ↓
Modify code
      ↓
Build Docker image
      ↓
Run tests
      ↓
Inspect CI failures
```

However:

> **Do not expose every system to Cline just because an MCP integration exists.**

Give it the minimum capabilities needed.

---

# 17. Build Project-Specific Tools

For example, your repository might contain:

```text
scripts/
├── reset_test_database.py
├── seed_test_data.py
├── run_integration_tests.py
├── inspect_events.py
└── create_test_user.py
```

Rather than allowing Cline to invent complex commands, expose safe, well-defined operations.

Conceptually:

```text
reset_test_database()
seed_test_data(customer_count=100)
run_integration_tests()
create_test_user(role="admin")
```

This is much safer than asking an AI agent to construct arbitrary infrastructure commands.

The principle is:

> **Expose capabilities, not arbitrary complexity.**

---

# 18. Hooks: Deterministic Guardrails

Hooks are especially useful for Python/Docker projects.

Possible policies:

```text
Reject:
- Secrets added to source code
- Changes to production Docker configuration
- Running destructive database commands
- Installing dependencies without approval
- Modifying migration files without tests
- Running commands against production
```

Conceptually:

```text
Cline wants to execute command
          ↓
        Hook
          ↓
   Policy evaluation
       ↙       ↘
    Allowed    Blocked
       ↓          ↓
   Execute     Explain
```

For example:

```text
if command contains "docker compose down -v":
    require approval

if file contains "SECRET_KEY =":
    reject

if a new package is added:
    require approval

if a migration is created:
    require database-test verification
```

The distinction is important:

> **Prompts express intent. Hooks enforce policy.**

---

# 19. Use Separate Architect, Implementer, and Reviewer Roles

A very effective workflow is:

## Architect

```text
Analyze the feature.

Do not modify files.

Produce:
- Architecture
- Data flow
- Risks
- Alternatives
- Implementation plan
```

## Implementer

```text
Implement the approved plan.

Follow the existing Flask architecture.
Add tests.
Run pytest.
Build the Docker image.
```

## Reviewer

```text
Review the resulting diff.

Look for:
- Bugs
- Security problems
- Architectural violations
- Missing tests
- Docker problems
- Performance issues
```

This reduces the tendency of one agent to create and approve its own work.

---

# 20. A Practical Advanced Workflow for Python + Flask + Docker

## Phase 1: Prepare the repository

Create:

```text
.clinerules/
├── 01-project.md
├── 02-architecture.md
├── 03-python.md
├── 04-flask.md
├── 05-testing.md
├── 06-database.md
├── 07-docker.md
└── 08-security.md
```

---

## Phase 2: Create a project map

```text
Analyze this repository.

Do not modify files.

Produce:
1. Project structure
2. Flask application startup flow
3. Blueprint structure
4. Dependency graph
5. Database architecture
6. Migration system
7. External integrations
8. Background processing
9. Docker architecture
10. Test architecture
11. CI/CD process
12. Architectural risks
```

---

## Phase 3: Define the feature

```text
We need to implement [FEATURE].

Before modifying files:

- Identify affected modules.
- Identify affected database models.
- Identify API changes.
- Identify Docker changes.
- Identify tests required.
- Identify deployment concerns.
- Identify backward compatibility concerns.

Create an implementation plan.
```

---

## Phase 4: Challenge the plan

```text
Challenge this plan.

Look for:
- Unnecessary complexity
- Violations of Flask architecture
- Missing failure modes
- Security problems
- Database migration risks
- Docker networking problems
- Testing gaps
- Performance problems
```

---

## Phase 5: Implement one vertical slice

```text
Implement Phase 1 only.

After implementation:

1. Run pytest.
2. Run linting.
3. Run type checking if configured.
4. Build the Docker image.
5. Start the Docker Compose environment.
6. Run relevant integration tests.
7. Fix failures.
8. Report all changed files.
9. Report all commands run.
10. Report anything not verified.
```

---

## Phase 6: Review

Use a separate review pass:

```text
Review the current implementation.

Do not modify files.

Analyze:
- Correctness
- Flask architecture
- Python quality
- Security
- Docker configuration
- Database behavior
- Error handling
- Test coverage
```

---

## Phase 7: Apply fixes

```text
Apply only the confirmed review findings.

Do not make unrelated refactors.

Run:
pytest
ruff check .
ruff format --check .
docker build .
docker compose up
```

---

# 21. Database Work Requires Extra Caution

For a Flask application using SQLAlchemy and Alembic, use:

```text
1. Inspect current models
2. Inspect existing migrations
3. Understand existing data
4. Design schema change
5. Identify migration risks
6. Create migration
7. Update application code
8. Add tests
9. Test against a clean database
10. Test against an existing database
```

Prompt:

```text
We need to add [DATABASE CHANGE].

Do not modify anything yet.

Analyze:
- Current SQLAlchemy models
- Existing Alembic migrations
- Foreign keys
- Indexes
- Existing data
- Nullability implications
- Migration risks
- Backward compatibility

Propose a migration strategy.
```

Then:

```text
Implement the approved migration.

Before declaring completion:

- Run the application tests.
- Test the migration against a clean PostgreSQL database.
- Test it against an existing database if possible.
- Verify the generated migration carefully.
- Build and run the Docker environment.
```

---

# 22. Security Reviews

For Flask applications, explicitly ask Cline to inspect:

```text
- Authentication
- Authorization
- Session handling
- CSRF
- XSS
- SQL injection
- Input validation
- File uploads
- SSRF
- Secrets
- JWT handling
- CORS
- Debug mode
- Error disclosure
- Logging of sensitive data
- Docker container privileges
- Dependency vulnerabilities
```

A useful prompt:

```text
Perform a security review of this Flask application.

Do not modify files.

Inspect:
- Authentication
- Authorization
- Session handling
- Secrets
- SQL queries
- Input validation
- File uploads
- SSRF risks
- XSS
- CSRF
- CORS
- Debug configuration
- Logging of sensitive data
- Docker security
- Dependency risks

Rank findings:
Critical
High
Medium
Low

For each finding:
- File
- Code location
- Problem
- Exploit scenario
- Recommended fix
```

---

# 23. Use Docker as an Environment Boundary

One of the biggest advantages of Docker is that it gives Cline a consistent execution environment.

For example:

```text
Host
 │
 └── Docker Compose
      │
      ├── Flask
      ├── PostgreSQL
      ├── Redis
      └── Celery Worker
```

Instead of:

```text
Developer machine
├── Python version A
├── PostgreSQL version B
├── Redis version C
└── Environment variable differences
```

You can tell Cline:

```text
All application verification should occur inside the Docker environment
unless there is a specific reason to run a command on the host.
```

This is especially valuable for catching:

* Missing system libraries
* Incorrect Python versions
* Missing dependencies
* Environment variable problems
* Network configuration problems
* File permission issues

---

# 24. The Most Important Cline Prompt for Docker

```text
Do not assume that a successful host-machine test means the application works in Docker.

After making changes:

1. Build the Docker image.
2. Start all required services with Docker Compose.
3. Wait for dependencies to become healthy.
4. Verify the Flask health endpoint.
5. Run the relevant tests against the containerized environment.
6. Inspect logs if any service fails.
7. Fix the root cause rather than masking the error.
```

This should be part of your project's Docker rules.

---

# 25. The Three Biggest Mistakes

## Mistake 1: Huge vague tasks

Bad:

```text
Build my entire Flask application.
```

Better:

```text
Analyze the application.
Create an architecture plan.
Implement the first vertical slice.
Verify it.
```

---

## Mistake 2: Too much autonomy too soon

Bad:

```text
Unfamiliar repository
+
full autonomous execution
+
production credentials
+
database access
+
no tests
```

Better:

```text
Git branch
+
checkpoint
+
Plan mode
+
limited permissions
+
automated tests
+
Docker isolation
```

---

## Mistake 3: Assuming “it runs” means “it works”

A Flask application can:

```text
Start successfully
```

and still:

```text
Have incorrect business logic
Return incorrect HTTP status codes
Have security vulnerabilities
Fail under concurrency
Lose database transactions
Have broken migrations
Fail inside Docker
Have inadequate tests
```

The solution is a verification process.

---

# 26. My Recommended Operating Model

For serious Python/Flask/Docker projects:

```text
1. Maintain a strong .clinerules/ directory.

2. Start every non-trivial task in Plan mode.

3. Have Cline inspect the existing Flask architecture before coding.

4. Use subagents for broad, parallel read-only investigation.

5. Break large work into vertical slices.

6. Use checkpoints before major implementation steps.

7. Have Cline write tests early.

8. Require pytest before completion.

9. Require linting and type checking where applicable.

10. Require Docker image builds.

11. Require Docker Compose integration testing for multi-service features.

12. Use a separate review pass after implementation.

13. Use MCP only for capabilities that genuinely need external systems.

14. Use hooks for deterministic security and policy enforcement.

15. Keep Git as the durable source of truth.

16. Increase autonomy only as the project gains safety mechanisms.
```

The biggest conceptual shift is:

> **Cline should not replace your Python/Flask/Docker software engineering process. It should become the execution engine inside a better software engineering process.**

For your stack specifically, the most important feedback loop is:

```text
Requirement
    ↓
Plan
    ↓
Python implementation
    ↓
pytest
    ↓
Lint/type checks
    ↓
Docker build
    ↓
Docker Compose
    ↓
Integration tests
    ↓
Browser/API verification
    ↓
Review
    ↓
Git
```

That is the workflow I would use for advanced Flask applications, data integration systems, Salesforce integrations, background processing systems, and multi-container applications.
