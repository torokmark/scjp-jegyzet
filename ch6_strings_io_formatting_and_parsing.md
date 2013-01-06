# Strings, I/O, Formatting, and Parsing #

## String, StringBuilder, and StringBuffer (Exam Objective 3.1) ##

### The String Class ###

##### Strings Are Immutable Objects #####
Each character in a string is a 16-bit Unicode character.
Strings are objects. Just like other objects, you can create an instance of a String with the `new` keyword, as follows:

	``` java
	String s = new String();  
	s = "abcdef";  
	String s = new String("abcdef");  

	String s = "abcdef";  
	String s2 = s; // refer s2 to the same String as s  
	```

Once you have assigned a String a value, that value can never change— it's immutable.
The good news is that while the String object is immutable, its reference variable is not, so to continue with our previous example:
	`s = s.concat(" more stuff");`

Let's look at what really happened:
The VM took the value of String s (which was "`abcdef`"), and tacked "` more stuff`" onto the end, giving us the value "`abcdef more stuff`". Since Strings are immutable, the VM couldn't stuff this new value into the old String referenced by s, so it created a new String object, gave it the value "`abcdef more stuff`", and made s refer to it. Technically there are now three String objects, because the literal argument to concat, "` more stuff`", is itself a new String object.

##### Important Facts About Strings and Memory #####
When the compiler encounters a String literal, it checks the pool to see if an identical String already exists. If a match is found, the reference to the new literal is directed to the existing String, and no new String literal object is created. Now we can start to see why making String objects immutable is such a good idea. If several reference variables refer to the same String without even knowing it, it would be very bad if any of them could change the String's value.

### Important Methods in the String Class ###

* charAt() Returns the character located at the specified index
* concat() Appends one String to the end of another ( "+" also works)
* equalsIgnoreCase() Determines the equality of two Strings, ignoring case
* length() Returns the number of characters in a String
* replace() Replaces occurrences of a character with a new character
* substring() Returns a part of a String
* toLowerCase() Returns a String with uppercase characters converted
* toString() Returns the value of a String
* toUpperCase() Returns a String with lowercase characters converted
* trim() Removes whitespace from the ends of a String

### The StringBuffer and StringBuilder Classes ###
The java.lang.StringBuffer and java.lang.StringBuilder classes should be used when you have to make a lot of modifications to strings of characters. String objects are immutable, so if you choose to do a lot of manipulations with String objects, you will end up with a lot of abandoned String objects in the String pool.

##### StringBuffer vs. StringBuilder #####
StringBuffer class has exactly the same API as the StringBuffer class, except StringBuilder is not thread safe. In other words, its methods are not synchronized. StringBuilder will run faster.


## File Navigation and I/O (Exam Objective 3.2) ##

* **File** The API says that the class File is "An abstract representation of file and directory pathnames". The File class isn't used to actually read or write data; it's used to work at a higher level, making new empty files, searching for files, deleting files, making directories, and working with paths.

* **FileReader** This class is used to read character files. Its `read()` methods are fairly low-level, allowing you to read single characters, the whole stream of characters, or a fixed number of characters. FileReaders are usually wrapped by higher-level objects such as BufferedReaders, which improve performance and provide more convenient ways to work with the data.

* **BufferedReader** This class is used to make lower-level Reader classes like FileReader more efficient and easier to use. Compared to FileReaders, BufferedReaders read relatively large chunks of data from a file at once, and keep this data in a buffer. When you ask for the next character or line of data, it is retrieved from the buffer, which minimizes the number of times that time-intensive, file read operations are performed. In addition, BufferedReader provides more convenient methods such as `readLine()`, that
allow you to get the next line of characters from a file.

* **FileWriter** This class is used to write to character files. Its `write()` methods allow you to write character(s) or Strings to a file. FileWriters are usually wrapped by higher-level Writer objects such as BufferedWriters or PrintWriters, which provide better performance and higher-level, more flexible methods to write data.

* **BufferedWriter** This class is used to make lower-level classes like FileWriters more efficient and easier to use. Compared to FileWriters, BufferedWriters write relatively large chunks of data to a file at once, minimizing the number of times that slow, file writing operations are performed. The BufferedWriter class also provides a `newLine()` method to create platform-specific line separators automatically.

* **PrintWriter** This class has been enhanced significantly in Java 5. Because of newly created methods and constructors (like building a PrintWriter with a File or a String), you might find that you can use PrintWriter in places where you previously needed a Writer to be wrapped with a FileWriter and/or a BufferedWriter. New methods like `format()`, `printf()`, and `append()` make PrintWriters very flexible and powerful.

* **Console** This new, Java 6 convenience class provides methods to read input from the console and write formatted output to the console.

##### Creating Files Using Class File #####
Objects of type File are used to represent the actual files.
When you make a new instance of the class File, *you're not yet making an actual file, you're just creating a filename*.

##### Using FileWriter and FileReaders #####
When you write data out to a stream, some amount of buffering will occur, and you never know for sure exactly when the last of the data will actually be sent. You might perform many write operations on a stream before closing it, and invoking the `flush()` method guarantees that the last of the data you thought you had already written actually gets out to the file. Whenever you're done using a file, either reading it or writing to it, you should invoke the `close()` method. When you are doing file I/O you're using expensive and limited operating system resources, and so when you're done, invoking `close()` will free up those resources.

##### Combining I/O classes #####
Java's entire I/O system was designed around the idea of using several classes in combination. Combining I/O classes is sometimes called *wrapping* and sometimes called *chaining*. The java.io package contains about 50 classes, 10 interfaces, and 15 exceptions.

How we'll hook them together:

* We know that ultimately we want to hook to a File object. So whatever other class or classes we use, one of them must have a constructor that takes an object of type File.
* Find a method that sounds like the most powerful, easiest way to accomplish the task. BufferedWriter has a `newLine()` method. PrintWriter has a method called `println()`.
* When we look at PrintWriter's constructors, we see that we can build a PrintWriter object if we have an object of type File, so all we need to do to create a PrintWriter object.

##### Working with Files and Directories #####

* **`delete()`** You can't delete a directory if it's not empty.
* **`renameTo()`** You must give the existing File object a valid new File object with the new name that you want. (If `newName` had been `null` we would have gotten a NullPointerException.)
* **`renameTo()`** It's okay to rename a directory, even if it isn't empty.

### The java.io.Console Class ###

The `Console` class makes it easy to accept input from the command line, both echoed and nonechoed (such as a password), and makes it easy to write formatted output to the command line.
The `readLine` method returns a string containing whatever the user keyed in. The `readPassword` method doesn't return a string: it returns a character array. If a string was returned, it could exist in a pool somewhere in memory and perhaps some nefarious hacker could find it.

## Serialization (Exam Objective 3.3) ##
Serialization lets you simply say "save this object and all of its instance variables".
Unless a variable marked as `transient`, which means, don't include the transient variable's value as part of the object's serialized state.

##### Working with ObjectOutputStream and ObjectInputStream #####
The magic of basic serialization happens with just two methods: one to serialize objects and write them to a stream, and a second to read the stream and deserialize objects.

``` java
ObjectOutputStream.writeObject() // serialize and write
ObjectInputStream.readObject() // read and deserialize
```

The java.io.ObjectOutputStream and java.io.ObjectInputStream classes are considered to be *higher*-level classes in the java.io package. That means that you'll wrap them around *lower*-level classes, such as java.io.FileOutputStream and java.io.FileInputStream.

##### Object Graphs #####
If the instance variables are all primitive types, it's pretty straightforward. But what if the instance variables are themselves references to *objects*? The reference would be useless.
what would happen if we didn't have access to the Collar class source code? Obviously we could subclass the Collar class, mark the subclass as Serializable, and then use the Collar subclass instead of the Collar class. But that's not always an option either for several potential reasons:

* The Collar class might be final, preventing subclassing.
* The Collar class might itself refer to other non-serializable objects, and without knowing the internal structure of Collar, you aren't able to make all these fixes.
* Subclassing is not an option for other reasons related to your design.

##### Using writeObject and readObject #####
Java serialization has a special mechanism just for this—a set of private methods you can implement in your class that, if present, will be invoked automatically during serialization and deserialization.
The two special methods you define must have signatures that look EXACTLY like this:

``` java
private void writeObject(ObjectOutputStream os) {
	// your code for saving the Collar variables
}
private void readObject(ObjectInputStream is) {
	// your code to read the Collar state, create a new Collar,
	// and assign it to the Dog
}
```

##### How Inheritance Affects Serialization #####
If a superclass is Serializable, then according to normal Java interface rules, all subclasses of that class automatically implement Serializable implicitly. 	

When an object is constructed using `new` the following things happen:

1. All instance variables are assigned default values.
2. The constructor is invoked, which immediately invokes the superclass constructor.
3. All superclass constructors complete.
4. Instance variables that are initialized as part of their declaration are assigned their initial value.
5. The constructor completes.

*But these things do NOT happen when an object is deserialized.* When an instance of a serializable class is deserialized, the constructor does not run, and instance variables are NOT given their initially assigned values!
If you have variables marked transient, they will not be restored to their original state (unless you implement `readObject()`), but will instead be given the default value for that data type.

If you are a serializable class, but your superclass is NOT serializable, then any instance variables you INHERIT from that superclass will be reset to the values they were given during the original construction of the object. This is because the nonserializable class constructor WILL run!
If you serialize a collection or an array, every element must be serializable! A single non-serializable element will cause serialization to fail. Note also that while the collection interfaces are not serializable, the concrete collection classes in the Java API are.

##### Serialization Is Not for Statics #####
We've talked ONLY about instance variables, not static variables.


## Dates, Numbers, and Currency (Exam Objective 3.4) ##

### Working with Dates, Numbers, and Currencies ###
You'll need to be familiar with at least four classes from the java.text and java.util packages.
The four date related classes:

* **`java.util.Date`** Most of this class's methods have been deprecated, but you can use this class to bridge between the `Calendar` and `DateFormat` class. An instance of `Date` represents a mutable date and time, to a millisecond.
* **`java.util.Calendar`** This class provides a huge variety of methods that help you convert and manipulate dates and times.
* **`java.text.DateFormat`** This class is used to format dates also to format dates for numerous locales around the world.
* **`java.text.NumberFormat`** This class is used to format numbers and currencies for locales around the world.
* **`java.util.Locale`** This class is used in conjunction with `DateFormat` and `NumberFormat` to format dates, numbers and currency for specific locales.

##### Orchestrating Date- and Number-Related Classes #####

##### The Date Class #####

##### The Calendar Class #####
The Calendar class is designed to make date manipulation easy! That it's an abstract class.

``` java
Calendar c = new Calendar(); // illegal, Calendar is abstract
```

You have to use one of the overloaded `getInstance()` static factory methods:

``` java
Calendar cal = Calendar.getInstance();
```

When you get a Calendar reference like cal, from above, your Calendar reference variable is actually referring to an instance of a concrete subclass of Calendar.
The `roll()` method acts like the `add()` method, except that when a part of a Date gets incremented or decremented, larger parts of the Date will not get incremented or decremented.

##### The DateFormat Class #####
Having learned how to create dates and manipulate them, let's find out how to format them.
The API for DateFormat.parse() explains that by default, the `parse()` method is lenient when parsing dates. Our experience is that `parse()` isn't very lenient about the formatting of Strings it will successfully parse into dates.


##### The Locale Class #####
The Locale class is your ticket to worldwide domination. Both the DateFormat class and the NumberFormat class can use an instance of Locale to customize formatted output to be specific to a locale.
There are a couple more methods in Locale (`getDisplayCountry()` and `getDisplayLanguage()`) that you'll have to know for the exam.

##### The NumberFormat Class #####
Like the DateFormat class, NumberFormat is abstract, so you'll typically use some version of either `getInstance()` or `getCurrencyInstance()` to create a NumberFormat object.


## Parsing, Tokenizing, and Formatting (Exam Objective 3.5) ##

### A Search Tutorial ###
Regular expressions (regex for short) are a kind of language within a language, designed to help programmers with searching tasks. Every language that provides regex capabilities uses one or more regex *engines*. Regex engines search through textual data using instructions that are coded into *expressions*.

##### When Metacharacters and Strings Collide #####

``` java
String pattern = "\d"; // compiler error!

String pattern = "\\d"; // a compilable metacharacter

String p = "."; // regex sees this as the "." metacharacter
String p = "\."; // the compiler sees this as an illegal Java escape sequence
String p = "\\."; // the compiler is happy, and regex sees a dot, not a metacharacter
```

### Tokenizing ###
Tokenizing is the process of taking big pieces of source data, breaking them into little pieces, and storing the little pieces in variables. Probably the most common tokenizing situation is reading a delimited file in order to get the contents of the file moved into useful places like objects, arrays or collections.

### Formatting with printf() and format() ###
Here's a rundown of the format string elements you'll need to know for the exam:

* **arg_index** An integer followed directly by a $, this indicates which argument should be printed in this position.
* **flags** While many flags are available, for the exam you'll need to know:
	* "-" Left justify this argument
	* "+" Include a sign (+ or -) with this argument
	* "0" Pad this argument with zeroes
	* "," Use locale-specific grouping separators (i.e., the comma in 123,456)
	* "(" Enclose negative numbers in parentheses
* **width** This value indicates the minimum number of characters to print.
* **precision** For the exam you'll only need this when formatting a floating-point number, and in the case of floating point numbers, precision indicates the number of digits to print after the decimal point.
* **conversion** The type of argument you'll be formatting. (b boolean, c char, d integer, f floating point, s string).




