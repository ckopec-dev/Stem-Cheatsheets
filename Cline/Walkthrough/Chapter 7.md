# Part IV

# Chapter 7

# Treating Cline Like a Senior Software Engineer

If you've followed the tutorial so far, you've probably noticed something.

We've spent far more time discussing **conversations** than **code**.

That isn't accidental.

The code is almost the least interesting part.

The real productivity gain comes from learning how to **manage engineering work**, not how to ask for individual functions.

This chapter is where most developers experience the biggest improvement in using Cline.

---

# The Four Levels of AI-Assisted Development

Most people stay at Level 1 forever.

## Level 1 — Code Generator

You ask:

> Write a function.

Cline writes it.

You copy it.

Repeat.

This is barely different from autocomplete.

---

## Level 2 — Multi-File Editor

Now you ask:

> Add user authentication.

Cline edits

* five files
* updates imports
* fixes routes
* runs tests

Much better.

Most developers stop here.

---

## Level 3 — Software Engineer

Instead of asking for code...

You ask for engineering work.

For example:

```text id="2q187d"
Investigate why startup time has increased.

Identify the root cause.

Recommend three possible solutions.

Rank them by complexity and expected benefit.

Wait for approval.
```

Notice something important.

There is no mention of writing code.

---

## Level 4 — Technical Advisor

This is where experienced users operate.

Instead of saying

> Implement feature X

they ask

```text id="rfrb4v"
Assume you're the lead engineer reviewing this project.

Identify the three highest-risk architectural issues.

Explain why they matter.

Recommend the smallest improvement that would reduce future technical debt.

Do not modify anything.
```

This transforms Cline into something much closer to an experienced consultant.

---

# The Three-Conversation Workflow

One of the biggest improvements you can make is separating engineering into three distinct conversations.

Conversation One

Planning

Conversation Two

Implementation

Conversation Three

Review

Beginners combine all three.

---

# Example

Instead of

```text id="ijr5k4"
Add Docker support.
```

Do this.

---

## Conversation 1

```text id="pq7scd"
Inspect the project.

Explain what would be required to containerize it.

Discuss possible Dockerfile designs.

Recommend one.

Wait for approval.
```

No edits.

Just planning.

---

## Conversation 2

```text id="mhr8sz"
Implement only the approved design.

Keep the Dockerfile simple.

Explain every decision.

Run a build.

Verify success.
```

Implementation only.

---

## Conversation 3

```text id="l8mld4"
Review the Docker configuration.

Identify

• security concerns

• performance improvements

• maintainability issues

Do not modify files.
```

Now you're getting an engineering review.

---

# Why This Works

Each conversation has exactly one purpose.

Planning is analytical.

Implementation is mechanical.

Review is critical.

Mixing all three usually produces weaker results.

---

# Asking Cline to Think

One of my favorite prompts is this:

```text id="rkbdzq"
Before proposing a solution, identify the assumptions you're making.

State anything that is uncertain.

Then recommend an implementation.
```

Why?

Because every software project contains uncertainty.

Experienced engineers recognize it.

Good AI should too.

---

# Design Reviews Before Coding

Suppose we want to add zooming.

A beginner asks:

> Add zoom controls.

Instead ask:

```text id="vgyx2q"
We want to support arbitrary zooming into the Mandelbrot set.

Recommend three possible architectures.

Discuss

• complexity

• extensibility

• performance

Recommend one.

Do not write code.
```

This is exactly what you'd ask another engineer.

---

# Ranking Alternatives

One of the most underused capabilities of Cline is comparing multiple designs.

For example:

```text id="j41t9s"
Compare these approaches.

A.

Generate images on demand.

B.

Cache generated images.

C.

Precompute image tiles.

Rank them for this project.

Justify the ranking.
```

Now Cline becomes an architecture advisor.

---

# Asking for Tradeoffs

Avoid questions like

```text id="ym5x4u"
Which is better?
```

Instead ask

```text id="i7cjlwm"
Compare these approaches.

Discuss

• readability

• maintainability

• performance

• scalability

Recommend one.

Explain why.
```

Tradeoffs are where engineering happens.

---

# Asking for Risks

Another excellent prompt:

```text id="bupjlwm"
Assume this application eventually serves one million users.

What parts of today's design would fail first?

Explain why.

Do not suggest changes yet.
```

Notice we're separating

identification

from

solution.

---

# Using Cline for Root Cause Analysis

Suppose the renderer becomes slow.

Instead of

> Make it faster.

Ask

```text id="o7kztt"
Investigate why rendering performance is poor.

Collect evidence.

Identify likely bottlenecks.

Estimate which optimization would provide the largest improvement.

Do not optimize yet.
```

This encourages data-driven engineering.

---

# Architecture Decision Records

A workflow I use frequently is asking Cline to produce a lightweight Architecture Decision Record (ADR) before making a significant change.

For example:

```text id="xk5efw"
Before implementing image caching,

write a short ADR describing

Problem

Options

Recommendation

Tradeoffs

Then wait.
```

Now every major change has documented reasoning.

---

# The "Convince Me" Prompt

One of the strongest prompts in my toolbox is simply:

```text id="jlwm2g"
Convince me this is the right solution.

Assume I disagree.

Use technical reasoning.
```

You'll often receive a far better explanation than if you merely asked

> Why?

---

# Challenging Cline

Don't always accept the first recommendation.

Try this:

```text id="fyjnzv"
Argue against your own proposal.

What are its weaknesses?

What would another senior engineer criticize?
```

This often surfaces assumptions that would otherwise remain hidden.

---

# Encouraging Humility

AI systems can sometimes sound more confident than the evidence warrants.

A useful prompt is:

```text id="ttn5qm"
For each recommendation,

state

High Confidence

Medium Confidence

Low Confidence

and explain why.
```

This helps distinguish well-supported conclusions from educated guesses.

---

# Using Cline as a Pair Programmer

Imagine sitting beside an experienced colleague.

You don't ask them to silently rewrite the project.

You ask questions.

You explore alternatives.

You challenge assumptions.

You review each other's ideas.

Cline works best in exactly this style of collaboration.

---

# The Review Triangle

For every significant task, ask three different questions:

```text id="n9x3pl"
1.

What is your recommendation?

↓

2.

What is the strongest argument against it?

↓

3.

What evidence would change your mind?
```

This simple pattern dramatically improves decision quality.

---

# A Complete Example Conversation

Suppose we're about to add color palettes to the Mandelbrot renderer.

Instead of jumping directly to implementation, the interaction might look like this:

**Planning Prompt**

```text id="p6j8ru"
We want to support multiple color palettes without making the renderer difficult to maintain.

Recommend an architecture.

Discuss alternatives.

Wait for approval.
```

**Follow-up Prompt**

```text id="h3amf7"
Compare your recommended approach against a strategy pattern and against a simple lookup table.

Which would you choose for this project, and why?
```

**Implementation Prompt**

```text id="j4ylwu"
Implement only the approved approach.

Keep the API small.

Add clear docstrings.

Verify the existing renderer still behaves correctly.
```

**Review Prompt**

```text id="b9k2sf"
Review your implementation as though it were a pull request from another engineer.

Identify any unnecessary complexity or future maintenance concerns.

Do not modify the code.
```

By separating the work into these conversations, you've effectively recreated the rhythm of a professional design review and code review.

---

# Chapter Summary

This chapter marks an important shift in how we use Cline. Instead of treating it as a sophisticated code generator, we've started treating it like a senior engineering collaborator—someone who can analyze problems, compare alternatives, explain tradeoffs, challenge assumptions, and critique its own work before implementation begins.

As projects grow, these conversations become far more valuable than the code itself. The better you become at structuring engineering discussions, the more effective Cline becomes.

In the next chapter, we'll tackle one of the most advanced topics in agentic development: **managing context**. We'll explore how Cline builds its understanding of a codebase, why context windows matter, techniques for preventing context overload, when to start a fresh conversation, and how to structure long-running projects so the AI remains effective even as the codebase grows to hundreds or thousands of files.
