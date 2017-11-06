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

