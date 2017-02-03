# SecureCodingInJava

1. Thread-Safety issues
  - Background threads during class initialization
  - Object construction (Reference escape)
  - Don't override thread-safe methods with methods that aren't thread-safe.
  - Partially initialized objects.

2. Numeric Types and Operations
   - Convert integers to floating point
   - Be aware of numeric promotoin behaviour
   - Do not use denormalized numbers
   - Use the stricftp
  
3. Locking
   - Do not synchronize on the class object returned by getClass()
   - Ensure actively held locks are released on exceptional conditions
   - Use a correct form of the double-checked locking idiom
   - Avoid client-side locking when using classes that do not commit to their locking strategy.
  
4. Visibility and Atomicity (VNA)
  - Ensure visibility when accessing shared primitive variables.
  - Ensure that calls to chained methods are atomic.
  - Ensure visibility of shared references to immutable objects.
  
SPRING SECURITY ISSUES:

Overview:

1. AbstractSecurityInterceptor uses AuthenticaionManager
2. FilterSecurityInterceptor secures FilterInvocation
3. MethodSecurityInterceptor secures MethodInvocation
4. AccessDecisionManager 

Defensive programming:

1. Minimize the scope of your variables:

Scope minimization helps developers avoid common programming errors, improves code readability by connecting the declaration and actual use of a variable, and improves maintainability because unused variables are more easily detected and removed.

2. Write  garbage collection friendly code.
3. Try to gracefully recover from system errors.
4. Use conservative file naming conventions.

Design classes and methods for inheritance or declare them final [6]. Left non-final, a class or method can be maliciously overridden by an attacker. A class that does not permit subclassing is easier to implement and verify that it is secure. Prefer composition to inheritance.

Thread API's:

Each single thread in Java is assigned to a thread group upon the thread's creation. These groups are implemented by the java.lang.ThreadGroup class. When the thread group name is not specified explicitly, the main default group is assigned by the Java Virtual Machine (JVM) [Java Tutorials]. The convenience methods of the ThreadGroup class can be used to operate on all threads belonging to a thread group at once. For instance, the ThreadGroup.interrupt() method interrupts all threads in the thread group. Thread groups also help reinforce layered security by confining threads into groups so that they avoid interference with threads in other groups.

DateTime API and Threads... Will Java 8 change this issue ?

Well, let's say Java 8 is a really big approach into the right direction.

Use of the Execute Work-Around-Pattern

One scenario to use the Execute Work-Around-Pattern is for I/O operations.

You can close I/O streams with this pattern.

Handling null errors with Java 8 and the Optional class.

Optional.isEmpty();

Can you use Optional class also with primtive datatypes?

Avoid Large Objects

The allocation of large objects is expensive, in part because the cost to initialize their fields is proportional to their size. Additionally, frequent allocation of large objects of different sizes can cause fragmentation issues or compacting collect operations.

Do Not Explicitly Invoke the Garbage Collector

The GC can be explicitly invoked by calling the System.gc() method. Even though the documentation says that it "runs the garbage collector," there is no guarantee as to when or whether the GC will actually run. In fact, the call merely suggests that the GC should subsequently execute; the JVM is free to ignore this suggestion.
Irresponsible use of this feature can severely degrade system performance by triggering garbage collection at inopportune moments rather than waiting until ripe periods when it is safe to garbage-collect without significant interruption of the program's execution.
In the Java Hotspot VM (default since JDK 1.2), System.gc() forces an explicit garbage collection. Such calls can be buried deep within libraries, so they may be difficult to trace. To ignore the call in such cases, use the flag -XX:+DisableExplicitGC. To avoid long pauses while performing a full garbage collection, a less demanding concurrent cycle may be invoked by specifying the flag -XX:ExplicitGCInvokedConcurrent.

What is a Memory Leak?
======================

Imagine you have an object A and an object B.

A extends B.

Lifetime of B is over.

But A is still alive and references B.

This are the ingredients for a Memory leak.

Strings and Memory leaks.

Don't do this:

String myName = new String("Helmut");

You should do it like this:

String myName = "Helmut";

Now, no new String object is created.

And you don't can create a memory leak.

Static polymorphism and security.

















   
