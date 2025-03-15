# Lily's Go (Golang) Development Guide

A structured approach to organizing Go projects, inspired by "Lily's Program Writing Sets" to ensure clarity, correctness, and maintainability. This guide focuses on developing applications using Go, a statically typed, compiled language known for its simplicity and efficiency.

---

## Introduction

This guide provides a systematic way to structure Go projects, whether you're building a small CLI tool or a large-scale web application. Go's simplicity and performance make it an excellent choice for a wide range of applications, but without proper organization, projects can become difficult to manage. This guide will help you structure your Go projects for scalability, readability, and maintainability.

## Guiding Principles

Following the core principles of "Lily's Program Writing Sets":

1. **Clarity** – Code should be easy to read, understand, and modify. Go's simplicity and strong typing help achieve this, but proper organization is still key.
2. **Correctness** – Code should function as intended and handle errors gracefully. Go's explicit error handling and testing tools help ensure robustness.
3. **Maintainability** – Code should be structured for long-term usability and modification. Modular design and separation of concerns are key.

---

## Go Project Structure

### **1. Basic Project (Simple CLI Tool)**

For simple Go applications, such as a CLI tool or a small utility.

### **📂 Project Structure**

```markdown
/simple_go_project
│── main.go            # Main entry point
│── go.mod             # Go module file
│── README.md          # Project documentation
```

#### **Example Code:**

**main.go**

```go
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Hello, World!")
}
```

**go.mod**

```go
module simple_go_project

go 1.20
```

**README.md**

```markdown
# Simple Go Project

This is a simple Go project that prints "Hello, World!" to the console.

## Installation

To run this project, clone the repository and run:

    ```bash
    go run main.go
    ```

## Usage

This project is a simple CLI tool that prints a greeting message.
```

#### **Explanation:**

- **`main.go`**: The main entry point of the application. It contains the `main` function, which is the starting point of the program.
- **`go.mod`**: The Go module file that defines the module path and Go version.
- **`README.md`**: Provides a brief description of the project, installation instructions, and usage.

This structure is suitable for small Go applications with minimal functionality.

---

### **2. Decent Project (Modular CLI Tool)**

For more complex Go applications with multiple packages, such as a CLI tool with subcommands or a small web server.

### **📂 Project Structure**

```markdown
/modular_go_project
│── main.go            # Main entry point
│── go.mod             # Go module file
│── README.md          # Project documentation
│── /cmd               # Folder for CLI commands
│   ├── root.go        # Root command
│   ├── greet.go       # Greet command
│── /pkg               # Folder for reusable packages
│   ├── greet          # Greet package
│   │   ├── greet.go   # Greet functionality
│── /internal          # Folder for internal packages
│   ├── utils          # Utility functions
│   │   ├── utils.go   # Utility functions
```

#### **Example Code:**

**main.go**

```go
package main

import (
    "modular_go_project/cmd"
)

func main() {
    cmd.Execute()
}
```

**cmd/root.go**

```go
package cmd

import (
    "fmt"
    "os"

    "github.com/spf13/cobra"
)

var rootCmd = &cobra.Command{
    Use:   "modular_go_project",
    Short: "A modular Go project",
    Long:  `This is a modular Go project with subcommands.`,
}

func Execute() {
    if err := rootCmd.Execute(); err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
}
```

**cmd/greet.go**

```go
package cmd

import (
    "fmt"

    "github.com/spf13/cobra"
    "modular_go_project/pkg/greet"
)

var greetCmd = &cobra.Command{
    Use:   "greet",
    Short: "Greet someone",
    Long:  `This command greets a person by name.`,
    Run: func(cmd *cobra.Command, args []string) {
        name := "World"
        if len(args) > 0 {
            name = args[0]
        }
        fmt.Println(greet.Greet(name))
    },
}

func init() {
    rootCmd.AddCommand(greetCmd)
}
```

**pkg/greet/greet.go**

```go
package greet

func Greet(name string) string {
    return fmt.Sprintf("Hello, %s!", name)
}
```

**internal/utils/utils.go**

```go
package utils

func IsEmpty(s string) bool {
    return len(s) == 0
}
```

**go.mod**

```go
module modular_go_project

go 1.20

require github.com/spf13/cobra v1.6.1
```

**README.md**

```markdown
# Modular Go Project

This is a modular Go project with subcommands.

## Installation

To run this project, clone the repository and run:

    ```bash
    go run main.go greet [name]
    ```

## Usage

This project is a CLI tool with subcommands. Use the `greet` command to greet someone by name.
```

#### **Explanation:**

- **`cmd/`**: Contains the CLI commands. The `root.go` file defines the root command, and `greet.go` defines a subcommand.
- **`pkg/`**: Contains reusable packages. The `greet` package provides the `Greet` function.
- **`internal/`**: Contains internal packages that are not meant to be imported by other projects. The `utils` package provides utility functions.
- **`go.mod`**: Defines the module path, Go version, and dependencies (e.g., `cobra` for CLI commands).

This structure is suitable for medium-sized Go applications with multiple packages and subcommands.

---

### **3. Larger Project (Web Application)**

For complex Go applications with multiple features, such as a web server with API endpoints, database integration, and user authentication.

### **📂 Project Structure**

```markdown
/larger_go_project
│── main.go            # Main entry point
│── go.mod             # Go module file
│── README.md          # Project documentation
│── /cmd               # Folder for CLI commands
│   ├── server.go      # Server command
│── /pkg               # Folder for reusable packages
│   ├── api            # API package
│   │   ├── api.go     # API handlers
│   ├── db             # Database package
│   │   ├── db.go      # Database connection and operations
│── /internal          # Folder for internal packages
│   ├── config         # Configuration package
│   │   ├── config.go  # Configuration management
│   ├── middleware     # Middleware package
│   │   ├── auth.go    # Authentication middleware
│── /migrations        # Folder for database migrations
│── /web               # Folder for web assets (templates, static files)
│   ├── templates      # HTML templates
│   ├── static         # Static files (CSS, JS, images)
```

#### **Example Code:**

**main.go**

```go
package main

import (
    "larger_go_project/cmd"
)

func main() {
    cmd.Execute()
}
```

**cmd/server.go**

```go
package cmd

import (
    "fmt"
    "log"
    "net/http"

    "github.com/spf13/cobra"
    "larger_go_project/pkg/api"
    "larger_go_project/pkg/db"
)

var serverCmd = &cobra.Command{
    Use:   "server",
    Short: "Start the web server",
    Long:  `This command starts the web server.`,
    Run: func(cmd *cobra.Command, args []string) {
        db.Connect()
        http.HandleFunc("/", api.HomeHandler)
        log.Fatal(http.ListenAndServe(":8080", nil))
    },
}

func init() {
    rootCmd.AddCommand(serverCmd)
}
```

**pkg/api/api.go**

```go
package api

import (
    "fmt"
    "net/http"
)

func HomeHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Welcome to the home page!")
}
```

**pkg/db/db.go**

```go
package db

import (
    "database/sql"
    "fmt"
    "log"

    _ "github.com/lib/pq"
)

var DB *sql.DB

func Connect() {
    var err error
    DB, err = sql.Open("postgres", "user=postgres dbname=mydb sslmode=disable")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Connected to the database!")
}
```

**internal/config/config.go**

```go
package config

import (
    "os"
)

type Config struct {
    DBUser string
    DBName string
}

func LoadConfig() Config {
    return Config{
        DBUser: os.Getenv("DB_USER"),
        DBName: os.Getenv("DB_NAME"),
    }
}
```

**go.mod**

```go
module larger_go_project

go 1.20

require (
    github.com/lib/pq v1.10.7
    github.com/spf13/cobra v1.6.1
)
```

**README.md**

```markdown
# Larger Go Project

This is a larger Go project with a web server, API endpoints, and database integration.

## Installation

To run this project, clone the repository and run:

    ```bash
    go run main.go server
    ```

## Usage

This project is a web server that listens on port 8080. Visit `http://localhost:8080` to see the home page.
```

#### **Explanation:**

- **`cmd/`**: Contains the CLI commands. The `server.go` file defines the command to start the web server.
- **`pkg/`**: Contains reusable packages. The `api` package defines API handlers, and the `db` package handles database connections.
- **`internal/`**: Contains internal packages. The `config` package manages configuration settings, and the `middleware` package provides middleware for authentication.
- **`migrations/`**: Contains database migration files.
- **`web/`**: Contains web assets such as HTML templates and static files.

This structure is suitable for large Go applications with multiple features, such as web servers, API endpoints, and database integration.

---

## Code Organization Guidelines

To maintain clarity, correctness, and maintainability in Go projects:

### **I. File & Module Organization**

- **Separate concerns**: Use separate packages for different components (e.g., API, database, configuration).
- **Modularize code**: Encapsulate related functionality in separate packages.
- **Use `internal/` for private packages**: Place packages that should not be imported by other projects in the `internal/` folder.

### **II. Functions & Control Flow**

- **Keep functions short**: Each function should perform a single task.
- **Avoid deep nesting**: Use early returns or guard clauses to simplify control flow.
- **Use meaningful names**: Function names should clearly describe their purpose (e.g., `Connect()`, `HomeHandler()`).

### **III. Naming & Conventions**

- **Follow Go conventions**: Use camelCase for function names and PascalCase for exported types and functions.
- **Use descriptive names**: Variable and function names should clearly convey their purpose.
- **Avoid magic numbers**: Use named constants instead of hardcoding values.

### **IV. Tools & Best Practices**

- **Use version control**: Track changes with Git to collaborate and manage code history.
- **Write meaningful comments**: Explain the "why" behind your code, especially for complex logic.
- **Test thoroughly**: Write unit tests for your functions and packages.
- **Optimize performance**: Use Go's profiling tools to identify and fix performance bottlenecks.

---

## Conclusion

By following this guide, Go projects will be structured in a way that ensures clarity, correctness, and maintainability. Whether you're building a simple CLI tool or a complex web application, these principles will help you create high-quality, maintainable code. Go's simplicity and performance make it an excellent choice for a wide range of applications, and with proper organization, you can ensure that your codebase remains easy to understand, debug, and extend.

Happy coding!
