# Lily's PHP Development Guide

A structured approach to organizing PHP projects, inspired by "Lily's Program Writing Sets" to ensure clarity, correctness, and maintainability. This guide focuses on developing web applications using PHP, a popular server-side scripting language.

---

## Introduction

This guide provides a systematic way to structure PHP projects, whether you're building a simple website or a complex web application. PHP is a versatile language, but without proper organization, projects can quickly become difficult to manage. This guide will help you structure your PHP projects for scalability, readability, and maintainability.

## Guiding Principles

Following the core principles of "Lily's Program Writing Sets":

1. **Clarity** – Code should be easy to read, understand, and modify. PHP scripts, functions, and classes should be well-organized and self-explanatory.
2. **Correctness** – Code should function as intended and handle errors gracefully. PHP applications should be robust, with proper error handling and validation.
3. **Maintainability** – Code should be structured for long-term usability and modification. Modular design and separation of concerns are key.

---

## PHP Project Structure

### **1. Basic Project (Simple PHP Script)**

For simple PHP scripts or small websites, such as a personal blog or a contact form.

### **📂 Project Structure**

```
/simple_php_project
│── index.php            # Main PHP script
│── contact.php          # Contact form script
│── styles.css           # Global styles
│── script.js            # Global JavaScript
│── /includes            # Folder for reusable PHP code
│   ├── header.php       # Header template
│   ├── footer.php       # Footer template
│── /assets             # Folder for static files (CSS, JS, images)
│   ├── images           # Folder for images
│   ├── fonts            # Folder for custom fonts
```

#### **Example Code:**

**index.php**

```php
<?php
// Include the header
include 'includes/header.php';
?>

<h1>Welcome to My PHP Website</h1>
<p>This is a simple PHP website.</p>

<?php
// Include the footer
include 'includes/footer.php';
?>
```

**contact.php**

```php
<?php
// Include the header
include 'includes/header.php';
?>

<h1>Contact Us</h1>
<form action="submit_contact.php" method="post">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" required>
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required>
  <label for="message">Message:</label>
  <textarea id="message" name="message" required></textarea>
  <button type="submit">Submit</button>
</form>

<?php
// Include the footer
include 'includes/footer.php';
?>
```

**includes/header.php**

```php
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My PHP Website</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header>
    <h1>My PHP Website</h1>
    <nav>
      <ul>
        <li><a href="index.php">Home</a></li>
        <li><a href="contact.php">Contact</a></li>
      </ul>
    </nav>
  </header>
```

**includes/footer.php**

```php
  <footer>
    <p>© 2023 My PHP Website</p>
  </footer>
  <script src="script.js"></script>
</body>
</html>
```

**styles.css**

```css
/* Global Styles */
body {
  font-family: Arial, sans-serif;
  line-height: 1.6;
  margin: 0;
  padding: 0;
}

header {
  background: #333;
  color: #fff;
  padding: 1rem 0;
  text-align: center;
}

nav ul {
  list-style: none;
  padding: 0;
}

nav ul li {
  display: inline;
  margin: 0 10px;
}

nav ul li a {
  color: #fff;
  text-decoration: none;
}

footer {
  background: #333;
  color: #fff;
  text-align: center;
  padding: 1rem 0;
}
```

**script.js**

```javascript
// Simple JavaScript for interactivity
document.addEventListener('DOMContentLoaded', () => {
  console.log('PHP website loaded!');
});
```

#### **Explanation:**

- **`index.php`**: The main PHP script that displays the home page. It includes the header and footer templates.
- **`contact.php`**: A PHP script that displays a contact form. It also includes the header and footer templates.
- **`includes/header.php` and `includes/footer.php`**: Reusable PHP templates for the header and footer, which are included in multiple pages.
- **`styles.css` and `script.js`**: Global styles and JavaScript for the website.
- **`/assets`**: Contains static files like images and fonts.

This structure is suitable for small PHP websites with minimal functionality.

---

### **2. Decent Project (Modular PHP App)**

For more complex PHP applications with multiple features, such as a blog with user authentication or a small e-commerce site.

### **📂 Project Structure**

```
/modular_php_app
│── index.php            # Main entry point
│── contact.php          # Contact form script
│── styles.css           # Global styles
│── script.js            # Global JavaScript
│── /includes            # Folder for reusable PHP code
│   ├── header.php       # Header template
│   ├── footer.php       # Footer template
│   ├── db.php           # Database connection script
│   ├── functions.php    # Reusable functions
│── /assets             # Folder for static files (CSS, JS, images)
│   ├── images           # Folder for images
│   ├── fonts            # Folder for custom fonts
│── /classes            # Folder for PHP classes
│   ├── User.php         # User class
│── /pages              # Folder for additional PHP pages
│   ├── about.php        # About page
│   ├── login.php        # Login page
│── /admin              # Folder for admin-related scripts
│   ├── dashboard.php    # Admin dashboard
```

#### **Example Code:**

**index.php**

```php
<?php
// Include the header
include 'includes/header.php';

// Include the database connection
include 'includes/db.php';

// Fetch some data from the database
$query = "SELECT * FROM posts LIMIT 5";
$result = mysqli_query($conn, $query);
?>

<h1>Welcome to My PHP App</h1>
<ul>
  <?php while ($row = mysqli_fetch_assoc($result)): ?>
    <li><?php echo $row['title']; ?></li>
  <?php endwhile; ?>
</ul>

<?php
// Include the footer
include 'includes/footer.php';
?>
```

**includes/db.php**

```php
<?php
// Database connection
$host = 'localhost';
$user = 'root';
$password = '';
$database = 'myapp';

$conn = mysqli_connect($host, $user, $password, $database);

if (!$conn) {
  die("Database connection failed: " . mysqli_connect_error());
}
?>
```

**includes/functions.php**

```php
<?php
function sanitize_input($data) {
  return htmlspecialchars(stripslashes(trim($data)));
}
?>
```

**classes/User.php**

```php
<?php
class User {
  private $id;
  private $username;
  private $email;

  public function __construct($id, $username, $email) {
    $this->id = $id;
    $this->username = $username;
    $this->email = $email;
  }

  public function getUsername() {
    return $this->username;
  }

  public function getEmail() {
    return $this->email;
  }
}
?>
```

**pages/login.php**

```php
<?php
// Include the header
include '../includes/header.php';

// Handle login logic here
?>

<h1>Login</h1>
<form action="login_process.php" method="post">
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" required>
  <label for="password">Password:</label>
  <input type="password" id="password" name="password" required>
  <button type="submit">Login</button>
</form>

<?php
// Include the footer
include '../includes/footer.php';
?>
```

#### **Explanation:**

- **Modular Structure**: The application is split into multiple files and folders for better organization. The `includes` folder contains reusable PHP code, such as database connections and functions.
- **Database Integration**: The `db.php` file handles the database connection, and the `index.php` script fetches data from the database.
- **Reusable Functions**: The `functions.php` file contains reusable functions, such as input sanitization.
- **Classes**: The `classes` folder contains PHP classes, such as the `User` class, which encapsulates user-related functionality.
- **Admin Section**: The `admin` folder contains scripts for admin-related functionality, such as a dashboard.

This structure is suitable for medium-sized PHP applications with multiple features and database integration.

---

### **3. Larger Project (Complex PHP App)**

For complex PHP applications with multiple features, such as user authentication, API integration, and advanced database operations.

### **📂 Project Structure**

```
/complex_php_app
│── index.php            # Main entry point
│── contact.php          # Contact form script
│── styles.css           # Global styles
│── script.js            # Global JavaScript
│── /includes            # Folder for reusable PHP code
│   ├── header.php       # Header template
│   ├── footer.php       # Footer template
│   ├── db.php           # Database connection script
│   ├── functions.php    # Reusable functions
│── /assets             # Folder for static files (CSS, JS, images)
│   ├── images           # Folder for images
│   ├── fonts            # Folder for custom fonts
│── /classes            # Folder for PHP classes
│   ├── User.php         # User class
│   ├── Post.php         # Post class
│── /pages              # Folder for additional PHP pages
│   ├── about.php        # About page
│   ├── login.php        # Login page
│── /admin              # Folder for admin-related scripts
│   ├── dashboard.php    # Admin dashboard
│── /api                # Folder for API-related scripts
│   ├── api.php          # API entry point
│── /utils              # Folder for utility functions
│   ├── validation.php   # Input validation functions
│── /logs               # Folder for log files
│   ├── error.log        # Error log file
```

#### **Example Code:**

**index.php**

```php
<?php
// Include the header
include 'includes/header.php';

// Include the database connection
include 'includes/db.php';

// Fetch some data from the database
$query = "SELECT * FROM posts LIMIT 5";
$result = mysqli_query($conn, $query);
?>

<h1>Welcome to My PHP App</h1>
<ul>
  <?php while ($row = mysqli_fetch_assoc($result)): ?>
    <li><?php echo $row['title']; ?></li>
  <?php endwhile; ?>
</ul>

<?php
// Include the footer
include 'includes/footer.php';
?>
```

**api/api.php**

```php
<?php
header('Content-Type: application/json');

// Include the database connection
include '../includes/db.php';

// Fetch some data from the database
$query = "SELECT * FROM posts LIMIT 5";
$result = mysqli_query($conn, $query);

$posts = [];
while ($row = mysqli_fetch_assoc($result)) {
  $posts[] = $row;
}

echo json_encode($posts);
?>
```

**utils/validation.php**

```php
<?php
function validate_email($email) {
  return filter_var($email, FILTER_VALIDATE_EMAIL);
}

function validate_password($password) {
  return strlen($password) >= 8;
}
?>
```

**logs/error.log**

```plaintext
[2023-10-01 12:00:00] ERROR: Database connection failed.
[2023-10-01 12:05:00] ERROR: Invalid user input.
```

#### **Explanation:**

- **Modular Structure**: The application is split into multiple modules (`api`, `utils`, `logs`) for better organization and scalability.
- **API Integration**: The `api` folder contains scripts for serving JSON data, which can be consumed by front-end applications or external services.
- **Utility Functions**: The `utils` folder contains reusable utility functions, such as input validation.
- **Logging**: The `logs` folder contains log files for tracking errors and other important events.

This structure is suitable for large PHP applications with multiple features, such as user authentication, API integration, and advanced error handling.

---

## Code Organization Guidelines

To maintain clarity, correctness, and maintainability in PHP projects:

### **I. File & Module Organization**

- **Separate concerns**: Use separate files for different components (e.g., database, templates, classes).
- **Modularize code**: Encapsulate related functionality in separate files and folders.
- **Use includes**: Reuse common code (e.g., header, footer) by including them in multiple scripts.

### **II. Functions & Control Flow**

- **Keep functions short**: Each function should perform a single task.
- **Avoid deep nesting**: Use early returns or guard clauses to simplify control flow.
- **Use meaningful names**: Function names should clearly describe their purpose (e.g., `validate_email()`, `fetch_posts()`).

### **III. Naming & Conventions**

- **Follow PHP conventions**: Use snake_case for file names and function names.
- **Use descriptive names**: Variable and function names should clearly convey their purpose.
- **Avoid magic numbers**: Use named constants instead of hardcoding values.

### **IV. Tools & Best Practices**

- **Use version control**: Track changes with Git to collaborate and manage code history.
- **Write meaningful comments**: Explain the "why" behind your code, especially for complex logic.
- **Test thoroughly**: Write unit tests for your functions and classes.
- **Optimize performance**: Use caching, database indexing, and other techniques to improve performance.

---

## Conclusion

By following this guide, PHP projects will be structured in a way that ensures clarity, correctness, and maintainability. Whether you're building a simple website or a complex web application, these principles will help you create high-quality, maintainable code. PHP's flexibility and simplicity make it an excellent choice for web development, and with proper organization, you can ensure that your codebase remains easy to understand, debug, and extend.

Happy coding!
