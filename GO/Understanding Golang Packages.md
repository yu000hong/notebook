# Understanding Golang Packages

In this post, we will take a look at packages in the Go programming language. 
When we develop software applications, writing maintainable and reusable pieces of code 
is very important. Go provides the modularity and code reusability through it’s package ecosystem. 
Go encourages you to write small pieces of software components through packages, and compose your 
applications with these small packages.

### Workspace

Let’s discuss how to organize code in Workspace before discussing Go packages. In Go, 
programs are kept in a directory hierarchy that is called a workspace. A workspace is 
simply a root directory of your Go applications. A workspace contains three subdirectories at its root:

- **src** - This directory contains source files organized as packages. You will write your Go applications inside this directory.
- **pkg** - This directory contains Go package objects.
- **bin** – This directory contains executable programs.

You have to specify the location of the workspace before developing Go programs. 
The **`GOPATH`** environment variable is used for specifying the location of Go workspaces.

### Packages

In Go, source files are organized into system directories called packages, which enable code reusability 
across the Go applications. The naming convention for Go package is to use the name of the system directory 
where we are putting our Go source files. Within a single folder, the package name will be same for the all 
source files which belong to that directory. We develop our Go programs in the **`$GOPATH`** directory, 
where we organize source code files into directories as packages. 

> In Go packages, all identifiers will be exported to other packages if the first letter of the identifier name 
starts with an uppercase letter. The functions and types will not be exported to other packages if we start 
with a lowercase letter for the identifier name.

Go’s standard library comes with lot of useful packages which can be used for building real-world applications. 
For example, the standard library provides a “net/http” package which can be used for building web applications 
and web services. The packages from the standard library are available at the “pkg” subdirectory of the **`GOROOT`** 
directory. When you install Go, an environment variable **`GOROOT`** will be automatically added to your system 
for specifying the Go installer directory. The Go developer community is very enthusiastic for developing 
third-party Go packages. When you develop Go applications, you can leverage these third-party Go packages.

### Package Main

When you build reusable pieces of code, you will develop a package as a shared library. But when you develop 
executable programs, you will use the package “main” for making the package as an executable program. 
The package “main” tells the Go compiler that the package should compile as an executable program instead of a shared library.
The main function in the package “main” will be the entry point of our executable program. 
When you build shared libraries, you will not have any main package and main function in the package.

> The main function in the package “main” will be the entry point of our executable program. 

Here’s a sample executable program that uses package main in which the function main is the entry point.

> Code Listing – 1

```go
package main

import (
"fmt"
)
func main(){
 fmt.Println("Hello, Gopher!")
}
```

### Import Packages

The keyword “import” is used for importing a package into other packages. In the Code Listing -1, 
we have imported the package “fmt” into the sample program for using the function Println. 
The package “fmt” comes from the Go standard library. When you import packages, the Go compiler 
will look on the locations specified by the environment variable `GOROOT` and `GOPATH`. 
Packages from the standard library are available in the `GOROOT` location. The packages that are 
created by yourself, and third-party packages which you have imported, are available in the `GOPATH` location.

### Install Third-Party Packages

We can download and install third-party Go packages by using “**`go get`**” command. The Go get command will 
fetch the packages from the source repository and put the packages on the `GOPATH` location.

The following command in the terminal will install “mgo”,  a third-party Go driver package for MongoDB, 
into your `GOPATH`, which can be used across the projects put on the GOPATH directory:

```bash
go get gopkg.in/mgo.v2
```

After installing the mgo, put the import statement in your programs for reusing the code, as shown below:

```go
import (        
        "gopkg.in/mgo.v2" 
        "gopkg.in/mgo.v2/bson"       
)
```

The MongoDB driver, mgo, provides two packages that we have imported in the above import statement.

### Init Function

When you write Go packages, you can provide a function “**`init`**” that will be called at the beginning 
of the execution time. The init method can be used for adding initialization logic into the package.

> Code Listing – 2

```go
package db
import (
       "gopkg.in/mgo.v2"
        "gopkg.in/mgo.v2/bson"
)
func init {
   // initialization code here    
}
```

In some contexts, we may need to import a package only for invoking it’s init method, where we don’t need 
to call forth other methods of the package. **If we imported a package and are not using the package identifier 
in the program, Go compiler will show an error**. In such a situation, we can use a blank identifier ( _ ) 
as the package alias name, so the compiler ignores the error of not using the package identifier, but will 
still invoke the init function.

> Code Listing – 3

```go
package main
import (
  _ "mywebapp/libs/mongodb/db"
	"fmt"
	"log"
)
func main() {
  //implementation here
}
```

In the above sample program, we imported a package named db. Let’s say we want to use this package 
only for calling the init method. The blank identifier will enable us to avoid the error from Go compiler, 
and we will be able to invoke the init method defined in the package.

We can use alias names for packages to avoid package name ambiguity.

> Code Listing – 4

```go
package main
import (
        mongo "mywebapp/libs/mongodb/db"
        mysql "mywebapp/libs/mysql/db"  	
)
func main() {
    mongodata := mongo.Get() //calling method of package  "mywebapp/libs/mongodb/db"
    sqldata := mysql.Get() //calling method of package "mywebapp/libs/mysql/db"  
    fmt.Println(mongodata )
    fmt.Println(sqldata )
}
```

We are importing two different packages from two different locations, but the name of both packages 
are the same. We can use an alias name for one package, and can use the alias name whenever we need 
to call a method of that package.

### Creating and Reusing Packages

Let’s create a sample program for demonstrating a package. Create a source file “languages.go” 
for the package “lib” at the location github.com/shijuvar/go-samples-thenewstack/packagedemo/lib” in the `GOPATH`. 
Since the “languages.go” belongs to the folder “lib”, the package name will be named “lib”.  All files 
created inside the lib folder belongs to lib package.

> Code Listing – 5

```go
package lib
var languages map[string]string
func init(){
	languages= make(map[string]string)
	languages["cs"] = "C#"
	languages["js"] = "JavaScript"
	languages["rb"] = "Ruby"
	languages["go"] = "Golang"
}
func Get(key string) (string){
	return languages[key]
}
func Add(key,value string){
	 languages[key]=value
}
func GetAll() (map[string]string){
	return languages
}
```

In the above program, we included an init method so that this method will be invoked at the beginning 
of execution. Let’s build our Go package for reusing with other packages. In the terminal window, go 
to the location lib folder, and run the following command:

```bash
go install
```

The go install command will build the package “lib” which will be available at the pkg subdirectory of `GOPATH`.

Let’s create a “main.go” for making an executable program where we want to reuse the code specified in the package “lib”.

> Code Listing – 6

```go
package main

import (
	"fmt"
	"github.com/shijuvar/go-samples-thenewstack/packagedemo/lib"
)

func main() {
	lib.Add("dr","Dart")
	fmt.Println(lib.Get("dr"))
	languages:=lib.GetAll()
	for _, v := range languages {
		fmt.Println(v)
	}
}
```

In the above program, we import the lib package and call the exported methods provided by the lib package.

### Source Code

The source code for the sample demo is available [here](https://github.com/shijuvar/go-samples-thenewstack/tree/master/packagedemo).

### Summary

The design philosophy of Go is to build reusable pieces of code as packages, and make your application using 
these packages. Go programs are organized into directories called packages. Go encourages you to perform software 
composition by composing individual pieces of Go packages as an application. Go packages enable code reusability, 
modularity and better maintainability.

# 链接

[Understanding Golang Packages](http://thenewstack.io/understanding-golang-packages/)
