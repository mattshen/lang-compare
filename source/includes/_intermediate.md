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


## interface

## classes

## modules / packages
testing modules

## IO, file read/write
## JSON deserialize/serialize
## Http Client / Http Server
## Dependency Management
## Project Structure