# 1. Basics

Basic language syntax, such as assignment, control structures, function defintion etc. 

## Assignment, Variable bindings

```go
var x int // declare
x = 3 // assignment
y := 4 // := indicates type inference

a, b := func1() // multiple assignments

var x int // declare
x = 3 // assignment
y := 4 // := indicates type inference
```

```javascript
var a = 42;
let x = 123;
const y = 'abc';
```

| feature \ lang        | Go        | JS               |
| --------------------- | --------- | ---------------- |
| Declare Variable      | __var__   | __var__, __let__ |
| Declare Constant      | __const__ | __const__        |
| binding / assignment  | __=__     | __=__            |
| binding w/ type infer | __:=__    | NA               |


## Define functions

```go
// no param, no return
func foo() {
  fmt.Println("hello world")
}

// with param and return
func sum(a, b int) int {
   return a + b
}

// return multiple restuls
func sumAndProduct(a, b int) (int, int) {
  return a + b, a * b
}

```

```javascript
// no param, no return
function foo() {
  console.log("hello world");
}

// with param and return
function sum(a, b) {
  return a + b;
}

// return multiple results
function sumAndProduct(a, b) {
  // or return { sum: a + b, product: a * b }
  return [a + b, a * b];
}

// arrow function
const sum = (a, b) => a + b;
```

| feature \ lang                  | Go       | JS           |
| ------------------------------- | -------- | ------------ |
| denote function definition      | __func__ | __function__ |
| denote arrow function defintion | NA       | __=>__       |


## Built-in Data types

### string and char
```go
var s1 string = "Hello"
var s2 string = `string goes to
the next line`

var c1 rune = '我' //rune is alias for int32, which hold a unicode for the character

```

### integer types
Go's integer types are: 
`uint8` , `uint16` , `uint32` , `uint64` , `int8` , `int16` , `int32` and `int64`

```go
var u uint = 42
```
> `unit` is either 32 or 64 bit
> `int` isn't alias to int32. `int` could be 32bit or 64bit.
> `uintptr` is an unsigned integer large enough to store the uninterpreted bits
> `byte` is alias for `uint8`
> `rune` is alias for `int32`

### float types
Go's float types are: `float32` and `float64`

```go
var f float64 = 3.1415826525
var pi float32 = 22. / 7
```

### boolean types

```go
var f bool = false
var t bool = true
```

### Collections

#### Arrays
```go
var xs [5]int = [5]int{1, 2, 3, 4, 5}
xs := [5]int{1, 2, 3, 4, 5}

var twoD [2][3]int
for i := 0; i < 2; i++ {
    for j := 0; j < 3; j++ {
        twoD[i][j] = i + j
    }
}
fmt.Println("2d: ", twoD)
```

#### Slices
#### Maps
#### Ranges

## Custom data types
## Conditional 

### If/Else

```go
// simple
if 7 % 2 == 0 {
  fmt.Println("7 is even")
} else {
  fmt.Println("7 is odd")
}

// if with preceding statement
if num := 9; num < 0 {
    fmt.Println(num, "is negative")
} else if num < 10 {
    fmt.Println(num, "has 1 digit")
} else {
    fmt.Println(num, "has multiple digits")
}
```

### Switch
```go

// simple
i := 2
fmt.Print("Write ", i, " as ")
switch i {
case 1:
  fmt.Println("one")
case 2:
  fmt.Println("two")
case 3:
  fmt.Println("three")
}

// multiple match in one branch
switch time.Now().Weekday() {
case time.Saturday, time.Sunday: 
  fmt.Println("It's the weekend")
default: 
  fmt.Println("It's a weekday")
}

// switch without expression
t := time.Now()
switch {
  case t.Hour() < 12:
    fmt.Println("It's before noon")
  default: 
    fmt.Println("It's after noon")
}

// check value type
whatAmI := func(i interface{}) {
  switch t := i.(type) {
  case bool:
    fmt.Println("I'm a bool")
  case int:
    fmt.Println("I'm an int")
  default:
    fmt.Printf("Don't know type %T\n", t)
  }
}
whatAmI(true)
whatAmI(1)
whatAmI("hey")

```

## Loops
```go
// while loop equivalent
i := 1
for i <=3 {
  fmt.Println(i)
  i = i + 1
}

// while-true loop, looping until break
for {
  fmt.Prinlnt("loop")
  break
}

// classic for loop
for j := 0; j < 10; j++ {
  fmt.Println(j)
}
```

## Paradigms: OOP, FP etc
