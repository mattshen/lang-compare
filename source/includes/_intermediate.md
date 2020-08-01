# 2. Intermediate

## Pointers / References / Pass-by-X

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


## Class / Struct

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


## Objects
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

## Methods
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

## Interface
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

## Enumeration
```go
import (
	"errors"
	"fmt"
)

type Season string

const (
	SUMMER Season = "summer"
	WINTER        = "winter"
	SPRING        = "spring"
	AUTUMN        = "autumn"
)

//alternative
const (  // iota is reset to 0
  c0 = iota  // c0 == 0
  c1 = iota  // c1 == 1
  c2 = iota  // c2 == 2
)

func monthToSeason(month int) (Season, error) {
	var season Season
	switch month {
	case 9, 10, 11:
		season = SPRING
	case 12, 1, 2:
		season = SUMMER
	case 3, 4, 5:
		season = AUTUMN
	case 6, 7, 8:
		season = WINTER
	default:
		return "", errors.New("Invalid month")
	}
	return season, nil
}

func main() {
	month := 8
	season1, err := monthToSeason(month)
	if err != nil {
		panic(err)
	}
	fmt.Printf("Month %d belongs to %s\n", month, season1)
}
```

```javascript
const seasons = {
    SUMMER: 'summer',
    WINTER: 'winter',
    SPRING: 'spring',
    AUTUMN: 'autumn'
}

switch(season){
    case seasons.SUMMER:
    // Do something for summer
    case seasons.WINTER:
    //Do something for winter
    case seasons.SPRING:
    //Do something for spring
    case seasons.AUTUMN:
    //Do something for autumn
}
```

Neither go or javascript has built-in support to enumeration. However, we can use some tricks to simulate it. 

### References
- https://www.ribice.ba/golang-enums/

## Error handling
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
```go
// dealing with small files
func rw_small_file() {
	// read the whole file at once
	b, err := ioutil.ReadFile("input.txt")
	if err != nil {
		panic(err)
	}

	// write the whole body at once
	err = ioutil.WriteFile("output.txt", b, 0644)
	if err != nil {
		panic(err)
	}
}

// dealing with big file
func rw_big_file() {
	// open input file
	fi, err := os.Open("input.txt")
	if err != nil {
		panic(err)
	}
	// close fi on exit and check for its returned error
	defer func() {
		if err := fi.Close(); err != nil {
			panic(err)
		}
	}()

	// open output file
	fo, err := os.Create("output.txt")
	if err != nil {
		panic(err)
	}
	// close fo on exit and check for its returned error
	defer func() {
		if err := fo.Close(); err != nil {
			panic(err)
		}
	}()

	// make a buffer to keep chunks that are read
	buf := make([]byte, 1024)
	for {
		// read a chunk
		n, err := fi.Read(buf)
		if err != nil && err != io.EOF {
			panic(err)
		}
		if n == 0 {
			break
		}

		// write a chunk
		if _, err := fo.Write(buf[:n]); err != nil {
			panic(err)
		}
	}
}

```

```javascript
// read the entire file
const contents = require('fs').readFileSync('filename', 'utf8');
console.log(contents);

// write to file
require('fs').writeFileSync("./tmp.txt", "Hey there!"); 

// read big file
const stream = require("fs").createReadStream("./bigfile.txt");
stream.on("data", function(data) {
    var chunk = data.toString();
    console.log(chunk);
}); 

// write to big file
const stream = require("fs").createWriteStream("tmp.txt", { flags:'a' });
stream.write("Tutorial on Node.js")
stream.write("Introduction")
```

Golang has very traditional style of dealing with files, which resulting clear but verbose coding. 

Javascript(Nodejs)'s API is much more concise. Also NodeJS provides both sync and async
style APIs. 

## Http Server
```go
import (
	"fmt"
	"net/http"
)

func hello(w http.ResponseWriter, req *http.Request) {
	fmt.Fprintf(w, "hello\n")
}

func main() {
	http.HandleFunc("/hello", hello)
	http.ListenAndServe(":8090", nil)
}
```

```javascript
const http = require('http')

const hello = (request, response) => {
  console.log(request.url);
  response.end('hello\n');
}

const server = http.createServer(hello);

server.listen(8090, (err) => {
  if (err) {
    return console.error('something bad happened', err)
  }
});
```

Both golang and Nodejs provide easy-to-use API to create a basic http server. 

## Http Client 
```go
import (
	"bufio"
	"fmt"
	"net/http"
)

func main() {

	resp, err := http.Get("http://localhost:8090/hello")
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()

	fmt.Println("Response status:", resp.Status)

	scanner := bufio.NewScanner(resp.Body)
	for i := 0; scanner.Scan() && i < 5; i++ {
		fmt.Println(scanner.Text())
	}

	if err := scanner.Err(); err != nil {
		panic(err)
	}
}
```

```javascript
// using axios
const axios = require('axios');
try {
  const res = await axios({
    method: 'POST',
    url: 'https://localhost:3000',
    headers: {
      client_id: 'client123',
      Authorization: 'Bearer token123'
    },
    data: {
      field1: 123,
      field2: 'abc'
    }
  });
  console.log(res.data); // 2xx
} catch (err) {
  console.error(err); // non-2xx
}
```

Consider timeout cases in case of prolonged server response. 

## JSON deserialize/serialize
```go
import ("encoding/json" "fmt"	"log" "time")
// deserialize

type FruitBasket struct {
	Name    string
	Fruit   []string
}

func deserialize() {
	jsonData := []byte(`{
		"Name":"Standard",
		"Fruit":["Apple","Banana","Orange"]
	}`)
	var basket FruitBasket
	err := json.Unmarshal(jsonData, &basket)
	fmt.Printf("%+v\n", basket)
}

func serialize() {
	jsonData, err := json.Marshal(FruitBasket{
		Name:    "Standard",
		Fruit:   []string{"Apple", "Banana", "Orange"}
	})
	fmt.Println(string(jsonData))
}
```

```javascript
// deserialize
JSON.parse('{"key1": 123, "key2": "abc"}') // => { key1: 123, key2: 'abc' }

// serialize
JSON.stringify({ key1: 123, key2: 'abc' }) // => '{"key1":123,"key2":"abc"}'
```

Javascript, is almost the easiest language in dealing with JSON.

Golang Like other static typed language, requires defined types to match JSON message.

## DateTime
```go
import "time"
// get timestamp in milliseconds
time.Now().UnixNano() / 1000000 // => 1596239578956

// create date time object from timestamp
time.Unix(0, 1596239578956 * int64(time.Millisecond)) // => 2020-07-31 23:52:58.956 +0000 UTC
time.Unix(1596239578, 0) // => seconds, 2020-07-31 23:52:58 +0000 UTC

// read/write ISO format without timezone information
// need to manually assign timezone to match RFC3339 
dt, _ := time.Parse(time.RFC3339, "2012-07-29T11:05:45" + "+00:00")// => 2012-07-29 11:05:45 +0000 UTC

// read/write ISO format with timezone information
dt, _ := time.Parse(time.RFC3339, "2012-03-29T10:05:45+10:00") // => 2012-03-29 10:05:45 +1000 +1000
dt, _ := time.Parse(time.RFC3339, "2012-03-29T10:05:45+08:00") // => 2012-03-29 10:05:45 +0800 +0800

// print local DateTime String, with specified timezone
tz1, _ := time.LoadLocation("Australia/Sydney")
tz2, _ := time.LoadLocation("Australia/Perth")
time.Unix(1596239578, 0).In(tz1).Format(time.RFC3339) // => '2020-08-01T09:52:58+10:00'

// read/write customized format
time.Unix(1596239578, 0).In(tz2).Format("2006-01-02 15:04:05") // => 2020-08-01 07:52:58
t, _ := time.ParseInLocation("2006-01-02 15:04:05", "2020-08-01 07:52:58", tz2)
t // => 2020-08-01 07:52:58 +0800 AWST
t.In(tz1) // => 2020-08-01 09:52:58 +1000 AEST
```

```javascript
// get timestamp in milliseconds
new Date().getTime() // current timestamp: 1596147462418

// create date time object from timestamp
new Date(1596147462418) // => 2020-07-30T22:17:42.418Z

// read/write ISO format without timezone information
new Date('2012-07-29T11:05:45') // => 2012-07-29T01:05:45.000Z, printed UTC value, local timezone is used during parsing

// read/write ISO format with timezone information
new Date('2012-03-29T10:05:45+10:00') // => 2012-03-29T00:05:45.000Z, printed UTC value
new Date('2012-03-29T10:05:45+08:00') // => 2012-03-29T02:05:45.000Z

// print local DateTime String, with specified timezone
const moment = require('moment-timezone');
moment(new Date('2012-03-29T10:05:45+08:00')).format(); // => '2012-03-29T13:05:45+11:00'
moment(new Date('2012-03-29T10:05:45+08:00')).tz('UTC').format(); // => '2012-03-29T02:05:45Z'

moment(new Date('2012-03-29T10:05:45+08:00')).tz('Australia/Perth').format(); // => '2012-03-29T10:05:45+08:00'

// read/write customized format
moment.tz("2012-03-29 13:05:45", "YYYY-MM-DD hh:mm:ss", "Australia/Perth").toDate(); // => 2012-03-29T05:05:45.000Z

moment("2012-03-29 13:05:45+11:00").format('YYYY-MM-DD hh:mm:ss') // => '2012-03-29 01:05:45'

moment("2012-03-29 13:05:45+11:00").tz('Australia/Perth').format('YYYY-MM-DD hh:mm:ss') // => '2012-03-29 10:05:45'
```

Javascript `Date` provides very basic utilites for DateTime. Hence, for complex parsing and formatting, devs usually use `moment` and `moment-timezone`.

Golang has quite sufficient built-in utilities from package `time`.


## Dealing with RDBMS (Postgres)


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