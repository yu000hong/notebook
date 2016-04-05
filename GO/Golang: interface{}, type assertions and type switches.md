# Golang: interface{}, type assertions and type switches

An `interface{}` type is a type that could be any value. It’s like Object in Java.

```go
var anything interface{} = "string"
var moreAnything interface{} = 123
```

This means you can create a function that can accept any type, custom or internal.

You can’t use a **`type conversion`** here since they’re not similar types. 

You must use a **`type assertion`**:

```go
aString := anything.(string)
```

> **We convert the ‘anything’ interface{} type using a dot and the required type in parentheses.**

If it fails, golang will panic and fail (unless you ‘recover’ from the panic). You can get around this 
by passing back two parameters:

```go
aString, found := anything.(string)
```

Now if the assertion fails, it won’t panic and crash, but will set found to false.

If you’re not sure about what the `interface{}` type could be, you can use a **`type switch`**:

```go
switch v := anything.(type) {
  case string:
    fmt.Println(v)
  case int32, int64:
    fmt.Println(v)
  case SomeCustomType:
    fmt.Println(v)
  default:
    fmt.Println("unknown")
}
```

Instead of putting the type in parentheses, you place the text ‘type’. The return value will be actual value of ‘anything’.

Now each case has possible types.

(Note that golang doesn’t need ‘break’ after each case. It does that automatically. You use **`fallthrough`** statement to fallthrough ot the next case.)

You normally would use polymorphism instead of this. But it can be useful, for example, in cases where you’re dealing with unknown external data.

# 链接

[Golang: interface{}, type assertions and type switches](https://blog.denevell.org/golang-interface-type-assertions-switch.html)
