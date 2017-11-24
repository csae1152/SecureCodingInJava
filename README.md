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

Java 8 Streams always create an new object.

It's very important for working with streams.

Mixing generically typed code with raw typed code is one common source of heap pollution. Generic types were unavailable prior to Java 5, so popular interfaces such as the Java Collection Framework relied on raw types. Mixing generically typed code with raw typed code allowed developers to preserve compatibility between nongeneric legacy code and newer generic code but also gave rise to heap pollution. Heap pollution can occur if the program performs some operation involving a raw type that would give rise to a compile-time unchecked warning.

Heap pollution explained

Let's take an example:

Working with StringBuilder.

Java 8 and secure coding.

Are Streams safe?

Yes, Java 8 Streams are threadsafe.

If you listen to people from Oracle talking about design choices behind Java 8, you will often hear that parallelism was the main motivation. Parallelization was the main driving force behind lambdas, stream API and others. Let's take a look at an example of stream API.

There are only two options how to make sure that such thing will never happen. The first is to ensure that all tasks submitted to the common fork-join pool will not get stuck and will finish in a reasonable time. But it's easier said than done, especially in complex applications. The other option is to not use parallel streams and wait until Oracle allows us to specify the thread pool to be used for parallel streams.

Exception objects may convey sensitive information. For example, if a method calls the java.io.FileInputStream constructor to read an underlying configuration file and that file is not present, a java.io.FileNotFoundException containing the file path is thrown. Propagating this exception back to the method caller exposes the layout of the file system. Many forms of attack require knowing or guessing locations of files.

Exposing a file path containing the current user's name or home directory exacerbates the problem. SecurityManager checks guard this information when it is included in standard system properties (such as user.home) and revealing it in exception messages effectively allows these checks to be bypassed.

Internal exceptions should be caught and sanitized before propagating them to upstream callers. The type of an exception may reveal sensitive information, even if the message has been removed. For instance, FileNotFoundException reveals whether or not a given file exists.

3 Injection and Inclusion

A very common form of attack involves causing a particular program to interpret data crafted in such a way as to cause an unanticipated change of control. Typically, but not always, this involves text formats.

Guideline 3-1 / INJECT-1: Generate valid formatting
Attacks using maliciously crafted inputs to cause incorrect formatting of outputs are well-documented [7]. Such attacks generally involve exploiting special characters in an input string, incorrect escaping, or partial removal of special characters.

If the input string has a particular format, combining correction and validation is highly error-prone. Parsing and canonicalization should be done before validation. If possible, reject invalid data and any subsequent data, without attempting correction. For instance, many network protocols are vulnerable to cross-site POST attacks, by interpreting the HTTP body even though the HTTP header causes errors.

Use well-tested libraries instead of ad hoc code. There are many libraries for creating XML. Creating XML documents using raw text is error-prone. For unusual formats where appropriate libraries do not exist, such as configuration files, create classes that cleanly handle all formatting and only formatting code.

Guideline 3-2 / INJECT-2: Avoid dynamic SQL
It is well known that dynamically created SQL statements including untrusted input are subject to command injection. This often takes the form of supplying an input containing a quote character (') followed by SQL. Avoid dynamic SQL.

Deserialization of untrusted data
=================================

Description

Data which is untrusted cannot be trusted to be well formed. Malformed data or unexpected data could be used to abuse application logic, deny service, or execute arbitrary code, when deserialized.

Consequences

Availability: The logic of deserialization could be abused to create recursive object graphs or never provide data expected to terminate reading.
Authorization: Potentially code could make assumptions that information in the deserialized object about the data is valid. Functions which make this dangerous assumption could be exploited.
Access control (instruction processing): malicious objects can abuse the logic of custom deserializers in order to affect code execution.
Exposure period

Requirements specification: A deserialization library could be used which provides a cryptographic framework to seal serialized data.
Implementation: Not using the safe deserialization/serializing data features of a language can create data integrity problems.
Implementation: Not using the protection accessor functions of an object can cause data integrity problems
Implementation: Not protecting your objects from default overloaded functions - which may provide for raw output streams of objects - may cause data confidentiality problems.
Implementation: Not making fields transient can often cause data confidentiality problems.
Platform

It is often convenient to serialize objects for convenient communication or to save them for later use. However, deserialized data or code can often be modified without using the provided accessor functions if it does not use cryptography to protect itself. Furthermore, any cryptography would still be client-side security - which is of course a dangerous security assumption.

An attempt to serialize and then deserialize a class containing transient fields will result in NULLs where the non-transient data should be. This is an excellent way to prevent time, environment-based, or sensitive variables from being carried over and used improperly.

Risk Factors

Does the deserialization take place before authentication?
Does the deserialization limit which types can be deserialized?
Does the deserialization host have types available which can be repurposed towards malicious ends? Sometimes, these types are called "gadgets", considering their similarity to abusable bits of code that already exist in machine code in Return-Oriented-Programming attacks.




















   
