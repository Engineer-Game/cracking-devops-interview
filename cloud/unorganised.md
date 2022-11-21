# Others

swap

phiscal vs virtual memory

threads and process

swap

Join Venture - AWS - VMWARE

Vshphere on top AWS - SDSC - Amazon Cloud

Install ESX - Build in cluster - VPC - Manage that - SOld customer with Vmware - New in October - 50

Group SRE / Devops team - Remedion / Troubleshooting - Palo Alto - Austin (same part of project)

Sitting the infrastructure - tools using Python and Java for deelopment - Flask , nagios event handling , rabbitmq, mariadb (

lifecycle code - linux server -

Deploy ??? SRE not deployment - Configuration Management - Update and patching - Monitoring

Pupept and Ansible

We handle the install vshpere - 16 tTB - E tree -Itrace -

we are not GA - customer install in a couple months - Agile - Runbooks - CI/CD SDDC - issues with customers - automated -

Service Continuity - Monitoring - SRE (third level)

Thousands of datacenters -

```
                             Linux Device Model \(LDM\)
```

Explain about the Linux Device Model (LDM)?

Explain about about ksets, kobjects and ktypes. How are they related?

Questions about sysfs.

```
                                   Linux Boot Sequence
```

Explain about the Linux boot sequence in case of ARM architecture?

How are the command line arguments passed to Linux kernel by the u-boot (bootloader)?

Explain about ATAGS?

Explain about command line arguments that are passed to linux kernel and how/where they are parsed in kernel code?

Explain about device tree.

```
                                  Interrupts in Linux
```

Explain about the interrupt mechanims in linux?

What are the APIs that are used to register an interrupt handler?

How do you register an interrupt handler on a shared IRQ line?

Explain about the flags that are passed to request\_irq().

Explain about the internals of Interrupt handling in case of Linux running on ARM.

What are the precautions to be taken while writing an interrupt handler?

Explain interrupt sequence in detail starting from ARM to registered interrupt handler.

What is bottom half and top half.

What is request\_threaded\_irq()

If same interrupts occurs in two cpu how are they handled?

How to synchronize data between 'two interrupts' and 'interrupts and process'.

How are nested interrupts handled?

How is task context saved during interrupt.

```
                                Bottom-half Mechanisms in Linux
```

What are the different bottom-half mechanisms in Linux?

Softirq, Tasklet and Workqueues

What are the differences between Softirq/Tasklet and Workqueue? Given an example what you prefer to use?

What are the differences between softirqs and tasklets?

Softirq is guaranteed to run on the CPU it was scheduled on, where as tasklets don’t have that guarantee.

The same tasklet can't run on two separate CPUs at the same time, where as a softirq can.

When are these bottom halfs executed?

Explain about the internal implementation of softirqs?

[Bottom-halves in Linux - Part 1: Softirqs](http://linuxblore.blogspot.com/2013/02/bottom-halves-in-linux-part-1-softirqs.html)

Explain about the internal implementation of tasklets?

[Bottom-halves in Linux - Part 2: Tasklets](http://linuxblore.blogspot.com/2013/02/bottom-halves-in-linux-part-2-tasklets.html)

Explain about the internal implementation of workqueues?

[Bottom-halves in Linux - Part 3: Workqueues](http://linuxblore.blogspot.in/2013/01/workqueues-in-linux.html)

Explain about the concurrent work queues.

```
                                   Linux Memory Management
```

What are the differences between vmalloc and kmalloc? Which is preferred to use in device drivers?

What are the differences between slab allocator and slub allocator?

What is boot memory allocator?

How do you reserve block of memory?

What is virtual memory and what are the advanatages of using virtual memory?

What's paging and swapping?

Is it better to enable swapping in embedded systems? and why?

What is the page size in Linux kernel in case of 32-bit ARM architecture?

What is page frame?

What are the different memory zones and why does different zones exist?

What is high memory and when is it needed?

Why is high memory zone not needed in case of 64-bit machine?

How to allocate a page frame from high memory?

In ARM, an abort exception if generated, if the page table doesn't contain a virtual to physical map for a particular page. How exactly does the MMU know that a virtual to physical map is present in the pagetable or not?

A Level-1 page table entry can be one of four possible types. The 1st type is given below:

A fault entry that generates an abort exception. This can be either a prefetch or data abort, depending on the type of access. This effectively indicates virtual addresses that are unmapped.

In this case the bit \[0] and \[1] are set to 0. This is how the MMU identifies that it's a fault entry.

Same is the case with Level-2 page table entry.

Does the Translation Table Base Address (TTBR) register, Level 1 page table and Level 2 page table contain Physical addresses or Virtual addresses?

TTBR: Contain physical address of the pgd base

Level 1 page table (pgd): Physical address pointing to the pte base

Level 2 page table (pte): Physical address pointing to the physical page frame

Since page tables are in kernel space and kernel virtual memory is mapped directly to RAM. Using just an easy macro like \_\_virt\_to\_phys(), we can get the physical address for the pgd base or pte base or pte entry.

Kernel Synchronization

Why do we need synchronization mechanisms in Linux kernel?

What are the different synchonization mechanisms present in Linux kernel?

What are the differences between spinlock and mutex?

What is lockdep?

Which synchronization mechanism is safe to use in interrupt context and why?

Explain about the implementation of spinlock in case of ARM architecture.

Explain about the implementation of mutex in case of ARM architecture.

Explain about the notifier chains.

Explain about RCU locks and when are they used?

Explain about RW spinlocks locks and when are they used?

Which are the synchronization technoques you use 'between processes', 'between processe and interrupt' and 'between interrupts'; why and how ?

What are the differences between semaphores and spinlocks?

```
                        Process Management and Process Scheduling
```

What are the different schedulers class present in the linux kernel?

How to create a new process?

What is the difference between fork( ) and vfork( )?

Which is the first task what is spawned in linux kernel?

What are the processes with PID 0 and PID 1?

PID 0 - idle task

PID 1 - init

How to extract task\_struct of a particular process if the stack pointer is given?

How does scheduler picks particular task?

When does scheduler picks a task?

How is timeout managed?

How does load balancing happens?

Explain about any scheduler class?

Explain about wait queues and how they implemented? Where and how are they used?

What is process kernel stack and process user stack? What is the size of each and how are they allocated?

Why do we need seperate kernel stack for each process?

What all happens during context switch?

What is thread\_info? Why is it stored at the end of kernel stack?

What is the use of preempt\_count variable?

What is the difference between interruptible and uninterruptible task states?

How processes and threads are created? (from user level till kernel level)

How is virtual run time (vruntime) calculated?

```
                                  Timers and Time Management
```

What are jiffies and HZ?

What is the initial value of jiffies when the system has started?

Explain about HR timers and normal timers?

On what hardware timers, does the HR timers are based on?

How to declare that a specific hardware timers is used for kernel periodic timer interrupt used by the scheduler?

How software timers are implemented?

```
                                   Power Management in Linux
```

Explain about cpuidle framework.

Explain about cpufreq framework.

Explain about clock framework.

Explain about regulator framework.

Explain about suspened and resume framwork.

Explain about early suspend and late resume.

Explain about wakelocks.

```
                                      Linux Kernel Modules
```

How to make a module as loadable module?

How to make a module as in-built module?

Explain about Kconfig build system?

Explain about the init call mechanism.

What is the difference between early init and late init?

Early init:

Early init functions are called when only the boot processor is online.

Run before initializing SMP.

Only for built-in code, not modules.

Late init:

Late init functions are called \_after\_ all the CPUs are online.

```
                                     Linux Kernel Debugging
```

What is Oops and kernel panic?

Does all Oops result in kernel panic?

What are the tools that you have used for debugging the Linux kernel?

What are the log levels in printk?

Can printk's be used in interrupt context?

How to print a stack trace from a particular function?

What's the use of early\_printk( )?

Explan about the various gdb commands.

```
                                              Miscellaneous
```

How are the atomic functions implemented in case of ARM architecture?

How is container\_of( ) macro implemented?

Explain about system call flow in case of ARM Linux.

What 's the use of \_\_init and \_\_exit macros?

How to ensure that init function of a partiuclar driver was called before our driver's init function is called (assume that both these drivers are built into the kenrel image)?

What's a segementation fault and what are the scenarios in which segmentation fault is triggered?

If the scenarios which triggers the segmentation fault has occurred, how the kernel identifies it and what are the actions that the kernel takes?
