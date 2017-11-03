# Chapter 10

# Python optimizations

In this chapter let us study about the optimizations performed by python and what is the
programmers role.

Create a sample file test.py with the following contents:

```python
if not 100:
           print "1000"
else:
           print "100"

``` 

In the debug configurations add the following arguments:

Open the file Python/peephole.c and open the line number 425 which is the function
PyCode_Optimize which is the function that performs the peephole optimizations. This chapter
is not an alternative to the optimizations that can be performed at the code level by the
programmer. This chapter is just intended to show the kind of optimizations the compiler
performs on the program.

Insert a breakpoint on line number 494, which is a giant for loop which iterates through the
entire code object instruction by instruction. On line number 364 we see a giant switch case to
specify the kind of optimizations that are performed for any opcode. Let us step by debugging
the individual lines.

The first switch case is on the bytecode UNARY_NOT. Let us examine the code for the same.

```c
if (codestr[i+1] != POP_JUMP_IF_FALSE
                   || !ISBASICBLOCK(blocks,i,4))
                   continue;
          j = GETARG(codestr, i+1);
          codestr[i] = POP_JUMP_IF_TRUE;
          SETARG(codestr, i, j);
          codestr[i+3] = NOP;
          goto reoptimize_current;
```
It replaces UNARY_NOT POP_JUMP_IF_FALSE by the bytecode POP_JUMP_IF_TRUE,
which seems logical. From our code examine how the optimization is performed.


Go through the individual cases to understand what kind of optimizations are performed for
every individual instruction bytecodes. Adios:)
