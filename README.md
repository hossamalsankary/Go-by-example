
# Go Mega-Guide (Updated)

##### This is a guide to the Go programming language. It covers the language itself, the standard library and tools .
---

## Table of Contents

1. [Getting Started](#getting-started)  
2. [Arrays, Slices, and Maps](#arrays-slices-and-maps)  
3. [Functions](#functions)  
4. [Pointers](#pointers)  
5. [Strings and Runes](#strings-and-runes)  
6. [Structs, Methods, and Embedding](#structs-methods-and-embedding)  
7. [Interfaces](#interfaces)  
8. [Generics](#generics)  
9. [Errors and Error Handling](#errors-and-error-handling)  
10. [Concurrency](#concurrency)  
11. [Sorting](#sorting)  
12. [String and Text Processing](#string-and-text-processing)  
13. [Data Formats and Parsing](#data-formats-and-parsing)  
14. [File and Directory Operations](#file-and-directory-operations)  
15. [Testing and Benchmarking](#testing-and-benchmarking)  
16. [Command-Line and Environment](#command-line-and-environment)  
17. [Networking and Processes](#networking-and-processes)  
18. [Additional References](#additional-references)

---

## 1. Getting Started

### Hello World

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

- For a quick walkthrough, see [Go by Example: Hello World](https://gobyexample.com/hello-world).

### Values, Variables, and Constants

```go
var a int = 10
var b = "Go"
c := 3.14 // short assignment inside functions

const Pi = 3.14159
```
- Check [Go by Example: Variables](https://gobyexample.com/variables) and [Go by Example: Constants](https://gobyexample.com/constants).

### Control Structures

**If/Else**:
```go
if a > 5 {
    // ...
} else {
    // ...
}
```
[Go by Example: If/Else](https://gobyexample.com/if-else)

**Switch**:
```go
switch day {
case "Saturday", "Sunday":
    // ...
default:
    // ...
}
```
[Go by Example: Switch](https://gobyexample.com/switch)

**For**:
```go
for i := 0; i < 5; i++ {
    // ...
}
```
[Go by Example: For](https://gobyexample.com/for)

---

## 2. Arrays, Slices, and Maps

### Arrays
Fixed-length container:
```go
var arr [3]int
arr[0] = 10
```
[Go by Example: Arrays](https://gobyexample.com/arrays)

### Slices
Dynamic length:
```go
nums := []int{1, 2, 3}
nums = append(nums, 4)
```
[Go by Example: Slices](https://gobyexample.com/slices)

### Maps
Key-value pairs:
```go
scores := make(map[string]int)
scores["Alice"] = 95
val := scores["Alice"]
```
[Go by Example: Maps](https://gobyexample.com/maps)

---

## 3. Functions

### Multiple Return Values
```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}
```
[Go by Example: Multiple Return Values](https://gobyexample.com/multiple-return-values)

### Variadic Functions
```go
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}
```
[Go by Example: Variadic Functions](https://gobyexample.com/variadic-functions)

### Closures & Recursion
```go
inc := func() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}()

fmt.Println(inc()) // 1
fmt.Println(inc()) // 2
```
[Go by Example: Closures](https://gobyexample.com/closures)

---

## 4. Pointers

```go
x := 10
p := &x
fmt.Println(*p) // 10
*x = 20
fmt.Println(x)  // 20
```

- Detailed pointer usage is covered in *Efficient Go*, discussing how to reduce unnecessary allocations.

---

## 5. Strings and Runes

```go
s := "Hello, 世界"
r := []rune(s)
fmt.Println(len(s), len(r)) // byte length vs rune length
```
[Go by Example: Strings](https://gobyexample.com/strings)

---

## 6. Structs, Methods, and Embedding

### Structs

```go
type Person struct {
    Name string
    Age  int
}
```
[Go by Example: Structs](https://gobyexample.com/structs)

### Methods

```go
func (p Person) Greet() {
    fmt.Printf("Hello, my name is %s
", p.Name)
}
```
[Go by Example: Methods](https://gobyexample.com/methods)

### Embedding

```go
type Animal struct {
    Name string
}

type Dog struct {
    Animal
    Breed string
}
```
Discussed more in *Effective Go* under “Embedding.”

---

## 7. Interfaces

```go
type Shape interface {
    Area() float64
}

type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}
```
[Go by Example: Interfaces](https://gobyexample.com/interfaces)

---

## 8. Generics

Introduced in Go 1.18+:
```go
func Map[T any](arr []T, f func(T) T) []T {
    result := make([]T, len(arr))
    for i, v := range arr {
        result[i] = f(v)
    }
    return result
}
```
- *Efficient Go* covers generics performance considerations.

---

## 9. Errors and Error Handling

```go
func doSomething() error {
    return fmt.Errorf("some error")
}

if err := doSomething(); err != nil {
    fmt.Println("Error:", err)
}
```
[Go by Example: Errors](https://gobyexample.com/errors)

### Panic, Defer, Recover
```go
defer func() {
    if r := recover(); r != nil {
        fmt.Println("Recovered:", r)
    }
}()
panic("something went wrong")
```
[Go by Example: Panic](https://gobyexample.com/panic)

---

## 10. Concurrency

### Goroutines

```go
go func() {
    fmt.Println("Concurrent!")
}()
```
[Go by Example: Goroutines](https://gobyexample.com/goroutines)

### Channels

```go
ch := make(chan int)
go func() { ch <- 42 }()
value := <-ch
```
[Go by Example: Channels](https://gobyexample.com/channels)

---

## File Export

Markdown has been updated with relevant links. Now saving this content to a file named `Updated_Go_Mega_Guide.md`.

---

