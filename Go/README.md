# Go Cheatsheet

## Table of Contents
- [Go Cheatsheet](#go-cheatsheet)
  - [Table of Contents](#table-of-contents)
  - [Formatting Verbs](#formatting-verbs)
    - [General Formatting Verbs](#general-formatting-verbs)
    - [Integer Formatting Verbs](#integer-formatting-verbs)
    - [String Formatting Verbs](#string-formatting-verbs)
    - [Boolean Formatting Verbs](#boolean-formatting-verbs)
    - [Float Formatting Verbs](#float-formatting-verbs)
  - [Arrays](#arrays)
    - [Syntax](#syntax)
    - [Array Initialization](#array-initialization)
  - [Slices](#slices)
    - [Create a Slice](#create-a-slice)
    - [Append](#append)
    - [Copy](#copy)
  - [Loop](#loop)
  - [Functions](#functions)
    - [Variadic](#variadic)
    - [Named Return Values](#named-return-values)
    - [Multiple Return Values](#multiple-return-values)
    - [Defer functions](#defer-functions)
    - [Pass Functions as Value](#pass-functions-as-value)
  - [Structure](#structure)
    - [Struct Embedding](#struct-embedding)
  - [Method](#method)
    - [Struct Type Receiver](#struct-type-receiver)
    - [Non-Struct Type Receiver](#non-struct-type-receiver)
    - [Pointer Receiver](#pointer-receiver)
  - [Interface](#interface)
    - [Type assertions](#type-assertions)
    - [Down Casting](#down-casting)
  - [Map](#map)
  - [String](#string)
    - [Access Bytes](#access-bytes)
    - [Create String From Slice](#create-string-from-slice)
    - [String Functions](#string-functions)
  - [Concurrency](#concurrency)
    - [Channel](#channel)
    - [Mutex](#mutex)
    - [WaitGroup](#waitgroup)
    - [Atomic](#atomic)
  - [time Package](#time-package)
  - [Sort Package](#sort-package)
    - [Built-in functions](#built-in-functions)
    - [Custom Sort](#custom-sort)
  - [Error](#error)
  - [File](#file)
    - [Read](#read)
  - [Generics](#generics)
  - [CLI](#cli)
    - [Arguments and Flags](#arguments-and-flags)
    - [Subcommands](#subcommands)
  - [HTTP](#http)
    - [Parse URL](#parse-url)
    - [Client](#client)
    - [Server](#server)
  - [Hash](#hash)
  - [Handwrite Notes](#handwrite-notes)
    - [Difference Between `var` and `:=`](#difference-between-var-and-)
    - [`rune` datatype](#rune-datatype)
    - [Stringer Interface](#stringer-interface)

## Formatting Verbs

### General Formatting Verbs

| Verb  | Description                                   |
| ----- | --------------------------------------------- |
| `%v`  | is used to print the *value* of the arguments |
| `%T`  | is used to print the *type* of the arguments  |
| `%#v` | Prints the value in Go-syntax format          |
| `%+v` | Prints the struct with field names            |

```go
func main() {
  var i = 15.5
  var txt = "Hello World!"

  fmt.Printf("%v\n", i)
  fmt.Printf("%#v\n", i)
  fmt.Printf("%v%%\n", i)
  fmt.Printf("%T\n", i)

  fmt.Printf("%v\n", txt)
  fmt.Printf("%#v\n", txt)
  fmt.Printf("%T\n", txt)
}
```
Outputs:
```
15.5
15.5
15.5%
float64
Hello World!
"Hello World!"
string
```

### Integer Formatting Verbs

| Verb   | Description                                |
| ------ | ------------------------------------------ |
| `%b`   | Base 2                                     |
| `%d`   | Base 10                                    |
| `%+d`  | Base 10 and always show sign               |
| `%o`   | Base 8                                     |
| `%O`   | Base 8, with leading 0o                    |
| `%x`   | Base 16, lowercase                         |
| `%X`   | Base 16, uppercase                         |
| `%#x`  | Base 16, with leading 0x                   |
| `%4d`  | Pad with spaces (width 4, right justified) |
| `%-4d` | Pad with spaces (width 4, left justified)  |
| `%04d` | Pad with zeroes (width 4)                  |

```go
func main() {
  var i = 15
 
  fmt.Printf("%b\n", i)
  fmt.Printf("%d\n", i)
  fmt.Printf("%+d\n", i)
  fmt.Printf("%o\n", i)
  fmt.Printf("%O\n", i)
  fmt.Printf("%x\n", i)
  fmt.Printf("%X\n", i)
  fmt.Printf("%#x\n", i)
  fmt.Printf("%4d\n", i)
  fmt.Printf("%-4d\n", i)
  fmt.Printf("%04d\n", i)
}
```
```
1111
15
+15
17
0o17
f
F
0xf
  15
15
0015
```

### String Formatting Verbs

| Verb   | Description                                                 |
| ------ | ----------------------------------------------------------- |
| `%s`   | Prints the value as plain string                            |
| `%q`   | Prints the value as a double-quoted string                  |
| `%8s`  | Prints the value as plain string (width 8, right justified) |
| `%-8s` | Prints the value as plain string (width 8, left justified)  |
| `%x`   | Prints the value as hex dump of byte values                 |
| `% x`  | Prints the value as hex dump with spaces                    |

```go
func main() {
  var txt = "Hello"
 
  fmt.Printf("%s\n", txt)
  fmt.Printf("%q\n", txt)
  fmt.Printf("%8s\n", txt)
  fmt.Printf("%-8s\n", txt)
  fmt.Printf("%x\n", txt)
  fmt.Printf("% x\n", txt)
}
```
```
Hello
"Hello"
   Hello
Hello
48656c6c6f
48 65 6c 6c 6f
```

### Boolean Formatting Verbs

| Verb | Description                                                                |
| ---- | -------------------------------------------------------------------------- |
| `%t` | Value of the boolean operator in true or false format (same as using `%v`) |

### Float Formatting Verbs

| Verb    | Description                               |
| ------- | ----------------------------------------- |
| `%e`    | Scientific notation with 'e' as exponent  |
| `%f`    | Decimal point, no exponent                |
| `%.2f`  | Default width, precision 2                |
| `%6.2f` | Width 6, precision 2                      |
| `%g`    | Exponent as needed, only necessary digits |

```go
func main() {
  var i = 3.141

  fmt.Printf("%e\n", i)
  fmt.Printf("%f\n", i)
  fmt.Printf("%.2f\n", i)
  fmt.Printf("%6.2f\n", i)
  fmt.Printf("%g\n", i)
}
```
```
3.141000e+00
3.141000
3.14
  3.14
3.141
```

## Arrays

### Syntax
```go
var array_name = [length]datatype{values} // here length is defined
var array_name = [...]datatype{values} // here length is inferred
array_name := [length]datatype{values} // here length is defined
```

### Array Initialization
```go
arr1 := [5]int{} //not initialized
arr2 := [5]int{1,2} //partially initialized
arr3 := [5]int{1,2,3,4,5} //fully initialized
arr4 := [5]int{1:10,2:40} // Only Specific Elements
```
```
[0 0 0 0 0]
[1 2 0 0 0]
[1 2 3 4 5]
[0 10 40 0 0]
```

## Slices

`len()` function - returns the length of the slice (the number of elements in the slice)

`cap()` function - returns the capacity of the slice (the number of elements the slice can grow or shrink to)

### Create a Slice
```go
slice_name := []datatype{values}
var myarray = [length]datatype{values} // An array
myslice := myarray[start:end] // A slice made from the array
slice_name := make([]type, length, capacity)
```

### Append
```go
slice_name = append(slice_name, element1, element2, ...)
slice3 = append(slice1, slice2...)
```

### Copy
Takes in two slices dest and src:
```go
copy(dest, src)
```

## Loop

```go
for index, value := array|slice|map {
   // code to be executed for each iteration
}

for {
  // forever loop
}
```

## Functions

### Variadic
```go
// Variadic function to join strings
func joinstr(elements ...string) string {
    return strings.Join(elements, "-")
}
```

### Named Return Values
```go
func myFunction(x int, y int) (result int) {
  result = x + y
  return
}
```

### Multiple Return Values
```go
func myFunction(x int, y string) (result int, txt1 string) {
  result = x + x
  txt1 = y + " World!"
  return
}
```

### Defer functions
multiple defer statements executed in LIFO order. Defer statements are generally used to ensure that the files are closed when their need is over, or to close the channel, or to catch the panics in the program.
```go
defer func (parameter_list) (return_type) {
// code
} (arguments)

// Main function
func main() {
 
    // Calling mul() function
    // Here mul function behaves
    // like a normal function
    mul(23, 45)
 
    // Calling mul()function
    // Using defer keyword
    // Here the mul() function
    // is defer function
    defer mul(23, 56)
}
```

### Pass Functions as Value

```go
func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}
```
```go
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	fmt.Println(pos(i), neg(-2*i))
}
```

## Structure

```go
type struct_name struct {
  member1 datatype;
  member2 datatype;
  member3 datatype;
  ...
}

var a = struct_name{"Akshay", "PremNagar", "Dehradun", "Uttarakhand", 252636}
```

### Struct Embedding
```go
type base struct {
  num int
}

type container struct {
  base
  str string
}
```

## Method

| Method                                                                      | Function                                                                                    |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| It contains a receiver.                                                     | It does not contain a receiver.                                                             |
| Methods of the same name but different types can be defined in the program. | Functions of the same name but different type are not allowed to be defined in the program. |

### Struct Type Receiver

```go
// Author structure
type author struct {
    name      string
    branch    string
    particles int
    salary    int
}
 
// Method with a receiver
// of author type
func (a author) show() {
 
    fmt.Println("Author's Name: ", a.name)
    fmt.Println("Branch Name: ", a.branch)
    fmt.Println("Published articles: ", a.particles)
    fmt.Println("Salary: ", a.salary)
}
// Main function
func main() {
    res := author{
        name:      "Sona",
        branch:    "CSE",
        particles: 203,
        salary:    34000,
    }
    // Calling the method
    res.show()
}
```

### Non-Struct Type Receiver
```go
// Type definition
type data int
 
// Defining a method with
// non-struct type receiver
func (d1 data) multiply(d2 data) data {
    return d1 * d2
}

func main() {
    value1 := data(23)
    value2 := data(20)
    res := value1.multiply(value2)
    fmt.Println("Final result: ", res)
}
```

### Pointer Receiver
```go
// Author structure
type author struct {
    name      string
    branch    string
    particles int
}
 
// Method with a receiver of author type
func (a *author) show(abranch string) {
    (*a).branch = abranch
}
// Main function
func main() {
    // Initializing the values
    // of the author structure
    res := author{
        name:   "Sona",
        branch: "CSE",
    }
 
    fmt.Println("Author's name: ", res.name)
    fmt.Println("Branch Name(Before): ", res.branch)
    // Creating a pointer
    p := &res
    // Calling the show method
    p.show("ECE")
    fmt.Println("Author's name: ", res.name)
    fmt.Println("Branch Name(After): ", res.branch)
}
```

## Interface

```go
type I interface {
	M()
}

type T struct {
	S string
}

// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"hello"}
	i.M()
}
```

**Interface values with nil underlying values:** If the concrete value inside the interface itself is `nil`, the method will be called with a nil **receiver**.

**The empty interface:** An empty interface may hold values of any type. (**Every type** implements at least zero methods.)

```go
func main() {
	var i interface{}
	describe(i)
	i = 42
	describe(i)
	i = "hello"
	describe(i)
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

### Type assertions
```go
t, ok := i.(T)
```
If `i` holds a `T`, then `t` will be the underlying value and ok will be true.

If not, `ok` will be false and `t` will be the zero value of type `T`, and no panic occurs.

**Type Switches**

```go
switch v := i.(type) {
case T:
    // here v has type T
case S:
    // here v has type S
default:
    // no match; here v has the same type as i
}
```

### Down Casting
```go
type Human interface {
	walk() string
}

type Person struct {
	name string
	age  int
}

func main() {
	var h Human = Person{"Moein", 20}
	fmt.Println(h.(Person).age) // Down casting
}
```

## Map

```go
var a = map[KeyType]ValueType{key1:value1, key2:value2,...}
var a = make(map[KeyType]ValueType)
delete(map_name, key)
val, ok := map_name[key]  // ok is true if key is present in the map, else false
```

## String
In Go language, strings are different from other languages like Java, C++, Python, etc. it is a sequence of **variable-width** characters where each and every character is represented by one or more bytes using **UTF-8** Encoding. In Go language, strings are **immutable**.

### Access Bytes
The string is of a byte so, we can access each byte of the given string.
```go
func main() {
    str := "Welcome to GeeksforGeeks"
    for c := 0; c < len(str); c++ {
        fmt.Printf("\nCharacter = %c Bytes = %v", str, str)
    }
}
```
Output:
```
Character = W Bytes = 87
Character = e Bytes = 101
Character = l Bytes = 108
...
```

### Create String From Slice
```go
myslice1 := []byte{0x47, 0x65, 0x65, 0x6b, 0x73}
mystring1 := string(myslice1)
```

### String Functions
```go
p("Contains:  ", s.Contains("test", "es"))
p("Count:     ", s.Count("test", "t"))
p("HasPrefix: ", s.HasPrefix("test", "te"))
p("HasSuffix: ", s.HasSuffix("test", "st"))
p("Index:     ", s.Index("test", "e"))
p("Join:      ", s.Join([]string{"a", "b"}, "-"))
p("Repeat:    ", s.Repeat("a", 5))
p("Replace:   ", s.Replace("foo", "o", "0", -1))
p("Replace:   ", s.Replace("foo", "o", "0", 1))
p("Split:     ", s.Split("a-b-c-d-e", "-"))
p("ToLower:   ", s.ToLower("TEST"))
p("ToUpper:   ", s.ToUpper("test"))
```
```
Contains:   true
Count:      2
HasPrefix:  true
HasSuffix:  true
Index:      1
Join:       a-b
Repeat:     aaaaa
Replace:    f00
Replace:    f0o
Split:      [a b c d e]
ToLower:    test
ToUpper:    TEST
```

## Concurrency

### Channel

```go
go f(x, y, z) // starts a new goroutine
ch := make(chan int, 100) // channel of integers, buffer size 100
v, ok := <-ch // receives from channel ch
close(ch)     // closes channel ch
```

Only the **sender** should **close** a channel, never the receiver. Sending on a closed channel will cause a panic.

The loop `for i := range c` receives values from the channel repeatedly **until** it is closed.

```go
select {
  case c <- x:
    x, y = y, x+y
  case <-quit:
    fmt.Println("quit")
    return
  default:
    fmt.Println("default")
  }
```

A select blocks until **one** of its cases can run, then it executes that case. It chooses one at **random** if multiple are ready.

### Mutex

Note that mutexes must not be **copied**

```go
type Container struct {
    mu       sync.Mutex
    counters map[string]int
}

func (c *Container) inc(name string) {
    c.mu.Lock()
    defer c.mu.Unlock() //using a defer statement.
    c.counters[name]++
}
```

### WaitGroup
```go
var wg sync.WaitGroup

	for i := 1; i <= 5; i++ {
		wg.Add(1)
		i := i
		go func() {
			defer wg.Done()
			worker(i)
		}()
	}
	wg.Wait()
```

### Atomic
```go
import "sync/atomic"

func main() {
  var ops uint64
  atomic.AddUint64(&ops, 1)
}
```

Reading atomics safely while they are being updated is also possible, using functions like `atomic.LoadUint64`.

## time Package

```go
tick := time.Tick(100 * time.Millisecond)
boom := time.After(500 * time.Millisecond)
```
```go
timer := time.NewTimer(2 * time.Second)
<-timer.C
//One reason a timer may be useful is that you can cancel the timer before it fires.
stop := timer.Stop()
  if stop {
    fmt.Println("Timer stopped")
  }
```

The tick **channel** will receive a value **every** 100ms. The boom channel will receive a single value **after** 500ms.

## Sort Package

### Built-in functions

`sort` package implements sorting for built-ins and user-defined types.
```go
sort.Strings(strs)
sort.Ints(ints)
sort.IntsAreSorted(ints)
```

### Custom Sort
```go
sort.Slice(p, func(i, j int) bool {
		return p[i].age < p[j].age
	})
```

## Error

Create a new error:

```go
error := errors.New("Invalid Name")
error := fmt.Errorf("%d is negative\nAge can't be negative", age)
```

For **custom** errors, implement `Error()` function.

Wrap and Unwrap errors:

```go
wrap = fmt.Errorf("...%w...",criticalError,...)
errors.Unwrap(wrap)
```

## File

### Read

```go
content, err := os.ReadFile("test.txt") // returns []byte

// Read line by line
f, err := os.Open("test.txt")
scanner := bufio.NewScanner(f)
for scanner.Scan() {
  s := scanner.Text()
  fmt.Println(s)
}
```

## Generics

```go
func MapKeys[K comparable, V any](m map[K]V) []K {
    r := make([]K, 0, len(m))
    for k := range m {
        r = append(r, k)
    }
    return r
}
```
`K` has the `comparable` constraint, meaning that we can compare values of this type with the `==` and `!=` operators. `V` has the `any` constraint, meaning that it’s not restricted in any way (any is an alias for `interface{}`).

## CLI

### Arguments and Flags

```go
args := os.Args // All arguments
value := flag.String("action", "read", "a string that represent the action")
flag.Parse()  // This should be called after all flags are defined and before flags are accessed by the program.
flag.Args() // All arguments after flags (tail arguments)
fmt.Println(*value)
```

### Subcommands

Create a custom subcommand:
```go
fooCmd := flag.NewFlagSet("foo", flag.ExitOnError)
fooEnable := fooCmd.Bool("enable", false, "enable")
fooCmd.Parse(os.Args[2:])
fooCmd.Args()
```

## HTTP

### Parse URL
```go
u, err := url.Parse(r.URL.String())
email, ok := u.Query()["email"]
```

### Client
```go
resp, _ := http.Get("https://google.com")
defer resp.Body.Close()
```

### Server
```go
func root(w http.ResponseWriter, _ *http.Request) {
	m := map[string]any{
		"name":   "Moein",
		"age":    20,
		"isNoob": true,
	}
	enc := json.NewEncoder(w)
	enc.Encode(m)
}

func main() {
	http.HandleFunc("/", root)
	http.ListenAndServe("localhost:80", nil)
}
```

## Hash
```go
s := "sha256 this string"
h := sha256.New()
h.Write([]byte(s))
bs := h.Sum(nil)
```

## Handwrite Notes

### Difference Between `var` and `:=`

| `var`                                                                | `:=`                                                                                                |
| -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Can be used **inside** and **outside** of functions                  | Can only be used **inside** functions                                                               |
| Variable declaration and value assignment can be done **separately** | Variable declaration and value assignment cannot be done separately (must be done in the same line) |

### `rune` datatype

rune in Go is a data type that stores codes that represent **Unicode** characters. This method results in an encoded output of variable length from **1-4 Bytes**. For this reason, rune is also known as an alias for `int32`, as each rune can store an integer value of at most 32-bits.

Furthermore, Go does **not** have a `char` data type, so all variables initialized with a character would automatically be **typecasted** into `int32`, which, in this case, represents a `rune`.

Here, it might seem like rune is the char alternative for Go, but that is not actually true. In Go, strings are actually made up of **sequences of bytes**, not a sequence of rune.

```go
package main

import "fmt"

func main() {

    var str string = "ABCD"
    r_array := []rune(str)
    fmt.Printf("Array of rune values for A, B, C and D: %U\n", r_array)
}
```

### Stringer Interface

A Stringer is a type that can describe itself as a string. The fmt package (and many others) look for this interface to print values.

```go
type Stringer interface {
    String() string
}
```