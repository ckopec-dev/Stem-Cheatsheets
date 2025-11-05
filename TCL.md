# Comprehensive TCL Tutorial

## Table of Contents

1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Basic Syntax](#basic-syntax)
4. [Variables and Data Types](#variables-and-data-types)
5. [Control Structures](#control-structures)
6. [Procedures (Functions)](#procedures-functions)
7. [Lists](#lists)
8. [Arrays and Dictionaries](#arrays-and-dictionaries)
9. [String Manipulation](#string-manipulation)
10. [File I/O](#file-io)
11. [Error Handling](#error-handling)
12. [Regular Expressions](#regular-expressions)
13. [Advanced Topics](#advanced-topics)
14. [Practical Examples](#practical-examples)

---

## Introduction

**TCL (Tool Command Language)** is a powerful, easy-to-learn scripting language designed by John Ousterhout in 1988. It's widely used for rapid prototyping, scripted applications, GUIs (with Tk), and testing. TCL is particularly popular in network management, EDA (Electronic Design Automation) tools, and embedded systems.

### Key Features

- Simple, consistent syntax
- Everything is a string
- Extensive string manipulation capabilities
- Built-in support for regular expressions
- Cross-platform compatibility
- Easy integration with C/C++
- Powerful GUI toolkit (Tk)

---

## Getting Started

### Installation

**Windows:**

- Download ActiveTcl from activestate.com
- Or install via package managers like Chocolatey: `choco install tcl`

**Linux:**

```bash
# Debian/Ubuntu
sudo apt-get install tcl

# Red Hat/Fedora
sudo dnf install tcl

# macOS
brew install tcl-tk
```

### Running TCL

**Interactive Shell:**

```bash
tclsh
```

**Running a Script:**

```bash
tclsh script.tcl
```

**Shebang for Unix/Linux:**

```tcl
#!/usr/bin/env tclsh
# Your code here
```

---

## Basic Syntax

### Core Principles

1. **Commands are words separated by whitespace**
2. **First word is the command, rest are arguments**
3. **Everything is a string**
4. **Substitutions happen before command execution**

### Comments

```tcl
# This is a single-line comment

# Multi-line comments use multiple # symbols
# or you can use the if 0 trick:
if 0 {
    This is a multi-line comment
    None of this code will execute
}
```

### Basic Commands

```tcl
# Print to console
puts "Hello, World!"

# Set variables
set name "Alice"
puts $name

# Command substitution with square brackets
set result [expr 5 + 3]
puts $result

# Multiple commands on one line (use semicolon)
set x 10; set y 20; puts [expr $x + $y]
```

### Substitutions

TCL performs three types of substitutions:

1. **Variable substitution** with `$`
2. **Command substitution** with `[ ]`
3. **Backslash substitution** for special characters

```tcl
set name "Bob"
puts "Hello, $name"                    # Variable substitution
puts "2 + 2 = [expr 2 + 2]"           # Command substitution
puts "Line 1\nLine 2"                  # Backslash substitution
```

### Quoting

```tcl
# Double quotes: Allow substitutions
set name "Alice"
puts "Hello, $name"                    # Output: Hello, Alice

# Curly braces: No substitutions
puts {Hello, $name}                    # Output: Hello, $name

# Useful for passing code blocks
if {$x > 10} {
    puts "x is greater than 10"
}
```

---

## Variables and Data Types

### Setting and Getting Variables

```tcl
# Set a variable
set myVar "Hello"

# Get a variable's value
puts $myVar

# You can also use set to get
puts [set myVar]

# Check if variable exists
if {[info exists myVar]} {
    puts "myVar exists"
}

# Unset (delete) a variable
unset myVar
```

### Variable Scope

```tcl
# Global scope
set globalVar "I'm global"

proc myProc {} {
    # Local scope
    set localVar "I'm local"
    
    # Access global variable
    global globalVar
    puts $globalVar
    
    # Or use upvar
    upvar globalVar gv
    puts $gv
}

myProc
# puts $localVar  # Error: localVar doesn't exist here
```

### Data Types

TCL treats everything as strings, but interprets them contextually:

```tcl
# Numbers
set integer 42
set float 3.14
set scientific 1.5e10

# Strings
set string "Hello, World!"

# Booleans (interpreted as 0 or non-zero)
set isTrue 1
set isFalse 0

# Lists (space or comma separated, enclosed in braces)
set myList {apple banana cherry}
set myList2 [list "apple" "banana" "cherry"]
```

---

## Control Structures

### If-Else

```tcl
set x 10

if {$x > 5} {
    puts "x is greater than 5"
} elseif {$x == 5} {
    puts "x equals 5"
} else {
    puts "x is less than 5"
}

# Ternary-like expression
set result [expr {$x > 5 ? "big" : "small"}]
```

### Switch

```tcl
set fruit "apple"

switch $fruit {
    apple {
        puts "It's an apple"
    }
    banana {
        puts "It's a banana"
    }
    orange -
    tangerine {
        puts "It's a citrus fruit"
    }
    default {
        puts "Unknown fruit"
    }
}

# Pattern matching
switch -glob $fruit {
    a* {puts "Starts with 'a'"}
    *e {puts "Ends with 'e'"}
}
```

### While Loop

```tcl
set i 0
while {$i < 5} {
    puts "i = $i"
    incr i
}

# Break and continue
set i 0
while {$i < 10} {
    incr i
    if {$i == 3} {continue}
    if {$i == 7} {break}
    puts $i
}
```

### For Loop

```tcl
# Standard for loop
for {set i 0} {$i < 5} {incr i} {
    puts "i = $i"
}

# Multiple variables
for {set i 0; set j 10} {$i < 5} {incr i; incr j -1} {
    puts "i=$i, j=$j"
}
```

### Foreach Loop

```tcl
# Iterate over list
set fruits {apple banana cherry}
foreach fruit $fruits {
    puts $fruit
}

# Multiple lists
set names {Alice Bob Charlie}
set ages {25 30 35}
foreach name $names age $ages {
    puts "$name is $age years old"
}

# Multiple variables per list
set pairs {1 2 3 4 5 6}
foreach {a b} $pairs {
    puts "$a + $b = [expr {$a + $b}]"
}
```

---

## Procedures (Functions)

### Basic Procedures

```tcl
# Define a procedure
proc greet {name} {
    puts "Hello, $name!"
}

# Call the procedure
greet "Alice"

# Return values
proc add {a b} {
    return [expr {$a + $b}]
}

set sum [add 5 3]
puts $sum
```

### Default Arguments

```tcl
proc greet {name {greeting "Hello"}} {
    puts "$greeting, $name!"
}

greet "Alice"              # Output: Hello, Alice!
greet "Bob" "Hi"           # Output: Hi, Bob!
```

### Variable Arguments

```tcl
proc sum {args} {
    set total 0
    foreach num $args {
        set total [expr {$total + $num}]
    }
    return $total
}

puts [sum 1 2 3 4 5]      # Output: 15
```

### Pass by Reference

```tcl
proc increment {varName} {
    upvar $varName var
    incr var
}

set x 10
increment x
puts $x                    # Output: 11
```

### Anonymous Procedures (Lambdas)

```tcl
# Using apply
set square [list {x} {expr {$x * $x}}]
puts [apply $square 5]     # Output: 25

# Inline lambda
puts [apply {{x y} {expr {$x + $y}}} 3 4]  # Output: 7
```

---

## Lists

Lists are fundamental in TCL. They're ordered collections of elements.

### Creating Lists

```tcl
# Method 1: Literal
set list1 {apple banana cherry}

# Method 2: list command (safer for special characters)
set list2 [list "apple" "banana" "cherry"]

# Method 3: Building incrementally
set list3 {}
lappend list3 apple
lappend list3 banana
lappend list3 cherry
```

### List Operations

```tcl
set fruits {apple banana cherry date elderberry}

# Length
puts [llength $fruits]              # Output: 5

# Access by index (0-based)
puts [lindex $fruits 0]             # Output: apple
puts [lindex $fruits end]           # Output: elderberry
puts [lindex $fruits end-1]         # Output: date

# Range
puts [lrange $fruits 1 3]           # Output: banana cherry date

# Search
puts [lsearch $fruits cherry]       # Output: 2 (index)
puts [lsearch -exact $fruits apple] # Output: 0

# Insert
set fruits [linsert $fruits 2 "blueberry"]
puts $fruits

# Replace
set fruits [lreplace $fruits 1 1 "blackberry"]
puts $fruits

# Append
lappend fruits "fig"
puts $fruits

# Sort
set numbers {5 2 8 1 9}
puts [lsort -integer $numbers]      # Output: 1 2 5 8 9
puts [lsort $fruits]                # Alphabetical sort

# Reverse
puts [lreverse $fruits]

# Join (convert list to string)
puts [join $fruits ", "]

# Split (convert string to list)
set str "a,b,c,d"
set list [split $str ","]
puts $list
```

### List Iteration

```tcl
set fruits {apple banana cherry}

foreach fruit $fruits {
    puts $fruit
}

# With index
set i 0
foreach fruit $fruits {
    puts "$i: $fruit"
    incr i
}
```

---

## Arrays and Dictionaries

### Arrays

TCL arrays are associative (like hash tables or dictionaries in other languages).

```tcl
# Set array elements
set person(name) "Alice"
set person(age) 30
set person(city) "New York"

# Access elements
puts $person(name)

# Check if element exists
if {[info exists person(name)]} {
    puts "Name exists"
}

# Get all keys
puts [array names person]

# Get all values
puts [array get person]

# Iterate over array
foreach key [array names person] {
    puts "$key: $person($key)"
}

# Array size
puts [array size person]

# Unset element
unset person(city)

# Unset entire array
array unset person
```

### Array Patterns

```tcl
set config(db.host) "localhost"
set config(db.port) 5432
set config(db.name) "mydb"
set config(app.name) "MyApp"
set config(app.version) "1.0"

# Get all keys matching pattern
puts [array names config db.*]
```

### Dictionaries (TCL 8.5+)

Dictionaries are ordered, nestable key-value pairs.

```tcl
# Create dictionary
set person [dict create name "Alice" age 30 city "New York"]

# Or use literal syntax
set person {name Alice age 30 city "New York"}

# Get value
puts [dict get $person name]

# Set value
dict set person age 31

# Check if key exists
if {[dict exists $person name]} {
    puts "Name exists"
}

# Get all keys
puts [dict keys $person]

# Get all values
puts [dict values $person]

# Size
puts [dict size $person]

# Iterate
dict for {key value} $person {
    puts "$key: $value"
}

# Remove key
set person [dict remove $person city]

# Merge dictionaries
set dict1 {a 1 b 2}
set dict2 {c 3 d 4}
set merged [dict merge $dict1 $dict2]

# Nested dictionaries
set employee {
    name "Bob"
    contact {
        email "bob@example.com"
        phone "555-1234"
    }
}

puts [dict get $employee contact email]
dict set employee contact phone "555-5678"
```

---

## String Manipulation

### Basic String Operations

```tcl
set str "Hello, World!"

# Length
puts [string length $str]           # Output: 13

# Index
puts [string index $str 0]          # Output: H
puts [string index $str end]        # Output: !

# Range
puts [string range $str 0 4]        # Output: Hello

# Find substring
puts [string first "World" $str]    # Output: 7
puts [string last "o" $str]         # Output: 8

# Replace
set newStr [string map {"World" "TCL"} $str]
puts $newStr                        # Output: Hello, TCL!

# Case conversion
puts [string toupper $str]
puts [string tolower $str]
puts [string totitle "hello world"] # Output: Hello World

# Trim whitespace
set padded "   hello   "
puts [string trim $padded]
puts [string trimleft $padded]
puts [string trimright $padded]

# Repeat
puts [string repeat "abc" 3]        # Output: abcabcabc

# Reverse
puts [string reverse $str]

# Check properties
puts [string is integer "123"]      # Output: 1
puts [string is alpha "abc"]        # Output: 1
puts [string is alnum "abc123"]     # Output: 1
```

### String Comparison

```tcl
set str1 "apple"
set str2 "banana"

# Equal
if {$str1 eq $str2} {
    puts "Equal"
} else {
    puts "Not equal"
}

# Compare (returns -1, 0, or 1)
puts [string compare $str1 $str2]   # Output: -1

# Case-insensitive compare
puts [string compare -nocase "ABC" "abc"]  # Output: 0

# Match (glob pattern)
if {[string match "a*" $str1]} {
    puts "Matches pattern"
}
```

### Format Strings

```tcl
# format works like C's printf
set name "Alice"
set age 30

puts [format "Name: %s, Age: %d" $name $age]
puts [format "%.2f" 3.14159]        # Output: 3.14
puts [format "%10s" "hello"]        # Right-aligned in 10 chars
puts [format "%-10s" "hello"]       # Left-aligned in 10 chars
puts [format "%08d" 42]             # Output: 00000042
```

### Scanning Strings

```tcl
# scan works like C's scanf
set str "Alice 30 Engineer"
scan $str "%s %d %s" name age job
puts "Name: $name, Age: $age, Job: $job"

# Extract numbers
set data "Temperature: 23.5 degrees"
scan $data "Temperature: %f degrees" temp
puts $temp
```

---

## File I/O

### Reading Files

```tcl
# Open file for reading
set fp [open "input.txt" r]

# Read entire file
set content [read $fp]
puts $content

# Close file
close $fp

# Read line by line
set fp [open "input.txt" r]
while {[gets $fp line] >= 0} {
    puts "Line: $line"
}
close $fp

# Shorter way to read entire file
set content [read [set fp [open "input.txt" r]]][close $fp]
```

### Writing Files

```tcl
# Open file for writing (creates/overwrites)
set fp [open "output.txt" w]
puts $fp "Hello, World!"
puts $fp "This is line 2"
close $fp

# Append mode
set fp [open "output.txt" a]
puts $fp "Appended line"
close $fp

# Write without newline
set fp [open "output.txt" w]
puts -nonewline $fp "No newline here"
close $fp
```

### File Operations

```tcl
# Check if file exists
if {[file exists "myfile.txt"]} {
    puts "File exists"
}

# File type
puts [file type "myfile.txt"]       # file, directory, link, etc.

# File size
puts [file size "myfile.txt"]

# File permissions
file attributes "myfile.txt" -permissions 0644

# Delete file
file delete "myfile.txt"

# Copy file
file copy "source.txt" "dest.txt"

# Rename/move file
file rename "old.txt" "new.txt"

# Directory operations
file mkdir "newdir"
file delete -force "olddir"

# Get file info
puts [file dirname "/path/to/file.txt"]    # /path/to
puts [file tail "/path/to/file.txt"]       # file.txt
puts [file extension "file.txt"]           # .txt
puts [file rootname "file.txt"]            # file

# Join paths
set path [file join "dir" "subdir" "file.txt"]

# Normalize path
puts [file normalize "../file.txt"]

# Get current directory
puts [pwd]

# Change directory
cd "/path/to/directory"

# List directory contents
foreach file [glob *] {
    puts $file
}

# List with pattern
foreach txtfile [glob *.txt] {
    puts $txtfile
}
```

### Binary I/O

```tcl
# Write binary data
set fp [open "binary.dat" wb]
puts -nonewline $fp [binary format "c*" {65 66 67}]  # ABC
close $fp

# Read binary data
set fp [open "binary.dat" rb]
set data [read $fp]
binary scan $data "c*" bytes
puts $bytes
close $fp
```

---

## Error Handling

### Try-Catch (TCL 8.6+)

```tcl
try {
    set result [expr {10 / 0}]
} on error {msg opts} {
    puts "Error occurred: $msg"
}

# Multiple handlers
try {
    # Some code
} trap {ARITH DIVZERO} {msg opts} {
    puts "Division by zero!"
} on error {msg opts} {
    puts "Other error: $msg"
} finally {
    puts "This always runs"
}
```

### Catch (Traditional)

```tcl
# catch returns 0 on success, 1 on error
if {[catch {expr {10 / 0}} result]} {
    puts "Error: $result"
} else {
    puts "Result: $result"
}

# Get error info
if {[catch {open "nonexistent.txt" r} result options]} {
    puts "Error: $result"
    puts "Error code: [dict get $options -errorcode]"
    puts "Error info: [dict get $options -errorinfo]"
}
```

### Throwing Errors

```tcl
proc divide {a b} {
    if {$b == 0} {
        error "Division by zero"
    }
    return [expr {$a / $b}]
}

if {[catch {divide 10 0} result]} {
    puts "Caught error: $result"
}

# Return with error code
proc myProc {} {
    return -code error -errorcode {CUSTOM ERROR_TYPE} "Custom error message"
}
```

---

## Regular Expressions

TCL has powerful built-in regex support.

### Basic Matching

```tcl
set str "The quick brown fox"

# Match
if {[regexp {brown} $str]} {
    puts "Found 'brown'"
}

# Case-insensitive
if {[regexp -nocase {BROWN} $str]} {
    puts "Found 'brown' (case-insensitive)"
}

# Capture groups
if {[regexp {(\w+) (\w+)} $str match word1 word2]} {
    puts "First word: $word1"   # Output: The
    puts "Second word: $word2"  # Output: quick
}

# All matches
set count [regexp -all {o} $str]
puts "Found 'o' $count times"
```

### Search and Replace

```tcl
set str "Hello World"

# Replace first occurrence
set newStr [regsub {o} $str "0"]
puts $newStr                    # Output: Hell0 World

# Replace all occurrences
set newStr [regsub -all {o} $str "0"]
puts $newStr                    # Output: Hell0 W0rld

# With capture groups
set str "2025-01-15"
set newStr [regsub {(\d{4})-(\d{2})-(\d{2})} $str {\2/\3/\1}]
puts $newStr                    # Output: 01/15/2025
```

### Common Patterns

```tcl
# Email validation
proc isValidEmail {email} {
    return [regexp {^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$} $email]
}

# Phone number
regexp {(\d{3})-(\d{3})-(\d{4})} "555-123-4567" match area prefix line

# Extract numbers
set str "Price: $19.99"
regexp {\$(\d+\.\d+)} $str match price
puts $price                     # Output: 19.99

# URL parsing
set url "https://example.com:8080/path?query=value"
regexp {^(\w+)://([^:]+):(\d+)(.+)$} $url match protocol host port path
```

---

## Advanced Topics

### Namespaces

```tcl
# Create namespace
namespace eval myNamespace {
    variable counter 0
    
    proc increment {} {
        variable counter
        incr counter
    }
    
    proc getCount {} {
        variable counter
        return $counter
    }
}

# Use namespace
myNamespace::increment
puts [myNamespace::getCount]

# Import commands
namespace import myNamespace::*
increment
puts [getCount]

# Nested namespaces
namespace eval outer {
    namespace eval inner {
        proc hello {} {
            puts "Hello from inner"
        }
    }
}

outer::inner::hello
```

### Object-Oriented Programming (TclOO)

```tcl
# Define a class
oo::class create Person {
    variable name age
    
    constructor {n a} {
        set name $n
        set age $a
    }
    
    method getName {} {
        return $name
    }
    
    method getAge {} {
        return $age
    }
    
    method birthday {} {
        incr age
    }
    
    method introduce {} {
        puts "Hi, I'm $name and I'm $age years old"
    }
}

# Create object
set alice [Person new "Alice" 30]

# Call methods
$alice introduce
$alice birthday
puts "Age: [$alice getAge]"

# Inheritance
oo::class create Employee {
    superclass Person
    variable jobTitle
    
    constructor {n a job} {
        next $n $a
        set jobTitle $job
    }
    
    method getJobTitle {} {
        return $jobTitle
    }
    
    method introduce {} {
        next
        puts "I work as a $jobTitle"
    }
}

set bob [Employee new "Bob" 35 "Engineer"]
$bob introduce
```

### Packages

```tcl
# Create a package (save as mypackage.tcl)
package provide mypackage 1.0

namespace eval ::mypackage {
    namespace export add multiply
    
    proc add {a b} {
        return [expr {$a + $b}]
    }
    
    proc multiply {a b} {
        return [expr {$a * $b}]
    }
}

# Use the package
package require mypackage
puts [::mypackage::add 5 3]

# Add package directory
lappend auto_path "/path/to/packages"
```

### Event Loop and After

```tcl
# Schedule command after delay (milliseconds)
after 1000 {puts "One second later"}

# Repeat every interval
proc tick {} {
    puts "Tick [clock format [clock seconds]]"
    after 1000 tick
}
tick

# Cancel scheduled command
set id [after 5000 {puts "This might not execute"}]
after cancel $id

# Idle execution (when event loop is idle)
after idle {puts "Executed when idle"}

# Enter event loop (needed for after to work in scripts)
vwait forever
```

---

## Practical Examples

### Example 1: File Processing

```tcl
#!/usr/bin/env tclsh

# Count word frequency in a file
proc countWords {filename} {
    set fp [open $filename r]
    set content [read $fp]
    close $fp
    
    # Convert to lowercase and split into words
    set words [regexp -all -inline {\w+} [string tolower $content]]
    
    # Count frequency
    array set wordCount {}
    foreach word $words {
        if {[info exists wordCount($word)]} {
            incr wordCount($word)
        } else {
            set wordCount($word) 1
        }
    }
    
    # Sort by frequency
    set sorted {}
    foreach {word count} [array get wordCount] {
        lappend sorted [list $count $word]
    }
    set sorted [lsort -integer -decreasing -index 0 $sorted]
    
    # Print top 10
    foreach item [lrange $sorted 0 9] {
        lassign $item count word
        puts "$word: $count"
    }
}

countWords "input.txt"
```

### Example 2: Simple Web Server

```tcl
#!/usr/bin/env tclsh

package require Tcl 8.5

proc handleClient {sock addr port} {
    fconfigure $sock -buffering line
    gets $sock line
    
    puts $sock "HTTP/1.1 200 OK"
    puts $sock "Content-Type: text/html"
    puts $sock ""
    puts $sock "<html><body>"
    puts $sock "<h1>Hello from TCL!</h1>"
    puts $sock "<p>Request: $line</p>"
    puts $sock "</body></html>"
    
    close $sock
}

set server [socket -server handleClient 8080]
puts "Server listening on port 8080"
vwait forever
```

### Example 3: CSV Parser

```tcl
proc parseCSV {filename} {
    set fp [open $filename r]
    set data {}
    
    # Read header
    gets $fp header
    set headers [split $header ","]
    
    # Read rows
    while {[gets $fp line] >= 0} {
        set values [split $line ","]
        set row {}
        foreach h $headers v $values {
            dict set row [string trim $h] [string trim $v]
        }
        lappend data $row
    }
    
    close $fp
    return $data
}

set data [parseCSV "data.csv"]
foreach row $data {
    dict for {key value} $row {
        puts "$key: $value"
    }
    puts "---"
}
```

### Example 4: Configuration File Parser

```tcl
proc loadConfig {filename} {
    set fp [open $filename r]
    array set config {}
    
    while {[gets $fp line] >= 0} {
        # Skip comments and empty lines
        set line [string trim $line]
        if {$line eq "" || [string index $line 0] eq "#"} {
            continue
        }
        
        # Parse key=value
        if {[regexp {^(\w+)\s*=\s*(.+)$} $line match key value]} {
            set config($key) $value
        }
    }
    
    close $fp
    return [array get config]
}

array set config [loadConfig "config.ini"]
puts "Database: $config(database)"
puts "Port: $config(port)"
```

### Example 5: Command-Line Argument Parser

```tcl
proc parseArgs {argv} {
    array set options {}
    set positional {}
    
    for {set i 0} {$i < [llength $argv]} {incr i} {
        set arg [lindex $argv $i]
        
        if {[string match "-*" $arg]} {
            set key [string range $arg 1 end]
            
            if {$i + 1 < [llength $argv]} {
                set nextArg [lindex $argv [expr {$i + 1}]]
                if {![string match "-*" $nextArg]} {
                    set options($key) $nextArg
                    incr i
                } else {
                    set options($key) 1
                }
            } else {
                set options($key) 1
            }
        } else {
            lappend positional $arg
        }
    }
    
    return [list [array get options] $positional]
}

lassign [parseArgs $argv] opts pos
array set options $opts
puts "Options: [array get options]"
puts "Positional: $pos"
```

---

## Best Practices

1. **Always use braces around expressions** in control structures for performance
2. **Use `list` command** to build lists with special characters safely
3. **Quote procedure bodies** with braces to defer substitution
4. **Use `string compare`** instead of `==` for string comparison
5. **Close files** explicitly or use the pattern `read [set fp [open ...]]`[close $fp]
6. **Use `dict` instead of arrays** for new code (TCL 8.5+)
7. **Namespace your code** to avoid naming conflicts
8. **Handle errors** with catch or try
9. **Use meaningful variable names** despite TCL's conciseness
10. **Comment your code**, especially complex regular expressions

---

## Resources

- **Official Documentation**: tcl.tk/man
- **TCL Wiki**: wiki.tcl-lang.org
- **Book**: "Practical Programming in Tcl and Tk" by Brent Welch
- **Community**: comp.lang.tcl newsgroup, Stack Overflow
- **Tk Toolkit**: For GUI development
- **Expect**: For automation (built on TCL)

---

This tutorial covers the fundamentals and many advanced features of TCL. The best way to learn is to practice writing scripts and experimenting with the language. TCL's simplicity and power make it excellent for rapid development, testing, and automation tasks.
