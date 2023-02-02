---
title: "x86 Assembly Basics"
layout: post
date: 2022-11-14
feature_image: images/x86_assembly.png
tags: [x86, assembly, programming]
---

x86 assembly is a low-level programming language that is converted into machine executable code by a utility program such as MASM (Windows) or NASM (Linux). Low-level languages use assemblers to directly translate and are machine-oriented, while high-level languages such as Python use a compiler or interpreter and are more user-friendly.
Low-level languages are faster than high-level but slower than machine languages!

<!--more-->

This blog post covers Intel syntax (AT&T syntax also exists).
  
&nbsp;
&nbsp;
&nbsp;

<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
```c
  printf("Hello world")
```

<br>

## Table of Contents

1. [Advantages & Disadvantages of Assembly](#advantages-of-assembly-language)
2. [Registers](#registers)
3. [Directives](#directives)
4. [Data Types](#data-types)
5. [Operations](#operations)
6. [Referencing & Dereferencing](#referencing-and-dereferencing)
7. [Importing Libraries](#importing-libraries)
8. [Stack](#stack)
9. [Sources](#sources)

<br>
<br>

<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
<br>

### Advantages of Assembly Language

- Less memory
- Shorter execution time & increased efficiency
- Can write code to access registers
- Helps understand processers and memory
- Better control on hardware

<br>

### Disadvantages of Assembly Language

- Lack of portability due to machine-architecture dependence
- Complex, and difficult syntax
- Effort.

<br>
<br>

<div align="center">.・。.・゜✭・.・✫・゜・。. </div>
  
<br>
<br>

### Registers

PC hardware has main components of a processor, memory, and registers.
Registers are essential for assembly and are processor components that hold data and addresses. They are like variables for the processor. Most processors now have eight 32-bit general purpose registers. They have names but are currently used for a variety of purposes. Case also does not matter, so EDX and edx mean the same.

[![Chart of registers](/images/x86-registers.png)](https://www.cs.virginia.edu/~evans/cs216/guides/x86.html)

*Source: From materials developed for University of Virginia cs216 by David Evans.*

- **EAX** - *Accumulator*
- **ECX** - *Counter*
- **EDX** - *Data*
- **EBX** - *Base*

*eax*, *ecx*, *edx*, and *ebx* are mainly used as temporary variables for the processor to execute machine instructions. For these four, you can use subsections and split it into a 16-bit register. For example, the least two significant bytes of EAX can become a 16-bit *AX*, with the same going for *ECX* and *CX*. From there, the least significant byte of *AX* can become a single 8-bit register *AL* and the most significant byte of *AX* can become *AH*. However, these names all point to the same physical register, so a change in one will affect all others. For example, placing a number into *DX* will also affect *EDX*, *DL*, and *DH*. 

- **ESP** - *Stack Pointer*
- **EBP** - *Base Pointer*
- **ESI** - *Source Index*
- **EDI** - *Destination Index*

*esp*, *ebp*, *esi*, and *edi* are general purpose registers but are also called pointers and indexes. esp and ebp hold 32-bit addresses which point to a memory location. 

<br>
<br>

<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
<br>

### Directives

Directives are instructions to the assembler. Some uses are to declare or reserve memory variables, declare code, data areas, etc. They start with `.`.
`.model`, `.data`, and `.code` are all directives. `INVOKE` and `PROC` are also directives.


**Pointer directives** are size specifying directives. DWORD variables are technically pointers - they point to the first byte of the character array (that would be the value stored at the lowest memory, see [Endianness](#data types) for more).

```asm
.data
    myWord DWORD 11223344h
.code
    mov myWord, 22h
    ;that line is the same as 'mov [myWord], 22h'
    mov eax, myWord ;this now holds 00000022h
```

eax holds 00000022h because the line `mov myWord, 44h` moves 4 bytes to the myWord memory location. As noted in the comment, this is the same as `mov [myWord], 22h`, and `mov DWORD PTR [myWord], 22h` (where *DWORD PTR* is a size directive telling that 4 bytes should be moved to memory starting at myWord, which due to little endian would be the 44.)

Automatically, since DWORDs are 4 bytes, 4 bytes would be moved. To move less bytes, a smaller size of data must be specified. This can be done with *PTR*, which will force it to be a pointer of specified size. These include `DWORD PTR` with 4 bytes, `WORD PTR` with 2 bytes, and `BYTE PTR` with 1 byte. See more on [data types & size](#data-types) in the next section below.

Therefore, moving only 1 byte (33h) to the previous example could be done with *BYTE PTR* like so: 

```asm
.data
    myWord DWORD 11223344h
    
.code
    mov BYTE PTR [myWord], 33h ;this would make it 11223333h
```

Similarly, moving 1 byte (22h) to another location instead of starting at myWord could be done like so (see [stack](#stack) for more on locating variables):

```asm
.data
    myWord DWORD 11223344h
    
.code
    mov BYTE PTR [myWord+3], 22h ;this would make it 22223333h
```

The variable locations can be seen below: 
![variable locations](/images/pointer_directives.png)]

<br>
<br> 
    
<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
<br>

### Data Types

Comments start with a semi-colon `;`. 

- **BYTE** db - 1 byte
- **WORD** dw - 2 bytes
- **DWORD** dd - 4 bytes

`BYTE` is typically used for characters while `DWORD` is typically used for integers. `db`, `dw`, and `dd` are shorthand for the data types. 

**Syntax:** <name><type><value>

Example variables and arrays:
    
```asm
myInt DWORD 10
mySecondInt dd 20 ;this is also a DWORD
message BYTE "ABC" ;an array of characters
```
    
Arrays in assembly can't be indexed like in other programming languages such as Python. 
Hexadecimal characters can also be used to make escaped characters such as a newline. `0Ah` in particular is a newline while `0` is a null-terminator. 
    
```asm
message db "ABC",0Ah,0 ;this puts a new line after 'ABC'
```
    
Variables can be declared in the `.data` directive section. 


Endianness is the order that bytes in computer memory are read in.
Big-endian is where the big end (most significant value) is stored first at the lowest storage address, while little-endian is where the little end (least significant value) is stored first at the lowest storage address. 
While network data uses big-endian, most systems assembly is compiled on use little-endian. For a network data example, the 32-bit IP address 127.0.0.1 (7F 00 00 01 in hex) would be sent over the network as 0x7F000001 but stored in memory as 0x0100007F.

![Endianness chart](/images/endianness.PNG)

<br>
<br>
    
<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
<br>  

### Operations
  
The basic syntax for instructions is typically `operation <destination>,<source>`. The destination and source values are usually a value, memory address, or a register.
    
- **mov** *<destination>,<value>* - moves a value from source to destination
- **sub** *<dest>,<src>* - subtracts source from destination and stores in destination
- **add** *<dest>,<src>* - adds values and stores in destination
- **mul** *<src>* - multiplies `eax` by source
- **div** *<src>* - divides `eax` by source.

To compute `((10 + 20) * 2) / 5`:

```asm
.data
  value DWORD 10
.code
  main PROC c
    mov eax,10 ;moves 10 into eax register
    add eax,20 ;adds 20 to eax, now holds 30
    mov ebx,2 ;moves 2 into ebx register
    mul ebx ;multiplies eax by ebx and stores in eax, now holds 60
    mov ecx,5 ;moves 5 into ecx register
    div ecx ;divides eax by ecx, eax now holds 12
  INVOKE ExitProcess,0
  main endp
end
```

The **DUP** operator can be used to allocate space for an array or string. 

Syntax: `count DUP(argument)`

<br>

Example:

```c
myVar BYTE 20 DUP(?) 
myVar2 BYTE 5 DUP(0)
myVar3 BYTE 3 DUP("ABCD")
```

myVar would be 20 uninitialized bytes.

myVar2 would be 5 bytes that all equal 0.

myVar 3 would be 12 bytes, as "ABCDABCDABCD".

<br>
<br>  
    
<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
<br>
    
### Referencing and Dereferencing
  
A pointer is a memory location/variable that points to another memory location. 
    
Dereferencing is used to acess or change data in a memory location that is pointed to by a pointer. 
    
Referencing is where the address of an existing variable is used and a pointer variable is set to point at that address location. 

To dereference in assembly, use square brackets to access the value at an address, such as `[eax]` to access the value held in the EAX register (if held). In the C programming language, this is equivalent to using `*`. 
To reference in assembly, use `offset <variable>` to get the address of the variable. In the C programming language, this is equivalent to using `&`.

```asm
varNum DWORD 20 ;int varNum = 20
mov ebx,offset var ;int *ebx = &varNum 
add varNum,10 ;varNum = varNum + 10

HERE
```
  
<br>
<br>
    
<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
<br>  
    
### Importing Libraries
  
Just like other programming languages, assembly can also import libraries as a `.lib` file. The imports are dynamic and must be compiled. 
The `includelib` directive is used to import a library (see [directives](#directives) for more information). 

Typically, for a standard assembly program with masm in Visual Studio, two basic libraries are needed. `libcmt.lib`, a Microsoft C runtime library, which is required by the other library - `legacy_stdio_definitions.lib`, which includes stdio.h functions such as printf.

Importing .lib files also uses `extern` or `PROTO`. Extern tells that a function will be in the library, and `NEAR` shows that the function is in the same segment of memory. In modern systems, functions are in the same memory segment, so it is typically seen. For example, `extern printf:NEAR`. To call the function, it can be invoked with `call printf`. The arguments should be passed to the function through placing them on the stack (see [Stack](#stack) for more). 

`PROTO`, on the other hand, will set up a function. It is called with `INVOKE`. For example, `ExitProcess PROTO,dwExitCode:DWORD` and `INVOKE ExitProcess,0`.

<br>
<br>

<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
<br>
    
### Stack
  
The stack grows from a high memory address to lower memory addresses.

*Return address* refers to the address of the next instruction that should execute next after the current function. **EBP** is a stack related register that points to the base of the stack frame and **ESP** points to the top of the stack. ESP moves as data is pushed or popped from the stack since the top is what changes. Because EBP, on the other hand, is fixed, arguments and local variables can be accessed through EBP (such as EBP + 4, etc). Pushing and popping will subtract and add 4 to ESP, respectively (adding and removing data from bytes). 

In the stack frame of a function, arguments are pushed in reverse order. 

Push and pop are *stack operations*. 

The syntax is `push <value>` and `pop <location>`. This places the <value> on the stack and subtracts 4 from ESP, or movesv 4 bytes from the top of the stack into <location> and adds 4 to ESP.

`push eax` would push the value that eax holds onto the stack. `pop eax` would move 4 bytes from the top of the stack into eax.

Example: 

Using *printf* using stack operations and imported legacy_stdio_definitions.lib
Arguments should be pushed onto the stack in reverse order. 

Translating the C equivalent of `printf("Hello World! Today is %s %d.\n", month, year)`

This line of code has 3 arguments that must be pushed: the whole string itself,the string month, and the int year. For the first two, sinc ethey are strings, `offset` must be used to retrieve their memory address (For more on offset, refer to [Referencing & Dereferencing](#referencing-dereferencing) above).

At the end, the arguments should be removed from the stack. This can be done by adding to ESP (which would be 4 times the number of arguments present, in this case, 3).

```c
.586
.model flat,stdcall
.stack 4096
includelib libcmt.lib
includelib legacy_stdio_definitions.lib
ExitProcess PROTO,dwExitCode:DWORD
extern printf:NEAR

.data
    year DWORD 2022
    month BYTE "November",0
    wholeLine BYTE "Hello World! Today is %s %d",0Ah,0

.code
    main PROC c
        push year
        push offset month
        push offset wholeLine
        
        call printf
        add esp,12

        INVOKE ExitProcess,0
    main endp
end
```


Example 2:

Using scanf

The assembly equivalent of C `scanf("My number is: %d", &myNum)`:

```asm
.586
.model flat,stdcall
.stack 4096
includelib libcmt.lib
includelib legacy_stdio_definitions.lib
ExitProcess PROTO,dwExitCode:DWORD
extern scanf:NEAR

.data
    myNum DWORD ?
    format BYTE "My number is: %d",0

.code
    main PROC c
        push myNum
        push offset format
        
        call scanf
        add esp,8

        INVOKE ExitProcess,0
    main endp
end
```

<br>
<br>
    
<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
<br>
<br>
    
#### Sources & References

Introduction to x86 Assembly for CSEC-201 by [Gahyun Park](https://www.rit.edu/directory/gxpics-gahyun-park)

Lectures by [James Brigden](https://www.rit.edu/directory/jgbics-james-brigden).

Adapted from [materials](https://www.cs.virginia.edu/~evans/cs216/guides/x86.html) developed for University of Virginia cs216 by David Evans. This guide was revised for cs216 by David Evans, based on materials originally created by Adam Ferrari many years ago, and since updated by Alan Batson, Mike Lack, and Anita Jones.
Erickson, Jon. Hacking : The Art of Exploitation, No Starch Press, Incorporated, 2008.


https://www.tutorialspoint.com/assembly_programming/assembly_introduction.htm
https://research.ncl.ac.uk/game/mastersdegree/programmingforgames/pointers/pointers.pdf
