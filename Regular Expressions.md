# Regular Expressions Tutorial

Regular expressions (regex) are powerful patterns used to match and manipulate text. Think of them as a search language for finding specific patterns in strings.

## Basic Concepts

**Literal Characters**
The simplest regex matches exact characters:
- `cat` matches "cat" in "The cat sat"
- `123` matches "123" in "Room 123"

**Metacharacters**
Special characters with meaning:
- `.` matches any single character except newline
- `^` matches the start of a string
- `$` matches the end of a string
- `|` means OR

Examples:
- `c.t` matches "cat", "cot", "cut"
- `^Hello` matches "Hello" only at the start
- `end$` matches "end" only at the end

## Character Classes

**Brackets [ ]**
Match any single character inside the brackets:
- `[aeiou]` matches any vowel
- `[0-9]` matches any digit
- `[a-z]` matches any lowercase letter
- `[^0-9]` matches anything that's NOT a digit (^ inside brackets means NOT)

**Predefined Classes**
- `\d` matches any digit (same as `[0-9]`)
- `\w` matches word characters (letters, digits, underscore)
- `\s` matches whitespace (space, tab, newline)
- `\D`, `\W`, `\S` are the opposites (uppercase = NOT)

## Quantifiers

Control how many times something appears:
- `*` means 0 or more times
- `+` means 1 or more times
- `?` means 0 or 1 time (optional)
- `{n}` means exactly n times
- `{n,}` means n or more times
- `{n,m}` means between n and m times

Examples:
- `colou?r` matches both "color" and "colour"
- `\d{3}` matches exactly 3 digits like "123"
- `\d{2,4}` matches 2 to 4 digits like "99" or "2024"
- `a+` matches "a", "aa", "aaa", etc.

## Grouping and Capturing

**Parentheses ( )**
Group parts of a pattern together:
- `(cat|dog)` matches either "cat" or "dog"
- `(ab)+` matches "ab", "abab", "ababab"
- Groups also "capture" matched text for later use

## Practical Examples

**Email validation (simple)**
```
\w+@\w+\.\w+
```
Matches: username@domain.com

**Phone number (US format)**
```
\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}
```
Matches: (555) 123-4567, 555-123-4567, 555.123.4567

**URL matching**
```
https?://[\w.-]+\.\w{2,}
```
Matches: http://example.com, https://sub.domain.org

**Extract words**
```
\b\w+\b
```
`\b` is a word boundary - matches whole words only

## Common Flags/Modifiers

- `i` - case insensitive matching
- `g` - global (find all matches, not just first)
- `m` - multiline (^ and $ match line breaks)

## Tips for Learning

Start simple and build up gradually. Test your patterns with online tools like regex101.com which shows you what matches in real-time. Remember that regex syntax can vary slightly between programming languages, but the core concepts remain the same.

**Practice Exercise**: Try matching dates in MM/DD/YYYY format:
```
\d{2}/\d{2}/\d{4}
```

Regular expressions take practice, but once you understand the building blocks, you can create patterns to match almost any text structure you need.
