# Debugging Java

## Debugging

Use `jps`and `jstac`to learn more about Java processes.`jstat`can also be useful for Java statistics monitoring.

jvmtop.sh --profile \<PID>

cd /opt/www/java/jdk/bin/

\# jmap -heap \<JAVA\_PID>

\# java -XX:+PrintFlagsFinal -version | grep -iE 'HeapSize|PermSize|ThreadStackSize'

[https://blogs.sap.com/2014/04/02/using-jvmtop-and-jvmmon-to-diagnose-tomcat-cpu-bottlenecks/](https://blogs.sap.com/2014/04/02/using-jvmtop-and-jvmmon-to-diagnose-tomcat-cpu-bottlenecks/)

## **Thread StackTrace**

In \*nix, with`top`by pressing`H`you can see the threads.

Then with`jps`you can see the`pid`bear in mind that if the process was started with privileges then you must execute it with`sudo`for instance.

If you take the thread id and converted it to hexadecimal then you can cross that data with the`jstack pid`output.

Both tools are in`$JAVA_HOME/bin`.

## **Explain memory management in java.**

Ans. In java, memory is managed via garbage collector. Few techniques for memory management are:

**1. Reference Counting:**

A count of references to each object is maintained. When garbage collector runs, it deletes objects with zero reference count.

**Drawback:**

Circular references are maintained in memory.

**2. Tracing collectors/Copy Collector/Stop and copy collector:**

Start from a root object and keep a track of all references which have direct/indirect reference to the root object. Then all the live objects are moved to another heap, taking care of references properly.

**Drawback:**

At each point of time, you will have 2 heaps thus consuming twice the memory.

**3. Mark sweep collectors/Stop and work collector:**

Similar to tracing collector except that instead of copying the references to the new heap, they are swept out of memory, after a list of live and dead objects is known.

Mark and sweep is a stop-the-world garbage collection technique; that is all application threads stop until garbage collection completes or until a higher-priority thread interrupts the garbage collector. If the garbage collector is interrupted it must restart which can lead to application thrashing with little apparent result.
