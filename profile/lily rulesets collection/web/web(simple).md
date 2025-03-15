# Lily's Web Development Guide

A structured approach to organizing web projects, inspired by "Lily's Program Writing Sets" to ensure clarity, correctness, and maintainability. This guide focuses on developing web applications using HTML, CSS, and JavaScript.

---

## Introduction

This guide provides a systematic way to structure web projects, whether you're building a simple static website or a complex web application. Web development involves multiple technologies (HTML, CSS, JavaScript), so organizing your code properly is essential for readability, scalability, and maintainability.

## Guiding Principles

Following the core principles of "Lily's Program Writing Sets":

1. **Clarity** – Code should be easy to read, understand, and modify. HTML, CSS, and JavaScript should be well-structured and self-explanatory.
2. **Correctness** – Code should function as intended and handle errors gracefully. JavaScript, in particular, should be robust and handle edge cases.
3. **Maintainability** – Code should be structured for long-term usability and modification. Modular design and separation of concerns are key.

---

## Web Project Structure

### **1. Basic Project (Static Website)**

For simple static websites with a few pages, such as a personal portfolio or a small business website.

### **📂 Project Structure**

```
/static_website
│── index.html          # Main HTML file
│── styles.css          # Global styles
│── script.js           # Global JavaScript
│── /images             # Folder for images
│── /fonts              # Folder for custom fonts
```

#### **Example Code:**

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Static Website</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header>
    <h1>Welcome to My Website</h1>
    <nav>
      <ul>
        <li><a href="#about">About</a></li>
        <li><a href="#services">Services</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
  </header>

  <section id="about">
    <h2>About Me</h2>
    <p>This is a simple static website.</p>
  </section>

  <section id="services">
    <h2>Services</h2>
    <p>Here are the services I offer.</p>
  </section>

  <section id="contact">
    <h2>Contact</h2>
    <p>Get in touch with me.</p>
  </section>

  <footer>
    <p>© 2023 My Static Website</p>
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

section {
  padding: 2rem;
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
  console.log('Website loaded!');
});
```

#### **Explanation:**

- **`index.html`**: The main HTML file that defines the structure of the website. It includes links to the CSS and JavaScript files.
- **`styles.css`**: Contains global styles for the website, such as fonts, colors, and layout.
- **`script.js`**: Contains JavaScript for interactivity. In this example, it simply logs a message when the page loads.
- **`/images` and `/fonts`**: Folders for storing images and custom fonts, respectively.

This structure is suitable for small, static websites with minimal interactivity.

---

### **2. Decent Project (Dynamic Website)**

For more complex websites with dynamic content, such as a blog or a small web application.

### **📂 Project Structure**

```
/dynamic_website
│── index.html          # Main HTML file
│── styles.css          # Global styles
│── script.js           # Global JavaScript
│── /components         # Folder for reusable components (e.g., header, footer)
│── /pages              # Folder for additional HTML pages
│── /images             # Folder for images
│── /fonts              # Folder for custom fonts
```

#### **Example Code:**

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Dynamic Website</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <!-- Header Component -->
  <header id="header"></header>

  <main>
    <section id="about">
      <h2>About Me</h2>
      <p>This is a dynamic website with reusable components.</p>
    </section>

    <section id="services">
      <h2>Services</h2>
      <p>Here are the services I offer.</p>
    </section>
  </main>

  <!-- Footer Component -->
  <footer id="footer"></footer>

  <script src="script.js"></script>
</body>
</html>
```

**components/header.html**

```html
<header>
  <h1>Welcome to My Website</h1>
  <nav>
    <ul>
      <li><a href="#about">About</a></li>
      <li><a href="#services">Services</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
  </nav>
</header>
```

**components/footer.html**

```html
<footer>
  <p>© 2023 My Dynamic Website</p>
</footer>
```

**script.js**

```javascript
// Load reusable components
document.addEventListener('DOMContentLoaded', () => {
  // Load header
  fetch('components/header.html')
    .then(response => response.text())
    .then(data => {
      document.getElementById('header').innerHTML = data;
    });

  // Load footer
  fetch('components/footer.html')
    .then(response => response.text())
    .then(data => {
      document.getElementById('footer').innerHTML = data;
    });
});
```

#### **Explanation:**

- **Reusable Components**: The header and footer are moved to separate HTML files (`header.html` and `footer.html`) in the `components` folder. This allows for easy reuse across multiple pages.
- **Dynamic Loading**: JavaScript (`script.js`) dynamically loads the header and footer components using the `fetch` API.
- **`/pages` Folder**: Additional HTML pages (e.g., `about.html`, `contact.html`) can be stored here for a multi-page website.

This structure is suitable for websites with reusable components and dynamic content.

---

### **3. Larger Project (Web Application)**

For complex web applications with multiple features, such as a dashboard, user authentication, or API integration.

### **📂 Project Structure**

```
/web_application
│── index.html          # Main HTML file
│── styles.css          # Global styles
│── script.js           # Global JavaScript
│── /components         # Folder for reusable components (e.g., navbar, sidebar)
│── /pages              # Folder for additional HTML pages
│── /styles             # Folder for modular CSS files
│── /scripts            # Folder for modular JavaScript files
│── /images             # Folder for images
│── /fonts              # Folder for custom fonts
│── /api                # Folder for API-related scripts
│── /utils              # Folder for utility functions
```

#### **Example Code:**

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Web Application</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <!-- Navbar Component -->
  <nav id="navbar"></nav>

  <main>
    <section id="dashboard">
      <h2>Dashboard</h2>
      <p>Welcome to the dashboard.</p>
    </section>
  </main>

  <!-- Footer Component -->
  <footer id="footer"></footer>

  <script src="scripts/app.js"></script>
</body>
</html>
```

**styles.css**

```css
/* Global Styles */
@import url('styles/global.css');
@import url('styles/navbar.css');
@import url('styles/footer.css');
```

**scripts/app.js**

```javascript
// Load reusable components
document.addEventListener('DOMContentLoaded', () => {
  // Load navbar
  fetch('components/navbar.html')
    .then(response => response.text())
    .then(data => {
      document.getElementById('navbar').innerHTML = data;
    });

  // Load footer
  fetch('components/footer.html')
    .then(response => response.text())
    .then(data => {
      document.getElementById('footer').innerHTML = data;
    });
});
```

**styles/global.css**

```css
/* Global Styles */
body {
  font-family: Arial, sans-serif;
  line-height: 1.6;
  margin: 0;
  padding: 0;
}
```

**styles/navbar.css**

```css
/* Navbar Styles */
nav {
  background: #333;
  color: #fff;
  padding: 1rem;
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
```

**api/fetchData.js**

```javascript
// Example API utility function
export async function fetchData(url) {
  const response = await fetch(url);
  const data = await response.json();
  return data;
}
```

#### **Explanation:**

- **Modular CSS**: CSS is split into modular files (`global.css`, `navbar.css`, `footer.css`) for better organization and maintainability.
- **Modular JavaScript**: JavaScript is split into modular files (`app.js`, `fetchData.js`) for better organization and reusability.
- **API Integration**: The `api` folder contains scripts for interacting with APIs, such as fetching data from a backend server.
- **Utility Functions**: The `utils` folder contains reusable utility functions, such as form validation or date formatting.

This structure is suitable for large web applications with multiple features and complex functionality.

---

## Code Organization Guidelines

To maintain clarity, correctness, and maintainability in web projects:

### **I. File & Module Organization**

- **Separate concerns**: Use separate files for HTML, CSS, and JavaScript. Keep styles and scripts modular.
- **Reusable components**: Create reusable components (e.g., headers, footers) and load them dynamically.
- **Organize by feature**: Group related files (e.g., styles, scripts, images) by feature or page.

### **II. Functions & Control Flow**

- **Keep functions short**: Each function should perform a single task.
- **Avoid deep nesting**: Use early returns or guard clauses to simplify control flow.
- **Use meaningful names**: Function names should clearly describe their purpose (e.g., `fetchData()`, `validateForm()`).

### **III. Naming & Conventions**

- **Follow web conventions**: Use kebab-case for file names (e.g., `header.html`) and camelCase for JavaScript functions.
- **Use descriptive names**: Variable and function names should clearly convey their purpose.
- **Avoid inline styles**: Use external CSS files instead of inline styles for better maintainability.

### **IV. Tools & Best Practices**

- **Use version control**: Track changes with Git to collaborate and manage code history.
- **Write meaningful comments**: Explain the "why" behind your code, especially for complex logic.
- **Test thoroughly**: Test your website in multiple browsers and devices to ensure compatibility.
- **Optimize performance**: Minify CSS and JavaScript files, and optimize images for faster loading.

---

## Conclusion

By following this guide, web projects will be structured in a way that ensures clarity, correctness, and maintainability. Whether you're building a simple static website or a complex web application, these principles will help you create high-quality, maintainable code. Web development is a dynamic field, and with proper organization, you can ensure that your codebase remains easy to understand, debug, and extend.

Happy coding!
