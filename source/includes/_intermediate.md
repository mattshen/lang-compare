# 2. Intermediate

## pointers / references / pass-by-???

```go
func zeroval(ival int) {
	ival = 0
}

func zeroptr(iptr *int) {
	*iptr = 0
}

func main() {
	i := 1
	fmt.Println("initial:", i)

	zeroval(i)
	fmt.Println("zeroval:", i)
	zeroptr(&i)
	fmt.Println("zeroptr:", i)

	fmt.Println("pointer:", &i)

}
```

```javascript
function changeStuff(a, b, c)
{
  a = a * 10;
  b.item = "changed";
  c = {item: "changed"};
}

var num = 10;
var obj1 = {item: "unchanged"};
var obj2 = {item: "unchanged"};

changeStuff(num, obj1, obj2);

console.log(num); // => 10
console.log(obj1.item); // => changed
console.log(obj2.item); // => unchanged
```

From programmer's perspective, we can always think it's passing value for both golang and javascript. 

For __golang__, it's either passing a normal value (a copy of the orginal vlaue), or passing a reference value.
For a reference value, dereference (`*`) is required to access the value. 

For __javascript__, it's the same. However there are two differences as compared to golang:
- programmer doesn't have control to passing a normal value or a reference value.
- dereference happens implicitly (with `.`). 


## class/struct

```go
// define struct
type person struct {
  name string
  age  int
}

// instantiate
p1 := person{name: "Alice", age: 30}
p2 := &person{name: "Bob", age: 29}

// change prop's value
p1.age = 40
(*p2).age = 39 // or p2.age = 39 (where auto deferencing happens)

// print objects
fmt.Println(p1) // => {Alice 40}
fmt.Println(p2) // => &{Bob 39}
```
```javascript
// define pre-ES6
function Person(name, age) {
  this.name = name;
  this.age = age;
}
// since ES6
class PersonES6 {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

// instantiate
const p1 = new Person("Alice", 30);
const p2 = new PersonES6("Bob", 29);

// change prop's value
p1.age = 40;
p2.age = 39;

// print objects
console.log(p1); // => Person { name: 'Alice', age: 40 }
console.log(p2); // => PersonES6 { name: 'Bob', age: 39 }
```

| feature \ lang | Go                   | JS        |
|----------------|----------------------|-----------|
| keyword        | __struct__, __type__ | __class__ |


## object
```go
type person struct {
  name string
  age  int
}

p1 := person{name: "Alice", age: 30} // struct literal
var p2 *person = new(person)         // with new keyword, it returns a pointer
p2.name = "Bob"
p2.age = 29

// print objects
fmt.Println(p1) // => {Alice 30}
fmt.Println(p2) // => &{Bob 29}
```

```javascript
class PersonES6 {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const p1 = new PersonES6("Alice", 30);
const p2 = {name: "Bob", age: 29}; // object literal, similar to strut literal

console.log(p1); // PersonES6 { name: 'Alice', age: 30 }
console.log(p2); // => { name: 'Bob', age: 29 }
```

| feature \ lang  | Go      | JS      |
|-----------------|---------|---------|
| keyword         | __new__ | __new__ |
| support literal | yes     | yes     |

## methods
```go
type rect struct {
    width, height int
}
func (r *rect) area() int {
    return r.width * r.height
}
r := &rect{width: 2, height: 4}
var area int = r.area()
fmt.Println(area) // => 8
```
```javascript
const r1 = {
  width: 2,
  height: 4,
  area: function() {
    return this.width * this.height;
  }
}
console.log(r1.area());
// or 
class Rect {
    constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  area() {
    return this.width * this.height;
  }
}
console.log(new Rect(2, 4).area())
```

Javascript's way of attaching function to object/class is quite traditional. 

Golang's way is sort of "de-coupled". 

## interface
```go
type geometry interface {
	area() float64
	perim() float64
}

type rect struct {
	width, height float64
}

func (r rect) area() float64 {
	return r.width * r.height
}

func (r rect) perim() float64 {
	return 2*r.width + 2*r.height
}

func measure(g geometry) {
	fmt.Printf("Geometry %#v, area=%f, perimeter=%f \n", g, g.area(), g.perim())
}

r := rect{width: 3, height: 4}
measure(r) // => Geometry main.rect{width:3, height:4}, area=12.000000, perimeter=14.000000 
```
```javascript
// NA
```

In go, this is no `implements` keyword for implement an interface. 
If type TA defines all methods specified in interface IA, it is said that type TA satifies interface IA. 

## error handling
```go
func f1(arg int) (int, error) {
	if arg == 42 {
		return -1, errors.New("can't work with 42")
	}
	return arg + 3, nil
}

// custom error, implement Error() method
func (e *CustomError) Error() string {
	return fmt.Sprintf("%d - %s", e.arg, e.prob)
}
func f2(arg int) (int, error) {
	if arg == 42 {
		return -1, &CustomError{arg, "can't work with it"}
	}
	return arg + 3, nil
}
```

```javascript
try {
  throw new Error('Whoops!')
} catch (e) {
  console.error(e.name + ': ' + e.message)
}

// custom error
class CustomError extends Error {
  constructor(foo = 'bar', ...params) {
    // Pass remaining arguments (including vendor specific ones) to parent constructor
    super(...params)

    // Maintains proper stack trace for where our error was thrown (only available on V8)
    if (Error.captureStackTrace) {
      Error.captureStackTrace(this, CustomError)
    }

    this.name = 'CustomError'
    // Custom debugging information
    this.foo = foo
    this.date = new Date()
  }
}

```
## Basic Asynchronous
```go
// defining two expensive computations
func compute1() string {
  // expensive computing
  time.Sleep(time.Second)
  return "3.14"
}
func compute2() string {
  // expensive computing
  time.Sleep(time.Second * 2)
  return "42"
}

//fire and forget, non-blocking
go func(){ r := compute1(); fmt.Println(r) }()

// wait for one result
r := make(chan string, 1)
go func() { r <- compute1() }()
fmt.Println(<-r) // => '3.14'

// wait for two results
r1 := make(chan string, 1)
r2 := make(chan string, 1)
go func() { r1 <- compute1() }()
go func() { r2 <- compute2() }()
fmt.Println(<-r1) // => 3.14
fmt.Println(<-r2) // => 42

// wait for first resolved
c1 := make(chan string)
c2 := make(chan string)
go func() { c1 <- compute1() }()
go func() { c2 <- compute2() }()
select {
case msg1 := <-c1:
    fmt.Println("received", msg1) 
case msg2 := <-c2:
    fmt.Println("received", msg2)
}
// => "received 3.14"
```

```javascript
async function fetchRemote1() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('3.14...');
    }, 1000);
  });
}
async function fetchRemote2() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('42');
    }, 2000);
  });
}
// fire and forget, non-blocking
fetchRemote1().then(console.log).catch(console.err)

// wait for one result
const pi = await fetchRemote1(); // => '42'

// wait for multiple results
const [pi, life] = await Promise.all([fetchRemote1(), fetchRemote2()]) // => ['3.14...', '42']

// wait for first resolved
const winner = await Promise.race([fetchRemote1(), fetchRemote2()]) // => '3.14...'
```

Golang goroutines are very lightweight, as well as JS Promises.

Golang is a bit verbose as compared to JS. 


## IO, file read/write
## JSON deserialize/serialize
## Http Client / Http Server

## Packages or Modules

In __golang__, a publishable unit is called module, consisting a list of packages. A package can be one file or many files. This is similar to Java. 

In __javascript__, a publishable is called package, consisting a list of modules. A module is one JS file.

### Links
- https://blog.golang.org/using-go-modules

### Useful commands
* golang
  * `go mod init` creates a new module, initializing the go.mod file that describes it.
  * `go build`, `go test`, and other package-building commands add new dependencies to go.mod as needed.
  * `go list -m all` prints the current module’s dependencies.
  * `go get` changes the required version of a dependency (or adds a new dependency).
  * `go mod tidy` removes unused dependencies.
* javascript
  * `npm init`
  * `npm install`
  * `npm add package-abc --save`
  * `npm add package-abc --save-dev`
  * `npm update [-g] [package-abc]` // update package to lastest
  * `npm prune --production=false`

## Dependency Management
## Project Structure