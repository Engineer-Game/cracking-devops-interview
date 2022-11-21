# Virtualization

## W**hat are the different types of virtualization available?**

Different types of virtualization available are

* Application virtualization
* Presentation virtualization
* Network virtualization
* Storage virtualization

### **Explain what is hypervisor?**

Hypervisor is a program that enables multiple operating systems to share a single hardware host. Each operating system has the host’s processor, memory and other resources all to itself. Hypervisor controls the resources and host processor, allocating what is required for each operating system in turn and make sure that the guest operating system cannot disrupt each other.

### Type-1, native or bare-metal hypervisors

These hypervisors run directly on the host's hardware to control the hardware and to manage guest operating systems. For this reason, they are sometimes called bare metal hypervisors. The first hypervisors, which IBM developed in the 1960s, were native hypervisors.\[4] These included the test software SIMMON and the CP/CMS operating system (the predecessor of IBM's z/VM). Modern equivalents include Xen, Oracle VM Server for SPARC, Oracle VM Server for x86, the Citrix XenServer, Microsoft Hyper-V and VMware ESX/ESXi.

### Type-2 or hosted hypervisors

These hypervisors run on a conventional operating system (OS) just as other computer programs do. A guest operating system runs as a process on the host. Type-2 hypervisors abstract guest operating systems from the host operating system. VMware Workstation, VMware Player, VirtualBox, Parallels Desktop for Mac and QEMU are examples of type-2 hypervisors.
