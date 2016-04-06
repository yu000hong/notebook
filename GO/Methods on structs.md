# Methods on structs

We have learnt that structs can contain data. **structs can also contain behavior in the form of methods.** 
The definition of a method attached to or associated with a struct or any other type for that matter, is quite similar 
to a normal func definition, the only difference being that you need to additionally specify the type. 

A normal function that we name my_func that takes no parameters and returns an int would be defined similar to that 
shown below.

```go
func my_func() int {
   //code
}
```

A function or method that we name my_func that takes no parameters and returns an int, but which is associated with a 
type we name my_type would be defined similar to that shown below.

```go
type my_type struct { }

func (m my_type) my_func() int {
   //code
}
```
Let’s extend our earlier Rectangle struct to add an Area function. This time we will define that the Area function 
works explicitly with the Rectangle type with func (r Rectangle) Area() int.

```go
package main

import "fmt"

type Rectangle struct {
    length, width int
}

func (r Rectangle) Area() int {
    return r.length * r.width
}

func main() {
    r1 := Rectangle{4, 3}
    fmt.Println("Rectangle is: ", r1)
    fmt.Println("Rectangle area is: ", r1.Area())
}
```

> output

```bash
Rectangle is: {4 3}
Rectangle area is: 12
```

Many object oriented languages have a concept of `this` or `self` that implicitly refers to the current instance. 
Go has no such keyword. When defining a function or method associated with a type, it is given as a named variable - 
in this case (r Rectangle) and then within the function the variable r is used.

In the above call to Area, the instance of Rectangle is **`passed as a value`**. You could also **`pass it by reference`**. 
In calling the function, there would be no difference whether the instance that you call it with is a pointer or a value 
because **Go will automatically do the conversion for you**. 

```go
package main

import "fmt"

type Rectangle struct {
    length, width int
}

func (r Rectangle) Area_by_value() int {
    return r.length * r.width
}

func (r *Rectangle) Area_by_reference() int {
    return r.length * r.width
}

func main() {
    r1 := Rectangle{4, 3}
    fmt.Println("Rectangle is: ", r1)
    fmt.Println("Rectangle area is: ", r1.Area_by_value())
    fmt.Println("Rectangle area is: ", r1.Area_by_reference())
    fmt.Println("Rectangle area is: ", (&r1).Area_by_value())
    fmt.Println("Rectangle area is: ", (&r1).Area_by_reference())
}
```

> output

```bash
Rectangle is: {4 3}
Rectangle area is: 12
Rectangle area is: 12
Rectangle area is: 12
Rectangle area is: 12
```

In the above code, we have defined two similar functions, one which takes the Rectangle instance as a pointer and 
one that takes it by value. We have called each of the functions, via a value r1 and once as an address &r1. 
The results however are all the same since Go performs appropriate conversions. 

Just to extend the example, let’s do one more function that works on the same type. In the below example, we ‘attach’ 
a function to calculate the perimeter of the Rectangle type. 

```go
package main

import "fmt"

type Rectangle struct {
    length, width int
}

func (r Rectangle) Area() int {
    return r.length * r.width
}

func (r Rectangle) Perimeter() int {
    return 2* (r.length + r.width)
}

func main() {
    r1 := Rectangle{4, 3}
    fmt.Println("Rectangle is: ", r1)
    fmt.Println("Rectangle area is: ", r1.Area())
    fmt.Println("Rectangle perimeter is: ", r1.Perimeter())
}
```

> output

```bash
Rectangle is: {4 3}
Rectangle area is: 12
Rectangle perimeter is: 14
```

You might be tempted now to see if you can attach methods and behavior to any type, say like an int or time.Time - 
`not possible`. 

> **You will be able to add methods for a type only if the type is defined in the same package.**

```go
func (t time.Time) first5Chars() string {
    return time.LocalTime().String()[0:5]
}
```

You will get error: `cannot define new methods on non-local type time.Time`

However, if you absolutely need to extend the functionality, you can easily use what we learnt about **`anonymous fields`** 
and extend the functionality. 

```go
package main

import "fmt"
import "time"

type myTime struct {
    time.Time //anonymous field
}

func (t myTime) first5Chars() string {
    return t.Time.String()[0:5]
}

func main() {
    m := myTime{*time.LocalTime()} //since time.LocalTime returns an address, we convert it to a value with *
    fmt.Println("Full time now:", m.String()) //calling existing String method on anonymous Time field
    fmt.Println("First 5 chars:", m.first5Chars()) //calling myTime.first5Chars
}
```

> output

```bash
Full time now: Tue Nov 10 23:00:00 UTC 2009
First 5 chars: Tue N
```

### Methods on anonymous fields

There was another item that we slipped into the previous program - method calls on anonymous fields. 
Since time.Time was an anonymous field within myTime, we were able to refer to a method of Time as if it were 
a method of myTime. i.e. we were able to do myTime.String(). Let’s do one program using an earlier example.

In the following code we go back to our house where we have a Kitchen as an anonymous field in House. 
As we learnt with member fields, we can also access methods of anonymous fields as if they belong directly to 
the composing type. So House has an anonymous Kitchen which in turn has a method totalForksAndKnives(); 
so now House.totalForksAndKnives() is a valid call.

```go
package main

import "fmt"

type Kitchen struct {
    numOfForks int 
    numOfKnives int
}

func(k Kitchen) totalForksAndKnives() int {
    return k.numOfForks + k.numOfKnives
}

type House struct {
    Kitchen //anonymous field
}

func main() {
    h := House{Kitchen{4, 4}} //the kitchen has 4 forks and 4 knives
    fmt.Println("Sum of forks and knives in house: ", h.totalForksAndKnives())  //called on House even though the method is associated with Kitchen
}
```

> output

```bash
Sum of forks and knives in house: 8
```

# 链接

[Methods on structs](http://golangtutorials.blogspot.jp/2011/06/methods-on-structs.html)
