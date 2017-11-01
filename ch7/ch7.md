# Chapter 7
# Debugging python objects
# 7.1 The PyObject
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
#definePyObject_HEAD

_PyObject_HEAD_EXTRA

Py_ssize_t ob_refcnt;

struct _typeobject ob_type;
```
It contains two important elements the reference count and the type object. We will look into the
_typeobject object in the coming section 7.3.

# 7.2 The PyVarObject
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
# 7.3 The PyTypeObject
The PyTypeObject is the representation of the type of the python object. To get the type of any
object in python open the python shell and enter the following statements:
>>>
