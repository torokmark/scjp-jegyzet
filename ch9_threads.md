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

``` java
MyRunnable r = new MyRunnable();
Thread t = new Thread(r); // Pass your Runnable to the Thread
```

The Runnable you pass to the Thread constructor is called the *target* or the *target Runnable*.
You can pass a single Runnable instance to multiple Thread objects, so that the
same Runnable becomes the target of multiple threads, as follows:

``` java
public class TestThreads {
    public static void main (String [] args) {
        MyRunnable r = new MyRunnable();
        Thread foo = new Thread(r);
        Thread bar = new Thread(r);
        Thread bat = new Thread(r);
    }
}
```

The constructors we care about are:

* Thread()
* Thread(Runnable target)
* Thread(Runnable target, String name)
* Thread(String name)

When a thread has been instantiated but not started the thread is said to be in the *new* state.
At this stage, the thread is not yet considered to be *alive*. Once the `start()` method is called, 
the thread is considered to be *alive*.
A thread is considered *dead* (no longer *alive*) after the `run()` method completes.

### Starting a Thread ###

``` java
t.start();
```

Prior to calling start() on a Thread instance, the thread is said to be in the *new* state as we said.
What happens after you call `start()`?

* A new thread of execution starts (with a new call stack).
* The thread moves from the *new* state to the *runnable* state.
* When the thread gets a chance to execute, its target `run()` method will run.

#### Starting and Running Multiple Threads ####
We already had two threads, because the `main()` method starts in a
thread of its own, and then `t.start()` started a *second* thread.

> *Each thread will start, and each thread will run to completion.*

Within each thread, things will happen in a predictable order. But the actions
of different threads can mix together in unpredictable ways.
You don't know, for example, if one thread will run to completion before the others
have a chance to get in or whether they'll all take turns nicely, or whether they'll do
a combination of both. There is a way, however, to start a thread but tell it not to
run until some other thread has finished. You can do this with the `join()` method.

> *A thread is done being a thread when its target run() method completes.*  
> *Once a thread has been started, it can never be started again.*

If you have a reference to a Thread, and you call `start()`, it's started. If you call
`start()` a second time, it will cause an exception (an IllegalThreadStateException).
This happens whether or not the `run()` method has completed from the first `start()` call.

#### The Thread Scheduler ####
The thread scheduler is the part of the JVM (although most JVMs map Java threads
directly to native threads on the underlying OS) that decides which thread should
run at any given moment, and also takes threads *out* of the run state. Assuming a
single processor machine, only one thread can actually *run* at a time. Only one stack
can ever be executing at one time. And it's the thread scheduler that decides *which*
thread will actually *run*.
Any thread in the *runnable* state can be chosen by the scheduler to be the one and
only running thread. 
Queue behavior means 
that when a thread has finished with its "turn," it moves to the end of the line of the
runnable pool and waits until it eventually gets to the front of the line, where it can
be chosen again.
Although we don't control the thread scheduler, we can sometimes influence it.

#### Methods from the java.lang.Thread Class ####
Methods that can help us influence thread scheduling are as follows:

``` java
public static void sleep(long millis) throws InterruptedException
public static void yield()
public final void join() throws InterruptedException
public final void setPriority(int newPriority)
```

#### Methods from the java.lang.Object Class ####
Every class in Java inherits the following three thread-related methods:

``` java
public final void wait() throws InterruptedException
public final void notify()
public final void notifyAll()
```

## Thread States and Transitions (Objective 4.2) ##

### Thread States ###
A thread can be only in one of five states:

* **New** This is the state the thread is in after the Thread instance has been
created, but the `start()` method has not been invoked on the thread. It is
a live Thread object, but not yet a thread of execution. At this point, the
thread is considered *not alive*.
* **Runnable** This is the state a thread is in when it's eligible to run, but the
scheduler has not selected it to be the running thread. A thread first enters
the runnable state when the `start()` method is invoked, but a thread can
also return to the runnable state after either running or coming back from a
blocked, waiting, or sleeping state. When the thread is in the runnable state,
it is considered *alive*.
* **Running** This is the state a
thread is in when the thread scheduler selects it (from the runnable pool) to
be the currently executing process. A thread can transition out of a running
state for several reasons, including because "the thread scheduler felt like it."

![Transitioning between thread states](https://github.com/torokmark/scjp-jegyzet/blob/master/images/ch9_threads_transitioning_between_thread_states.png)

![Transitioning between thread states](https://raw.github.com/torokmark/scjp-jegyzet/master/images/ch9_threads_transitioning_between_thread_states.png)



asdfasdfasfd

