---
date: 2024-03-04 14:01:10
tags:
  - golang
  - go
  - testing
  - tdd
  - blog
slug: full-introduction-to-golang-with-test-driven-development-part-1
title: Full Introduction to Golang with Test-Driven Development. Part I
keys:
  - go
  - golang
  - tdd
  - golang test
  - golang testing
  - golang test coverage
  - golang testmain
  - golang test setup
  - golang test framework
  - golang testdata
  - golang test assert
  - golang test package
  - golang test main
  - golang test files
  - golang test example
  - golang test all packages
  - golang assert slice equal
  - assert in golang test
  - golang test cases
  - golang unit test
  - golang unit testing
  - golang unit
  - golang test command
  - golang test coverage report
  - coverage golang test
  - table driven test golang
  - example golang test
  - golang unit test example
  - golang table tests
  - golang table test
  - golang table test example
  - golang table test parallel
  - golang table driven tests
  - testing table example
  - golang table testing
  - table test go
  - go table based test
  - golang test cases table
  - golang table driven test
  - golang table driven tests parallel
  - test table golang
  - golang unit test table driven
  - unit testing
  - unit test
  - unit tests
  - golang test function
type: blog
public: true
---

> This is a post based on the script for my video ["A new learning way on how to write "Hello, World!" program in Go"](https://www.youtube.com/watch?v=EECXgocw4N0&t=487s) which available ["on my YouTube channel"](https://www.youtube.com/channel/UCvjx6Q_8iyME0FHiDB-nxiA) and it is a part of the upcoming course "Grokking Go Fundamentals with Tests" (GGFT).

Hello and welcome. In this chapter, we will write our first [Go](https://kovalevsky.io/golang/) program. As a tradition, we will create a classic "hello world" program. It is a program that prints "Hello, World!" in the terminal after you run it. In this series, we'll do all examples in the [TDD](https://kovalevsky.io/test-driven-development/) way. [TDD (test-driven development)](https://kovalevsky.io/test-driven-development/) is a special way of writing software. That special way of writing software is based on the Test-driven development cycle. 

The Test-driven development cycle consists of the following steps: 
1. Add a test, which means you have to write tests first for the code that solves your problem. 
2. Run tests. All new tests must be failed because you don't have enough code that passes these tests. 
3. Write the simplest code that passes the new test. 
4. All tests now should be passed. 
5. Refactor as needed, using tests after each refactor to ensure that functionality is preserved. 
6. Repeat the cycle. So, for our first and all future examples, we're gonna follow the TDD way in general and TDD cycle in particular. 

I found tests quite useful in daily programming practice but also it's a very practical way to learn a new programming language.

So first of all, we need to create a folder for our project. In this series, all our examples will be in one folder. Let's call it "go-sandbox," but you can name it whatever you want. I decided to name it "sandbox" because it will be a folder with a bunch of small examples that are mostly not related to each other; it will be basically a sandbox for our experiments with [Go](https://kovalevsky.io/golang/). I will create a folder using a Terminal emulator. You can use any terminal emulator app, for example, you can use one of the most popular terminal emulators on macOS that's called iTerm 2. In the Terminal, open the path where you're gonna create a folder for the project using the command `cd`.

```bash
cd projects/personal
```

Now I'm in the folder with all my personal projects, I'm gonna create a folder called "go-sandbox" using the command `mkdir`.

```bash
mkdir go-sandbox
```

Open this folder in the terminal using the `cd` command again. We will need the terminal with this opened folder later. After creating a folder open it in your editor or IDE of choice. I use different editors and IDEs for writing Go code such as Goland, VSCode, and [Neovim](https://kovalevsky.io/neovim/) but now I will open it inside Neovim. Once we opened the project folder in our editor, we need to talk about Go program structure. First of all, all Go projects must have a "go.mod" file where you define information about your module, such as the module's path, the minimum version of Go that uses this module, information about dependencies on other modules, etc.

But what is the module? Go code is organized into modules and packages. Basically, every Go program or project is a module. It should have a unique identifier, a path, which could be a location from which you can download this module. A module can be distributed as a dependency for another module. So, if you don't have a "go.mod" file in your project you need to use Go tool command `go mod` to create it or you can create it manually. Let's go back to the terminal and open the project's folder again. Type command `go mod init <module_name>` which in our case is `go mod init go-sandbox`. You will see that file "go.mod" appeared in the folder.

Now, let's follow our TDD cycle and do the first step. The first step of the test-driven development cycle is to add a test. But in the first example, let's start with creating a file that will contain the simplified version of the code for which we'll add a test. We're doing it first just to have some basic understanding of how Go code looks like. So first of all, let's start with creating a new folder for our package "helloworld" in the root of the project folder.

```bash
mkdir helloworld
```

Open the new folder in the terminal.

There, inside the folder, create a file called "helloworld.go" by using the command "touch," but you can create it inside your code editor.

```bash
touch helloworld.go
```

Open the file in your code editor. Our Go file was created and it's empty for now. Just follow me and write the first line in the file.

```go
package helloworld
```

The very first line says package helloworld and that line is "package declaration." "Package declaration" always begins with the keyword "package" and must be in every Go file, and it specifies to which package file belongs to. But what is a package? Go programs are structured into packages - some kind of grouping units which Go program consist of. And as I mentioned before, every module has at least one package. We will talk about packages in future chapters.

Okay, we have our first Go file "helloworld.go"; so what's next? In most programming languages, the program "Hello, World!" contains a few lines of code where you call some method with the string "Hello, World!" as the first argument of this method. When you run the program, this method prints the string in the terminal. Since we're following the test-driven development way, we need to write a test that checks that our code works correctly and prints a string "Hello, World!" But we are not going to check the method that prints the string in the terminal. It requires a bit more knowledge for the first lesson. Instead, we're gonna write a simple function called "HelloWorld" that returns the string "Hello, World!".

If you had an experience with other programming languages before you probably know what a function is. But if not, here is a simple explanation of what a function is and what it is used for: a function is a named block of code that performs a specific task. It is like a mini-program within your program that you can call and use multiple times. 

- A function has a name that you choose to describe what it does. In our case, its name is "HelloWorld". 
- It can take inputs or parameters (if needed) to perform its task. Our function "HelloWorld" won't have any parameters. 
- It can also return a result (if needed) after completing its task. As I mentioned before, our function must return a string "Hello, World!" 
- Inside the function, you write the code that defines the task or actions it should perform.

For the function HelloWorld, we'll write a test that checks the function returns the expected value. And then in our main file of the program, we will call the function "HelloWorld" to pass the result of calling this function to the method that prints the string in the terminal. Okay, now let's start to write function HelloWorld. To write a function in Go you need to use the keyword "func".

```go
func HelloWorld() {
	
}
```

This function doesn't return any values and doesn't have any logic inside for now. However, if we still follow the test-driven development cycle, we have to write the code inside the function on step 3. 

So let's go back to step 1 and add a test for the function HelloWorld. In the folder of the package "helloworld," create the file called "helloworld_test.go." There are some naming rules for tests. Any test files that will be run by Go's built-in test runner must be named as "xxx_test.go" where "xxx" is the name of the file that contains the code which tests are written for. In our case, we're gonna write tests for the code inside the file called "helloworld.go," so we need to name our test file "helloworld_test.go." 

In Go, to test some function, you need to write another function, a test function, inside a test file. This test function should contain checking of the function that we want to test. In our case, in the test function, we need to check that function HelloWorld returns the string "Hello, World!" So first of all, we need to write a function called "TestHelloWorld".

```go
func TestHelloWorld() {

}
```

The name "TestHelloWorld" is very important here. There is a convention for naming a test function which looks like "TestXxx" where "Xxx" is the name of the function that we want to test, which is HelloWorld. 

Okay, so now we need to write a checking that function HelloWorld returns a string "Hello, World!". To check the result of executing the function, we need to save this result somewhere first. We can use a variable to save the result of executing a function.

A variable is a named container that holds a value. It's like a labeled box that can store different types of information, such as numbers, text, or more complex data. 

- A variable has a name that you choose to identify it. In our case, the name is "got." 
- It can hold different types of values, such as numbers, strings, or booleans. Since the function "HelloWorld" returns a string, the variable "got" also should have a string type. 
- You can assign a value to a variable using the "=" operator. Once a value is assigned, you can change it or use it in your program. 

There are several ways of declaring a variable in Go, but now we're gonna use the shortest one. It's called short variable declaration and it uses a special operator :=. The "short variable declaration" combines declaration and assignment of the variable. That means that you declare a type of the variable and also store some value in the variable.

```
someVariable := someValue
```

In our test function, we're gonna create a variable called "got":

```go
func TestHelloWorld() {
	got := HelloWorld()
}
```

The line "HelloWorld()" means that we're calling the function HelloWorld() and it should return some value, and that value should be stored in a variable "got." Now let's declare another variable called "want."

```go
func TestHelloWorld() {
	got := HelloWorld()
	want := "Hello, World!"
}
```

We just created a new variable called "want" and stored a value there, which is a string "Hello, World!" We're gonna use this variable to compare it with the variable "got," and that's how we will check that function HelloWorld returns the correct value. 

Let's use an "if statement" to compare variables.

```go
func TestHelloWorld() {
	got := HelloWorld()
	want := "Hello, World!"
	
	if expected != got {
		
	}
}
```

The "if statement" in Go is pretty much the same as the if statement in another programming language. It is used to make decisions based on a condition. It checks if something is true or false and executes different code depending on the result. 

Here is a simplified algorithm for working with an "if statement": 

- You use the keyword "if" followed by a condition in parentheses. 
- If the condition is true, the code inside the "if" block is executed. 
- If the condition is false, the code inside the "if" block is skipped. 

We use the "inequality operator" (!=) to check if the variable "want" and "got" have different values. 

Okay, so what are we gonna do if variables will have different values? We need to show a message that this test failed. This message will be printed in the terminal where we will run tests for our Go program. 

To show the message about the failed test, we need to use special methods that can be provided to us by Go's built-in testing framework. We can have access to Go's built-in testing framework's methods in each test function. All we need to do is just specify a parameter of the test function. So let's do it. In the function TestHelloWorld, we're gonna specify the parameter called "t," which has a special type, which is actually a struct. But don't focus too much on that now, we'll talk about this later.

```go
func TestHelloWorld(t *testing.T) {
	got := HelloWorld()
	want := "Hello, World!"

	if want != got {

	}
}
```

All we need to know is that parameter "t" includes all methods of Go's built-in test framework that we need. The parameter "t" has type (capital) "T," and that capital T type is provided by the testing package. That's why we write "testing.T," not just capital T. So basically, we have a package called "testing" which is Go's built-in package. 

However, to use type T from the package "testing," we need to be sure that we have access to things from package "testing" in OUR source file. For that, we need to import package "testing" in this file. In the beginning of the file, right below the package declaration, write a new line import "testing".

```go
package helloworld

import "testing"

// ...
```

Now, inside the if statement body, let's call a method that will show a message that the test failed.

```go
	if want != got {
		t.Errorf("Failed! Expected %q but received %q", want, got)
	}
```

We're using the method called "Errorf," and it takes the string as the first parameter. This string will be printed as a message that the test failed. You may notice that the string contains some special characters %q. The thing is that the method Errorf replaces all %q substrings by values from parameters after the first one. But there are plenty of combinations of substrings that begin with %q, and they all have their special meaning. All substrings that begin with % are called formatting verbs in Go. In this case, the formatting verb %q means that the value from the second parameter will be wrapped into double quotes and then inserted into the string from the first parameter. 

Okay, we finished with our first test, and it means that we've done the first step in the Test-driven development cycle. Now it's time to make step 2 - run the test. To run the test, we're gonna use the go tool. 

Open the terminal in the project folder. Now, in the terminal, type the command go test. It will show you the message:

```
❯ go test 
# hello-world [hello-world.test]
./hello_test.go:6:12: HelloWorld() (no value) used as value
FAIL    hello-world [build failed]
```

It says that the function that we're checking and calling inside the test does not return any values. And now we passed step 2 in our test-driven development cycle. Now it's time to do step 3 and write the simplest code that will pass our test. Open the file "helloworld.go" and go to the function declaration. Write type 'string' after paired parentheses.

```go
func HelloWorld() string {

}
```

Now Go is expecting that function HelloWorld returns a string value. However, we didn't write the code that returns any values from HelloWorld, and that's why your IDE will probably show you some signs that something is wrong. Okay, now let's write this line which means that function HelloWorld returns a string.

```go
func HelloWorld() string {
	return ""
}
```

Let's run the test again (`go test`). Okay, we did enough to call our function inside the test properly, and finally, we see that our test case failed.

```bash
❯ go test 
--- FAIL: TestHelloWorld (0.00s)
    hello_test.go:10: Failed! Expected "Hello, World!" but received ""
FAIL
exit status 1
FAIL    hello-world     0.624s
```

That's alright. We just need to make sure that our test works correctly. Now it's time to write the code inside HelloWorld to pass the test.

```go
func HelloWorld() string {
	return "Hello, World!"
}
```

Great! There are not many things to refactor, so we can skip step 5. We successfully finished our test-development cycle, and now let's complete our program so that it will print the string "Hello, World!" in the terminal. 

We need to run our program, and for that, we can use a special command provided by go tool. But it's not gonna work for several reasons. First of all, we didn't call our HelloWorld anywhere and didn't use the method that prints the string in the terminal.
But where do we need to write this code? We cannot just call the function inside the file we want, like in programming languages such as JavaScript or Python. In Go, we need to have a function called "main," which is an entry point to our program. This function will be called first when we run our program. Let's write this function in the file "hello.go".

```go
func main() {
	
}
```

Now we need to use some method that will print the string in the terminal. Go has a built-in package called "fmt," which stands for formatting. The package "fmt" has a bunch of methods for formatting and printing strings. The method that we need is called "Println," so let's use it in our code.

```go
func main() {
	fmt.Println(HelloWorld())
}
```

Don't forget about the import of the package which method you're gonna use in your code.

```go
import "fmt"
```

The method Println takes the string as the first argument and prints it in the terminal. But it also prints a string on a newline. That's what the 'ln' in the name of the method stands for. 

Okay, now it's time to run our program. Open the terminal in the root of the project's folder and type go run hello.go. It will show you the message:

```
❯ go run hello.go 
package command-line-arguments is not a main package
```

Well, the thing is that every Go program should also have a main package the same way as the main function. So the main package is an entry point in a Go program. When you run a Go program by command go run, it will look at the package "main" first and will execute the function main inside that package. Remember that we declared that our package name is "hello" in hello.go and hello_test.go files. So all we need to do is just to create a file called "main.go" in the root of the project folder.

```bash
cd ..
touch main.go
```

Inside the new file write the package declaration line.

```go
package main
```

And move the function "main" from the file helloworld.go to main.go.

```go
package main

func main() {
	fmt.Println(HelloWorld())
}
```

Since we are using the function HelloWorld from the package "helloworld," we need to import this package in the file main.go; also, don't forget about importing the package "fmt."

```go
package main

import (
	"fmt"
	"go-sandbox/helloworld"
)

func main() {
	fmt.Println(HelloWorld())
}
```

But this is still not enough for this code to work. In Go, if you use some function from another package, you need to write the package's name before the name of the function on the line where you call it; like that:

```go
	fmt.Println(helloworld.HelloWorld())
```

Final code:

```go
package main

import (
	"fmt"
	"go-sandbox/helloworld"
)

func main() {
	fmt.Println(helloworld.HelloWorld())
}
```

Let's try to run our program again, but this time you need to run the file main.go in the project root folder instead of helloworld.go in the helloworld package folder.

```
go run main.go
```

This should work perfectly. You can also run it by using go run . instead of writing the file name properly since it will look at package main first anyway.

```bash
go run .
```

The full source code is available on GitHub repository [GGFT](https://github.com/kovalevsky0/ggft).

