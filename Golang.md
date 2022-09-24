---
title: Golang
tags:
  - golang
public: true
date: 2020-12-16
---

# Golang

**Go** or **Golang** is a programming language that designed at *Google*. 

The language can be described by these following items:

* it's [compiled language](https://en.wikipedia.org/wiki/Compiled_language)
* similar to **C** language by syntax
* the language has:
  * [memory safety](https://en.wikipedia.org/wiki/Memory_safety)
  * [garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))
  * [structural typing](https://en.wikipedia.org/wiki/Structural_type_system)
* [Official website](https://golang.org/)

## cli

- **go run \<module\> - run the file/module as a program
- **go test** - run tests
- **go mod init \<module\>** - initialise a [module](https://go.dev/doc/modules/gomod-ref)


## godoc

It provides an ability to open Go documentation locally and even offline

**Install**

```
go install golang.org/x/tools/cmd/godoc@latest
```

**Run**

```
godoc -http :8000
```

## "Hello, World!" Example

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, World!")
}
```

- **Go** has **standard library** that is built-in modules of functions
- for example: **fmt** is a built-in package
- **Println(value)** is a method that prints *value* in a console/terminal and also prints break line in the end
- **func main()** is a function
	- it is a main function of the module and it will be executed automatically when you run command **go run hello.go**

## Variables

### Declaring 

Example 1:

```go
func main() {
	var greetingText string
	greetingText = "Hello, World!"

	fmt.Println(greetingText)
}
```

Example 2:

```go
func main() {
	var greetingText string = "Hello, World!"

	fmt.Println(greetingText)
}
```

Example 3 (with **type inference**):

```go
func main() {
	greetingText := "Hello, World!"

	fmt.Println(greetingText)
}
```

- type inference is when you declare variable without specifying a type
- the variables type is *inferred* by the value

## Types: string

Declaring a string variable

Example 1:

```go
func main() {
	var greetingText string
	greetingText = "Hello, World!"

	fmt.Println(greetingText)
}
```

Example 2:

```go
func main() {
	var greetingText string = "Hello, World!"

	fmt.Println(greetingText)
}
```

Example 3 (with **type inference**):

```go
func main() {
	greetingText := "Hello, World!"

	fmt.Println(greetingText)
}
```

### concatenating 

An example:

```go
func main() {
	firstName := "Max"
	lastName := "K"

	fullName := firstName + " " + lastName

	fmt.Println(fullName) // Max K
}
```

### add a number to a string

JavaScript devs be like:

![](https://i.giphy.com/media/32mC2kXYWCsg0/giphy.webp)

```go
func main() {
	numb := "1"

	fmt.Println(numb + 7) // invalid operation: numb + 7 (mismatched types string and untyped int)
}
```


## Types: int

### Declaring an int (number) variable

Example 1:

```go
func main() {
	var luckyNumber int
	luckyNumber = 17

	fmt.Println(luckyNumber)
}
```

Example 2:

```go
func main() {
	var luckyNumber int = 17

	fmt.Println(luckyNumber)
}
```

Example 3 (with type inference)

```go
func main() {
	luckyNumber := 17

	fmt.Println(luckyNumber)
}
```

### Interesting stuff

if I declare variable like this:

```go
func main() {
	var luckyNumber int

	fmt.Println(luckyNumber)
}
```

it will print the result: **0**

### Operations: add (+)

```go
func main() {
	luckyNumber := 17
	anotherOneLuckyNumber := luckyNumber + 8

	fmt.Println(luckyNumber) // 17
	fmt.Println(anotherOneLuckyNumber) // 25
}
```

**Reassigning a variable**

```go
func main() {
	luckyNumber := 17
	anotherOneLuckyNumber := luckyNumber + 8

	fmt.Println(luckyNumber)           // 17
	fmt.Println(anotherOneLuckyNumber) // 25

	// reassigning a variable
	anotherOneLuckyNumber = 33

	fmt.Println(anotherOneLuckyNumber) // 33
}
```

### Operations: multiply (\*)

```go
func main() {
	luckyNumber := 17
	anotherOneLuckyNumber := luckyNumber * 8

	fmt.Println(luckyNumber)           // 17
	fmt.Println(anotherOneLuckyNumber) // 136
}
```

## Types: float64

An example: 

```go
func main() {
	luckyNumber := 17
	anotherOneLuckyNumber := luckyNumber / 8

	fmt.Println(luckyNumber)           // 17
	fmt.Println(anotherOneLuckyNumber) // 3
}
```

- **anotherOneLuckyNumber** will still be *integer* (int), no numbers after decimal point
- we should use **float64** type to have full number with part after point

**Using float64**

We need to declare that variable **anotherOneLuckyNumber** should be with type **float** not **int**:

```go
func main() {
	luckyNumber := 17

	var anotherOneLuckyNumber float64 = luckyNumber / 8 // cannot use luckyNumber / 8 (value of type int) as float64 value in variable

	fmt.Println(luckyNumber)
	fmt.Println(anotherOneLuckyNumber)
}
```

- there will be an error
- we need to convert **luckyNumber** into float64 but only in operation of dividing with 8 - for this we can use a *command* (function) **float64**

```go
func main() {
	luckyNumber := 17

	var anotherOneLuckyNumber float64 = float64(luckyNumber) / 8

	fmt.Println(luckyNumber)           // 17
	fmt.Println(anotherOneLuckyNumber) // 2.125
}
```

## Types: float32

The difference between types **float32** and **float64** is how many numbers will you have after the point sign. 

For example, the result of operation will be:

- 17 / 7 =
	- float32
		- 2.4285715
	- float64
		- 2.4285714285714284

An example with **float32**:

```go
func main() {
	luckyNumber := 17

	var anotherOneLuckyNumber float32 = float32(luckyNumber) / 7

	fmt.Println(anotherOneLuckyNumber) // 2.4285715
}
```

An example with **float64**:

```go
func main() {
	luckyNumber := 17

	var anotherOneLuckyNumber float64 = float64(luckyNumber) / 7

	fmt.Println(anotherOneLuckyNumber) // 2.4285714285714284
}
```


## Types: rune

single unicode character

An example:

> notice that there is a single quotes (\'') instead of double quotes (\"") because double quotes are used for string type

```go
func main() {
	var someRune rune = '👍'

	fmt.Println(someRune) // 128077
}
```

- **128077** is a special identifier for the sign '👍'

to print the sign 👍 as it is instead of integer we need to convert it to a string by function **string()**

```go
func main() {
	var someRune rune = '👍'

	fmt.Println(string(someRune)) // 👍
}
```

## Types: byte

single byte (ASCII character)

An example:

> notice that there is a single quotes (\'') instead of double quotes (\"") because double quotes are used for string type

```go
func main() {
	var someByte byte = 'c'

	fmt.Println(someByte) // 99
}
```

To print a 'c' value as it is we need to convert value to a string by using function **string()**

```go
func main() {
	var someByte byte = 'c'

	fmt.Println(string(someByte)) // c
}
```

## Types: byte vs rune

A type **byte** is a *smallest* type in Go so you can initialise a variable only with one character (letter or number) but not with something like emoji or special character/symbol like '€'

```go
var someByte byte = '👍' // cannot use '👍' (untyped rune constant 128077) as byte value in variable declaration (overflows)
```

so you need to use type **rune** for such characters/symbols

## Formatting strings (fmt)

- (package) **fmt**
	- print the argument **str** in command line (terminal)
		- .Print(str) - prints **str* as it is
		- .Printf(formatStr, args...)
		- .Println(str) - as .Print(str) but adds "\\n" in the end of string
	- return a new formatted string
		- .Sprint(str) - returns a new formatted string, uses default formats
		- .Sprintf(formatStr, args...)
		- .Sprintln(str) - as .Sprint(str) but adds "\\n" in the end of string
	- write a new formatted string to specified **writer** (for example: a file or command-line/terminal)
		- .Fprint(writer, str) - writes a new formatted string to a **writer** (file, command-line, etc)
		- .Fprintf(writer, formatStr, args...)
		- .Fprintln(writer, str) - as .Fprint(str) but adds "\\n" in the end of string

### Examples

**Printf(formatStr, args...)**

```go
func main() {
	nickname := "byteski"
	level := 77

	fmt.Printf("Greetings, %v (%v lvl)!", nickname, level) // Greetings, byteski (77 lvl)!
}
```

**Sprintf(formatStr, args...)**

```go
func main() {
	nickname := "byteski"
	level := 77
	greeting := fmt.Sprintf("Greetings, %v (%v lvl)!", nickname, level)

	fmt.Printf(greeting) // Greetings, byteski (77 lvl)!
}
```

**Fprintf(writer, formatStr, args...)**

```go
import (
	"fmt"
	"os"
)

func main() {
	nickname := "byteski"
	level := 77

	fmt.Fprintf(os.Stdout, "Greetings, %v (%v lvl)!", nickname, level) // Greetings, byteski (77 lvl)!
}
```

## Modules vs Packages

- Go code is organised in **modules** and **packages**
	- modules
		- are bigger than packages
		- has unique identifier (name)
		- can be distributed (as a library)
		- every Go project is a module
		- projects can use multiple modules
		- created and managed by commands **go mod \<smth\>** and file **go.mod**
	- packages
		- modules contains packages (at least one package - for example **main**)
		- package can have multiple files
		- packages are stored in subfolders in a module
		- can be imported

## Pascal Case vs Camel Case naming of variables

- pascal case
	- `var Greeting string = "Hello there!"`
		- in Go it means that variable will be used also outside of the *package* where was declared
- camel case
	- `var greeting string = "Hello there!"`
		- in Go it means that variable will be used only inside the same package as where it was declared

An example:

**camel case**

*variables.go*

```go
package greeting

var greetingText = "Hello there!"
```

*hello.go*

```go
package main

import (
	"fmt"
	"new-module/greeting"
)

func main() {
	// Error: greetingText not exported by package greeting
	fmt.Println(greeting.GreetingText)
}
```

**pascal case**

*variables.go*

```go
package greeting

var GreetingText = "Hello there!"
```

*hello.go*

```go
package main

import (
	"fmt"
	"new-module/greeting"
)

func main() {
	fmt.Println(greeting.GreetingText) // Hello there!
}
```

## Constants

Constant is like a variable but it cannot be reassigned. Constant will have the same value.

```go
func main() {
	const COUNT = 10
	
	COUNT = 15 // Error: cannot assign to COUNT (untyped int constant 10)

	fmt.Println(COUNT)
}
```


## Writing tests in Go

- File should have name like "xxx_test.go"
- The test function must start with word "Test"
```go
func TestHello(t *testing.T) {
}
```
- The test function has one argument *"t \*testing.T"*
- Need to import "testing" to use argument *"t \*testing.T"*

### t.Errorf()

- t.Errorf() - prints message fail the test

```go
func TestHello(t *testing.T) {
	result := Hello()
	expectedValue := "Hello, World!2"

	if result != expectedValue {
		t.Errorf("expected `%s` but got `%s`", expectedValue, result)
		/* will print:
		--- FAIL: TestHello (0.00s)
		hello_test.go:10: expected `Hello, World!2` but got `Hello, World!`
		*/
	}
}
```

### t.Run()

> it seems like an equivalent of "describe" or "it" in [[JestJS]]
> 
```go
func TestHello(t *testing.T) {
	t.Run("should return `Hello, <name>!`", func(t *testing.T) {
		result := Hello("Max")
		expectedValue := "Hello, Max!"

		if result != expectedValue {
			t.Errorf("expected `%s` but got `%s`", expectedValue, result)
		}
	})
}
```

### t.Helper()

- it declares that the method where it was called is a helper function
- because of this the line number of the assertion (where *helper* function is called) will be printed out

```go
assert := func(t testing.TB, expectedValue string, resultValue string) {
	t.Helper()

	if resultValue != expectedValue {
		t.Errorf("expected `%s` but got `%s`", expectedValue, resultValue)
	}
}

// ...

t.Run("should return `Hello, <name>!`", func(t *testing.T) {
	result := Hello("Max")
	expectedValue := "Hello, Max!"

	assert(t, expectedValue, result)
})
```

