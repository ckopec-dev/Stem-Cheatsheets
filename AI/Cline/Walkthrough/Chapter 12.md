# Part VI

# Chapter 12

# Docker Development with Cline

One of the biggest misconceptions about Docker is that it's a deployment tool.

It isn't.

Docker is a **development environment**.

Production deployment is merely one consequence.

For AI-assisted development, Docker becomes even more valuable because it gives Cline something extremely important:

> A predictable environment.

Predictable environments produce predictable results.

That means fewer debugging sessions and fewer "it works on my machine" problems.

---

# Our Goal

By the end of this chapter our project should run with a single command.

```bash
docker compose up
```

A browser should display

```
http://localhost:5000
```

and the Mandelbrot application should work exactly as it did outside Docker.

---

# Current Project

Our project now looks something like this:

```text
mandelbrot-demo/

app.py
mandelbrot.py
requirements.txt
.clinerules

templates/

tests/

.gitignore
```

Time to containerize it.

---

# The Beginner Prompt

Most people ask:

```text
Create a Dockerfile.
```

That's not an engineering task.

That's a typing task.

Instead, ask Cline to think.

---

# Conversation 1

## Planning

Prompt:

```text
Inspect the project.

Recommend a Docker strategy.

Discuss

• base image

• dependency installation

• image size

• development workflow

Recommend one design.

Wait for approval.
```

No code.

Just engineering.

---

# Expected Discussion

A good answer should include topics like

```
python:3.12-slim

↓

requirements.txt

↓

application files

↓

non-root user

↓

run Flask
```

Notice

We're discussing architecture.

Not syntax.

---

# Why "Slim"?

A common discussion might compare:

```
python:3.12
```

versus

```
python:3.12-slim
```

Ask Cline

```text
Compare these images.

Discuss

• image size

• compatibility

• security

• build speed

Recommend one.
```

This teaches Docker,

not Dockerfiles.

---

# Conversation 2

## Dockerfile Design Review

Before implementation

Prompt:

```text
Show the proposed Dockerfile.

Explain every instruction.

Explain why the instructions appear in that order.

Do not create files yet.
```

This is incredibly valuable.

Many Docker tutorials simply present a finished file.

We're learning *why*.

---

# Layer Ordering

One of the first things Cline should discuss is build caching.

For example,

instead of

```
COPY .

RUN pip install
```

we prefer

```
COPY requirements.txt

RUN pip install

COPY .
```

Why?

Because dependency installation rarely changes.

Docker can reuse that layer.

Builds become much faster.

---

# Asking Better Questions

Instead of

```
Why did you copy requirements first?
```

Ask

```text
Explain how Docker layer caching affects rebuild performance.

Estimate which project changes invalidate each layer.
```

Now Cline becomes a Docker instructor.

---

# Conversation 3

## Implementation

Prompt:

```text
Implement the approved Dockerfile.

Requirements

• Follow .clinerules

• Keep it readable

• Use a non-root user

• Explain every decision

• Build the image

• Verify success

Stop afterward.
```

Notice

Implementation

Verification

Stop.

---

# Watching Cline Work

A typical sequence looks like

```
Read requirements.txt

↓

Create Dockerfile

↓

docker build

↓

Analyze output

↓

Verify image
```

This is exactly what we want.

---

# Don't Skip Verification

Many developers stop after

```
docker build
```

Instead ask

```text
Run the container.

Verify the Flask application starts correctly.

Document exactly what was tested.
```

Verification matters.

---

# Introducing Docker Compose

Now we want a reproducible development workflow.

Again

Don't ask

```
Create docker-compose.yml
```

Instead ask

```text
Recommend whether Docker Compose is appropriate for this project.

Explain the advantages and disadvantages.

Wait for approval.
```

Notice

We're evaluating.

Not assuming.

---

# Why Compose?

Today

our application has one container.

Tomorrow it might have

```
Flask

Redis

Worker

Database

Nginx
```

Compose scales naturally.

---

# Implementing Compose

Prompt

```text
Create a minimal Docker Compose configuration.

Support

• development

• automatic restart

• port mapping

Keep it intentionally simple.

Verify it works.
```

Again

Goal

Constraints

Verification.

---

# Volume Mounts

One of the best discussions to have with Cline concerns bind mounts.

Ask

```text
Compare

copying the source code into the image

versus

using bind mounts during development.

Recommend one.
```

This is where Docker becomes enjoyable.

---

# Development vs Production

Experienced engineers usually maintain separate strategies.

Ask Cline

```text
Explain how the Docker configuration should differ between

development

and

production.

Do not modify files.
```

You'll likely get discussion around:

* bind mounts
* debug mode
* image size
* logging
* environment variables

Excellent.

---

# Environment Variables

Avoid hard-coded configuration.

Prompt

```text
Recommend which application settings should become environment variables.

Explain why.

Do not implement yet.
```

You're planning for growth.

---

# Debugging Docker Builds

Suppose

```
docker build
```

fails.

Don't ask

```
Fix it.
```

Ask

```text
Inspect the build output.

Identify

the first failing step

the underlying cause

possible fixes

confidence level

Do not modify files.
```

Evidence first.

---

# Optimizing Image Size

One of my favorite prompts

```text
Review the Docker image.

Identify

the largest contributors to image size.

Recommend optimizations ranked by impact.

Do not implement.
```

Now Cline becomes a performance engineer.

---

# Multi-Stage Builds

We're not using one yet.

That's intentional.

Ask

```text
Would a multi-stage build benefit this project?

Why or why not?

Estimate the expected improvement.
```

Notice

We're resisting complexity until it becomes justified.

---

# Using Cline to Explain Docker

Suppose you see

```dockerfile
WORKDIR /app
```

Instead of searching Google,

ask

```text
Explain

WORKDIR

COPY

CMD

ENTRYPOINT

using this Dockerfile as context.
```

This creates project-specific learning.

---

# Complete Conversation Example

A professional workflow might look like this:

### Planning

```text
Inspect the project.

Recommend a Docker development strategy.

Wait for approval.
```

↓

### Review

```text
Show the proposed Dockerfile.

Explain every instruction.

Do not create files.
```

↓

### Implementation

```text
Implement the approved Dockerfile.

Build it.

Verify success.
```

↓

### Compose

```text
Add Docker Compose.

Verify the application starts correctly.
```

↓

### Review

```text
Review the Docker configuration.

Identify future improvements.

Do not modify code.
```

Every step has one purpose.

---

# My Docker Workflow with Cline

For almost every new project, I follow a consistent pattern:

1. Build and verify the application locally.
2. Ask Cline to recommend a containerization strategy.
3. Review the proposed Dockerfile before accepting it.
4. Build the image and verify it.
5. Introduce Docker Compose only when it improves the workflow.
6. Review the final configuration for maintainability and security.
7. Commit the completed work.

The result is a development environment that is reproducible, easy to understand, and evolves naturally with the project.

---

# Chapter Summary

Docker is far more than a deployment mechanism—it's a way to create a stable, repeatable development environment that makes Cline more effective. By separating planning, design review, implementation, verification, and optimization into distinct conversations, you gain a much deeper understanding of both Docker and your application. Rather than memorizing `Dockerfile` syntax, you learn to reason about build layers, caching, environment configuration, and long-term maintainability.

