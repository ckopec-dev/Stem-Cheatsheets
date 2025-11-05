# Assembly Cheatsheet

## Install the developer documentation on Linux

`$ sudo apt install manpages-posix-dev`

## View a syscall man page

`$ man 2 write`

## Install the assembler

`$ sudo apt install nasm`

## Hello world example

~~~asm
$ nano hello_world.asm

; this is a commnent
;
; hello_world.asm

global _start

section .text:

_start:
        mov eax, 0x4            ; use the write syscall
        mov ebx, 1              ; stdout
        mov ecx, message        ; the output
        mov edx, message_length ; the output length
        int 0x80                ; run the syscall

        ; gracefully exit
        mov eax, 0x1
        mov ebx, 0
        int 0x80

section .data:
        message: db "Hello, World!", 0xA
        message_length equ $-message


# Compile to object code
$ nasm -f elf32 -o hello_world.o hello_world.asm

# Compile to executable
$ ld -m elf_i386 -o hello_world hello_world.o

# Execute
$ ./hello_world
~~~

## Hands-On Assembly Programming Tutorial

### 1. Introduction

Assembly is a low-level language that directly maps to CPU instructions. This tutorial uses **x86 Assembly** with **NASM** on Linux.

- **Pros:** High control, fast execution, learning CPU internals  
- **Cons:** Verbose, platform-specific  

### 2. Setup

~~~bash
# Update package repo
$ sudo apt update
# Install compiler
$ sudo apt install nasm gcc
~~~

### 3. Hello World

~~~asm
; hello.asm
section .data
msg db "Hello, Assembly!", 0xA
len equ $ - msg

section .text
global _start

_start:
    mov eax, 4      ; sys_write
    mov ebx, 1      ; stdout
    mov ecx, msg
    mov edx, len
    int 0x80

    mov eax, 1      ; sys_exit
    mov ebx, 0
    int 0x80
~~~

~~~bash
# Create object file
$ nasm -f elf32 hello.asm -o hello.o
# Create machine code
$ ld -m elf_i386 hello.o -o hello
# Run code
$ ./hello
~~~

### 4. Arithmetic Operations

Adds two numbers and stores the result in memory.

~~~asm
; arithmetic.asm
section .data
num1 db 5
num2 db 10
result db 0

section .text
global _start

_start:
    mov al, [num1]
    add al, [num2]
    mov [result], al

    mov eax, 1
    mov ebx, 0
    int 0x80
~~~

### 5. Loops

~~~asm
; loop.asm
section .data
count db 5

section .text
global _start

_start:
    mov ecx, [count] ; loop counter

loop_start:
    ; do something
    dec ecx
    jnz loop_start   ; jump if not zero

    mov eax, 1
    mov ebx, 0
    int 0x80
~~~

### 6. Functions / Procedures

call pushes return address, ret returns.

~~~asm
; function.asm
section .text
global _start

_start:
    mov eax, 5
    mov ebx, 3
    call add_numbers
    ; eax now has 8

    mov eax, 1
    mov ebx, 0
    int 0x80

add_numbers:
    add eax, ebx
    ret
~~~

### 7. Stack Operations

Useful for temporary storage and passing function arguments.

~~~asm
push eax   ; push register onto stack
pop ebx    ; pop stack into register
~~~

### 8. Conditional Execution

~~~asm
; compare.asm
section .data
a db 5
b db 10
section .text
global _start

_start:
    mov al, [a]
    cmp al, [b]
    jl less
    jmp end

less:
    ; code if a < b
end:
    mov eax, 1
    mov ebx, 0
    int 0x80
~~~

### 9. Array Processing Example

~~~asm
; array_sum.asm
section .data
arr db 1,2,3,4,5
sum db 0
len equ 5

section .text
global _start

_start:
    xor al, al    ; clear AL for sum
    mov ecx, len
    mov esi, 0    ; index

sum_loop:
    add al, [arr + esi]
    inc esi
    loop sum_loop

    mov [sum], al

    mov eax, 1
    mov ebx, 0
    int 0x80
~~~

### Tips & Best Practices

- Always clear registers before use (xor eax, eax).
- Use comments for clarity.
- Learn system calls (int 0x80 Linux).
- Use GDB to debug: gdb ./hello
- Inspect binaries: objdump -d hello
