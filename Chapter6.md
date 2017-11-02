<h1 align="center">Chapter 6</h1>

## Debugging the interpreter loop

Once the bytecode is generated the next task is to execute the program by the interpreter. Let
us come back to the file `Python/pythonrun.c` on line number 980. This is a call to the function
PyEval_EvalCode which is defined in the file `Python/ceval.c` on line number 693. It internally
calls the function `PyEval_EvalCodeEx`. In this function initially we see that the stack frame is
setup. So let us examine the structure of the frame object, which is defined in the file
`Include/frameobject.h`

```python
typedef struct_frame {

    PyObject_VAR_HEAD

    struct_frame*f_back;    /*previous frame, or NULL*/​
​
    PyCodeObject*f_code;    /* code segment */
​
    PyObject*f_builtins;    /* builtin symbol table (PyDictObject) */
​
    PyObject*f_globals;     /* global symbol table (PyDictObject) */
​
    PyObject*f_locals;      /* local symbol table (any mapping) */
​
    PyObject**f_valuestack;     /* points after the last local */


/* Next free slot in f_valuestack.  Frame creation sets to f_valuestack.
​
Frame evaluation usually NULLs it, but a frame that yields sets it to the current stack top.*/

    PyObject**f_stacktop;

    PyObject*f_trace;   /* Trace function */

/* If an exception is raised in this frame, the next three are used to

* record the exception info (if any) originally in the  thread state.
* See comments before set_exec_info() -- it's not obvious.
* Invariant: if_type is NULL, then so are _value and _traceback.
* Desired invariant:  all three are NULL, or all three are non-NULL.
* That one isn't currently true, but "should be".
*/

    PyObject*f_exc_type,*f_exc_value,*f_exc_traceback;
​
    PyThreadState*f_tstate;

    int f_lasti;    /* Last instruction if called */

/* Call PyFrame_GetLineNumber() instead of reading this field
    directly.As of 2.3 f_lineno is only valid when tracing is
    active (i.e when f_trace is set). At other times we use Pycode_Addr2Line bytecode index. */

    int f_lineno;   /* Current line number*/
    int f_iblock;   /* index in f_blockstack */
    PyTryBlock f_blockstack[CO_MAXBLOCKS];  /* for try and loop blocks */
    PyObject *f_localsplus[1];   /* locals+stack, dynamically sized */
} PyFrameObject;
```
I shall not explain the individual fields as it is quite clear from the comments itself. What is
important is that this frame contains an instruction pointer which is very useful when we
understand how generators work in the coming chapters.  In this function on line number 4136
is the call to the function _PyEval_EvalCodeWithName which internally calls the function
PyEval_EvalFrameEx which calls the eval_frame function on the PyThreadState which is
nothing but the function _PyEval_EvalFrameDefault. This function can also be said the virtual
machine of python. Let us step into this function and insert a breakpoint on line number 1109.
Here we observe that there is an infinite for loop which iterates through every opcode of the
program and executes it. On line number 1220 we observe how the next opcode is fetched. Let
us examine the value of our opcode.

[image]

It is exactly as we expected from our program. Let us insert a breakpoint on line number 1199
which is a giant switch case for all the opcodes.  Continue debugging and we observe that it
breaks at line number 1245 which is the implementation for LOAD_CONST. In this way we can
debug our entire program opcode by opcode.
In this way we have understood how the opcodes are executed by the python virtual machine. I
would suggest you to debug the interpreter loop for the following python example programs:

1. ```l=range(0,10)
    m= map(lambda x:x * 2, l)
    ```


2. ```python
    def logger(func):
    def logged(name):
        print "logging starts"
        func(name)
        print "logging ends"
    return logged

    @logger

    def printName(name):
        print name
        printName("Guido Van Rossum")
    ```
Adios :). You are now familiar with the python interpreter loop.
