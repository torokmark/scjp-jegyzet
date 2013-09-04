# Chapter 2: Object Orientation

## Encapsulation (Exam Objective 5.1)

Want to hide implementation details behind a public programming interface. By interface, we mean the set of accessible methods your code makes available for other code to call—in other words, your code's API. By hiding implementation details, you can rework your method code without forcing a change in the code that calls your changed method.

If you want maintainability, flexibility, and extensibility, your design must include encapsulation. 

* Keep instance variables protected
* Make public accessor methods, and force calling code to use those methods rather than directly accessing the instance variable.
* For the methods, use the JavaBeans naming convention of `set<someProperty>` and `get<someProperty>`.

## Inheritance, Is-A, Has-A (Exam Objective 5.5)

Every class in Java is a subclass of class Object, (except of course class Object itself).
You'll always have an equals method, a clone method, notify, wait, and others, available to use.

The methods of the `Object` class can be grouped as follows:

* Methods that are usually overriden
    * `toString`
    * `equal`
    * `hashCode`
* Methods that are responsible for synchronization
    * `wait` -  Three overloaded `wait` methods belong to the `Object` class
    * `notify`
    * `notifyAll`
* Methods that we never override
    * `clone` - sometimes (later we are returning to this topic)
    * `finalize`
    * `getClass`

You can create inheritance relationships in Java by extending a class.

* To promote code reuse
* To use polymorphism

### IS-A

The concept of IS-A is based on class inheritance or interface implementation. IS-A is a way of saying, "this thing is a type of that thing."
You express the IS-A relationship in Java through the keywords `extends` (for *class* inheritance) and `implements` (for *interface* implementation).

### HAS-A

HAS-A relationships are based on usage, rather than inheritance. In other words, class A HAS-A B if code in class A has a reference to an instance of class B. For example, you can say the following, A Horse IS-A Animal. A Horse HAS-A Halter.

## Polymorphism (Exam Objective 5.2)

Any Java object that can pass more than one IS-A test can be considered polymorphic. Other than objects of type Object, all Java objects are polymorphic in that they pass the IS-A test for their own type and for class Object.

The only way to access an object is through a reference variable:

* A reference variable can be of only one type, and once declared, that type can never be changed (although the object it references can change).
* A reference is a variable, so it can be reassigned to other objects, (unless the reference is declared final).
* A reference variable's type determines the methods that can be invoked on the object the variable is referencing.
* A reference variable can refer to any object of the same type as the declared reference, or **it can refer to any** *subtype* **of the declared type!**
* A reference variable can be declared as a class type or an interface type. If the variable is declared as an interface type, it can reference any object of any class that *implements* the interface.

A class cannot *extend* more than one class. That means one parent per class. A class can have multiple ancestors.
The compiler only knows about the declared reference type, the Java Virtual Machine (JVM) at runtime knows what the object really is.

**Polymorphic method invocations apply only to *instance methods*. You can always refer to an object with a more general reference variable type (a superclass or interface), but at runtime, the ONLY things that are dynamically selected based on the actual *object* (rather than the *reference* type) are instance methods. Not *static* methods. Not *variables*. Only overridden instance methods are dynamically invoked based on the real object's type.**

## Overriding / Overloading (Exam Objectives 1.5 and 5.4)

### Overridden Methods

Any time you have a class that inherits a method from a superclass, you have the opportunity to override the method (unless, as you learned earlier, the method is marked `final`).

For abstract methods you inherit from a superclass, you have no choice. You *must* implement the method in the subclass **unless the subclass is also abstract**. Abstract methods must be *implemented* by the concrete subclass.

Polymorphism lets you use a more abstract supertype (including an interface) reference to refer to one of its subtypes (including interface implementers).
The overriding method cannot have a more restrictive access modifier than the method being overridden.
You can't override a method marked `public` and make it `protected`.

The rules for overriding a method are as follows:

* The argument list must exactly match that of the overridden method. If they don't match, you can end up with an overloaded method you didn't intend.
* The return type must be the same as, or a subtype of, the return type declared in the original overridden method in the superclass.
* The access level can't be more restrictive than the overridden method's.
* The access level CAN be less restrictive than that of the overridden method.
* Instance methods can be overridden only if they are inherited by the subclass. A subclass within the same package as the instance's superclass can override any superclass method that is not marked `private` or `final`. A subclass in a different package can override only those non-final methods marked `public` or `protected`.
* The overriding method CAN throw any unchecked (runtime) exception, regardless of whether the overridden method declares the exception.
* The overriding method must NOT throw checked exceptions that are new or broader than those declared by the overridden method. For example, a method that declares a FileNotFoundException cannot be overridden by a method that declares a SQLException, Exception, or any other non-runtime exception unless it's a subclass of FileNotFoundException.
* The overriding method can throw narrower or fewer exceptions. Just because an overridden method "takes risks" doesn't mean that the overriding subclass' exception takes the same risks.

* You cannot override a method marked final.
* You cannot override a method marked static.
* If a method can't be inherited, you cannot override it.

#### Invoking a Superclass Version of an Overridden Method

It's easy to do in code using the keyword `super`.

### Overloaded Methods

Overloaded methods let you reuse the same method name in a class, but with different arguments.
The rules:

* Overloaded methods MUST change the argument list.
* Overloaded methods CAN change the return type.
* Overloaded methods CAN change the access modifier.
* Overloaded methods CAN declare new or broader checked exceptions.
* A method can be overloaded in the same class or in a subclass.

## Reference Variable Casting (Objective 5.2)

What happens when you want to use that `animal` reference variable to invoke a method that only class `Dog` has?

``` java  
Animal animal = new Dog();  
  
animal.playDead();  // try to do a Dog behavior ?  
cannot find symbol  

Dog d = (Dog) animal; // casting the ref. var.  
d.playDead();    
```

In this case is sometimes called a downcast, because we're casting down the inheritance tree to a more specific class.

Unlike downcasting, upcasting (casting up the inheritance tree to a more general type) works implicitly because when you upcast you're implicitly restricting the number of methods you can invoke.
implicit upcast is always legal for assigning an object of a subtype to a reference of one of its supertype classes (or interfaces).

## Implementing an Interface (Exam Objective 1.2)

When you implement an interface, you're agreeing to adhere to the contract defined in the interface. That means you're agreeing to provide legal implementations for every method defined in the interface, and that anyone who knows what the interface methods look like can rest assured that they can invoke those methods on an instance of your implementing class.

A nonabstract implementation class must do the following:

* Provide concrete (nonabstract) implementations for all methods from the declared interface.
* Follow all the rules for legal overrides.
* Declare no checked exceptions on implementation methods other than those declared by the interface method, or subclasses of those declared by the interface method.
* Maintain the signature of the interface method, and maintain the same return type (or a subtype). (But it does not have to declare the exceptions declared in the interface method declaration.)

1. A class can implement more than one interface.
2. An interface can itself extend another interface, but never implement anything.

## Legal Return Types (Exam Objective 1.5)

### Return Type Declarations

#### Return Types on Overloaded Methods

Method overloading is not much more than name reuse. What you can't do is change *only* the return type. To overload a method, remember, you must change the argument list.

#### Overriding and Return Types, and Covariant Returns

You're allowed to change the return type in the overriding method as long as the new return type is a *subtype* of the declared return type of the overridden (superclass) method.

``` java
class Alpha {  
    Alpha doStuff(char c) {  
        return new Alpha();  
    }  
}  
  
class Beta extends Alpha {  
    Beta doStuff(char c) { // legal override in Java 1.5  
        return new Beta();  
    }  
}  
```

    javac -source 1.4 Beta.java


### Returning a Value

Six rules for returning a value:

1. You can return `null` in a method with an object reference return type.
2. An array is a perfectly legal return type.
3. In a method with a primitive return type, you can return any value or variable that can be implicitly converted to the declared return type.
4. In a method with a primitive return type, you can return any value or variable that can be explicitly cast to the declared return type.
5. You must not return anything from a method with a void return type.
6. In a method with an object reference return type, you can return any object type that can be implicitly cast to the declared return type.


## Constructors and Instantiation (Exam Objectives 1.6, 5.3, and 5.4)

Objects are constructed. You can't make a new object without invoking a constructor.
Constructors are the code that runs whenever you use the keyword `new`.

#### Constructor Basics

Every class, *including abstract classes*, MUST have a constructor.
They have no return type and their names must exactly match the class name.

#### Constructor Chaining

What really happens when you say `new Horse()` ?

1. Horse constructor is invoked. Every constructor invokes the constructor of its superclass with an (implicit) call to super(), unless the constructor invokes an overloaded constructor of the same class.
2. Animal constructor is invoked.
3. Object constructor is invoked.
4. Object instance variables are given their explicit values. By *explicit* values, we mean values that are assigned at the time the variables are declared
5. Object constructor completes.
6. Animal instance variables are given their explicit values
7. Animal constructor completes.
8. Horse instance variables are given their explicit values
9. Horse constructor completes.

#### Rules for Constructors

* Constructors can use any access modifier, including private.
* The constructor name must match the name of the class.
* Constructors must not have a return type.
* It's legal to have a method with the same name as the class, but that doesn't make it a constructor.
* If you don't type a constructor into your class code, a default constructor will be automatically generated by the compiler.
* The default constructor is ALWAYS a no-arg constructor.
* If you want a no-arg constructor and you've typed any other constructor(s) into your class code, the compiler won't provide the no-arg constructor for you.
* Every constructor has, as its first statement, either a call to an overloaded constructor (this()) or a call to the superclass constructor (super()).
* If you do type in a constructor, and you do not type in the call to super() or a call to this(), the compiler will insert a no-arg call to super() for you.
* A call to super() can be either a no-arg call or can include arguments passed to the super constructor.
* A no-arg constructor is not necessarily the default (i.e., compiler-supplied)
constructor. You're free to put in your own no-arg constructor.
* You cannot make a call to an instance method, or access an instance variable, until after the super constructor runs.
* Only static variables and methods can be accessed as part of the call to `super()` or `this()`.
* Abstract classes have constructors, and those constructors are always called when a concrete subclass is instantiated.
* Interfaces do not have constructors. Interfaces are not part of an object's inheritance tree.
* The only way a constructor can be invoked is from within another constructor. You can't write code that actually calls a constructor as follows:

``` java
class Horse {  
    Horse() { } // constructor  
    void doStuff() {  
        Horse(); // calling the constructor - illegal!  
    }  
}  
```

### Determine Whether a Default Constructor Will Be Created

#### How do you know for sure whether a default constructor will be created?
Because you didn't write any constructors in your class.

#### How do you know what the default constructor will look like?

* The default constructor has the same access modifier as the class.
* The default constructor has no arguments.
* The default constructor includes a no-arg call to the super constructor (`super()`).

#### What happens if the super constructor has arguments?
Constructors can have arguments just as methods can, and if you try to invoke a method that takes, but you don't pass anything to the method, the compiler will complain

**Constructors are never inherited.**

### Overloaded Constructors

Overloading a constructor means typing in multiple versions of the constructor, each having a different argument list.
Since there's already a constructor in this class, the compiler won't supply a default constructor.
Overloading a constructor is typically used to provide alternate ways for clients to instantiate objects of your class.

**Key Rule: The first line in a constructor must be a call to super() or a call to this().**

The preceding rule means a constructor can never have both a call to `super()` and a call to `this()`.
Because each of those calls must be the first statement in a constructor, you can't legally use both in the same constructor.
only two constructors in the class both have calls to this(), and in fact you'll get exactly what you'd get if you typed the following method code:

``` java
public void go() {  
    doStuff();  
}  
public void doStuff() {  
    go();  
}  
```

The stack explodes.

    Exception in thread "main" java.lang.StackOverflowError


## Statics (Exam Objective 1.3)

### Static Variables and Methods

The method's behavior has no dependency on the state (instance variable values) of an object.
Variables and methods marked `static` belong to the class, rather than to any particular instance. In fact, you can use a `static` method or variable without having any instances of that class at all.
if there are instances, a `static` variable of a class will be shared by all instances of that class
A `static` method can't access a nonstatic (instance) variable, a `static` method can't directly invoke a nonstatic method.

#### Accessing Static Methods and Variables
Finally, remember that *static methods can't be overridden*. This doesn't mean they can't be redefined in a subclass.

## Coupling and Cohesion (Exam Objective 5.1)

The goals for an application are:

* Ease of creation
* Ease of maintenance
* Ease of enhancement

#### Coupling
Coupling is the degree to which one class knows about another class.
If the only knowledge that class A has about class B, is what class B has exposed through its interface, then class A and class B are said to be loosely coupled.
If A knows more than it should about the way in which B was implemented, then A and B are tightly coupled.

#### Cohesion
Cohesion is all about how a single class is designed. The term *cohesion* is used to indicate the degree to which a class has a single, well-focused purpose.
