# Inner Classes #

### Inner Classes ###
A class should have code *only* for the things an object of that particular type needs to do; any *other* behavior should be part of another class better suited for *that* job.
The inner class is a part of the outer class. An inner class instance has access to all members of the outer class, *even those marked private*.

### Coding a "Regular" Inner Class ###
We use the term *regular* here to represent inner classes that are not

* Static
* Method-local
* Anonymous

``` java
class MyOuter {  
    class MyInner { }   
}   
```

If you compile it: `%javac MyOuter.java`, you'll end up with *two* class files: `MyOuter.class`, `MyOuter$MyInner.class`.
You can't say 
    
    %java MyOuter$MyInner

in hopes of running the `main()` method of the inner class, because a *regular* inner class can't have static declarations of any kind. *The only way you can access the inner class is through a live instance of the outer class!*

``` java
class MyOuter {  
    private int x = 7;  
    // inner class definition  
    class MyInner {   
        public void seeOuter() {  
            System.out.println("Outer x is " + x);  
        }  
    } // close inner class definition  
} // close outer class  
```

The preceding code is perfectly legal. Notice that the inner class is indeed accessing a private member of the outer class. That's fine, because the inner class is also a member of the outer class. As any member of the outer class can access any other member of the outer class, private or not, the inner class—also a member—can do the same.

#### Instantiating an Inner Class ####
To create an instance of an inner class, *you must have an instance of the outer class* to tie to the inner class.

#### Instantiating an Inner Class from Within the Outer Class ####

``` java
class MyOuter {  
    private int x = 7;  
    public void makeInner() {  
        MyInner in = new MyInner(); // make an inner instance    
        in.seeOuter();   
    }  
    class MyInner {   
        public void seeOuter() {   
            System.out.println("Outer x is " + x);   
        }   
    }   
}  
```

#### Creating an Inner Class Object from Outside the Outer Class Instance Code ####
Without a reference to an instance of the outer class, you can't instantiate the inner class from a `static` method of the outer class.

``` java
public static void main(String[] args) {   
    MyOuter mo = new MyOuter(); // gotta get an instance!   
    MyOuter.MyInner inner = mo.new MyInner();   
    inner.seeOuter();    
}   
```

Or

``` java
public static void main(String[] args) {   
    MyOuter.MyInner inner = new MyOuter().new MyInner();   
    inner.seeOuter();   
}   
```  

Instantiating an inner class is the *only* scenario in which you'll invoke `new` *on* an instance as opposed to invoking `new` to *construct* an instance.
The differences between inner class instantiation code that's *within* the outer class (but not `static`), and inner class instantiation code that's *outside* the outer class:

* From *inside* the outer class instance code, use the inner class name in the normal way:
    
        ``` java
        MyInner mi = new MyInner();
        ```

* From *outside* the outer class instance code (including static method code within the outer class), the inner class name must now include the outer class's name:
    
        ``` java
        MyOuter.MyInner
        ```

    To instantiate it, you must use a reference to the outer class:

        ``` java
        new MyOuter().new MyInner(); or outerObjRef.new MyInner();
        ```
    
    if you already have an instance of the outer class.

### Referencing the Inner or Outer Instance from Within the Inner Class ###

* The keyword `this` can be used only from within instance code. In other words, not within `static` code.
* The `this` reference is a reference to the currently executing object. In other words, the object whose reference was used to invoke the currently running method.
* The `this` reference is the way an object can pass a reference to itself to some other code, as a method argument:

``` java
public void myMethod() {   
    MyClass mc = new MyClass();   
    mc.doStuff(this); // pass a ref to object running myMethod   
}   
```


``` java
class MyOuter {   
    private int x = 7;    
    public void makeInner() {    
        MyInner in = new MyInner();   
        in.seeOuter();    
    }    
    class MyInner {   
        public void seeOuter() {    
            System.out.println("Outer x is " + x);    
            System.out.println("Inner class ref is " + this);    
            System.out.println("Outer class ref is " + MyOuter.this);    
        }    
    }   
    public static void main (String[] args) {    
        MyOuter.MyInner inner = new MyOuter().new MyInner();    
        inner.seeOuter();    
    }     
}   
```

* To reference the inner class instance itself, from *within* the inner class code, use `this`.
* To reference the "*outer* `this`" (the outer class instance) from within the inner class code, use `NameOfOuterClass.this` (example, `MyOuter.this`).

#### Member Modifiers Applied to Inner Classes ####

* `final`
* `abstract`
* `public`
* `private`
* `protected`
* `static` — *but `static` turns it into a `static` nested class not an inner class.*
* `strictfp`


## Method-Local Inner Classes ##

``` java  
class MyOuter2 {  
    private String x = "Outer2";   
    void doStuff() {   
        class MyInner {   
            public void seeOuter() {    
                System.out.println("Outer x is " + x);    
            } // close inner class method    
        } // close inner class definition    
    
        MyInner mi = new MyInner(); // This line must come after the class    
   
        mi.seeOuter();    
    } // close outer class method doStuff()    
} // close outer class       
```

To *use* the inner class you must make an instance of it somewhere *within the method but below the inner class definition*.

### What a Method-Local Inner Object Can and Can't Do ###
*A method-local inner class can be instantiated only within the method where the inner class is defined. he inner class object cannot use the local variables of the method the inner class is in.*
the local variables aren't guaranteed to be alive as long as the method-local inner class object, the inner class object can't use them. *Unless the local variables are marked `final`!*
You can't, for example, mark a method-local inner class `public`, `private`, `protected`, `static`, `transient`, and the like. For the purpose of the exam, the only modifiers you can apply to a method-local inner class are `abstract` or `final`.

    A local class declared in a `static` method has access to only `static` members of the enclosing class, since there is no associated instance of the enclosing class.

## Anonymous Inner Classes ##
Inner classes declared without any class name at all (hence the word *anonymous*).

### Plain-Old Anonymous Inner Classes, Flavor One ###

``` java
class Popcorn {   
    public void pop() {    
        System.out.println("popcorn");    
    }    
}    
class Food {    
    Popcorn p = new Popcorn() {    
        public void pop() {    
            System.out.println("anonymous popcorn");   
        }   
    };    
}   
```

The Popcorn reference variable refers not to an instance of Popcorn, but to *an instance of an anonymous (unnamed) subclass of Popcorn*.

``` java
class Popcorn {
    public void pop() {
       System.out.println("popcorn");
    }
}
class Food {
    Popcorn p = new Popcorn() {
        public void sizzle() {
            System.out.println("anonymous sizzling popcorn");
        }
        public void pop() {
            System.out.println("anonymous popcorn");
        }
    };
    public void popIt() {
        p.pop(); // OK, Popcorn has a pop() method
        p.sizzle(); // Not Legal! Popcorn does not have sizzle()
    }
}
```

It is sad, but the `sizzle()` method is unreachable in the outer class.

### Plain-Old Anonymous Inner Classes, Flavor Two ###
The only difference between flavor one and flavor two is that flavor one creates an anonymous *subclass* of the specified *class* type, whereas flavor two creates an anonymous *implementer* of the specified *interface* type.

``` java
interface Cookable {   
    public void cook();    
}    
class Food {     
    Cookable c = new Cookable() {     
        public void cook() {    
            System.out.println("anonymous cookable implementer");    
        }    
    };    
}    
```

*Anonymous interface implementers can implement only one interface.*

### Argument-Defi ned Anonymous Inner Classes ###

``` java
class MyWonderfulClass {   
    void go() {      
        Bar b = new Bar();    
        b.doStuff(new Foo() {       
            public void foof() {    
                System.out.println("foofy");   
            } // end foof method     
        }); // end inner class def, arg, and b.doStuff stmt.    
    } // end go()    
} // end class    
    
interface Foo {   
    void foof();     
}    
class Bar {      
    void doStuff(Foo f) { }     
}    
```

## Static Nested Classes ##
A static nested class is simply *a class that's a static member of the enclosing class*:

``` java
class BigOuter {    
    static class Nested { }    
}    
```

### Instantiating and Using Static Nested Classes ###

``` java
class BigOuter {   
    static class Nest {void go() { System.out.println("hi"); } }   
}   
class Broom {   
    static class B2 {void goB2() { System.out.println("hi 2"); } }   
    public static void main(String[] args) {   
        BigOuter.Nest n = new BigOuter.Nest(); // both class names   
        n.go();   
        B2 b2 = new B2(); // access the enclosed class   
        b2.goB2();  
    }    
}   
```


