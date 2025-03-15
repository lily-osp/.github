# Lily's Arduino (C/C++) Development Guide

A structured approach to organizing Arduino projects, inspired by "Lily's Program Writing Sets" to ensure clarity, correctness, and maintainability. This guide focuses on developing Arduino libraries and code, which are typically written in C/C++.

---

## Introduction

This guide provides a systematic way to structure Arduino projects, whether you're writing a simple sketch or developing a reusable library. Arduino projects often involve hardware interaction, so clarity and maintainability are crucial for debugging and extending functionality.

## Guiding Principles

Following the core principles of "Lily's Program Writing Sets":

1. **Clarity** – Code should be easy to read, understand, and modify. Arduino code often interacts with hardware, so clear and well-documented code is essential.
2. **Correctness** – Code should function as intended and handle errors gracefully. Hardware-related code must be robust and account for unexpected behavior.
3. **Maintainability** – Code should be structured for long-term usability and modification. Modular design and reusable libraries are key.

---

## Arduino Project Structure

### **1. Basic Project (Simple Sketch)**

For simple Arduino sketches that perform a single task, such as blinking an LED or reading a sensor.

### **📂 Project Structure**

```
/simple_sketch
│── simple_sketch.ino       # Main Arduino sketch file
```

#### **Example Code:**

**simple_sketch.ino**

```cpp
// Pin where the LED is connected
const int ledPin = 13;

void setup() {
  // Initialize the LED pin as an output
  pinMode(ledPin, OUTPUT);
}

void loop() {
  // Blink the LED
  digitalWrite(ledPin, HIGH);  // Turn the LED on
  delay(1000);                 // Wait for 1 second
  digitalWrite(ledPin, LOW);   // Turn the LED off
  delay(1000);                 // Wait for 1 second
}
```

#### **Explanation:**

- **`setup()`**: This function runs once when the Arduino starts. It initializes the LED pin as an output.
- **`loop()`**: This function runs repeatedly. It turns the LED on and off with a 1-second delay.
- **`pinMode()`**: Configures the specified pin to behave either as an input or an output.
- **`digitalWrite()`**: Sets the pin to either HIGH (5V) or LOW (0V).

This is the simplest structure for an Arduino project, suitable for small tasks.

---

### **2. Decent Project (Modular Sketch)**

For more complex sketches that involve multiple components or tasks, such as controlling multiple LEDs or reading multiple sensors.

### **📂 Project Structure**

```
/decent_sketch
│── decent_sketch.ino       # Main Arduino sketch file
│── led_controller.h        # Header file for LED control
│── led_controller.cpp      # Implementation file for LED control
```

#### **Example Code:**

**decent_sketch.ino**

```cpp
#include "led_controller.h"

// Pins where the LEDs are connected
const int ledPin1 = 13;
const int ledPin2 = 12;

// Create an instance of the LED controller
LEDController led1(ledPin1);
LEDController led2(ledPin2);

void setup() {
  // Initialize the LEDs
  led1.begin();
  led2.begin();
}

void loop() {
  // Blink the LEDs alternately
  led1.turnOn();
  led2.turnOff();
  delay(500);

  led1.turnOff();
  led2.turnOn();
  delay(500);
}
```

**led_controller.h**

```cpp
#ifndef LED_CONTROLLER_H
#define LED_CONTROLLER_H

class LEDController {
  private:
    int ledPin;  // Pin where the LED is connected

  public:
    LEDController(int pin);  // Constructor
    void begin();            // Initialize the LED
    void turnOn();           // Turn the LED on
    void turnOff();          // Turn the LED off
};

#endif
```

**led_controller.cpp**

```cpp
#include "led_controller.h"
#include <Arduino.h>

// Constructor
LEDController::LEDController(int pin) {
  ledPin = pin;
}

// Initialize the LED
void LEDController::begin() {
  pinMode(ledPin, OUTPUT);
}

// Turn the LED on
void LEDController::turnOn() {
  digitalWrite(ledPin, HIGH);
}

// Turn the LED off
void LEDController::turnOff() {
  digitalWrite(ledPin, LOW);
}
```

#### **Explanation:**

- **Modularization**: The LED control logic is moved to a separate class (`LEDController`), making the main sketch cleaner and easier to understand.
- **`led_controller.h`**: The header file declares the `LEDController` class and its methods.
- **`led_controller.cpp`**: The implementation file defines the methods for controlling the LED.
- **Reusability**: The `LEDController` class can be reused in other projects or extended to control more complex LED behaviors.

---

### **3. Larger Project (Library Development)**

For developing reusable Arduino libraries that can be shared across multiple projects or with the community.

### **📂 Project Structure**

```
/arduino_library
│── /examples
│   ├── example_sketch.ino  # Example sketch to demonstrate library usage
│
│── /src
│   ├── MyLibrary.h          # Header file for the library
│   ├── MyLibrary.cpp        # Implementation file for the library
│
│── library.properties       # Metadata for the Arduino Library Manager
│── README.md                # Documentation for the library
│── keywords.txt            # Syntax highlighting for the Arduino IDE
```

#### **Example Code:**

**MyLibrary.h**

```cpp
#ifndef MY_LIBRARY_H
#define MY_LIBRARY_H

#include <Arduino.h>

class MyLibrary {
  private:
    int pin;  // Pin connected to the hardware

  public:
    MyLibrary(int pin);  // Constructor
    void begin();        // Initialize the hardware
    void doSomething();  // Perform an action
};

#endif
```

**MyLibrary.cpp**

```cpp
#include "MyLibrary.h"

// Constructor
MyLibrary::MyLibrary(int pin) {
  this->pin = pin;
}

// Initialize the hardware
void MyLibrary::begin() {
  pinMode(pin, OUTPUT);
}

// Perform an action
void MyLibrary::doSomething() {
  digitalWrite(pin, HIGH);
  delay(500);
  digitalWrite(pin, LOW);
  delay(500);
}
```

**example_sketch.ino**

```cpp
#include <MyLibrary.h>

// Pin where the hardware is connected
const int myPin = 13;

// Create an instance of the library
MyLibrary myLibrary(myPin);

void setup() {
  // Initialize the library
  myLibrary.begin();
}

void loop() {
  // Perform an action
  myLibrary.doSomething();
}
```

**library.properties**

```
name=MyLibrary
version=1.0.0
author=Lily
maintainer=Lily <lily@example.com>
sentence=A simple Arduino library for demonstration purposes.
paragraph=This library provides basic functionality for controlling hardware.
category=Signal Input/Output
url=https://github.com/lily/my-library
architectures=*
```

**keywords.txt**

```plaintext
MyLibrary KEYWORD1
begin KEYWORD2
doSomething KEYWORD2
```

#### **Explanation:**

- **Library Structure**: The library is split into a header file (`MyLibrary.h`) and an implementation file (`MyLibrary.cpp`).
- **Example Sketch**: The `examples` folder contains a sketch that demonstrates how to use the library.
- **`library.properties`**: This file provides metadata for the Arduino Library Manager, such as the library name, version, and author.
- **`keywords.txt`**: This file defines syntax highlighting for the Arduino IDE, making it easier to read the library code.

---

## Code Organization Guidelines

To maintain clarity, correctness, and maintainability in Arduino projects:

### **I. File & Module Organization**

- **Separate concerns**: Use separate files for different components (e.g., sensors, actuators).
- **Modularize code**: Encapsulate hardware control logic in classes or functions.
- **Use libraries**: Create reusable libraries for common tasks (e.g., controlling LEDs, reading sensors).

### **II. Functions & Control Flow**

- **Keep functions short**: Each function should perform a single task.
- **Avoid deep nesting**: Use early returns or guard clauses to simplify control flow.
- **Use meaningful names**: Function names should clearly describe their purpose (e.g., `turnOn()`, `readSensor()`).

### **III. Naming & Conventions**

- **Follow Arduino conventions**: Use camelCase for function names and ALL_CAPS for constants.
- **Use descriptive names**: Variable and function names should clearly convey their purpose.
- **Avoid magic numbers**: Use named constants instead of hardcoding values (e.g., `const int ledPin = 13;`).

### **IV. Tools & Best Practices**

- **Use version control**: Track changes with Git to collaborate and manage code history.
- **Write meaningful comments**: Explain the "why" behind your code, especially for hardware-specific logic.
- **Test thoroughly**: Test your code with different hardware configurations to ensure robustness.
- **Document your library**: Provide clear documentation and examples for others to use your library.

---

## Conclusion

By following this guide, Arduino projects will be structured in a way that ensures clarity, correctness, and maintainability. Whether you're writing a simple sketch or developing a reusable library, these principles will help you create high-quality, maintainable code. Arduino's simplicity and versatility make it an excellent platform for hardware projects, and with proper organization, you can ensure that your codebase remains easy to understand, debug, and extend.

Happy coding!
