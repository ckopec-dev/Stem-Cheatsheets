
# 🧠 1. What Is Lisp?

**Lisp** stands for **LISt Processing**.
It was invented in 1958 by John McCarthy and is the second-oldest high-level programming language still in use.

Key ideas that make Lisp unique:

* Code is written as **lists**
* **Code = Data** (homoiconicity)
* Extremely powerful **macro system**
* Interactive development (REPL-driven)
* Excellent for AI, symbolic computation, compilers, automation, and research

Example Lisp code:

```lisp
(+ 2 3 4)
```

This is a **list**:

```
(operator operand operand operand)
```

---

# 🧰 2. Setting Up a Lisp Environment

### Recommended Common Lisp stack

* **SBCL** – fast Common Lisp compiler
* **Quicklisp** – package manager
* **Emacs + SLIME** (or VS Code + Alive) – IDE

### Quick setup (Windows/macOS/Linux)

1. Install SBCL: [https://www.sbcl.org](https://www.sbcl.org)
2. Install Quicklisp: [https://www.quicklisp.org](https://www.quicklisp.org)
3. Optional but powerful: Emacs + SLIME

Once installed, run:

```bash
sbcl
```

You’ll see the **REPL**:

```
* 
```

This is where Lisp shines.

---

# ▶️ 3. The REPL (Read–Eval–Print Loop)

Everything you type:

1. Is read as a list
2. Evaluated
3. Printed

Try:

```lisp
(+ 5 7)
```

```lisp
(* 4 (+ 2 3))
```

```lisp
(format t "Hello, Lisp!~%")
```

---

# 🧱 4. Lisp Syntax Fundamentals

### Atoms

```lisp
42
3.14
"hello"
foo
```

### Lists

```lisp
(1 2 3)
(+ 1 2)
(print "hi")
```

### Prefix notation

Instead of:

```
2 + 3
```

Lisp uses:

```lisp
(+ 2 3)
```

This makes the language extremely consistent and easy to parse.

---

# 📦 5. Variables and Data Types

### Variables

```lisp
(defparameter x 10)
(defvar y 20)

(setf x 30)
```

### Core data types

```lisp
42            ; integer
3.14          ; float
"hello"       ; string
#\A           ; character
t             ; true
nil           ; false / empty list
'(1 2 3)      ; list
#(1 2 3)      ; vector
```

---

# 🧮 6. Functions

### Defining functions

```lisp
(defun square (x)
  (* x x))
```

```lisp
(square 5)
```

### Multiple return values

```lisp
(defun divide (a b)
  (values (floor (/ a b)) (mod a b)))
```

```lisp
(multiple-value-bind (q r) (divide 17 5)
  (format t "Q=~a R=~a~%" q r))
```

---

# 🔁 7. Control Flow

### Conditionals

```lisp
(if (> x 10)
    "big"
    "small")
```

```lisp
(cond
  ((< x 0) 'negative)
  ((> x 0) 'positive)
  (t 'zero))
```

### Loops

```lisp
(loop for i from 1 to 5
      do (print i))
```

```lisp
(dotimes (i 5)
  (print i))
```

```lisp
(dolist (x '(a b c))
  (print x))
```

---

# 📚 8. Lists and Recursion

### Constructing lists

```lisp
(cons 1 '(2 3))   ; (1 2 3)
(list 1 2 3)
(car '(1 2 3))    ; 1
(cdr '(1 2 3))    ; (2 3)
```

### Recursion

```lisp
(defun factorial (n)
  (if (<= n 1)
      1
      (* n (factorial (- n 1)))))
```

### Mapping

```lisp
(mapcar #'square '(1 2 3 4))
```

---

# 🧰 9. Local Variables and Scope

```lisp
(let ((x 10)
      (y 20))
  (+ x y))
```

```lisp
(let* ((x 5)
       (y (+ x 3)))
  (* x y))
```

---

# 🏗️ 10. Structs, Objects, and CLOS

### Structures

```lisp
(defstruct person
  name
  age)
```

```lisp
(defparameter p (make-person :name "Chris" :age 30))
(person-name p)
```

---

### Common Lisp Object System (CLOS)

```lisp
(defclass animal ()
  ((name :initarg :name :accessor name)))

(defclass dog (animal)
  ((breed :initarg :breed :accessor breed)))

(defmethod speak ((d dog))
  (format t "~a says woof~%" (name d)))
```

```lisp
(defparameter rex (make-instance 'dog :name "Rex" :breed "Shepherd"))
(speak rex)
```

CLOS supports:

* Multiple dispatch
* Mixins
* Method combination
* Runtime class changes

---

# ⚡ 11. Macros (Lisp’s Superpower)

Macros transform **code before it runs**.

### Example macro

```lisp
(defmacro unless (condition &body body)
  `(if (not ,condition)
       (progn ,@body)))
```

Usage:

```lisp
(unless (> x 10)
  (print "small")
  (setf x 20))
```

Macros let you create **new language features**.

---

# 🧬 12. Code Is Data

```lisp
'(+ 1 2 3)
```

is a list, not executed.

```lisp
(eval '(+ 1 2 3))
```

Lisp programs can generate Lisp programs.

This is why Lisp dominates in:

* AI systems
* Symbolic engines
* Theorem provers
* Compilers
* DSLs

---

# 📂 13. Files, Packages, and Projects

### Files

```lisp
(load "myfile.lisp")
```

### Packages (namespaces)

```lisp
(defpackage :myapp
  (:use :cl))

(in-package :myapp)
```

### Quicklisp

```lisp
(ql:quickload :hunchentoot)
```

---

# 🌐 14. Real-World Lisp Examples

### Web server

```lisp
(ql:quickload :hunchentoot)

(hunchentoot:start
 (make-instance 'hunchentoot:easy-acceptor :port 8080))
```

---

### File processing

```lisp
(with-open-file (s "data.txt")
  (loop for line = (read-line s nil)
        while line
        do (print line)))
```

---

### Simple DSL

```lisp
(defmacro rule (if then)
  `(list :if ',if :then ',then))
```

```lisp
(rule (> temp 100) alarm)
```

---

# 🧠 15. How Lisp Is Evaluated

Lisp evaluates:

```
(list form form form)
```

Unless:

* It is quoted: `'(...)`
* It is a macro
* It is a special form (if, let, defun…)

Evaluation model:

1. Evaluate operator
2. Evaluate arguments
3. Apply function

---

# 🧪 16. Debugging and Introspection

```lisp
(trace factorial)
```

```lisp
(describe 'square)
```

```lisp
(apropos "string")
```

Lisp environments can inspect live objects, functions, and memory.

---

# 🏁 17. What To Learn Next

* Advanced macros (`gensym`, `macroexpand`)
* CLOS method combinations
* Writing compilers in Lisp
* Building DSLs
* Embedding Lisp in C/C++
* Exploring Scheme or Clojure

---

# 📘 18. Recommended Resources

* *Practical Common Lisp* – Peter Seibel
* *Structure and Interpretation of Computer Programs*
* *On Lisp* – Paul Graham
* *Land of Lisp* – Conrad Barski

---

# 🧩 Final Thoughts

Lisp isn’t just a language — it’s a **programmable programming language**.

Once macros and homoiconicity “click,” you stop asking:

> “How do I solve this in Lisp?”

and start asking:

> “What language should I create to solve this?”


