# Chapter 1 Operating system interfaces
Operating systems provides services to user programs through an interface. xv6 has a traditional form of *kernel*, a special program that provides services to running programs. Each running program is called a *process*, has memory containing instructions, data, and a stack. The instructions implement the program's computation. The data are the variables on which the computation acts. The stack organizes the program's procedure calls. A given computer typically has many processes but only a single kernel.
When a process needs to invoke a kernel service, it invokes a *system call*, one of the calls in the operating system's interface. The system call enters the kernel; the kernel performs the service and returns. Thus a process alternates between executing in *user space* and *kernel space*.

## 1.1 Process and memory
### I. `fork` create *child* process
1. *child* process has exactly the same memory contents as the calling process with *parent* process
2. `fork` returns the child's PID in the parent; returns zero in the child.
### II. `exit` stop process
1. stop executing and release resources such as memory and open files
2. `exit` takes an integer argument, `exit(0)` indicate success, `exit(1)` indicate failure
### III. `wait` wait for child
1. returns the PID of an exited (or killed) child of the current process and copiesthe exit status of the child to the address passed to wait.
2. no excited children, `wait` waits for one to do so.
3. no children, immediately returns -1
4. if parent doesn't care about the exit status, pass a 0 address to `wait`: `wait((int*)0)`
change of PID of parent won't affect PID of child
### IV.  `exec` execute
1. replaces the calling process's memory with a new memory image (loaded from a file stored in the file system)
2. after succeeds, `exec` does not return to the calling rogram; instead, the instruction loaded from the file start executing at the entry point declared in the ELF header
3. `exec` takes two arguments: the name of the file containing the executable and an array of string arguments
4. Most programs ignore the first element of the argument array, which is conventionally the name of the program
### V. `sbrk`
1. `sbrk(n)` to grow data memory by n bytes; sbrk returns the location of the new memory

## 1.2 I/O and File descriptor
