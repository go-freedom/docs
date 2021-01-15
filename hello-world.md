# Example: Hello world

In this article, you'll build a simple web application using [Freedom](https://github.com/8treenet/freedom) which named `helloworld`. For more examples, see [Examples](https://github.com/go-freedom/examples).

## What you'll need

- [Go](https://golang.org/)
- Internet connection
- Code editor (VSCode, GoLand, etc.)

## Initialize

1. Create an folder for this project

2. Open this project in IDE

3. Setup Go mod for this project

  > Before Go 1.14, you may need to enable Go mod feature manually. See this article [Go Modules in 2019](https://blog.golang.org/modules2019)

  Open terminal and switch directory to this project (In this article, the project's directory is `/home/whoami/examples/simple/helloworld`)

  ```bash
  cd /home/whoami/examples/simple/helloworld
  ```

  Initialize go mod (In this article, the project's name is `helloworld`, you could specify another name)

  ```bash
  go mod init helloworld
  ```

4. Write `main.go`

  Copy following code and save as `main.go` (In this article, `main.go` located at `~/cmd/main` directory)

  ```go
  package main

  import "fmt"

  func main() {
  	fmt.Println("Hello, world!")
  }
  ```

  Run it

  ```bash
  go run cmd/main/main.go
  ```

  You'll get the output like following
  
  ```plain
  Hello, world!
  ```

  If you've got error or other output, please check your environment and Go configuration.

  Now, let's step by step to write our first web application using [Freedom](https://github.com/8treenet/freedom).

  Download [Freedom](https://github.com/8treenet/freedom) dependencies using `go get`

  ```bash
  go get github.com/8treenet/freedom
  ```

  You'll get the output like following

  ```plain
  go: downloading github.com/8treenet/freedom v1.8.7
  go: github.com/8treenet/freedom upgrade => v1.8.7
  ```

  Add import to [Freedom](https://github.com/8treenet/freedom)

  ```go
  package main

  import (
    "fmt"
    "github.com/8treenet/freedom" // add this import to your main.go
  )
  ```

  Replace `main` function with following code snippet

  ```go
  func main() {
    // Create a instance of freedom.Application. freedom.Application is a 
    // manager to freedom application, you could think it is a origin of any 
    // freedom application
    app := freedom.NewApplication()
    
    // So far, you only created a manager to freedom application. If you need
    // to serve HTTP request, you also need to create a HTTP runner to your
    // freedom application. In this example, we've created a HTTP runner and 
    // listening on 0.0.0.0:31234
  	runner := app.NewRunner(":31234")

    // After HTTP runner created, you need to add request processors to 
    // process incoming HTTP request. Freedom uses iris as http server, you
    // can through app.Iris() to get iris.Application instance and add routes 
    // by it. In this example, we've added a route to path "/" that accepts 
    // GET method, and send a response with plain-text "Hello, world!". 
  	app.Iris().Get("/", func(ctx freedom.Context) {
  		ctx.WriteString("Hello, world!")
  	})

    // So far, all staff works have been done. We have only one step to get 
    // the program run. We only pass the HTTP runner and default iris
    // configuration that comes freedom.DefaultConfiguration() to app.Run()
  	app.Run(runner, freedom.DefaultConfiguration())
  }
  ```

  Run it

  ```bash
  go run cmd/main/main.go
  ```

  You'll get the output like following

  ```plain
  [DBUG] 2021-01-15 15:05:20.981 [./cmd\main\main.go:12] GET: / -> main.main.func1()
  [DBUG] 2021-01-15 15:05:21.005 Application: running using 1 host(s)
  [DBUG] 2021-01-15 15:05:21.006 Host: addr is :31234
  [DBUG] 2021-01-15 15:05:21.007 Host: virtual host is localhost:31234
  [DBUG] 2021-01-15 15:05:21.007 Host: register startup notifier
  [DBUG] 2021-01-15 15:05:21.009 Host: register server shutdown on interrupt(CTRL+C/CMD+C)
  Now listening on: http://localhost:31234
  Application started. Press CTRL+C to shut down.
  ```

  Open your browser and visit http://localhost:31234, you'll see the content like following

  ```plain
  Hello, world!
  ```

  Back to your terminal, stop the program by `Ctrl`+`C`

  So far, your first freedom application has completed! Worry!!!
  