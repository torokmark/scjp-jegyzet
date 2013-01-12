# Threads #

## Defining, Instantiating, and Starting Threads (Objective 4.1) ##

In Java, "thread" means two different things:

* An instance of class java.lang.Thread
* A thread of execution

An instance of Thread is just ... an object. A thread of execution is an individual 
process (a "lightweight" process) that has its own call stack.

#### Making a Thread ####
A thread in Java begins as an instance of java.lang.Thread. You'll find methods
in the Thread class for managing threads including creating, starting, and pausing
them.

The action happens in the `run()` method.

``` java
public void run() {
    // your job code goes here
}
```

You can define and instantiate a thread in one of two ways:

* Extend the java.lang.Thread class.
* Implement the Runnable interface.

### Defining a Thread ###
To define a thread, you need a place to put your `run()` method, and as we just
discussed, you can do that by extending the Thread class or by implementing the
Runnable interface.

#### Extending java.lang.Thread ####
The simplest way to define code to run in a separate thread is to

* Extend the java.lang.Thread class.
* Override the `run()` method.

``` java
class MyThread extends Thread {
    public void run() {
        System.out.println("Important job running in MyThread");
    }
}
```

The limitation with this approach is that if you extend Thread, *you can't extend anything else*.
*The Thread class expects a `run()` method with no arguments, and it will execute this method for you in a separate call stack after the thread has been started*.

#### Implementing java.lang.Runnable ####

``` java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Important job running in MyRunnable");
    }
}
```

### Instantiating a Thread ###
Every thread of execution begins as an instance of class Thread.
If you extended the Thread class, instantiation is dead simple:

``` java
MyThread t = new MyThread();
```

If you implement Runnable, instantiation is only slightly less simple. To have
code run by a separate thread, *you still need a Thread instance*. But rather than
combining both the *thread* and the *job* (the code in the `run()` method) into one
class, you've split it into two classes — the Thread class for the *thread-specific* code
and your Runnable implementation class for your *job-that-should-be-run-by-a-thread*
code.

> Another common way to think about this is that the Thread is the "worker,"   
> and the Runnable is the "job" to be done.


