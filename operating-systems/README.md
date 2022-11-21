# Operating Systems

## **Process**

### Define the kind of types of process in Linux

TASK\_RUNNING—The process is runnable; it is either currently running or on a runqueue waiting to run. This is the only possible state for a process executing in user-space; it can also apply to a process in kernel-space that is actively running.

TASK\_INTERRUPTIBLE—The process is sleeping (that is, it is blocked), waiting for some condition to exist. When this condition exists, the kernel sets the process's state to TASK\_RUNNING. The process also awakes prematurely and becomes runnable if it receives a signal.

TASK\_UNINTERRUPTIBLE—This state is identical to TASK\_INTERRUPTIBLE except that it does not wake up and become runnable if it receives a signal. This is used in situations where the process must wait without interruption or when the event is expected to occur quite quickly. Because the task does not respond to signals in this state, TASK\_UNINTERRUPTIBLE is less often used than TASK\_INTERRUPTIBLE6.

TASK\_ZOMBIE—The task has terminated, but its parent has not yet issued a wait4() system call. The task's process descriptor must remain in case the parent wants to access it. If the parent calls wait4(), the process descriptor is deallocated.

TASK\_STOPPED—Process execution has stopped; the task is not running nor is it eligible to run. This occurs if the task receives the SIGSTOP, SIGTSTP, SIGTTIN, or SIGTTOU signal or if it receives any signal while it is being debugged.

[http://lxr.free-electrons.com/source/include/linux/sched.h\\#L207](http://lxr.free-electrons.com/source/include/linux/sched.h#L207)

### Zombie Process

When a process dies on Linux, it isn’t all removed from memory immediately — its process descriptor stays in memory (the process descriptor only takes a tiny amount of memory). The process’s status becomes EXIT\_ZOMBIE and the process’s parent is notified that its child process has died with the SIGCHLD signal. The parent process is then supposed to execute the wait system call to read the dead process’s exit status and other information. This allows the parent process to get information from the dead process. After wait is called, the zombie process is completely removed from memory.

This normally happens very quickly, so you won’t see zombie processes accumulating on your system. However, if a parent process isn’t programmed properly and never calls wait, its zombie children will stick around in memory until they’re cleaned up.

Zombie processes don’t use up any system resources. Actually, each one uses a very tiny amount of system memory to store its process descriptor.) However, each zombie process retains its process ID (PID). Linux systems have a finite number of process IDs – 32767 by default on 32-bit systems. If zombies are accumulating at a very quick rate – for example, if improperly programmed server software is creating zombie processes under load — the entire pool of available PIDs will eventually become assigned to zombie processes, preventing other processes from launching.

### Getting Rid of Zombie Processes

You can’t kill zombie processes as you can kill normal processes with the SIGKILL signal — zombie processes are already dead. Bear in mind that you don’t need to get rid of zombie processes unless you have a large amount on your system – a few zombies are harmless. However, there are a few ways you can get rid of zombie processes.

One way is by sending the SIGCHLD signal to the parent process. This signal tells the parent process to execute the wait system call and clean up its zombie children. Send the signal with the kill command, replacing\_pid\_in the command below with the parent process’s PID:

> kill -s SIGCHLD pid

However, if the parent process isn’t programmed properly and is ignoring SIGCHLD signals, this won’t help. You’ll have to kill or close the zombies’ parent process. When the process that created the zombies ends, init inherits the zombie processes and becomes their new parent. (init is the first process started on Linux at boot and is assigned PID 1.) init periodically executes the wait system call to clean up its zombie children, so init will make short work of the zombies. You can restart the parent process after closing it.

If a parent process continues to create zombies, it should be fixed so that it properly calls wait to reap its zombie children. File a bug report if a program on your system keeps creating zombies.

### Difference between Process and a Thread

A thread is a lightweight process. Each process has a separate stack, text, data and heap. Threads have their own stack, but share text, data and heap with the process. Text is the actual program itself, data is the input to the program and heap is the memory which stores files, locks, sockets. Reference: [https://computing.llnl.gov/tutorials/pthreads/#Thread](https://computing.llnl.gov/tutorials/pthreads/#Thread)

### IPC methods

POSIX mmap, message queues, semaphores, and shared memory

Anonymous pipes and named pipes

Unix domain sockets

RPC For a complete list see [http://en.wikipedia.org/wiki/Inter-process\\\_communication](http://en.wikipedia.org/wiki/Inter-process\_communication)

### Ulimit per process

A process can change its limits via the setrlimit(2) system call. When you run ulimit -n you should see a number. That's the current limit on number of open file descriptors (which includes files, sockets, pipes, etc) for the process. The ulimit command executed the getrlimit(2) system call to find out what the current value is.The pam\_limits PAM module sets limits on the system resources that can be obtained in a user-session. Users of uid=0 are affected by this limits, too.

cd /proc/7671/

\[root@host]# cat limits | grep nice

Max nice priority 0 0

\[root@host]# echo -n "Max nice priority=5:6" > limits

\[root@host]# cat limits | grep nice

Max nice priority 5 6

### Context switch

Context switching can be described in slightly more detail as the [kernel](http://www.linfo.org/kernel.html) (i.e., the core of the [operating system](http://www.linfo.org/operating\_systems\_list.html)) performing the following activities with regard to processes (including threads) on the CPU: (1) suspending the progression of one process and storing the CPU's state(i.e., the context) for that process somewhere in memory, (2) retrieving the context of the next process from memory and restoring it in the CPU's registers and (3) returning to the location indicated by the program counter (i.e., returning to the line of code at which the process was interrupted) in order to resume the process.

A context switch is sometimes described as the kernel suspending execution of one process on the CPU and resuming execution of some other process that had previously been suspended. Although this wording can help clarify the concept, it can be confusing in itself because a process is, by definition, an executing instance of a program. Thus the wording suspending progression of a process might be preferable.

Context switches can occur only in kernel mode. Kernel mode is a privileged mode of the CPU in which only the kernel runs and which provides access to all memory locations and all other system resources. Other programs, including applications, initially operate in user mode, but they can run portions of the kernel code via system calls. A system call is a request in a Unix-like operating system by an active process (i.e., a process currently progressing in the CPU) for a service performed by the kernel, such as input/output (I/O) or process creation (i.e., creation of a new process). I/O can be defined as any movement of information to or from the combination of the CPU and main memory (i.e. RAM), that is, communication between this combination and the computer's users (e.g., via the keyboard or mouse), its storage devices (e.g., disk or tape drives), or other computers.

### What is the number of child process created by fork(); fork(); fork(); ?

Start with 1(Main Process) and for every fork make it twice if fork is not inside if(pid == 0) else add 1/2 of current process to current number of process.

In your code: 1P Got #1 fork() so double up current number of processes. Now new number of process 2P

Got #2 fork() so double up current number of processes. Now new number of process 4P

Got #3 fork() so double up current number of processes. Now new number of process 8P

Got #4 fork() but wait it's in if condition so (8+4 = 12)P

Got #5 fork() so double up the current number of processes. Now new number of process 24P

The fork() sys. call grows using a binary tree. So, the number of child created after its implementation are generally given by 2^n - 1, though the results may vary as per implementation(looping) and calling of wait().

### For (int i=0;i<10;i++) fork(); how many process will be created?

So in short 2^10 - 1 process will spawn out of your for loop.

### Differenced between fork(), pthread() and vfork() system calls

fork() - it does call clone() inside it but with the flag as SIGCHLD i.e. clone(SIGCHLD, 0). So, none of the resources are shared. But, copied to child on COW basis.

pthread() - is a POSIX wrapper for clone() system call to create a thread, in this case clone is called like clone( CLONE\_VM | CLONE\_FS | CLONE\_FILE | CLONE\_SIGHNDL). So, address space, open files, file system handlers, signal handlers are shared.

vfork() - is also calling clone() similar to fork(), only difference is process address space is not copied to child and execution of parent is suspended till the execution of child. They used it before COW was introduced.

### Copy on write

Copy-on-write finds its main use in sharing the [virtual memory](https://en.wikipedia.org/wiki/Virtual\_memory) of [operating system](https://en.wikipedia.org/wiki/Operating\_system) [processes](https://en.wikipedia.org/wiki/Computer\_process), in the implementation of the\[fork system call]\([https://en.wikipedia.org/wiki/Fork\_(system\_call)\\](https://en.wikipedia.org/wiki/Fork\_\(system\_call\)\)/). Typically, the process does not modify any memory and immediately executes a new process, replacing the address space entirely. Thus, it would be wasteful to copy all of the process's memory during a fork, and instead the copy-on-write technique is used. It can be implemented efficiently using the [page table](https://en.wikipedia.org/wiki/Page\_table) by marking certain pages of [memory](https://en.wikipedia.org/wiki/Computer\_storage) as read-only and keeping a count of the number of references to the page. When data is written to these pages, the\[kernel]\([https://en.wikipedia.org/wiki/Kernel\_(computing))intercepts](https://en.wikipedia.org/wiki/Kernel\_\(computing\)\)intercepts) the write attempt and allocates a new physical page, initialized with the copy-on-write data, although the allocation can be skipped if there is only one reference. The kernel then updates the page table with the new (writable) page, decrements the number of references, and performs the write. The new allocation ensures that a change in the memory of one process is not visible in another's. The copy-on-write technique can be extended to support efficient [memory allocation](https://en.wikipedia.org/wiki/Memory\_allocation) by having a page of [physical memory](https://en.wikipedia.org/wiki/Physical\_memory) filled with zeros. When the memory is allocated, all the pages returned refer to the page of zeros and are all marked copy-on-write. This way, physical memory is not allocated for the process until data is written, allowing processes to reserve more virtual memory than physical memory and use memory sparsely, at the risk of running out of virtual address space. The combined algorithm is similar to[demand paging](https://en.wikipedia.org/wiki/Demand\_paging).[\[3\]](https://en.wikipedia.org/wiki/Copy-on-write#cite\_note-Linux-3)

Copy-on-write pages are also used in the[Linux kernel](https://en.wikipedia.org/wiki/Linux\_kernel)'s[kernel same-page merging](https://en.wikipedia.org/wiki/Kernel\_same-page\_merging)feature.[\[4\]](https://en.wikipedia.org/wiki/Copy-on-write#cite\_note-4)

### Number of threads

Linux doesn't have a separate threads per process limit, just a limit on the total number of processes on the system (threads are essentially just processes with a shared address space on Linux) which you can view like this:

> cat /proc/sys/kernel/threads-max
>
> Limit CPU intensive and io intensive

This is WRONG to say that LINUX doesn't have a separate threads per process limit.

Linux implements max number of threads per process indirectly

number of threads = total virtual memory / (stack size\*1024\*1024)

Thus, the number of threads per process can be increased by increasing total virtual memory or by decreasing stack size. But, decreasing stack size too much can lead to code failure due to stack overflow while max virtual memory is equals to the swap memory.

Check you machine:

Total Virtual Memory: ulimit -v (default is unlimited, thus you need to increase swap memory to increase this)

Total Stack Size: ulimit -s (default is 8Mb)

Command to increase these values:

ulimit -s newvalue

ulimit -v newvalue

\*Replace new value with the value you want to put as limit.

### Limit CPU intensive and io intensive

[http://stackoverflow.com/questions/12892941/cpu-usage-vs-number-of-threads](http://stackoverflow.com/questions/12892941/cpu-usage-vs-number-of-threads)

[http://changelog.ca/quote/2013/04/07/cpu\_vs\_io\_bound\_threads](http://changelog.ca/quote/2013/04/07/cpu\_vs\_io\_bound\_threads)

### Stack VS Heap (global or shared)

Example: One stack per thread. 5 processes with 2 threads each equals 10 stacks.

Brek

## **Memory**

### Demand paging

In computer operating systems, demand paging (as opposed to anticipatory paging) is a method of virtual memory management. In a system that uses demand paging, the operating system copies a disk page into physical memory only if an attempt is made to access it and that page is not already in memory (i.e., if a page fault occurs). It follows that a process begins execution with none of its pages in physical memory, and many page faults will occur until most of a process's working set of pages is located in physical memory. This is an example of a lazy loading technique.

Demand paging follows that pages should only be brought into memory if the executing process demands them. This is often referred to as lazy evaluation as only those pages demanded by the process are swapped from secondary storage to main memory. Contrast this to pure swapping, where all memory for a process is swapped from secondary storage to main memory during the process startup.

Commonly, to achieve this process a page table implementation is used. The page table maps logical memory to physical memory. The page table uses a bitwise operator to mark if a page is valid or invalid. A valid page is one that currently resides in main memory. An invalid page is one that currently resides in secondary memory. When a process tries to access a page, the following steps are generally followed:

Attempt to access page.

If page is valid (in memory) then continue processing instruction as normal.

If page is invalid then a page-fault trap occurs.

Check if the memory reference is a valid reference to a location on secondary memory. If not, the process is terminated (illegal memory access). Otherwise, we have to page in the required page.

Schedule disk operation to read the desired page into main memory.

Restart the instruction that was interrupted by the operating system trap.

### Page fault

A page fault (sometimes called #PF, PF or hard fault\[a]) is a type of exception raised by computer hardware when a running program accesses a memory page that is not currently mapped by the memory management unit (MMU) into the virtual address space of a process. Logically, the page may be accessible to the process, but requires a mapping to be added to the process page tables, and may additionally require the actual page contents to be loaded from a backing store such as a disk. The processor's MMU detects the page fault, while the exception handling software that handles page faults is generally a part of the operating system kernel. When handling a page fault, the operating system generally tries to make the required page accessible at the location in physical memory, or terminates the program in case of an illegal memory access.

Contrary to what "fault" might suggest, valid page faults are not errors, and are common and necessary to increase the amount of memory available to programs in any operating system that utilizes virtual memory, including OpenVMS, Microsoft Windows, Unix-like systems (including Mac OS X, Linux, \*BSD, Solaris, AIX, and HP-UX), and z/OS.

### What is cached?

In top that number before cached is the amount of physical memory that is being used by cache buffers for your filesystems.

[http://www.linuxatemyram.com/play.html](http://www.linuxatemyram.com/play.html)

To verify my answer with a little experiment try the following:

Run top and note the value of 'cached'. Now run

```
dd if=/dev/zero of=~/trick bs=1M count=128
```

if you run top again you'll notice that 'cached' has grown by**128M**

Now remove the file

```
rm ~/trick
```

The Linux kernel will use available memory for disk caching, unless it's required by a running program.

This is considered good; say you have 4 GB RAM, and your programs are using only 1 GB. The other 3 GB are going to waste. Despite the "feel-good" impression from knowing you're only using 25% of your memory, the counterpart is that the other 75% is going unused. So the kernel uses that for caching files which significantly improves performance. It's automatic; unlike older operating systems you don't need to decide how much to devote to disk cache, or manually configure it.

"The Linux disk cache is very unobtrusive. It uses spare memory to greatly increase disk access speeds, and without taking any memory away from applications. A fully used store of ram on Linux is efficient hardware use, not a warning sign."

### How malloc works?

The sbrk system call moves the "border" of the data segment. This means it moves a border of an area in which a program may read/write data (letting it grow or shrink, although AFAIK no malloc really gives memory segments back to the kernel with that method). Aside from that, there's also mmap which is used to map files into memory but is also used to allocate memory (if you need to allocate shared memory, mmap is how you do it).

So you have two methods of getting more memory from the kernel: sbrk and mmap. There are various strategies on how to organize the memory that you've got from the kernel.

One naive way is to partition it into zones, often called "buckets", which are dedicated to certain structure sizes. For example, a malloc implementation could create buckets for 16, 64, 256 and 1024 byte structures. If you ask malloc to give you memory of a given size it rounds that number up to the next bucket size and then gives you an element from that bucket. If you need a bigger area malloc could use mmap to allocate directly with the kernel. If the bucket of a certain size is empty malloc could use sbrk to get more space for a new bucket.

There are various malloc designs and there is propably no one true way of implementing malloc as you need to make a compromise between speed, overhead and avoiding fragmentation/space effectiveness. For example, if a bucket runs out of elements an implementation might get an element from a bigger bucket, split it up and add it to the bucket that ran out of elements. This would be quite space efficient but would not be possible with every design. If you just get another bucket via sbrk/mmap that might be faster and even easier, but not as space efficient. Also, the design must of course take into account that "free" needs to make space available to malloc again somehow. You don't just hand out memory without reusing it.

If you're interested, the OpenSER/Kamailio SIP proxy has two malloc implementations (they need their own because they make heavy use of shared memory and the system malloc doesn't support shared memory). See: [https://github.com/OpenSIPS/opensips/tree/master/mem](https://github.com/OpenSIPS/opensips/tree/master/mem)

Then you could also have a look at the GNU libc malloc implementation.

### Physical vs virtual memory

Physical memory refers to the RAM and the hard disks in the computer that are used to store data. In a computer the operating system, application programs and currently used data are kept in the RAM, so that they could be accessed quickly by the processor. RAM could be accessed faster than the other storage devices such as the hard disk and CD-ROM. But the data in the RAM exists only while the computer is running. When the power is turned off, all the data in the RAM are lost and the operating system and other data are loaded again to the RAM from the hard disk when the computer is turned on. Hard disk is a non-volatile memory (a memory that keeps data even when it is not powered) that is used to store data in a computer. It is made up of circular disks called platters that stores magnetic data. Data is written and read to and from the platters using read/ write heads.

**Virtual Memory**

Virtual memory is used when the computer lacks the RAM space for executing a program. Virtual memory combines the RAM space with the hard disk space. When the computer does not have enough RAM space to execute a program, the virtual memory transfers data from the RAM to a paging file, which frees up the space in the RAM. A portion of the hard disk is used to store the page file. This transferring process is done so rapidly so that the user does not feel a difference. Virtual memory can hold an entire block of data while the part that is currently executing resides on the RAM. Therefore the virtual memory allows the operating system to run several programs at the same time thus increasing the degree of multiprogramming. While increasing the size of the programs that could be executed, virtual memory provides cost benefits since hard disk memory is less expensive than the RAM.

**What is the difference between Physical and Virtual Memory**

While physical memory refers to physical devices that stores data in a computer such as the RAM and hard disk drives, virtual memory combines the RAM space with the hard drive space to store data in the RAM, when the RAM space is not enough. Part of the hard disk is used to store the page files that are used by the virtual memory to store the data that are transferred from the RAM. Even though swapping the data between the page files in the hard disk and the RAM (via the virtual memory) is very fast, too much swapping could slower the overall performance of the system.

## Protected mode

[https://en.wikipedia.org/wiki/Protected\\\_mode](https://en.wikipedia.org/wiki/Protected\_mode)

[https://en.wikipedia.org/wiki/Long\\\_mode](https://en.wikipedia.org/wiki/Long\_mode)

[https://en.wikipedia.org/wiki/Paging](https://en.wikipedia.org/wiki/Paging)

## **CPU**

### Registers

Registers are the most important components of CPU. Each register performs a specific function. A brief description of most important CPU's registers and their functions are given below:

Memory Address Register (MAR):

This register holds the address of memory where CPU wants to read or write data. When CPU wants to store some data in the memory or reads the data from the memory, it places the address of the required memory location in the MAR.

Memory Buffer Register (MBR):

This register holds the contents of data or instruction read from, or written in memory. The contents of instruction placed in this register are transferred to the Instruction Register, while the contents of data are transferred to the accumulator or I/O register.

In other words you can say that this register is used to store data/instruction coming from the memory or going to the memory.

I/O Address Register (I/O AR):\
I/O Address register is used to specify the address of a particular I/O device.

I/O Buffer Register (I/O I3R):

I/O Buffer Register is used for exchanging data between the I/O module and the processor.

Program Counter (PC)

Program Counter register is also known as Instruction Pointer Register. This register is used to store the address of the next instruction to be fetched for execution. When the instruction is fetched, the value of IP is incremented. Thus this register always points or holds the address of next instruction to be fetched.

Instruction Register (IR):

Once an instruction is fetched from main memory, it is stored in the Instruction Register. The control unit takes instruction from this register, decodes and executes it by sending signals to the appropriate component of computer to carry out the task.

Accumulator Register:

The accumulator register is located inside the ALU, It is used during arithmetic & logical operations of ALU. The control unit stores data values fetched from main memory in the accumulator for arithmetic or logical operation. This register holds the initial data to be operated upon, the intermediate results, and the final result of operation. The final result is transferred to main memory through MBR.

Stack Control Register:

A stack represents a set of memory blocks; the data is stored in and retrieved from these blocks in an order, i.e. First In and Last Out (FILO). The Stack Control Register is used to manage the stacks in memory. The size of this register is 2 or 4 bytes.

Flag Register:

The Flag register is used to indicate occurrence of a certain condition during an operation of the CPU. It is a special purpose register with size one byte or two bytes. Each bit of the flag register constitutes a flag (or alarm), such that the bit value indicates if a specified condition was encountered while executing an instruction.

### What is IOWait?

IOWait is a CPU metric, measuring the percent of time the CPU is idle, but waiting for an I/O to complete. Strangely - It is possible to have healthy system with nearly 100% iowait, or have a disk bottleneck with 0% iowait.

This usually means that the block devices (i.e. physical disks, not memory) is too slow, or simply saturated.

You should hence note that if you see a high load average on your system, and on inspection notice that most of this is actually due to I/O wait, it does not necessarily mean that your system is in trouble - and this occurs when your machine simply has nothing to do, other than than I/O-bound processes (i.e. processes that do more I/O than anything else (non-I/O-bound system calls)). That should also be apparent from the fact that anything you do on the system is still very responsive.

tools:

sar (from the sysstat package, available on most \*nix machines)

iostat

sarface (a front-end to sar)

### What happens when you type in shelllist=$(ls)

```
Some highlights:
- You will see your strace command running the bash executable via the exec syscall


execve("/bin/bash")
- The bash process will read the text file and interpret the commands.
- This bash process will create a pipe to read output from the subshell that $(command) creates


pipe([3,4])
.
- The bash proces will call fork() (clone in linux) to create a subprocess for the subshell


clone(...)
- Parent process does a blocking read on the first file descriptor of the pipe (waiting to read output from the subshell)


read(3,  <unfinished ...>
- The new child process will use the second fd in the pipe for it's new stdout


dup2(4, 1)
- That new child pid will look for ls in the system path, then call exec() to run ls in the subshell process


execve("/bin/ls", ["ls"], [/* 22 vars */]) = 0
- The ls program uses open() to open the current directory, then readdir() / getdents() to read the directory entries:


26849 openat(AT_FDCWD, ".", O_RDONLY|O_NONBLOCK|O_DIRECTORY|O_CLOEXEC) = 3
26849 getdents(3, /* 13 entries */, 32768) = 416
26849 getdents(3, /* 0 entries */, 32768) = 0
- Next, the ls command writes to stdout


write(1, "file1"..., 128) = 128
- This wakes up the parent process blocked on the pipe (since child fd 1 is stdout, which was dup2'd to the second pipe fd 4)
26848 <... read resumed> "file1"..., 128) = 128
- The parent process waits for the child to exit, in case it hasn't already


wait4(-1, 0x7fff2eeb2398, WNOHANG, NULL)
```

### During boot, after the BIOS performs a successful power-on-self-test, describe everything that occurs until the console is presented to the user.

PCI device tree is formed

BIOS loads the MBR

MBR points to the kernel image stored which is then loaded

Kernel boots with one process with pid 1

Kernel imports the PCI device tree and based on PCI IDs loads the corresponding modules.

Now the kernel is able to do DMA by itself (either over disk or network)

After this comes the init levels which I am not really sure about. Here's a skeletal idea.

The first level is single user mode

Then a multi-user mode and a shell is spawned.

Then X is launched if configured.

Order of speed between: read CPU register, Disk Seek, Context Switch and Read from memory

## Filesystem

### List open file handles

lsof -p process-id

Or ls /proc/process-id/fd

### What is an inode?

An inode is a data structure in Unix that contains metadata about a file. Some of the items contained in an inode are:

1\) mode

2\) owner (UID, GID)

3\) size

4\) atime, ctime, mtime

5\) acl’s

6\) blocks list of where the data is

The filename is present in the parent directory’s inode structure. The file name is stored in the respective directory ("directory file"). This entry points to an inode.

### Difference between a hard and soft link

![](https://i.stack.imgur.com/f7Ijz.jpg)

Underneath the file system files are represented by inodes (or is it multiple inodes not sure)

A file in the file system is basically a link to an inode.\
A hard link then just creates another file with a link to the same underlying inode.

When you delete a file it removes one link to the underlying inode. The inode is only deleted (or deletable/over-writable) when all links to the inode have been deleted.

A symbolic link is a link to another name in the file system.

Once a hard link has been made the link is to the inode. deleting renaming or moving the original file will not affect the hard link as it links to the underlying inode. Any changes to the data on the inode is reflected in all files that refer to that inode.

Note: Hard links are only valid within the same File System. Symbolic links can span file systems as they are simply the name of another file.

## Signals

### What is the default kill signal?

The default signal is TERM which allows the program being killed to catch it and do some cleanup before exiting. A program can ignore it, too, if it's written that way.

Specifying -9 or KILL as the signal does not allow the program to catch it, do any cleanup or ignore it. It should only be used as a last resort.

### When you send a HUP signal to a process, you notice that it has no impact, what could have happened?

During critical section execution, some processes can setup signal blocking. The system call to mask signals is ‘sigprocmask’. When the kernel raises a blocked signal, it is not delivered. Such signals are called pending. When a pending signal is unblocked, the kernel passes it off to the process to handle. It is possible that the process was masking SIGHUP.
