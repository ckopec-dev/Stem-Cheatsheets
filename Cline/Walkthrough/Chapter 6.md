# Part III

# Chapter 6

# Building the Mandelbrot Renderer

This is the chapter where Cline starts to feel like magic.

Until now we've built a very ordinary Flask application.

Now we're going to ask Cline to implement a mathematical algorithm.

Many people make the mistake of asking:

> Generate the Mandelbrot set.

That usually works...

But it's not how experienced Cline users work.

Instead, we're going to teach Cline exactly how to collaborate with us.

---

# Our Goal

By the end of this chapter we'll have this architecture:

```text
Browser

↓

Flask

↓

mandelbrot.py

↓

NumPy

↓

Pillow

↓

PNG Image
```

Notice something important.

Flask **knows nothing** about the Mandelbrot algorithm.

Likewise,

the renderer knows nothing about Flask.

That separation is intentional.

---

# Why Separate the Renderer?

Beginners often produce code like this:

```python
@app.route("/")
def index():

    width = 800

    height = 600

    max_iterations = 200

    # 200 lines of Mandelbrot math...

    return image
```

This works.

It is also terrible architecture.

Instead we want:

```text
Flask

↓

Renderer

↓

Image
```

Small modules.

Single responsibility.

Easy to test.

---

# Before Writing Code

Ask Cline to think first.

Prompt:

```text
Inspect the current project.

We want to add Mandelbrot rendering.

Without modifying anything:

• Recommend where the rendering logic should live.

• Explain how Flask should interact with it.

• Suggest the minimum files required.

• Wait for approval.
```

Notice the pattern.

Still...

No code.

---

# Expected Plan

A good answer might be:

```
New file

mandelbrot.py

Contains

generate_image()

Flask route

Calls generate_image()

Future

Support configurable resolution

Support color palettes

Support zooming
```

Perfect.

Approve only the first task.

---

# Prompt 2

Now we implement.

```text
Proceed with only the first task.

Create a standalone renderer.

Requirements

• No Flask imports.

• Return a Pillow Image.

• Use NumPy.

• Keep the implementation readable.

• Explain every design decision.

• Run a small verification after implementation.
```

Again...

Notice we describe the **goal**, not the implementation.

---

# What Cline Will Probably Create

```
mandelbrot.py
```

containing something similar to

```python
def generate_image():

    ...

    return image
```

Exactly what we want.

---

# Why We Didn't Mention Pixels

We never said

```
Loop over x

Loop over y

Use escape time

```

Those are implementation details.

Cline already knows multiple Mandelbrot algorithms.

We care about the result.

Not the syntax.

---

# Reviewing the Design

Before looking at the code, ask yourself:

Does this module

✔ import Flask?

It shouldn't.

✔ depend upon HTML?

It shouldn't.

✔ know anything about HTTP?

It shouldn't.

If the answer is yes—

reject the change.

---

# Asking Better Questions

Instead of

```
Why did you do this?
```

Ask

```
Why is this implementation preferable to a pure Python implementation?
```

Notice the difference.

You're requesting engineering reasoning.

---

# Performance Discussion

This is where Cline becomes extremely useful.

Ask:

```text
Explain the performance characteristics of this renderer.

Would vectorization improve performance?

Would multiprocessing help?

Do not modify the code.
```

Now you're learning.

Not merely generating code.

---

# Expected Discussion

Cline will probably explain:

```
Pure Python

↓

Simple

↓

Slow
```

versus

```
NumPy

↓

Vectorized

↓

Much Faster
```

It might also explain

* cache locality

* SIMD

* reduced Python loops

* array operations

Excellent.

That's the conversation we wanted.

---

# Verify the Renderer

Now ask Cline to verify the module independently.

Prompt:

```text
Create a temporary Python script that imports the renderer.

Generate one image.

Save it as

test.png

Verify the image was created successfully.

After verification, remove the temporary script.
```

This is a fantastic habit.

You're testing the module

**before Flask ever sees it.**

---

# Why Independent Testing Matters

Imagine the image doesn't generate.

Without separation you'll wonder

Was Flask wrong?

Was HTML wrong?

Was Pillow wrong?

Was NumPy wrong?

Instead

```
Renderer

↓

Verified

↓

Done
```

Only then do we integrate.

---

# Connecting Flask

Once verification succeeds

ask Cline

```text
Connect the renderer to Flask.

Create a route that returns the generated image.

Do not modify the renderer itself unless necessary.

Explain every change.
```

Notice

We're not rebuilding anything.

We're simply wiring modules together.

---

# A Beautiful Architecture

We now have

```
Browser

↓

HTTP

↓

Flask

↓

Renderer

↓

PNG
```

Each component has exactly one responsibility.

---

# The Beginner Mistake

Many people would have started here:

```
Create a Flask application that renders the Mandelbrot set.
```

That single prompt mixes

* web programming

* mathematics

* image generation

* testing

* architecture

* debugging

into one giant task.

Large prompts often produce large, tangled solutions.

---

# Cline Conversation Pattern

Instead

```
Plan

↓

Renderer

↓

Verify

↓

Integrate

↓

Verify Again
```

Five tiny conversations.

Five opportunities to review.

Five chances to catch mistakes early.

---

# Improving the Renderer

Once everything works

don't immediately optimize.

Instead ask

```text
Review the renderer.

Recommend improvements ranked by

1. readability

2. maintainability

3. performance

Explain the tradeoffs.

Do not modify code.
```

This prompt is gold.

It encourages Cline to prioritize improvements rather than making arbitrary optimizations.

---

# Using Cline as an Architect

One of the most underused workflows is to ask Cline to critique its own design.

For example:

```text
Assume this project will eventually support:

• deep zooming

• multiple color palettes

• animation

• tiled rendering

Would you keep the current architecture?

Explain what would eventually need to change.
```

This is not about making those changes today. It's about understanding whether today's simple design can evolve naturally or whether it paints you into a corner. You'll often get thoughtful discussions about separating rendering configuration, introducing immutable parameter objects, or keeping rendering stateless.

---

# When to Stop

At the end of this chapter, resist the temptation to add zoom controls, color schemes, caching, or asynchronous rendering. The renderer has one job: produce a correct image. If it does that cleanly and predictably, the task is complete.

Professional software development is as much about **knowing when to stop** as it is about knowing what to build next.

---

# Chapter Summary

This chapter introduced the first substantial piece of application logic while reinforcing the development habits we've built throughout the tutorial. Rather than asking Cline to create an entire feature in one step, we separated planning, implementation, verification, and integration. The result is a renderer that can be tested independently, reused outside of Flask, and extended in future chapters without coupling mathematical code to the web framework.

In the next chapter, we'll make our interaction with Cline even more sophisticated. Instead of simply asking it to write code, we'll learn how to have it produce implementation plans, evaluate alternatives, estimate tradeoffs, and even critique its own proposals before a single file is modified. This is the point where Cline starts acting less like an AI coding assistant and more like a collaborative software architect.
