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

    ![Transitioning between thread states](https://github.com/torokmark/scjp-jegyzet/raw/master/images/ch9_threads_transitioning_between_thread_states.png)

* **Waiting/blocked/sleeping** The thread is still alive, but is currently not 
eligible to run. A thread may be *blocked* waiting for a resource. A thread may be
*sleeping* because the thread's run code *tells* it to sleep for some period of time.
Or the thread may be *waiting*, because the thread's run code *causes* it to wait.
The important point is that one thread does not tell another thread to block.

* **Dead** A thread is considered dead when its run() method completes.

### Sleeping ###
The `sleep()` method is a `static` method of class Thread. When a thread
sleeps, it drifts off somewhere and doesn't return to runnable until it wakes up.
Giving it a time in milliseconds.

``` java
try {
    Thread.sleep(5*60*1000); // Sleep for 5 minutes
} catch (InterruptedException ex) { }
```

`sleep()` method can throw a checked InterruptedException.
Typically, you wrap calls to `sleep()` in a `try/catch`.

*Still, using sleep() is the best way to help all threads get a chance to run!*

### Thread Priorities and yield() ###
Threads always run with some priority, usually represented as a number between 1
and 10.

*The lower-priority running thread usually will be bumped back to runnable and the highest-priority thread will be chosen to run.*
*In most cases, the running thread will be of equal or greater priority than the highest priority threads in the pool.*

A scheduler might do one of the following:

* Pick a thread to run, and run it there until it blocks or completes.
* Time slice the threads in the pool to give everyone an equal opportunity to run.

#### Setting a Thread's Priority ####

A thread gets a default priority that is *the priority of the thread of execution that creates it*.

Priorities are set using a positive integer, usually between 1 and 10, and the JVM will never change a thread's priority.

*The default priority is 5*, the Thread class has the three following constants (`static final` variables) that define the range of thread priorities:

    Thread.MIN_PRIORITY (1)
    Thread.NORM_PRIORITY (5)
    Thread.MAX_PRIORITY (10)

#### The yield( ) Method ####
`yield()` is *supposed* to make the currently running thread head back to runnable to allow other  threads of the same priority to get their turn.
A `yield()` won't ever cause a thread to go to the waiting/sleeping/ blocking state. At most, a `yield()` will cause a thread to go from running to runnable, but again, it might have no effect at all.

#### The join( ) Method ####
The non-`static` `join()` method of class Thread lets one thread "join onto the end" of another thread.

Three ways a running thread could leave the running state:

* **A call to sleep()** Guaranteed to cause the current thread to stop executing for at least the specified sleep duration
* **A call to yield()** Not guaranteed to do much of anything, although typically it will cause the currently running thread to move back to runnable so that a thread of the same priority can have a chance.
* **A call to join()** Guaranteed to cause the current thread to stop executing until the thread it joins with completes, or if the thread it's trying to join with is not alive, however,the current thread won't need to back out.

The following scenarios in which a thread might leave the running state:

* The thread's `run()` method completes.
* A call to `wait()` on an object
* A thread can't acquire the *lock* on the object whose method code it's attempting to run.
* The thread scheduler can decide to move the current thread from running to runnable in order to give another thread a chance to run. No reason is needed—the thread scheduler can trade threads in and out whenever it likes.

## Synchronizing Code (Objective 4.3) ##

### Synchronization and Locks ###

Every object in Java has a built-in lock that only comes into play when the object has synchronized method code. Acquiring a lock for an object is also known as getting the lock, or locking the object, locking *on* the object, or synchronizing on the object. We may also use the term *monitor* to refer to the object whose lock we're acquiring.


The following key points about locking and synchronization:

* Only methods (or blocks) can be `synchronized`, not variables or classes.
* Each object has just one lock.
* Not all methods in a class need to be `synchronized`. A class can have both `synchronized` and non-synchronized methods.
* If two threads are about to execute a `synchronized` method in a class, and both threads are using the same instance of the class to invoke the method, only one thread at a time will be able to execute the method.
* If a class has both `synchronized` and `non-synchronized` methods, multiple threads can still access the class's `non-synchronized` methods.
* If a thread goes to sleep, it holds any locks it has—it doesn't release them.
* A thread can acquire more than one lock.

Because synchronization does hurt concurrency, you don't want to synchronize any more code than is necessary to protect your data.

``` java
class SyncTest {
    public void doStuff() {
        System.out.println("not synchronized");
        synchronized(this) {
            System.out.println("synchronized");
        }
    }
}
```

``` java
public synchronized void doStuff() {
    System.out.println("synchronized");
}
```

is equivalent to this:

``` java
public void doStuff() {
    synchronized(this) {
        System.out.println("synchronized");
    }
}
```

#### So What About Static Methods? Can They Be Synchronized? ####
`static` methods can be `synchronized`. There is only one copy of the static data you're trying to protect, so you only need one lock per class to synchronize static methods—a lock for the whole class.

*Access to static fields should be done from `static synchronized` methods. Access to non-`static` fields should be done from non-`static synchronized` methods.*

``` java
public class Thing {
    private static int staticField;
    private int nonstaticField;
    public static synchronized int getStaticField() {
        return staticField;
    }
    public static synchronized void setStaticField(int staticField) {
        Thing.staticField = staticField;
    }
    public synchronized int getNonstaticField() {
        return nonstaticField;
    }
    public synchronized void setNonstaticField(int nonstaticField) {
        this.nonstaticField = nonstaticField;
    }
}
```

#### Thread-Safe Classes ####

When a class has been carefully synchronized to protect its data we say the class is "thread-safe".


### Thread Deadlock ###
Deadlock occurs when two threads are blocked, with each waiting for the other's lock. Neither can run until the other gives up its lock, so they'll sit there forever.

## Thread Interaction (Objective 4.4) ##

The last thing we need to look at is how threads can interact with one another to communicate about — among other things — their locking status.
The Object class has three methods, `wait()`, `notify()`, and `notifyAll()` that help threads
communicate about the status of an event that the threads care about.

*`wait()`, `notify()`, and `notifyAll()` must be called from within a synchronized context! A thread can't invoke a wait or notify method on an object unless it owns that object's lock.*

The methods `wait()` and `notify()`, remember, are instance methods of Object. In the same way that every object has a lock, every object can have a list of threads that are waiting for a signal (a notification) from the object.
A thread gets on this waiting list by executing the `wait()` method of the target object. From that moment, it doesn't execute any further instructions until the `notify()` method of the target object is called. If many threads are waiting on the same object, only one will be chosen (in no guaranteed order) to proceed with its execution.

### Using notifyAll( ) When Many Threads May Be Waiting ###
In most scenarios, it's preferable to notify *all* of the threads that are waiting on a particular object.
All of the threads will be notified and start competing to get the lock. As the lock is used and released by each thread, all of them will get into action without a need for further notification.






