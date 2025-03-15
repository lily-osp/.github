# Lily's Rust Development Guide

A structured approach to organizing Rust projects, inspired by "Lily's Program Writing Sets" to ensure clarity, correctness, and maintainability. This guide focuses on developing applications using Rust, a systems programming language known for its safety, performance, and concurrency features.

---

## Introduction

This guide provides a systematic way to structure Rust projects, whether you're building a small CLI tool, a web server, or a complex system-level application. Rust's emphasis on memory safety and zero-cost abstractions makes it a powerful choice for a wide range of applications. However, without proper organization, Rust projects can become difficult to manage. This guide will help you structure your Rust projects for scalability, readability, and maintainability.

## Guiding Principles

Following the core principles of "Lily's Program Writing Sets":

1. **Clarity** – Code should be easy to read, understand, and modify. Rust's strong type system and ownership model help achieve this, but proper organization is still key.
2. **Correctness** – Code should function as intended and handle errors gracefully. Rust's compile-time checks and explicit error handling ensure robustness.
3. **Maintainability** – Code should be structured for long-term usability and modification. Modular design and separation of concerns are key.

---

## Rust Project Structure

### **1. Basic Project (Simple CLI Tool)**

For simple Rust applications, such as a CLI tool or a small utility.

### **📂 Project Structure**

```markdown
/simple_rust_project
│── src/
│   ├── main.rs          # Main entry point
│── Cargo.toml           # Project metadata and dependencies
│── README.md            # Project documentation
```

#### **Example Code:**

**src/main.rs**

```rust
fn main() {
    println!("Hello, World!");
}
```

**Cargo.toml**

```toml
[package]
name = "simple_rust_project"
version = "0.1.0"
edition = "2021"

[dependencies]
```

**README.md**

```markdown
# Simple Rust Project

This is a simple Rust project that prints "Hello, World!" to the console.

## Installation

To run this project, clone the repository and run:

    ```bash
    cargo run
    ```

## Usage

This project is a simple CLI tool that prints a greeting message.
```

#### **Explanation:**

- **`src/main.rs`**: The main entry point of the application. It contains the `main` function, which is the starting point of the program.
- **`Cargo.toml`**: The manifest file for Rust projects. It defines the project's metadata and dependencies.
- **`README.md`**: Provides a brief description of the project, installation instructions, and usage.

This structure is suitable for small Rust applications with minimal functionality.

---

### **2. Decent Project (Modular CLI Tool)**

For more complex Rust applications with multiple modules, such as a CLI tool with subcommands or a small web server.

### **📂 Project Structure**

```markdown
/modular_rust_project
│── src/
│   ├── main.rs          # Main entry point
│   ├── lib.rs           # Library crate root
│   ├── cli/             # CLI module
│   │   ├── mod.rs       # CLI module root
│   │   ├── greet.rs     # Greet subcommand
│── Cargo.toml           # Project metadata and dependencies
│── README.md            # Project documentation
```

#### **Example Code:**

**src/main.rs**

```rust
use modular_rust_project::cli::Cli;

fn main() {
    Cli::run();
}
```

**src/lib.rs**

```rust
pub mod cli;
```

**src/cli/mod.rs**

```rust
use clap::{Parser, Subcommand};

#[derive(Parser)]
pub struct Cli {
    #[clap(subcommand)]
    command: Commands,
}

impl Cli {
    pub fn run() {
        let cli = Cli::parse();
        match cli.command {
            Commands::Greet { name } => {
                println!("Hello, {}!", name.unwrap_or("World".to_string()));
            }
        }
    }
}

#[derive(Subcommand)]
enum Commands {
    Greet {
        #[clap(short, long)]
        name: Option<String>,
    },
}
```

**src/cli/greet.rs**

```rust
pub fn greet(name: Option<String>) {
    println!("Hello, {}!", name.unwrap_or("World".to_string()));
}
```

**Cargo.toml**

```toml
[package]
name = "modular_rust_project"
version = "0.1.0"
edition = "2021"

[dependencies]
clap = { version = "4.0", features = ["derive"] }
```

**README.md**

```markdown
# Modular Rust Project

This is a modular Rust project with subcommands.

## Installation

To run this project, clone the repository and run:

    ```bash
    cargo run -- greet --name Alice
    ```

## Usage

This project is a CLI tool with subcommands. Use the `greet` command to greet someone by name.
```

#### **Explanation:**

- **`src/lib.rs`**: The root of the library crate. It declares the `cli` module.
- **`src/cli/mod.rs`**: The root of the CLI module. It defines the `Cli` struct and `Commands` enum using the `clap` crate for command-line argument parsing.
- **`src/cli/greet.rs`**: Contains the `greet` function, which is called when the `greet` subcommand is executed.
- **`Cargo.toml`**: Defines the project's metadata and dependencies (e.g., `clap` for CLI argument parsing).

This structure is suitable for medium-sized Rust applications with multiple modules and subcommands.

---

### **3. Larger Project (Web Application)**

For complex Rust applications with multiple features, such as a web server with API endpoints, database integration, and user authentication.

### **📂 Project Structure**

```markdown
/larger_rust_project
│── src/
│   ├── main.rs          # Main entry point
│   ├── lib.rs           # Library crate root
│   ├── api/             # API module
│   │   ├── mod.rs       # API module root
│   │   ├── handlers.rs  # API handlers
│   ├── db/              # Database module
│   │   ├── mod.rs       # Database module root
│   │   ├── models.rs    # Database models
│   ├── config/          # Configuration module
│   │   ├── mod.rs       # Configuration module root
│   │   ├── settings.rs  # Configuration settings
│   ├── middleware/      # Middleware module
│   │   ├── mod.rs       # Middleware module root
│   │   ├── auth.rs      # Authentication middleware
│── migrations/          # Folder for database migrations
│── Cargo.toml           # Project metadata and dependencies
│── README.md            # Project documentation
```

#### **Example Code:**

**src/main.rs**

```rust
use larger_rust_project::api::start_server;

fn main() {
    start_server();
}
```

**src/lib.rs**

```rust
pub mod api;
pub mod db;
pub mod config;
pub mod middleware;
```

**src/api/mod.rs**

```rust
use actix_web::{web, App, HttpServer};
use larger_rust_project::middleware::auth::AuthMiddleware;

pub fn start_server() {
    HttpServer::new(|| {
        App::new()
            .wrap(AuthMiddleware)
            .route("/", web::get().to(handlers::home))
    })
    .bind("127.0.0.1:8080")
    .unwrap()
    .run()
    .unwrap();
}

mod handlers {
    use actix_web::HttpResponse;

    pub async fn home() -> HttpResponse {
        HttpResponse::Ok().body("Welcome to the home page!")
    }
}
```

**src/db/mod.rs**

```rust
use diesel::prelude::*;
use dotenv::dotenv;
use std::env;

pub fn establish_connection() -> PgConnection {
    dotenv().ok();

    let database_url = env::var("DATABASE_URL").expect("DATABASE_URL must be set");
    PgConnection::establish(&database_url).expect(&format!("Error connecting to {}", database_url))
}
```

**src/config/mod.rs**

```rust
use std::env;

pub struct Config {
    pub db_url: String,
}

impl Config {
    pub fn new() -> Self {
        let db_url = env::var("DATABASE_URL").expect("DATABASE_URL must be set");
        Config { db_url }
    }
}
```

**src/middleware/mod.rs**

```rust
pub mod auth;
```

**src/middleware/auth.rs**

```rust
use actix_web::{dev::ServiceRequest, Error};
use actix_web_httpauth::extractors::bearer::BearerAuth;

pub struct AuthMiddleware;

impl actix_web::dev::Transform<S, ServiceRequest> for AuthMiddleware {
    type Response = ServiceResponse;
    type Error = Error;
    type Transform = AuthMiddlewareService<S>;
    type InitError = ();
    type Future = Ready<Result<Self::Transform, Self::InitError>>;

    fn new_transform(&self, service: S) -> Self::Future {
        ready(Ok(AuthMiddlewareService { service }))
    }
}

pub struct AuthMiddlewareService<S> {
    service: S,
}

impl<S, B> Service<ServiceRequest> for AuthMiddlewareService<S>
where
    S: Service<ServiceRequest, Response = ServiceResponse<B>, Error = Error>,
{
    type Response = ServiceResponse<B>;
    type Error = Error;
    type Future = S::Future;

    fn poll_ready(&self, ctx: &mut Context<'_>) -> Poll<Result<(), Self::Error>> {
        self.service.poll_ready(ctx)
    }

    fn call(&self, req: ServiceRequest) -> Self::Future {
        let bearer = BearerAuth::from_request(&req).unwrap();
        // Perform authentication logic here
        self.service.call(req)
    }
}
```

**Cargo.toml**

```toml
[package]
name = "larger_rust_project"
version = "0.1.0"
edition = "2021"

[dependencies]
actix-web = "4.0"
diesel = { version = "1.4", features = ["postgres"] }
dotenv = "0.15"
actix-web-httpauth = "0.6"
```

**README.md**

```markdown
# Larger Rust Project

This is a larger Rust project with a web server, API endpoints, and database integration.

## Installation

To run this project, clone the repository and run:

    ```bash
    cargo run
    ```

## Usage

This project is a web server that listens on port 8080. Visit `http://localhost:8080` to see the home page.
```

#### **Explanation:**

- **`src/lib.rs`**: The root of the library crate. It declares the `api`, `db`, `config`, and `middleware` modules.
- **`src/api/mod.rs`**: The root of the API module. It defines the `start_server` function, which sets up the web server using the `actix-web` framework.
- **`src/db/mod.rs`**: The root of the database module. It provides a function to establish a connection to the database using the `diesel` ORM.
- **`src/config/mod.rs`**: The root of the configuration module. It defines the `Config` struct for managing configuration settings.
- **`src/middleware/mod.rs`**: The root of the middleware module. It declares the `auth` module for authentication middleware.
- **`src/middleware/auth.rs`**: Contains the `AuthMiddleware` struct, which implements authentication logic for the web server.
- **`Cargo.toml`**: Defines the project's metadata and dependencies (e.g., `actix-web` for web server functionality, `diesel` for database operations).

This structure is suitable for large Rust applications with multiple features, such as web servers, API endpoints, and database integration.

---

## Code Organization Guidelines

To maintain clarity, correctness, and maintainability in Rust projects:

### **I. File & Module Organization**

- **Separate concerns**: Use separate modules for different components (e.g., API, database, configuration).
- **Modularize code**: Encapsulate related functionality in separate modules.
- **Use `lib.rs` for library code**: Place reusable code in the `lib.rs` file to create a library crate.

### **II. Functions & Control Flow**

- **Keep functions short**: Each function should perform a single task.
- **Avoid deep nesting**: Use early returns or guard clauses to simplify control flow.
- **Use meaningful names**: Function names should clearly describe their purpose (e.g., `establish_connection()`, `start_server()`).

### **III. Naming & Conventions**

- **Follow Rust conventions**: Use snake_case for function and variable names, and PascalCase for types and traits.
- **Use descriptive names**: Variable and function names should clearly convey their purpose.
- **Avoid magic numbers**: Use named constants instead of hardcoding values.

### **IV. Tools & Best Practices**

- **Use version control**: Track changes with Git to collaborate and manage code history.
- **Write meaningful comments**: Explain the "why" behind your code, especially for complex logic.
- **Test thoroughly**: Write unit tests for your functions and modules.
- **Optimize performance**: Use Rust's profiling tools to identify and fix performance bottlenecks.

---

## Conclusion

By following this guide, Rust projects will be structured in a way that ensures clarity, correctness, and maintainability. Whether you're building a simple CLI tool or a complex web application, these principles will help you create high-quality, maintainable code. Rust's emphasis on safety and performance makes it an excellent choice for a wide range of applications, and with proper organization, you can ensure that your codebase remains easy to understand, debug, and extend.

Happy coding!
