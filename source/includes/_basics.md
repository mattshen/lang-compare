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

// string interpolation
import ("fmt")
date := fmt.Sprintf("%d-%d-%d", year, month, day)

var c1 rune = '我' //rune is alias for int32, which hold a unicode for the character

```

```javascript
const s1 = "hello";
const s2 = 'world';
const s3 =`hello
world`;

// string interpolation
const s4 = `hello ${s2}`;
```

| feature \ lang       | Go                 | JS                 |
| -------------------- | ------------------ | ------------------ |
| with quote           | Yes                | Yes                |
| with single-quote    | Yes                | Yes                |
| multiline string     | Yes, with backtick | Yes, with backtick |
| string interpolation | No, via `fmt`      | Yes                |

### numbers
Go's integer types are: 

Go's float types are: `float32` and `float64`

```go
var u uint = 42

var f float64 = 3.1415826525
var pi float32 = 22. / 7

// > `unit` is either 32 or 64 bit.
// > `int` isn't alias to int32. `int` could be 32bit or 64bit.
// > `uintptr` is an unsigned integer large enough to store the uninterpreted bits.
// > `byte` is alias for `uint8`.
// > `rune` is alias for `int32`.
```

```javascript
const i = 123;
const f = 123.123;
```

| feature \ lang | Go                                                                            | JS                        |
| -------------- | ----------------------------------------------------------------------------- | ------------------------- |
| integer types  | `uint8`, `uint16`, `uint32`, `uint64` <br/> `int8`, `int16`, `int32`, `int64` | "integer" via Number type |
| float types    | `float32`, `float64`                                                          | float via Number type     |

### boolean types

```go
var f bool = false
var t bool = true
```

```javascript
const f = false;
const t = true;
```

| feature \ lang | Go     | JS                                                         |
| -------------- | ------ | ---------------------------------------------------------- |
| Truthy         | `true` | `true`, others values not listed below                     |
| Falsy          | `false` | `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN` |

## Collections/Arrays
```go
var xs [5]int = [5]int{1, 2, 3, 4, 5}
xs := [5]int{1, 2, 3, 4, 5} // or [...]int{1, 2, 3, 4, 5}

var twoD [2][3]int
for i := 0; i < 2; i++ {
    for j := 0; j < 3; j++ {
        twoD[i][j] = i + j
    }
}
fmt.Println("2d: ", twoD)

// copy/clone array
ys := xs
// reference original array
zs := &xs
```

```javascript
var cars = ["apple", "organce", "pear"];
var cars = new Array("Saab", "Volvo", "BMW");
for (let i = 0; i < 3; i ++ ) {
  console.log(cars[i]);
}

// copy/clone array
const newCars = cars.slice();
const newCars1 = [...cars];

// reference original array
const carsRef = cars;
```

| feature \ lang          | Go                         | JS                                  |
| ----------------------- | -------------------------- | ----------------------------------- |
| Define & Initialization | `var xs [5]int = [5]int{}` | `var xs = []`                       |
| Subscript               | `xs[i]`                    | `xs[i]`                             |
| Size of Array           | `len(xs)`                  | `xs.length`                         |
| Copy Array              | `ys := xs`                 | `ys = xs.slice()` or `ys = [...xs]` |


## Collections/List(or List-like structure)
```go
// slice literal
letters := []string{"a", "b", "c", "d"}

// using function make
// func make([]T, len, cap) []T
s := make([]byte, 5, 5)

// create from any array
x := [3]string{"hello", "world"}
s := x[:] // a slice referencing to the storage of x

// sub-slice
b := []byte{'g', 'o', 'l', 'a', 'n', 'g'}
// b[1:4] == []byte{'o', 'l', 'a'}, sharing the same storage as b

// append
s := []string{"a", "b", "c"}
s = append(s, "d")
s = append(s, "e", "f")

// copy
c := make([]string, len(s))
copy(c, s)

// remove
r := []string{"a", "x", "b", "c"} 
r1 := append(r[:1], r[2:]...) 
// r  => [a b c c] // side effects
// r1 => [a b c]


```

```javascript
const xs = ['a', 'b', 'c'];
// sub array/list
xs.slice(0, 2); // => ['a', 'b']
// append
xs.push('d'); // => ['a', 'b', 'c', 'd'];
// pop
xs.pop(); // => 'd', xs becomes ['a', 'b', 'c']
// copy
xs.slice(); // => ['a', 'b', 'c']
// remove 'b'
xs.splice(1, 1); // 'b', xs == ['a', 'c']
```

In Golang, Slice is usually used instead of List, due to it's first-class suppport in syntax and performance advantage. 

| feature \ lang          | Go                             | JS                           |
| ----------------------- | ------------------------------ | ---------------------------- |
| Define & Initialization | `int{}` or `make([]int, 5, 5)` | `const xs = ['a', 'b', 'c']` |
| append                  | `append(xs, "e", "f")`         | `xs.push('e')`               |
| Pop                     | `xs[:len(xs) -1]`              | `xs.pop()`                   |
| sub-list                | `xs[a:b]`                      | `xs.slice(a,b)`              |
| copy                    | `copy(target, source)`         | `xs.slice()`                 |
| remove                  | via `append`                   | via `splice`                 |


## Collections/Maps

```go
// create empty map
m := make(map[string]int)

// create with initial values
m: = map[string]int{"foo": 1, "bar": 2}

// put value
m["k1"] = 49
m["k2"] = 81

// get value
m["k1"] //=> 49

// delete k-v
delelte(m, "k2")

// iterate
for k, v := range m {
  // do something with k and v
}

```

```javascript
// create empty map
let m = new Map();

// create with initial values
let m = new Map([["foo", 1], ["bar", 2]]);

// put value
m.set("k1", 1);

// get value
m.get("k1");

// delete k-v
m.delete("k1");

// iterate
for (let [k, v] of m.entries()) {
  // do something with k and v
}

```

| feature \ lang | Go                                   | JS                                  |
| -------------- | ------------------------------------ | ----------------------------------- |
| Define         | `make(map[string]int)`               | `new Map()`                         |
| Initialization | `map[string]int{"foo": 1, "bar": 2}` | `new Map([["foo", 1], ["bar", 2]])` |


## Collections/Iterator(or Range)
```go
// iterate over an array
nums := []int{2, 3, 4}
sum := 0
for _, num := range nums {
    sum += num
}

// iterate over an map
kvs := map[string]string{"a": "apple", "b": "banana"}
for k, v := range kvs {
    fmt.Printf("%s -> %s\n", k, v)
}
```

```javascript
// iterate over an array
const array1 = ['a', 'b', 'c'];
for (const element of array1) {
  console.log(element);
}

// iterate over an Map
const myMap = new Map();
myMap.set("0", "foo");
myMap.set("1", "bar");
myMap.set("2", "baz");

for (const [key, value] of myMap.entries()) {
  console.log(`${key} -> ${value}`);
}

// iterate over an object - method 1
for (const [key, value] of Object.entries(obj)) {
  console.log(`${key} -> ${value}`);
}

// iterate over an object - method 2
for (const k in obj) {
  console.log(`${k} -> ${obj[k]}`);
}

```

| feature \ lang | Go                                   | JS                                 |
| -------------- | ------------------------------------ | ---------------------------------- |
| Syntax         | `for i, value := range iterable {} ` | `for (const value of iterable) {}` |


## Conditional/If-Else

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

```javascript
let x = 7;
if (x % 2 === 0 ) {
  console.log(`${x} is even`);
} else {
  console.log(`${x} is odd`);
}

```

| feature \ lang | Go           | JS           |
| -------------- | ------------ | ------------ |
| Keywords       | `if`, `else` | `if`, `else` |


## Conditional/Switch
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

```javascript
const expr = 'Papayas';
switch (expr) {
  case 'Oranges':
    console.log('Oranges are $0.59 a pound.');
    break;
  case 'Mangoes':
  case 'Papayas':
    console.log('Mangoes and papayas are $2.79 a pound.');
    // expected output: "Mangoes and papayas are $2.79 a pound."
    break;
  default:
    console.log(`Sorry, we are out of ${expr}.`);
}

```

| feature \ lang | Go                          | JS                                   |
| -------------- | --------------------------- | ------------------------------------ |
| Keywords       | `switch`, `case`, `default` | `switch`, `case`, `default`, `break` |


## Loops/for
```go
// classic for loop
for j := 0; j < 10; j++ {
  fmt.Println(j)
}
```
```javascript
let str = '';
for (let i = 0; i < 9; i++) {
  str = str + i;
}
console.log(str);
```

| feature \ lang | Go    | JS    |
| -------------- | ----- | ----- |
| Keywords       | `for` | `for` |


## Loops/while
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
```
```javascript
let n = 0;

while (n < 3) {
  n++;
}
```

| feature \ lang | Go    | JS      |
| -------------- | ----- | ------- |
| Keywords       | `for` | `while` |

