**[Intro to Java](Intro-to-Java)**
* **[Overview](Intro-to-Java#overview)**
* **[Java and Object Oriented Programming](Intro-to-Java#java-and-object-oriented-programming)**
* **[Comments](Intro-to-Java#comments)**
* **[Blocks](Intro-to-Java#blocks)**
* **[White Space](Intro-to-Java#white-space)**
* **[Variable Descriptors](Intro-to-Java#variable-descriptors)**
* **[Class Declaration](Intro-to-Java#class-declaration)**
* **[Methods](Intro-to-Java#methods)**
  * **[Main](Intro-to-Java#main)**
  * **[Constructor](Intro-to-Java#constructor)**
  * **[Getter](Intro-to-Java#getter)**
  * **[Setter](Intro-to-Java#setter)**
  * **[General](Intro-to-Java#general)**
* **[Using a Class](Intro-to-Java#using-a-class)**

##Overview
This guide describes the peculiarities of java. It is intended for those who have programmed before, and therefore know basic concepts such as common data types (such as integers and booleans), structures (such as arrays) and control flow (like for and while loops). If you've never programmed before, Code Academy has a great [Introduction to Java course](https://www.codecademy.com/learn/learn-java) that starts you off with the basics.There is also a really handy [Java Programming Cheatsheet](http://introcs.cs.princeton.edu/java/11cheatsheet/) you can use as a quick reference.

Keep in mind this is far from an extensive guide, and is more meant to provide some background information to people working on programming the robot.

##Java and Object Oriented Programming
**Object Oriented Programming (OOP)** is a type of programming in which programmers define not only the data type of a data structure, but also the types of operations that can be applied to the data structure. These operations are referred to as **functions**. In this way, the data structure becomes an **object** that includes both data and functions. 

The first step in OOP is to identify all the objects the programmer wants to manipulate and how they relate to each other, an exercise often known as data modeling. Once an object has been identified, it is generalized as a **class** of objects which defines the kind of data it contains and any logic sequences that can manipulate it. Each distinct logic sequence is known as a **method**. Objects communicate with well-defined interfaces called messages.

Java is object-oriented, which means that, among other characteristics, an object can take advantage of being part of a class of objects and inherit code that is common to the class. Objects are thought of as "nouns" that a user might relate to rather than the traditional procedural "verbs." A method can be thought of as one of the object's capabilities or behaviors.

Throughout this guide we will work through an example Student class to better understand the different components of Java. A student is a good example of a class, as there can be many instances of it (there are probably many students in the school). A student has certain information that you would want to remember, like their first and last name. Additionally there are certain operations that would be nice to perform on a student, like advancing their year.

Below is the full class, each part of which will be addressed in the following sections.
```java
1   public class Student {
2       private String first_name;
3       private String last_name;
4       private int year;
5       private static int years_required;
6       private String email;    
7   
8       private Student(String first_name, String last_name, int year) {
9           this.first_name = first_name;
10          this.last_name = last_name;
11          this.year = year;
12      }
13  
14      public String getFirstName() {
15          return irst_name;      
16      }
17  
18      public void setEmail(String email) {
19          this.email = email;
20      }
21  
22      public void advanceYear() {
23          this.year = this.year + 1;
24      }
25  }
```

##Comments
The syntax for comments is pretty simple and is illustrated below. Java supports single line comments with `//` and multiline comments with `/* <insert comment here> */`.

```java
// This here is a comment.

/*
 * This is a multiline comment.
 * Which means this line is still a comment.
 * Here too, but this is the last one.
 */
```

##Blocks
Blocking is extremely important in Java, as it allows programmers to split their code into sections. A block is indicated with curly braces `{` and `}` as shown below. A block marks the beginning and end of a segment of code. Some examples of segments that require being surrounded by braces are classes, methods, and loops. A variable declared within a block does not extend outside the block that is was declared in, unless otherwise specified. The area of the code in which a variable is declared is referred to as its **scope**.

```java
if(true) {
   // The following code is inside the this block
   int i = 3;

   if(true) {

      // This is a nested block. 
      // ‘i’ can still be used, as this is still within the outer block.

   } // The inner block is closed

} // The outer block is closed, and the variable ‘i’ is no longer declared

// Code here is outside the block
```

##White Space
Each line in Java should end with a `;`, unless it is the beginning of a block. This signifies that a line is completed and java can progress to the next new instruction. Unlike Python, java does not depend on spacing. As long as you have `;`, `{` and `}` all in the right places to denote new instruction beginnings and blocks, java will interpret your code correctly. It is **highly** advised though, that you follow spacing standards to aid in code legibility. New instructions should start on a new line, and any code within a block should be tabbed in. If there is a block inside a block, the tabbing should nest as well.

##Variable Descriptors
The **scope** or area in which a variable is defined can be modified with the use of block. It is also possible to control scope using descriptors, such as **private** and **public**. A private variable or method can only be accessed (or “seen”) by the class it is contained in. For example, the first and last name fields in the Student class are private and therefore cannot be directly accessed by another class. These variables can only used by the Student class. Public, on the other hand, means that the variable or method is available to all outside classes. The method `getFirstName` can be seen and called by any other class, as well as within the Student class.

Another modifier for variables is **static**. This does not change the scope of a variable, but rather marks it as unchanging between different instances of the class. In the Student class example `years_required` is not unique between students. All the students created, no matter their name, email or current year have the same number of years of school to complete. This variable can still be modified, but will always contain the same value for all instances of the Student class.

##Class Declaration
To create a new class, simply declare it as shown. `Public` states that outside code will be able to see the existence of this new class. `Class`, obviously, marks it as a class. The last string is the name of the class, in this case `Student`. 

```java
1   public class Student {

    ... 

24  }
```

##Methods
Methods are routines that provide a class with its functionality. Each method has a **scope**, a **return type**,  and **parameters**. The scope determines whether or not the method can be called by outside classes. The return type specifies what kind of data the method reports when it completes all the code it contains and the parameters are the inputs (and their types) that must be provided when the function gets called.

###Main
You can specify some classes to have a main method, but it is not required for every class. A main method is what gets run when the overall program first starts.

```java
public static void main(String [] args)
```

###Constructor
Each class comes with a special kind of method called a constructor. There is a default constructor if one is not explicitly provided, but you can add as many constructors to a class as needed (as long as the parameters are different for each one). A constructor is what sets up the class whenever a new instance of it is created. In this case it allows a program to create a student with a specific first name, last name and year.  The lines within the constructor set the class variables equal to the strings passed into the function in order to store the provided information. A constructor does not have a return value.

```java
7       private Student(String first_name, String last_name, int year) {
8           this.first_name = first_name;
9           this.last_name = last_name;
10          this.year = year;
11      }
```

###Getter
A getter is a type of method that simply returns a stored value that otherwise may not be visible to outside classes. In this case getFirstName just returns the stored first_name field. This is the only way outside classes would be able to see the value contained in first_name, since that variable is private. This is a good way of controlling access and the format in which the data is seen.

```java
13      public String getFirstName() {
14          return first_name;      
15      }
```

###Setter
A setter is basically the opposite of a getter. It takes a value as a parameter and set the field of the class to match. In this case the students email field is being updated to the email provided to the function. Similar to getters, these types of functions allow specific accesses to otherwise private variables. Their return type is typically **void**, which means they don’t actually return a value.

```java
17      public void setEmail(String email) {
18          this.email = email;
19      }
```

###General
A class can have as many methods as needed, and they don’t all have to fit into the types specified above. In fact, most of them won’t. A method is simply a task that the class can perform. In the example below, a students year can be increased by one (at the end of the year perhaps). Methods can have any type of return value and any number of parameter. They can be incredibly complex and call other methods, even from different classes. The possibilities are endless.

```java
21      public void advanceYear() {
22          this.year = this.year + 1;
23      }
```

##Using a Class
To create a new instance of a class (or **instantiate** it) simply declare a new variable of the class type and call the class's constructor. This is done in a different class, and will sometimes require you to **import** the class you are trying to use. Imports go at the very top of a file and basically just specify what other classes you will need to reference throughout your code.

```java
1   import Student;
2   
3   private class School() {
4    
5       public static void main(String [] args) {
6           private Student student_one = new Student("Jane", "Doe", 10);
7           private Student student_two = new Student("John", "Smith", 6);
8    
9           student_one.getFullName(); //this will return "Doe, Jane"
10          student_two.advanceYear(); //this will change student_twos year to 7
11          student_two.setEmail("john.smith@gmail.com");
12          //and so on
13
14      }
15  }
```