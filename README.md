svelTest Language Tutorial
=================

1. [Introduction](#introduction)
2. [Setup](#setup)
3. [Hello World](#hello-world)
4. [File](#file)
5. [Funct](#funct)
<br>&nbsp;&nbsp;&nbsp;&nbsp; 5.1 [What the Funct?](#what-the-funct)
6. [I/0](#io)
<br>&nbsp;&nbsp;&nbsp;&nbsp; 6.1 [Input](#input)
<br>&nbsp;&nbsp;&nbsp;&nbsp; 6.2 [Output](#output)
7. [Return & Assert](#return--assert)
8. [Main](#main)
9. [If/Else](#ifelse)
10. [Another Example: Add](#another-example-add)
11. [Arrays](#arrays)
12. [Conclusion](#conclusion)

# Introduction
The svelTest Language Tutorial will serve as a quick guide for getting svelTest programs up and running. This guide will go through several major aspects of the svelTest language by looking at various simple examples.

The svelTest programming language aims to simplify the seemingly complex process of testing production code. Our language targets any programmer looking for a simplified way to test source code to their specifications, in their own environment.

The purpose of this tutorial is to introduce the user to the basics of svelTest, allowing them to easily pick up svelTest syntax and code style. These examples concentrate on the basics of the language like variable declaration, arithmetic operations, loading source code files, and manipulating test case output.

It is important for the user to understand that this is not a complete tutorial on svelTest. Like any programming language, practice makes a great programmer. For more in-depth information on the intricacies of svelTest, please refer to our Language Reference Manual.

Remember to have fun, and you’ll be writing svelte code before you know it!

# Setup
svelTest does not require any outside packages or installers. The compiler was developed under Python version 2.7.3, so it will work best with that (but any Python 2.x version should be fine). Fire up your favorite text editor and you’re ready to start programming! We do however, expect if you want to test Java programs that you to have configured your Java environment properly, and can compile and run Java programs from your favorite shell using javac and java respectively.  svelTest currently supports Java SE 6. To check your Java version, type java -version into your shell.  If you see java version "1.6.0_XX", you’re in luck! If you see java version 1.7.X or 1.8.X, please make sure to roll back to Java 6. We also support Python 2.x and a valid C99 compiler; you can perform similar checks for these dependencies. For the rest of this tutorial, we will use Java.

# Hello World
To get familiar with svelTest syntax, we can look at the svelTest’s interpretation of Hello World. This program prints “Hello World!” to the console.
```java
/* 
 * hello.svel
 * prints ‘Hello World!’ to console 
 */
main()
{
	string hello = “Hello World!”;
	print hello;
}
```
Each svelTest file needs a `main` function to run a program. Each declaration is ended with a semicolon; whitespace and indentation do not matter to the compiler. To declare an object like a `string` or an `int`, much like Java, you need to first declare the type, then name the object. A string literal is indicated by quotation marks.

`print` is a reserved word in svelTest that prints to the console.  

Though `hello.svel` is a valid svelTest program, it does not really show what the language can do. Let’s look at Java’s Hello World. 

```java
/* Hello.java
 * prints ‘Hello World!’ to console 
 */
public class Hello {
	public static void main (String args[]) {
		String hello = “Hello World!”;
		System.out.println(hello);
	}
}
```
A svel program to test the output of `Hello.java` (which should be “Hello World!”) would look like this:

```java
/* helloJava.svel
 * tests Hello.java 
 */
boolean helloWorldTest() 
{
	file helloFile = “Hello.java”;
	funct helloMain = {__main__, (j_String[]), helloFile};
	input in = ();
	output out = “Hello World!”;

	return helloMain.assert(in, out);
}

main()
{
	if (helloWorldTest()) {
		print “Hello World passed!”;
	} else {
		print “Hello World failed.”;
	}
}
```
After you compile `helloJava.svel`, two files will be produced: `run.sh` and `test.py`. The `test.py` file does the grunt work of testing and comparing output; `run.sh` is a wrapper script that coordinates all the testing for you. Running `$ ./run.sh` will print “Hello World passed!” to the console. 

Let’s go through the program line by line, starting with the following:
```java
// tests Hello.java
```
Comments in svelTest are indicated by a `//` or  `/* ... */` for block comments; comments cannot be nested. The following are all valid comments:

```java
// this is valid

/* this is valid too */
	
/* this is 
*  also valid
*/
```

Next, we have:

```java
boolean helloWorldTest ()
{
	...
}
```

Methods in svelTest need to be declared with their return type; in this case, `helloWorldTest()` returns a `boolean`. Function definitions are enclosed by curly braces. If the svelTest method does not return a value, declare the method with the `void` return type.

# File
```java
file helloFile = “../Hello.java”;
```
`file` is used to select the file you want to test; it is followed by the object name. On the right side of the equals, the name of the file is entered as a string literal. In this case, we’re making a `file` object called `helloFile` and indicating that the `Hello.java` file is one directory above the svelTest program.

# Funct
The `funct` type is a wrapper to specify the method to test. 

### What the Funct?
The `funct` type is easy to use and declare.
```java
funct helloMain = {__main__, (j_String[]), helloFile};
```
`funct` is used to select which method of the file to test. It requires the name of the method, the the formal parameter type(s) of the method (more parameters can be added, separated by commas), and the `file` containing the method. 

# I/O
### Input
```java
input in = ();
```
`input` is used to denote the input required by the method being tested. Multiple parameters should be separated by commas.  In this case, it is empty because the main method of `Hello.java` does not take any parameters.

### Output
```
output out = “Hello World!”;
```
`output` is use to denote the expected output of the method you are testing. An `output` can take the value of any type in the source programming language.

# Return & Assert
```
return helloMain.assert(in, out);
```
The `helloWorldTest()` function declaration requires it to return a `boolean`. When the `assert` statement is called on a `funct`, it uses the input and output parameters to test the method. `assert` then returns either `true` or `false` depending if the expected output matches the method’s console output. 

# Main
```
main()
{
...
}
```
Each svelTest file is required to have a `main` method. It should be at the bottom of the file, after the methods it calls are declared.

# If/Else
```java
if (helloWorldTest()) {
	print “Hello World passed!”;
} else {
	print “Hello World failed.”;
}
```
This is a standard `if/else` statement that tells the program what to do in case of success or failure. If `helloWorldTest()` returns `true` (the `assert` test matches), it will print “Hello World passed!” to the console. If not, it will print “Hello World failed.”

# Another Example: Add

Consider the following Java file that contains a method to add two Java integers:
```java
/* 
 * Add.java
 */

public class Add {
	public static void main(String[] args) {
		int z = add(1, 2);
		System.out.println("The sum of 1 and 2 is " + z);
	}

	public static int add(int x, int y) {
		return x + y;
	}
}
```
Suppose you wanted to make sure that just the function `add()` worked correctly. You want to check multiple cases and track which tests it passes. A svelTest program to test the `add()` function would look like this:
```java
/*
 * add.svel
 * test suite for add function in Add.java
 */

int addTestSuite() 
{
	int addSuccessCounter = 0;
	
	/* svelTest test idiom:
		1. declare a file
		2. declare a funct (ie: the source program or method to test)
		3. create input(s) and corresponding output(s)
		4. call assert() on the funct with the input(s) and output(s)
	*/
	file addFile = “Add.java”;
	funct add = {“add”, (j_int, j_int), addFile};
	input in1 = (1, 2);
	output out1 = 3;
	input in2 = (-2, 2);
	output out2 = 0;
	input[] inArray = {in1, in2};
	output[] outArray;
	outArray.append(out1);
	outArray.append(out2);
	
	for(int i = 0; i < inArray.size(); i++)
	{
		if (add.assert(inArray[i], outArray[i])){
			addSuccessCounter++;
		}
	}

	return addSuccessCounter;
}

main()
{
	int successNumber = addTestSuite();
	
	if (successNumber == 2){
		print “2/2 cases are successful”;
	} else {
		print “method did not work as intended”;
	}
}
```
The method `addTestSuite()`, is declared with a return type of `int`. It declares an int called `addSuccessCounter` and initializes it to zero. It then specifies the location of `Add.java` with a `file` and creates a `funct` to test the `add()` function, which takes two Java integers (`j_int`) as parameters. 

# Arrays
Arrays can be instantiated directly or dynamically.
```
input in1 = (1, 2);
output out1 = 3;
input in2 = (-2, 2);
output out2 = 0;
input[] inArray = {in1, in2};
output[] outArray;
outArray.append(out1);
outArray.append(out2);
```
In this section we create two `input`s that have two `int`s enclosed in parentheses and separated by commas. We also make two corresponding `output`s. The following lines show how to add objects to arrays. In the `input` array, we add the already created objects to `inArray` in the same line as the declaration. In the `output` array, we declare the array and use `append()` to add the two `output`s to the end of the array.
```java
for(int i = 0; i < inArray.size(); i++)
{
	if (add.assert(inArray[i], outArray[i])){
		addSuccessCounter++;
	}
}
```
svelTest supports for loops that have 3 parameters: one that initializes a counter, a boolean expression, and an incrementer. This boolean expression uses `size()` to compare `i` with the number of elements in the array. The `for` loop contains an `if` statement to check each element of the `input` array against the corresponding element with the same index of the `output` array. If the test evaluates as `true`, `addSuccessCounter` will increment. 

The end of the method returns `addSuccessCounter`.

The `main` method of this `.svel` file runs `addTestSuite` and prints out that the cases were successful if it passed two out of two tests.

# Conclusion

This Language Tutorial covers the basic capabilities of the svelTest programming language. We hope that this tutorial will be useful in assisting users in writing their own, more complex, testing programs. As stated previously, please see our Language Reference Manual for a more comprehensive analysis of the features of svelTest.
