# Part III

# Chapter 5

# Building the Application with Cline

This is where the tutorial changes.

Up until now we've been discussing **how to use Cline**.

Beginning with this chapter we'll actually build our project.

However...

We're going to do something that almost no Cline tutorial demonstrates.

Instead of showing only the final code, we're going to show the **entire conversation** between the developer and Cline.

This is where experienced users separate themselves from beginners.

---

# The Goal of Today

At the end of this chapter we want exactly one thing.

When we visit

```
http://localhost:5000
```

we should see

```
Mandelbrot Demo

Welcome!

The renderer will be added in the next chapter.
```

That's it.

Nothing more.

---

# Current Project

```
mandelbrot-demo/

.git
.gitignore
.clinerules
requirements.txt
```

Still no application.

Perfect.

---

# Before Asking Cline Anything

Think before typing.

A beginner immediately asks:

> Build the application.

Instead, we ask ourselves:

**What is the smallest meaningful improvement?**

Answer:

> Create a Flask application that starts successfully.

Not...

> Build the website.

Not...

> Add Docker.

Not...

> Generate fractals.

Small steps win.

---

# Prompt 1

## Project Inspection

Open Cline.

Type exactly this:

```text
Inspect the current workspace.

Without making any modifications:

1. Summarize the current project.
2. Identify the minimum files required for a Flask application.
3. Explain why each file is necessary.
4. Recommend the smallest possible implementation.
5. Wait for my approval.
```

---

## Why This Prompt Works

Notice we asked Cline to:

✔ inspect

✔ analyze

✔ explain

✔ wait

We never asked it to write code.

Experienced users spend much more time asking for plans than code.

---

# Expected Response

Cline will probably reply with something similar to:

```
Current project

requirements.txt
.gitignore
.clinerules

Recommended Files

app.py
templates/index.html

Optional Later

Dockerfile
docker-compose.yml
static/style.css
mandelbrot.py

Suggested Task

Create a minimal Flask application that serves a single HTML page.
```

This is a good answer.

Approve it.

---

# Prompt 2

## Implement Only Task One

Now tell Cline:

```text
Proceed with only the first task.

Create the minimum files necessary for a Flask application.

Keep the implementation intentionally simple.

After creating the files:

• Explain every change.
• Run the application.
• Verify it starts successfully.
• Wait for additional instructions.
```

---

# Watching Cline Work

This is where many developers simply click **Approve** repeatedly.

Don't.

Watch everything.

A typical session looks like:

```
Reading

requirements.txt
```

```
Reading

.clinerules
```

```
Reading

.gitignore
```

```
Planning...
```

```
Creating

app.py
```

```
Creating

templates/index.html
```

```
Running

python app.py
```

Observe every step.

You're learning how Cline thinks.

---

# Reviewing the Proposed Changes

Before approving edits, Cline typically shows a diff. This is your chance to act like a code reviewer.

Questions to ask yourself:

* Does each file have a single responsibility?
* Is there unnecessary complexity?
* Did Cline introduce a dependency we didn't ask for?
* Are there any "future-proofing" abstractions we don't need yet?

If you see something like a configuration class, dependency injection framework, or elaborate folder hierarchy in a project with two files, reject the change and ask Cline to simplify it.

A useful response might be:

```text
This is more complex than necessary for the current stage of the project.

Please simplify the implementation while preserving functionality.
```

---

# The Generated Files

After approval, the project should look something like:

```
mandelbrot-demo/

app.py
requirements.txt
.clinerules
.gitignore

templates/
    index.html
```

Notice how small it still is.

---

# Inspecting `app.py`

A minimal implementation might look like:

```python
from flask import Flask, render_template

app = Flask(__name__)


@app.route("/")
def index():
    return render_template("index.html")


if __name__ == "__main__":
    app.run(debug=True)
```

Let's review it like a senior engineer.

### Imports

Only the required imports are present.

Good.

### Flask Instance

```python
app = Flask(__name__)
```

Simple.

No unnecessary configuration.

Good.

### Route

Only one route.

Perfect.

### Main Entry Point

Allows us to run

```bash
python app.py
```

Excellent.

No changes needed.

---

# Inspecting `index.html`

```html
<!doctype html>
<html lang="en">

<head>
    <title>Mandelbrot Demo</title>
</head>

<body>

<h1>Mandelbrot Demo</h1>

<p>Welcome!</p>

<p>The renderer will be added soon.</p>

</body>
</html>
```

Again—

Tiny.

Readable.

Exactly what we wanted.

---

# Verify the Application

Don't assume it works.

Have Cline verify it.

Prompt:

```text
Run the application.

Verify that the homepage loads successfully.

Describe what you tested.

Do not modify any files.
```

Notice:

Verification is separate from implementation.

---

# Why Separate Verification?

Many beginners combine everything into one prompt.

```
Build the app and make sure it works.
```

Experienced developers split tasks.

```
Implement.

↓

Verify.

↓

Review.

↓

Improve.
```

Each phase has a different objective.

---

# Asking for a Self-Review

Once the application starts successfully, ask Cline to critique its own work.

Prompt:

```text
Review the implementation you just created.

Identify:

• unnecessary complexity
• possible improvements
• future architectural concerns

Do not make any changes.
```

This prompt often surfaces ideas you may not have considered, such as:

* moving to an application factory later,
* introducing configuration management only when needed,
* separating rendering logic from the web layer.

The key phrase is "later." Resist the temptation to implement every suggestion immediately.

---

# Resist Feature Creep

At this point, you might feel tempted to ask:

* Add Bootstrap.
* Add Docker.
* Add logging.
* Add tests.
* Add configuration.
* Add CI.

Don't.

A disciplined workflow means declaring this task complete once it meets the original goal.

---

# Commit the Work

Only after implementation, verification, and review should you commit.

```bash
git status
```

Expected:

```
modified:
    app.py
    templates/index.html
```

Stage the changes:

```bash
git add .
```

Commit them:

```bash
git commit -m "Create minimal Flask application"
```

One logical task.

One logical commit.

---

# What Most Developers Would Do Next

Most tutorials would immediately begin adding features.

We won't.

Instead, we'll spend time improving how we collaborate with Cline.

The quality of the next 100 prompts matters more than the quality of the next 100 lines of code.

---

# Cline Conversation Pattern #1

You've now learned a reusable conversation pattern:

```
Inspect

↓

Plan

↓

Implement

↓

Verify

↓

Review

↓

Commit
```

You'll use this dozens of times.

---

# Cline Conversation Pattern #2

When asking for implementation, provide constraints instead of prescribing every detail.

Instead of:

```text
Create app.py.

Import Flask.

Create one route.

Use render_template.

Return index.html.
```

Prefer:

```text
Create the smallest possible Flask application that serves a single HTML page.

Follow the project's .clinerules.

Keep the implementation intentionally minimal.

After implementation, explain your design decisions and verify that the application starts successfully.
```

The second prompt gives Cline room to make sensible engineering decisions while still constraining the outcome.

---

# A Note on Trust

One of the biggest adjustments when working with Cline is learning **when not to interfere**.

If you've provided clear project rules, a focused prompt, and a well-defined objective, let Cline propose a solution before jumping in with implementation details. Review the result critically, but don't micromanage every line. Over time you'll develop a sense of which tasks benefit from detailed guidance and which are better handled autonomously.

---

# Chapter Summary

By the end of this chapter we've accomplished something deceptively important: we've established a repeatable, professional workflow for using Cline on real projects. We didn't just generate a Flask application—we practiced planning before coding, reviewing before approving, verifying before committing, and resisting the urge to over-engineer.

In the next chapter, we'll add the first real feature: a standalone Mandelbrot rendering engine. We'll ask Cline to design the renderer as an independent Python module, verify it in isolation, and only then connect it to Flask. Along the way we'll explore how to guide Cline through algorithmic programming tasks, how to request performance-conscious implementations, and how to iteratively refine generated code without sacrificing simplicity.
