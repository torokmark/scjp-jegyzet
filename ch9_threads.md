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