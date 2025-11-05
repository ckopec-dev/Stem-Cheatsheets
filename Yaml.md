# Yaml Cheatsheet

## Whitespace

- Nesting is created by whitespace indentation, not tabs.

## Comments

\# This is a comment

## Node types

- Mappings (maps/dictionaries): unordered key/value pairs. Keys must be unique. Can be nested.
- Sequences (arrays/lists): ordered series of 0+ nodes. Represented by hyphen and space.
- Scalars (strings, numbers, etc.): 0+ Unicode characters. Strings do not need to be quoted, however it's a good practice.
  
## Operators

- Fold(>): allows strings to wrap.
- Block(|): allows strings to wrap, preserving line breaks.
- Type(!!): explictly expresses type. E.g. company: !!str Microsoft
- Anchor(&): marks a snippet as reusable
- Alias(*): reuses an anchored snippet
- Override(<<:): allows an anchor snippet to be overridden when reused

## Documents

- A section of yaml is called a document. A yaml file may contain multiple documents. A document begins with '---' and ends with '...'.
