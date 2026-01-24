# 🚀 Go (Golang) Comprehensive Tutorial

Go is a modern, compiled, strongly typed language created at Google. It is designed for:

* Simplicity
* High performance
* Concurrency
* Networking & cloud systems

It’s widely used for **backend services, DevOps tools, cloud platforms, distributed systems, and CLI apps.**

---

# 📦 1. Installing Go

Download from:
👉 [https://go.dev/dl/](https://go.dev/dl/)

Verify:

```bash
go version
```

---

# 📁 2. Your First Go Program

Create a file:

```bash
main.go
```

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

Run:

```bash
go run main.go
```

Build executable:

```bash
go build main.go
```

---

# 🧱 3. Program Structure

```go
package main     // every file belongs to a package

import "fmt"     // imports

func main() {    // entry point
    fmt.Println("Hello")
}
```

Go programs start from `main()` in package `main`.

---

# 🔤 4. Variables & Constants

```go
var age int = 30
var name = "Chris"

count := 10   // short declaration (inside functions only)

const PI = 3.14159
```

Multiple:

```go
var (
    x int = 10
    y int = 20
)
```

---

# 🔢 5. Basic Types

```go
bool
string
int, int8, int16, int32, int64
uint, uintptr
float32, float64
complex64, complex128
```

Example:

```go
var a int = 5
var b float64 = 3.14
var c bool = true
```

---

# ➗ 6. Operators

```go
+  -  *  /  %
== != < > <= >=
&& || !
```

---

# 🔁 7. Control Flow

### If

```go
if x > 10 {
    fmt.Println("Big")
} else {
    fmt.Println("Small")
}
```

With initialization:

```go
if n := 5; n > 3 {
    fmt.Println(n)
}
```

---

### For (only loop in Go)

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

While-style:

```go
for x < 10 {
    x++
}
```

Infinite:

```go
for {
    break
}
```

---

### Switch

```go
switch day {
case "Mon":
    fmt.Println("Start")
case "Fri":
    fmt.Println("End")
default:
    fmt.Println("Mid")
}
```

---

# 🔧 8. Functions

```go
func add(a int, b int) int {
    return a + b
}
```

Multiple returns:

```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}
```

---

# 📦 9. Packages & Modules

Initialize module:

```bash
go mod init example.com/myapp
```

Directory:

```
myapp/
  main.go
  mathutils/
     math.go
```

math.go:

```go
package mathutils

func Add(a, b int) int {
    return a + b
}
```

main.go:

```go
import "example.com/myapp/mathutils"

fmt.Println(mathutils.Add(2,3))
```

Public = Capitalized
Private = lowercase

---

# 📚 10. Arrays, Slices, Maps

### Arrays

```go
var a [3]int = [3]int{1,2,3}
```

### Slices (dynamic)

```go
nums := []int{1,2,3}
nums = append(nums, 4)
```

```go
part := nums[1:3]
```

### Maps

```go
ages := map[string]int{
    "Alice": 30,
    "Bob": 25,
}

ages["Chris"] = 40
delete(ages, "Bob")
```

Check existence:

```go
v, ok := ages["Alice"]
```

---

# 🧱 11. Structs

```go
type Person struct {
    Name string
    Age  int
}

p := Person{"Chris", 40}
p.Age++
```

Pointers:

```go
pp := &p
pp.Age = 41
```

---

# 🧩 12. Methods & Interfaces

### Methods

```go
func (p Person) Greet() string {
    return "Hello " + p.Name
}
```

---

### Interfaces

```go
type Speaker interface {
    Speak() string
}
```

```go
func (p Person) Speak() string {
    return "Hi"
}
```

```go
func talk(s Speaker) {
    fmt.Println(s.Speak())
}
```

Go uses **implicit interfaces** (no implements keyword).

---

# 🧵 13. Goroutines (Concurrency)

```go
go doWork()
```

```go
go func() {
    fmt.Println("running")
}()
```

They run concurrently.

---

# 📡 14. Channels

```go
ch := make(chan int)
```

Send:

```go
ch <- 5
```

Receive:

```go
x := <-ch
```

Example:

```go
func worker(ch chan int) {
    ch <- 42
}

func main() {
    ch := make(chan int)
    go worker(ch)
    fmt.Println(<-ch)
}
```

---

# 🧠 15. Buffered Channels & Select

```go
ch := make(chan int, 3)
```

Select:

```go
select {
case msg := <-ch:
    fmt.Println(msg)
case <-time.After(time.Second):
    fmt.Println("timeout")
}
```

---

# 🛑 16. Error Handling

```go
file, err := os.Open("test.txt")
if err != nil {
    log.Fatal(err)
}
```

Custom error:

```go
errors.New("something failed")
```

---

# 📂 17. File I/O

```go
data, err := os.ReadFile("a.txt")
```

```go
os.WriteFile("b.txt", []byte("hello"), 0644)
```

---

# 🌐 18. HTTP & Web Servers

### Simple server

```go
http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Hello Web")
})

http.ListenAndServe(":8080", nil)
```

### Client

```go
resp, _ := http.Get("https://example.com")
body, _ := io.ReadAll(resp.Body)
```

---

# 🗃 19. JSON

```go
type User struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}
```

Encode:

```go
json.NewEncoder(w).Encode(user)
```

Decode:

```go
json.Unmarshal(data, &user)
```

---

# 🧪 20. Testing

File: `math_test.go`

```go
func TestAdd(t *testing.T) {
    if Add(2,3) != 5 {
        t.Fail()
    }
}
```

Run:

```bash
go test ./...
```

Benchmark:

```go
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(1,2)
    }
}
```

---

# ⚙️ 21. Build, Run, Install

```bash
go run .
go build
go install
```

Cross compile:

```bash
GOOS=linux GOARCH=amd64 go build
```

---

# 🏗 22. Project Layout

```
cmd/app/main.go
internal/
pkg/
api/
```

Common pattern in production Go systems.

---

# 🚦 23. Context & Cancellation

```go
ctx, cancel := context.WithTimeout(context.Background(), time.Second)
defer cancel()
```

Used in servers, DB, APIs.

---

# 🧰 24. Reflection & Generics

### Generics (Go 1.18+)

```go
func Max[T comparable](a, b T) T {
    if a == b {
        return a
    }
    return a
}
```

---

# 🚀 25. Example Mini-Project (Worker Pool)

```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        results <- j * 2
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)

    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs)

    for a := 1; a <= 5; a++ {
        fmt.Println(<-results)
    }
}
```

---

# 📘 26. Key Go Principles

* Composition over inheritance
* Explicit error handling
* Fast compilation
* Built-in concurrency
* Simple tooling (`go fmt`, `go test`, `go mod`)

---

# 🎯 What to Learn Next

* REST APIs (Gin, Echo, Fiber)
* Databases (sqlx, gorm, pgx)
* gRPC
* Kubernetes operators
* Microservices
* CLI tools (cobra)
* WebAssembly with Go


