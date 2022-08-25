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

Daemon
* Leverages multitasking -runs in a background by design
* Can run without any human user being logged on to the system

