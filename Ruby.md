
# 🧠 Ruby Programming — Comprehensive Tutorial

Ruby is a high-level, interpreted, object-oriented programming language known for its clean syntax and developer happiness. It’s widely used for scripting, automation, web development (Ruby on Rails), APIs, and tooling.

---

# 📦 1. Installing Ruby

### ✅ Windows

* Download from: rubyinstaller.org
* Choose the version with **DevKit**
* Check installation:

```bash
ruby -v
```

### ✅ macOS

```bash
brew install ruby
```

### ✅ Linux (Ubuntu/Debian)

```bash
sudo apt install ruby-full
```

Verify:

```bash
ruby -v
```

---

# ▶️ 2. Your First Ruby Program

Create a file called:

```bash
hello.rb
```

```ruby
puts "Hello, world!"
```

Run:

```bash
ruby hello.rb
```

---

# 🔤 3. Variables and Data Types

Ruby is dynamically typed.

```ruby
name = "Chris"
age = 30
price = 19.99
active = true
nothing = nil
```

### Common Types

* String
* Integer
* Float
* Boolean (`true`, `false`)
* Nil
* Array
* Hash

Check a type:

```ruby
puts name.class
```

---

# ➗ 4. Operators

```ruby
a = 10
b = 3

a + b
a - b
a * b
a / b
a % b
a ** b   # exponent
```

Comparison:

```ruby
a == b
a != b
a > b
a < b
```

Logical:

```ruby
true && false
true || false
!true
```

---

# 🧵 5. Strings

```ruby
first = "Ruby"
last = "Lang"

puts first + " " + last
puts "#{first} #{last}"
```

Useful methods:

```ruby
text.upcase
text.downcase
text.length
text.include?("Ru")
text.split(" ")
```

---

# 📦 6. Arrays

```ruby
numbers = [1, 2, 3, 4]
mixed = [1, "two", true]

numbers << 5
numbers.push(6)
numbers.pop

puts numbers[0]
```

Iterating:

```ruby
numbers.each do |n|
  puts n
end
```

Common methods:

```ruby
map
select
reject
include?
sort
```

---

# 🗂️ 7. Hashes (Dictionaries)

```ruby
person = {
  name: "Chris",
  age: 30,
  active: true
}

puts person[:name]
person[:age] = 31
```

Iterating:

```ruby
person.each do |key, value|
  puts "#{key} => #{value}"
end
```

---

# 🔀 8. Control Flow

### If / Else

```ruby
if age >= 18
  puts "Adult"
else
  puts "Minor"
end
```

### Unless

```ruby
puts "Not active" unless active
```

### Case

```ruby
case grade
when "A"
  puts "Excellent"
when "B"
  puts "Good"
else
  puts "Try harder"
end
```

---

# 🔁 9. Loops

```ruby
5.times do
  puts "Hello"
end

while x < 10
  puts x
  x += 1
end

for i in 1..5
  puts i
end
```

Ranges:

```ruby
(1..5)    # inclusive
(1...5)   # exclusive
```

---

# 🧩 10. Methods (Functions)

```ruby
def greet(name)
  "Hello, #{name}"
end

puts greet("Chris")
```

Default arguments:

```ruby
def power(base, exp = 2)
  base ** exp
end
```

Return is implicit.

---

# 🧱 11. Classes and Objects

```ruby
class Person
  attr_accessor :name, :age

  def initialize(name, age)
    @name = name
    @age = age
  end

  def greet
    "Hi, I'm #{@name}"
  end
end

p = Person.new("Chris", 30)
puts p.greet
```

---

# 🧬 12. Inheritance

```ruby
class Animal
  def speak
    "Some sound"
  end
end

class Dog < Animal
  def speak
    "Woof"
  end
end
```

---

# 🔒 13. Modules and Mixins

```ruby
module Flyable
  def fly
    "I'm flying"
  end
end

class Bird
  include Flyable
end
```

Used for:

* Namespacing
* Reusable behavior

---

# 📁 14. Files and I/O

Write:

```ruby
File.write("test.txt", "Hello Ruby")
```

Read:

```ruby
content = File.read("test.txt")
puts content
```

Append:

```ruby
File.open("log.txt", "a") do |file|
  file.puts "New entry"
end
```

---

# ⚠️ 15. Error Handling

```ruby
begin
  x = 10 / 0
rescue ZeroDivisionError => e
  puts e.message
ensure
  puts "Done"
end
```

Raise:

```ruby
raise "Something went wrong"
```

---

# 🧰 16. Blocks, Procs, and Lambdas

Block:

```ruby
[1,2,3].each { |n| puts n }
```

Proc:

```ruby
my_proc = Proc.new { |x| puts x }
my_proc.call(5)
```

Lambda:

```ruby
square = ->(x) { x * x }
puts square.call(4)
```

---

# 🧵 17. Symbols

```ruby
:name
:age
```

* Immutable
* Memory efficient
* Commonly used as hash keys

---

# 📦 18. RubyGems and Bundler

Install a gem:

```bash
gem install httparty
```

Bundler:

```bash
gem install bundler
bundle init
```

Gemfile:

```ruby
gem "sinatra"
```

```bash
bundle install
```

---

# 🌐 19. Building a Simple CLI App

```ruby
puts "Enter your name:"
name = gets.chomp

puts "Enter your age:"
age = gets.chomp.to_i

puts "Hello #{name}, next year you will be #{age + 1}"
```

---

# 🕸️ 20. Intro to Web Development (Sinatra)

```bash
gem install sinatra
```

```ruby
require "sinatra"

get "/" do
  "Hello from Ruby!"
end
```

Run:

```bash
ruby app.rb
```

Visit: [http://localhost:4567](http://localhost:4567)

---

# 🧪 21. Testing with Minitest

```ruby
require "minitest/autorun"

class TestMath < Minitest::Test
  def test_add
    assert_equal 4, 2 + 2
  end
end
```

Run:

```bash
ruby test.rb
```

---

# ⚡ 22. Useful Ruby Idioms

```ruby
arr.map(&:upcase)
hash.transform_values(&:to_i)
array.select(&:even?)
```

Safe navigation:

```ruby
user&.profile&.email
```

---

# 🏗️ 23. Project Structure Example

```
project/
  lib/
  models/
  services/
  app.rb
  Gemfile
```

---

# 🚀 24. Where Ruby Is Commonly Used

* Ruby on Rails web apps
* APIs
* DevOps scripts
* Web scraping
* Automation
* CLI tools

---

# 📚 25. Learning Path (Recommended)

1. Core syntax & collections
2. OOP & modules
3. File handling
4. Gems & Bundler
5. Testing
6. Sinatra → Rails
7. Performance & metaprogramming

---

# 🧠 26. Advanced Topics to Explore

* Metaprogramming
* Threads & fibers
* Refinements
* C extensions
* Rails internals
* WebSockets
* Background jobs
* Ractors

---

# 🧩 27. Capstone Practice Ideas

* CLI password manager
* Web scraper
* REST API
* Blog system
* Chat server
* Stock tracker
* Discord bot
* Ruby on Rails SaaS app

