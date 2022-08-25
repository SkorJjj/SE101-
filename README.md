# SE101-Week2

Program : A computer program is an organized collection of instructions and a set of data these instructions can operate on

Machine code of a computer program consists of
* Headers
  - Administrative data structures required to run a program
* Text 
  - All instructions of a program; most architectures assume read-only access
* ROData
  - Read-only static data (i.e text string literals); it is meant for read-only access
* data
  - Writable static data (i.e. global variables) initialized with non-zero values
* BSS("block starting symbol")
  - Zero-initialized or non-initialized static data (i.e. global variables).
  
Process 
     - A program in execution
     - An instance of a program running on a computer
     - The entity that can be assigned to and executed on a processor
     - A unit of activity characterized by a single sequential thread of execution,  a current state
     
Consists of following processes
  - Machine code :  A sequence of instructions
  - State : values of CPU registers and the contents of allocated memory
  - acquired resources : resources from operating system and not yet released such as file descriptors, sockets, handles, memory blocks etc.
  - execution resources : resources needed to execute the code, including a process state records, CPU affinity, a priority level, accounting information, and security       configuration
How an OS starts a process?
* Generates a unique process identifier
* Creates a process image:
  - Allocated memory and copies program's segments from an executable into that memory
  - Allocated a BSS segment with the size specified in the executable's headers and fills it with zeros
* Updates OS' data structures:
  - Expands accounting, auditing and billing logs
  - Adds a process to the list of scheduled processes
* Creates a default thread, often referred to as the main thread:
  - Creates a stack segment for the thread with the size as specified in the excutable's headers
  - Adds the thread to the OS list of threads for the current process

1. Text Segment: A text segment, also known as a code segment or simply as text, is one of the sections of a program in an object file or in memory, which contains executable instructions.
As a memory region, a text segment may be placed below the heap or stack in order to prevent heaps and stack overflows from overwriting it. 

Usually, the text segment is sharable so that only a single copy needs to be in memory for frequently executed programs, such as text editors, the C compiler, the shells, and so on. Also, the text segment is often read-only, to prevent a program from accidentally modifying its instructions.

2. Initialized Data Segment: Initialized data segment, usually called simply the Data Segment. A data segment is a portion of the virtual address space of a program, which contains the global variables and static variables that are initialized by the programmer.
Note that, the data segment is not read-only, since the values of the variables can be altered at run time.
This segment can be further classified into the initialized read-only area and the initialized read-write area.
For instance, the global string defined by char s[] = “hello world” in C and a C statement like int debug=1 outside the main (i.e. global) would be stored in the initialized read-write area. And a global C statement like const char* string = “hello world” makes the string literal “hello world” to be stored in the initialized read-only area and the character pointer variable string in the initialized read-write area.
Ex: static int i = 10 will be stored in the data segment and global int i = 10 will also be stored in data segment

3. Uninitialized Data Segment: Uninitialized data segment often called the “bss” segment, named after an ancient assembler operator that stood for “block started by symbol.” Data in this segment is initialized by the kernel to arithmetic 0 before the program starts executing uninitialized data starts at the end of the data segment and contains all global variables and static variables that are initialized to zero or do not have explicit initialization in source code.
For instance, a variable declared static int i; would be contained in the BSS segment. 
For instance, a global variable declared int j; would be contained in the BSS segment.

4. Stack: The stack area traditionally adjoined the heap area and grew in the opposite direction; when the stack pointer met the heap pointer, free memory was exhausted. (With modern large address spaces and virtual memory techniques they may be placed almost anywhere, but they still typically grow in opposite directions.)
The stack area contains the program stack, a LIFO structure, typically located in the higher parts of memory. On the standard PC x86 computer architecture, it grows toward address zero; on some other architectures, it grows in the opposite direction. A “stack pointer” register tracks the top of the stack; it is adjusted each time a value is “pushed” onto the stack. The set of values pushed for one function call is termed a “stack frame”; A stack frame consists at minimum of a return address.
Stack, where automatic variables are stored, along with information that is saved each time a function is called. Each time a function is called, the address of where to return to and certain information about the caller’s environment, such as some of the machine registers, are saved on the stack. The newly called function then allocates room on the stack for its automatic variables. This is how recursive functions in C can work. Each time a recursive function calls itself, a new stack frame is used, so one set of variables doesn’t interfere with the variables from another instance of the function.

5. Heap: Heap is the segment where dynamic memory allocation usually takes place.
The heap area begins at the end of the BSS segment and grows to larger addresses from there. The Heap area is managed by malloc, realloc, and free, which may use the brk and sbrk system calls to adjust its size (note that the use of brk/sbrk and a single “heap area” is not required to fulfill the contract of malloc/realloc/free; they may also be implemented using mmap to reserve potentially non-contiguous regions of virtual memory into the process’ virtual address space). The Heap area is shared by all shared libraries and dynamically loaded modules in a process.

#include <stdio.h>
 
int main(void)
{
    return 0;
}

[narendra@CentOS]$ gcc memory-layout.c -o memory-layout
[narendra@CentOS]$ size memory-layout
text       data        bss        dec        hex    filename
960        248          8       1216        4c0    memory-layout


Add one global variable in the program, now check the size of bss (highlighted in red color).

#include <stdio.h>
 
int global; /* Uninitialized variable stored in bss*/
 
int main(void)
{
    return 0;
}

[narendra@CentOS]$ gcc memory-layout.c -o memory-layout
[narendra@CentOS]$ size memory-layout
text       data        bss        dec        hex    filename
 960        248         12       1220        4c4    memory-layout
 
 
Add one static variable which is also stored in bss.

#include <stdio.h>
 
int global; /* Uninitialized variable stored in bss*/
 
int main(void)
{
    static int i; /* Uninitialized static variable stored in bss */
    return 0;
}

[narendra@CentOS]$ gcc memory-layout.c -o memory-layout
[narendra@CentOS]$ size memory-layout
text       data        bss        dec        hex    filename
 960        248         16       1224        4c8    memory-layout
 
 
Initialize the static variable which will then be stored in the Data Segment (DS)

#include <stdio.h>
 
int global; /* Uninitialized variable stored in bss*/
 
int main(void)
{
    static int i = 100; /* Initialized static variable stored in DS*/
    return 0;
}

[narendra@CentOS]$ gcc memory-layout.c -o memory-layout
[narendra@CentOS]$ size memory-layout
text       data        bss        dec        hex    filename
960         252         12       1224        4c8    memory-layout


Initialize the global variable which will then be stored in the Data Segment (DS)

#include <stdio.h>
 
int global = 10; /* initialized global variable stored in DS*/
 
int main(void)
{
    static int i = 100; /* Initialized static variable stored in DS*/
    return 0;
}

[narendra@CentOS]$ gcc memory-layout.c -o memory-layout
[narendra@CentOS]$ size memory-layout
text       data        bss        dec        hex    filename
960         256          8       1224        4c8    memory-layout


Daemon
* Leverages multitasking -runs in a background by design
* Can run without any human user being logged on to the system

Thread : a basic unit of CPU utilization; it comprises a thread ID, a program counter (PC), a register set, and a stack

Multi-tasking
* Cooperative multi-tasking
  - Each processes voluntarily yields control to make space for another process
    - Easy to implement
    - Fast execution
    - Needs trust and correctdness
* Pre-emptive multi-tasking
  - OS gives every process and its threads a time quantum for execution, after which its suspends them and performs switching
    - system call
    - interrupt
    - trap

Scheduling
* Managing execution time of multiple processes or threads by:
  - Applying the process switching mechanisms on a single CPU
  - Allowing different processes to run on multiple CPUs at the same time.
