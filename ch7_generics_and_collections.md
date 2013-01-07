# Generics and Collections #

## Overriding hashCode() and equals() (Objective 6.2) ##

#### The toString() Method ####
Override `toString()` when you want a mere mortal to be able to read something meaningful about the objects of your class.
When you don't override the `toString()` method of class Object it gives you the class name followed by the @ symbol, followed by the unsigned hexadecimal representation of the object's hashcode.

### Overriding equals() ###
You saw that the String class and the wrapper classes have overridden the `equals()` method. So that you could compare two different objects (of the same type) to see if their contents are meaningfully equivalent.
When you really need to know if two references are identical, use ==. But when you need to know if the objects themselves (not the references) are equal, use the `equals()` method.

#### What It Means If You Don't Override equals() ####
If you want objects of your class to be used as keys for a hashtable, then you must override `equals()` so that two different instances can be considered the same.

``` java
@Override
public boolean equals(Object o) {
    if ((o instanceof Clazz) && (((Clazz) o).s
            == this.getS())) {
        return true;
    } else {
        return false;
    }
}
```

#### The equals() Contract ####

* It is **reflexive**. For any reference value `x`, `x.equals(x)` should return `true`.
* It is **symmetric**. For any reference values `x` and `y`, `x.equals(y)` should return `true` if and only if `y.equals(x)` returns `true`.
* It is **transitive**. For any reference values `x`, `y`, and `z`, if `x.equals(y)` returns `true` and `y.equals(z)` returns `true`, then `x.equals(z)` must return `true`.
* It is **consistent**. For any reference values `x` and `y`, multiple invocations of `x.equals(y)` consistently return `true` or consistently return `false`, provided no information used in equals comparisons on the object is modified.
* For any non-`null` reference value `x`, `x.equals(null)` should return `false`.

If two objects are considered equal using the `equals()` method, then they must have identical hashcode values. If you override `equals()`, override `hashCode()` as well.

### Overriding hashCode() ###
Hashcodes are typically used to increase the performance of large collections of data. The hashcode value of an object is used by some collection classes. It isn't necessarily unique. Collections such as HashMap and HashSet use the hashcode value of an object to determine how the object should be *stored* in the collection, and the hashcode is used again to help *locate* the object in the collection.

#### Understanding Hashcodes ####
We don't introduce anything random, we simply have an algorithm that will always run the same way given a specific input, so the output will always be identical for any two identical inputs. The hashcode tells you only which bucket to go into, but not how to locate the name once we're in that bucket.

In real-life hashing, it’s not uncommon to have more than one entry in a bucket. Hashing retrieval is a two-step process.

1. Find the right bucket (using `hashCode()` )
2. Search the bucket for the right element (using `equals()` ).

#### Implementing hashCode() ####
Now that we know that two equal objects must have identical hashcodes, is the reverse true? Do two objects with identical hashcodes have to be considered equal?

#### The hashCode() Contract ####

* Whenever it is invoked on the same object more than once during an execution of a Java application, the `hashCode()` method must consistently return the same integer, provided  no information used in `equals()` comparisons on the object is modified. This integer need not remain consistent from one execution of an application to another execution of the same application.
* If two objects are equal according to the `equals(Object)` method, then calling the `hashCode()` method on each of the two objects must produce the same integer result.
* It is NOT required that if two objects are unequal according to the `equals(java.lang.Object)` method, then calling the `hashCode()` method on each of the two objects must produce distinct integer results. However, the programmer should be aware that producing distinct integer results for unequal objects may improve the performance of hashtables.

## Collections (Exam Objective 6.1) ##
The Collections Framework in Java gives you lists, sets, maps, and queues to satisfy most of your coding needs.

### So What Do You Do with a Collection? ###
There are a few basic operations you'll normally use with collections:

* Add objects to the collection.
* Remove objects from the collection.
* Find out if an object (or group of objects) is in the collection.
* Retrieve an object from the collection (without removing it).
* Iterate through the collection, looking at each element (object) one after another.

#### Key Interfaces and Classes of the Collections Framework ####
The core interfaces you need to know for the exam (and life in general) are the following nine:
    Collection, Set, SortedSet, List, Map, SortedMap, Queue, NavigableSet, NavigableMap.

The core concrete implementation classes you need to know for the exam are the following 13:

* **Maps** HashMap, Hashtable, TreeMap, LinkedHashMap
* **Sets** HashSet, LinkedHashSet, TreeSet
* **Lists** ArrayList, Vector, LinkedList
* **Queues** PriorityQueue
* **Utilities** Collections, Arrays

None of the Map-related classes and interfaces extend from Collection.

* collection (lowercase *c*), which represents any of the data structures in which objects are stored and iterated over.
* Collection (capital C), which is actually the java.util.Collection interface from which Set, List, and Queue extend.
* Collections (capital C and ends with *s*) is the java.util.Collections class that holds a pile of `static` utility methods for use with collections.

Collections come in four basic flavors:

* **Lists** *Lists* of things (classes that implement List).
* **Sets** *Unique* things (classes that implement Set).
* **Maps** Things with a *unique* ID (classes that implement Map).
* **Queues** Things arranged by the order in which they are to be processed.

But there are sub-flavors within those four flavors of collections: Sorted, Unsorted, Ordered, Unordered.
An implementation class can be unsorted and unordered, ordered but unsorted, or both ordered and sorted. But an implementation can never be sorted but unordered, because sorting is a specific type of ordering.

#### Ordered ####
When a collection is ordered, it means you can iterate through the collection in a specific (not-random) order. A Hashtable collection is not ordered. An ArrayList, however, keeps the order established by the elements' index position.

#### Sorted ####
A *sorted* collection means that the order in the collection is determined according to some rule or rules, known as the sort order. You put objects into the collection, and the collection will figure out what order to put them in, based on the sort order. A collection that keeps an order (such as any List, which uses insertion order) is not really considered *sorted* unless it sorts using some kind of sort order.
Through an interface (*Comparable*) that defines how instances of a class can be compared to one another.

### List Interface ###
A List cares about the index. The one thing that List has that non-lists don't have is a set of methods related to the index.
`get(int index)`, `indexOf(Object o)`, `add(int index, Object obj)`, and so. All three List implementations are ordered by index position.
    
* **ArrayList** Think of this as a growable array. It gives you fast iteration and fast random access. To state the obvious: it is an ordered collection (by index), but not sorted. ArrayList now implements the new RandomAccess interface — a marker interface.
* **Vector** Vector and Hashtable were the two original collections. Vector is basically the same as an ArrayList, but Vector methods are synchronized for thread safety. You'll normally want to use ArrayList instead of Vector because the synchronized methods add a performance hit you might not need. Vector is the only class other than ArrayList to implement RandomAccess.
* **LinkedList** A LinkedList is ordered by index position, like ArrayList, except that the elements are doubly-linked to one another. This linkage gives you new methods for adding and removing from the beginning or end, which makes it an easy choice for implementing a stack or queue. Good choice when you need fast insertion and deletion. As of Java 5, the LinkedList class has been enhanced to implement the java.util.Queue interface.

### Set Interface ###
A Set cares about uniqueness—it doesn't allow duplicates. Your good friend the `equals()` method determines whether two objects are identical.

* **HashSet** A HashSet is an unsorted, unordered Set. It uses the hashcode of the object being inserted, so the more efficient your `hashCode()` implementation the better access performance you'll get. Use this class when you want a collection with no duplicates and you don't care about order when you iterate through it.
* **LinkedHashSet** A LinkedHashSet is an ordered version of HashSet that maintains a doubly-linked List across all elements. Use this when you care about the iteration order.
* **TreeSet** The TreeSet is one of two sorted collections. It uses a Red-Black tree structure and guarantees that the elements will be in ascending order, according to natural order. As of Java 6, TreeSet implements NavigableSet.

### Map Interface ###
A Map cares about unique identifiers. You map a unique key (the ID) to a specific value, where both the key and the value are, of course, objects. Like Sets, Maps rely on the `equals()` method to determine whether two keys are the same or different.

* **HashMap** The HashMap gives you an unsorted, unordered Map. When you need a Map and you don't care about the order. HashMap allows one `null` key and multiple `null` values in a collection.
* **Hashtable** Hashtable is the synchronized counterpart to HashMap. Hashtable doesn't let you have anything that's `null`.
* **LinkedHashMap** Like its Set counterpart, LinkedHashSet, the LinkedHash - Map collection maintains insertion order.
* **TreeMap** sorted by the natural order of the elements.

### Queue Interface ###
A Queue is designed to hold a list of "to-dos," or things to be processed in some way. Queues are typically thought of as FIFO.

* **PriorityQueue** the LinkedList class has been enhanced to implement the Queue interface, basic queues can be handled with a LinkedList. A PriorityQueue's elements are ordered either by natural ordering or according to a Comparator.


## Using the Collections Framework (Objectives 6.3 and 6.5) ##

### ArrayList Basics ###

Advantages ArrayList has over arrays are 

* It can grow dynamically.
* It provides more powerful insertion and search mechanisms than arrays.

In many ways, `ArrayList<String>` is similar to a `String[]` in that it declares a container that can hold only Strings, but it's more powerful than a `String[]`.

### Autoboxing with Collections ###
In general, collections can hold Objects but not primitives. Prior to Java 5, a very common use for the wrapper classes was to provide a way to get a primitive into a collection.
 
``` java
myInts.add(42); // autoboxing handles it!
```

### Sorting Collections and Arrays ###

#### Sorting Collections ####
ArrayList doesn't give you any way to sort its contents, but the java.util.Collections class does.

``` java
Collections.sort(stuff);
```

#### The Comparable Interface ####
The Comparable interface is used by the Collections.sort() method and the java.util.Arrays.sort() method to sort Lists and arrays of objects, respectively. To implement Comparable, a class must implement a single method, compareTo().

``` java
int x = thisObject.compareTo(anotherObject);
```

The `compareTo()` method returns an int with the following characteristics:

* negative If thisObject < anotherObject
* zero If thisObject == anotherObject
* positive If thisObject > anotherObject

The `sort()` method uses `compareTo()` to determine how the List or object array should be sorted.

It’s important to remember that when you override `equals()` you MUST take an argument of type `Object`, but that when you override `compareTo()` you should take an argument of the type you’re sorting.

#### Sorting with Comparator ####
While you were looking up the `Collections.sort()` method you might have noticed that there is an overloaded version of `sort()` that takes both a List AND something called a *Comparator*. The Comparator interface gives you the capability to sort a given collection any number of different ways. The Comparator interface is also very easy to implement, having only one method, `compare()`.
The `Comparator.compare()` method returns an `int` whose meaning is the same as the `Comparable.compareTo()` method's return value.

#### Sorting with the Arrays Class ####
The `Arrays.sort()` method is overridden in the same way the `Collections.sort()` method is.

* `Arrays.sort(arrayToSort)`
* `Arrays.sort(arrayToSort, Comparator)`

The `Arrays.sort()` methods that sort primitives always sort based on natural order.
The `sort()` methods for both the Collections class and the Arrays class are `static` methods, and that they alter the objects they are sorting, instead of returning a different sorted object.

#### Searching Arrays and Collections ####
The Collections class and the Arrays class both provide methods that allow you to search for a specific element.

* Searches are performed using the `binarySearch()` method.
* Successful searches return the `int` index of the element being searched.
* Unsuccessful searches return an `int` index that represents the insertion point. The *insertion* point is the place in the collection/array where the element would be inserted to keep the collection/array properly sorted.
* The collection/array being searched must be sorted before you can search it.
* If you attempt to search an array or collection that has not already been sorted, the results of the search will not be predictable.
* If the collection/array you want to search was sorted in natural order, it *must* be searched in natural order.
* If the collection/array you want to search was sorted using a Comparator, it *must* be searched using the same Comparator passed as the second argument to the `binarySearch()` method. Comparators cannot be used when searching arrays of primitives.

#### Using Lists ####
Lists are usually used to keep things in some kind of order. You can use a LinkedList to create a first-in, first-out queue. You can use an ArrayList to keep track of what locations were visited, and in what order.
The two Iterator methods you need to understand for the exam are:

* **boolean hasNext()** Returns `true` if there is at least one more element in the collection being traversed. Invoking `hasNext()` does NOT move you to the next element of the collection.
* **Object next()** This method returns the next object in the collection, AND moves you forward to the element after the element just returned.

#### Using Sets ####
Remember that Sets are used when you don't want any duplicates in your collection. If you attempt to add an element to a set that already exists in the set, the duplicate element will not be added, and the `add()` method will return `false`.
HashSets tend to be very fast because, as we discussed earlier, they use hashcodes. You can also create a TreeSet, which is a Set whose elements are sorted. The issue is that whenever you want a collection to be sorted, its elements must be mutually comparable.

#### Using Maps ####
Remember that when you use a class that implements Map, any classes that you use as a part of the keys for that map must override the `hashCode()` and `equals()` methods.

1. Use the `hashCode()` method to find the correct bucket
2. Use the `equals()` method to find the object in the bucket

### Navigating (Searching) TreeSets and TreeMaps ###
Let's turn our attention to searching TreeSets and TreeMaps. Java 6 introduced (among others) two new interfaces: `java.util.NavigableSet` and `java.util.NavigableMap`.
The NavigableSet methods related to this type of navigation are `lower()`, `floor()`, `higher()`, `ceiling()`, and the mostly parallel NavigableMap methods are `lowerKey()`, `floorKey()`, `ceilingKey()`, and `higherKey()`. The difference between `lower()` and `floor()` is that `lower()` returns the element less than the given element, and `floor()` returns the element less than *or equal to* the given element.

### Other Navigation Methods ###

#### Polling ####
The idea of polling is that we want *both* to retrieve *and* remove an element from either the beginning or the end of a collection. In TreeSet: pollFirst(), pollLast() returns and removes the first entry in the set, in TreeMap: pollFirstEntry(), pollLastEntry() to retrieve and remove key-value pairs.

#### Descending Order ####
The important methods for the exam are TreeSet.descendingSet() and TreeMap.descendingMap().

### Backed Collections ###
The `subMap()` method is making a copy of a portion of the TreeMap.

#### Using the PriorityQueue Class ####
PriorityQueue orders its elements using a user-defined priority. PriorityQueue can be ordered using a Comparator, which lets you define any ordering you want. Queues have a few methods not found in other collection interfaces: `peek()`, `poll()`, and `offer()`. `offer()` method adds element to the PriorityQueue. `poll()` method returns the highest priority entry AND removes the entry from the queue. `peek()` returns the highest priority element in the queue without removing it.

## Generic Types (Objectives 6.3 and 6.4) ##

#### The Legacy Way to Do Collections ####
A non-generic collection can hold any kind of object! A non-generic collection is quite happy to hold anything that is NOT a primitive.

### Generics and Legacy Code ###

### Mixing Generic and Non-generic Collections ###
You can mix correct generic code with older non-generic code, and everyone is happy.
Now imagine that you call a legacy method that doesn't just *read* a value but *adds* something to the ArrayList?
Sure, this code works. It compiles, and it runs.
If we pass String to the method like this:

``` java
List list = new LinkedList();
list.add(new String("42"));
```

Sadly, it works! 
The older legacy code was allowed to put anything at all (except primitives) into a collection.

### Polymorphism and Generics ###

``` java
List<Integer> myList = new ArrayList<Integer>();
```

We are able to assign an ArrayList to a List reference, because List is a supertype of ArrayList. This polymorphic assignment works the way it always works in Java.

``` java
class Parent { }  
class Child extends Parent { }  
List<Parent> myList = new ArrayList<Child>();  
```

It will not compile. The type of the variable declaration must match the type you pass to the actual object type. 
Polymorphism does not work the same way for generics as it does with arrays.

### Generic Methods ###
ArrayList<Dog> cannot be passed into a method with an argument of ArrayList<Animal>.
You simply CANNOT assign the individual ArrayLists of Animal subtypes (<Dog>, <Cat>, or <Bird>) to an ArrayList of the supertype <Animal>, which is the declared type of the argument.

We have two real issues:

1. Why doesn't this work?
2. How do you get around it?

``` java
List<Animal> animals = new ArrayList<Animal>();  
animals.add(new Cat()); // OK  
animals.add(new Dog()); // OK  
```

This part works with both arrays and generic collections—we can add an instance of a subtype into an array or collection declared with a supertype.

``` java
public void foo() {  
    Cat[] cats = {new Cat(), new Cat()};  
    addAnimal(cats); // no problem, send the Cat[] to the method  
}

public void addAnimal(Animal[] animals) {
    animals[0] = new Dog(); // Eeek! We just put a Dog in a Cat array!
}
```

If you passed in an array of an Animal subtype (Cat, Dog, or Bird), the compiler does not know. The compiler does not realize that out on the heap somewhere is an array of type `Cat[]`, not `Animal[]`, and you're about to try to add a Dog to it.

``` java
public void addAnimal(List<Animal> animals) {  
    animals.add(new Dog()); // this is always legal, since Dog can  
                            // be assigned to an Animal reference  
}  
public static void main(String[] args) {  
    List<Animal> animals = new ArrayList<Animal>();  
    animals.add(new Dog());  
    animals.add(new Dog());  
    AnimalDoctorGeneric doc = new AnimalDoctorGeneric();  
    doc.addAnimal(animals); // OK, since animals matches  
    // the method arg  
}  
```

It won't compile, because the method was invoked with wrong arguments.
We need is a way to tell the compiler, "Hey, I'm using the collection passed in just to invoke methods on the elements — and I promise not to ADD anything into the collection."
And there IS a mechanism to tell the compiler that you can take any generic subtype of the declared argument type because you won't be putting anything in the collection. And that mechanism is the wildcard `<?>`.

``` java
public void addAnimal(List< ? extends Animal > animals);
```

By saying `<? extends Animal>`, we're saying, "I can be assigned a collection that is a subtype of List and typed for <Animal> or anything that extends Animal. And oh yes, I SWEAR that I will not ADD anything into the collection."

``` java
public void addAnimal(List< ? extends Animal> animals) {
    animals.add(new Dog()); // NO! Can't add if we use < ? extends Animal>
}
```

First, the `<? extends Animal>` means that you can take any subtype of Animal; however, that subtype can be EITHER a subclass of a class (abstract or concrete) OR a type that implements the interface after the word `extends`. The keyword `extends` in the context of a wildcard represents BOTH subclasses and interface implementations. There is no `<? implements Serializable>` syntax.
There is only ONE wildcard keyword that represents both interface implementations and subclasses. And that keyword is `extends`.

``` java
public void addAnimal(List< ? super Dog> animals)
```

Hey compiler, please accept any List with a generic type that is of type Dog, or a supertype of Dog. Nothing lower in the inheritance tree can come in, but anything higher than Dog is OK.

`List<?>`, which is the wildcard `<?>` without the keywords extends or super, simply means "any type". That means any type of List can be assigned to the argument. Using the wildcard alone, without the keyword `super` means that you cannot ADD anything to the list referred to as `List<?>`.
`List<Object>` means that the method can take ONLY a `List<Object>`.

``` java
class Bar {  
    void doInsert(List< ? > list) {  
        list.add( new Dog() );  
    }  
}  
```

Bar class won't compile because it does an `add()` in a method that uses a wildcard (without `super`).

`List<? extends Object>` and `List<?>` are absolutely identical!

### Generic Declarations ###

#### Making Your Own Generic Class ####

``` java
public class TestGenerics<T> { // as the class type  
    T anInstance; // as an instance variable type  
    T [] anArrayOfTs; // as an array type  
  
    TestGenerics(T anInstance) { // as an argument type  
        this.anInstance = anInstance;   
    }  
  
    T getT() { // as a return type   
        return anInstance;   
    }   
}   
```

More then one type parameter:

``` java
public class UseTwo<T, X> {  
    T one;   
    X two;   
    UseTwo(T one, X two) {   
        this.one = one;   
        this.two = two;   
    }   
   
    T getT() { return one; }   
    X getX() { return two; }   
       
    // test it by creating it with <String, Integer>   
    public static void main (String[] args) {   
        UseTwo<String, Integer> twos = new UseTwo<String, Integer>("foo", 42);   
   
        String theT = twos.getT(); // returns a String   
        int theX = twos.getX(); // returns Integer, unboxes to int   
    }   
}   
```

You can use a form of wildcard notation in a class definition, to specify a range (called "bounds") for the type that can be used for the type parameter:

``` java
public class AnimalHolder<T extends Animal> { // use "T" instead of "?"   
    T animal;   
    public static void main(String[] args) {   
        AnimalHolder<Dog> dogHolder = new AnimalHolder<Dog>(); // OK    
        AnimalHolder<Integer> x = new AnimalHolder<Integer>(); // NO!   
    }   
}   
```

#### Creating Generic Methods ####

Imagine you want to create a method that takes an instance of any type, instantiates an ArrayList of that type, and adds the instance to the ArrayList. The class itself doesn't need to be generic;

``` java
import java.util.*;  
  
public class CreateAnArrayList {   
    public <T> void makeArrayList(T t) { // take an object of an unknown type and use a "T" to represent the type   
        List<T> list = new ArrayList<T>(); // now we can create the list using "T"   
        list.add(t);   
    }   
}   
```

If you invoke the method with an Integer, then the T is replaced by Integer.
The strangest thing about generic methods is that you must declare the type variable BEFORE the return type of the method:

``` java
public <T> void makeArrayList(T t)
```

The <T> before `void` simply defines what T is before you use it as a type in the argument.

You're also free to put boundaries on the type you declare:

``` java
public <T extends Number> void makeArrayList(T t)
```

What about constructor arguments? They, too, can be declared with a generic type, but then it looks even stranger since constructors have no return type at all:

``` java
public class Radio {
    public <T> Radio(T t) { } // legal constructor
}
```

There is no naming conflict between class names, type parameter placeholders, and variable identifiers:

``` java
class X { public <X> X(X x) { } }
```

While the question mark works when declaring a reference for a variable, it does NOT work for generic class and method declarations.

