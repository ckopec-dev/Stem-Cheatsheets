# Part I

# Chapter 3

# Your First Real Cline Session

This chapter is where most tutorials go wrong.

Typically you'll see something like:

> Open Cline and ask it to create a Flask application.

That certainly works, but you won't learn **how to collaborate with Cline**. You'll end up treating it like an expensive autocomplete engine.

Instead, we're going to work exactly like a senior engineer mentoring a junior developer.

Our workflow will always be:

```
Observe

↓

Plan

↓

Review

↓

Implement

↓

Verify

↓

Commit
```

Notice that **"Write Code"** is only one small part of the process.

---

# Before We Begin

Our repository currently looks like this:

```
mandelbrot-demo/

.git/
.gitignore
.clinerules
requirements.txt
.venv/
```

No Flask application exists yet.

That's perfect.

---

# Lesson 1

# Don't Ask For Code

The biggest beginner mistake is this:

```
Create a Flask application.
```

Will it work?

Probably.

Will you learn much?

Not really.

Instead, imagine walking over to another software engineer's desk.

You wouldn't say:

> Build the website.

You would start a conversation.

---

# Better Prompt #1

This is the first prompt we'll actually send to Cline.

```
Inspect the current workspace.

Without modifying anything:

• Explain what currently exists.
• Identify the minimum files needed to create a Flask application.
• Explain why each file is necessary.
• Suggest an implementation plan consisting of no more than five small tasks.

Wait for approval before making any changes.
```

Notice what we've done.

We have **forbidden Cline from editing anything**.

This is incredibly useful.

---

# What Cline Will Probably Do

It will inspect the workspace.

Then respond with something similar to:

```
Current Files

- requirements.txt
- .gitignore
- .clinerules

Recommended Files

app.py
templates/index.html

Optional Later

Dockerfile
static/style.css
docker-compose.yml

Implementation Plan

Task 1
Create Flask entry point

Task 2
Create template

Task 3
Verify application starts

Task 4
Commit changes

Task 5
Continue development
```

This is exactly what we wanted.

No code yet.

Only planning.

---

# Lesson 2

# Approving the Plan

Once you've reviewed the plan:

Approve it.

Do **not** immediately ask for Docker.

Do **not** ask for NumPy.

Keep the task tiny.

---

# Prompt #2

```
Proceed with Task 1 only.

Create the minimum files necessary.

Do not implement anything beyond displaying a simple HTML page.

After making changes:

• Explain every file created.
• Explain every command you executed.
• Wait for further instructions.
```

Notice something subtle.

We never said

> Create app.py

We allowed Cline to decide.

That sounds insignificant, but it's a huge mindset change.

---

# Watching Cline Work

Depending on your permissions, Cline might display something like:

```
Proposed Actions

Create:

app.py

Create:

templates/index.html

Run:

python app.py
```

or

```
Create:

app.py

Run:

flask --app app run
```

Review each action.

Approve only if it makes sense.

---

# Understanding Tool Calls

One of Cline's strengths is that you can literally watch it think.

You'll see something like:

```
Reading:

requirements.txt

Reading:

.clinerules

Reading:

.gitignore
```

Then

```
Creating:

app.py
```

Then

```
Running:

python app.py
```

Watching these steps teaches you **how Cline reasons**.

Don't skip over them.

---

# Lesson 3

# Read Every Diff

Many beginners simply click **Accept**.

Don't.

Open the diff.

Ask yourself:

Why did it import this?

Why is this function here?

Why did it choose this filename?

Why wasn't another approach used?

This review process is where you'll learn the most.

---

# Our First Files

Suppose Cline generated:

```
app.py
```

```python
from flask import Flask, render_template

app = Flask(__name__)


@app.route("/")
def index():
    return render_template("index.html")


if __name__ == "__main__":
    app.run(debug=True)
```

and

```
templates/index.html
```

```html
<!doctype html>
<html>
<head>
    <title>Mandelbrot Demo</title>
</head>
<body>

<h1>Mandelbrot Demo</h1>

<p>Hello from Flask!</p>

</body>
</html>
```

Excellent.

Stop.

Don't add anything else yet.

---

# Lesson 4

# Verify Before Continuing

Ask Cline to verify its own work.

Prompt:

```
Review the changes you just made.

Check for:

• unnecessary complexity
• missing files
• style issues
• possible improvements

Do not modify anything.

Only provide a review.
```

This is one of the most powerful prompts in Cline.

You're essentially asking it to perform its own code review.

---

# Lesson 5

# Request Improvements Separately

Suppose Cline says:

> app.py would benefit from an application factory.

Don't immediately say:

> Great, do it.

Ask:

```
Explain why an application factory would improve this project.

Would you recommend doing it now or later?

Justify your answer.
```

Now Cline becomes a technical advisor instead of merely a code generator.

---

# Learning Prompt

Instead of asking

```
Fix the code.
```

Ask

```
Teach me why this code could be improved before changing it.
```

This transforms Cline into a tutor.

---

# Lesson 6

# Commit Frequently

Once you're satisfied:

```
git status
```

Suppose you see

```
modified:

app.py

templates/index.html
```

Commit immediately.

```
git add .
```

```
git commit -m "Create minimal Flask application"
```

Small commits make experimentation safe.

---

# A Better Commit Message

Instead of

```
Initial Commit
```

Use

```
Create minimal Flask application
```

Six months later you'll thank yourself.

---

# Lesson 7

# Don't Build Features

Build Capabilities

Bad mindset:

```
Now build Mandelbrot.
```

Good mindset:

```
Next, create a module capable of generating a single image.

Do not connect it to Flask yet.

We'll verify the renderer independently first.
```

Notice we're separating concerns.

Experienced developers constantly do this.

---

# A Pattern You'll Use Constantly

You'll find yourself repeating this structure over and over:

```
1. Inspect

2. Explain

3. Plan

4. Wait

5. Implement exactly one task

6. Verify

7. Review

8. Commit
```

Eventually, this becomes second nature.

---

# Prompt Patterns Worth Saving

Here are several prompts you'll use repeatedly.

### Planning

```
Inspect the project.

Identify the smallest next task.

Explain your reasoning.

Wait for approval.
```

---

### Code Review

```
Review the changes you just made.

Identify anything you would improve.

Do not modify files.
```

---

### Refactoring

```
Refactor only if it improves readability.

Do not change behavior.

Explain every modification.
```

---

### Verification

```
Run the application.

Verify everything works.

Summarize what you tested.
```

---

### Architecture

```
Given the current project size, recommend the next architectural improvement.

Justify your recommendation before making changes.
```

---

# Why This Workflow Scales

The same pattern works whether your project is:

* 5 files
* 500 files
* 50,000 files

The only thing that changes is the size of the task.

Large requests like:

> Rewrite the application.

almost always produce worse results than:

> Review the authentication module and recommend one small improvement.

---

# Chapter Summary

In this chapter, you learned that the most valuable skill with Cline isn't writing better code—it's **managing the development conversation**. By asking for inspection before implementation, reviewing plans before approving changes, verifying results before moving on, and committing frequently, you turn Cline into a collaborative engineering partner rather than a code generator.

In the next chapter, we'll go much deeper into one of Cline's most powerful features: **`.clinerules`**. We'll start with a minimal ruleset, evolve it into a production-quality project guide, examine how different rules influence Cline's behavior, and learn how to structure rules so they remain effective as a project grows from a handful of files to a large codebase.
