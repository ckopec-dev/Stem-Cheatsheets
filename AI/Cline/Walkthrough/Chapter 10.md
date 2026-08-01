# Part V

# Chapter 10

# Debugging with Cline

Most developers use Cline to write code.

The best developers use Cline to debug code.

Why?

Because writing new code is usually the easy part.

The difficult part is understanding:

* why something broke
* where it broke
* when it broke
* how to fix it without creating new problems

This chapter focuses on turning Cline into a systematic debugging partner.

---

# The Biggest Debugging Mistake

Suppose your Flask application crashes.

A beginner immediately asks:

```text id="h8v61r"
Fix this error.
```

This is usually the wrong approach.

Why?

Because you haven't established:

* root cause
* scope
* reproduction steps
* affected files

You're asking for treatment before diagnosis.

---

# Think Like an Incident Response Team

Professional debugging looks like this:

```text id="7snjzk"
Observe

↓

Reproduce

↓

Collect Evidence

↓

Identify Cause

↓

Design Fix

↓

Verify Fix

↓

Prevent Recurrence
```

Notice that "write code" appears very late.

---

# The Investigation Prompt

Whenever you encounter a bug, start here:

```text id="4c0kmc"
Investigate the issue.

Determine:

• reproduction steps
• affected files
• likely root cause
• confidence level

Do not modify code.

Collect evidence first.
```

This single prompt will save countless hours.

---

# Example Bug

Suppose Flask starts successfully.

But navigating to

```text id="p3m20l"
/
```

returns

```text id="7vvl2h"
500 Internal Server Error
```

Many people immediately ask Cline to fix it.

Instead:

---

# Step 1

# Gather Evidence

Prompt:

```text id="n2yk4f"
Inspect the application logs.

Identify the exception.

Explain what the exception means.

Do not modify files.
```

Notice:

We're not asking for a solution.

We're asking for understanding.

---

# What Good Debuggers Do

Good debuggers ask:

```text id="t56gzt"
What happened?

↓

Why?

↓

How do we know?

↓

What should we do?
```

Bad debuggers skip directly to:

```text id="n1z8zv"
What should we do?
```

---

# Understanding Stack Traces

Many developers see a stack trace and immediately scroll to the bottom.

Don't.

Ask Cline:

```text id="r7vmyw"
Explain this stack trace.

Identify:

• where the error originated

• where it was propagated

• the first likely fault

Do not propose fixes yet.
```

This often reveals the true problem much faster.

---

# Example

Suppose the trace ends with:

```text id="fcrnh2"
TemplateNotFound:
index.html
```

A beginner asks:

> How do I fix it?

An experienced engineer asks:

```text id="ux6v5j"
Why did Flask fail to locate the template?

Inspect the project structure and explain the expected template location.
```

The difference matters.

---

# Cline as a Detective

One of the best uses of Cline is investigating the entire chain of events.

Prompt:

```text id="2h5gde"
Trace the execution path for this request.

Start at the route handler.

Follow execution until the exception occurs.

Explain each step.
```

Now you're debugging the system.

Not the symptom.

---

# Root Cause vs Symptom

Suppose an image fails to render.

Symptom:

```text id="j1mh4g"
Image generation failed.
```

Root cause:

```text id="eg5h4q"
Invalid NumPy array dimensions.
```

Never stop at the symptom.

---

# The Five Whys

An excellent debugging technique.

Ask repeatedly:

```text id="p8hdn1"
Why?
```

Example:

```text id="8h4u8m"
Image missing

↓

Why?

File wasn't created

↓

Why?

Renderer failed

↓

Why?

Array dimensions invalid

↓

Why?

Width parameter was zero

↓

Why?

Input validation missing
```

Now you've found the real problem.

---

# Prompt Pattern

# Root Cause Analysis

```text id="g6c4fw"
Perform a root cause analysis.

Identify:

Immediate failure

Contributing factors

Underlying cause

Recommended fix

Confidence level

Do not modify code.
```

This is one of the highest-value prompts in your toolbox.

---

# Reproducing Bugs

Never debug an unreproducible issue.

Prompt:

```text id="jz1l88"
Create reliable reproduction steps.

Verify the bug occurs consistently.

Document the exact sequence.
```

If the bug cannot be reproduced, fixing it becomes guesswork.

---

# Isolating the Problem

Suppose the Mandelbrot image is wrong.

Possible causes:

```text id="jlwm7d"
Flask

↓

Route

↓

Renderer

↓

NumPy

↓

Pillow

↓

File Output
```

Don't investigate everything.

Ask:

```text id="a2wnv8"
Determine which component first produces incorrect output.

Ignore downstream effects.
```

Now you're narrowing the search.

---

# Binary Search Debugging

Large systems often require narrowing the scope.

Example:

```text id="z4d26j"
Request

↓

Route

↓

Service

↓

Renderer

↓

Image
```

Ask Cline:

```text id="6hm6g0"
Identify the midpoint of the execution path.

Verify whether the data is correct there.

Use the result to narrow the search.
```

This mimics binary search.

Extremely effective.

---

# Investigating Regressions

One of the most powerful workflows combines Cline with Git.

Suppose everything worked yesterday.

Today it doesn't.

Prompt:

```text id="0q2ylo"
Inspect recent commits.

Identify changes likely related to this failure.

Rank them by probability.
```

Now Cline becomes a forensic investigator.

---

# Debugging Failed Tests

Suppose a test fails.

Bad prompt:

```text id="zrmglh"
Fix the test.
```

Good prompt:

```text id="j0ahc9"
Analyze the failing test.

Determine whether:

• the implementation is wrong

• the test is wrong

• both are wrong

Explain your reasoning.
```

This avoids blindly modifying code.

---

# The "Don't Fix It Yet" Rule

One of the strongest debugging habits:

Forbid code changes during investigation.

Prompt:

```text id="c5os2h"
You are not allowed to modify code.

Your only goal is to understand the failure.

Explain what you discover.
```

This forces evidence gathering.

---

# When Cline Suggests a Fix

After investigation:

Ask:

```text id="i4yfr1"
What evidence supports this fix?

What alternative explanations remain possible?

What is your confidence level?
```

Now you're evaluating the solution.

Not just accepting it.

---

# Verifying the Fix

After implementation:

```text id="vhjks8"
Verify:

• original bug resolved

• no regressions introduced

• existing functionality preserved

Document what was tested.
```

Verification is mandatory.

---

# Creating Regression Tests

Every significant bug should result in a test.

Prompt:

```text id="e1mjfr"
Create a regression test that fails before the fix and passes afterward.

Explain how it prevents future regressions.
```

This turns today's bug into tomorrow's protection.

---

# Advanced Debugging Prompt

One of my favorites:

```text id="7cxm1r"
Assume the obvious explanation is wrong.

Identify three alternative root causes.

Rank them by likelihood.

Explain how to eliminate each possibility.
```

This often uncovers hidden issues.

---

# Debugging Large Projects

As projects grow, debugging changes.

Instead of:

```text id="j7xgpp"
Fix this exception.
```

You'll ask:

```text id="1wjw5u"
Identify the subsystem responsible.

Explain how the subsystem is intended to work.

Determine where reality diverges from expectations.
```

This is much closer to how senior engineers debug production systems.

---

# A Complete Debugging Workflow

Suppose users report that image generation intermittently fails.

A professional Cline workflow might be:

### Investigation

```text id="mxp6qt"
Investigate the failure.

Collect evidence.

Do not modify code.
```

↓

### Reproduction

```text id="w8g9g7"
Determine reliable reproduction steps.
```

↓

### Root Cause Analysis

```text id="jlwm5k"
Identify immediate and underlying causes.
```

↓

### Design Fix

```text id="h1i8xw"
Recommend the smallest safe fix.

Discuss alternatives.
```

↓

### Implement

```text id="ywmv5m"
Implement the approved fix.

Verify behavior.
```

↓

### Regression Protection

```text id="8a7rqp"
Create a regression test.
```

This process scales from toy projects to enterprise systems.

---

# Chapter Summary

Debugging is fundamentally an exercise in understanding, not coding. The best use of Cline during debugging is not to immediately generate fixes, but to investigate, collect evidence, trace execution paths, analyze stack traces, identify root causes, and evaluate alternatives before making any changes. By separating investigation from implementation and verification, you dramatically improve the quality of your fixes and reduce the likelihood of introducing new bugs.

In the next chapter, we'll shift from fixing problems to preventing them. We'll explore how to use Cline for **test-driven development (TDD)**, automated testing, regression prevention, and building confidence in your codebase as it grows. This is where Cline begins to function not only as a developer and debugger, but also as a quality engineer.
