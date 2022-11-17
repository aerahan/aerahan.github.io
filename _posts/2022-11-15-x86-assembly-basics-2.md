---
title: "x86 Assembly Basics Part 2"
layout: post
date: 2022-11-15
feature_image: images/
tags: [x86, assembly, programming]
---

x86 assembly is a low-level programming language. This is a continuation of assembly basics. The first part can be found [here](https://aerahan.github.io/x86-assembly-basics)!

<!--more-->

&nbsp;
&nbsp;
&nbsp;

<div align="center">.・。.・゜✭・.・✫・゜・。. </div>

<br>
<br>

## Table of Contents

1. [EFLAGS Register] part of conditional jumps
2. [Control Flow & Jump]
3. [Conditional Jumps]
4. [Comparisons]
5. [IF Conditional Statements]
6. [WHILE Conditional Loops]
7. [FOR Conditional Loops]
8. Function Calls



### Function Calls

The ***eip*** pointer points to the next instruction. This means the value saved at the eip register is the return address.

To call a function, the syntax is `call <label>` where label is the function 'name'. `call` does two things: it will **push eip** which will save the return address, and **jmp <label>** jump. 
  
`ret` is basically **pop eip** and obtains the return address. This assumes that the stack pointer, *esp*, is currently pointing at the return address (eip). All stack usage inside of called functions must be cleaned up. 
  

**x86 STDCALL Convention**
  To pass arguments to a called function, arguments should be pushed in reverse order to the stack. To return a value to the caller, the return value should be restored in the **eax** register.
  
  Example: Convert C program to ASM:
  
C code:

```c
E() {
    int a = 10;
    b = F(10,50);
    a += b;
}
```

Assembly code:
```asm
E:
    mov ebx, 10
    push 50
    push 10
    call E
    add esp,8
    
    add ebx,eax
```
  
  
If the value is changed by the called function but you need to keep the original value, save the register.
```asm
E:
    mov ebx, 10
    push ebx ;this saves it
    
    push 50
    push 10
    call E
    add esp,8
    
    pop ebx ;this restores it so ebx is now 10 again
    add ebx,eax
```

The push and pops can also be done by the called function before and after the ebx register (in the example) is used or changed. 

The caller function must save/push the eip register (this is already done in the `call` instruction!). The callee (function) must save/push the ebp register (this would be done with `ret`). 

Since **EPB** always points at the base of the stack, the value saved at **EBP** should be the saved base pointer of the calling function. `EBP + 4` would be the saved **EIP** pushed during the `call` instruction. This tells the processor where to return to. `EBP + 8` would be the first argument, `EBP + 12` would be the second argument, and `EBP + 16` would be the third argument. More can be read in [part 1](https://aerahan.github.io/x86-assembly-basics#stack).






