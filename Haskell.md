
# 🟣 Haskell Programming Language — Comprehensive Tutorial

Haskell is a **purely functional**, **statically typed**, **lazy** programming language designed for correctness, abstraction, and mathematical clarity.

It’s widely used in:

* Compilers and language tooling
* Financial systems
* Research and AI
* Distributed and concurrent systems

---

# 1. 🔧 Installing Haskell

The easiest way is **GHCup** (manages compiler, tools, libraries):

👉 [https://www.haskell.org/ghcup/](https://www.haskell.org/ghcup/)

It installs:

* `ghc` – the compiler
* `ghci` – interactive REPL
* `cabal` – build system
* `stack` – project manager

Verify:

```bash
ghc --version
ghci
```

---

# 2. ▶️ Hello World

### hello.hs

```haskell
main :: IO ()
main = putStrLn "Hello, Haskell!"
```

Run:

```bash
runghc hello.hs
```

---

# 3. 🧮 Basic Syntax

### Variables (immutable)

```haskell
x = 5
y = x + 3
```

### Functions

```haskell
add a b = a + b
square x = x * x
```

### If expressions

```haskell
absVal x = if x < 0 then -x else x
```

### Comments

```haskell
-- single line
{- multi
   line -}
```

---

# 4. 🧠 Types and Type Signatures

Haskell is **strongly and statically typed**.

```haskell
age :: Int
age = 42

add :: Int -> Int -> Int
add a b = a + b
```

Common types:

| Type           | Meaning                 |
| -------------- | ----------------------- |
| Int            | Integer (machine sized) |
| Integer        | Arbitrary precision     |
| Float / Double | Floating point          |
| Bool           | Boolean                 |
| Char           | Character               |
| String         | [Char]                  |

Check types in GHCI:

```haskell
:t add
```

---

# 5. 📦 Lists and Tuples

### Lists

```haskell
nums = [1,2,3,4]
names = ["Alice", "Bob"]

head nums
tail nums
length nums
```

### Ranges

```haskell
[1..10]
[0,2..20]
```

### Tuples

```haskell
point = (3,4)
fst point
snd point
```

---

# 6. 🔁 Recursion

Haskell uses recursion instead of loops.

```haskell
factorial 0 = 1
factorial n = n * factorial (n-1)
```

```haskell
sumList [] = 0
sumList (x:xs) = x + sumList xs
```

---

# 7. 🧩 Pattern Matching

```haskell
describe 0 = "zero"
describe 1 = "one"
describe _ = "many"
```

```haskell
first (x,_,_) = x
```

---

# 8. 🏗 Guards

Cleaner conditionals:

```haskell
grade score
  | score >= 90 = "A"
  | score >= 80 = "B"
  | score >= 70 = "C"
  | otherwise  = "F"
```

---

# 9. 🧰 Higher-Order Functions

Functions that take functions.

```haskell
applyTwice f x = f (f x)
```

Built-in:

```haskell
map (*2) [1..5]
filter even [1..20]
foldl (+) 0 [1..10]
```

---

# 10. 🧵 Lazy Evaluation

Haskell evaluates **only when needed**.

```haskell
ones = 1 : ones
take 5 ones
```

Infinite lists are normal.

---

# 11. 📐 Algebraic Data Types (ADT)

### Custom types

```haskell
data Color = Red | Green | Blue
```

```haskell
data Shape
  = Circle Float
  | Rectangle Float Float
```

```haskell
area (Circle r) = pi * r^2
area (Rectangle w h) = w * h
```

---

# 12. 🧬 Records

```haskell
data Person = Person
  { name :: String
  , age  :: Int
  } deriving Show
```

```haskell
p = Person "Chris" 30
name p
```

---

# 13. 🧪 Type Classes (Haskell’s Interfaces)

```haskell
class Describable a where
  describe :: a -> String
```

```haskell
instance Describable Bool where
  describe True = "yes"
  describe False = "no"
```

Common typeclasses:

* `Eq`, `Ord`, `Show`, `Read`
* `Functor`, `Applicative`, `Monad`

---

# 14. 🎁 Functors, Applicatives, Monads

### Functor

```haskell
fmap (+1) [1,2,3]
```

### Applicative

```haskell
(+) <$> [1,2] <*> [10,20]
```

### Monad (do notation)

```haskell
main = do
  putStrLn "Name?"
  name <- getLine
  putStrLn ("Hello " ++ name)
```

Monad examples:

* `IO`
* `Maybe`
* `Either`
* `List`

---

# 15. ❓ Maybe and Either

```haskell
safeDiv _ 0 = Nothing
safeDiv a b = Just (a `div` b)
```

```haskell
case safeDiv 10 0 of
  Nothing -> putStrLn "Error"
  Just x  -> print x
```

```haskell
Right 42
Left "Invalid input"
```

---

# 16. 📂 Modules

```haskell
module MathUtils (add, square) where
```

```haskell
import MathUtils
import qualified Data.Map as Map
```

---

# 17. 🧵 IO and Files

```haskell
main = do
  contents <- readFile "test.txt"
  writeFile "out.txt" (map toUpper contents)
```

---

# 18. ⚡ Concurrency

```haskell
import Control.Concurrent

main = do
  forkIO (printNumbers)
  printLetters
```

```haskell
printNumbers = mapM_ print [1..10]
printLetters = mapM_ putChar ['a'..'z']
```

---

# 19. 🏗 Project Structure

Create project:

```bash
cabal init
cabal build
cabal run
```

Structure:

```
src/
  Main.hs
test/
```

---

# 20. 🧪 Testing

```haskell
import Test.HUnit

test1 = TestCase (assertEqual "add" 4 (add 2 2))
```

Property testing:

```haskell
import Test.QuickCheck
quickCheck (\x -> x + 0 == (x :: Int))
```

---

# 21. 🧠 Advanced Topics

* Lazy vs strict evaluation
* Monoid & Semigroup
* Lenses
* STM (Software Transactional Memory)
* Type families
* GADTs
* Free monads
* Category theory patterns

---

# 22. 🚀 Example Mini Project

```haskell
import Data.List

wordCount text =
  sortOn snd $
  map (\w -> (head w, length w)) $
  group $
  sort $
  words text

main = do
  txt <- readFile "file.txt"
  print (wordCount txt)
```

---

# 23. 📚 Learning Resources

* “Learn You a Haskell for Great Good”
* “Programming in Haskell” – Graham Hutton
* “Category Theory for Programmers”
* [https://hoogle.haskell.org](https://hoogle.haskell.org) (type-based search)

---

# ✅ What makes Haskell special

* No nulls, no undefined behavior
* Fearless refactoring
* Extremely powerful type system
* Lazy evaluation
* Excellent concurrency model


Just tell me 👍
