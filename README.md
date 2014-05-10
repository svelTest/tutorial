svelTest Language Tutorial
=================
This tutorial will serve as a quick guide for getting svelTest programs up and running. All of the code used throughout the tutorial can be found [here](https://github.com/svelTest/svelTest/tree/master/tutorial).
#Table of Contents
1. [Introduction](#introduction)
2. [Setup](#setup)
3. [Hello World](#hello-world)
4. [File](#file)
5. [Funct](#funct)
<br>&nbsp;&nbsp;&nbsp;&nbsp;5.1 [What the Funct?](#what-the-funct)
6. [I/O](#io)L
<br>&nbsp;&nbsp;&nbsp;&nbsp;6.1 [Input](#input)
<br>&nbsp;&nbsp;&nbsp;&nbsp;6.2 [Output](#output)
<br>&nbsp;&nbsp;&nbsp;&nbsp;6.3 [readlines()](#readlines)
7. [Return & Assert](#return--assert)
<br>&nbsp;&nbsp;&nbsp;&nbsp;7.1 [verbose](#verbose)
8. [Main](#main)
9. [If/Else](#ifelse)
10. [Another Example: Add](#another-example-add)
11. [Arrays](#arrays)
12. [Writing Svelter Code: Looking at Add Again](#writing-svelter-code-looking-at-add-again)
13. [Conclusion](#conclusion)

# Introduction
The svelTest Language Tutorial will serve as a quick guide for getting svelTest programs up and running. This guide will go through several major aspects of the svelTest language by looking at various simple examples.

The svelTest programming language aims to simplify the complex process of testing production code. Our language targets any programmer looking for a simplified way to test source code to their specifications, in their own environment.

The purpose of this tutorial is to introduce the user to the basics of svelTest, allowing them to easily pick up svelTest syntax and code style. These examples concentrate on the basics of the language like variable declaration, arithmetic operations, loading source code files, and manipulating test case output.

It is important for the user to understand that this is not a complete tutorial on svelTest. Like any programming language, practice makes a great programmer. For more in-depth information on the intricacies of svelTest, please refer to our [Language Reference Manual](http://sveltest.github.io/manual/).

Remember to have fun, and you’ll be writing svelte code before you know it!

# Setup
svelTest does not require any outside packages or installers. The compiler was developed under Python version 2.7.3, so it will work best with that (but any Python 2.7.x version should be fine). Fire up your favorite text editor and you’re ready to start programming! We do, however, expect if you want to test Java programs that you to have configured your Java environment properly, and can compile and run Java programs from your favorite shell using javac and java respectively.  svelTest currently supports Java SE 6. To check your Java version, type java -version into your shell.  If you see java version "1.6.0_XX", you’re in luck! If you see java version 1.7.X or 1.8.X, please make sure to roll back to Java 6. We also support Python 2.7.x and any version of gcc that supports C99; you can perform similar checks for these dependencies. For the rest of this tutorial, we will use Java.

# Hello World
To get familiar with svelTest syntax, we can look at the svelTest’s interpretation of Hello World. This program prints “Hello World!” to the console.

```java
// hello.svel

// prints "Hello World!" to console 
lang = None;
main() {
	string hello = "Hello World!";
	print(hello);
}
```

Each svelTest program needs a `lang` declaration and a `main` function to run a program. (In this case, since we’re not testing any code, we use the `None` keyword; we’ll discuss lang more in our next example.) Each declaration is ended with a semicolon; whitespace and indentation do not matter to the compiler. To declare an object like a `string` or an `int`, much like Java, you need to first declare the type, then name the object. A string literal is indicated by quotation marks.

`print` is a reserved word in svelTest that prints to the console.  

Though `hello.svel` is a valid svelTest program, it does not really show what the language can do. Let’s look at Java’s Hello World. 


```java
// Hello.java

// prints "Hello World!" to console
public class Hello {
	public static void main (String args[]) {
		String hello = "Hello World!";
		System.out.println(hello);
	}
}
```
A svelTest program to test the output of `Hello.java` (which should be “Hello World!”) would look like this:

```java
// helloJava.svel

// tests Hello.java
lang = Java;

boolean helloWorldTest() {
file helloFile = "HelloWorld.java";
funct helloMain = {__main__, (j_String[]), helloFile};
input _in = ();
output out = "Hello World!";

	return helloMain.assert(_in, out);
}

main() {
	if (helloWorldTest()) {
		print("Hello World passed!");
	} else {
		print("Hello World failed.");
	}
}
```
Compiling your svelTest program is easy. You should have the `lib` folder containing the compile script; let’s say you also have another folder called `tutorial`. (We have this structure on our repo [here](https://github.com/svelTest/svelTest).) Navigate into `lib`, then type:

	./compile.sh ../tutorial/helloJava.svel

This will produce `helloJava.py` in the testing folder. The `helloJava.py` file does the grunt work of testing and comparing output as well as manipulating that output however you wish. All you have to do is change into the testing directory and run the compiled Python program:

	cd ../testing
	python helloJava.py


####Let’s go through the program line by line, starting with the following:

	//tests Hello.java

Comments in svelTest are indicated by a // or  /* ... */ for block comments; comments cannot be nested. The following are all valid comments:

	// this is valid

	/* this is valid too */
	
     /* this is 
	  * also valid
	  */

`lang`, as we mentioned earlier, is the language specifier.
	
	lang = Java;

In this case, we’re testing a Java program, so we must specify that `lang` equals `Java`. We also support `Python`, `C`, and `None`. These specifications determine which helper files to import to the compiled code; using `None` can avoid that overhead if it isn’t need.


####Next, we have:

```java
boolean helloWorldTest () {
	...
}
```

Methods in svelTest must be declared with their return type; in this case, `helloWorldTest()` returns a `boolean`. Function definitions are enclosed by curly braces. If the svelTest method does not return a value, declare the method with the `void` return type.

Functions in svelTest must also be fully defined before they are used. Note that `main()` uses `helloWorldTest()`; if `helloWorldTest()` was not fully defined before `main()`, the compiler would throw an error.

# File

	file helloFile = "../Hello.java";

`file` is used to select the file you want to test; it is followed by the object name. On the right side of the equals, the name of the file is entered as a string literal. In this case, we’re making a `file` object called `helloFile` and indicating that the `Hello.java` file is one directory above the svelTest program.

# Funct
The `funct` type is a wrapper to specify the method to test. 

### What the Funct?
The `funct` type is easy to use and declare.

	funct helloMain = {__main__, (j_String[]), helloFile};

`funct` is used to select which method of the file to test. It requires the name of the method, the the formal parameter type(s) of the method (more parameters can be added, separated by commas), and the file containing the method. 

# I/O
### Input

	input _in = ();

`input` is used to denote the input required by the method being tested. Multiple parameters should be separated by commas.  In this case, it is empty because the main method of `Hello.java` does not take any parameters. Note that we cannot call this input `in` but instead must call it `_in`; this is because `in` is a reserved keyword in svelTest programs. (See our [Language Reference Manual](http://sveltest.github.io/manual/) for a full list of reserved words.)

# Output

	output out = "Hello World!";

`output` is use to denote the expected output of the method you are testing. An `output` can take the value of any type in the source programming language.

### readlines()
####While we don’t use `readlines()` in our `helloJava.svel` program, we want to cover it briefly in our discussion of file I/O. 
Take a look at the following snippet:

	file outputs = "outputs.txt";
	output[] out = outputs.readlines();

The `file` type has the reserved, built-in function `readlines()` that reads the entire contents of a `file` into a `string` array. (In this case, we can put those results directly into an `output` array, since `output`s take `string`s.) The trailing newline character is stripped from each string as each line is copied to the array. This allows the user to easily load sets of `input`s and `output`s from ASCII text files into string arrays for use in a svelTest program.	

# Return & Assert
#### Let’s return to our discussion of `helloJava.svel`.

	return helloMain.assert(_in, out);

The `helloWorldTest()` function must return a `boolean`. When the `assert()` function is called on a `funct`, it uses the `input` and `output` parameters to test the method. `assert()` then returns either true or false depending if the expected output matches the method’s standard output. 

### verbose
`assert()` also has an optional third argument, `verbose`. Specifying `verbose` as the third argument adds additional functionality to `assert()`: instead of just returning true or false, `assert()` will also print test information to standard output for you. For example, something like

	helloMain.assert(_in, out, verbose);

would print

	main()...		PASS

without any additional code.

# Main

```java
main(){
	...
}
```

Each svelTest file is required to have a `main` method. It should be at the bottom of the file, after the methods it calls are declared.

# If/Else

```java
if (helloWorldTest()) {
	print("Hello World passed!");
} else {
	print("Hello World failed.");
}
```

This is a standard if/else statement that tells the program what to do in case of success or failure. If `helloWorldTest()` returns true (the `assert` test matches), it will print “Hello World passed!” to standard output. If not, it will print “Hello World failed.”

# Another Example: Add

Consider the following Java file that contains a method to add two Java integers:

```java
// Add.java

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
// add.svel

// test suite for add function in Add.java
lang = Java;
int addTestSuite() {
	int addSuccessCounter = 0;

	// svelTest test idiom:
	//	1. declare a file
	//	2. declare a funct (ie: the source program or method to test)
	//	3. create input(s) and corresponding output(s)
	//	4. call assert() on the funct with the input(s) and output(s)
	
	file addFile = "Add.java";
	funct add = {"add", (j_int, j_int), addFile};
	input in1 = (1, 2);
	output out1 = 3;
	input in2 = (-2, 2);
	output out2 = 0;
	input[] inArray = {in1, in2};
	output[] outArray = {};

	outArray.append(out1);
	outArray.append(out2);

	for(int i = 0; i < inArray.size(); i = i+1) {
		if (add.assert(inArray[i], outArray[i])) {
			addSuccessCounter = addSuccessCounter + 1;
		}
	}

	return addSuccessCounter;
}

main() {
	int successNumber = addTestSuite();
	
	if (successNumber == 2) {
		print("2/2 cases are successful");
	} else {
		print("method did not work as intended");
	}
}
```
The method `addTestSuite()` is declared with a return type of `int`. It declares an `int` called `addSuccessCounter` and initializes it to zero. It then specifies the location of `Add.java` with a `file` and creates a `funct` to test the `add()` function, which takes two Java integers (`j_int`) as parameters. 

# Arrays
Arrays can be instantiated directly or dynamically.

```java
input in1 = (1, 2);
output out1 = 3;
input in2 = (-2, 2);
output out2 = 0;
input[] inArray = {in1, in2};
output[] outArray = {};
outArray.append(out1);
outArray.append(out2);
```

In this section we create two `input`s that have two `int`s enclosed in parentheses and separated by commas. We also make two corresponding `output`s. The following lines show how to add objects to arrays. In the input array, we add the already created objects to `inArray` in the same line as the declaration. In the output array, we declare the array and use `append()` to add the two `output`s to the end of the array. Note that we must initialize `outArray` as empty with curly brackets in order to append or insert values into it.

```java
for(int i = 0; i < inArray.size(); i++) {
		if (add.assert(inArray[i], outArray[i])) {
		addSuccessCounter++;
	}
}
```

svelTest supports `for` loops that have 3 parameters: one that initializes a counter, a boolean expression, and an incrementer. This boolean expression uses `size()` to compare `i` with the number of elements in the array. The `for` loop contains an `if` statement to check each element of the `input` array against the corresponding element with the same index of the `output` array. If the test evaluates as true, `addSuccessCounter` will increment. 

The end of the method returns `addSuccessCounter`.

The `main` method of this .svel file runs `addTestSuite()` and prints out that the cases were successful if it passed two out of two tests.

A last note on arrays: Other supported array functions are `remove()`, `insert()`, `size()`, and `replace()`. The prototypes are outlined below:

* `a.remove(int index)` 
* `a.insert(int index, object o)`
* `a.size()`
* `a.replace(int index, object o)`

# Writing Svelter Code: Looking at Add Again
You might notice that our `add.svel` program looks a little bulky: only 10 lines of Java code to test led to over 40 lines of svelTest code. The `add.svel` program was intentionally written to be very clear, but a lot of the idioms can be condensed. Take a look at our svelter version:

```java
// addSvelter.svel

// more succinct test suite for add function in Add.java
lang = Java;

main() {
	funct add = {"add", (j_int, j_int), "Add.java"};
	input[] inArray = {(1,2), (-2,2), (4, 5)};
	output[] outArray = {3, 0, 10};

	// the third test case should fail (4+5 != 10)
	for(int i = 0; i < inArray.size(); i = i+1) {
		add.assert(inArray[i], outArray[i], verbose);
	}
}
```

Hopefully you can convince yourself that you’ll see the following output:

	add(1, 2)... 					 PASS
	add(-2, 2)... 					 PASS
	add(4, 5)... 					 FAIL
		returned: 9
		expected: 10

If you don’t believe us, give it a try yourself! As you can see, only 10 lines of code now successfully run three test cases.


# Conclusion

This Language Tutorial covers the basic capabilities of the svelTest programming language. We hope that this tutorial will be useful in assisting users in writing their own, more complex, testing programs. Although we only used Java in this tutorial, writing your own svelTest programs to test C and Python should be nearly identical, but check out the Reference Manual for more extensive discussion. As stated previously, please see our [Language Reference Manual](http://sveltest.github.io/manual/) for a more comprehensive analysis of the features of svelTest.
