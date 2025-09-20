# Java Memory Management

Java code runs in a Java Virtual Machine (JVM). The JVM takes care of the memory management in Java so we developers
don't have to.

## Memory allocation

3 main memory "areas" from a Java perspective: Stack, Heap, Metaspace.

Stack is for storing classes that are needed for the application, methods that have been called, primitive types and
references to objects in the heap, ...and probably some other things I'm not aware of.

Heap is for storing object values, ...and probably some other things I'm not aware of.

Metaspace is for storing metadata - which are JVM structures created whilst parsing Java `.class` files - like: Internal
representation of Java classes, Methods with their bytecode, Field descriptors, Constant pools, Symbols, and
Annotations.

## Types in memory

Primitive types are the types with lower case letters: `byte`, `short`, `int`, `long`, `float`, `double`, `boolean`,
`char`. They value of these are stored directly in the Stack.

Reference types are the types with upper case letters: `String`, `Double`, `HashMap`, etc. The value of these are store
in the Heap, and a "reference" is created in the Stack which points to the location in the Heap where the value is
stored.

Reference types are kind of like [pointers](https://en.wikipedia.org/wiki/Pointer_(computer_programming)) in that way,
but unlike pointers like you might see in C or C++, these references are more just handlers for the object rather than
actual pointers that you can manipulate. After garbage collection, the Heap memory location can change, and the
references in the Stack are updated.

## Garbage collection

Java has garbage collection on objects in Heap memory, meaning deallocation of memory is not done manually by developers
or automatically by the JVM as code is dereferenced. Instead, objects in Heap memory that are no longer referenced in
Stack memory are periodically cleaned up by a garbage collector.

A generational garbage collector is one that organises objects into different spaces or "generations" depending on the
objects age, size and other factors.

Different garbage collectors are available:

- G1 or "G1GC" or [Garbage First](https://en.wikipedia.org/wiki/Garbage-first_collector)
	- Introduced as experimental in JVM 6 Update 14, supported from JVM 7, defaulted in Java 9.
	- Designed to be a compacting and more predicatable garbage collector than the previous CMS collector.
	- Is a generational garbage collector.
- ZGC or [The Z Garbage Collector](https://openjdk.org/projects/zgc/)
	- Introduced in experimental in JDK 11 and production ready in JDK 15.
	- Reimplemented to support "generations" in JDK 21, which defaulted in JDK 23, and the non-generational mode was
	  removed in JDK 24.
	- Designed to be a scalable low latency garbage collector by being concurrent.
- and a bunch of others.

## Memory according to metrics

If you're running a Java Spring application and gathering metrics about the JVM, you'll probably see metrics about
memory.

If you run other things on the JVM as well as your application, like Java Agents, the memory those processes use will be
included in these metrics, as these metrics are for the _entire_ JVM.

There are 3 main kinds of JVM memory metric:

- `jvm_memory_used_bytes`
	- The number of bytes in memory in _actual_ use, according to the JVM.
	- It will go up and down as memory is allocated, deallocated and garbage collected.
- `jvm_memory_committed_bytes`
	- The number of bytes that the JVM has actually allocated for itself (from the Operating System) for memory.
	- Committed will go up and down as the memory is allocated and the JVM realises it needs to allocate more memory for
	  use by the processes running in the JVM.
- `jvm_memory_max_bytes`
	- The number of bytes that the JVM is allowed to use (by the Operating System) for memory.
	- This should be probably be fixed when the JVM boots up and won't change whilst the JVM is running.

Used < Committed < Max.

Different "spaces" are mentioned in these metrics:

- `G1 Eden Space`
	- Seen if using the G1 Garbage Collector.
	- Space is in "Heap" memory.
	- This memory space is for objects when they are initially created.
- `G1 Survivor Space`
	- Seen if using the G1 Garbage Collector.
	- Space is in "Heap" memory.
	- This memory space is for objects that have "survived" garbage collection of the Eden Space.
- `G1 Old Gen`
	- Seen if using the G1 Garbage Collector.
	- Space is in "Heap" memory.
	- This memory space is for objects that have existed for a long time in the Survivor Space.
- `ZGC Young Generation`
	- Seen if using the Z Garbage Collector.
	- Space is in "Heap" memory.
	- This memory space is for newly created objects and ones that have survived 1 or some garbage collection cycles.
- `ZGC Old Generation`
	- Seen if using the Z Garbage Collector.
	- Space is in "Heap" memory.
	- This memory space is for objects that have survived multiple garbage collection cycles.
- `Metaspace`
	- Space is in Metaspace ("Non-Heap") memory.
	- Used for storing metaspace data (see above).
	- Used to be called "PermGen" or "Permanent Generation".
	- Not garbage-collected.
- `Compressed Class Space`
	- Space is in Metaspace ("Non-Heap") memory.
	- Technically, it is a space _inside_ the `Metaspace` and so will always be smaller than `Metaspace`.
	- It stores specifically class-part metadata using 32-bit references.
	- It's size (`CompressedClassSpaceSize`) is 1 Gi by default (at least, in Hotspot it is), and is not affected by
	  flags configure max heap size, since it's not part of the Heap.
- `CodeCache`
	- Space is in "Non-Heap" memory.
	- Used for compilation and storage of native code.
	- Existed until Java 9, when it was replaced with [segmented Code Heaps](https://openjdk.org/jeps/197) (see below).
- `CodeHeap`
	- Space is in "Non-Heap" memory.
	- Used for compilation and storage of native code.
	- Introduced in Java 9 as [segmented code heaps](https://openjdk.org/jeps/197), in the following segments:
		- `non-nmethods`
		- `profiled-nmethods`
		- `non-profiled-nmethods`

## Configuring the JVM for memory

Different flags are available to configure memory usage for the JVMs. Different JVMs (Hotspot, JRockit, etc.) might have
different flags:

- [Hotspot JVM options](https://www.oracle.com/java/technologies/javase/vmoptions-jsp.html)
- [JRockit JVM -X options](https://docs.oracle.com/cd/E13150_01/jrockit_jvm/jrockit/jrdocs/refman/optionX.html)
- [JRockit JVM -XX options](https://docs.oracle.com/cd/E13150_01/jrockit_jvm/jrockit/jrdocs/refman/optionXX.html)

Use the `JAVA_TOOL_OPTIONS` environment variable to set these. Set these at deployment time (e.g. in deployment
manifests / patches) according to the needs of the environment you're deploying to (prod, test, dev, local).

- `-Xss`
	- Sets thread Stack size
	- e.g. `-Xss1m` sets it to 1 MB.
- `-Xms`
	- Set _initial_ and _minimum_ Heap memory size
	- e.g. `-Xms512m` sets it to 512 MB.
- `-Xmx`
	- Set _max_ Heap memory size
	- e.g. `-Xms512m` sets it to 512 MB.
	- When Spring JVM metrics are observed, and when setting this when `G1GC` is in use, it only sets the max Heap size
	  for the `Old Gen` Heap space. `Eden Space` and `Survivor Space` are left at `-1` (assuming this menas unlimited).
	  I assume this is because Eden and Survivor spaces are garbage collected and cleaned quite often and quickly and so
	  objects are usually garbage-collected or find themselves quickly in `Old Gen`.
	- When Spring JVM metrics are observed, and when setting this when `ZGC` is in use, it only sets the max Heap size
	  for the `Old Generation` Heap space **and** the `Young Generation` heap space. No other metrics seem affected.
	  I assume this is because in ZGC, there it seems to be only 2 generations (in G1GC there seems to be 3) and new
	  objects in the `Young Generation` that survive first garbage collection are left there until multiple garbage
	  collection cycles have passed before moving into the `Old Generation` Heap space - and so both heap spaces could
	  grow to a significant size.
- `-XX:InitialRAMPercentage`
	- Sets _initial_ Heap memory size to be a percentage of the total machine (or
	  container) memory.
	- e.g. `-XX:InitialRAMPercentage=50.0` sets it to 50%.
- `-XX:MinRAMPercentage`
	- Sets _minimum_ Heap memory size to be a percentage of the total machine (or
	  container) memory.
	- e.g. `-XX:MinRAMPercentage=10.0` sets it to 10%.
- `-XX:MaxRAMPercentage`
	- Sets _max_ Heap memory size to be a percentage of the total machine (or
	  container) memory.
	- e.g. `-XX:MaxRAMPercentage=50.0` sets it to 50%.
	- When Spring JVM metrics are observed, and when setting this when `G1GC` is in use, it only sets the max Heap size
	  for the `Old Gen` Heap space. `Eden Space` and `Survivor Space` are left at `-1` (assuming this menas unlimited).
	  I assume this is because Eden and Survivor spaces are garbage collected and cleaned quite often and quickly and so
	  objects are usually garbage-collected or find themselves quickly in `Old Gen`.
	- When Spring JVM metrics are observed, and when setting this when `ZGC` is in use, it only sets the max Heap size
	  for the `Old Generation` Heap space **and** the `Young Generation` heap space. No other metrics seem affected.
	  I assume this is because in ZGC, there it seems to be only 2 generations (in G1GC there seems to be 3) and new
	  objects in the `Young Generation` that survive first garbage collection are left there until multiple garbage
	  collection cycles have passed before moving into the `Old Generation` Heap space - and so both heap spaces could
	  grow to a significant size.
- `-XX:+UseG1GC`
	- Enables using G1 Garbage Collector.
- `-XX:+UseZGC`
	- Enabled using the Z Garbage Collector.
- `-XX:MaxMetaspaceSize`
	- Sets the max size for the Metaspace.
	- e.g. `-XX:MaxMetaspaceSize=512m` sets it to 512 MiB.
	- You might see that `MaxMetaspaceSize` is `18446744073709551615`, but metrics says `-1` - this is because those two
	  numbers are effectively the same (think unsigned and signed numbers and overflows).
- `-XX:CompressedClassSpaceSize`
	- Sets the max size for the Compressed Class Space.
	- e.g. `-XX:CompressedClassSpaceSize=512m` sets it to 512 MiB.
	- In Hotspot (at least) this is defaulted to 1 Gi.

Find out all the flags you can set doing:

```bash
java -XX:+PrintFlagsFinal --version
```
