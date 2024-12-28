# Go Mega-Guide (Updated and Expanded)

This is a guide to the Go programming language. It covers the language itself, the standard library, tools, and advanced topics to help you build robust, efficient, and maintainable Go applications.

---

## Table of Contents

1. [Getting Started](#1-getting-started)  
2. [Arrays, Slices, and Maps](#2-arrays-slices-and-maps)  
3. [Functions](#3-functions)  
4. [Pointers](#4-pointers)  
5. [Strings and Runes](#5-strings-and-runes)  
6. [Structs, Methods, and Embedding](#6-structs-methods-and-embedding)  
7. [Interfaces](#7-interfaces)  
8. [Generics](#8-generics)  
9. [Errors and Error Handling](#9-errors-and-error-handling)  
10. [Concurrency](#10-concurrency)  
11. [Sorting](#11-sorting)  
12. [String and Text Processing](#12-string-and-text-processing)  
13. [Data Formats and Parsing](#13-data-formats-and-parsing)  
14. [File and Directory Operations](#14-file-and-directory-operations)  
15. [Testing and Benchmarking](#15-testing-and-benchmarking)  
16. [Command-Line and Environment](#16-command-line-and-environment)  
17. [Networking and Processes](#17-networking-and-processes)  
18. [Reflection](#18-reflection)  
19. [Synchronization and Advanced Concurrency](#19-synchronization-and-advanced-concurrency)  
20. [Date and Time](#20-date-and-time)  
21. [Logging](#21-logging)  
22. [Package Management](#22-package-management)  
23. [Performance and Profiling](#23-performance-and-profiling)  
24. [Additional References](#24-additional-references)

---

## 1. Getting Started

### Hello World

```go
```go
package main
```

```go
import "fmt"
```

```go
func main() {
    fmt.Println("Hello, World!")
}
```

    -    For a quick walkthrough, see Go by Example: Hello World.

Values, Variables, and Constants

```go
var a int = 10      // explicit type
var b = "Go"        // inferred type (string)
c := 3.14           // short assignment, only allowed inside functions
```

```go
const Pi = 3.14159  // constant
```

    -    References:
    -    Go by Example: Variables
    -    Go by Example: Constants

Control Structures

If/Else:

if a > 5 {
    // ...
} else {
    // ...
}

    -    Go by Example: If/Else

Switch:

switch day {
case "Saturday", "Sunday":
    fmt.Println("It's the weekend!")
default:
    fmt.Println("It's a weekday.")
}

    -    Go by Example: Switch

For:

for i := 0; i < 5; i++ {
    fmt.Println(i)
}

    -    Go by Example: For

Advanced Example: Looping Over Command-Line Args

```go
package main
```

```go
import (
    "fmt"
    "os"
)
```

```go
func main() {
    // Start from index 1 to skip the program name itself
    for i, arg := range os.Args[1:] {
        fmt.Printf("Arg %d: %s\n", i+1, arg)
    }
}
```

    -    This example shows how to iterate over command-line arguments passed to your program.

2. Arrays, Slices, and Maps

Arrays

Fixed-length container:

```go
var arr [3]int
arr[0] = 10
arr[1] = 20
arr[2] = 30
```

// Declaration with initialization
nums := [5]int{1, 2, 3, 4, 5}

    -    Go by Example: Arrays

Advanced Example: Multidimensional Arrays

matrix := [2][3]int{
    {1, 2, 3},
    {4, 5, 6},
}
fmt.Println(matrix) // [[1 2 3] [4 5 6]]

Slices

Dynamic-length, built on top of arrays:

nums := []int{1, 2, 3}
nums = append(nums, 4, 5)

    -    Go by Example: Slices

Advanced Example: Sub-slicing and Capacity

arr := []int{10, 20, 30, 40, 50}
sliceA := arr[1:4]   // elements at indices 1, 2, 3 → [20, 30, 40]
sliceB := sliceA[:2] // first 2 elements of sliceA → [20, 30]

fmt.Println("sliceA:", sliceA, "len:", len(sliceA), "cap:", cap(sliceA))
fmt.Println("sliceB:", sliceB, "len:", len(sliceB), "cap:", cap(sliceB))

// Modifying an element through sliceB
sliceB[0] = 999
fmt.Println("arr:", arr) // arr now shows [10 999 30 40 50]

    -    Demonstrates that slices share the underlying array memory.

Maps

Key-value pairs:

scores := make(map[string]int)
scores["Alice"] = 95
scores["Bob"] = 88

val, exists := scores["Alice"]
if exists {
    fmt.Println("Alice's score:", val)
} else {
    fmt.Println("No score for Alice")
}

    -    Go by Example: Maps

Advanced Example: Using Structs as Map Values

```go
type Grade struct {
    Score   int
    Comment string
}
```

grades := map[string]Grade{
    "Alice": {95, "Great job"},
    "Bob":   {88, "Good effort"},
}

// Updating a map entry
grades["Alice"] = Grade{97, "Improved score"}

3. Functions

Multiple Return Values

```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}
```

res, err := divide(10, 2)
if err != nil {
    // handle error
}
fmt.Println(res)

    -    Go by Example: Multiple Return Values

Variadic Functions

```go
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}
```

fmt.Println(sum(1,2,3,4,5)) // 15

    -    Go by Example: Variadic Functions

Closures & Recursion

inc := func() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}()

fmt.Println(inc()) // 1
fmt.Println(inc()) // 2

    -    Go by Example: Closures

Advanced Example: Higher-Order Functions

// Filter returns a new slice containing all elements of the
// input slice for which the predicate returns true.
```go
func Filter(nums []int, predicate func(int) bool) []int {
    var result []int
    for _, n := range nums {
        if predicate(n) {
            result = append(result, n)
        }
    }
    return result
}
```

filtered := Filter([]int{1,2,3,4,5,6}, func(n int) bool {
    return n%2 == 0
})
fmt.Println(filtered) // [2 4 6]

    -    Shows passing functions as parameters and using them to transform data.

4. Pointers

x := 10
p := &x
fmt.Println(*p) // 10
*p = 20
fmt.Println(x)  // 20

    -    Details on pointer usage and best practices can be found in Effective Go.

Advanced Example: Pointer to a Struct

```go
type Person struct {
    Name string
    Age  int
}
```

```go
func birthday(p *Person) {
    p.Age++
}
```

me := Person{"John", 30}
birthday(&me)
fmt.Println(me) // {John 31}

    -    Demonstrates passing a struct pointer to a function to mutate the original data.

5. Strings and Runes

s := "Hello, 世界"
r := []rune(s)
fmt.Println(len(s), len(r)) // byte length vs rune length

    -    Go by Example: Strings

Advanced Example: Unicode/Emoji Handling

emoji := "😁"
fmt.Println(len(emoji))                    // 4 bytes (UTF-8 encoded)
fmt.Println(utf8.RuneCountInString(emoji)) // 1 rune

for i, runeVal := range emoji {
    fmt.Printf("Index %d: %U '%c'\n", i, runeVal, runeVal)
}

    -    This illustrates dealing with multi-byte UTF-8 characters and runes.

6. Structs, Methods, and Embedding

Structs

```go
type Person struct {
    Name string
    Age  int
}
```

p := Person{Name: "Alice", Age: 25}
fmt.Println(p.Name)

    -    Go by Example: Structs

Methods

```go
func (p Person) Greet() {
    fmt.Printf("Hello, my name is %s\n", p.Name)
}
```

p.Greet()

    -    Go by Example: Methods

Advanced Example: Pointer Receiver vs Value Receiver

```go
func (p Person) ValueMethod() {
    p.Age += 1 // modifies a copy
}
```

```go
func (p *Person) PointerMethod() {
    p.Age += 1 // modifies the original
}
```

    -    Use a pointer receiver to mutate the original struct.

Embedding

```go
type Animal struct {
    Name string
}
```

```go
func (a Animal) Speak() {
    fmt.Println("Some generic sound!")
}
```

```go
type Dog struct {
    Animal
    Breed string
}
```

```go
func (d Dog) Speak() {
    fmt.Printf("%s says: Woof!\n", d.Name)
}
```

d := Dog{Animal{"Rex"}, "Golden Retriever"}
d.Speak() // Rex says: Woof!

    -    Discussed more in Effective Go.

7. Interfaces

```go
type Shape interface {
    Area() float64
}
```

```go
type Rectangle struct {
    Width, Height float64
}
```

```go
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}
```

    -    Go by Example: Interfaces

Advanced Example: Empty Interface and Type Assertion

```go
func describe(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Println("Integer:", v)
    case string:
        fmt.Println("String:", v)
    default:
        fmt.Printf("Unknown type %T\n", v)
    }
}
```

describe(42)        // Integer: 42
describe("hello")   // String: hello
describe(3.14)      // Unknown type float64

    -    This pattern is common when handling arbitrary data structures (e.g., JSON decoding).

8. Generics

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

    -    Efficient Go covers generics performance considerations.

Advanced Example: Constraints and Multiple Type Parameters

```go
type Number interface {
    ~int | ~float64
}
```

```go
func Add[T Number](a, b T) T {
    return a + b
}
```

```go
func main() {
    fmt.Println(Add[int](3, 5))         // 8
    fmt.Println(Add[float64](2.5, 3.5)) // 6.0
}
```

    -    Demonstrates how to create custom constraints (Number) and how to restrict types.

9. Errors and Error Handling

```go
func doSomething() error {
    return fmt.Errorf("some error")
}
```

if err := doSomething(); err != nil {
    fmt.Println("Error:", err)
}

    -    Go by Example: Errors

Panic, Defer, Recover

defer func() {
    if r := recover(); r != nil {
        fmt.Println("Recovered from:", r)
    }
}()
panic("something went wrong")

    -    Go by Example: Panic

Advanced Example: Custom Error Types

```go
type MyError struct {
    Code    int
    Message string
}
```

```go
func (e *MyError) Error() string {
    return fmt.Sprintf("Error %d: %s", e.Code, e.Message)
}
```

```go
func riskyOperation() error {
    return &MyError{404, "Resource not found"}
}
```

```go
func main() {
    err := riskyOperation()
    if err != nil {
        fmt.Println(err)
        if myErr, ok := err.(*MyError); ok {
            fmt.Println("Error Code:", myErr.Code)
        }
    }
}
```

    -    Creating your own error types is helpful for more granular error handling.

10. Concurrency

Goroutines

go func() {
    fmt.Println("Concurrent!")
}()

    -    Go by Example: Goroutines

Channels

ch := make(chan int)
go func() {
    ch <- 42
}()

value := <-ch
fmt.Println(value) // 42

    -    Go by Example: Channels

Advanced Example: Select Statement

```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Printf("Worker %d started job %d\n", id, j)
        time.Sleep(time.Second)
        fmt.Printf("Worker %d finished job %d\n", id, j)
        results <- j * 2
    }
}
```

```go
func main() {
    jobs := make(chan int, 5)
    results := make(chan int, 5)
```

    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs)

    for a := 1; a <= 5; a++ {
        <-results
    }
}

    -    Demonstrates how to distribute jobs among multiple goroutines with channels.

11. Sorting

The standard library provides sort for built-in sorting functionality:

```go
import "sort"
```

nums := []int{5, 2, 6, 3, 1}
sort.Ints(nums)
fmt.Println(nums) // [1 2 3 5 6]

Advanced Example: Custom Sorting

```go
type Person struct {
    Name string
    Age  int
}
```

people := []Person{
    {"Alice", 25},
    {"Bob", 30},
    {"Charlie", 20},
}

sort.Slice(people, func(i, j int) bool {
    return people[i].Age < people[j].Age
})

// people is now sorted by Age ascending
fmt.Println(people)

    -    Use sort.SliceStable if you want to preserve the order of equal elements.

12. String and Text Processing
    -    Basic operations like trimming, splitting, and joining are in the standard library (strings package).
    -    Regular expressions via the regexp package.

Advanced Example: Using strings.Builder

```go
import (
    "fmt"
    "strings"
)
```

```go
func main() {
    var sb strings.Builder
    for i := 0; i < 5; i++ {
        sb.WriteString("Go!")
    }
    fmt.Println(sb.String()) // Go!Go!Go!Go!Go!
}
```

    -    strings.Builder is useful for efficient string concatenation in a loop.

Advanced Example: Regular Expressions

```go
import (
    "fmt"
    "regexp"
)
```

re := regexp.MustCompile(`(\w+)@(\w+)\.com`)
matches := re.FindStringSubmatch("contact me at test@example.com")
if len(matches) > 0 {
    fmt.Println("Full match:", matches[0])
    fmt.Println("Username:", matches[1])
    fmt.Println("Domain:", matches[2])
}

13. Data Formats and Parsing
    -    encoding/json, encoding/xml, encoding/csv
    -    YAML support via third-party packages like gopkg.in/yaml.v2

Advanced Example: JSON Tag Customization

```go
type Person struct {
    Name   string `json:"name"`
    Age    int    `json:"age"`
    hidden string // not exported
}
```

p := Person{Name: "Alice", Age: 25}
data, _ := json.Marshal(p)
fmt.Println(string(data)) // {"name":"Alice","age":25}

    -    Unexported fields (hidden) are not marshaled.

14. File and Directory Operations
    -    The os package provides file operations: Create, Open, Read, Write, etc.
    -    The io/ioutil (or os / io in modern Go) package helps with quick reads and writes.

Advanced Example: Walking a Directory

```go
import (
    "fmt"
    "io/fs"
    "path/filepath"
)
```

```go
func main() {
    err := filepath.WalkDir(".", func(path string, d fs.DirEntry, err error) error {
        if err != nil {
            return err
        }
        fmt.Println(path)
        return nil
    })
    if err != nil {
        panic(err)
    }
}
```

    -    Iterates over all files and directories starting from . (current directory).

15. Testing and Benchmarking
    -    Go has a built-in testing framework (testing package).
    -    Run tests with go test.
    -    Benchmarks with go test -bench ..

Advanced Example: Table-Driven Tests

```go
func Add(a, b int) int {
    return a + b
}
```

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        a, b     int
        expected int
    }{
        {1, 2, 3},
        {2, 3, 5},
        {10, 5, 15},
    }
    for _, tt := range tests {
        result := Add(tt.a, tt.b)
        if result != tt.expected {
            t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, result, tt.expected)
        }
    }
}
```

    -    A common, clean approach to test multiple inputs/outputs.

16. Command-Line and Environment
    -    Use flag to parse command-line flags.
    -    os.Args or third-party libraries (like Cobra) for more complex CLI apps.

Advanced Example: flag Package

```go
import (
    "flag"
    "fmt"
)
```

```go
func main() {
    name := flag.String("name", "Go Developer", "Your name")
    age := flag.Int("age", 0, "Your age")
    flag.Parse()
```

    fmt.Printf("Name: %s, Age: %d\n", *name, *age)
}

    -    Example usage: go run main.go -name="Alice" -age=25

17. Networking and Processes
    -    net/http for HTTP servers and clients.
    -    os/exec for running external commands.
    -    net package for TCP/UDP.

Advanced Example: HTTP Server with Handlers

```go
import (
    "fmt"
    "net/http"
)
```

```go
func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Hello from Go!")
}
```

```go
func main() {
    http.HandleFunc("/hello", helloHandler)
    fmt.Println("Starting server at :8080")
    http.ListenAndServe(":8080", nil)
}
```

    -    Access at http://localhost:8080/hello.

18. Reflection
    -    The reflect package lets you inspect types at runtime.
    -    Useful for writing libraries or frameworks that need to work with arbitrary data types.

Example: Inspecting a Struct with Reflection

```go
import (
    "fmt"
    "reflect"
)
```

```go
type Product struct {
    Name  string
    Price float64
}
```

```go
func main() {
    p := Product{Name: "Book", Price: 9.99}
    val := reflect.ValueOf(p)
```

    for i := 0; i < val.NumField(); i++ {
        fmt.Printf("Field %d: %v\n", i, val.Field(i))
    }
}

    -    Reflection is powerful but should be used sparingly due to complexity and performance overhead.

19. Synchronization and Advanced Concurrency

WaitGroup

```go
import (
    "fmt"
    "sync"
)
```

```go
func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Worker %d started\n", id)
    // do some work...
    fmt.Printf("Worker %d done\n", id)
}
```

```go
func main() {
    var wg sync.WaitGroup
    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }
    wg.Wait() // wait for all workers to finish
}
```

Mutex

```go
import (
    "fmt"
    "sync"
)
```

```go
var (
    count int
    mu    sync.Mutex
)
```

```go
func increment(wg *sync.WaitGroup) {
    defer wg.Done()
    mu.Lock()
    count++
    mu.Unlock()
}
```

```go
func main() {
    var wg sync.WaitGroup
    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go increment(&wg)
    }
    wg.Wait()
    fmt.Println("Final count:", count)
}
```

Advanced Concurrency Patterns: Pipelines and Fan-out

```go
func generator(nums ...int) <-chan int {
    out := make(chan int)
    go func() {
        for _, n := range nums {
            out <- n
        }
        close(out)
    }()
    return out
}
```

```go
func square(in <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for n := range in {
            out <- n * n
        }
        close(out)
    }()
    return out
}
```

```go
func main() {
    in := generator(1, 2, 3, 4, 5)
    out := square(in)
    for n := range out {
        fmt.Println(n) // 1, 4, 9, 16, 25
    }
}
```

    -    Shows how to chain goroutines together into a pipeline.

20. Date and Time
    -    time package offers time parsing, formatting, and operations.

Example: Formatting Dates

now := time.Now()
fmt.Println(now.Format("2006-01-02 15:04:05"))

    -    Remember the reference date/time for formatting in Go is Mon Jan 2 15:04:05 MST 2006.

Example: Timers and Tickers

ticker := time.NewTicker(1 * time.Second)
go func() {
    for t := range ticker.C {
        fmt.Println("Tick at", t)
    }
}()

time.Sleep(5 * time.Second)
ticker.Stop()
fmt.Println("Ticker stopped")

21. Logging
    -    Basic logging with log package.
    -    Structured logging with third-party packages like zap or logrus.

```go
import "log"
```

```go
func main() {
    log.Println("This is a simple log message.")
    log.Fatalf("This is a fatal log message with exit.")
}
```

    -    log.SetFlags and log.SetPrefix let you customize output format and prefixes.

22. Package Management
    -    Go modules introduced in Go 1.11+.
    -    go mod init, go mod tidy, etc.

go mod init github.com/yourname/yourmodule
go get some/package@latest
go mod tidy

23. Performance and Profiling
    -    Go provides pprof for CPU and memory profiling.
    -    Commonly used with net/http/pprof.

Example: CPU Profiling

```go
import (
    "log"
    "os"
    "runtime/pprof"
)
```

```go
func main() {
    f, err := os.Create("cpu.prof")
    if err != nil {
        log.Fatal(err)
    }
    pprof.StartCPUProfile(f)
    defer pprof.StopCPUProfile()
```

    // ... your code to profile ...
}

    -    After running, analyze with go tool pprof cpu.prof.

24. Additional References
    -    Official Go Documentation
    -    Effective Go
    -    Go by Example
    -    Golang Code
    -    Go Forum

File Export

This comprehensive Markdown has been updated with relevant links, more advanced examples, and additional topics.

Happy Coding in Go!
Feel free to adapt any of these examples to suit your specific application or project.

