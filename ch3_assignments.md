# Assignments

### Stack and Heap -- Quick Review

The various pieces (methods, variables, and objects) of Java programs live in one of two places in memory: the stack or the heap.

* Instance variables and objects live on the heap.
* Local variables live on the stack.

## Literals, Assignments, and Variables (Exam Objectives 1.3 and 7.6)

#### Literal Values for All Primitive Types

A primitive literal is merely a source code representation of the primitive data types.

#### Integer Literals
There are three ways to represent integer numbers in the Java language: decimal (base 10), octal (base 8), and hexadecimal (base 16).

#### Decimal Literals

#### Octal Literals
Octal integers use only the digits 0 to 7. You represent an integer in octal form by placing a zero in front of the number.

#### Hexadecimal Literals
Hexadecimal (hex for short) numbers are constructed using 16 distinct symbols. Because we never invented single digit symbols for the numbers 10 through 15, we use alphabetic characters to represent these digits.
    
    0 1 2 3 4 5 6 7 8 9 a b c d e f

Including the prefix 0x.

``` java
int x = 0X0001;
int y = 0x7fffffff;
int z = 0xDeadCafe;
```

All three integer literals are defined as `int` by default.

#### Floating-Point Literals
Floating-point literals are defined as double (64 bits) by default. You must attach the suffix F or f to the number.

``` java
float f = 23.467890;           // Compiler error, possible loss of precision
float g = 49837849.029847F;    // OK; has the suffix "F"
```

Optionally attach a D or d to double literals, but it is not necessary because this is the default behavior.

``` java
double d = 110599.995011D;  // Optional, not required
double g = 987.897;         // No 'D' suffix, but OK because the literal is a double by default
```

#### Boolean Literals
Boolean literals are the source code representation for boolean values. A boolean value can only be defined as `true` or `false`.

#### Character Literals
A char literal is represented by a single character in single quotes.

``` java
char a = 'a';  
char b = '@';
char letterN = '\u004E';   // The letter 'N'
char a = 0x892;             // hexadecimal literal
char b = 982;               // int literal
char c = (char)70000;       // The cast is required; 70000 is out of char range
char d = (char) -98;        // Ridiculous, but legal
```

#### Literal Values for Strings
A string literal is a source code representation of a value of a String object.

The only other nonprimitive type that has a literal representation is an array.

#### Assignment Operators 
Simply assign the stuff on the right side of the = to the variable on the left.
Variables are just bit holders, with a designated type. You can have an int holder, a double holder, a Button holder, and even a String[] holder. Within that holder is a bunch of bits representing the value.

A reference variable bit holder contains bits representing a way to get to the object.
All we can say for sure is that the variable's value is *not* the object, but rather a value representing a specific object on the heap. Or `null`.

#### Primitive Assignments 
The most important point to remember is that a literal integer is always implicitly an int.
A byte-sized holder can't hold as many bits as an int-sized holder.

``` java
byte b = 27;
byte b = (byte) 27;    // Explicitly cast the int literal to a byte
```

``` java  
byte a = 3;  
byte b = 8;  
byte c = b + c;  
```

Won't compile! `...possible loss of precision...`
We tried to assign the sum of two bytes to a byte variable, the result of which (11) was definitely small enough to fit into a byte, but the compiler didn't care. It knew the rule about int-or-smaller expressions always resulting in an int. It would have compiled if we'd done the *explicit* cast:

``` java
byte c = (byte) (a + b);
```

#### Primitive Casting
Casts can be implicit or explicit. An implicit cast means you don't have to write code for the cast; the conversion happens automatically. Typically, an implicit cast happens when you're doing a widening conversion. In other words, putting a smaller thing (say, a `byte`) into a bigger container (like an `int`).
The large-value-into-small-container conversion is referred to as *narrowing* and requires an explicit cast, where you tell the compiler that you're aware of the danger and accept full responsibility.

##### Assigning Floating-Point Numbers
Every floating-point literal is implicitly a `double` (64 bits)

##### Assigning a Literal That Is Too Large for the Variable
When you narrow a primitive, Java simply truncates the higher-order bits that won't fit. In other words, it loses all the bits to the left of the bits you're narrowing to.

##### Assigning One Primitive Variable to Another Primitive Variable
When you assign one primitive variable to another, the contents of the right-hand variable are copied.

#### Reference Variable Assignments
You can assign a newly created object to an object reference variable as follows:
    
``` java
Button b = new Button();
```

* Makes a reference variable named `b`, of type `Button`
* Creates a new `Button` object on the heap
* Assigns the newly created `Button` object to the reference variable `b`

You can also assign `null` to an object reference variable, which simply means the
variable is not referring to any object.

#### Variable Scope

* Static variables have the longest scope; they are created when the class is
loaded, and they survive as long as the class stays loaded in the Java Virtual
Machine
* Instance variables are the next most long-lived; they are created when a new
instance is created, and they live until the instance is removed.
* Local variables are next; they live as long as their method remains on the
stack.
* Block variables live only as long as the code block is executing

* Attempting to access an instance variable from a static context
* Attempting to access a local variable from a nested method.
* Attempting to use a block variable after the code block has completed.

### Using a Variable or Array Element That Is Uninitialized and Unassigned

Java gives us the option of initializing a declared variable or leaving it uninitialized.
Local variables are sometimes called stack, temporary, automatic, or method
variables, but the rules for these variables are the same regardless of what you
call them. Although you can leave a local variable uninitialized, the compiler
complains if you try to use a local variable before initializing it with a value.

#### Primitive and Object Type Instance Variables
Instance variables (also called *member* variables) are variables defined at the class level.
Instance variables are initialized to a default value each time a new instance is created.

#### Primitive Instance Variables

#### Object Reference Instance Variables
A `null` value means the reference variable is not referring to any object on the heap.

#### Array Instance Variables
An array is an object; thus, an array instance variable that's declared but not explicitly initialized will have a value of `null`, just as any other object reference instance variable.
All array elements are given their default values — the same default values that elements of that type get when they're instance variables.
*Array elements are always, always, always given default values, regardless of where the array itself is declared or instantiated.*

### Local (Stack, Automatic) Primitives and Objects

Local variables are defined within a method, and they include a method's parameters.

#### Local Primitives
Local variables, including primitives, always, always, always must be initialized before you attempt to use them.

#### Local Object References
Objects references, too, behave differently when declared within a method rather than as instance variables. With instance variable object references, you can get away with leaving an object reference uninitialized, as long as the code checks to make sure the reference isn't `null` before using it.
To the compiler `null` is a value.
You can't use the dot operator on a `null` reference, because *there is no object at the other end of it*, but a `null` reference is not the same as an *uninitialized* reference.

#### Local Arrays
Just like any other object reference, array references declared within a method must be assigned a value before use. Array elements are given their default values.

#### Assigning One Reference Variable to Another
With primitive variables, an assignment of one variable to another means the contents (bit pattern) of one variable are *copied* into another. Object reference variables work exactly the same way.
If we assign an existing instance of an object to a new reference variable, then two reference variables will hold the same bit pattern - a bit pattern referring to a specific object on the heap.
In Java, String objects are given special treatment. For one thing, String objects are immutable; you can't change the value of it.
*Any time we make any changes at all to a String, the VM will update the reference variable to refer to a different object.*

When you use a String reference variable to modify a string:

* A new string is created leaving the original String object untouched.
* The reference used to modify the String is then assigned the brand new String object.

## Passing Variables into Methods (Objective 7.3)

### Passing Object Reference Variables

When you pass an object variable into a method, you must keep in mind that you're passing the object reference, and not the actual object itself. A reference variable holds bits that represent (to the underlying VM) a way to get to a specific object in memory (on the heap).

### Does Java Use Pass-By-Value Semantics?
It makes no difference if you're passing primitive or reference variables, you are always passing a copy of the bits in the variable. So for a primitive variable, you're passing a copy of the bits representing the value. And if you're passing an object reference variable, you're passing a copy of the bits representing the reference to an object.

## Array Declaration, Construction, and Initialization (Exam Objective 1.3)

Arrays are objects in Java that store multiple variables of the same type. Arrays can hold either primitives or object references, but the array itself will always be an object on the heap.

### Declaring an Array

Arrays are declared by stating the type of element the array will hold, which can be an object or a primitive, followed by square brackets to the left or right of the identifier.

``` java
int[] key; // brackets before name (recommended)
int key []; // brackets after name (legal but less readable)
```
  
``` java
Thread[] threads; // Recommended
Thread threads[]; // Legal but less readable
```

We can also declare multidimensional arrays, which are in fact arrays of arrays.

``` java
String[][][] occupantName; // recommended
String[] ManagerName []; // yucky, but legal
```

It is never legal to include the size of the array in your declaration.

### Constructing an Array

Constructing an array means creating the array object on the *heap*, doing a `new` on the array type. To create an array object, Java must know how much space to allocate on the heap, so you must specify the size of the array at creation time.

#### Constructing One-Dimensional Arrays

The most straightforward way to construct an array is to use the keyword new followed by the array type, with a bracket specifying how many elements of that type the array will hold.

#### Constructing Multidimensional Arrays

Multidimensional arrays are simply arrays of arrays.

``` java
int[][] myArray = new int[3][];
```

Notice that only the first brackets are given a size.

### Initializing an Array
Initializing an array means putting things into it. A reference that has not had an object assigned to it is a `null` reference.
The individual elements in the array can be accessed with an index number. The index number always begins with zero.

#### Initializing Elements in a Loop
Array objects have a single public variable, `length` that gives you the number of elements in the array.

#### Declaring, Constructing, and Initializing on One Line
You can use two different array-specific syntax shortcuts to both initialize and construct in a single statement.

``` java  
int x = 9;  
int[] dots = { 6, x, 8 };  
```

* Declares an int array reference variable named dots.
* Creates an int array with a length of three (three elements).
* Populates the array's elements with the values 6, 9, and 8.
* Assigns the new array object to the reference variable dots.

### Constructing and Initializing an Anonymous Array

Anonymous array creation:

``` java
int[] testScores;  
testScores = new int[] { 4, 7, 2 };  
```

#### Legal Array Element Assignments
Arrays can have only one declared type.

#### Arrays of Primitives
Primitive arrays can accept any value that can be promoted implicitly to the declared type of the array.

#### Arrays of Object References
If the declared array type is a class, you can put objects of any subclass of the declared type into the array.
If the array is declared as an interface type, the array elements can refer to any instance of any class that implements the declared interface.

#### Array Reference Assignments for One-Dimensional Arrays
All arrays are objects, so an `int` array reference cannot refer to an `int` primitive.
An array declared as an interface type can reference an array of any type that implements the interface.

#### Array Reference Assignments for Multidimensional Arrays
When you assign an array to a previously declared array reference, the array you're assigning must be the same dimension as the reference you're assigning it to.

### Initialization Blocks
Initialization blocks are the third place - beside methods and constructors - in a Java program where operations can be performed. Initialization blocks run when the class is first loaded (a static initialization block) or when an instance is created.
They don't have names, they can't take arguments, and they don't return anything. A *static* initialization block runs *once*, when the class is first loaded. An *instance* initialization block runs once *every time a new instance is created*. Instance init block code runs right after the call to `super()` in a constructor, in other words, after all super-constructors have run.
*The order in which initialization blocks appear in a class matters*. When it's time for initialization blocks to run, if a class has more than one, they will run in the order in which they appear in the class file.

## Using Wrapper Classes and Boxing (Exam Objective 3.1)

The wrapper classes in the Java API serve two primary purposes:

* To provide a mechanism to "wrap" primitive values in an object so that the primitives can be included in activities reserved for objects.
* To provide an assortment of utility functions for primitives. Most of these functions are related to various conversions: converting primitives to and from String objects.

### An Overview of the Wrapper Classes
There is a wrapper class for every primitive in Java.

### Creating Wrapper Objects
There are three most common approaches for creating wrapper objects. Some approaches take a String representation of a primitive as an argument. *Wrapper objects are immutable.*

#### The Wrapper Constructors
All of the wrapper classes except Character provide two constructors: one that takes a primitive of the type being constructed, and one that takes a String representation of the type being constructed.

``` java
Integer i1 = new Integer(42);
Integer i2 = new Integer("42");
```

The constructors for the Boolean wrapper take either a `boolean` value `true` or `false`, or a String.

#### The valueOf() Methods
The two static `valueOf()` methods provided in most of the wrapper classes give you another approach to creating wrapper objects. Both methods take a String representation of the appropriate type of primitive as their first argument, the second method takes an additional argument, `int radix`, which indicates in what base the first argument is represented.

### Using Wrapper Conversion Utilities

#### xxxValue()
When you need to convert the value of a wrapped numeric to a primitive, use one of the many `xxxValue()` methods. Each of the six numeric wrapper classes has six methods, so that any numeric wrapper can be converted to any primitive numeric type.

#### parseXxx() and valueOf()
The six `parseXxx()` methods (one for each numeric wrapper type) are closely related to the `valueOf()` method that exists in all of the numeric wrapper classes. Both `parseXxx()` and `valueOf()` take a String as an argument, throw a NumberFormatException if the String argument is not properly formed.
The difference between the two methods is:

* `parseXxx()` returns the named primitive.
* `valueOf()` returns a newly created wrapped object of the type that invoked the method.

#### toString()
Class Object, the alpha class, has a `toString()` method. All other Java classes have a `toString()` method. 
The idea of the `toString()` method is to get some meaningful representation of a given object.
All of the numeric wrapper classes provide an overloaded, `static toString()` method that takes a primitive numeric of the appropriate type (`Double.toString()` takes a `double`, `Long.toString()` takes a `long`, and so on) returns a String.
Integer and Long provide a third toString() method. It's static, its first argument is the primitive, and its second argument is a radix.

#### toXxxString() (Binary, Hexadecimal, Octal)
The Integer and Long wrapper classes let you convert numbers in base 10 to other bases.

### Autoboxing
Autoboxing, auto-unboxing, boxing, and unboxing. Boxing and unboxing make using wrapper classes more .convenient.
Before Java 5:

``` java
Integer y = new Integer(567); // make it  
int x = y.intValue(); // unwrap it  
x++; // use it  
y = new Integer(x); // re-wrap it  
System.out.println("y = " + y); // print it  
```

Since Java 5 has released:

``` java
Integer y = new Integer(567); // make it  
y++; // unwrap it, increment it,  
// rewrap it  
System.out.println("y = " + y); // print it  
```

#### Boxing, ==, and equals()

In order to save memory, two instances of the following wrapper objects (created through boxing), will always be == when their primitive values are the same:

* Boolean
* Byte
* Character from \u0000 to \u007f (7f is 127 in decimal)
* Short and Integer from -128 to 127

When == is used to compare a primitive to a wrapper, the wrapper will be unwrapped and the comparison will be primitive to primitive.

## Overloading (Exam Objectives 1.5 and 5.4)

#### Overloading Made Hard—Method Matching
To take a look at three factors that can make overloading a little tricky:

* Widening
* Autoboxing
* Var-args

When a class has overloaded methods, one of the compiler's jobs is to determine which method to use whenever it finds an invocation for the overloaded method.
*In every case, when an exact match isn't found, the JVM uses the method with the smallest argument that is wider than the parameter.*

#### Overloading with Boxing and Var-args

``` java
class AddBoxing {  
    static void go(Integer x) { System.out.println("Integer"); }  
    static void go(long x) { System.out.println("long"); }  
    public static void main(String [] args) {  
        int i = 5;  
        go(i); // which go() will be invoked?  
    }  
}  
```

Does the compiler think that widening a primitive parameter is more desirable than performing an autoboxing operation? The answer is that the compiler will choose widening over boxing, so the output will be `long`.

``` java
class AddVarargs {  
    static void go(int x, int y) { System.out.println("int,int");}  
    static void go(byte... x) { System.out.println("byte... "); }  
    public static void main(String[] args) {  
        byte b = 5;  
        go(b,b); // which go() will be invoked?  
    }  
}  
```

The output is `int,int`.

The compiler will choose the older style before it chooses the newer style, keeping existing code more robust. So far we've seen that:

* Widening beats boxing
* Widening beats var-args

Does boxing beat var-args?

``` java
class BoxOrVararg {  
    static void go(Byte x, Byte y) {} System.out.println("Byte, Byte"); }  
    static void go(byte... x) { System.out.println("byte... "); }  
    public static void main(String [] args) {  
        byte b = 5;  
        go(b,b); // which go() will be invoked?  
    }  
}  
```

The output is `Byte, Byte`.

Widening, boxing, var-args!

#### Widening Reference Variables
The reference widening depends on inheritance, in other words the IS-A test. Because of this, it's not legal to widen from one wrapper class to another, because the wrapper classes are peers to one another.

#### Overloading When Combining Widening and Boxing
What happens when more than one conversion is required, say the compiler will have to widen and then autobox the parameter for a match.

``` java
class WidenAndBox {  
    static void go(Long x) { System.out.println("Long"); }  
    public static void main(String [] args) {  
        byte b = 5;  
        go(b); // must widen then box - illegal  
    }  
}  
```

This is just too much for the compiler. But:

``` java
class BoxAndWiden {  
    static void go(Object o) {  
        Byte b2 = (Byte) o; // ok - it's a Byte object  
        System.out.println(b2);  
    }  
    public static void main(String [] args) {  
        byte b = 5;  
        go(b); // can this byte turn into an Object ?  
    }  
}
```

The output is `5`.

#### Overloading in Combination with Var-args
You can successfully combine var-args with either widening or boxing.
Rules for overloading methods using widening, boxing, and var-args:

* Primitive widening uses the "smallest" method argument possible.
* Used individually, boxing and var-args are compatible with overloading.
* You CANNOT widen from one wrapper type to another. (IS-A fails.)
* You CANNOT widen and then box. (An `int` can't become a Long.)
* You can box and then widen. (An `int` can become an Object, via Integer.)
* You can combine var-args with either widening or boxing.

## Garbage Collection (Exam Objective 7.4)

### Overview of Memory Management and Garbage Collection

### Overview of Java's Garbage Collector

It's typical for memory to be used to create a stack, a heap, in Java's case constant pools, and method areas. The heap is that part of memory where Java objects live, and it's the one and only part of memory that is in any way involved in the garbage collection process.
*A heap is a heap is a heap. For the exam it's important to know that you can call it the heap, you can call it the garbage collectible heap, or you can call it Johnson, but there is one and only one heap.*

#### When Does the Garbage Collector Run?
The garbage collector is under the control of the JVM. The JVM decides when to run the garbage collector.

#### How Does the Garbage Collector Work?
You just can't be sure. The Java specification doesn't guarantee any particular implementation.
You might hear that the garbage collector uses reference counting; once again maybe yes maybe no.
*An object is eligible for garbage collection when no live thread can access it.*

When GC discovers an object that can't be reached by any live thread, it will consider that object as eligible for deletion, and it might even delete it at some point.

### Writing Code That Explicitly Makes Objects Eligible for Collection

#### Nulling a Reference
The first way to remove a reference to an object is to set the reference variable that refers to the object to `null`.

#### Reassigning a Reference Variable
We can also decouple a reference variable from an object by setting the reference variable to refer to another object.
When a method is invoked, any local variables created exist only for the duration of the method. Once the method has returned, the objects created in the method are eligible for garbage collection. If an object is returned from the method, its reference might be assigned to a reference variable in the method that called it, hence, it will not be eligible for collection.

#### Isolating a Reference
There is another way in which objects can become eligible for garbage collection, even if they still have valid references!
A simple example is a class that has an instance variable that is a reference variable to another instance of the same class.

``` java
public class Island {  
    Island i;  
    public static void main(String [] args) {  
        Island i2 = new Island();  
        Island i3 = new Island();  
        Island i4 = new Island();  
        
        i2.i = i3; // i2 refers to i3  
        i3.i = i4; // i3 refers to i4  
        i4.i = i2; // i4 refers to i2  
          
        i2 = null;  
        i3 = null;  
        i4 = null;  
        // do complicated, memory intensive stuff  
    }  
}  
```  

When the code reaches `// do complicated`, the three Island objects have instance variables so that they refer to each other, but their links to the outside world (`i2`, `i3`, and `i4`) have been nulled.
These three objects are eligible for garbage collection.

#### Forcing Garbage Collection
Garbage collection cannot be forced. 
In reality, it is possible only to suggest to the JVM that it perform garbage collection. However, there are no guarantees the JVM will actually remove all of the unused objects from memory
The garbage collection routines that Java provides are members of the Runtime class. The Runtime class is a special class that has a single object (a Singleton) for each main program. To get the Runtime instance use the method Runtime.getRuntime(), which returns the Singleton. Once you have the Singleton you can invoke the garbage collector using the `gc()` method.
The simplest way to ask for garbage collection is `System.gc();`. After calling `System.gc()`, you will have as much free memory as possible.

#### Cleaning Up Before Garbage Collection — the finalize() Method
Java provides you a mechanism to run some code just before your object is deleted by the garbage collector. This code is located in a method named `finalize()` that all classes inherit from class Object.

#### Tricky Little finalize() Gotcha's
There are a couple of concepts concerning `finalize()` that you need to remember.

* For any given object, `finalize()` will be called only once (at most) by the garbage collector.
* Calling `finalize()` can actually result in saving an object from deletion.
