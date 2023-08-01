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
    - [Create a Slice With `[]datatype{values}`](#create-a-slice-with-datatypevalues)
    - [Create a Slice From an Array](#create-a-slice-from-an-array)
    - [Create a Slice With The `make()` Function](#create-a-slice-with-the-make-function)
    - [Append](#append)
    - [Copy](#copy)
    - [Sort](#sort)
  - [The Range Keyword](#the-range-keyword)
  - [Functions](#functions)
    - [Variadic](#variadic)
    - [Named Return Values](#named-return-values)
    - [Multiple Return Values](#multiple-return-values)
    - [Defer functions](#defer-functions)
  - [Structure](#structure)
    - [Create](#create)
  - [Method](#method)
    - [Struct Type Receiver](#struct-type-receiver)
    - [Non-Struct Type Receiver](#non-struct-type-receiver)
    - [Pointer Receiver](#pointer-receiver)
  - [Map](#map)
    - [Create](#create-1)
    - [Delete Element](#delete-element)
    - [Key Exists](#key-exists)
  - [String](#string)
    - [Access Bytes](#access-bytes)
    - [Create String From Slice](#create-string-from-slice)
  - [Handwrite Notes](#handwrite-notes)
    - [Difference Between `var` and `:=`](#difference-between-var-and-)

## Formatting Verbs

### General Formatting Verbs

| Verb  | Description                                   |
| ----- | --------------------------------------------- |
| `%v`  | is used to print the *value* of the arguments |
| `%T`  | is used to print the *type* of the arguments  |
| `%#v` | Prints the value in Go-syntax format          |

```go
package main
import ("fmt")

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
package main
import ("fmt")

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

| Verb    | Description                                                 |
| ------- | ----------------------------------------------------------- |
| `%s`    | Prints the value as plain string                            |
| `%q`    | Prints the value as a double-quoted string                  |
| `%8s`   | Prints the value as plain string (width 8, right justified) |
| ` %-8s` | Prints the value as plain string (width 8, left justified)  |
| `%x`    | Prints the value as hex dump of byte values                 |
| `% x`   | Prints the value as hex dump with spaces                    |

```go
package main
import ("fmt")

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

| Verb | Description                                                              |
| ---- | ------------------------------------------------------------------------ |
| `%t` | Value of the boolean operator in true or false format (same as using %v) |

### Float Formatting Verbs

| Verb    | Description                               |
| ------- | ----------------------------------------- |
| `%e`    | Scientific notation with 'e' as exponent  |
| `%f`    | Decimal point, no exponent                |
| `%.2f`  | Default width, precision 2                |
| `%6.2f` | Width 6, precision 2                      |
| `%g`    | Exponent as needed, only necessary digits |

```go
package main
import ("fmt")

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

- `len()` function - returns the length of the slice (the number of elements in the slice)
- `cap()` function - returns the capacity of the slice (the number of elements the slice can grow or shrink to)

### Create a Slice With `[]datatype{values}`
```go
slice_name := []datatype{values}
```

### Create a Slice From an Array
```go
var myarray = [length]datatype{values} // An array
myslice := myarray[start:end] // A slice made from the array
```
```go
arr1 := [6]int{10, 11, 12, 13, 14, 15}
myslice := arr1[2:4]
```
```
myslice = [12 13]
length = 2
capacity = 4
```

### Create a Slice With The `make()` Function
```go
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

### Sort

**Functions:**
1. `Ints`
2. `IntsAreSorted`

## The Range Keyword
```go
for index, value := array|slice|map {
   // code to be executed for each iteration
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

## Structure

### Create
```go
type struct_name struct {
  member1 datatype;
  member2 datatype;
  member3 datatype;
  ...
}

var a = struct_name{"Akshay", "PremNagar", "Dehradun", "Uttarakhand", 252636}
```

## Method

|Method|Function|
|------|----------|
|It contains a receiver.|It does not contain a receiver.|
|Methods of the same name but different types can be defined in the program.|Functions of the same name but different type are not allowed to be defined in the program.|

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

## Map

### Create

Note: The `make()` function is the **right way** to create an empty map. If you make an empty map in a different way and write to it, it will causes a runtime **panic**.
```go
var a = map[KeyType]ValueType{key1:value1, key2:value2,...}
var a = make(map[KeyType]ValueType)
```

### Delete Element
```go
delete(map_name, key)
```

### Key Exists
```go
val, ok := map_name[key]
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

## Handwrite Notes

### Difference Between `var` and `:=`

| `var`                                                                | `:=`                                                                                                |
| -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Can be used **inside** and **outside** of functions                  | Can only be used **inside** functions                                                               |
| Variable declaration and value assignment can be done **separately** | Variable declaration and value assignment cannot be done separately (must be done in the same line) |