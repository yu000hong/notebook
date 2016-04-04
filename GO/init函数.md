# init函数

GO语言中init函数用于包(package)的初始化，该函数是GO语言的一个重要特性，有如下特征：
- init函数是用于程序执行前做包的初始化的函数，比如初始化包里的变量等
- 每个包可以拥有多个init函数
- 包的每个源文件也可以拥有多个init函数
- 同一个包中多个init函数的执行顺序go语言没有明确的定义(说明)
- 不同包的init函数按照包导入的依赖关系决定该初始化函数的执行顺序
- init函数不能被其他函数调用，而是在main函数执行之前，自动被调用

下面这个示例摘自《the way to go》，OS差异在应用程序初始化时被隐藏掉了
```go
var prompt = "Enter a digit, e.g. 3 " + "or %s to quit."

func init() {
    if runtime.GOOS == "windows" {
        prompt = fmt.Sprintf(prompt, "Ctrl+Z, Enter")
    } else { // Unix-like
        prompt = fmt.Sprintf(prompt, "Ctrl+D")
    }
}
```

下面的两个go文件演示了：
- 一个package或者是go文件可以包含多个init函数
- init函数是在main函数之前执行的
- init函数被自动调用，不能在其他函数中调用，显式调用会报该函数未定义

> gprog.go

```go
package main

import (
    "fmt"
)

// the other init function in this go source file
func init() {
    fmt.Println("do in init")
}

func main() {
    fmt.Println("do in main")
}

func testf() {
    fmt.Println("do in testf")
    //if uncomment the next statment, then go build give error message : .\gprog.go:19: undefined: init
    //init()
}
```

> ginit1.go

```go
package main

import (
    "fmt"
)

// the first init function in this go source file
func init() {
    fmt.Println("do in init1")
}

// the second init function in this go source file
func init() {
    fmt.Println("do in init2")
}
```

编译上面两个文件：go build gprog.go ginit1.go

编译之后执行gprog.exe后的结果表明，gprog.go中的init函数先执行，然后执行了ginit1.go中的两个init函数，然后才执行main函数。

```bash
E:\opensource\go\prj\hellogo>gprog.exe
do in init
do in init1
do in init2
do in main
```

# 链接

[init函数](http://www.cnblogs.com/youyou/archive/2013/04/21/3034211.html)
