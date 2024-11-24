# Assembly Cheatsheet

## Install the developer documentation on Linux
`$ sudo apt install manpages-posix-dev`

## View a syscall man page
`$ man 2 write`

## Install the assembler
`$ sudo apt install nasm`

## Hello world example

~~~
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
