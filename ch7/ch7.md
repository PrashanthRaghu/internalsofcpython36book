# Chapter 7 
# Debugging python objects
## 7.1 The PyObject
In this chapter we shall understand the implementation of the python objects. Let us begin with
the PyObject which is the generic python object. It can be also called as the Object class of
python. It is defined in the file Include/object.h.
It’s is defined as follows:
```
typedef struct

 _object {
PyObject_HEAD
}

PyObject;
```
It contains a macro PyObject_HEAD which we will look into in the coming section.
```
#define PyObject_HEAD

_PyObject_HEAD_EXTRA

Py_ssize_t ob_refcnt;

struct _typeobject ob_type;
```
It contains two important elements the reference count and the type object. We will look into the
_typeobject object in the coming section 7.3.

## 7.2 The PyVarObject
Let us look at the definition for the python objects of variable length which is defined as:
```
typedef struct
{

PyObject_VAR_HEAD

}PyVarObject;
```
It contains a usage of the macro PyObject_VAR_HEAD which is explained in the coming paragraph.
```
#define PyObject_VAR_HEAD

PyObject_HEAD

Py_ssize_t ob_size;
/* Number of items in variable part */
```
It is basically the same as PyObject but with an additional field to denote the size of the variable
length of the object.
## 7.3 The PyTypeObject
The PyTypeObject is the representation of the type of the python object. To get the type of any
object in python open the python shell and enter the following statements:
```
>>> t = type(1)
>>> dir(t)

[​'__abs__'​,​​'__add__'​,​​'__and__'​,​​'__class__'​,​​'__cmp__'​,​​'__coerce__'​,​​'__delattr__'​,

'__div__'​,​​'__divmod__'​,​​'__doc__'​,​​'__float__'​,​​'__floordiv__'​,​​'__format__'​,

'__getattribute__'​,​​'__getnewargs__'​,​​'__hash__'​,​​'__hex__'​,​​'__index__'​,​​'__init__'​,

'__int__'​,​​'__invert__'​,​​'__long__'​,​​'__lshift__'​,​​'__mod__'​,​​'__mul__'​,​​'__neg__'​,​​'__new__'​,

'__nonzero__'​,​​'__oct__'​,​​'__or__'​,​​'__pos__'​,​​'__pow__'​,​​'__radd__'​,​​'__rand__'​,​​'__rdiv__'​,

'__rdivmod__'​,​​'__reduce__'​,​​'__reduce_ex__'​,​​'__repr__'​,​​'__rfloordiv__'​,​​'__rlshift__'​,

'__rmod__'​,​​'__rmul__'​,​​'__ror__'​,​​'__rpow__'​,​​'__rrshift__'​,​​'__rshift__'​,​​'__rsub__'​,

'__rtruediv__'​,​​'__rxor__'​,​​'__setattr__'​,​​'__sizeof__'​,​​'__str__'​,​​'__sub__'​,

'__subclasshook__'​,​​'__truediv__'​,​​'__trunc__'​,​​'__xor__'​,​​'bit_length'​,​​'conjugate'​,

'denominator'​,​​'imag'​,​​'numerator'​,​​'real']
```
We shall see the definition of these methods in the PyTypeObject. It is defined in the same file
object.h.

```
typedef struct _typeobject {
   PyObject_VAR_HEAD
   const char *tp_name; /* For printing, in format "<module>.<name>" */
   Py_ssize_t tp_basicsize,tp_itemsize; /* For allocation */
   /* Methods to implement standard operations */
   
    destructor tp_dealloc;
    
    printfunc tp_print;
    
    getattrfunc tp_getattr;
    
    setattrfunc tp_setattr;
    
    cmpfunc tp_compare;
    
    reprfunc tp_repr;
    
    /* Method suites for standard classes */
    
    PyNumberMethods *tp_as_number;
    
    PySequenceMethods *tp_as_sequence;
    
    PyMappingMethods *tp_as_mapping;
    
    /* More standard operations (here for binary compatibility) */
    
    hashfunc tp_hash;
    
    ternaryfunc tp_call;
    
    reprfunc tp_str;
    
    getattrofunc tp_getattro;
    
    setattrofunc tp_setattro;
    
    /* Functions to access object as input/output buffer */
    
    PyBufferProcs *tp_as_buffer;
    
    /* Flags to define presence of optional/expanded features */
    
    long tp_flags;
    
    const char *tp_doc;/* Documentation string */
    
    /* Assigned meaning in release 2.0 */
    
    /* call function for all accessible objects */
    
    traverseproc tp_traverse;
    
    /* delete references to contained objects */
    
    inquiry tp_clear;
    
    /* Assigned meaning in release 2.1 */
    
    /* rich comparisons */
    
    richcmpfunc tp_richcompare;
    
    /* weak reference enabler */
    
    Py_ssize_t tp_weaklistoffset;
    
    /* Added in release 2.2 */
    
    /* Iterators */
    
    getiterfunc tp_iter;
    
    iternextfunc tp_iternext;
    
    /* Attribute descriptor and subclassing stuff */
    
    struct PyMethodDef *tp_methods;
    
    struct PyMemberDef *tp_members;
    
    struct PyGetSetDef *tp_getset;
    
    struct _typeobject *tp_base;
    
    PyObject *tp_dict;
    
    descrgetfunc tp_descr_get;
    
    descrgetfunc tp_descr_set;
    
    Py_ssize_t tp_dictoffset;
    
    initproc tp_init;
    
    allocfunc tp_alloc;
    
    newfunc tp_new;
    
    freefunc tp_free; /* Low-level free-memory routine */
    
    inquiry tp_is_gc; /* For PyObject_IS_GC */
    
    PyObject *tp_bases;
    
    PyObject *tp_mro;
    
    PyObject *tp_cache;
    
    PyObject *tp_subclasses;
    
    PyObject *tp_weaklist;
    
    destructor tp_del;
    
      /* Type attribute cache version tag. Added in version 2.6 */

      unsigned int tp_version_tag;

    #ifdef COUNT_ALLOCS
    
    /* these must be last and never explicitly initialized */
    
    Py_ssize_t tp_allocs;
    
    Py_ssize_t tp_frees;
    
    Py_ssize_t tp_maxalloc;
    
    struct _typeobject *tp_prev;
    
    struct _typeobject *tp_next;
    
    #endif
    
    }PyTypeObject;
```
I shall not dwelve into the explanation for each of these as it is quite straightforward to
understand the same. However I shall explain how the typeobject is created for each type by
stating an example for the PyIntObject. It is defined in the file Objects/intobject.c on line number
1407.  I shall type it down here for easy reference. However it is better to observe it in the
source code.

```
PyTypeObject​​PyInt_Type​​=​{


​PyVarObject_HEAD_INIT​(&​PyType_Type​,​​0)


​"int",


​sizeof​(​PyIntObject​),


​0,


​(​destructor​)​int_dealloc​,​	​/* tp_dealloc */


​(​printfunc​)​int_print​,​	​/* tp_print */


​0​,​	​/* tp_getattr */


​0​,​	​/* tp_setattr */


​(​cmpfunc​)​int_compare​,​	​/* tp_compare */

​(​reprfunc​)​int_to_decimal_string​,  ​/* tp_repr */

​&​int_as_number​,​  ​/* tp_as_number */

​0​,​   /* tp_as_sequence */

​0​,​   ​/* tp_as_mapping */

​(​hashfunc​)​int_hash​,​  ​/* tp_hash */

​0​,​  ​/* tp_call */

​(​reprfunc​)​int_to_decimal_string​,​ ​/* tp_str */

​PyObject_GenericGetAttr​,​
​/* tp_getattro */
​0​,​  ​/* tp_setattro */
​0​,   ​/* tp_as_buffer */
​Py_TPFLAGS_DEFAULT​​|​​Py_TPFLAGS_CHECKTYPES​|


​Py_TPFLAGS_BASETYPE​​|​​Py_TPFLAGS_INT_SUBCLASS​,​ ​/* tp_flags */
int_doc​,  ​/* tp_doc */

​0​,​	​/* tp_traverse */
​0​,​	​/* tp_clear */


​0​,​	​/* tp_richcompare */


​0​,​	​/* tp_weaklistoffset */


​0​,​	​/* tp_iter */


​0​,​	​/* tp_iternext */


int_methods​,​	​/* tp_methods */


​0​,​	​/* tp_members */


int_getset​,​	​/* tp_getset */


​0​,​	​/* tp_base */


​0​,​	​/* tp_dict */


​0​,​	​/* tp_descr_get */


​0​,​	 ​/* tp_descr_set */


​0​,​	​   /* tp_dictoffset */


​0​,​	   ​/* tp_init */
​0​,​    ​/* tp_alloc */

int_new​,​  ​/* tp_new */


};


```
## 7.4 The PyIntObject



The PyIntObject is used to store the integer types. T he definition for this object is in the file Include/intobject.h.

```
typedef​​struct​{


PyObject_HEAD


long​ob_ival;


}​​PyIntObject;

```
The important thing to consider is that it is just a wrapping around a plain long type of the C programming language. To understand it let us debug an example. Open the file Objects/intobject.c and place a debug point on line number 89. Start debugging the application. Open the python shell in the debugger and type the expression.

```
a = 10l
```
The debugger is activated. Debug step by step to understand how the int object is created. The other methods in the file are fairly straight forward. I would urge you to go through some of them for better understanding.






## 7.5 The PyBoolObject



Thee PyBoolObject is used to store boolean types in python. The definition is available in the file Include/boolobject.h.

```
typedef​​PyIntObject​​PyBoolObject;
```


Now let us debug the application to understand how a bool object is created. Start debugging the application and open the python shell. Set a debug point in the file Objects/boolobject.c on line number 42.

Type the expression:

```
a = bool(20l)
```

We can see how the PyBoolObject is created. I would urge you to look into more methods to get a better understanding.
## 7.6 The PyLongObject



The PyLongObject is used to store long values in python. The definition of the object is present in the file Include/longobject.h.

```
typedef struct _longobject PyLongObject; /* Revealed in longintrepr.h */


struct​_longobject {


PyObject_VAR_HEAD


digit ob_digit​[​1​];


};

```
A long object is a set of two digits.


```
typedef PY_UINT32_T digit;



#define PY_UINT32_T uint32_t

```

## 7.7 The PyFloatObject



The PyFloatObject is used to store floating point numbers in python. The definition of this is in the file Include/floatobject.h

```
typedef​​struct​{

PyObject_HEAD


double​ob_fval;


}​​PyFloatObject;
```

We observe that it is just a wrapping around the plain double object of the C programming language.

## 7.8 The PyListObject



The PyListObject is the structure that is used to store the Python Lists.


```
typedef​​struct​{


PyObject_VAR_HEAD



/* Vector of pointers to list elements. list[0] is ob_item[0], etc. */


PyObject​​**​ob_item;


/* ob_item contains space for 'allocated' elements. The number


*​currently ​in​​use​​is​ob_size.


*​​Invariants:


*​​0​​<=​ob_size ​<=​allocated

language.
*​len​(​list​)​​==​ob_size


*​ob_item ​==​NULL implies ob_size ​==​allocated ​==​0


*​list​.​sort​()​temporarily sets allocated to ​-​1​to detect mutations.


*


*​​Items​must normally ​not​be NULL​,​​except​during construction ​when


*​the list ​is​​not​yet visible outside the ​function​that builds it.


*/


Py_ssize_t​allocated;


}​​PyListObject;

```
We see that the data is stored as an array of PyObjects. This is the main portion of the lists.



Open the debugger and set a debug point in the file Objects/listobject.c on line number 115.



In the debugger type the expression:



a = []
We see that the debugger is trapped. Debug step by step and understand how the list is created.

Set a debug point on line number 327 and type the expression:

a= range(0,10)

print a

In this function observe how the list is traversed and printed. Do check the other methods in listobject.c for further details on how lists work.

## 7.9 The PyDictObject



The PyDictObject is used to store python dictionaries. The declaration of the

structure is in the file Objects/dictobject.h.


```
typedef​​struct​{


/* Cached hash code of me_key. Note that hash codes are C longs.


*​​We​have to ​use​​Py_ssize_t​instead because dict_popitem​()​abuses


*​me_hash to hold a search finger.


*/
Py_ssize_t​me_hash;


PyObject​​*​me_key;


PyObject​​*​me_value;


}​​PyDictEntry;


typedef​​struct​_dictobject ​PyDictObject;


struct​_dictobject {


PyObject_HEAD


Py_ssize_t​ma_fill​;​​/* # Active + # Dummy */


Py_ssize_t​ma_used​;​​/* # Active */


/* The table contains ma_mask + 1 slots, and that's a power of 2.


*​​We​store the mask instead of the size because the mask ​is​more


*​frequently needed.


*/


Py_ssize_t​ma_mask;


/* ma_table points to ma_smalltable for small tables, else to
*​additional malloc​'​ed memory​.​ma_table ​is​never NULL​!​​This​rule


*​saves repeated runtime ​null​-​tests ​in​the workhorse getitem ​and


*​setitem calls.


*/


PyDictEntry​​*​ma_table;


PyDictEntry​​*(*​ma_lookup​)(​PyDictObject​​*​mp​,​​PyObject​​*​key​,​​long​hash​);


PyDictEntry​ma_smalltable​[​PyDict_MINSIZE​];


};
```


The dictionary contains a list of PyDictEntries which are mapped using the hashfunction of the key of the entry being stored into the dictionary.

Let us open Objects/dictobject.c and insert a breakpoint on line number 259.

Open the python console and type:



a = {}



We can see how the memory is allocated to objects using PyObject_GC_New which we will discuss in the coming posts. Also observe how the list is created.
mp->ma_lookup = lookdict_string;



The lookup method for the dictionary is the method lookdict_string which is defined in the same file. It contains the code on how to lookup values from the dictionary. I would suggest you to read this up.

## 7.10 Class related structures



Now let us examine the structures related to class operations.


```
typedef​​struct​{


PyObject_HEAD


PyObject​​*​cl_bases​;​​/* A tuple of class objects */


PyObject​​*​cl_dict​;​​/* A dictionary */


PyObject​​*​cl_name​;​​/* A string */


/* The following three are functions or NULL */


PyObject​​*​cl_getattr;


PyObject​​*​cl_setattr;


PyObject​​*​cl_delattr;
PyObject​​*​cl_weakreflist​;​​/* List of weak references */


}​​PyClassObject;


typedef​​struct​{


PyObject_HEAD


PyClassObject​​*​in_class​;​​/* The class object */


PyObject​​*​in_dict​;​​/* A dictionary */


PyObject​​*​in_weakreflist​;​​/* List of weak references */


}​​PyInstanceObject;


typedef​​struct​{


PyObject_HEAD


PyObject​​*​im_func​;​​/* The callable object implementing the method */


PyObject​​*​im_self​;​​/* The instance it is bound to, or NULL */


PyObject​​*​im_class​;​​/* The class that asked for the method */


PyObject​​*​im_weakreflist​;​​/* List of weak references */


}​​PyMethodObject;
```
The structures are found in Include/classobject.h and are self explanatory.



Open the file Objects/classobject.c and insert a breakpoint in line number 34. Debug the application and type the expression:

class a: pass

Observe how the breakpoint is trapped and a new class is created by going through the flow. Also insert breakpoints in other methods of the file and see how the class is manipulated and used.

## 7.11 The PyFunctionObject

The PyFunctionObject refers to a method created without the scope of a class.

The structure representing the function is declared in the file Include/funcobject.h


```
typedef​​struct​{


PyObject_HEAD


PyObject​​*​func_code​;​​/* A code object */


PyObject​​*​func_globals​;​​/* A dictionary (other mappings won't do) */


PyObject​​*​func_defaults​;​​/* NULL or a tuple */
PyObject​​*​func_closure​;​​/* NULL or a tuple of cell objects */


PyObject​​*​func_doc​;​​/* The __doc__ attribute, can be anything */


PyObject​​*​func_name​;​​/* The __name__ attribute, a string object */


PyObject​​*​func_dict​;​​/* The __dict__ attribute, a dict or NULL */


PyObject​​*​func_weakreflist​;​​/* List of weak references */


PyObject​​*​func_module​;​​/* The __module__ attribute, can be anything */


/* Invariant:


*​func_closure contains the bindings ​for​func_code​->​co_freevars​,​so


*​​PyTuple_Size​(​func_closure​)​​==​​PyCode_GetNumFree​(​func_code)


*​​(​func_closure may be NULL ​if​​PyCode_GetNumFree​(​func_code​)​​==​​0​).


*/


}​​PyFunctionObject;
```

The structure is self explanatory and I will not dwelve into further details. Also insert breakpoints in the file funcobject.c and observe the lifecycle of functions.
## 7.12 The PyCodeObject

The PyCodeObject represents the bytecode that is executed by the interpreter. We have seen this in the function PyEval_EvalFrameEx which contains the frame object which internally contains the code to be executed. The structure is defined in the file Include/code.h.
```
typedef​​struct​{



PyObject_HEAD


int​co_argcount​;​​/* #arguments, except *args */



int​co_nlocals​;​​/* #local variables */



int​co_stacksize​;​​/* #entries needed for evaluation stack */



int​co_flags​;​​/* CO_…, see below */



PyObject​​*​co_code​;​​/* instruction opcodes */



PyObject​​*​co_consts​;​​/* list (constants used) */



PyObject​​*​co_names​;​​/* list of strings (names used) */



PyObject​​*​co_varnames​;​​/* tuple of strings (local variable names) */



PyObject​​*​co_freevars​;​​/* tuple of strings (free variable names) */



PyObject​​*​co_cellvars​;​​/* tuple of strings (cell variable names) */


/* The rest doesn't count for hash/cmp */


PyObject​​*​co_filename​;​​/* string (where it was loaded from) */



PyObject​​*​co_name​;​​/* string (name, for reference) */



int​co_firstlineno​;​​/* first source line number */



PyObject​​*​co_lnotab​;​​/* string (encoding addr<->lineno mapping) See



Objects​/​lnotab_notes​.​txt ​for​details​.​​*/



void​​*​co_zombieframe​;​​/* for optimization only (see frameobject.c) */



PyObject​​*​co_weakreflist​;​​/* to support weakrefs to code objects */



}​​PyCodeObject;

```

The most important entry is the co_code entry which contains the python bytecode.



Open the file named codeobject.c and insert a breakpoint in line number 101. Open the debug console and type any expression:
```
>>> a = 100
```


We see that a new corresponding CodeObject is created. I would further urge you to debug and understand more about this object.
## 7.13 The PyFrameObject

The python frame object contains an entry in the execution stack and contains the wrapper to the code being executed and an instruction pointer. The declaration is in the file Include/frameobject.h.

```
typedef​​struct​_frame {



PyObject_VAR_HEAD


struct​_frame ​*​f_back​;​​/* previous frame, or NULL */



PyCodeObject​​*​f_code​;​​/* code segment */



PyObject​​*​f_builtins​;​​/* builtin symbol table (PyDictObject) */



PyObject​​*​f_globals​;​​/* global symbol table (PyDictObject) */



PyObject​​*​f_locals​;​​/* local symbol table (any mapping) */



PyObject​​**​f_valuestack​;​​/* points after the last local */



/* Next free slot in f_valuestack. Frame creation sets to f_valuestack.


Frame​evaluation usually ​NULLs​it​,​but a frame that yields sets it



to the current stack top​.​​*/



PyObject​​**​f_stacktop;

PyObject​​*​f_trace​;​​/* Trace function */



/* If an exception is raised in this frame, the next three are used to


*​record the exception info ​(​if​any​)​originally ​in​the thread state​.​​See



*​comments before set_exc_info​()​​—​it​'​s ​not​obvious.



*​​Invariant​:​​if​_type ​is​NULL​,​​then​so are _value ​and​_traceback.



*​​Desired​invariant​:​all three are NULL​,​​or​all three are non​-​NULL​.​​That



*​one isn​'​t currently ​true​,​but ​"​should be​".



*/


PyObject​​*​f_exc_type​,​​*​f_exc_value​,​​*​f_exc_traceback;



PyThreadState​​*​f_tstate;



int​f_lasti​;​​/* Last instruction if called */



/* Call PyFrame_GetLineNumber() instead of reading this field


directly​.​​As​of ​2.3​f_lineno ​is​only valid ​when​tracing ​is



active ​(​i​.​e​.​​when​f_trace ​is​​set​).​​At​other times we ​use



PyCode_Addr2Line​to calculate the line ​from​the current

bytecode index​.​​*/



int​f_lineno​;​​/* Current line number */



int​f_iblock​;​​/* index in f_blockstack */



PyTryBlock​f_blockstack​[​CO_MAXBLOCKS​];​​/* for try and loop blocks */



PyObject​​*​f_localsplus​[​1​];​​/* locals+stack, dynamically sized */



}​​PyFrameObject;
```

To understand more about the frame object debug the interpreter loop to see how opcodes are fetched from the frame object and further executed.

## 7.14 The PyGenObject



The PyGenObject is the structure that wraps the Python generator. The declaration is in the file Include/genobject.h.
```
typedef​​struct​{



PyObject_HEAD



/* The gi_ prefix is intended to remind of generator-iterator. */



/* Note: gi_frame can be NULL if the generator is "finished" */
struct​_frame ​*​gi_frame;



/* True if generator is being executed. */


int​gi_running;



/* The code object backing the generator */


PyObject​​*​gi_code;



/* List of weak reference. */


PyObject​​*​gi_weakreflist;



}​​PyGenObject;

```

You can observe the generator object contains a reference to the frame to which the function must enter after yielding.

Open the file Objects/genobject.c and insert a breakpoint in line number 385.



Enter the function:


```
>>>​def​gen​(​a​):



while​a ​<​​10:



​+=​1


yield


>>>​a ​=​gen​(​5)



You see that the breakpoint is trapped. Observe how the generator is created.



>>​a​.​next​()

```

And insert a breakpoint into line number 283. There is call to function gen_send_ex. Insert a breakpoint into line number 85, which is a call to the interpreter loop for the current frame and instruction pointer.

## 7.15 ​The PyFileObject


The PyFileObject is used in file management of python. The structure is defined

in the file Include/fileobject.h


```
typedef​​struct​{



PyObject_HEAD


FILE ​*​f_fp;



PyObject​​*​f_name;



PyObject​​*​f_mode;

int​​(*​f_close​)(​FILE ​*);



int​f_softspace​;​​/* Flag used by 'print' command */



int​f_binary​;​​/* Flag which indicates whether the file is



open ​in​binary ​(​1​)​​or​text ​(​0​)​mode ​*/



char​*​f_buf​;​​/* Allocated readahead buffer */



char​*​f_bufend​;​​/* Points after last occupied position */



char​*​f_bufptr​;​​/* Current buffer position */



char​​*​f_setbuf​;​​/* Buffer for setbuf(3) and setvbuf(3) */



int​f_univ_newline​;​​/* Handle any newline convention */



int​f_newlinetypes​;​​/* Types of newlines seen */



int​f_skipnextlf​;​​/* Skip next \n */



PyObject​​*​f_encoding;



PyObject​​*​f_errors;



PyObject​​*​weakreflist​;​​/* List of weak references */



int​unlocked_count​;​​/* Num. currently running sections of code

using​f_fp ​with​the GIL released​.​​*/



int​readable;



int​writable;



}​​PyFileObject;

```

We observe that this is a wrapper to a plain C FILE object and has three buffers to process the data.

Open the file Objects/fileobject.c and insert a breakpoint on line number 322 and in the debug console type the expression:

a = open(‘test.py’)



You see that the debug point is trapped. Observe how the file is created and the pointers are set for operations.

Adios, you are now familiar with most objects of python that we commonly use in our everyday operations.

## 7.16 The Interpreter state Object



The object that handles the interpreter of python. It is located in the file Include/pystate.h.


```
typedef​​struct​_is {


​struct​_is ​*​next;


​struct​_ts ​*​tstate_head;


​PyObject​​*​modules;


​PyObject​​*​modules_by_index;


​PyObject​​*​sysdict;


​PyObject​​*​builtins;


​PyObject​​*​importlib;


​PyObject​​*​codec_search_path;


​PyObject​​*​codec_search_cache;
​PyObject​​*​codec_error_registry;


​int​codecs_initialized;


​int​fscodec_initialized;


#ifdef​HAVE_DLOPEN


​int​dlopenflags;


#endif

​PyObject​​*​builtins_copy;


​PyObject​​*​import_func;


​/* Initialized to PyEval_EvalFrameDefault(). */


​PyFrameEvalFunction​eval_frame;



}​​PyInterpreterState;
```

## 7.17 The PyThreadStateObject



Handles the current state of the executing threads. It is also defined in the file Include/pystate.h


```
typedef​​struct​_ts {


​/* See Python/ceval.c for comments explaining most fields */


​struct​_ts ​*​prev;


​struct​_ts ​*​next;


​PyInterpreterState​​*​interp;


​struct​_frame ​*​frame;


​int​recursion_depth;


​char​overflowed​;​​/* The stack has overflowed. Allow 50 more calls


to handle the runtime error​.​​*/


​char​recursion_critical​;​​/* The current calls must not cause
a stack overflow​.​​*/


​/* 'tracing' keeps track of the execution depth when tracing/profiling.


​This​​is​to prevent the actual trace​/​profile code ​from​being recorded ​in


the trace​/​profile​.​​*/


​int​tracing;


​int​use_tracing;

​Py_tracefunc​c_profilefunc;


​Py_tracefunc​c_tracefunc;


​PyObject​​*​c_profileobj;


​PyObject​​*​c_traceobj;

​PyObject​​*​curexc_type;


​PyObject​​*​curexc_value;
PyObject​​*​curexc_traceback;

​PyObject​​*​exc_type;


​PyObject​​*​exc_value;


​PyObject​​*​exc_traceback;

​PyObject​​*​dict​;​ ​/* Stores per-thread state */

​int​gilstate_counter;

​PyObject​​*​async_exc​;​​/* Asynchronous exception to raise */


​long​thread_id​;​​/* Thread id where this tstate was created */


​int​trash_delete_nesting;
​PyObject​​*​trash_delete_later;

​/* Called when a thread state is deleted normally, but not when it


​*​​is​destroyed after fork​().


​*​​Pain​:​ to prevent rare but fatal shutdown errors ​(​issue ​18808​),


​*​​Thread​.​join​()​must wait ​for​the join​'ed thread'​s tstate to be unlinked


​*​​from​the tstate chain​.​ ​That​happens at the ​end​of a thread​'s life,


​*​​in​pystate​.​c.


​*​​The​obvious way doesn​'t quite work:	create a lock which the tstate


​*​unlinking code releases​,​​and​have ​Thread​.​join​()​wait to acquire that


​*​​lock​.​ ​The​problem ​is​that we _are_ at the ​end​of the thread​'s life:


​*​​if​the thread holds the ​last​reference to the ​lock​,​decref​'ing the


​*​​lock​will ​delete​the ​lock​,​​and​that may trigger arbitrary ​Python​code


​*​​if​there​'s a weakref, with a callback, to the lock.	But by this time
*​​PyThreadState_Current​​is​already NULL​,​so only the simplest of C code



​*​can be allowed to run ​(​in​particular it must ​not​be possible to


​*​release the GIL​).


​*​​So​instead of holding the ​lock​directly​,​the tstate holds a weakref to


​*​the ​lock​:​ that​'s the value of on_delete_data below.	Decref'​ing a


​*​weakref ​is​harmless.


​*​on_delete points to _threadmodule​.​c​'s static release_sentinel() function.


​*​​After​the tstate ​is​unlinked​,​release_sentinel ​is​called ​with​the


​*​weakref​-​to​-​lock​​(​on_delete_data​)​argument​,​​and​release_sentinel releases


​*​the indirectly held ​lock.


​*/

​void​​(*​on_delete​)(​void​​*);

​void​​*​on_delete_data;
​PyObject​​*​coroutine_wrapper;


​int​in_coroutine_wrapper;

​Py_ssize_t​co_extra_user_count;


freefunc co_extra_freefuncs​[​MAX_CO_EXTRA_USERS​];

​PyObject​​*​async_gen_firstiter;


​PyObject​​*​async_gen_finalizer;

​/* XXX signal handlers should also be here */

}​​PyThreadState;
```
