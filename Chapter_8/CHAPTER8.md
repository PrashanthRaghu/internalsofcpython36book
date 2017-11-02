<h1 align="center"> chapter 8 </h1>

<h2 align="center"> Debugging the memory allocator </h2>

To understand how the memory allocator works let us understand by using an example of how
lists are created. The example holds good for other memory objects as well. Open the file
Objects/listobject.c and insert a breakpoint into the line 163. It is a call to the macro
PyObject_GC_New which internally calls the function _PyObject_GC_New which is defined in
the file Modules/gcmodule.c which internally calls the function _PyObject_GC_Malloc defined in
the same file on line number 149 which internally calls the function _PyObject_GC_Alloc which
internally calls the function _PyObject_Alloc defined in the file Objects/obmalloc.c on line
number 1220.

In this function we observe that there is a set of preallocated pools in the array usedpools from
which we try to select a pool and allocate memory into the pool. Before we understand further
let us understand the structure of python memory allocation.

```python

struct pool_header {
    union { block *_padding;
            uint count; } ref;          /* number of allocated blocks    */
    block *freeblock;                   /* pool's free list head         */
    struct pool_header *nextpool;       /* next pool of this size class  */
    struct pool_header *prevpool;       /* previous pool       ""        */
    uint arenaindex;                    /* index into arenas of base adr */
    uint szidx;                         /* block size class index        */
    uint nextoffset;                    /* bytes to virgin block         */
    uint maxnextoffset;                 /* largest valid nextoffset      */
};


typedef struct pool_header* poolp; 


/* Record keeping for arenas. */


struct arena_object {


/* The address of the arena, as returned by malloc.  Note that 0
* will never be returned by a successful malloc , and is used
* here to mark an arena_object that doesn 't correspond to an
* allocated arena.
*/

    uptr address;

/* Pool-aligned pointer to the next pool to be carved off. */

    block* pool_address;

/* The number of available pools in the arena:  free pools + never-
* allocated pools.
*/

    uint nfreepools;

/* The total number of pools in the arena, whether or not available. */

    uint ntotalpools;

/* Singly-linked list of available pools. */

    struct pool_header* freepools;

/* Whenever this arena_object is not associated with an allocated
* arena , the nextarena member is used to link all unassociated
* arena_objects in the singly - linked `unused_arena_objects` list.
* The prevarena member is unused in this case.
*
* When this arena_object is associated with an allocated arena
* with at least one available pool , both members are used in the
* doubly - linked `usable_arenas` list , which is maintained in
* increasing order of `nfreepools` values.
*
* Else this arena_object is associated with an allocated arena
* all of whose pools are in use .  `nextarena` and `prevarena`
* are both meaningless in this case.
*/

    struct arena_object* nextarena;
    struct arena_object* prevarena;
}; 

```

From this we understand that the arena is an encapsulation of pools from which blocks of
memory are carved out according to the requirement of the application and given back. Let us
understand how this is done.
On line number 1249 we observe that a pool of the desired size is searched for. It is found and
contains a free block of the required size, it is assigned. Then if the pool is full, it is deallocated
from the usedpools list.
If the usable_arenas is empty then a new set of arenas are created using the new_arena
function on line number 1300. On line number 1314, we observe that the newly allocated pool is
linked to the usedpools array for further allocation. From this the selected block of memory is
carved out and assigned to the application. You are now familiar with the way memory is
allocated to the application by python. This flow is valid only when the size of the memory
requested is less than 512 bytes. Else malloc is directly invoked to assign memory to the
application. This can be observed in line number 1427.
Now that we have understood memory allocation let us understand the memory deallocation.
For that we need to understand two important macros Py_INCREF and Py_DECREF.

```python

#define Py_INCREF(op) (                      \
    _Py_INC_REFTOTAL _Py_REF_DEBUG_COMMA         \
    ((PyObject*)(op))->ob_refcnt++)

#define Py_DECREF(op)                        \
    do {
                                                \
        if ( _Py_DEC_REFTOTAL _Py_REF_DEBUG_COMMA       \
        --((PyObject*)(op))->ob_refcnt != 0)        \
            _Py_CHECK_REFCNT(op)                            \
        else                                            \
            _Py_Dealloc((PyObject *)(op));               \
       } while (0)

```

These are defined in the file Include/object.h.
From these two macros we observe that one increments the reference count of the object by 1
and the other decrements it when called. These macros can be seen all over the python source
code as it is imperative to call them when we are using and removing the reference to a
PyObject. When the reference count becomes 0, the function _Py_Dealloc is called. It internally
calls the tp_dealloc function defined on the type object. Let us observe how this is done for lists.
Let us open the file Objects/listobject.c on line number 2630, which defines the deallocator to be
the function list_dealloc. This is defined in the same file on line number 314.  On line number
326 we can see that it iterates through the list and deallocates the individual elements and on
line number 311 it calls the macro PyMem_FREE which is defined as the c memory collector
function free. Later it calls the function tp_free defined on lists which is the function
PyObject_GC_Del as defined in the type object on line number 2809. PyObject_GC_Del
internally calls PyObject_FREE which internally checks if the memory was allocated from the
pools and does the freeing operation. I shall skip the details here and suggest you to debug the
function step by step to understand how it works.
