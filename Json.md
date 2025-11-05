
# Json Cheatsheet

A compact, printable reference for JSON syntax, types, examples, and common operations.

---

## Basics

- **JSON** = JavaScript Object Notation — lightweight data interchange format.
- Two primary structures:
  - **Object**: `{ "key": value }` — unordered collection of key/value pairs.
  - **Array**: `[ value1, value2 ]` — ordered list.
- Strings **must** use double quotes (`"`). No trailing commas. No comments.

---

## Data Types

| Type     | Example                     | Notes |
|---------:|----------------------------:|-------|
| String   | `"Hello World"`             | Double quotes required |
| Number   | `42`, `3.14`                | Integers or floats (no quotes) |
| Boolean  | `true`, `false`             | Lowercase |
| Null     | `null`                      | Represents an empty value |
| Object   | `{ "name": "Alice" }`       | Keys are strings |
| Array    | `[1, 2, 3]`                 | Ordered list, can contain mixed types |

---

## Syntax Rules

- Key/value pair: `"key": value`
- Strings and keys must use double quotes.
- No trailing commas in objects or arrays.
- Values can be nested objects and arrays.
- Whitespace (spaces, tabs, newlines) is ignored — use indentation for readability.

~~~json
{
  "person": {
    "name": "Alice",
    "age": 30,
    "hobbies": ["reading", "cycling"]
  }
}
~~~

## Examples

### Simple object

~~~json
{
  "name": "John",
  "age": 25,
  "isStudent": true
}
~~~

### Array of objects

~~~json
[
  { "name": "Alice", "age": 30 },
  { "name": "Bob", "age": 25 }
]
~~~

### Nested object

~~~json
{
  "company": "TechCorp",
  "employees": [
    { "name": "Alice", "role": "Developer" },
    { "name": "Bob", "role": "Designer" }
  ]
}
~~~

### Mixed types

~~~json
{
  "string": "Hello",
  "number": 42,
  "boolean": true,
  "null": null,
  "array": [1, "two", false],
  "object": {"key": "value"}
}
~~~

## Common Operations

### Javascript

~~~javascript
// Parse JSON string -> object
const obj = JSON.parse('{"name":"Alice"}');

// Object -> JSON string
const jsonStr = JSON.stringify({ name: "Alice" });

// Pretty-print (2-space indent)
const pretty = JSON.stringify(obj, null, 2);

// Access properties
console.log(obj.name);         // Alice
console.log(obj.hobbies[0]);   // reading

~~~

### Python

~~~python
import json

# String -> dict
obj = json.loads('{"name": "Alice"}')

# Dict -> JSON string
json_str = json.dumps({"name": "Alice"})

# Pretty-print (2-space indent)
pretty = json.dumps(obj, indent=2)

# Access properties
print(obj["name"])            # Alice
print(obj["hobbies"][0])      # reading
~~~
