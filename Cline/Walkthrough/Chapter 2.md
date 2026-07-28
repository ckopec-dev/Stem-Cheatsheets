# Part I

# Chapter 2

# Setting Up the Development Environment

In this chapter, we'll build an environment that lets Cline operate efficiently and safely. A common mistake is to install Cline and immediately start asking it to write code. Spending a few minutes on the environment pays off throughout the project.

By the end of this chapter you'll have:

* Visual Studio Code installed
* Docker installed and working
* Python installed
* Git configured
* A Cline-compatible AI model configured
* A Git repository initialized
* The project open in VS Code
* Your first `.clinerules` file

---

# Step 1 – Install the Required Software

Install the following before opening VS Code.

| Software                                                | Version | Purpose              |
| ------------------------------------------------------- | ------- | -------------------- |
| Python                                                  | 3.12+   | Flask application    |
| Docker Desktop (Windows/macOS) or Docker Engine (Linux) | Latest  | Containerization     |
| Git                                                     | Latest  | Source control       |
| Visual Studio Code                                      | Latest  | IDE                  |
| Cline Extension                                         | Latest  | AI software engineer |

Verify each installation.

---

## Verify Python

Windows

```powershell
python --version
```

Linux/macOS

```bash
python3 --version
```

Expected

```text
Python 3.12.x
```

---

## Verify Git

```bash
git --version
```

Example

```text
git version 2.48.0
```

---

## Verify Docker

```bash
docker --version
```

Then

```bash
docker run hello-world
```

If this succeeds, Docker is ready.

---

# Step 2 – Create the Project

We'll create everything from scratch.

Linux/macOS

```bash
mkdir mandelbrot-demo
cd mandelbrot-demo
```

Windows PowerShell

```powershell
mkdir mandelbrot-demo
cd mandelbrot-demo
```

---

# Step 3 – Initialize Git

```bash
git init
```

Expected output

```text
Initialized empty Git repository
```

Why initialize Git immediately?

Because Cline works best when it can:

* compare changes
* inspect history
* suggest commits
* revert mistakes

Treat Git as part of your AI workflow rather than something you add later.

---

# Step 4 – Create a Virtual Environment

Linux/macOS

```bash
python3 -m venv .venv
```

Windows

```powershell
python -m venv .venv
```

Activate it.

Linux/macOS

```bash
source .venv/bin/activate
```

Windows

```powershell
.venv\Scripts\activate
```

Install the initial dependencies.

```bash
pip install flask pillow numpy
```

Freeze the versions.

```bash
pip freeze > requirements.txt
```

At this point the project contains:

```text
mandelbrot-demo/
    .venv/
    requirements.txt
```

Notice we haven't written any application code yet.

---

# Step 5 – Open VS Code

From the project directory:

```bash
code .
```

The Explorer should show only a few files.

Resist the temptation to create more.

We'll let Cline do most of that.

---

# Step 6 – Install the Cline Extension

Open the Extensions panel.

Search for:

```
Cline
```

Install it.

After installation you'll see a Cline icon in the Activity Bar.

---

# Step 7 – Configure an AI Provider

Cline can work with several providers. Popular choices include:

* OpenAI
* Anthropic
* Google
* OpenRouter
* Local models through Ollama

For this tutorial, choose a capable reasoning model rather than the cheapest available. Agentic tools benefit from models that can plan across multiple files and tool invocations.

After configuring your API key, test the connection by asking Cline a simple question such as:

> What files are currently in this workspace?

If everything is configured correctly, Cline should inspect the workspace instead of inventing an answer.

---

# Step 8 – Configure Cline Permissions

One of Cline's defining features is that it can execute actions on your behalf. Configure permissions deliberately.

Recommended settings for a new project:

| Capability                  | Recommendation | Why                                |
| --------------------------- | -------------- | ---------------------------------- |
| Read files                  | Allow          | Required for context               |
| Edit files                  | Ask            | Review every change while learning |
| Run terminal commands       | Ask            | Prevent unexpected commands        |
| Delete files                | Ask            | Avoid accidental removals          |
| Browser access (if enabled) | Ask            | Useful but should be reviewed      |

As you become more comfortable, you can relax some permissions for trusted projects.

---

# Step 9 – Create `.gitignore`

Create the file manually.

```text
.venv/
__pycache__/
*.pyc
.vscode/
.env
```

Commit this early so generated files don't clutter your repository.

```bash
git add .
git commit -m "Initial project setup"
```

---

# Step 10 – Create the First `.clinerules`

This file acts as persistent guidance for Cline. Unlike a one-off prompt, it applies to every task unless you change it.

Create `.clinerules` in the project root with the following content:

```text
Project: Mandelbrot Demo

General
-------
- Use Python 3.12.
- Use Flask.
- Prefer standard library unless another dependency is justified.
- Add type hints for public functions.
- Keep functions focused and under approximately 40 lines.
- Use pathlib instead of os.path.

Architecture
------------
- Separate web logic from Mandelbrot rendering logic.
- Keep rendering code independent of Flask.
- Avoid global mutable state.

Quality
-------
- Explain significant architectural decisions before making major edits.
- Prefer small, reviewable changes.
- Preserve readability over cleverness.
- If requirements are unclear, ask for clarification instead of guessing.

Docker
------
- Keep the image as small as practical.
- Use a non-root user where reasonable.
```

### Why this works

Notice these aren't implementation instructions. They describe expectations and constraints:

* coding standards
* architecture
* quality goals
* review behavior

That gives Cline room to choose good implementations while keeping it aligned with your project's style.

---

# Your First Cline Conversation

Don't start with "Build the app."

Start with a planning request.

**Prompt:**

```text
Please inspect the current project.

Explain what files currently exist.

Recommend the smallest possible Flask application we should build first.

Do not modify anything until I approve the plan.
```

This prompt teaches an important habit:

1. Inspect
2. Plan
3. Review
4. Implement

instead of immediately generating code.

A good response should summarize the current workspace, propose creating only the minimal files needed (such as `app.py` and `templates/index.html`), explain why those files are sufficient, and wait for your approval before making changes.

---

# Understanding the Workflow

At this point, the project still contains almost no code, and that's intentional. We've established the scaffolding—tools, repository, environment, permissions, and project rules—so that every subsequent interaction with Cline is grounded in a consistent context.

In the next chapter, we'll begin using Cline in earnest. Rather than asking it to "build the app," we'll learn how to decompose work into small, high-quality tasks, compare different prompt styles, review proposed changes before accepting them, and create the first runnable Flask application while examining every prompt and every file Cline generates.
