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






#### Sources & References
Adapted from materials developed for University of Virginia cs216 by David Evans. This guide was revised for cs216 by David Evans, based on materials originally created by Adam Ferrari many years ago, and since updated by Alan Batson, Mike Lack, and Anita Jones.
https://www.cs.virginia.edu/~evans/cs216/guides/x86.html

https://www.tutorialspoint.com/assembly_programming/assembly_introduction.htm


Erickson, Jon. Hacking : The Art of Exploitation, No Starch Press, Incorporated, 2008. ProQuest Ebook Central, http://ebookcentral.proquest.com/lib/rit/detail.action?docID=1137538.
Created from rit on 2022-11-02 14:07:18.
