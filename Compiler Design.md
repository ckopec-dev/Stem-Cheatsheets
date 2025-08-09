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
