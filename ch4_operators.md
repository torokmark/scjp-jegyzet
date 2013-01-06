# Operators #

## Java Operators (Exam Objective 7.6) ##

Java operators produce new values from one or more operands. The result of most operations is either a `boolean` or numeric value.

* The + operator can be used to add two numeric primitives together, or to perform a concatenation operation if either operand is a String.
* The &, |, and ^ operators can all be used in two different ways, although as of this version of the exam, their bit-twiddling capabilities won't be tested.

### Assignment Operators ###
We covered most of the functionality of the assignment operator, "=":

* When assigning a value to a primitive, *size* matters. Be sure you know when implicit casting will occur, when explicit casting is necessary, and when truncation might occur.
* Remember that a reference variable isn't an object; it's a way to *get* to an object.
* When assigning a value to a reference variable, *type* matters. Remember the rules for supertypes, subtypes, and arrays.

#### Compound Assignment Operators

### Relational Operators ###
Relational operators always result in a boolean (`true` or `false`) value.
When comparing a character with a character, or a character with a number, Java will use the Unicode value of the character as the numerical value, for comparison.

#### "Equality" Operators ####
Java also has two relational operators (sometimes called "equality operators") that compare two similar "things" and return a `boolean` the represents what's true about the two "things" being equal. These operators are:

* == equals (also known as "equal to")
* != not equals (also known as "not equal to")

Each individual comparison can involve two numbers (including `char`), two `boolean` values, or two object reference variables.
There are four different types of things that can be tested:

* numbers
* characters
* boolean primitives
* Object reference variables

#### Equality for Primitives ####
if a floating-point number is compared with an integer and the values are the same, the == operator returns `true` as expected.

#### Equality for Reference Variables ####
   
    ``` java
    JButton a = new JButton("Exit");  
    JButton b = a;  
    ```

After running this code, both variable a and variable b will refer to the same object. Reference variables can be tested to see if they refer to the same object by using the == operator. The == operator is looking at the bits in the variable, so for reference variables this means that if the bits in both reference variables are identical, then both refer to the same object.
The == operator will not test whether two objects are "meaningfully equivalent".

#### Equality for Enums ####
Once you've declared an enum, it's not expandable. At runtime, there's no way to make additional enum constants. You can have as many variables as you'd like refer to a given enum constant, so it's important to be able to compare two enum reference variables to see if they're "equal". 
You can use either the == operator or the `equals()` method to determine if two variables are referring to the same enum constant.

### instanceof Comparison ###
The `instanceof` operator is used for object reference variables only, and you can use it to check whether an object is of a particular type.

#### instanceof Compiler Error #### 
You can't use the `instanceof` operator to test across two different class hierarchies.

    ``` java
    class Cat { }  
    class Dog {  
        public static void main(String [] args) {  
            Dog d = new Dog();  
            System.out.println(d instanceof Cat);  
        }  
    }  
    ```

### Arithmetic Operators ###
The basic arithmetic operators:

* + addition
* – subtraction
* * multiplication
* / division

#### The Remainder (%) Operator ####
The remainder operator divides the left operand by the right operand, and the result is the remainder.

Expressions are evaluated from left to right by default. You can change this sequence, or *precedence*, by adding parentheses. Also remember that the * , / , and % operators have a higher precedence than the + and - operators.

#### String Concatenation Operator ####
The plus sign can also be used to concatenate two strings together.

    ``` java
    String a = "String";  
    int b = 3;  
    int c = 7;  
    System.out.println(a + b + c);  
    ```

The `int` values were simply treated as characters and glued on to the right side of the String, giving the result:

    `String37`

If you put parentheses around the two `int` variables: 
    
    ``` java
    System.out.println(a + (b + c));  // the result is String10
    ```

The rule to remember is this:
*If either operand is a String, the + operator becomes a String concatenation operator. If both operands are numbers, the + operator is the addition operator.*

#### Increment and Decrement Operators ####
Java has two operators that will increment or decrement a variable by exactly one.

* ++ increment (prefix and postfix)
* -- decrement (prefix and postfix)

The operator is placed either before (prefix) or after (postfix) a variable to change its value.

### Conditional Operator ###
The conditional operator is a *ternary* operator (it has three operands) and is used to evaluate `boolean` expressions. Like an *if* statement except instead of executing a block of code if the test is `true`, a conditional operator will assign a value to a variable.

    `x =` (boolean expression) `?` value to assign if `true :` value to assign if `false`

### Logical Operators ###

#### Bitwise Operators (Not on the Exam!) ####
Okay, this is going to be confusing. Of the six logical operators listed above, three of them (&, |, and ^) can also be used as "bitwise" operators.

#### Short-Circuit Logical Operators ####
The two *short-circuit* logical operators.

* && short-circuit AND
* || short-circuit OR

They are used to link little `boolean` expressions together to form bigger `boolean` expressions.
The && and || operators evaluate only `boolean` values.
*The short-circuit feature of the && operator is so named because it doesn't waste its time on pointless evaluations.* A short-circuit && evaluates the left side of the operation first (operand one), and if it resolves to `false`, the && operator doesn't bother looking at the right side of the expression (operand two) since the && operator already *knows* that the complete expression can't possibly be `true`.
The || operator is similar to the && operator, except that it evaluates to `true` if EITHER of the operands is true.

#### Logical Operators (Not Short-Circuit) ####
Two *non-short-circuit* logical operators.

* & non-short-circuit AND
* | non-short-circuit OR

They evaluate both sides of the expression, always!

#### Logical Operators ^ and ! ####
Two logical operators:

* ^ exclusive-OR (XOR)
* ! boolean invert

The ^ (exclusive-OR) operator evaluates only `boolean` values.
For an exclusive-OR (^)expression to be `true`, EXACTLY one operand must be `true`.

The ! (boolean invert) operator returns the opposite of a boolean's current value.


