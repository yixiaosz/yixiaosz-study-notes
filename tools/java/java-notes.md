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

### Primitive Datatypes

```java
int age = 25; // whole numbers
double salary = 25.42; //floating points
boolean isEmployed = true; // stores true and false
char grade = 'A'; // stores single characters using single quotes.

// String is a non-primitive object. Objects have built-in behavior.
String name = "Jane";
```









## Useful Resources

[Google Java style guide](https://google.github.io/styleguide/javaguide.html)

