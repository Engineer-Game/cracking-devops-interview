# Containers

### What are kernel namespaces?

Linux namespaces were inspired by the more general namespace functionality used heavily throughout Plan 9 from Bell Labs.\[1]

The Linux Namespaces originated in 2002 in the 2.4.19 kernel with work on the mount namespace kind. Additional namespaces were added beginning in 2006\[2] and continuing into the future.

Adequate containers support functionality was finished in kernel version 3.8 with the introduction of User namespaces.

Namespace kinds

As of kernel version 3.8, there are 6 kinds of namespaces. Namespace functionality is the same across all kinds: each process is associated with a namespace and can only see or use the resources associated with that namespace, and descendant namespaces where applicable. This way each process (or group thereof) can have a unique view on the resource. Which resource is isolated depends on the kind of the namespace has been created for a given process group.

Mount (mnt)

Mount namespaces control mount points. Upon creation the mounts from the current mount namespace are copied to the new namespace, but mount points created afterwards do not propagate between namespaces (using shared subtrees, it is possible to propagate mount points between namespaces\[3]).

The clone flag CLONE\_NEWNS - short for "NEW NameSpace" - was used because the mount namespace kind was the first to be introduced. At the time nobody thought of other namespaces but the name has stuck for backwards compatibility.

Process ID (pid)

The PID namespace provides processes with an independent set of process IDs (PIDs) from other namespaces. PID namespaces are nested, meaning when a new process is created it will have a PID for each namespace from its current namespace up to the initial PID namespace. Hence the initial PID namespace is able to see all processes, albeit with different PIDs than other namespaces will see processes with.

The first process created in a PID namespace is assigned the process id number 1 and receives most of the same special treatment as the normal init process, most notably that orphaned processes within the namespace are attached to it. This also means that the termination of this PID 1 process will immediately terminate all processes in its PID namespace and any descendants.\[4]

Network (net)

Network namespaces virtualize the network stack. On creation a network namespace contains only a loopback interface.

Each network interface (physical or virtual) is present in exactly 1 namespace and can be moved between namespaces.

Each namespace will have a private set of IP addresses, its own routing table, socket listing, connection tracking table, firewall, and other network-related resources.

On its destruction, a network namespace will destroy any virtual interfaces within it and move any physical interfaces back to the initial network namespace.

Interprocess Communication (ipc)\[edit]

IPC namespaces isolate processes from SysV style inter-process communication. This prevents processes in different IPC namespaces from using, for example, the SHM family of functions to establish a range of shared memory between the two processes. Instead each process will be able to use the same identifiers for a shared memory region and produce two such distinct regions.

UTS

UTS namespaces allow a single system to appear to have different host and domain names to different processes.

User ID (user)

User namespaces are a feature to provide both privilege isolation and user identification segregation across multiple sets of processes. With administrative assistance it is possible to build a container with seeming administrative rights without actually giving elevated privileges to user processes. Like the PID namespace, user namespaces are nested and each new user namespace is considered to be a child of the user namespace that created it.

A user namespace contains a mapping table converting user IDs from the container's point of view to the system's point of view. This allows, for example, the root user to have user id 0 in the container but is actually treated as user id 1,400,000 by the system for ownership checks. A similar table is used for group id mappings and ownership checks.

To facilitate privilege isolation of administrative actions, each namespace type is considered owned by a user namespace based on the active user namespace at the moment of creation. A user with administrative privileges in the appropriate user namespace will be allowed to perform administrative actions within that other namespace type. For example, if a process has administrative permission to change the IP address of a network interface, it may do so as long as the applicable network namespace is owned by a user namespace that either matches or is a child (direct or indirect) of the process' user namespace. Hence the initial user namespace has administrative control over all namespace types in the system.\[5]

cgroup namespace

To prevent leaking the control group to which a process belongs, a new namespace type has been suggested\[6] and created to hide the actual control group a process is a member of. A process in such a namespace checking which control group any process is part of would see a path that is actually relative to the control group set at creation time, hiding its true control group position and identity.

[https://www.toptal.com/linux/separation-anxiety-isolating-your-system-with-linux-namespaces](https://www.toptal.com/linux/separation-anxiety-isolating-your-system-with-linux-namespaces)

### What is Docker?

Docker provides an additional layer of abstraction and automation of operating-system-level virtualization on Linux. Docker uses the resource isolation features of the Linux kernel such as cgroups and kernel namespaces, and a union-capable file system such as OverlayFS and others to allow independent "containers" to run within a single Linux instance, avoiding the overhead of starting and maintaining virtual machines.

### Differences with virtualization

Docker originally used LinuX Containers (LXC), but later switched to runC (formerly known as libcontainer), which runs in the same operating system as its host. This allows it to share a lot of the host operating system resources. Also, it uses a layered filesystem (AuFS) and manages networking.

AuFS is a layered file system, so you can have a read only part and a write part which are merged together. One could have the common parts of the operating system as read only (and shared amongst all of your containers) and then give each container its own mount for writing.

So, let's say you have a 1GB container image; if you wanted to use a Full VM, you would need to have 1GB times x number of VMs you want. With docker and AuFS you can share the bulk of the 1GB between all the containers and if you have 1000 containers you still might only have a little over 1GB of space for the containers OS (assuming they are all running the same OS image).

A full virtualized system gets its own set of resources allocated to it, and does minimal sharing. You get more isolation, but it is much heavier (requires more resources). With docker you get less isolation, but the containers are lightweight (require fewer resources). So you could easily run thousands of containers on a host, and it won't even blink. Try doing that with Xen, and unless you have a really big host, I don't think it is possible.

A full virtualized system usually takes minutes to start, whereas docker/LXC/runC containers take seconds, and often even less than a second.

### Security concerns around Docker

* Kernel Exploits
* Denial of Service Attacks
* Container Breakouts
* Poisoned Images
* Compromising Secrets

[https://github.com/docker/docker-bench-security](https://github.com/docker/docker-bench-security)

[https://www.oreilly.com/ideas/five-security-concerns-when-using-docker](https://www.oreilly.com/ideas/five-security-concerns-when-using-docker)

## Daemonset Vs Sidecar

A \_DaemonSet \_ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

In a sidecar pattern, the functionality of the main container is extended or enhanced by a sidecar container without strong coupling between two. This pattern is particularly useful when using Kubernetes as container orchestration platform. Kubernetes uses Pods. A Pod is composed of one or more application containers. A sidecar is a \_utility \_container in the Pod and its purpose is to support the main container.

## Deployments

Deployment is the easiest and most used resource for deploying your application. It is a Kubernetes controller that matches the current state of your cluster to the desired state mentioned in the Deployment manifest. e.g. If you create a deployment with 1 replica, it will check that the desired state of ReplicaSet is 1 and current state is 0, so it will create a ReplicaSet, which will further create the pod. If you create a deployment with name **counter**, it will create a ReplicaSet with name **counter-\<replica-set-id>**, which will further create a Pod with name **counter-\<replica-set->-\<pod-id>.**

## **Statefulsets**

StatefulSet(stable-GA in k8s v1.9) is a Kubernetes resource used to manage stateful applications. It manages the deployment and scaling of a set of Pods, and provides guarantee about the ordering and uniqueness of these Pods.

StatefulSets are useful in case of Databases especially when we need Highly Available Databases in production as we create a cluster of Database replicas with one being the primary replica and others being the secondary replicas. The primary will be responsible for read/write operations and secondary for read only operations and they will be syncing data with the primary one.

StatefulSets don’t create ReplicaSet or anything of that sort, so you cant rollback a StatefulSet to a previous version.

## Replicasets

A ReplicaSet ensures that a specified number of pod replicas are running at any given time. However, a Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features. Therefore, we recommend using Deployments instead of directly using ReplicaSets, unless you require custom update orchestration or don't require updates at all.

This actually means that you may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.



