
# Part I

# Learning to Think Like an AI-Assisted Developer

## Chapter 1

# What Makes Cline Different?

Many developers initially use Cline the same way they use GitHub Copilot:

> Write a function...
>
> Fix this bug...
>
> Explain this code...

While Cline can certainly do those things, that isn't where it shines.

Instead, think of Cline as an autonomous junior software engineer that can:

* inspect your project
* read documentation
* edit dozens of files
* execute terminal commands
* run tests
* fix failing tests
* commit code
* iterate until a task is complete

The biggest mindset shift is this:

**You stop asking for code.**

Instead, you assign software engineering tasks.

For example, instead of saying:

> Write a Flask route.

you say:

> Build a REST endpoint that returns a PNG rendering of the Mandelbrot set. Update any necessary files, run the application, verify it works, and explain any assumptions.

That is much closer to how you'd assign work to another developer.

---

# Cline's Development Loop

Think of Cline operating in this loop:

```
Understand

↓

Plan

↓

Inspect Code

↓

Edit Files

↓

Run Commands

↓

Observe Results

↓

Fix Problems

↓

Repeat
```

Traditional autocomplete tools only participate during the **Edit Files** step.

Cline participates in the *entire* development lifecycle.

---

# Why Small Projects Teach Better

Our Mandelbrot project intentionally avoids complicated business logic.

Instead we'll focus on learning:

* prompting
* planning
* reviewing edits
* using .clinerules
* keeping context small
* refactoring
* debugging
* Docker workflows

The programming language is almost irrelevant.

---

# The Finished Application

By the end the application will look like this:

```
Browser

↓

Flask

↓

Mandelbrot Generator

↓

PNG Image

↓

Browser Display
```

Project structure:

```
mandelbrot-demo/

├── app.py
├── mandelbrot.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── .clinerules
├── static/
│   └── style.css
├── templates/
│   └── index.html
└── README.md
```

Nothing fancy.

That's intentional.

---

# Our Development Philosophy

We're going to let Cline write almost everything.

Our responsibilities become:

* defining requirements
* reviewing changes
* correcting mistakes
* improving prompts

This is surprisingly difficult at first.

Many new users constantly tell Cline exactly **how** to implement things.

Experienced users instead describe **what** they want.

---

# Good Prompt

```
Create a Flask application that serves a single HTML page.

Requirements:

- Python 3.12
- Flask
- Follow .clinerules
- Keep the application minimal.
- Explain every file you create before editing it.
```

Notice this prompt never says:

* create app.py
* import Flask
* use render_template()

Those are implementation details.

---

# Poor Prompt

```
Create app.py

Import Flask

Create a route

Return render_template

Now make index.html

Now create static

Now...
```

This treats Cline like autocomplete.

It removes most of the value.

---

# The Human's Job

As AI becomes better, the human's job changes.

Instead of programming every line:

```
Human
↓

Writes Code
↓

Computer Executes
```

it becomes

```
Human

↓

Designs Requirements

↓

AI Implements

↓

Human Reviews

↓

AI Refines
```

Notice that review becomes far more important than typing.

---

# Rule #1

Never ask Cline to write lots of code immediately.

Instead ask it to make small, verifiable progress.

Bad:

> Build the whole website.

Good:

> Create the smallest application that starts successfully.

---

# Rule #2

Always ask for a plan first.

Instead of:

> Add Docker support.

Ask:

```
Before making changes:

1. Inspect the project.

2. Explain what needs to change.

3. Wait for approval.
```

This dramatically improves quality.

---

# Rule #3

Prefer Constraints Over Instructions

Bad:

```
Use Flask.

Use Bootstrap.

Use Pillow.

Use Docker.

Use Python.

Use NumPy.

```

Good:

```
Requirements:

- Application must start in under five seconds.
- Final Docker image should be under 200 MB.
- Python 3.12.
- Rendering should be deterministic.
- No global mutable state.
```

Notice the second prompt tells Cline what success looks like.

---

# Rule #4

Keep Tasks Small

Instead of:

> Build a Mandelbrot viewer.

Break it apart.

Task 1

```
Create a Flask application that returns "Hello World".
```

Task 2

```
Replace Hello World with an HTML template.
```

Task 3

```
Display a placeholder image.
```

Task 4

```
Generate a Mandelbrot image.
```

Task 5

```
Move rendering into a separate module.
```

Each task should take only a few minutes.

---

# Rule #5

Treat Cline Like a Junior Engineer

Imagine giving work to a talented junior developer.

You wouldn't say:

> Write code.

You would say:

> Investigate the issue, explain your findings, implement a solution, run the tests, and summarize what changed.

That style of communication works remarkably well with Cline.

---
