---
title: "x86 Assembly Basics"
layout: post
date: 2022-11-01
feature_image: images/x86_assembly.png
tags: [x86, assembly, programming]
---

x86 assembly is a low-level programming language that is converted into machine executable code by a utility program such as MASM (Windows) or NASM (Linux). Low-level languages use assemblers to directly translate and are machine-oriented, while high-level languages such as Python use a compiler or interpreter and are more user-friendly.
Low-level languages are faster than high-level but slower than machine languages!

<!--more-->

This blog post covers Intel syntax (AT&T syntax also exists).


<div align="center">.・。.・゜✭・.・✫・゜・。. </div>


## Table of Contents
1. [Advantages & Disadvantages of Assembly](#advantages-of-assembly-language)
2. [Registers](#registers)
3. [Directives](#directives)
4. [Data Types](#data-types)
5. [Operations](#operations)
6. [Referencing & Dereferencing(#referencing-dereferencing)
[Sources](#sources)


<div align="center">.・。.・゜✭・.・✫・゜・。. </div>


### Advantages of Assembly Language
- Less memory
- Shorter execution time & increased efficiency
- Can write code to access registers
- Helps understand processers and memory
- Better control on hardware

### Disadvantages of Assembly Language
- Lack of portability due to machine-architecture dependence
- Complex, and difficult syntax
- Effort.


<div align="center">.・。.・゜✭・.・✫・゜・。. </div>


### Registers
PC hardware has main components of a processor, memory, and registers.
Registers are essential for assembly and are processor components that hold data and addresses. They are like variables for the processor. Most processors now have eight 32-bit general purpose registers. They have names but are currently used for a variety of purposes. Case also does not matter, so EDX and edx mean the same.

[![Chart of registers](/images/x86-registers.png)](https://www.cs.virginia.edu/~evans/cs216/guides/x86.html)

Source: From materials developed for University of Virginia cs216 by David Evans.

- **EAX** - *Accumulator*
- **ECX** - *Counter*
- **EDX** - *Data*
- **EBX** - *Base*

eax, ecx, edx, and ebx are mainly used as temporary variables for the processor to execute machine instructions. For these four, you can use subsections and split it into a 16-bit register. For example, the least two significant bytes of EAX can become a 16-bit AX, with the same going for ECX and CX. From there, the least significant byte of AX can become a single 8-bit register AL and the most significant byte of AX can become AH. However, these names all point to the same physical register, so a change in one will affect all others. For example, placing a number into DX will also affect EDX, DL, and DH. 

- **ESP** - *Stack Pointer*
- **EBP** - *Base Pointer*
- **ESI** - *Source Index*
- **EDI** - *Destination Index*

esp, ebp, esi, and edi are general purpose registers but are also called pointers and indexes. esp and ebp hold 32-bit addresses which point to a memory location. 



<div align="center">.・。.・゜✭・.・✫・゜・。. </div>


### Directives
Directives are instructions to the assembler. Some uses are to declare or reserve memory variables, declare code, data areas, etc. They start with `.`.
`.model`, `.data`, and `.code` are all directives. `INVOKE` and `PROC` are also directives.


<div align="center">.・。.・゜✭・.・✫・゜・。. </div>


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
While network data uses big-endian, most systems assembly is compiled on use little-endian. 


<div align="center">.・。.・゜✭・.・✫・゜・。. </div>


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

<div align="center">.・。.・゜✭・.・✫・゜・。. </div>


### Referencing & Dereferencing
A pointer is a memory location/variable that points to another memory location. 
Dereferencing is used to acess or change data in a memory location that is pointed to by a pointer. 
Referencing is where the address of an existing variable is used and a pointer variable is set to point at that address location. 

To dereference in assembly, use square brackets to access the value at an address, such as `[eax]` to acess the memory address held in the EAX register (if held). In the C programming language, this is equivalent to using `*`. 
To reference in assembly, use `offset <variable>` to get the address of the variable. In the C programming language, this is equivalent to using `&`.

```asm
varNum DWORD 20 ;int varNum = 20
mov ebx,offset var ;int *ebx = &varNum 
add varNum,10 ;varNum = varNum + 10

UM
```



<div align="center">.・。.・゜✭・.・✫・゜・。. </div>


#### Sources & References
Introduction to x86 Assembly for CSEC-201 by Gahyun Park,
Lectures by James Brigden.

Adapted from materials developed for University of Virginia cs216 by David Evans. This guide was revised for cs216 by David Evans, based on materials originally created by Adam Ferrari many years ago, and since updated by Alan Batson, Mike Lack, and Anita Jones.
https://www.cs.virginia.edu/~evans/cs216/guides/x86.html

https://www.tutorialspoint.com/assembly_programming/assembly_introduction.htm

https://research.ncl.ac.uk/game/mastersdegree/programmingforgames/pointers/pointers.pdf



Erickson, Jon. Hacking : The Art of Exploitation, No Starch Press, Incorporated, 2008. ProQuest Ebook Central, http://ebookcentral.proquest.com/lib/rit/detail.action?docID=1137538.
Created from rit on 2022-11-02 14:07:18.





%x for hexadecimal %i for integers
