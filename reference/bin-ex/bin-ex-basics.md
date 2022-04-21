# Binary Exploitation - Basics

## Basics

Assuming Intel syntax

### Registers and their purposes

`eip` --> _instruction pointer_. Currently executing instructions

#### General purpose (usually CPU vars)

`eax` --> _accumulator_
`ecx` --> _counter_
`edx` --> _data_
`ebx` --> _base_

#### General purpose, usually pointers to addresses

`esp` --> _stack pointer_. Stores an address
`ebp` --> _base pointer_. Stores an address
`esi` --> _source index_. 
`edi` --> _destination index_. 

`sp` and `bp` define a stack frame. The memory between them are the local variables

### Instruction format

`operation <destination>, <source>`

### Instructions

`lea` --> load effective address (into a register)

### system calls

Functions provided by the kernel to programs to do things on the OS

Identifying syscalls gives you a good idea about what a binary is doing, e.g. a string compare or opening a TCP socket.

Syscalls are specific to the OS. Also architecture.

Syscalls are associated with numbers. The numbers will be different on different architectures even for the same OS.

[Linux 32 bit syscalls by number](asm.sourceforge.net/syscall.html)