# Part VI

# Chapter 11

# Test-Driven Development with Cline

This chapter is where Cline starts paying enormous dividends.

Many developers think AI writes code faster.

That's true.

But the real productivity gain comes from writing **correct code** faster.

The biggest difference between junior and senior developers isn't typing speed.

It's confidence.

Senior developers can make large changes confidently because they have a safety net.

That safety net is automated testing.

---

# The AI Trap

Here's a pattern you'll see repeatedly:

AI writes code.

Developer tests it manually.

Everything seems fine.

Two weeks later...

A completely unrelated feature breaks.

Nobody notices.

Why?

Because there were no automated tests.

---

# A Better Workflow

Instead of

```text id="tdd001"
Write Code

↓

Test Manually

↓

Ship
```

we're going to use

```text id="tdd002"
Write Test

↓

Watch Test Fail

↓

Write Code

↓

Watch Test Pass

↓

Refactor

↓

Commit
```

This is classic Test-Driven Development (TDD), and Cline is exceptionally good at it because it can generate tests, run them, interpret failures, and iterate.

---

# Why TDD Works Well with Cline

Humans often dislike writing tests.

AI doesn't.

In fact, Cline is often better at writing repetitive test cases than humans are.

The trick is making sure it's testing the **right behavior**.

---

# Our First Testing Goal

We're not going to test Flask yet.

We're going to test the renderer.

Why?

Because the renderer is pure logic.

Pure logic is easy to test.

---

# Good Architecture Pays Off

Remember our architecture?

```text id="tdd003"
Flask

↓

Renderer

↓

Image
```

Since Flask and the renderer are separate, we can test the renderer without starting a web server.

That's exactly why we designed it this way.

---

# Before Writing Tests

Don't ask:

> Write unit tests.

Instead ask:

```text id="tdd004"
Inspect the renderer module.

Identify the public behaviors that should be tested.

Recommend the smallest useful test suite.

Do not create any files yet.
```

Again...

Planning first.

---

# Expected Response

Cline might recommend tests like:

```text id="tdd005"
✓ Generates an image

✓ Image dimensions are correct

✓ Deterministic output

✓ Invalid parameters raise exceptions

✓ Large images generate successfully
```

Excellent.

Approve only the first few.

---

# Don't Chase Coverage

Many teams obsess over:

```text id="tdd006"
100% Code Coverage
```

Coverage is useful.

But it isn't the goal.

You can have

100%

coverage

and still have terrible tests.

Instead ask:

```text id="tdd007"
What behaviors are most important to verify?
```

Behavior matters more than line coverage.

---

# Prompt

# Create the First Test

```text id="tdd008"
Create the smallest meaningful unit test.

Verify that the renderer produces an image of the requested dimensions.

Run the test.

Explain the result.
```

Simple.

Focused.

---

# Watching Cline Work

You'll likely see something like:

```text id="tdd009"
Reading

mandelbrot.py
```

↓

```text id="tdd010"
Creating

tests/test_renderer.py
```

↓

```text id="tdd011"
Running

pytest
```

↓

```text id="tdd012"
Test Passed
```

Notice that Cline is doing far more than generating code.

It's executing the entire engineering workflow.

---

# Red, Green, Refactor

Classic TDD has three stages.

```text id="tdd013"
Red

↓

Green

↓

Refactor
```

Let's look at each one.

---

# Red

Write a test that fails.

Prompt:

```text id="tdd014"
Write a test for invalid image dimensions.

Run the test.

Do not modify the renderer.
```

Ideally

the test fails.

Good.

---

# Why Failure Is Good

Many beginners become worried when tests fail.

In TDD,

failure is success.

The failing test proves the test is capable of detecting the bug.

---

# Green

Now ask

```text id="tdd015"
Modify the renderer only enough to satisfy the failing test.

Avoid unrelated improvements.

Run the test suite afterward.
```

Notice

"only enough."

That's classic TDD.

---

# Refactor

Once tests pass

Prompt:

```text id="tdd016"
Review the renderer.

Refactor for readability.

Preserve behavior.

Run all tests afterward.
```

Tests allow us to refactor confidently.

---

# Test Behavior, Not Implementation

Bad test:

```text id="tdd017"
Verify the renderer uses NumPy arrays.
```

Why bad?

Because we don't care.

Maybe someday it won't.

Good test:

```text id="tdd018"
Verify the generated image has the requested dimensions.
```

Now the implementation can evolve freely.

---

# A Great Testing Prompt

One of my favorites:

```text id="tdd019"
Assume another developer completely rewrites the renderer.

Which tests should still pass?

Generate only those tests.
```

This naturally encourages behavior-based testing.

---

# Property-Based Thinking

Instead of writing one test:

Think about properties.

Example:

```text id="tdd020"
Given identical parameters,

the renderer should always generate identical output.
```

Notice

We're testing a mathematical property.

Not one specific image.

---

# Edge Cases

Ask Cline

```text id="tdd021"
Identify edge cases the current tests do not cover.

Rank them by importance.

Do not write code.
```

Possible answers:

* zero width
* zero height
* negative iterations
* extremely large images
* floating point overflow

Excellent.

---

# Regression Tests

Suppose we fix a bug.

Immediately ask:

```text id="tdd022"
Create a regression test that reproduces the original bug.

Verify it fails before the fix and passes afterward.
```

Now the bug never returns.

---

# Mutation Thinking

An advanced technique.

Prompt:

```text id="tdd023"
If one line of the renderer were accidentally changed,

which existing tests would detect the mistake?

Which mistakes would go unnoticed?
```

This often reveals weaknesses in the test suite.

---

# Testing Flask

Notice we've delayed testing Flask.

Why?

Because web applications have layers.

Test them independently.

```text id="tdd024"
Renderer

↓

Route

↓

HTTP

↓

Browser
```

Each layer deserves its own tests.

---

# Integration Tests

Eventually we'll test the whole application.

Prompt:

```text id="tdd025"
Create an integration test.

Start the Flask application.

Request the image endpoint.

Verify

HTTP status

Content-Type

Image dimensions

Do not duplicate unit tests.
```

Notice the distinction.

Unit tests verify the renderer.

Integration tests verify the application.

---

# Using Cline to Review Tests

One of the most valuable prompts in this chapter:

```text id="tdd026"
Review the current test suite.

Identify

Untested behavior

Duplicate tests

Fragile tests

Missing edge cases

Do not modify anything.
```

Now Cline becomes your QA engineer.

---

# Continuous Verification

As the project grows,

every feature should follow the same rhythm.

```text id="tdd027"
Plan

↓

Write Test

↓

Fail

↓

Implement

↓

Pass

↓

Review

↓

Commit
```

Notice how similar this looks to our earlier engineering workflow.

---

# The Testing Pyramid

As our project expands, we'll gradually build a balanced suite of tests:

```text id="tdd028"
          Browser Tests
        -----------------
      Integration Tests
    -----------------------
         Unit Tests
```

Most of your tests should be unit tests because they're fast, deterministic, and easy to diagnose.

---

# A Complete TDD Conversation

Suppose we want to add support for custom color palettes.

Rather than asking Cline to implement the feature directly, the conversation might look like this:

**Planning**

```text id="tdd029"
Inspect the renderer.

Recommend the behaviors that should be tested before implementing color palettes.

Wait for approval.
```

↓

**Write Tests**

```text id="tdd030"
Create tests for the approved behaviors.

Run them.

Do not modify the renderer.
```

↓

**Implementation**

```text id="tdd031"
Implement only enough functionality to satisfy the failing tests.

Run the full test suite afterward.
```

↓

**Review**

```text id="tdd032"
Review both the implementation and the tests.

Identify any unnecessary complexity or missing edge cases.

Do not modify code.
```

This workflow keeps implementation driven by observable behavior rather than by assumptions.

---

# Chapter Summary

Test-driven development complements Cline remarkably well because it transforms vague coding tasks into concrete, verifiable engineering work. By asking Cline to identify behaviors, write focused tests, observe failures, implement the smallest possible changes, and continuously review the test suite, you create a development process that is both faster and more reliable. The result isn't just more tests—it's a codebase that can evolve with confidence.

In the next chapter, we'll return to our Mandelbrot project and introduce **Docker**. Rather than simply writing a `Dockerfile`, we'll use Cline to evaluate different containerization strategies, build a production-quality image, optimize build performance, and establish workflows for debugging and iterating inside containers. This will also introduce techniques for having Cline reason about deployment environments rather than just application code.
