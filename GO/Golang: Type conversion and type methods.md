# Golang: Type conversion and type methods

You can convert one basic type to another similar type with possible loss of accuracy using:

```go
f := 234.234
i := int64(f) // We're losing the decimals here
```

This will also work for strings and []byte, etc. They have to be similar types. Converting a string to an int will 
produce a compilation error.

To convert dissimilar types (i.e. interface{} and anything else) we use type assertions. We’ll talk about them in 
the next post.

You can convert to and from custom types of the same type.

```go
type customslice []string
...
realslice := []string(customslice{"one", "two"})    // convert from custom type to a basic type
cs customslice = customslice([]string{"one", "two"}) // convert from a basic type to a custom type
```

The customslice(…) was redundant, however, since golang will automatically recognise they’re the same type.

For example: if you have a function that takes in either a ‘customslice’ or a []string then you can pass either 
a customslice or []string to either.

```go
Thing(customslice{"one", "two"}) // Works
Thing([]string{"one", "two"})    // Works

func Thing(s customslice) {
  fmt.Println(s)
}

OtherThing(customslice{"one", "two"}) // Works
OtherThing([]string{"one", "two"})    // Works

func OtherThing1(s []string) {
  fmt.Println(s)
}
```

You often want to convert to function to a type to gain its methods.

```go
hf := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
  //
})
```

Your anonymous function is now of the type http.HandlerFunc. This type has the 
ServeHTTP method ([http://golang.org/pkg/net/http/#HandlerFunc](http://golang.org/pkg/net/http/#HandlerFunc)).

This means your anonymous function can be used anywhere a http.Handler – 
[http://golang.org/pkg/net/http/#Handler](http://golang.org/pkg/net/http/#Handler) – is needed. This is 
because both the HandlerFunc and Handler define the same methods.

> **You couldn’t directly convert to a http.Handler, since http.Handler is of the type `interface` - not `function`.** 

More on interfaces later.

# 链接

[Golang: Type conversion and type methods](https://blog.denevell.org/golang-type-conversion-custom-types.html)
