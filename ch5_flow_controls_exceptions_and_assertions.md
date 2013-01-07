# Flow Control, Exceptions, and Assertions #

## if and switch Statements (Exam Objective 2.1) ##
The `if` and `switch` statements are commonly referred to as decision statements.

### if-else Branching ###

``` java
if (booleanExpression) {  
    System.out.println("Inside if statement");  
}  
```  

The expression in parentheses must evaluate to (a boolean) `true` or `false`.
If it's true and then running a code block (one or more statements) if it is true, and (optionally) another block of code if it isn't.

The `else` block is optional.
Even the curly braces are optional if you have only one statement to execute within the body of the conditional block.

There are a couple of rules for using `else` and `else if`:

* You can have zero or one `else` for a given if, and it must come after any `else if`s.
* You can have zero to many `else if`s for a given `if` and they must come before the (optional) `else`.
* Once an `else if` succeeds, none of the remaining `else if`s or `else`s will be tested.

An `else` clause belongs to the innermost `if` statement to which it might possibly belong.

``` java
if (exam.done())  
    if (exam.getScore() < 0.61)  
        System.out.println("Try again.");  
    // Which if does this belong to?  
    else  
        System.out.println("Java master!");   
```

``` java
if (exam.done())  
    if (exam.getScore() < 0.61)  
        System.out.println("Try again.");  
else  
    System.out.println("Java master!"); // Hmmmmm… now where does it belong?  
```

It would be even easier to read:

``` java
if (exam.done()) {  
    if (exam.getScore() < 0.61) {  
        System.out.println("Try again.");  
    // Which if does this belong to?  
    } else {  
        System.out.println("Java master!");  
    }  
}  
```

#### Legal Expressions for if Statements ####
The expression in an `if` statement must be a `boolean` expression.

### switch Statements ###

#### Legal Expressions for switch and case ####
The general form of the `switch` statement is:

``` java
switch (expression) {  
    case constant1: code block  
    case constant2: code block  
    default: code block    
}  
```

A switch's expression must evaluate to a `char`, `byte`, `short`, `int`, or, as of Java 6, an `enum`. If you're not using an `enum`, only variables and values that can be automatically promoted (in other words, implicitly cast) to an `int` are acceptable.

A `case` constant must evaluate to the same type as the `switch` expression can use, with one additional—and big—constraint: the `case` constant must be a compile time constant!

``` java
final int a = 1;  
final int b;  
b = 2;  
int x = 0;  
switch (x) {  
    case a: // ok  
    case b: // compiler error  
```

What happens if I switch on a variable smaller than an `int`?

``` java
byte g = 2;  
switch(g) {  
    case 23:  
    case 128:  
}  
```

``` dos
Test.java:6: possible loss of precision  
found    : int  
required : byte  
    case 128:  
         ^  
```

It's also illegal to have more than one `case` label using the same value.

It *is* legal to leverage the power of boxing in a `switch` expression.

``` java
switch(new Integer(4)) {  
    case 4: System.out.println("boxing is OK");  
}  
```

#### Break and Fall-Through in switch Blocks ####

**`case` constants are evaluated from the top down, and the first `case` constant that matches the `switch`'s expression is the execution *entry point*.**

When the program encounters the keyword `break` during the execution of a `switch` statement, execution will immediately move out of the `switch` block to the next statement after the `switch`.

#### The Default Case ####
The `default` case doesn’t have to come at the end of the `switch`.
The rule to remember is that `default` works just like any other `case`.

### Creating a switch-case Statement ###

## Loops and Iterators (Exam Objective 2.2) ##

### Using while Loops ###
The `while` loop is good for scenarios where you don't know how many times a block or statement should repeat, but you want to continue looping as long as some condition is true.

In all loops, the expression (test) must evaluate to a `boolean` result.
    
``` java
while (int x = 2) { } // not legal  
```

### Using do Loops ###
The `do` loop is similar to the `while` loop, except that the expression is not evaluated until after the `do` loop's code is executed. Therefore the code in a `do` loop is guaranteed to execute at least once. Use of the semicolon at the end of the `while` expression.

### Using for Loops ###

#### The Basic for Loop ####
The `for` loop is especially useful for flow control when you already know how many times you need to execute the statements in the loop's block. The `for` loop declaration has three main parts:

* Declaration and initialization of variables
* The boolean expression (conditional test)
* The iteration expression

#### The Basic for Loop: Declaration and Initialization ####
The first part of the `for` statement lets you declare and initialize zero, one, or multiple variables of the same type inside the parentheses after the `for` keyword. More than one variable of the same type:

``` java
for (int x = 10, y = 3; y > 3; y++) { }  
```

#### Basic for Loop: Conditional (boolean) Expression ####
Must evaluate to a `boolean` value.
*You can have only one test expression.*

#### Basic for Loop: Iteration Expression ####
After each execution of the body of the `for` loop, the iteration expression is executed.

**Barring a forced exit, evaluating the iteration expression and then evaluating the conditional expression are always the last two things that happen in a `for` loop!**

* `break` Execution jumps immediately to the 1st statement after the `for` loop.
* `return` Execution jumps immediately back to the calling method.
* `System.exit()` All program execution stops; the VM shuts down.

#### Basic for Loop: for Loop Issues ####
None of the three sections of the `for` declaration are required.

``` java
for( ; ; ) {  
    System.out.println("Inside an endless loop");  
}  
```

The absence of the initialization and increment sections, the loop will act like a `while` loop.
It is legal:

``` java
for (int i = 0,j = 0; (i<10) && (j<10); i++, j++) {  
    System.out.println("i is " + i + " j is " +j);  
}  
```

A variable declared in the `for` loop can’t be used beyond the `for` loop. But a variable only initialized in the `for` statement (but declared earlier) can be used beyond the loop. For example:

``` java
int x = 3;  
for (x = 12; x < 20; x++) { }  
System.out.println(x);  
```

Three sections of the for loop are independent of each other.
    
``` java
int b = 3;  
for (int a = 1; b != 1; System.out.println("iterate")) {  
    b = b - a;  
}  
// the output is:  
// iterate  
// iterate  
```

#### The Enhanced for Loop (for Arrays) ####
The enhanced `for` loop, new to Java 6, is a specialized `for` loop that simplifies looping through an array or a collection.
The enhanced `for` has *two* components.

    for(declaration : expression)

The two pieces of the `for` statement are:

* **declaration** The *newly declared* block variable, of a type compatible with the elements of the array you are accessing. This variable will be available within the `for` block, and its value will be the same as the current array element.
* **expression** This must evaluate to the array you want to loop through. This could be an array variable or a method call that returns an array. The array can be any type: primitives, objects, even arrays of arrays.

### Using break and continue ###
The `break` and `continue` keywords are used to stop either the entire loop (`break`) or just the current iteration (`continue`).

`continue` statements must be inside a loop; otherwise, you’ll get a compiler error.

### Unlabeled Statements ###
Both the `break` statement and the `continue` statement can be unlabeled or labeled.

### Labeled Statements ###
A label statement must be placed just before the statement being labeled, and it consists of a valid identifier that ends with a colon (:).
The labeled varieties are needed only in situations where you have a nested loop, and need to indicate which of the nested loops you want to break from, or from which of the nested loops you want to continue with the next iteration.


## Handling Exceptions (Exam Objectives 2.4 and 2.5) ##

### Catching an Exception Using try and catch ###
The `try` is used to define a block of code in which exceptions may occur. This block of code is called a guarded region (which really means "risky code goes here"). One or more `catch` clauses match a specific exception to a block of code that handles it.
If you have one or more `catch` blocks, they must immediately follow the `try` block. The `catch` blocks must all follow each other, without any other statements or blocks in between.

### Using finally ###
Although `try` and `catch` provide a terrific mechanism for trapping and handling exceptions, we are left with the problem of how to clean up after ourselves if an exception occurs. Because execution transfers out of the `try` block as soon as an exception is thrown.
A `finally` block encloses code that is always executed at some point after the `try` block, whether an exception was thrown or not. Even if there is a `return` statement in the `try` block, the `finally` block executes right after the `return` statement is encountered, and before the `return` executes!
The `try` block executes with no exceptions, the `finally` block is executed immediately after the `try` block completes. If there was an exception thrown, the `finally` block executes immediately after the proper `catch` block completes.

### Propagating Uncaught exceptions ###

### Defining Exceptions ###
Every exception is an instance of a class that has class `Exception` in its inheritance hierarchy. In other words, exceptions are always some subclass of `java.lang.Exception`.
When an exception is thrown, an object of a particular `Exception` subtype is instantiated and handed to the exception handler as an argument to the `catch` clause.

### Exception Hierarchy ###
All exception classes are subtypes of class `Exception`.
There are two subclasses that derive from `Throwable`: `Exception` and `Error`. Classes that derive from `Error` represent unusual situations that are not caused by program errors. Errors are technically not exceptions because they do not derive from class `Exception`.
An exception represents something that happens not as a result of a programming error, but rather because some resource is not available or some other condition required for correct execution is not present.

### Handling an Entire Class Hierarchy of Exceptions ###
We've discussed that the `catch` keyword allows you to specify a particular type of exception to catch. You can actually catch more than one type of exception in a single `catch` clause. If the exception class that you specify in the `catch` clause has no subclasses, then only the specified class of exception will be caught.

### Exception Matching ###
When an exception is thrown, Java will try to find (by looking at the available `catch` clauses from the top down) a `catch` clause for the exception type. If it does not find a `catch` clause that matches a supertype for the exception, then the exception is propagated down the call stack.
The handlers for the most specific exceptions must always be placed above those for more general exceptions. Otherwise it won't compile.

The FileNotFoundException was placed above the handler for the IOException. The opposite way, the program will not compile.

### Exception Declaration and the Public Interface ###
As a method must specify what type and how many arguments it accepts and what is returned, the exceptions that a method can throw must be *declared*.

Unless the exceptions are subclasses of `RuntimeException`.

The list of thrown exceptions is part of a method's public interface. The `throws` keyword is used as follows to list the exceptions that a method can throw:

``` java
void myFunction() throws MyException1, MyException2 {  
    // code for the method here  
}  
```

But all non-`RuntimeExceptions` are considered "checked" exceptions, because the compiler checks to be certain you've acknowledged that "bad things could happen here".

*Each method must either handle all checked exceptions by supplying a `catch` clause or list each unhandled checked exception as a thrown exception.*

This rule is referred to as Java's "handle or declare" requirement. (Sometimes called "catch or declare.")

An object of type `RuntimeException` may be thrown from any method without being specified as part of the method's public interface (and a handler need not be present).  
Runtime exceptions are referred to as *unchecked* exceptions. All other exceptions are *checked* exceptions, and they don't derive from `java.lang.RuntimeException`. A checked exception must be caught somewhere in your code. If you invoke a method that throws a checked exception but you don't catch the checked exception somewhere, your code will not compile.

Objects of type `Error` are not `Exception` objects, although they do represent exceptional conditions. Both `Exception` and `Error` share a common superclass, `Throwable`, thus both can be thrown using the `throw` keyword. You can also throw an `Error` yourself.

``` java
class TestEx {  
    public static void main (String [] args) {  
        badMethod();  
    }  
    static void badMethod() { // No need to declare an Error  
        doStuff();  
    }  
    static void doStuff() { //No need to declare an Error  
        try {  
            throw new Error();  
        }  
        catch(Error me) {  
            throw me; // We catch it, but then rethrow it  
        }  
    }  
}  
```

Since `Error` is not a subtype of `Exception`, it doesn't need to be declared.

### Rethrowing the Same Exception ###
All other `catch` clauses associated with the same `try` are ignored, if a `finally` block exists, it runs, and the exception is thrown back to the calling method.

## Common Exceptions and Errors (Exam Objective 2.6) ##

#### Where Exceptions Come From ####

* **JVM exceptions** Those exceptions or errors that are either exclusively or most logically thrown by the JVM.
* **Programmatic exceptions** Those exceptions that are thrown explicitly by application and/or API programmers.


#### JVM Thrown Exceptions ####

* NullPointerException.
* StackOverflowError.

#### Programmatically Thrown Exceptions ####
Created by an application and/or API developer.


## Working with the Assertion Mechanism (Exam Objective 2.3) ##

### Assertions Overview ###
If your assertion turns out to be wrong (`false`), then a stop-the-world `AssertionError` is thrown right then.
Assertions come in two flavors: *really simple* and *simple*, as follows:

Really simple:

``` java
private void doStuff() {  
    assert (y > x);  
    // more code assuming y is greater than x  
}  
```

Simple:

``` java
private void doStuff() {  
    assert (y > x): "y is " + y + " x is " + x;  
    // more code assuming y is greater than x  
}  
```

#### Assertion Expression Rules ####
Assertions can have either one or two expressions, depending on whether you're using the "simple" or the "really simple". The first expression must always result in a boolean value! Follow the same rules you use for `if` and `while` tests. If its result is not `true`, however, then your assumption was wrong and
you get an `AssertionError`.
The second expression, used only with the simple version of an `assert` statement, can be anything that results in a value. The second expression is used to generate a `String` message that displays in the stack trace. It works much like `System.out.println()` in that you can pass it a primitive or an object.

### Enabling Assertions ###

#### Identifier vs. Keyword ####
Prior to version 1.4, you might very well have written code like this:

``` java
int assert = getInitialValue();  
if (assert == getActualResult()) {  
    // do something  
}  
```

**You can use `assert` as a keyword or as an identifier, but not both.**

    javac -source 1.4 com/geeksanonymous/TestClass.java

#### Use Version 6 of java and javac ####

#### Compiling Assertion-Aware Code ####
The Java 6 compiler will use the `assert` keyword by default. Otherwise, the compiler will generate an error message if it finds the word `assert` used as an identifier.

    javac -source 1.3 OldCode.java
    -source 1.6 or -source 6.

#### Enabling Assertions at Runtime ####

    java -ea com.geeksanonymous.TestClass
    java -enableassertions com.geeksanonymous.TestClass

#### Disabling Assertions at Runtime ####

    java -da com.geeksanonymous.TestClass
    java -disableassertions com.geeksanonymous.TestClass

#### Selective Enabling and Disabling ####
The command-line switches for assertions can be used in various ways:

* **With no arguments (as in the preceding examples)** Enables or disables assertions in all classes, except for the system classes.
* **With a package name** Enables or disables assertions in the package specified, and any packages below this package in the same directory hierarchy.
* **With a class name** Enables or disables assertions in the class specified.

Disable assertions in a single class, but keep them enabled for all others:
    
    java -ea -da:com.geeksanonymous.Foo
    java -ea -da:com.geeksanonymous...

Enable assertions in general, but disable them in the package `com.geeksanonymous`, and all of its subpackages.

### Using Assertions Appropriately ###

#### Don't Use Assertions to Validate Arguments to a Public Method ####
A `public` method might be called from code that you don't control. Because `public` methods are part of your interface to the outside world, you're supposed to guarantee that any constraints on the arguments will be enforced by the method itself.
If you need to validate `public` method arguments, you'll probably use exceptions to throw, say, an `IllegalArgumentException` if the values passed to the `public` method are invalid.

#### Do Use Assertions to Validate Arguments to a Private Method ####
When you assume that the logic in code calling your `private` method is correct, you can test that assumption with an assertion as follows:

``` java
private void doMore(int x) {
    assert (x > 0);
    // do things with x
}
```

#### Don't Use Assertions to Validate Command-Line arguments ####

#### Do Use Assertions, Even in Public Methods, to Check for Cases that You Know Are Never, Ever Supposed to Happen ####
This can include code blocks that should never be reached, including the default of
a `switch` statement as follows:

``` java
switch(x) {
    case 1: y = 3; break;
    case 2: y = 9; break;
    case 3: y = 27; break;
    default: assert false; // we're never supposed to get here!
}
```

#### Don't Use Assert Expressions that Can Cause Side Effects! ####
The following would be a very bad idea:

``` java
public void doStuff() {
    assert (modifyThings());
    // continues on
}
public boolean modifyThings() {
    y = x++;
    return true;
}
```









