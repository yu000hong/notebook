# Understanding Golang Type System

In the previous post “[A Closer Look at Golang From an Architect’s Perspective](http://thenewstack.io/a-closer-look-at-golang-from-an-architects-perspective/)”, 
we offered a high level look at the Go programming language. In this post, we’ll take a look at the type system of Go, 
with a primary focus on user-defined types.

The type system is the most important feature of a programming language, letting you organize your application data. 
Go follows a minimalistic approach for its type system. It provides several built-in types such as `string`, `bool`, `int` and 
`float64`.

Go supports composite types such as `array`, `slice`, `map` and `channel`. Composite types are made up of other types – 
built-in types and user-defined types. For example, the composite type “`map[string]string`” represents a collection of 
string values, in which each string value persists with a string key, where values can be retrieved with the corresponding key.

### User Defined Types with Structs

In Go, **`structs`** are the way to create user-defined concrete types. The design of the `struct` type is unique compared 
to other programming languages.

> In simple terms, a `struct` is a collection of fields or properties. 

Unlike other object-oriented languages, Go does not provide a “class” keyword. Instead, in Go you’ll find structs — 
a lightweight version of classes. A struct type is exported into other packages if the name of the struct starts with a 
capital letter. Unlike other object-oriented languages, you don’t need to provide getters and setters on struct fields, 
as they can access from the same package and can access from other packages as well, **if the name of the fields start with a 
capital letter**.

Let’s create a struct type.

> Code Listing – 1

```go
type Person struct {
	name string
	age int
	city,phone string
}
```

In the above code block, we created a struct with the name Person. Now, we can create objects of Person type and we can 
store our application data.

### Create Instances of Struct Types

Let’s create an instance of a Person type and assign the values to the fields.

> Code Listing – 2

```go
var p Person
p.name="shiju"
p.age=35
p.city="Kochi"
p.phone="+91-94003372xx"
```

In the above code block, we create a Person object and assign the values to the fields, one by one. We can also use 
a struct literal to create instances of struct types.

> Code Listing – 3

```go
p:= Person {
     name : "shiju",
     age : 35,
     city : "Kochi",
     phone : "+91-94003372xx",		
}
```

We created an instance of a Person type by using a struct literal. When we create instances of struct types using 
struct literals, we can split the code into multiple lines, but **we need to put an `extra comma` at the end of the last assignment**.

**`We can create the instances in a shorter way if we clearly know the order of the struct fields.`**

> Code Listing – 4

```go
p:= Person {
     "shiju",
     35,
     "Kochi",
     "+91-94003372xx",		
}
```

We can create the instance with a single line statement.

> Code Listing – 5

```go
p:= Person {"shiju",35,"Kochi","+91-94003372xx"}
```

### Defining Behavior with Interface Type

Interface types provide contracts to concrete types, which lets you define behaviors for your objects. Let’s create 
an interface type for specifying the behavior for Person objects.

> Code Listing – 6

```go
type People interface {
	SayHello()
	GetDetails()
}
```

We defined an interface type with the name People, in which we specified two behaviors – SayHello and GetDetails.

### Adding Behavior to Concrete Type

In Code Listing – 6, we defined an interface for implementing into Person objects. Let’s implement these behaviors 
to our concrete types.

In this sample, we are going to implement the behaviors defined in the interface type People into a Person type with methods. 
In most object-oriented languages, methods are associated with class, `but in Go, methods associate with a struct type.`

Let’s add a couple of behaviors to our Person type. In Go, explicit declaration of interface implementation is not required. 
You just need to implement the methods defined in the interface into your struct type where you want to implement an 
interface type.

> Code Listing – 7

```go
type Person struct {
	name string
	age int
	city,phone string
}
//A person method
func (p Person ) SayHello() {
	fmt.Printf("Hi, I am %s, from %s\n", p.name, p.city)
}
//A person method
func (p Person) GetDetails() {
	fmt.Printf("[Name: %s, Age: %d, City: %s, Phone: %s]\n", p.name,p.age, p.city, p.phone)
}
```

**A method in Go is simply a function that is declared with a receiver.** 

**If you want to modify the data of a receiver from the method, the receiver must be a pointer as shown below:**

> Code Listing – 8

```go
//A person method
func (p *Person ) SayHello() {
//Implementations here 
}
//A person method
func (p *Person) GetDetails() {
//Implementations here 
}
```

### Embedding Type with Composition

Go’s type system does not support inheritance. In Go, composition is preferred over inheritance where type embedding 
is the way to implement composition. Many pragmatic developers are proponents of using composition over inheritance.

In this sample program, we are going to create more specialized types of the Person type for representing the meetup 
people such as Organizer, Speaker and Attendee, by embedding the Person type into new types. Let’s create the types 
for representing Organizer, Speaker and Attendee by embedding the Person type.

> Code Listing – 9

```go
type Speaker struct {
	Person //type embedding for composition
	speaksOn []string
	pastEvents []string
}

type Organizer struct {
	Person //type embedding for composition
	meetups []string
}

type Attendee struct {
  Person //type embedding for composition
  interests []string
}
```

In the above code block, we create three new types — Organizer, Speaker and Attendee — by embedding the Person type. 
**When we embed a type into another type, the methods of the embedded type will avail to the new type.** So the methods 
SayHello and GetDetails will be available for the newly created types Organizer, Speaker and Attendee. And our new types 
also implement the People interface, since the methods SayHello and GetDetails are available on new types. **We can also 
override the methods of Person in the new types.**

### Running a Sample Program with Struct and Interface

Let’s create types for representing a conference meetup.

> Code Listing – 10

```go
type Meetup struct{
	location string
	city string
	date time.Time
	people []People
}
func (m Meetup) MeetupPeople(){
	for _, v := range m.people {
		v.SayHello()
		v.GetDetails()
	}
}
```

Here, we create a type named Meetup in which a slice of the interface type Person is included as a struct field. 
Since we are implementing the interface People into Organizer, Speaker and Attendee types, we can assign instances of 
these types into the People struct field. We added a method named MeetupPeople to the Meetup struct, in which we 
iterate through slice of interface type People and call methods SayHello and GetDetails.

In the final program, we **`override`** the GetDetails method for the Speaker and Organizer types since they are 
representing some extra information than an Attendee. For the Attendee object, the GetDetails method of Person will be used.

Here is the final program:

> Code listing – 11 (meetup.go)

```go
package main
import (
	"fmt"
	"time"
)
type People interface {
	SayHello()
	GetDetails()
}
type Person struct {
	name string
	age int
	city,phone string
}
//A person method
func (p Person ) SayHello() {
	fmt.Printf("Hi, I am %s, from %s\n", p.name, p.city)
}
//A person method
func (p Person) GetDetails() {
	fmt.Printf("[Name: %s, Age: %d, City: %s, Phone: %s]\n", p.name,p.age, p.city, p.phone)
}
type Speaker struct {
	Person //type embedding for composition
	speaksOn []string
	pastEvents []string
}
//overrides GetDetails
func (s Speaker) GetDetails() {
	//Call person GetDetails
	s.Person.GetDetails()
	fmt.Println("Speaker talks on following technologies:")
	for _, value := range s.speaksOn {
		fmt.Println(value)
	}
	fmt.Println("Presented on the following conferences:")
	for _, value := range s.pastEvents {
		fmt.Println(value)
	}
}
type Organizer struct {
	Person //type embedding for composition
	meetups []string
}
//overrides GetDetails
func (o Organizer) GetDetails() {
	//Call person GetDetails
	o.Person.GetDetails()
	fmt.Println("Organizer, conducting following Meetups:")
	for _, value := range o.meetups {
		fmt.Println(value)
	}
}
type Attendee struct {
  Person //type embedding for composition
  interests []string
}
type Meetup struct{
	location string
	city string
	date time.Time
	people []People
}
func (m Meetup) MeetupPeople(){
	for _, v := range m.people {
		v.SayHello()
		v.GetDetails()
	}
}
func main() {
	shiju := Speaker{Person{"Shiju", 35,"Kochi" ,"+91-94003372xx"},
		[]string{"Go","Docker", "Azure","AWS"},
		[]string{"FOSS","JSFOO","MS TechDays"}}
	satish := Organizer{Person{"Satish", 35,"Pune" ,"+91-94003372xx"},
		[]string{"Gophercon","RubyConf"}}
	alex := Attendee{Person{"Alex", 22,"Bangalore" ,"+91-94003672xx"},
		[]string{"Go","Ruby"}}
  meetup:=Meetup {
		"Royal Orchid",
		"Bangalore",
		time.Date(2015, time.February, 19, 9, 0, 0, 0, time.UTC),
		[]People{shiju,satish,alex},
	}
	//get details of meetup people
	meetup.MeetupPeople()
}
```

In the main function, which is the starting point of our sample program, we create an object for Speaker, Organizer, 
and Attendee types, then create a slice object with three objects, and assign the slice object into the People field 
of Meetup type, which is a slice of the People interface. If you look deeply into the source, meetup.go, you will 
understand the power of Go’s interface types.

You can run the program meetup.go by entering the following in the terminal:

```bash
go run meetup.go
```

The source of the meetup.go is available from [here](https://github.com/shijuvar/go-samples-thenewstack/blob/master/typesystem/meetup.go).

### Summary

Go provides a light-weight type system that enables code reusability through type embedding and promotes composition 
over inheritance. **Types in Go are exported into other packages if the name of the type starts with an upper case.** 
Structs are used to create user-defined concrete types in Go. In Go, methods are associated with a struct type. 
A method in Go is simply a function that is declared with a receiver and methods can be called through the receiver types. 
Interface is an awesome feature of Go which comes with a lot of power and extensibility. When you implement interface 
types into concrete types, you don’t need to explicitly declare the interface type along with your struct definition. 
Instead, it will implicitly implement the interface types into your types at run time.

# 链接

[Understanding Golang Type System](http://thenewstack.io/understanding-golang-type-system/)

