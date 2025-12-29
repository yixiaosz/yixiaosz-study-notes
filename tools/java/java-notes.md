# Java Study Notes

Created on 12/28/2025 using Typora



## Fundamentals

### Compiling

```shell
ls
>> myfile.java
javac myfile.java # run the java compiler directly in terminal. 
ls
>> myfile.java myfile.class # the file's executable is a .class file. 
java myfile # run the executable (no need to mention .class).
```

### Basic Class structure

```java
//the Class, also it must match the filename: myClass.java
public class myClass{
  	//the Method
  public static void main(String[] args) {
		//the Statement
    System.out.println("Hello World!");
  } 
}

/*
this is a multi-line comment.
line 2
*/
```

### Datatypes

```java
// Primitive datatypes
int age = 25; // whole numbers
double salary = 25.42; //floating points
boolean isEmployed = true; // stores true and false
char grade = 'A'; // stores single characters using single quotes.

// Non-primitive *objects*
String name = "Jane";

// String concatenation
String intro = "My name is " + name +", I am " + age + " years old."
```

### Equality operators

```java
int age1 = 25;
int age2 = 30;
System.out.println(age1 != age2); // primitive equality operators: == != 
>> true

String name1 = "Jane";
String name2 = "Jack";
System.out.println(name1.equals(name2)); // non-primitive operators: .equals()
>> false
```

### Final keyword

To declare a variable with a value that **cannot be manipulated**, we need to use the `final` keyword. 

```java
final double pi = 3.14; 
pi += 1;
>> error: cannot assign a value to final variable pi
```

### Custom Class and Objects

A Class is a template, and objects are the instances of the class. 

```java

```





## Useful Resources

[Google Java style guide](https://google.github.io/styleguide/javaguide.html)

