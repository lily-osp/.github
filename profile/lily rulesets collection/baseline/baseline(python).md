# Lily's Python Development Guide

A structured approach to organizing Python projects, inspired by "Lily's Program Writing Sets" to ensure clarity, correctness, and maintainability.

---

## Introduction

This guide provides a systematic way to structure Python projects based on their complexity. Whether you're working on a small script or a large application, following this structure ensures readability, scalability, and maintainability. Python is a versatile language, and with proper organization, you can make your codebase easy to understand, debug, and extend.

## Guiding Principles

Following the core principles of "Lily's Program Writing Sets":

1. **Clarity** – Code should be easy to read, understand, and modify. Python's readability is one of its strengths, so leverage it by writing clean and self-explanatory code.
2. **Correctness** – Code should function as intended and handle errors gracefully. Python's dynamic nature requires careful error handling and testing to ensure robustness.
3. **Maintainability** – Code should be structured for long-term usability and modification. Python's modular design allows for easy refactoring and scaling.

## Python Project Structure

### **1. Basic Project (Small Scripts)**

For simple programs requiring minimal organization, such as one-off scripts or small utilities.

### **📂 Project Structure**

```
/project_name
│── main.py         # Core logic and execution
│── helper.py       # Utility functions to keep main.py clean
```

#### **Example Code:**

**main.py**

```python
from helper import greet

def main():
    name = input("Enter your name: ")
    print(greet(name))

if __name__ == "__main__":
    main()
```

**helper.py**

```python
def greet(name):
    return f"Hello, {name}! Welcome to the program."
```

#### **Explanation:**

- **`main.py`**: This is the entry point of the script. It handles the main logic and user interaction.
- **`helper.py`**: Contains reusable utility functions like `greet()`, which keeps the main file clean and focused.
- **`if __name__ == "__main__":`**: This ensures that the `main()` function runs only when the script is executed directly, not when imported as a module.

### **2. Decent Project (Small Applications)**

For small applications that require more organization, such as CLI tools or simple web services.

### **📂 Project Structure**

```
/decent_project
│── main.py         # Entry point, initializes the app
│── routes.py       # Manages API endpoints or function routing
│── helper.py       # Contains reusable utility functions
```

#### **Example Code:**

### **📌 `helper.py` (Utility Functions)**

```python
def greet(name):
    return f"Hello, {name}! Welcome to our system."
```

### **📌 `routes.py` (Function Routing)**

```python
from helper import greet

def handle_user_input():
    name = input("Enter your name: ")
    return greet(name)
```

### **📌 `main.py` (Application Entry Point)**

```python
from routes import handle_user_input

def main():
    print("Starting Decent Project...")
    message = handle_user_input()
    print(message)

if __name__ == "__main__":
    main()
```

#### **✅ How It Works**

1. **`main.py`**: This is the entry point of the application. It initializes the app and calls the `handle_user_input()` function from `routes.py`.
2. **`routes.py`**: Handles user input and logic flow. It calls the `greet()` function from `helper.py` to generate the greeting message.
3. **`helper.py`**: Contains reusable utility functions like `greet()` that are used across the application.

When you run the program, it will ask for a name and display a greeting. This structure keeps the code modular and easy to extend.

### **3. Larger Project (Well-Organized Structure)**

For scalable projects requiring better modularization, such as web applications, data pipelines, or complex CLI tools.

## **📂 Project Structure**

```
/larger_project
│── main.py         # Application entry point
│── routes.py       # Manages API endpoints
│── helper.py       # Utility functions
│── models.py       # Database models (SQLAlchemy)
│── config.py       # Configuration settings
│── exceptions.py   # Custom error handling
│── requirements.txt # Dependencies list
│── README.md       # Project documentation
│── .gitignore      # Git ignore file
│
├── /logs
│   ├── app.log     # Main log file
│   ├── error.log   # Error log file
│
├── /tests
│   ├── test_main.py
│   ├── test_routes.py
│
├── /static         # Static files (if needed)
│── /templates      # HTML templates (if using a web framework)
│── /data           # Data storage (SQLite database)
│   ├── database.db # SQLite database file
```

## **📜 Detailed Code Implementation**

### **1️⃣ `config.py` (Configuration Management)**

This file handles environment variables, debug settings, and database configurations.

```python
import os

class Config:
    APP_NAME = "Larger Project"
    DEBUG = True
    LOG_FILE = "logs/app.log"
    ERROR_LOG_FILE = "logs/error.log"
    DATABASE_URL = "sqlite:///data/database.db"

# Load environment variables if available
Config.APP_NAME = os.getenv("APP_NAME", Config.APP_NAME)
Config.DEBUG = os.getenv("DEBUG", str(Config.DEBUG)).lower() == "true"
```

#### **Explanation:**

- **`Config` class**: Centralizes all configuration settings, such as the application name, debug mode, and database URL.
- **Environment variables**: You can override default settings using environment variables, which is useful for deployment.

### **2️⃣ `models.py` (Database Models using SQLAlchemy)**

Using **SQLAlchemy** to define a User model and handle database operations.

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, sessionmaker
from config import Config

Base = declarative_base()
engine = create_engine(Config.DATABASE_URL, echo=Config.DEBUG)
Session = sessionmaker(bind=engine)
session = Session()

class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String, nullable=False)
    age = Column(Integer, nullable=False)

    def __repr__(self):
        return f"<User(id={self.id}, name='{self.name}', age={self.age})>"

# Create tables if they don’t exist
Base.metadata.create_all(engine)
```

#### **Explanation:**

- **`Base`**: A base class for all models, provided by SQLAlchemy.
- **`User` class**: Defines the structure of the `users` table in the database.
- **`session`**: Manages database operations like adding, querying, and deleting records.
- **`Base.metadata.create_all(engine)`**: Creates the database tables if they don't already exist.

### **3️⃣ `helper.py` (Utility Functions)**

A helper function to validate user input.

```python
def validate_age(age):
    """Check if the age is a valid positive integer."""
    return age.isdigit() and int(age) > 0
```

#### **Explanation:**

- **`validate_age()`**: Ensures that the age input is a valid positive integer. This function is reusable and can be called from multiple places in the code.

### **4️⃣ `exceptions.py` (Custom Error Handling)**

Defines custom exceptions for better error handling.

```python
class InvalidUserError(Exception):
    """Raised when user input is invalid."""
    pass

class DatabaseError(Exception):
    """Raised when a database operation fails."""
    pass
```

#### **Explanation:**

- **Custom exceptions**: These exceptions make it easier to handle specific errors, such as invalid user input or database failures.

### **5️⃣ `routes.py` (Handling User Interactions & Database Operations)**

Manages user interactions, input validation, and database interactions.

```python
from models import User, session
from helper import validate_age
from exceptions import InvalidUserError, DatabaseError
import logging

# Configure logging
logging.basicConfig(filename="logs/app.log", level=logging.INFO,
                    format="%(asctime)s - %(levelname)s - %(message)s")

def add_user(name, age):
    """Adds a user to the database."""
    try:
        if not name or not validate_age(age):
            raise InvalidUserError("Invalid input: Name cannot be empty, and age must be a positive number.")

        user = User(name=name, age=int(age))
        session.add(user)
        session.commit()

        logging.info(f"User added: {user}")
        return f"User {name} added successfully!"

    except InvalidUserError as e:
        logging.error(f"InvalidUserError: {e}")
        return str(e)

    except Exception as e:
        session.rollback()
        logging.error(f"DatabaseError: {e}")
        raise DatabaseError("An error occurred while adding the user.")

def list_users():
    """Retrieves all users from the database."""
    users = session.query(User).all()
    return users if users else "No users found."
```

#### **Explanation:**

- **`add_user()`**: Adds a new user to the database after validating the input. It logs the operation and handles errors gracefully.
- **`list_users()`**: Retrieves all users from the database and returns them as a list.
- **Logging**: Logs important events and errors to `logs/app.log` for debugging and monitoring.

### **6️⃣ `main.py` (Application Entry Point)**

Handles user interaction via console.

```python
from routes import add_user, list_users

def main():
    print("Welcome to Larger Project")

    while True:
        print("\nOptions:")
        print("1. Add User")
        print("2. List Users")
        print("3. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            name = input("Enter name: ")
            age = input("Enter age: ")
            result = add_user(name, age)
            print(result)

        elif choice == "2":
            users = list_users()
            print("\nUsers in Database:")
            for user in users:
                print(user)

        elif choice == "3":
            print("Exiting...")
            break

        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
```

#### **Explanation:**

- **`main()`**: The entry point of the application. It provides a simple menu for adding users, listing users, and exiting the program.
- **User interaction**: The program interacts with the user via the console, making it easy to test and use.

### **7️⃣ `requirements.txt` (Dependencies List)**

```
sqlalchemy
```

#### **Explanation:**

- **`requirements.txt`**: Lists all the dependencies required for the project. In this case, only `SQLAlchemy` is needed for database operations.

### **8️⃣ `tests/test_routes.py` (Unit Tests for User Handling)**

Using `pytest` to test adding users and retrieving data.

```python
import pytest
from routes import add_user, list_users
from models import session, User

def test_add_valid_user():
    """Test adding a valid user"""
    result = add_user("Alice", "25")
    assert "added successfully" in result

def test_add_invalid_user():
    """Test adding a user with invalid data"""
    result = add_user("", "25")
    assert "Invalid input" in result

def test_list_users():
    """Test retrieving users from database"""
    users = list_users()
    assert isinstance(users, list)

@pytest.fixture(scope="function", autouse=True)
def cleanup():
    """Clean database after tests"""
    yield
    session.query(User).delete()
    session.commit()
```

#### **Explanation:**

- **Unit tests**: These tests ensure that the `add_user()` and `list_users()` functions work as expected.
- **`cleanup` fixture**: Cleans up the database after each test to ensure a clean state.

## **✅ Features & Expansion Plan**

1. **💾 Persistent Data** → Uses **SQLite** for user storage.
2. **📜 Logging System** → Stores logs in `logs/app.log`.
3. **⚡ Modular Structure** → Scalable for web API integration (Flask, FastAPI).
4. **🧪 Unit Testing** → Tests user input validation and database operations.
5. **🔧 Future Expansions:**
   - API version (Flask/FastAPI).
   - Authentication (JWT/OAuth).
   - Docker containerization.

## Code Organization Guidelines

To maintain clarity, correctness, and maintainability:

### **I. File & Module Organization**

- **Minimize nesting**: Keep the project hierarchy simple.
- **Encapsulate common logic**: Use `helper.py` for reusable functions.
- **Separate concerns**: Use dedicated files for configuration, models, and error handling.

### **II. Functions & Control Flow**

- **Restrict function complexity**: Keep functions short and focused.
- **Limit function parameters**: Use objects or dictionaries to group related data.
- **Use clear control flow structures**: Avoid deep nesting and excessive conditions.

### **III. Naming & Conventions**

- Follow a consistent naming convention for files, functions, and variables.
- Use descriptive names that clearly convey functionality.

### **IV. Tools & Best Practices**

- Use **version control (Git)** to track changes and collaborate.
- Write **meaningful comments** to explain the "why" behind decisions.
- Log important application events using structured logging.
- Write **unit tests** to ensure code reliability.

## Conclusion

By following this guide, Python projects will be structured in a way that ensures clarity, correctness, and maintainability. As projects grow, refining these principles will help maintain high-quality, maintainable code. Python's flexibility and readability make it an excellent choice for projects of all sizes, and with proper organization, you can ensure that your codebase remains easy to understand, debug, and extend.

Happy coding!
