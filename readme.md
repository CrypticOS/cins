# Cryptic Instruction Specification and Design
*By Daniel C*  
*December 2020*

## Abstract
Cryptic Instruction (CINS) is an esoteric programming language based on  
BrainF\*ck and designed to mimic modern assembly and computer architecture. 
It is designed to be a “better” and more practical BrainF\*ck, while still  
maintaining the minimalist design. This document will describe how CINS works,  
why it works, and why I created it, as well as the function of each command in detail.  

## Introduction
CINS has sixteen instructions. Six of the instructions in CINS are based on BrainF\*ck, for familiarity.  
It is designed to be as simple as BrainF\*ck, but be even more powerful. Although they are representable with digits 1-16,  
sixteen ASCII characters were chosen to make programs easier to visualize.

## Memory Instructions
The most important concept in CINS is the fact that there are two memory pointers, the “top”, and the “bottom”.  
The bottom is the main program memory. This is where variables and program data are stored. The top acts similar to  
your CPU registers, where data can temporarily be stored. Of course, 4 instructions will be needed to  
increment/decrement these pointers. Or in other words, pop/push them.

```
d    Increment top pointer
a    Decrement top pointer
>    Increment bottom pointer
<    Decrement bottom pointer
```
*A and D are taken from the common usage in gaming, and <> is taken from BrainF\*ck.  

The next two instructions are crucial.  
```
^    Copy current value from bottom pointer to top
v    Copy current value from top pointer to bottom
```

These allow cells to be copied easily, and opens up a lot of possibilities for optimizations and features in an assembler.  

Increment/Decrement Instructions
```
+    Add 1 to current bottom cell
-    Subtract 1 from current bottom cell
*    Add 5 to current bottom cell
%    Add 50 to current bottom cell
```

The ‘\*’ and ‘%’ instructions seem to be insignificant and maybe even necessary, yet they are essential for generating  
sane code output, as CINS does not have a “while not zero” loop for quick multiplication.  
Consider these examples (counting to 16):

```
BrainF\*ck        CINS (just plus)      CINS
++++[->++++<]    ++++++++++++++++      ***+
```

## Logic Instructions
One of the most notable differences between CINS and BrainF\*ck is with logic and jumping.  
BrainF\*ck logic only has basic WHILE TRUE loops, and cannot dynamically jump anywhere in the code.  

`?    “IF EQUAL THEN GOTO”`  
This instruction compares 3 cells from the top part of the memory. It compares the first and the second cell and checks  
if they are equal. If equal, it jumps to the label of the third cell. The 2 cells are to the right of the pointer.

`$    “JUMP”`
This jumps to the label defined in the current top cell.  

`|    “DEFINE LABEL”`
Instead of jumping to a specific place in memory or the beginning of a while loop, CINS has labels that are  
defined throughout the code. The labels are referenced to by their occurrence within the program code. This defines a label.  
Jumping to it will start to execute the instructions directly beside it.

## Input/Output
IO is the same as BrainF\*ck.
.    Print the current bottom cell.
,    Input a character into the current bottom cell

## Usage
This will show several snippets that would be used often in an assembler for CINS.

*Copying cells*  
1. Move to the source cell
2. Copy value top top (^)
3. Move to destination cell
4. Move top value down (v)

*Label calling*  
1. In an unused cell, write the label ID
2. Goto ($)
3. Make a new label. Write it into the temp cell, and move it up (^)
4. Put the label (|)

*Comparisons*  
1. Write first value, move up (^), right (d)
2. Write second value, move up (^), right (d)
3. Write second value, move up (^), left 2 times (aa)
4. Compare (?)
