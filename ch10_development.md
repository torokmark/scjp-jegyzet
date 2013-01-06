# Development #

## Using the javac and java Commands ##

### Compiling with javac ###

The `javac` command is used to invoke Java's compiler.

#### Compiling with -d ###
By default, the compiler puts a `.class` file in the same directory as the `.java` source file. This is fine for very small projects, but once you're working on a project of any size at all, you'll want to keep your `.java` files separated from your `.class` files. The `-d` option lets you tell the compiler in which directory to put the `.class` file(s) it generates.

### Launching Applications with java ###

The `java` command is used to invoke the Java virtual machine.

    java [options] class [args]

#### Using System Properties ####

``` java
import java.util.*;  
public class TestProps {  
    public static void main(String[] args) {  
        Properties p = System.getProperties();  
        p.setProperty("myProp", "myValue");  
        p.list(System.out);  
    }  
}  
```

    java -DcmdProp=cmdVal TestProps

The output is:

```
...
os.name=Mac OS X
myProp=myValue
...
java.specification.vendor=Sun Microsystems Inc.
user.language=en
java.version=1.6.0_05
...
cmdProp=cmdVal
...
```

*The name and value are sometimes called the key and the property.*
`myProp=myValue` was added via the `setProperty` method, and `cmdProp=cmdVal` was added via the `-D` option at the command line.


#### Handling Command-Line Arguments ####

Like all arrays, `args` index is zero based.

The following are all legal declarations for `main()`:

* `static public void main(String[] args)`
* `public static void main(String... x)`
* `static public void main(String bang_a_gong[])`

### Searching for Other Classes ###

Both `java` and `javac` use the same basic search algorithm:

1. They both have the same list of places (directories) they search, to look for classes.
2. They both search through this list of directories in the same order.
3. As soon as they find the class they're looking for, they stop searching for that class. In the case that their search lists contain two or more files with the same name, the first file found will be the file that is used.
4. The first place they look is in the directories that contain the classes that come standard with J2SE.
5. The second place they look is in the directories defined by classpaths.
6. Classpaths should be thought of as "class search paths." They are lists of directories in which classes might be found.
7. There are two places where classpaths can be declared:
    * A classpath can be declared as an operating system environment variable.
    * The classpath declared here is used by default, whenever `java` or `javac` are invoked.

*Classpaths declared as command-line options override the classpath declared as an environment variable, but they persist only for the length of the invocation.*

#### Declaring and Using Classpaths ####

Classpaths consist of a variable number of directory locations, separated by delimiters.

    -classpath /com/foo/acct:/com/foo

specifies two directories in which classes can be found: `/com/foo/acct` and `/com/foo`.
A very common situation occurs in which `java˛` or `javac` complains that it can't find a class file, and yet you can see that the file is IN the current directory! When searching for class files, the `java` and `javac` commands don't search the current directory by default. You must tell them to search there.

    -classpath /com/foo/acct:/com/foo:.

Classpaths are searched from left to right. `java` command allows you to abbreviate `-classpath` with `-cp`.

#### Packages and Searching ####

You start to put classes into packages, and then start to use classpaths to find these classes.

``` java
package com.foo;
public class MyClass { 
    public void hi() { } 
}
```

Once a class is in a package, the package part of its fully qualified name is *atomic*.
You can't split it up on the command line, and you can't split it up in an `import` statement.

``` java
package com.foo;
public class MyClass { 
    public void hi() { } 
}
```

``` java
import com.foo.MyClass; // either import will work
import com.foo.*;
public class Another {
    void go() {
        MyClass m1 = new MyClass(); // alias name
        com.foo.MyClass m2 = new com.foo.MyClass(); // full name
        m1.hi();
        m2.hi();
    }
}
```

The `import` statement is like an alias for the class's fully qualified name. You define the fully qualified name for the class with an `import` statement.

#### Relative and Absolute Paths ####

A classpath is a collection of one or more paths. Each path in a classpath is an absolute path or a relative path. An absolute path in Unix begins with a forward slash (/) (on Windows it would be something like c:\ ).
A relative path is one that does NOT start with a slash.

## JAR Files ##

Once you've built and tested your application, you might want to bundle it up so that it's easy to distribute and easy for other people to install. One mechanism that Java provides for these purposes is a JAR file.
You can create a single JAR file that contains all of the files in myApp, and also maintains myApp's directory structure. Once this JAR file is created, it can be moved from place to place, and from machine to machine, and all of the classes in the JAR file can be accessed, via classpaths, by `java` and `javac`, without ever unJARing the JAR file.

    jar -cf MyJar.jar myApp

The `jar` command will create a JAR file called `MyJar.jar` and it will contain the `myApp` directory and `myApp`'s entire subdirectory tree and files.


## Using Static Imports ##

### Static Imports ###

*Static imports* can be used when you want to use a class's `static` members.

* "static import" the syntax MUST be `import static` followed by the fully qualified name of the `static` member you want to import
* static import statement uses the wildcard to import of ALL the static members.




