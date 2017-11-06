<p align="center"
# Chapter 9
/>

## Debugging python threads and sockets
### 9.1 Python threads

Let us dwelve deeper into how python threads are implemented in linux. Open the file
Python/thread.c and observe how different headers are included based on the implementation
of the thread library in the OS. On linux we use pthreads. So open the file
Python/thread_pthread.h.
Insert a breakpoint on line number 172.
Open the debugger and insert the following statements:
