# Appendix A  


# Python Standerd Libery


Once you have python installed in your system you need a set of standard libraries to help build
applications. On python these are present in the Lib/ folder of your python installation. These
are written in python and act as the building base of applications.



# Appendix B

# Introduction to PyPy

## Setting up PyPy

## Download PyCharm :

### [https://www.jetbrains.com/pycharm/download/#section=linux](https://www.jetbrains.com/pycharm/download/#section=linux)
 
## Clone pypy:

### $git clone [https://bitbucket.org/pypy/pypy](https://bitbucket.org/pypy/pypy)

## Import pypy into PyCharm:

## What is PyPy ?

#### PyPy is a [Python](https://en.wikipedia.org/wiki/Python_(programming_language)) interpreter and [just-in-time compiler](https://en.wikipedia.org/wiki/Just-in-time_compilation). PyPy focuses on speed, efficiency and compatibility with the original [CPython](https://en.wikipedia.org/wiki/CPython) interpreter.[[1]](https://en.wikipedia.org/wiki/PyPy#cite_note-mission-statement-1)
#### PyPy started out being a Python interpreter written in the Python language itself. Current PyPy versions are translated from [RPython](https://en.wikipedia.org/wiki/PyPy#RPython) to [C code](https://en.wikipedia.org/wiki/C_(programming_language) and compiled. The PyPy JIT compiler is capable of turning Python code into [machine code](https://en.wikipedia.org/wiki/Machine_code)at run time.
### Code Object:
It is defined in the file pypy/interpreter/pycode.py. Let us examine the fields of the class.
```
self.co_argcount
self.co_nlocals
self.co_stacksize
self.co_flags
self.co_code
self.co_consts_w
self.co_names_w]
self.co_varnames
self.co_freevars
self.co_cellvars
self.co_filename
self.co_name
self.co_firstlineno
self.co_lnotab

# store the first globals object that the code object is run in in
# here. if a frame is run in that globals object, it does not need to
# store it at all

self.w_globals
self.hidden_applevel
self.magic
```
It is very visibly similar to the PyCodeObject of Cpython.
### Function Object:
It is defined in the file pypy/interpreter/function.py. Let us examine the fields of the class.
```
self.name
self.w_doc
self.code
self.w_func_globals
self.closure
self.defs_w
self.w_func_dict
self.w_module
```
It is very similar to the function object defined as a C structure in CPython.
### Frame Object:
It is defined in the file pypy/interpreter/pyframe.py

###### frame_finished_execution
###### last_instr = -1
###### last_exception = None
###### f_backref =  jit.vref_None
###### escaped = False # see mark_as_escaped()
###### debugdata = None
###### pycode = None # code object executed by that frame
###### pycode = None # code object executed by that frame
###### locals_cells_stack_w = None # the list of all locals, cells and the valuestack
###### valuestackdepth = 0 # number of items on valuestack
###### lastblock = None
#### Generator Object:
It is defined in the file pypy/interpreter/generator.py.
```
self.space
self.frame
self.pycode
Self.running
```
It is strikingly similar to the generator object of Cpython.
### Integer Object:
It is defined in the file pypy/objspace/std/intobject.py on line number 311.
### Complex Object:
It is defined in the file pypy/objspace/std/complexobject.py on line number 186.
### Float Object:
It is defined in the file pypy/objspace/std/floatobject.py on line number 139.
### Dictionary Object:
It is defined in the file pypy/objspace/std/dictmultiobject.py on line number 360.
### List Object:
It is defined in the file pypy/objspace/std/listobject.py on line number 168.
### Interpreter Loop:
The interpreter loop of pypy is located in the file pypy/interpreter/pyopcode.py on line number 145 which is the function dispatch_bytecode. It is very similar to the PyEvalFrameEx function which is the interpreter loop of Cpython.


#### Parsing ([US/ˈpɑːrsɪŋ/](https://en.wikipedia.org/wiki/Help:IPA_for_English) ; [UK](https://en.wikipedia.org/wiki/British_English)[/ˈpɑːrzɪŋ/](https://en.wikipedia.org/wiki/Help:IPA_for_English) or syntactic analysis is the process of analysing a [string](https://en.wikipedia.org/wiki/String_(computer_science)) of  [symbols](https://en.wikipedia.org/wiki/Symbol_(programming)), either in [natural language](https://en.wikipedia.org/wiki/Natural_language) or in [computer languages](https://en.wikipedia.org/wiki/Computer_languages), conforming to the rules of a [formal grammar](https://en.wikipedia.org/wiki/Formal_grammar).

#### A parser is a software component that takes input data (frequently text) and builds a [data structure] – often some kind of [parse tree](https://en.wikipedia.org/wiki/Data_structure), [abstract syntax tree](https://en.wikipedia.org/wiki/Parse_tree) or [other hierarchical structure](https://en.wikipedia.org/wiki/Abstract_syntax_tree) – giving a structural representation of the input, checking for correct syntax in the process. The parsing may be preceded or followed by other steps, or these may be combined into a single step.

#### AST

#### In [computer science](https://en.wikipedia.org/wiki/Computer_science), an abstract syntax tree (AST), or just syntax tree, is a [tree](https://en.wikipedia.org/wiki/Directed_tree) representation of the [abstract syntactic](https://en.wikipedia.org/wiki/Abstract_syntax) structure of [source code](https://en.wikipedia.org/wiki/Source_code) written in a [programming language](https://en.wikipedia.org/wiki/Programming_language). Each node of the tree denotes a construct occurring in the source code. The syntax is "abstract" in not representing every detail appearing in the real syntax.
