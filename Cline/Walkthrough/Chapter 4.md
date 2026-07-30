# Part II

# Chapter 4

# Mastering `.clinerules`

If I could teach only **one** advanced Cline feature, it would be `.clinerules`.

Most people think prompt engineering is the key to using Cline effectively.

It isn't.

The real secret is giving Cline a **persistent engineering handbook** that it automatically consults every time it works on your project.

Think of `.clinerules` as the equivalent of a senior engineer telling every new team member:

> "This is how we build software here."

Without it, every conversation starts from scratch.

With it, Cline gradually becomes an engineer who understands **your project**.

---

# What is `.clinerules`?

`.clinerules` is not a configuration file.

It is not source code.

It is not documentation.

Instead, think of it as:

* project standards
* engineering philosophy
* architectural constraints
* workflow expectations
* review criteria

The file answers questions like:

* How should code be organized?
* What libraries should be preferred?
* What coding style should be used?
* How should Cline behave before making changes?
* How much explanation should it provide?

---

# The Biggest Beginner Mistake

Beginners write rules like this:

```text
Always use Flask.

Always use NumPy.

Always use pathlib.

Always use Docker.

Always use logging.

Always use Bootstrap.

Always use dataclasses.
```

This is merely a checklist.

It doesn't help Cline make engineering decisions.

---

# A Better Philosophy

Instead, describe the project.

```text
This project emphasizes readability over cleverness.

Small commits are preferred.

Avoid unnecessary dependencies.

Favor explicit code over abstraction.

Optimize for maintainability.
```

Notice the difference.

These are values.

Not instructions.

---

# Building Our Rules Incrementally

We'll evolve our `.clinerules` throughout the project.

Version 1 is intentionally small.

```text
Project: Mandelbrot Demo

Goal:
Build a simple educational Flask application.

Requirements:

• Python 3.12

• Flask

• Docker

• Separate rendering logic from web logic.

• Keep code readable.

• Explain significant changes before editing.

• Prefer small incremental tasks.
```

That's enough to begin.

---

# Why Short Rules Work Better

Many people create a 500-line `.clinerules` file on day one.

That's a mistake.

Large rule files often contain:

* contradictions
* duplicated instructions
* obsolete requirements
* implementation details

Instead, let the rules evolve alongside the project.

---

# Organizing Rules into Sections

As the project grows, organization becomes important.

I recommend something like:

```text
Project

Architecture

Code Style

Dependencies

Testing

Docker

Git

Review Process

Documentation

Performance

Security

Future Considerations
```

Each section has a distinct purpose.

---

# Project Section

Describe the application.

```text
Project

This application demonstrates Mandelbrot rendering using Flask.

Educational value is more important than feature count.

Readability is more important than optimization.

Changes should preserve simplicity.
```

Notice we're teaching Cline **why** the project exists.

---

# Architecture Section

This is arguably the most valuable part.

```text
Architecture

Keep rendering independent of Flask.

Business logic must not import Flask.

The web layer should only coordinate requests.

Avoid circular dependencies.

Favor composition over inheritance.
```

These rules continue to guide Cline months later.

---

# Why Architecture Rules Matter

Suppose later we ask:

> Add an API endpoint.

Without architecture rules, Cline might write:

```python
@app.route("/render")
def render():
    # 250 lines of Mandelbrot code here...
```

With the rules, it's much more likely to create:

```text
Flask

↓

Renderer

↓

Image Generator
```

instead of a giant route handler.

---

# Style Rules

Keep these practical.

```text
Style

Add type hints.

Use descriptive names.

Keep functions short.

Avoid nested conditionals.

Document non-obvious algorithms.

Prefer clarity over brevity.
```

Notice we didn't specify a maximum line length.

That's usually better handled by a formatter.

---

# Dependency Rules

This section prevents dependency creep.

```text
Dependencies

Prefer the standard library.

Only introduce dependencies with clear justification.

Remove unused packages promptly.
```

One sentence here can prevent years of technical debt.

---

# Workflow Rules

These dramatically improve collaboration.

```text
Workflow

Before changing code:

Inspect relevant files.

Explain the proposed approach.

Wait for approval when making major changes.

Summarize completed work afterward.
```

This transforms Cline from an eager code generator into a thoughtful collaborator.

---

# Review Rules

These are my favorites.

```text
Review

After implementing a task:

Review your own work.

Identify possible improvements.

Mention tradeoffs.

Call out technical debt.

Recommend the next logical task.
```

You're essentially asking Cline to think like a senior engineer performing a pull request review.

---

# Documentation Rules

```text
Documentation

Explain architectural decisions.

Avoid redundant comments.

Document algorithms rather than syntax.

Update README when appropriate.
```

Notice that we don't ask for comments everywhere.

Good code rarely needs them.

---

# Git Rules

```text
Git

Prefer small commits.

Suggest meaningful commit messages.

Avoid unrelated changes in one commit.
```

This subtly encourages disciplined version control.

---

# Docker Rules

```text
Docker

Use slim base images.

Run as a non-root user.

Minimize image size.

Separate build and runtime stages when appropriate.
```

When we eventually ask Cline to create a Dockerfile, these expectations are already established.

---

# Performance Rules

Performance advice should be specific to the project.

```text
Performance

Correctness before optimization.

Avoid premature optimization.

Optimize only after measuring.

Keep rendering deterministic.
```

This discourages unnecessary complexity.

---

# Security Rules

Even toy projects benefit from basic security guidance.

```text
Security

Never hard-code secrets.

Use environment variables.

Validate external input.

Avoid unnecessary privileges.
```

These rules become increasingly valuable as the project grows.

---

# Anti-Rules

Sometimes it's useful to tell Cline what **not** to do.

```text
Avoid

Large refactors without approval.

Introducing frameworks unnecessarily.

Changing unrelated files.

Adding abstractions before they are needed.
```

Negative guidance is often just as important as positive guidance.

---

# The Complete `.clinerules` (Version 2)

At this point, our file might look like this:

```text
Project
-------
Educational Flask application that renders the Mandelbrot set.

Architecture
------------
- Separate rendering from HTTP.
- Keep modules focused.
- Prefer composition.
- Avoid circular dependencies.

Workflow
--------
- Inspect before editing.
- Explain plans first.
- Implement one task at a time.
- Summarize changes.

Code Style
----------
- Python 3.12
- Type hints
- Readable names
- Small functions
- Standard library first

Dependencies
------------
- Justify new packages.
- Remove unused packages.

Git
---
- Small commits
- Good commit messages

Docker
------
- Small images
- Non-root user

Performance
-----------
- Optimize after measurement.

Security
--------
- Never hard-code secrets.

Review
------
- Perform a self-review after every implementation.
```

Notice it's still under a page long.

That's intentional.

---

# Should Everything Go Into `.clinerules`?

No.

A useful rule of thumb is:

**If it applies to every future task, put it in `.clinerules`.**

Otherwise, keep it in the prompt.

For example:

Good candidate:

> Always add type hints.

Bad candidate:

> Create a navigation bar.

That's task-specific.

---

# Prompt vs. `.clinerules`

A common question is: "Should this be in the prompt or in `.clinerules`?"

Use this guideline:

| Belongs in the Prompt | Belongs in `.clinerules`   |
| --------------------- | -------------------------- |
| Build a login page    | Use type hints             |
| Add Docker support    | Keep functions focused     |
| Create an API         | Prefer standard library    |
| Refactor the renderer | Explain plans before edits |
| Fix this bug          | Use small commits          |

The prompt describes **today's task**.

`.clinerules` describes **how your team works**.

---

# Evolving the Rules

One of the best habits you can develop is to update `.clinerules` whenever you notice a recurring issue.

Imagine that Cline repeatedly creates overly long functions. Rather than reminding it every time, you add a concise rule:

```text
Functions exceeding roughly 50 lines should usually be split unless there is a compelling reason not to.
```

From that point forward, every future task benefits from the improvement.

This is how your project gradually becomes easier for Cline to work on.

---

# Chapter Summary

By now, you should see `.clinerules` not as a static checklist, but as a living engineering guide that captures your team's standards, architectural philosophy, and preferred workflow. The best rule files are concise, organized, and evolve alongside the project. Instead of micromanaging Cline with increasingly detailed prompts, you invest in persistent guidance that improves every interaction.

In the next chapter, we'll begin building the actual application. We'll use the workflow established so far—inspect, plan, review, implement, verify—to create the first runnable Flask application, examining every prompt, every proposed change, every generated file, and every terminal command along the way.
