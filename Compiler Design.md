# Compiler Design Cheatsheet

A compiler converts source code written in one language into another language, and reports any errors encountered in the process. 

There are two main parts of compilation:
- Analysis: splits source into pieces using a tree structure
- Synthesis: creates destination source from analysis pieces

Major compilation components:
- Preprocessor: turns skeletal source into source
- Compiler: turns source into target assembly
- Assembler: turns target assembly into relocatable machine code
- Loader: relocatable machine code into absolute machine code

Lexical analysis:
- The source is read left-to-right and grouped into sequences of characters called tokens which have a collective meaning.

Syntax analysis:
- Tokens are grouped into nested collections with collective meaning, typically represented by a parse tree.
