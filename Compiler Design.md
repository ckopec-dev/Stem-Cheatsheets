# Compiler Design Cheatsheet

# 🖥️ Compiler Design Handbook
*A complete guide — from beginner fundamentals to advanced compiler construction.*

---

## 📌 Overview
This handbook covers:
- **Beginner Concepts** – What a compiler is, how it works, key phases.
- **Advanced Concepts** – Optimizations, JIT, LLVM, DSLs, real-world compiler architectures.

---

## 1️⃣ Introduction to Compiler Design

### 🔹 Beginner
A **compiler** is a program that:
1. Reads **source code** written in a high-level language.
2. Checks for **syntax and semantic correctness**.
3. Produces **machine code**, **bytecode**, or another target language.

**Why use a compiler?**
- Efficiency: Machine-executable programs run faster than interpreted code.
- Portability: Same high-level code can be compiled for different platforms.

**Examples of Compilers:**
- GCC (C/C++)
- javac (Java)
- Clang (C/C++/Objective-C)

---

### 🔹 Advanced
Compilers can be categorized by:
- **Compilation Strategy:**
  - Ahead-of-Time (AOT)
  - Just-In-Time (JIT)
  - Hybrid (e.g., Java HotSpot)
- **Optimization Level:**
  - Debug builds vs Release builds
  - Profile-Guided Optimization (PGO)
- **Target Platforms:**
  - Native binaries
  - Virtual machines (JVM, CLR)
  - WebAssembly

---

## 2️⃣ Compiler Phases

### 2.1 Lexical Analysis

#### Beginner:
- Converts **character stream** into **tokens**.
- Removes whitespace and comments.
- Uses **regular expressions** to define token patterns.

Example:
```text
int x = 5 + 10;
Tokens: [int] [identifier:x] [=] [5] [+] [10] [;]
```

#### Advanced:
- Implemented using **Finite Automata**.
- Performance tuned with **buffered I/O** and **token lookahead**.
- Tools: **Lex**, **Flex**, **ANTLR**.

---

### 2.2 Syntax Analysis

#### Beginner:
- Checks if tokens form a valid structure using **grammars**.
- Produces **Parse Tree** or **Abstract Syntax Tree (AST)**.

Parsing methods:
- **Top-down**: LL(1), Recursive Descent.
- **Bottom-up**: LR, SLR, LALR.

#### Advanced:
- Error recovery strategies: Panic mode, Phrase-level recovery.
- Ambiguity resolution in grammar.
- Parser generators: **Bison**, **ANTLR**.

---

### 2.3 Semantic Analysis

#### Beginner:
- Checks meaning according to language rules.
- Type checking, scope resolution.
- Uses a **symbol table**.

#### Advanced:
- Attribute grammars.
- Constant propagation.
- Inline type inference (e.g., C++ `auto`, TypeScript type inference).

---

### 2.4 Intermediate Code Generation

#### Beginner:
- Produces a platform-independent code representation.
- Common form: **Three-Address Code (TAC)**.

Example:
```text
t1 = 5
t2 = 10
t3 = t1 + t2
```

#### Advanced:
- Static Single Assignment (SSA) form.
- IR optimizations: Loop unrolling, strength reduction.
- LLVM IR, JVM bytecode.

---

### 2.5 Code Optimization

#### Beginner:
- Removes inefficiencies without changing output.
- Examples: Constant folding, Dead code elimination.

#### Advanced:
- Machine-independent optimizations: Loop fusion, Common subexpression elimination.
- Machine-dependent optimizations: Register allocation, Instruction scheduling.
- PGO (Profile-Guided Optimization) & LTO (Link-Time Optimization).

---

### 2.6 Code Generation

#### Beginner:
- Translates IR to target machine code.
- Assigns registers, generates instructions.

#### Advanced:
- Peephole optimization.
- Instruction selection using DAG covering.
- JIT compilation with runtime profiling.

---

### 2.7 Linking & Loading

#### Beginner:
- Combines object files into an executable.
- Resolves function calls and global variables.

#### Advanced:
- Dynamic linking.
- Relocation tables.
- Symbol resolution across shared libraries.

---

## 3️⃣ Compiler Architecture Diagram
```
Source Code
    ↓
Lexical Analysis → Syntax Analysis → Semantic Analysis
    ↓                 ↓                 ↓
Intermediate Code Generation → Code Optimization → Code Generation
    ↓
Linking → Executable
```

---

## 4️⃣ Advanced Compiler Design Topics

- **JIT Compilation** (used in JVM, .NET CLR, V8)
- **Self-hosting Compilers** (compilers written in the language they compile)
- **DSL (Domain-Specific Language) Design**
- **Garbage Collector Integration**
- **LLVM Compiler Infrastructure**
- **Parallel Compilation**
- **Cross-compilation** for embedded systems

---

## 5️⃣ Tools & Frameworks

| Tool      | Purpose                      |
|-----------|------------------------------|
| Lex/Flex  | Lexical analysis generator    |
| Yacc/Bison| Parser generator              |
| ANTLR     | Language recognition          |
| LLVM      | Compiler backend framework    |
| GCC/Clang | Full production compilers     |

---

## 6️⃣ Example: Building a Mini Compiler

**Language:** Simple arithmetic expressions

**Workflow:**
1. **Lexing:** Tokenize `3 + 4 * 5`.
2. **Parsing:** Build AST.
3. **Semantic Analysis:** Check operand types.
4. **Intermediate Code:**
    ```
    t1 = 4 * 5
    t2 = 3 + t1
    ```
5. **Optimization:** Constant folding → `t2 = 3 + 20`
6. **Code Generation:** Emit target instructions.

---

## 📚 References
- [Compilers: Principles, Techniques, and Tools (Dragon Book)](https://en.wikipedia.org/wiki/Compilers:_Principles,_Techniques,_and_Tools)
- [LLVM Project](https://llvm.org/)
- [ANTLR Documentation](https://www.antlr.org/)
- [GCC Internals](https://gcc.gnu.org/onlinedocs/)

---

✅ This handbook serves as **both a crash course for beginners** and a **technical reference for advanced compiler developers**.

