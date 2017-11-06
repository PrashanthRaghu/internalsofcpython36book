# Chapter 9

## Debugging python threads and sockets

### 9.1 Python threads

Let us dwelve deeper into how python threads are implemented in linux. Open the file
Python/thread.c and observe how different headers are included based on the implementation
of the thread library in the OS. On linux we use pthreads. So open the file
Python/thread_pthread.h.
Insert a breakpoint on line number 172.
Open the debugger and insert the following statements:
```python
>>> def threadFunc():
        
        print "hello"

>>> import thread

>>> thread.start_new_thread(threadFunc,())
```
See how the thread is initialized and created and spawned to execute the function. Continue
debugging step by step and observce how it internally calls the PyEval_EvalFrameEx function /
the interpreter loop to be executed on the defined function.

### 9.2 Python sockets
This section contains a brief about how sockets are implemented in python. This is OS specific
hence I will only be covering for UNIX based systems. The main module is located in the file
Modules/socketmodule.h. Observe how the appropriate files are included into the system
depending on the operating system.
```python
typedef struct {

PyObject_HEAD

SOCKET_T sock_fd; /*Soclet file descriptor*/

int sock_family; /*Address family, e.g., AF_INET*/

int sock_type; /*Socket type, e.g., SOCK_STREAM*/

int sock_proto; /*Protocol type, usually 0*/

PyObject *(*errorhandler)(void); /*Error handler; checks errno, returns NULL and sets a Python exception*/

double sock_timeout; /*Operation timeout in seconds 0.0 means non-blocking*/

PyObject *weakreflist;

}PySocketSockObject;
```
Is the main socket object. Insert a breakpoint in the file Modules/socketmodule.c
on line number 806 and type the following statements.
```python
>>> import socket

>>> s = socket.socket(

    socket.AF_INET, socket.SOCK_STREAM)

Insert a breakpoint on line number 2155 and type the following statement:

>>> s.connect(("www.mcmillan-inc.com", 80))
```
Observe how the connection is established to the host.
The list of socket methods is defined in the same file on line number 3061.
Observe the mapping from the python methods to the corresponding C functions.
I would suggest you to debug each method to understand in depth of how
sockets are implemented in python.

