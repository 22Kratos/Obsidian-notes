# Introduction

In the Java programming language, all source code is first written in plain text files ending with the `.java` extension. Those source files are then compiled into `.class` files by the `javac` compiler. A `.class` file does not contain code that is native to your processor; it instead contains _bytecodes_ — the machine language of the Java Virtual Machine[1](https://docs.oracle.com/javase/tutorial/getStarted/intro/definition.html#FOOT) (Java VM). The `java` launcher tool then runs your application with an instance of the Java Virtual Machine.

![[Pasted image 20260516124245.png]]

![[Pasted image 20260516124312.png]]

## Java Platform

A _platform_ is the hardware or software environment in which a program runs. The Java platform has two components:
- The _Java Virtual Machine_
- The _Java Application Programming Interface_ (API)

`The API is a large collection of ready-made software components that provide many useful capabilities. It is grouped into libraries of related classes and interfaces; these libraries are known as _packages_.`

### Features of Java

- **Development Tools**: The development tools provide everything you'll need for compiling, running, monitoring, debugging, and documenting your applications. As a new developer, the main tools you'll be using are the `javac` compiler, the `java` launcher, and the `javadoc` documentation tool.
- **Application Programming Interface (API)**: The API provides the core functionality of the Java programming language. It offers a wide array of useful classes ready for use in your own applications. It spans everything from basic objects, to networking and security, to XML generation and database access, and more.
- **Deployment Technologies**: The JDK software provides standard mechanisms such as the Java Web Start software and Java Plug-In software for deploying your applications to end users.
- **User Interface Toolkits**: The JavaFX, Swing, and Java 2D toolkits make it possible to create sophisticated Graphical User Interfaces (GUIs).
- **Integration Libraries**: Integration libraries such as the Java IDL API, JDBC API, Java Naming and Directory Interface (JNDI) API, Java RMI, and Java Remote Method Invocation over Internet Inter-ORB Protocol Technology (Java RMI-IIOP Technology) enable database access and manipulation of remote objects.

# JVM

## Data Types

JVMs operates on 2 data types: `primitive types` and `reference types`. 
There are two kinds of values that can be stored in variables, passed as arguments, returned by methods, and operated upon: `primitive values` and `reference values`.

### Primitive Types and Values

Primitive types in JVM are `numeric types`, `boolean type` and `returnAddress type`.
Numeric type consist of `integral types`, and the `floating point type`.

The integral types are:

- `byte`, whose values are 8-bit signed two's-complement integers, and whose default value is zero
- `short`, whose values are 16-bit signed two's-complement integers, and whose default value is zero
- `int`, whose values are 32-bit signed two's-complement integers, and whose default value is zero
- `long`, whose values are 64-bit signed two's-complement integers, and whose default value is zero
- `char`, whose values are 16-bit unsigned integers representing Unicode code points in the Basic Multilingual Plane, encoded with UTF-16, and whose default value is the null code point (`'\u0000'`)

The floating-point types are:
- `float`, whose values are elements of the float value set or, where supported, the float-extended-exponent value set, and whose default value is positive zero
- `double`, whose values are elements of the double value set or, where supported, the double-extended-exponent value set, and whose default value is positive zero

Values of boolean type encode `true` and `false`, default value is `false`.

`returnAddress` type are pointers to the `opcode` of JVM instructions.

```
opcode (or "operation code") is one-byte numeric representation of a specific command that the JVM executes
```

```
Note: Of all primtive types, only the returnAddress type is not directly associated with Java Programming Language
```

## PC Register

The Java Virtual Machine can support many threads of execution at once. Each Java Virtual Machine thread has its own `pc` (program counter) register. At any point, each Java Virtual Machine thread is executing the code of a single method, namely the current method for that thread. If that method is not `native`, the `pc` register contains the address of the Java Virtual Machine instruction currently being executed. If the method currently being executed by the thread is `native`, the value of the Java Virtual Machine's `pc` register is undefined. The Java Virtual Machine's `pc` register is wide enough to hold a `returnAddress` or a native pointer on the specific platform.

## JVM Stack

Each Java Virtual Machine thread has a private _Java Virtual Machine stack_, created at the same time as the thread. A Java Virtual Machine stack stores frames. A Java Virtual Machine stack is analogous to the stack of a conventional language such as C: it holds local variables and partial results, and plays a part in method invocation and return. Because the Java Virtual Machine stack is never manipulated directly except to push and pop frames, frames may be heap allocated. The memory for a Java Virtual Machine stack does not need to be contiguous.

This specification permits Java Virtual Machine stacks either to be of a fixed size or to dynamically expand and contract as required by the computation. If the Java Virtual Machine stacks are of a fixed size, the size of each Java Virtual Machine stack may be chosen independently when that stack is created.

The following exceptional conditions are associated with Java Virtual Machine stacks:

- If the computation in a thread requires a larger Java Virtual Machine stack than is permitted, the Java Virtual Machine throws a `StackOverflowError`.
- If Java Virtual Machine stacks can be dynamically expanded, and expansion is attempted but insufficient memory can be made available to effect the expansion, or if insufficient memory can be made available to create the initial Java Virtual Machine stack for a new thread, the Java Virtual Machine throws an `OutOfMemoryError`.

## Heap

The Java Virtual Machine has a _heap_ that is shared among all Java Virtual Machine threads. The heap is the run-time data area from which memory for all class instances and arrays is allocated.

The heap is created on virtual machine start-up. Heap storage for objects is reclaimed by an automatic storage management system (known as a _garbage collector_); objects are never explicitly deallocated. The Java Virtual Machine assumes no particular type of automatic storage management system, and the storage management technique may be chosen according to the implementor's system requirements. The heap may be of a fixed size or may be expanded as required by the computation and may be contracted if a larger heap becomes unnecessary. The memory for the heap does not need to be contiguous.

The following exceptional condition is associated with the heap:

- If a computation requires more heap than can be made available by the automatic storage management system, the Java Virtual Machine throws an `OutOfMemoryError`.

## Method Area

The Java Virtual Machine has a _method area_ that is shared among all Java Virtual Machine threads. The method area is analogous to the storage area for compiled code of a conventional language or analogous to the "text" segment in an operating system process. It stores per-class structures such as the run-time constant pool, field and method data, and the code for methods and constructors, including the special methods used in class and instance initialization and interface initialization.

The method area is created on virtual machine start-up. Although the method area is logically part of the heap, simple implementations may choose not to either garbage collect or compact it. This specification does not mandate the location of the method area or the policies used to manage compiled code. The method area may be of a fixed size or may be expanded as required by the computation and may be contracted if a larger method area becomes unnecessary. The memory for the method area does not need to be contiguous.

# Garbage Collection

Garbage collection in Java is an automatic memory management process that helps Java programs run efficiently.
- Objects are created on the heap area.
- Eventually, some objects will no longer be needed.
- Garbage collection is an automatic process that removes unused objects from heap.

## Working

- It identifies which objects are still in use (referenced) and which are not in use (unreferenced).
- It removes the objects that are unreachable (no longer referenced).
- The programmer does not need to mark objects to be deleted explicitly. Garbage collection is implemented within the JVM.

## Types of Activities in Java Garbage Collection

Java heap is divided into generations:

- **Young Generation:** In this new objects are allocated.
- **Old Generation:** In this long-lived objects are stored.

Two types of garbage collection activities usually happen in Java. These are:

- **Minor or incremental Garbage Collection (GC)**: This occurs when unreachable objects in the Young Generation heap memory are removed.
- **Major or Full Garbage Collection (GC)**: This happens when objects that survived minor garbage collection are removed from the Old Generation heap memory. It occurs less frequently than minor garbage collection.

# Input/Output Streams

An _I/O Stream_ represents an input source or an output destination. A stream can represent many different kinds of sources and destinations, including disk files, devices, other programs, and memory arrays.

## Byte Streams

There are many byte stream classes. To demonstrate how byte streams work, we'll focus on the file I/O byte streams, [`FileInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/FileInputStream.html) and [`FileOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/FileOutputStream.html). Other kinds of byte streams are used in much the same way; they differ mainly in the way they are constructed.

```
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyBytes {
    public static void main(String[] args) throws IOException {

        FileInputStream in = null;
        FileOutputStream out = null;

        try {
            in = new FileInputStream("xanadu.txt");
            out = new FileOutputStream("outagain.txt");
            int c;

            while ((c = in.read()) != -1) {
                out.write(c);
            }
        } finally {
            if (in != null) {
                in.close();
            }
            if (out != null) {
                out.close();
            }
        }
    }
}
```

### Always Closing Stream

Closing a stream when it's no longer needed is very important — so important that `CopyBytes` uses a `finally` block to guarantee that both streams will be closed even if an error occurs. This practice helps avoid serious resource leaks.

## Character Streams

The Java platform stores character values using Unicode conventions. Character stream I/O automatically translates this internal format to and from the local character set. In Western locales, the local character set is usually an 8-bit superset of ASCII.

```
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class CopyCharacters {
    public static void main(String[] args) throws IOException {

        FileReader inputStream = null;
        FileWriter outputStream = null;

        try {
            inputStream = new FileReader("xanadu.txt");
            outputStream = new FileWriter("characteroutput.txt");

            int c;
            while ((c = inputStream.read()) != -1) {
                outputStream.write(c);
            }
        } finally {
            if (inputStream != null) {
                inputStream.close();
            }
            if (outputStream != null) {
                outputStream.close();
            }
        }
    }
}
```

## Buffered Stream

Buffered input streams read data from a memory area known as a _buffer_; the native input API is called only when the buffer is empty. Similarly, buffered output streams write data to a buffer, and the native output API is called only when the buffer is full.
A program can convert an unbuffered stream into a buffered stream using the wrapping idiom we've used several times now, where the unbuffered stream object is passed to the constructor for a buffered stream class. Here's how you might modify the constructor invocations in the `CopyCharacters` example to use buffered I/O:

```
inputStream = new BufferedReader(new FileReader("xanadu.txt"));
outputStream = new BufferedWriter(new FileWriter("characteroutput.txt"));
```

There are four buffered stream classes used to wrap unbuffered streams: [`BufferedInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedInputStream.html) and [`BufferedOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedOutputStream.html) create buffered byte streams, while [`BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html) and [`BufferedWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedWriter.html) create buffered character streams.

It often makes sense to write out a buffer at critical points, without waiting for it to fill. This is known as _flushing_ the buffer.

## Scanning and Formatting

Programming I/O often involves translating to and from the neatly formatted data humans like to work with. To assist you with these chores, the Java platform provides two APIs. The [scanner](https://docs.oracle.com/javase/tutorial/essential/io/scanning.html) API breaks input into individual tokens associated with bits of data. The [formatting](https://docs.oracle.com/javase/tutorial/essential/io/formatting.html) API assembles data into nicely formatted, human-readable form.

### Scanning

```
import java.io.*;
import java.util.Scanner;

public class ScanXan {
    public static void main(String[] args) throws IOException {

        Scanner s = null;

        try {
            s = new Scanner(new BufferedReader(new FileReader("xanadu.txt")));

            while (s.hasNext()) {
                System.out.println(s.next());
            }
        } finally {
            if (s != null) {
                s.close();
            }
        }
    }
}
```

### Formatting

Invoking `print` or `println` outputs a single value after converting the value using the appropriate `toString` method. We can see this in the [`Root`](https://docs.oracle.com/javase/tutorial/essential/io/examples/Root.java) example:

```
public class Root {
    public static void main(String[] args) {
        int i = 2;
        double r = Math.sqrt(i);
        
        System.out.print("The square root of ");
        System.out.print(i);
        System.out.print(" is ");
        System.out.print(r);
        System.out.println(".");

        i = 5;
        r = Math.sqrt(i);
        System.out.println("The square root of " + i + " is " + r + ".");
    }
}
```

# Concurrency

## Processes

A process has a self-contained execution environment. A process generally has a complete, private set of basic run-time resources; in particular, each process has its own memory space.
To facilitate communication between processes, most operating systems support `Inter Process Communication (IPC)` resources, such as `pipes and sockets`. IPC is used not just for communication between processes on the same system, but processes on different systems.
Java application can create additional processes using `ProcessBuilder` Object.

## Threads
Threads are sometimes called _lightweight processes_. Both processes and threads provide an execution environment, but creating a new thread requires fewer resources than creating a new process.
Threads exist within a process — every process has at least one. Threads share the process's resources, including memory and open files. This makes for efficient, but potentially problematic, communication.
Multi threaded execution is an essential feature of the Java platform. Every application has at least one thread — or several, if you count "system" threads that do things like memory management and signal handling. But from the application programmer's point of view, you start with just one thread, called the `main thread`. The `main thread` has the ability to create additional threads.

## Thread Objects
Each thread is associated with an instance of the class `Thread`. There are two basic ways of using `Thread` object to create concurrent application.

1. To directly control thread creation and management, simply instantiate `Thread` each time the application needs to initiate an asynchronous task.
2. To abstract thread management from the rest of your application, pass the application's tasks to an `executor`.

## Defining and Starting Thread

There are 2 ways to create a `Thread`.

1. Provide a `Runnable` object. The `Runnable` interface defines a single method, `run`, meant to contain the code executed in the thread.

```
public class HelloRunnable implements Runnable {

    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new Thread(new HelloRunnable())).start();
    }

}
```

2. _Subclass `Thread`._ The `Thread` class itself implements `Runnable`, though its `run` method does nothing. An application can subclass `Thread`, providing its own implementation of `run`

```
public class HelloThread extends Thread {

    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new HelloThread()).start();
    }

}
```

## Pausing Execution with Sleep

`Thread.sleep` causes the current thread to suspend execution for a specified period. This is an efficient means of making processor time available to the other threads of an application or other applications that might be running on a computer system. The `sleep` method can also be used for pacing, and waiting for another thread with duties that are understood to have time requirements

```
public class SleepMessages {
    public static void main(String args[])
        throws InterruptedException {
        String importantInfo[] = {
            "Mares eat oats",
            "Does eat oats",
            "Little lambs eat ivy",
            "A kid will eat ivy too"
        };

        for (int i = 0;
             i < importantInfo.length;
             i++) {
            //Pause for 4 seconds
            Thread.sleep(4000);
            //Print a message
            System.out.println(importantInfo[i]);
        }
    }
}
```

## Interupts

An `_interrupt_` is an indication to a thread that it should stop what it is doing and do something else. It's up to the programmer to decide exactly how a thread responds to an interrupt, but it is very common for the thread to terminate.

## Joins

The `join` method allows one thread to wait for the completion of another. If `t` is a `Thread` object whose thread is currently executing,

```
t.join();
```

causes the current thread to pause execution until `t`'s thread terminates. Overloads of `join` allow the programmer to specify a waiting period. However, as with `sleep`, `join` is dependent on the OS for timing, so you should not assume that `join` will wait exactly as long as you specify.
Like `sleep`, `join` responds to an interrupt by exiting with an `InterruptedException`.

## Synchronization

Threads communicate primarily by sharing access to fields and the objects reference fields refer to. This form of communication is extremely efficient, but makes two kinds of errors possible: `thread interference and memory consistency errors`. The tool needed to prevent these errors is `synchronization`.
However, `synchronization` can introduce `thread contention`, which occurs when two or more threads try to access the same resource simultaneously _and_ cause the Java runtime to execute one or more threads more slowly, or even suspend their execution.

## Atomic Access

In programming, an _atomic_ action is one that effectively happens all at once. An atomic action cannot stop in the middle: it either happens completely, or it doesn't happen at all. No side effects of an atomic action are visible until the action is complete.
We have already seen that an increment expression, such as `c++`, does not describe an atomic action. Even very simple expressions can define complex actions that can decompose into other actions. However, there are actions you can specify that are atomic:
- Reads and writes are atomic for reference variables and for most primitive variables (all types except `long` and `double`).
- Reads and writes are atomic for _all_ variables declared `volatile` (_including_ `long` and `double` variables).

## Liveness

A concurrent application's ability to execute in a timely manner is known as its _liveness_.

### DeadLock

_Deadlock_ describes a situation where two or more threads are blocked forever, waiting for each other. Here's an example.
```
Alphonse and Gaston are friends, and great believers in courtesy. A strict rule of courtesy is that when you bow to a friend, you must remain bowed until your friend has a chance to return the bow. Unfortunately, this rule does not account for the possibility that two friends might bow to each other at the same time.
```

### Starvation

_Starvation_ describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress. This happens when shared resources are made unavailable for long periods by "greedy" threads. For example, suppose an object provides a synchronized method that often takes a long time to return. If one thread invokes this method frequently, other threads that also need frequent synchronized access to the same object will often be blocked.

### Livelock

A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then _livelock_ may result. As with deadlock, livelocked threads are unable to make further progress. However, the threads are not blocked — they are simply too busy responding to each other to resume work. This is comparable to two people attempting to pass each other in a corridor: Alphonse moves to his left to let Gaston pass, while Gaston moves to his right to let Alphonse pass. Seeing that they are still blocking each other, Alphone moves to his right, while Gaston moves to his left.

## Immutable Objects

An object is considered _immutable_ if its state cannot change after it is constructed. Maximum reliance on immutable objects is widely accepted as a sound strategy for creating simple, reliable code.
Immutable objects are particularly useful in concurrent applications. Since they cannot change state, they cannot be corrupted by thread interference or observed in an inconsistent state.

# JDBC

The JDBC API is a java API that can access any kind of tabular data, especially from a relational database.

It does 3 activities:
- Connect to data source
- Send queries and update statements
- Retrieve and process the results recieved

### Example Code:

```
public void connectToAndQueryDatabase(String Username, String Password){

Connection con = DriverManager.getConnection(
					"jdbc:myDriver.myDatabase",
					username,
					password);
Statement stmt = con.createStatement();
ResultSet rs = stmt.executeQuery("SELECT a, b, c FROM TABLE1");

while (rs.next()){
	int x = rs.getInt("a");
	String s = rs.getString("b");
	float f = rs.getFloat("c");}
}
```

## JDBC Components

There are 4 components:

1. JDBC API: Provides access to RDBMS from Java. It can execute SQL statements, retrieve data, and propagate changes back to data source. 
   Contains two packages: `java.sql` and `javax.sql`
2. JDBC Driver Manager: `DriverManager` class defines objects which can connect Java apps to JDBC driver. Backbone of JDBC. Small and simple.
   Has extension package `javax.naming` and `javax.sql` which lets you use `JNDI (Java Naming and Directory Interface)`
3. JDBC Test Suite: helps you determine that JDBC drivers will run your program.
4. JDBC-ODBC Bridge: Allows JDBE access via ODBC drivers.

## Processing SQL Statements

There are 5 steps:
1. Establish Connection
2. Create statement
3. Execute statement
4. Process the `ResultSet` object
5. Close connection

## JDBC Architecture

The JDBC API supports both two-tier and three-tier processing models for database access.

### Two Tier Architecture

![[Pasted image 20260429233404.png]]

In the two-tier model, a Java applet or application talks directly to the data source. This requires a JDBC driver that can communicate with the particular data source being accessed. A user's commands are delivered to the database or other data source, and the results of those statements are sent back to the user. The data source may be located on another machine to which the user is connected via a network. This is referred to as a client/server configuration, with the user's machine as the client, and the machine housing the data source as the server. The network can be an intranet, which, for example, connects employees within a corporation, or it can be the Internet.

### Three Tier Architecture

![[Pasted image 20260429233428.png]]

In the three-tier model, commands are sent to a "middle tier" of services, which then sends the commands to the data source. The data source processes the commands and sends the results back to the middle tier, which then sends them to the user. MIS directors find the three-tier model very attractive because the middle tier makes it possible to maintain control over access and the kinds of updates that can be made to corporate data. Another advantage is that it simplifies the deployment of applications. Finally, in many cases, the three-tier architecture can provide performance advantages.

## Metadata in database

Databases store user data, and they also store information about the database itself. Most DBMSs have a set of system tables, which list tables in the database, column names in each table, primary keys, foreign keys, stored procedures, and so forth. Each DBMS has its own functions for getting information about table layouts and database features. JDBC provides the interface `DatabaseMetaData`, which a driver writer must implement so that its methods return information about the driver and/or DBMS for which the driver is written. For example, a large number of methods return whether or not the driver supports a particular functionality. This interface gives users and tools a standardized way to get metadata. In general, developers writing tools and drivers are the ones most likely to be concerned with metadata.

# Serialization

To _serialize_ an object means to convert its state to a byte stream so that the byte stream can be reverted back into a copy of the object. A Java object is _serializable_ if its class or any of its superclasses implements either the java.io.Serializable interface or its subinterface, java.io.Externalizable. _Deserialization_ is the process of converting the serialized form of an object back into a copy of the object.
For example, the java.awt.Button class implements the Serializable interface, so you can serialize a java.awt.Button object and store that serialized state in a file. Later, you can read back the serialized state and deserialize into a java.awt.Button object.

# Sockets/Networking

`A socket is one end-point of a two-way communication link between two programs running on the network. Socket classes are used to represent the connection between a client program and a server program. The java.net package provides two classes--Socket and ServerSocket--that implement the client side of the connection and the server side of the connection, respectively.`

![[Pasted image 20260430072650.png]]

The `java.net` package in the Java platform provides a class, `Socket`, that implements one side of a two-way connection between your Java program and another program on the network. The `Socket` class sits on top of a platform-dependent implementation, hiding the details of any particular system from your Java program. By using the `java.net.Socket` class instead of relying on native code, your Java programs can communicate over the network in a platform-independent fashion.

# RMI (Remote Method Invokation)

The Java Remote Method Invocation (RMI) system allows an object running in one Java virtual machine to invoke methods on an object running in another Java virtual machine. RMI provides for remote communication between programs written in the Java programming language.

## Overview

RMI applications often comprise two separate programs, a server and a client. A typical server program creates some remote objects, makes references to these objects accessible, and waits for clients to invoke methods on these objects. A typical client program obtains a remote reference to one or more remote objects on a server and then invokes methods on them. RMI provides the mechanism by which the server and the client communicate and pass information back and forth. Such an application is sometimes referred to as a _distributed object application_.

- **Locate remote objects.** Applications can use various mechanisms to obtain references to remote objects. For example, an application can register its remote objects with RMI's simple naming facility, the RMI registry. Alternatively, an application can pass and return remote object references as part of other remote invocations.
- **Communicate with remote objects.** Details of communication between remote objects are handled by RMI. To the programmer, remote communication looks similar to regular Java method invocations.
- **Load class definitions for objects that are passed around.** Because RMI enables objects to be passed back and forth, it provides mechanisms for loading an object's class definitions as well as for transmitting an object's data.

![[Pasted image 20260430074532.png]]

An object becomes remote by implementing a _remote interface_, which has the following characteristics:

- A remote interface extends the interface `java.rmi.Remote`.
- Each method of the interface declares `java.rmi.RemoteException` in its `throws` clause, in addition to any application-specific exceptions.

# JNI (Java Native Interface)

`The JNI is a native programming interface. It allows Java code that runs inside a Java Virtual Machine (VM) to interoperate with applications and libraries written in other programming languages, such as C, C++, and assembly. The most important benefit of the JNI is that it imposes no restrictions on the implementation of the underlying Java VM. Therefore, Java VM vendors can add support for the JNI without affecting other parts of the VM. Programmers can write one version of a native application or library and expect it to work with all Java VMs supporting the JNI.`

# Collections

`A _collection_ — sometimes called a container — is simply an object that groups multiple elements into a single unit. Collections are used to store, retrieve, manipulate, and communicate aggregate data. Typically, they represent data items that form a natural group, such as a poker hand (a collection of cards), a mail folder (a collection of letters), or a telephone directory (a mapping of names to phone numbers).`

## Collections Framework

A _collections framework_ is a unified architecture for representing and manipulating collections. All collections frameworks contain the following:

- **Interfaces:** These are abstract data types that represent collections. Interfaces allow collections to be manipulated independently of the details of their representation. In object-oriented languages, interfaces generally form a hierarchy.
- **Implementations:** These are the concrete implementations of the collection interfaces. In essence, they are reusable data structures.
- **Algorithms:** These are the methods that perform useful computations, such as searching and sorting, on objects that implement collection interfaces. The algorithms are said to be _polymorphic_: that is, the same method can be used on many different implementations of the appropriate collection interface. In essence, algorithms are reusable functionality.

## Benefits of Java Collection Framework

The Java Collections Framework provides the following benefits:

- **Reduces programming effort:** By providing useful data structures and algorithms, the Collections Framework frees you to concentrate on the important parts of your program rather than on the low-level "plumbing" required to make it work. By facilitating interoperability among unrelated APIs, the Java Collections Framework frees you from writing adapter objects or conversion code to connect APIs.
- **Increases program speed and quality:** This Collections Framework provides high-performance, high-quality implementations of useful data structures and algorithms. The various implementations of each interface are interchangeable, so programs can be easily tuned by switching collection implementations. Because you're freed from the drudgery of writing your own data structures, you'll have more time to devote to improving programs' quality and performance.
- **Allows interoperability among unrelated APIs:** The collection interfaces are the vernacular by which APIs pass collections back and forth. If my network administration API furnishes a collection of node names and if your GUI toolkit expects a collection of column headings, our APIs will interoperate seamlessly, even though they were written independently.
- **Reduces effort to learn and to use new APIs:** Many APIs naturally take collections on input and furnish them as output. In the past, each such API had a small sub-API devoted to manipulating its collections. There was little consistency among these ad hoc collections sub-APIs, so you had to learn each one from scratch, and it was easy to make mistakes when using them. With the advent of standard collection interfaces, the problem went away.
- **Reduces effort to design new APIs:** This is the flip side of the previous advantage. Designers and implementer don't have to reinvent the wheel each time they create an API that relies on collections; instead, they can use standard collection interfaces.
- **Fosters software reuse:** New data structures that conform to the standard collection interfaces are by nature reusable. The same goes for new algorithms that operate on objects that implement these interfaces.

# Interfaces

The _core collection interfaces_ encapsulate different types of collections, which are shown in the figure below. These interfaces allow collections to be manipulated independently of the details of their representation. Core collection interfaces are the foundation of the Java Collections Framework.

![[Pasted image 20260604172301.png]]

A `Set` is a special kind of `Collection`, a `SortedSet` is a special kind of `Set`, and so forth.

```
NOTE:
A map is not a true Collection
```

List of core collection interfaces:

- `Collection` — the root of the collection hierarchy. A collection represents a group of objects known as its _elements_. The `Collection` interface is the least common denominator that all collections implement and is used to pass collections around and to manipulate them when maximum generality is desired. Some types of collections allow duplicate elements, and others do not. Some are ordered and others are unordered.
- `Set` — a collection that cannot contain duplicate elements. This interface models the mathematical set abstraction and is used to represent sets, such as the cards comprising a poker hand.
- `List` — an ordered collection (sometimes called a _sequence_). `List`s can contain duplicate elements. The user of a `List` generally has precise control over where in the list each element is inserted and can access elements by their integer index (position). If you've used `Vector`, you're familiar with the general flavor of `List`.
- `Queue` — a collection used to hold multiple elements prior to processing. Besides basic `Collection` operations, a `Queue` provides additional insertion, extraction, and inspection operations.
  
  Queues typically, but do not necessarily, order elements in a FIFO (first-in, first-out) manner. Among the exceptions are `priority queues`, which order elements according to a supplied comparator or the elements' natural ordering. Whatever the ordering used, the head of the queue is the element that would be removed by a call to `remove` or `poll`. In a FIFO queue, all new elements are inserted at the tail of the queue. Other kinds of queues may use different placement rules. Every `Queue` implementation must specify its ordering properties.
 
- `Deque` — a collection used to hold multiple elements prior to processing. Besides basic `Collection` operations, a `Deque` provides additional insertion, extraction, and inspection operations.
 
  Deques can be used both as FIFO (first-in, first-out) and LIFO (last-in, first-out). In a deque all new elements can be inserted, retrieved and removed at both ends.  
- `Map` — an object that maps keys to values. A `Map` cannot contain duplicate keys; each key can map to at most one value. If you've used `Hashtable`, you're already familiar with the basics of `Map`.

The last two core collection interfaces are merely sorted versions of `Set` and `Map`:

- `SortedSet` — a `Set` that maintains its elements in ascending order. Several additional operations are provided to take advantage of the ordering. Sorted sets are used for naturally ordered sets, such as word lists and membership rolls.
- `SortedMap` — a `Map` that maintains its mappings in ascending key order. This is the `Map` analog of `SortedSet`. Sorted maps are used for naturally ordered collections of key/value pairs, such as dictionaries and telephone directories.


## Vector

- Like an **ArrayList** but **synchronized** (thread-safe)
- Grows dynamically; doubles its size when full
- Slower than ArrayList due to synchronization overhead

```
Vector<Integer> v = new Vector<>();
v.add(10);
v.add(20);
v.remove(0);           // removes index 0
System.out.println(v); // [20]
```

**Key methods:** `add()`, `remove()`, `get()`, `size()`, `elementAt()`, `capacity()`

## Stack

- Extends `Vector` — works on **LIFO** (Last In, First Out)
- Think of a stack of plates

```
Stack<String> s = new Stack<>();
s.push("A");
s.push("B");
System.out.println(s.pop());  // B (removes & returns)
System.out.println(s.peek()); // A (just looks, no remove)
System.out.println(s.empty()); // false
```

## Hashtable

- Stores **key-value pairs**, like a dictionary
- **Synchronized** (thread-safe), does **not allow null** keys or values
- Legacy version of `HashMap`

```
Hashtable<String, Integer> ht = new Hashtable<>();
ht.put("Alice", 90);
ht.put("Bob", 85);
System.out.println(ht.get("Alice")); // 90
System.out.println(ht.containsKey("Bob")); // true
```

## Set (interface)

- A collection that **does NOT allow duplicates**
- Does **not guarantee order** (unless you use `LinkedHashSet` or `TreeSet`)
- Common implementations: `HashSet`, `TreeSet`, `LinkedHashSet`

```
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");
set.add("Apple"); // duplicate — ignored!
System.out.println(set); // [Apple, Banana] (order may vary)
```

## List (Interface)

- **Ordered** collection, **allows duplicates**
- Elements accessible by **index**
- Common implementations: `ArrayList`, `LinkedList`, `Vector`

```
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("A"); // duplicate allowed
System.out.println(list.get(0)); // A
System.out.println(list.size()); // 3
```

## Map (Interface)

- Stores **key-value pairs**; keys are **unique**, values can repeat
- Common implementations: `HashMap`, `LinkedHashMap`, `TreeMap`, `Hashtable`

```
Map<String, Integer> map = new HashMap<>();
map.put("Math", 95);
map.put("Science", 88);
map.put("Math", 99); // overwrites previous value
System.out.println(map.get("Math")); // 99
```

**Key methods:** `put()`, `get()`, `remove()`, `containsKey()`, `keySet()`, `values()`, `entrySet()`

## Iterator

- Used to **traverse/loop through** any Collection (List, Set, etc.)
- One-directional (forward only)
- Safer than a for-loop when **removing elements** during iteration

```
List<String> list = new ArrayList<>(List.of("A", "B", "C"));
Iterator<String> it = list.iterator();

while (it.hasNext()) {
    String val = it.next();
    if (val.equals("B")) it.remove(); // safe removal
    else System.out.println(val);
}
// Output: A, C
```

## Enumeration

- Older version of `Iterator`, used with legacy classes like `Vector` and `Hashtable`
- **Cannot remove** elements (read-only traversal)

```
Vector<String> v = new Vector<>();
v.add("X"); v.add("Y"); v.add("Z");

Enumeration<String> e = v.elements();
while (e.hasMoreElements()) {
    System.out.println(e.nextElement());
}
```

**Key methods:** `hasMoreElements()`, `nextElement()`

# Dynamic Method Dispatch

## Polymorphism

Subclasses of a class can define their own unique behaviors and yet share some of the same functionality of the parent class.

### Example:

```
public void printDescription(){
    System.out.println("\nBike is " + "in gear " + this.gear
        + " with a cadence of " + this.cadence +
        " and travelling at a speed of " + this.speed + ". ");
}
```

To demonstrate polymorphic features in the Java language, extend the `Bicycle` class with a `MountainBike` and a `RoadBike` class. For `MountainBike`, add a field for `suspension`, which is a `String` value that indicates if the bike has a front shock absorber, `Front`. Or, the bike has a front and back shock absorber, `Dual`.

Here is the updated class:

```
public class MountainBike extends Bicycle {
    private String suspension;

    public MountainBike(
               int startCadence,
               int startSpeed,
               int startGear,
               String suspensionType){
        super(startCadence,
              startSpeed,
              startGear);
        this.setSuspension(suspensionType);
    }

    public String getSuspension(){
      return this.suspension;
    }

    public void setSuspension(String suspensionType) {
        this.suspension = suspensionType;
    }

    public void printDescription() {
        super.printDescription();
        System.out.println("The " + "MountainBike has a" +
            getSuspension() + " suspension.");
    }
}
```

Note the overridden `printDescription` method. In addition to the information provided before, additional data about the suspension is included to the output.

Next, create the `RoadBike` class. Because road or racing bikes have skinny tires, add an attribute to track the tire width. Here is the `RoadBike` class:

```
public class RoadBike extends Bicycle{
    // In millimeters (mm)
    private int tireWidth;

    public RoadBike(int startCadence,
                    int startSpeed,
                    int startGear,
                    int newTireWidth){
        super(startCadence,
              startSpeed,
              startGear);
        this.setTireWidth(newTireWidth);
    }

    public int getTireWidth(){
      return this.tireWidth;
    }

    public void setTireWidth(int newTireWidth){
        this.tireWidth = newTireWidth;
    }

    public void printDescription(){
        super.printDescription();
        System.out.println("The RoadBike" + " has " + getTireWidth() +
            " MM tires.");
    }
}
```

Note that once again, the `printDescription` method has been overridden. This time, information about the tire width is displayed.

To summarize, there are three classes: `Bicycle`, `MountainBike`, and `RoadBike`. The two subclasses override the `printDescription` method and print unique information.

Here is a test program that creates three `Bicycle` variables. Each variable is assigned to one of the three bicycle classes. Each variable is then printed.

```
public class TestBikes {
  public static void main(String[] args){
    Bicycle bike01, bike02, bike03;

    bike01 = new Bicycle(20, 10, 1);
    bike02 = new MountainBike(20, 10, 5, "Dual");
    bike03 = new RoadBike(40, 20, 8, 23);

    bike01.printDescription();
    bike02.printDescription();
    bike03.printDescription();
  }
}
```

Output:

```
Bike is in gear 1 with a cadence of 20 and travelling at a speed of 10. 

Bike is in gear 5 with a cadence of 20 and travelling at a speed of 10. 
The MountainBike has a Dual suspension.

Bike is in gear 8 with a cadence of 40 and travelling at a speed of 20. 
The RoadBike has 23 MM tires.
```

The Java virtual machine (JVM) calls the appropriate method for the object that is referred to in each variable. It does not call the method that is defined by the variable's type. This behaviour is referred to as _virtual method invocation_ and demonstrates an aspect of the important isomorphism features in the Java language.

## Overriding and Hiding Methods

### Instance Methods

An instance method in a subclass with the same signature (name, plus the number and the type of its parameters) and return type as an instance method in the superclass _overrides_ the superclass's method.

The ability of a subclass to override a method allows a class to inherit from a superclass whose behavior is "close enough" and then to modify behavior as needed. The overriding method has the same name, number and type of parameters, and return type as the method that it overrides. An overriding method can also return a subtype of the type returned by the overridden method. This subtype is called a _covariant return type_.

When overriding a method, you might want to use the `@Override` annotation that instructs the compiler that you intend to override a method in the superclass. If, for some reason, the compiler detects that the method does not exist in one of the superclasses, then it will generate an error.

### Static Methods

If a subclass defines a static method with the same signature as a static method in the superclass, then the method in the subclass _hides_ the one in the superclass.

The distinction between hiding a static method and overriding an instance method has important implications:

- The version of the overridden instance method that gets invoked is the one in the subclass.
- The version of the hidden static method that gets invoked depends on whether it is invoked from the superclass or the subclass.

Consider an example that contains two classes. The first is `Animal`, which contains one instance method and one static method:

```
public class Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Animal");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Animal");
    }
}
```

The second class, a subclass of `Animal`, is called `Cat`:

```
public class Cat extends Animal {
    public static void testClassMethod() {
        System.out.println("The static method in Cat");
    }
    public void testInstanceMethod() {
        System.out.println("The instance method in Cat");
    }

    public static void main(String[] args) {
        Cat myCat = new Cat();
        Animal myAnimal = myCat;
        Animal.testClassMethod();
        myAnimal.testInstanceMethod();
    }
}
```

```
NOTE:
Overriding basically supports late binding. Therefore, it's decided at run time which method will be called. It is for non-static methods.

Hiding is for all other members (static methods, instance members, static members). It is based on the early binding. More clearly, the method or member to be called or used is decided during compile time.
```

