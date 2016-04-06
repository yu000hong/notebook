# Set Data Structure in Golang

### Question

I really like google golang but could some one explain what the rationale is for the implementors having left out 
a basic data structure such as sets from the standard library?

### Answer

One potential reason for this omission is that it's really easy to model sets with a **`map`**.

To be honest I think it's a bit of an oversight too, however looking at Perl, the story's exactly the same. 
In Perl you get lists and hashtables, in Go you get `arrays`, `slices`, and `maps`. In Perl you'd generally use a 
hashtable for any and all problems relating to a set, the same is applicable to Go.

#### Example

to imitate a set ints in Go, we define a map:

```go
set := make(map[int]bool)
```

To add something is as easy as:

```go
i := valueToAdd()
set[i] = true
```

Deleting something is just

```go
delete(set, i)
```

And the potential awkwardness of this construct is easily abstracted away:

```go
type IntSet struct {
    set map[int]bool
}

func (set *IntSet) Add(i int) bool {
    _, found := set.set[i]
    set.set[i] = true
    return !found   //False if it existed already
}
```

And delete and get can be defined similarly, I have the complete implementation here . The major disatvantage 
here is the fact that **`go doesn't have generics`**. However it is possible to do this with **`interface{}`** 
in which case you'd have cast the results of get.

# 链接

[Set Data Structure in Golang](http://programmers.stackexchange.com/questions/177428/sets-data-structure-in-golang)
