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

Thread API's:

Each single thread in Java is assigned to a thread group upon the thread's creation. These groups are implemented by the java.lang.ThreadGroup class. When the thread group name is not specified explicitly, the main default group is assigned by the Java Virtual Machine (JVM) [Java Tutorials]. The convenience methods of the ThreadGroup class can be used to operate on all threads belonging to a thread group at once. For instance, the ThreadGroup.interrupt() method interrupts all threads in the thread group. Thread groups also help reinforce layered security by confining threads into groups so that they avoid interference with threads in other groups.

DateTime API and Threads... Will Java 8 change this issue ?

Well, let's say Java 8 is a really big approach into the right direction.

Java's new DateTime API.









   
