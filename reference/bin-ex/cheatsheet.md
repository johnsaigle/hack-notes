# Cheatsheet

(x86) --> little-endian --> least significant byte is stored first --> "reverse order"

## gdb

`gdb -q a.out`

`set dis intel` --> get Intel syntax

`i r <reg>`
`info register <reg>` --> inspect contents of register `<reg>`

`disassemble <func>` --> show disassembly for a function

`break <func>` --> set breakpoint at function

`list` --> show source code (e.g. from `gcc -c <source.c>`)

`x` --> short for examine memory

`x 2/x $eip` --> show two words (4 byte chunks) of the value stored in pointer `eip`, formatted as hex numbers

`x 2/i $eip` --> show two instructions at the address stored in the `$eip` pointer

`nexti` --> execute next instruction (from breakpoint)

## gcc

Include extra debugging info in compiled binary (e.g. give gdb source code)

`gcc -c source.c` 

## radare2

`aaa` analyze everything

`pdf` disassembly function

`pdg` print ghidra decompilation (requires ghidra plugin, `r2pm install r2ghidra`)

`s` seek to an address


`ax` cross-references

`?` to get any info about any command, e.g `ps?`

`iR` inspect resources

`iS` inspect sections

`VV` graph

`R` randomize colours in graph mode

`eco` list colour schemes. `eco <scheme>` to load one

### Radare2 Debugging

_function keys work in a similar way to ollydbg or x64dbg_

`afvd` show values of variables

`db main` breakpoint at main

`ood` re-open the file in read-write mode for modification/debugging

`dc` continue to breakpoint

`dr $reg` show register value

`dr 1` shows all flags

### Ollydbg and panel-mode

`V!` panel view, similar to ollydbg. 

Customization: https://www.youtube.com/watch?v=JuAM-zLC4D4

#### Patch binary

e.g. replacing `jne` (opcode `75`) with `jz` (opcode `74`)

While in debugging mode: 

- `s` to address containing `75` opcode
- `wx 74`. writes (opcode) 74. requires `ood` beforehand to be able to write to the file
- This will not persist after closing r2

Do `r2 -w` and repeat the procedure to make a persistent change

