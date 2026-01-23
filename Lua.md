
# 🟦 Lua Programming Language – Comprehensive Tutorial

---

# 1. What is Lua?

**Lua** is a lightweight, fast, embeddable scripting language designed to be:

* ✔ Simple
* ✔ Extremely fast
* ✔ Portable
* ✔ Easy to embed into C/C++
* ✔ Popular in game engines, embedded systems, and scripting

Lua is widely used in:

* 🎮 Game development (Roblox, World of Warcraft, Love2D)
* ⚙ Embedded systems
* 🌐 Web servers (OpenResty / Nginx)
* 🤖 Tools and automation

---

# 2. Installing Lua

## Windows

1. Download from: [https://www.lua.org/download.html](https://www.lua.org/download.html)
2. Recommended: **LuaBinaries** or **Lua for Windows**
3. Verify:

```bash
lua -v
```

## macOS

```bash
brew install lua
```

## Linux

```bash
sudo apt install lua5.4
```

Run interactive mode:

```bash
lua
```

---

# 3. Your First Lua Program

Create `hello.lua`:

```lua
print("Hello, Lua!")
```

Run:

```bash
lua hello.lua
```

---

# 4. Lua Basics

## Comments

```lua
-- single line comment

--[[ 
multi-line
comment
]]
```

## Variables and Types

```lua
x = 10
name = "Chris"
pi = 3.14159
active = true
nothing = nil
```

Lua has 8 core types:

* nil
* boolean
* number
* string
* function
* table
* userdata
* thread

Check type:

```lua
print(type(x))
```

---

# 5. Strings

```lua
s = "Hello"
t = 'World'

multi = [[
This is
multi-line
]]

print(s .. " " .. t)
print(#s) -- length
```

Common functions:

```lua
string.upper("lua")
string.lower("LUA")
string.sub("Hello", 2, 4)
string.find("hello", "ll")
```

---

# 6. Math and Operators

```lua
a = 10 + 5
b = 10 - 5
c = 10 * 5
d = 10 / 5
e = 10 // 3   -- integer division
f = 10 % 3
g = 2^3
```

Relational:

```lua
== ~= < > <= >=
```

Logical:

```lua
and or not
```

---

# 7. Control Structures

## If

```lua
age = 20

if age >= 18 then
    print("Adult")
elseif age >= 13 then
    print("Teen")
else
    print("Child")
end
```

## While

```lua
i = 1
while i <= 5 do
    print(i)
    i = i + 1
end
```

## Repeat

```lua
repeat
    i = i + 1
until i > 10
```

## For

```lua
for i = 1, 10, 2 do
    print(i)
end
```

---

# 8. Functions

```lua
function greet(name)
    return "Hello " .. name
end

print(greet("Lua"))
```

Anonymous:

```lua
add = function(a, b)
    return a + b
end
```

Multiple returns:

```lua
function stats(a, b)
    return a+b, a*b
end

sum, product = stats(3, 4)
```

---

# 9. Tables (Lua’s Core Data Structure)

Tables act as:

* arrays
* dictionaries
* objects
* modules

## Arrays

```lua
nums = {10, 20, 30}
print(nums[1])
```

## Dictionaries

```lua
person = {
    name = "Chris",
    age = 30
}

print(person.name)
print(person["age"])
```

## Loops

```lua
for i, v in ipairs(nums) do
    print(i, v)
end

for k, v in pairs(person) do
    print(k, v)
end
```

## Nested tables

```lua
game = {
    player = { x = 10, y = 20 },
    score = 0
}
```

---

# 10. Modules and Files

### math_utils.lua

```lua
local M = {}

function M.square(x)
    return x * x
end

return M
```

### main.lua

```lua
local math_utils = require("math_utils")
print(math_utils.square(5))
```

---

# 11. Error Handling

```lua
function risky()
    error("Something went wrong")
end

status, err = pcall(risky)

if not status then
    print("Error:", err)
end
```

---

# 12. Metatables (Operator Overloading)

```lua
Vector = {}
Vector.__index = Vector

function Vector.new(x, y)
    return setmetatable({x=x, y=y}, Vector)
end

function Vector.__add(a, b)
    return Vector.new(a.x + b.x, a.y + b.y)
end

v1 = Vector.new(1, 2)
v2 = Vector.new(3, 4)
v3 = v1 + v2

print(v3.x, v3.y)
```

---

# 13. Object-Oriented Style

```lua
Account = {}
Account.__index = Account

function Account.new(balance)
    return setmetatable({balance = balance}, Account)
end

function Account:deposit(amount)
    self.balance = self.balance + amount
end

a = Account.new(100)
a:deposit(50)
print(a.balance)
```

---

# 14. Coroutines (Concurrency)

```lua
co = coroutine.create(function()
    for i = 1, 3 do
        print("Coroutine:", i)
        coroutine.yield()
    end
end)

coroutine.resume(co)
coroutine.resume(co)
coroutine.resume(co)
```

---

# 15. File I/O

```lua
file = io.open("data.txt", "w")
file:write("Hello file\n")
file:close()

file = io.open("data.txt", "r")
print(file:read("*a"))
file:close()
```

---

# 16. Standard Libraries

* `math` → sin, cos, random
* `string` → manipulation
* `table` → insert, remove, sort
* `os` → system time, execute
* `io` → file access
* `coroutine` → threading

Example:

```lua
print(math.random(1, 100))
table.insert(nums, 99)
```

---

# 17. Embedding Lua in C (Core Use Case)

```c
#include <lua.h>
#include <lauxlib.h>
#include <lualib.h>

int main() {
    lua_State *L = luaL_newstate();
    luaL_openlibs(L);
    luaL_dofile(L, "script.lua");
    lua_close(L);
}
```

Compile:

```bash
gcc main.c -llua -lm -ldl
```

---

# 18. Practical Project Examples

## 🧮 Calculator

```lua
while true do
    io.write("> ")
    expr = io.read()
    if expr == "exit" then break end
    print(load("return " .. expr)())
end
```

## 📁 File Scanner

```lua
for file in io.popen("dir"):lines() do
    print(file)
end
```

## 🎮 Love2D Game Skeleton

```lua
function love.load()
end

function love.update(dt)
end

function love.draw()
    love.graphics.print("Hello Game!", 100, 100)
end
```

---

# 19. Lua Best Practices

* Use `local` aggressively
* Avoid global pollution
* Modularize large projects
* Use metatables sparingly
* Validate inputs
* Use `pcall` around risky code

---

# 20. Learning Path

1. Core syntax
2. Tables deeply
3. Modules
4. Metatables
5. Coroutines
6. Embedding Lua
7. Game scripting or automation projects

---

# 21. Recommended Resources

* 📘 Programming in Lua – Roberto Ierusalimschy
* 🌐 lua.org
* 🎮 Love2D.org
* 🧱 OpenResty.org
* 🧠 Lua Users Wiki
