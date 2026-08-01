# Part V

# Chapter 9

# Advanced Prompt Engineering for Cline

Most articles about prompt engineering are written for ChatGPT.

They usually look something like:

> Be specific.

> Give examples.

> Ask clearly.

Those are good general guidelines.

They are **not** how experienced Cline users work.

Cline is fundamentally different because it can:

* inspect files
* edit code
* execute terminal commands
* read documentation
* run tests
* analyze logs
* iterate on failures

That changes how prompts should be written.

This chapter is about writing prompts for **software engineering**, not for conversation.

---

# The Anatomy of a Great Cline Prompt

Over time, I've found that the best prompts almost always contain five parts.

```text
Goal

↓

Constraints

↓

Scope

↓

Verification

↓

Stopping Point
```

Let's examine each one.

---

# 1. Goal

Start with **what you want to accomplish**.

Bad

```text
Refactor app.py
```

Good

```text
Improve the readability of the Flask application without changing behavior.
```

Notice that the second prompt defines success.

---

# 2. Constraints

Now define the boundaries.

Example

```text
Requirements

• Preserve existing functionality.

• Follow .clinerules.

• Keep the implementation simple.

• Do not introduce new dependencies.
```

Constraints are often more valuable than implementation details.

---

# 3. Scope

Tell Cline where to work.

Bad

```text
Improve performance.
```

Good

```text
Focus only on

mandelbrot.py

Ignore Flask, Docker, tests, and HTML.
```

This dramatically reduces unnecessary edits.

---

# 4. Verification

Every engineering task should end with verification.

Example

```text
After implementation

Run the application.

Verify the renderer still generates an image.

Summarize what you tested.
```

Never assume success.

---

# 5. Stopping Point

This is perhaps the most overlooked part of prompting.

Tell Cline when to stop.

Example

```text
Do not implement additional improvements.

Stop after completing this task.

Wait for approval.
```

Otherwise, Cline may continue making changes you didn't request.

---

# The Universal Prompt Template

Nearly every software engineering task can be expressed using this structure:

```text
Goal

<what you want>

Constraints

<rules>

Scope

<where to work>

Verification

<how to confirm success>

Stopping Point

<when to stop>
```

You'll see this pattern throughout the rest of the book.

---

# Prompt Pattern 1

# Investigation

Don't start by fixing.

Start by investigating.

Example

```text
Investigate why the Flask application starts slowly.

Collect evidence.

Identify the likely root cause.

Recommend the smallest fix.

Do not modify any files.
```

This is an engineering investigation.

---

# Prompt Pattern 2

# Code Review

```text
Review the current implementation.

Identify

• readability issues

• maintainability issues

• unnecessary complexity

• technical debt

Do not modify anything.
```

One of the highest-value prompts you can use.

---

# Prompt Pattern 3

# Refactoring

```text
Refactor only for readability.

Preserve behavior.

Do not introduce new abstractions unless clearly justified.

Explain every modification.

Verify functionality afterward.
```

Notice we aren't asking for "better" code.

We're asking for **more readable** code.

That's measurable.

---

# Prompt Pattern 4

# Architecture

```text
Inspect the project.

Recommend one architectural improvement.

Explain

Why

Benefits

Tradeoffs

Future impact

Do not implement it.
```

Many engineers skip this step entirely.

---

# Prompt Pattern 5

# Performance

Instead of

> Make this faster.

Ask

```text
Measure the likely performance bottlenecks.

Estimate the expected improvement for each optimization.

Rank them.

Recommend one.

Do not optimize yet.
```

Optimization should always follow analysis.

---

# Prompt Pattern 6

# Bug Hunting

Instead of

> Fix this bug.

Ask

```text
Investigate the reported bug.

Determine

Root cause

Contributing factors

Possible fixes

Recommended fix

Wait for approval before changing code.
```

This mirrors a professional debugging workflow.

---

# Prompt Pattern 7

# Design Comparison

```text
Compare three implementations.

Discuss

Complexity

Maintainability

Performance

Extensibility

Recommend one.

Justify the recommendation.
```

Cline excels at structured comparisons.

---

# Prompt Pattern 8

# Risk Assessment

One of my favorites.

```text
Assume this code will be maintained for five years.

Identify the largest maintenance risks.

Rank them.

Recommend the smallest mitigation.
```

Now you're thinking long-term.

---

# Prompt Pattern 9

# Self Critique

```text
Review your own implementation.

Assume another senior engineer disagrees with it.

Identify the strongest criticisms.

Explain whether you agree.
```

This often produces surprisingly thoughtful analyses.

---

# Prompt Pattern 10

# Pull Request Review

Instead of reviewing your own code manually:

```text
Review the current changes as though this were a pull request.

Comment on

Architecture

Naming

Error handling

Testing

Documentation

Maintainability

Do not modify files.
```

You've effectively created an AI reviewer.

---

# Prompt Pattern 11

# Teaching Mode

One of the most underrated prompts.

```text
Teach me.

Explain every design decision.

Assume I am familiar with Python but unfamiliar with this architecture.

Do not simplify the explanation unnecessarily.
```

Notice we're asking to learn.

Not merely to finish.

---

# Prompt Pattern 12

# Exploration

```text
List three possible approaches.

Recommend one.

Explain why the other two were rejected.
```

This often produces better solutions than asking for the first idea.

---

# The "One Thing" Rule

Every prompt should ask Cline to accomplish **one primary objective**.

Bad

```text
Add Docker.

Improve performance.

Rewrite HTML.

Refactor Flask.

Write tests.

Update README.
```

Good

```text
Containerize the application.

Stop after verification.
```

One objective.

---

# The "Evidence" Rule

Whenever possible, ask Cline to support recommendations with evidence.

Example

```text
Recommend one optimization.

Explain what evidence suggests it will help.

State your confidence level.
```

This encourages reasoned responses rather than guesses.

---

# Prompt Chaining

Complex engineering work is best done as a sequence of prompts rather than one giant request.

For example, adding image caching might look like this:

**Prompt 1 — Investigation**

```text
Inspect the renderer.

Identify where caching would provide the greatest benefit.

Do not modify anything.
```

↓

**Prompt 2 — Design**

```text
Recommend a caching strategy.

Compare in-memory caching and disk caching.

Wait for approval.
```

↓

**Prompt 3 — Implementation**

```text
Implement the approved strategy.

Keep the API unchanged.

Verify existing functionality.
```

↓

**Prompt 4 — Review**

```text
Review the implementation.

Identify future maintenance concerns.

Do not modify code.
```

Each prompt has one purpose.

---

# Advanced Prompt: Devil's Advocate

One of the most powerful prompts I use is:

```text
Assume your recommendation is wrong.

Construct the strongest technical argument against it.

Explain what evidence would convince you to change your recommendation.
```

This often uncovers hidden assumptions and produces a much more balanced engineering discussion.

---

# Advanced Prompt: What Would You Delete?

Engineers often focus on what to add.

Try asking:

```text
If you had to remove 20% of this code without reducing functionality,

what would you delete?

Explain why.
```

This frequently identifies duplication, unnecessary abstractions, or dead code that would otherwise go unnoticed.

---

# Building Your Own Prompt Library

As you gain experience, you'll notice that certain prompts consistently produce excellent results. Rather than rewriting them each time, create a personal prompt library organized by category:

* Planning
* Debugging
* Refactoring
* Architecture
* Performance
* Code Review
* Documentation
* Testing
* Security

Over time, these become reusable engineering workflows rather than one-off questions.

---

# Chapter Summary

The most effective Cline prompts aren't clever—they're structured. By consistently defining the goal, constraints, scope, verification, and stopping point, you make it easier for Cline to focus on the right problem. Combined with prompt chaining, investigation before implementation, and explicit review phases, these techniques turn complex engineering work into a series of manageable, verifiable steps.

In the next chapter, we'll move from prompting to **debugging**. We'll explore how to use Cline to investigate failing tests, diagnose runtime errors, analyze stack traces, isolate regressions, and systematically work toward root causes without jumping straight to code changes. This is where Cline becomes an exceptionally effective debugging partner rather than just a code-writing assistant.
