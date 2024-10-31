# Yaml Cheatsheet

## Whitespace

- Nesting is created by whitespace indentation, not tabs.

# Comments

\# This is a comment

## Node types

- Mappings (maps/dictionaries): unordered key/value pairs. Keys must be unique. Can be nested.
- Sequences (arrays/lists): ordered series of 0+ nodes. Represented by hyphen and space.
- Scalars (strings, numbers, etc.): 0+ Unicode characters. Strings do not need to be quoted, however it's a good practice.
  - Fold operator (>): allows strings to wrap.
  - Block operator (|): allows strings to wrap, preserving line breaks.

