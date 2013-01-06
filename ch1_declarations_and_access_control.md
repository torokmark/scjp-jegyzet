# Chapter 1: Declaration and Access Control

### Java Refresher
A Java program is mostly a collection of objects talking to other objects by invoking each other's methods.

* **Class** A template that describes the kinds of state and behavior that objects of its type support.
* **Object** At runtime, when the Java Virtual Machine (JVM) encounters the new keyword, it will use the appropriate class to make an object hich is an instance of that class. That object will have its own state, and access to all of the behaviors defined by its class.
* **State (instance variables)** Each object (instance of a class) will have its own unique set of instance variables as defined in the class. Collectively, the values assigned to an object's instance variables make up the object's state.
* **Behavior (methods)** When a programmer creates a class, she creates methods for that class. Methods are where the class' logic is stored. Methods are where the real work gets done. They are where algorithms get executed, and data gets manipulated.

## Identifiers & JavaBeans (Objectives 1.3 and 1.4)
   
### Legal Identifiers 

* Identifiers must start with a letter, a currency character ($), or a connecting character such as the underscore ( _ ). Identifiers cannot start with a number!
* After the first character, identifiers can contain any combination of letters, currency characters, connecting characters, or numbers.
* In practice, there is no limit to the number of characters an identifier can contain.
* No Java keyword can be identifier.
* Identifiers in Java are case-sensitive; foo and FOO are two different identifiers.

#### Legal and illegal identifiers

``` java
    int _a;   
    int $c;  
    int ______2_w;  
    int _$;  
    int this_is_a_very_detailed_name_for_an_identifier;
```

``` java
    int :b;  
    int -d;  
    int e#;  
    int .f;  
    int 7g;  
```

### Code Convetions

* **Classes and interfaces** The first letter should be capitalized, and if several words are linked together to form the name, the first letter of the inner words should be uppercase. (Dog, Account, PrintWriter, Runnable, Serializable)
* **Methods The first letter should be lowercase, and then normal camelCase rules should be used. In addition, the names should typically be verb-noun pairs. (getBalance, doCalculation, setCustomerName)
* **Variables** Like methods, the camelCase format should be used, starting with a lowercase letter. (buttonWidth, accountBalance, myString)
* **Constants** Java constants are created by marking variables static and final. They should be named using uppercase letters with underscore characters as separators: MIN_HEIGHT

### JavaBeans Standards

#### JavaBean Property Naming Rules

* If the property is not a boolean, the getter method's prefix must be get. For example, getSize().
* If the property is a boolean, the getter method's prefix is either get or is. For example, getStopped() or isStopped().
* The setter method's prefix must be set. For example, setSize().
* Setter method signatures must be marked public, with a void return type and an argument that represents the property type.
* Getter method signatures must be marked public, take no arguments, and have a return type that matches the argument type of the setter method for that property.

#### JavaBean Listener Naming Rules

* Listener method names used to "register" a listener with an event source must use the prefix add, followed by the listener type. For example, addActionListener().
* Listener method names used to remove ("unregister") a listener must use the prefix remove, followed by the listener type.
* The type of listener to be added or removed must be passed as the argument to the method.
* Listener method names must end with the word "Listener".

## Declare Classes (Exam Objective 1.1)

### Source File Declaration Rules

* There can be only one public class per source code file.
* Comments can appear at the beginning or end of any line in the source code file.
* If there is a public class in a file, the name of the file must match the name of the public class.
* If the class is part of a package, the package statement must be the first line in the source code file, before any import statements that may be present.
* If there are import statements, they must go between the package statement (if there is one) and the class declaration.
* import and package statements apply to all classes within a source code file.
* A file can have more than one nonpublic class.
* Files with no public classes can have a name that does not match any of the classes in the file.

### Class Declarations and Modifiers

* Access modifiers: public, protected, private.
* Non-access modifiers (including strictfp, final, and abstract).

The fourth access control level (called default or package access) is what you get when you don't use any of the three access modifiers. In other words, every class, method, and instance variable you declare has an access control.

#### Class Access

One class (class A) has access to another class (class B).

* Create an instance of class B.
* Extend class B (in other words, become a subclass of class B).
* Access certain methods and variables within class B, depending on the access control of those methods and variables.

Access means visibility.

#### Default Access

Think of default access as package-level access, because a class with default access can be seen only by classes within the same package.

#### Public Access

A class declaration with the public keyword gives all classes from all packages access to the public class.

#### Other (Nonaccess) Class Modifiers

#### Final Classes

When used in a class declaration, the `final` keyword means the class can't be subclassed.
final gives you the security that nobody can change the implementation out from under you.
Many classes in the Java core libraries are final. For example, the String class.
A benefit of having nonfinal classes is this scenario: Imagine you find a problem
with a method in a class you're using, but you don't have the source code. So you
can't modify the source to improve the method, but you can extend the class and
override the method in your new subclass, and substitute the subclass everywhere
the original superclass is expected. If the class is final, though, then you're stuck.

#### Abstract Classes

An abstract class can never be instantiated. Its sole purpose, mission in life, raison d'être, is to be extended (subclassed).
Imagine you have a class Car that has generic methods common to all vehicles. But you don't want anyone actually creating a generic, abstract Car object.
Notice that the methods marked abstract end in a semicolon rather than curly braces. If the method is in a class—as opposed to an interface—then both the method and the class must be marked abstract.
You can't mark a class as both abstract and final.

## Declare Interfaces (Exam Objectives 1.1 and 1.2)

### Declaring an interface

When you create an interface, you're defining a contract for what a class can do, without saying anything about how the class will do it.
An interface is a contract. Interfaces can be implemented by any class, from any inheritance tree.

Think of an interface as a 100-percent abstract class. Like an abstract class, an interface defines abstract methods.
An abstract class can define both abstract and non-abstract methods, an interface can have only abstract methods. Another way interfaces differ from abstract classes is that interfaces have very little flexibility in how the methods and variables defined in the interface are declared. These rules are strict:

* All interface methods are implicitly public and abstract.
* All variables defined in an interface must be public, static, and final. Interfaces can declare only constants.
* Interface methods must not be static.
* Because interface methods are abstract, they cannot be marked final, strictfp, or native.
* An interface can extend one or more other interfaces.
* An interface cannot extend anything but another interface.
* An interface cannot implement another interface or class.
* An interface must be declared with the keyword interface.
* Interface types can be used polymorphically.

`public abstract interface Rollable { }`

Typing in the abstract modifier is considered redundant; interfaces are implicitly abstract.

Typing in the public and abstract modifiers on the methods is redundant.

### Declaring Interface Constants

You're allowed to put constants in an interface. By doing so, you guarantee that any class implementing the interface will have access to the same constant.

Key rule for interface constants. They must always be `public static final`.
Because interface constants are defined in an interface, they don't have to be declared as public, static, or final. They must be `public`, `static`, and `final`, but you don't have to actually declare them that way.


## Declare Class Members (Objectives 1.3 and 1.4)

### Access Modifiers

Whereas a class can use just two of the four access control levels (default or public), members can use all four:

* `public`
* `protected`
* default
* `private`

If class A has access to a member of class B, it means that class B's member is visible to class A.

#### Public Members

When a method or variable member is declared `public`, it means all other classes, regardless of the package they belong to, can access the member.

#### Private Members

When a member is declared private, a subclass can't inherit it.

#### Protected and Default Members

A default member may be accessed only if the class accessing the member belongs to the same package, whereas a protected member can be accessed (through inheritance) by a subclass even if the subclass is in a different package.

Default and protected behavior differ only when we talk about subclasses. If the protected keyword is used to define a member, any subclass of the class declaring the member can access it through inheritance.

Subclass-outside-the-package to have access to a superclass (parent) member means the subclass inherits the member.
The subclass can see the protected member only through inheritance.

#### Protected Details

For a subclass outside the package, the protected member can be accessed only through inheritance.
Once the subclass-outside-the-package inherits the protected member, that member (as inherited by the subclass) becomes private to any code outside the subclass, with the exception of subclasses of the subclass.

#### Default Details

if you don't type an access modifier before a class or member declaration, the access control is default, which means package level.

#### Local Variables and Access Modifiers

Never a case where an access modifier can be applied to a local variable.
There is only one modifier that can ever be applied to local variables — `final`.

### Nonaccess Member Modifiers

We've discussed member access, which refers to whether code from one class can invoke a method (or access an instance variable) from another class.

#### Final Methods

The final keyword prevents a method from being overridden in a subclass, and is often used to enforce the API functionality of a method.

#### Final Arguments

Method arguments are the variable declarations that appear in between the parentheses in a method declaration. A `final` argument must keep the same value that the parameter had when it was passed into the method.

#### Abstract Methods

An `abstract` method is a method that's been declared (as abstract) but not implemented.
Notice that the abstract method ends with a semicolon instead of curly braces. It is illegal to have even a single abstract method in a class that is not explicitly declared abstract!

Three different clues tell you it's not an `abstract` method:

* The method is not marked `abstract`.
* The method declaration includes curly braces, as opposed to ending in a semicolon. In other words, the method has a method body. 
* The method provides actual implementation code.

Any class that extends an `abstract` class must implement all `abstract` methods of the superclass, unless the subclass is also `abstract`. The rule is this: 
The first concrete subclass of an abstract class must implement all abstract methods of the superclass.
A method can never, ever, ever be marked as both `abstract` and `final`, or both `abstract` and `private` nor `abstract` and `static`.

#### Synchronized Methods

The `synchronized` keyword indicates that a method can be accessed by only one thread at a time.
`synchronized` modifier can be matched with any of the four access control levels.

#### Native Methods

The `native` modifier indicates that a method is implemented in platform-dependent code, often in C.
`native` can be applied only to methods.
Note that a native method's body must be a semicolon (;) (like abstract methods), indicating that the implementation is omitted.

#### Strictfp Methods

Using `strictfp` as a class modifier, but even if you don't declare a class as `strictfp`, you can still declare an individual method as `strictfp`. 
Remember, `strictfp` forces floating points (and any floating-point operations) to adhere to the IEEE 754 standard. With `strictfp`, you can predict how your floating points will behave regardless of the underlying platform the JVM is running on.

#### Methods with Variable Argument Lists (var-args)

Java allows you to create methods that can take a variable number of arguments.

* **arguments** The things you specify between the parentheses when you're invoking a method:
    `doStuff("a", 2); // invoking doStuff, so a & 2 are arguments`
* **parameters** The things in the method's signature that indicate what the method must receive when it's invoked:
    `void doStuff(String s, int a) { }   // we're expecting two parameters: String and int`

Review the declaration rules for var-args:

* **Var-arg type** When you declare a var-arg parameter, you must specify the type of the argument(s) this parameter of your method can receive. (This can be a primitive type or an object type.)
* **Basic syntax** To declare a method using a var-arg parameter, you follow the type with an ellipsis (...), a space, and then the name of the array that will hold the parameters received.
* **Other parameters** It's legal to have other parameters in a method that uses a var-arg.
* **Var-args limits** The var-arg must be the last parameter in the method's signature, and you can have only one var-arg in a method.

### Constructor Declarations

Every class has a constructor, although if you don't create one explicitly, the compiler will build one for you.
A constructors look an awful lot like methods. A key difference is that a constructor can't ever, ever, ever, have a return type ... ever!
Constructor declarations can however have all of the normal access modifiers, and they can take arguments (including var-args), just like methods. 
The other BIG RULE, to understand about constructors is that they must have the same name as the class in which they are declared. Constructors can't be marked `static`, they can't be marked `final` or `abstract`.

### Variable Declarations

There are two types of variables in Java:

* **Primitives** A primitive can be one of eight types: char, boolean, byte, short, int, long, double, or float.
* **Reference variables** A reference variable is used to refer to (or access) an object. A reference variable is declared to be of a specific type and  that type can never be changed. A reference variable can be used to refer to any object of the declared type, or of a *subtype* of the declared type (a compatible type).

#### Declaring Primitives and Primitive Ranges

Primitive variables can be declared as class variables (statics), instance variables, method parameters, or local variables. You can declare one or more primitives, of the same primitive type, in a single line.
For boolean types there is not a range; a boolean can be only true or false.
The char type (a character) contains a single, 16-bit Unicode character.

#### Declaring Reference Variables

Reference variables can be declared as static variables, instance variables, method parameters, or local variables.
You can declare one or more reference variables, of the same type, in a single line.

#### Instance Variables

Instance variables are defined inside the class, but outside of any method, and are only initialized when the class is instantiated. Instance variables are the fields that belong to each unique object.

That instance variables:

* Can use any of the four access levels (which means they can be marked with
any of the three access modifiers)
* Can be marked final
* Can be marked transient
* Cannot be marked abstract
* Cannot be marked synchronized
* Cannot be marked strictfp
* Cannot be marked native
* Cannot be marked static, because then they'd become class variables.

#### Local (Automatic/Stack/Method) Variables

Local variables are variables declared within a method. That means the variable is not just initialized within the method, but also declared within  the method. Just as the local variable starts its life inside the method, it's also destroyed when the method has completed.
Local variables are always on the stack. If the variable is an object reference, the object itself will still be created on the heap.
Local variable declarations can't use most of the modifiers that can be applied to instance variables, such as public (or the other access modifiers), transient, volatile, abstract, or static, but as we saw earlier, local variables can be marked final.
And before a local variable can be used, it must be initialized with a value.

It is possible to declare a local variable with the same name as an instance variable. It's known as shadowing.

#### Array Declarations

Arrays are objects that store multiple variables of the same type, or variables that are all subclasses of the same type. Arrays can hold either primitives or object references, but the array itself will always be an object on the heap.

##### Declaring an Array of Primitives

`int[] key;`  
`int key [];`  

##### Declaring an Array of Object References

`Thread[] threads;`  
`Thread threads [];`

Declare multidimensional arrays, which are in fact arrays of arrays.
`String[][][] occupantName;`
`String[] managerName [];`

It is illegal to include the size of the array in your declaration.
`int[5] scores;`

#### Final Variables
Declaring a variable with the `final` keyword makes it impossible to reinitialize that variable once it has been initialized with an explicit value.
A reference variable marked `final` can't ever be reassigned to refer to a different object. 
The data within the object can be modified, but the reference variable cannot be changed. In other words, a `final` reference still allows you to modify the state of the object it refers to, but you can't modify the reference variable to make it refer to a different object.
There are no `final` objects, only `final` references.

#### Transient Variables
If you mark an instance variable as `transient`, you're telling the JVM to skip (ignore) this variable when you attempt to serialize the object containing it.

#### Volatile Variables

The `volatile` modifier tells the JVM that a thread accessing the variable must always reconcile its own private copy of the variable with the master copy in memory.

#### Static Variables and Methods

The `static` modifier is used to create variables and methods that will exist independently of any instances created for the class.
All `static` members exist before you ever make a new instance of a class, and there will be only one copy of a `static` member regardless of the number of instances of that class.

Things you can mark as static: 

* Methods
* Variables
* A class nested within another class, but not within a method
* Initialization blocks

Things you can't mark as static:

* Constructors (makes no sense; a constructor is used only to create instances)
* Classes (unless they are nested)
* Interfaces
* Method local inner classes
* Inner class methods and instance variables
* Local variables

### Declaring Enums

The basic components of an enum are its constants. Enums can be declared as their own separate class, or as a class member.
They must not be declared within a method!
Enums can be declared as their own class, or enclosed in another class, and that the syntax for accessing an enum's members depends on where the enum was declared.
To make it more confusing for you, the Java language designers made it optional to put a semicolon at the end of the `enum` declaration.

`enum CoffeeSize { BIG, HUGE, OVERWHELMING }; // <--semicolon is optional here`

Each of the enumerated values are represented as static and final.

Because an `enum` really is a special kind of class, you can do more than just list the enumerated constant values. You can add constructors, instance variables, methods, and something really strange known as a constant specific class body.

* Every `enum` has a static method, values(), that returns an array of the enum's values in the order they're declared.
You can NEVER invoke an `enum` constructor directly. The `enum` constructor is invoked automatically, with the arguments you define after the constant value. For example, `BIG(8)`.
* You can define more than one argument to the constructor, and you can overload the `enum` constructors.
