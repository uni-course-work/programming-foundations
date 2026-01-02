# Chapter 16: Decomposition with Functions

## Introduction: Programs That Are Too Large to Manage

Look back at the programs you wrote in Chapter 15. The Grade Management System was over 100 lines. The Banking System had 80 lines. The Pattern Generator had nested loops everywhere.

Here's the problem: as programs grow, they become **harder to understand, harder to test, and harder to modify**.

Consider the banking program:
- The menu code is mixed with deposit logic, withdrawal logic, and balance checking
- If you want to reuse the deposit logic somewhere else, you can't—it's trapped inside main
- If there's a bug in the validation logic, you have to find it among 80 lines
- If you want to test whether withdrawal correctly handles "insufficient funds," you can't isolate that logic

This is where **functions** come in.

A **function** is a named block of code that solves a specific problem. Instead of writing all your code in `main()`, you break the program into smaller functions, each solving one piece of the puzzle. Each function:
- **Solves one problem** (calculating average, getting valid age, displaying menu)
- **Can be tested independently** (prove it works correctly in isolation)
- **Can be reused** (called from anywhere in the program)
- **Is easier to understand** (focused, named purpose)
- **Makes debugging easier** (isolate the problem to one function)

In this chapter, we'll explore:
- **Functions as a response to complexity** (why they matter)
- **Enabling reuse, testing, and reasoning** (benefits they provide)
- **Parameters and return values** (how functions communicate)
- **Local scope** (data encapsulation)
- **Pass-by-value vs. pass-by-reference** (control over data)
- **Overloading** (same name, different versions)

By the end of this chapter, you'll understand how to decompose complex programs into smaller, manageable, reusable functions.

---

## Part 1: Functions as a Response to Complexity

### What is a Function?

A **function** is a named block of code that performs a specific task. It can:
- **Accept input** (through parameters)
- **Process that input**
- **Return output** (through return value)
- **Be called** from anywhere in the program

**Function structure:**
```cpp
returnType functionName(parameterList) {
    // Function body: code that does the work
    return value;  // Return a value (if returnType is not void)
}
```

**Simple example:**
```cpp
int add(int a, int b) {
    return a + b;
}
```

This function:
- Takes two integers as input (parameters `a` and `b`)
- Adds them
- Returns the result

### The Problem Functions Solve

Without functions, you write everything in `main()`:

```cpp
// Without functions (all in main)
int main() {
    // Get age
    int age;
    std::cout << "Enter age (0-150): ";
    std::cin >> age;
    while (age < 0 || age > 150) {
        std::cout << "Invalid. Enter 0-150: ";
        std::cin >> age;
    }
    
    // Get height
    int height;
    std::cout << "Enter height (0-300): ";
    std::cin >> height;
    while (height < 0 || height > 300) {
        std::cout << "Invalid. Enter 0-300: ";
        std::cin >> height;
    }
    
    // Get weight
    int weight;
    std::cout << "Enter weight (0-500): ";
    std::cin >> weight;
    while (weight < 0 || weight > 500) {
        std::cout << "Invalid. Enter 0-500: ";
        std::cin >> weight;
    }
    
    // ... 50 more lines
}
```

**Problems:**
- Validation code is repeated three times (copy-paste nightmare)
- Hard to find bugs (buried in 100+ lines)
- Can't reuse validation (trapped in main)
- Main is huge and unfocused

**With functions:**
```cpp
int getValidAge() {
    int age;
    std::cout << "Enter age (0-150): ";
    std::cin >> age;
    while (age < 0 || age > 150) {
        std::cout << "Invalid. Enter 0-150: ";
        std::cin >> age;
    }
    return age;
}

int getValidHeight() {
    int height;
    std::cout << "Enter height (0-300): ";
    std::cin >> height;
    while (height < 0 || height > 300) {
        std::cout << "Invalid. Enter 0-300: ";
        std::cin >> height;
    }
    return height;
}

int getValidWeight() {
    int weight;
    std::cout << "Enter weight (0-500): ";
    std::cin >> weight;
    while (weight < 0 || weight > 500) {
        std::cout << "Invalid. Enter 0-500: ";
        std::cin >> weight;
    }
    return weight;
}

int main() {
    int age = getValidAge();
    int height = getValidHeight();
    int weight = getValidWeight();
    
    // ... rest of program
}
```

**Benefits:**
- Each function has one clear purpose
- Validation code is written once, reused three times
- Main is short and readable
- Easy to find and fix bugs
- Functions can be tested independently

---

## Part 2: Benefits of Functions

### 1. Reusability

Write code once, use it many times:

```cpp
double calculateAverage(double a, double b, double c) {
    return (a + b + c) / 3.0;
}

int main() {
    // Use in first part of program
    double examAverage = calculateAverage(85, 90, 88);
    
    // Use in different part of program
    double assignmentAverage = calculateAverage(92, 87, 95);
    
    // Use again elsewhere
    double projectAverage = calculateAverage(88, 91, 85);
}
```

Without functions, you'd write the same calculation three times.

### 2. Testability

Test functions in isolation:

```cpp
// Function to test
int findLargest(int a, int b, int c) {
    if (a > b && a > c) return a;
    if (b > a && b > c) return b;
    return c;
}

// Test cases (mental or in testing framework)
findLargest(5, 3, 8);      // Should return 8
findLargest(10, 10, 10);   // Should return 10
findLargest(1, 2, 3);      // Should return 3
```

In main, you can call the function with test cases and verify results.

### 3. Reasoning (Understanding)

Functions are easier to reason about because they're **focused**:

```cpp
// Name says what it does
int countEvenNumbers(int a, int b, int c, int d, int e) {
    int count = 0;
    if (a % 2 == 0) count++;
    if (b % 2 == 0) count++;
    if (c % 2 == 0) count++;
    if (d % 2 == 0) count++;
    if (e % 2 == 0) count++;
    return count;
}
```

When you see `countEvenNumbers()`, you immediately know what it does—you don't need to read the code inside.

### 4. Maintainability

Change the implementation once, affects everywhere:

```cpp
// Old version
double convertFahrenheitToCelsius(double f) {
    return (f - 32) * 5 / 9;
}

// If you realize there's a bug, fix it once
double convertFahrenheitToCelsius(double f) {
    return (f - 32) * 5.0 / 9.0;  // Use 5.0 and 9.0 for floating-point
}

// Now all calls use the fixed version
```

If the conversion logic was copy-pasted 10 times, you'd have to fix 10 places.

---

## Part 3: Function Syntax and Declaration

### Function Declaration (Prototype)

Before using a function, you must **declare** it:

```cpp
returnType functionName(parameterList);
```

**Example:**
```cpp
int add(int a, int b);
double calculateAverage(double x, double y, double z);
void displayMenu();
bool isEven(int n);
```

The semicolon at the end makes it a **declaration** (promise that the function exists).

### Function Definition

The actual implementation:

```cpp
returnType functionName(parameterList) {
    // Function body
    return value;
}
```

**Example:**
```cpp
int add(int a, int b) {
    return a + b;
}

double calculateAverage(double x, double y, double z) {
    return (x + y + z) / 3.0;
}

void displayMenu() {
    std::cout << "1. Add" << std::endl;
    std::cout << "2. Subtract" << std::endl;
    std::cout << "3. Exit" << std::endl;
}

bool isEven(int n) {
    return (n % 2 == 0);
}
```

### Program Structure

Typical structure:

```cpp
#include <iostream>
using namespace std;

// Forward declarations (prototypes)
int add(int a, int b);
double calculateAverage(double x, double y, double z);
void displayMenu();

// Main function
int main() {
    int result = add(5, 3);
    std::cout << "5 + 3 = " << result << std::endl;
    
    double avg = calculateAverage(85, 90, 88);
    std::cout << "Average: " << avg << std::endl;
    
    displayMenu();
    
    return 0;
}

// Function definitions
int add(int a, int b) {
    return a + b;
}

double calculateAverage(double x, double y, double z) {
    return (x + y + z) / 3.0;
}

void displayMenu() {
    std::cout << "1. Add" << std::endl;
    std::cout << "2. Subtract" << std::endl;
    std::cout << "3. Exit" << std::endl;
}
```

**Order matters:**
1. Forward declarations (at top, after includes)
2. Main function
3. Function definitions (after main)

---

## Part 4: Parameters and Return Values

### Return Values

A function returns **one value** (or void for no value):

```cpp
// Returns int
int square(int n) {
    return n * n;
}

// Returns double
double divide(double a, double b) {
    return a / b;
}

// Returns bool
bool isPositive(int n) {
    return (n > 0);
}

// Returns nothing (void)
void printMessage() {
    std::cout << "Hello, World!" << std::endl;
    // No return needed (or return; with no value)
}
```

**Using return values:**
```cpp
int result = square(5);           // result = 25
double quotient = divide(10, 3);  // quotient = 3.333...
bool positive = isPositive(-5);   // positive = false
printMessage();                   // Just executes, returns nothing
```

### Parameters

Functions accept **zero or more parameters**:

```cpp
// No parameters
void sayHello() {
    std::cout << "Hello!" << std::endl;
}

// One parameter
int square(int n) {
    return n * n;
}

// Two parameters
int add(int a, int b) {
    return a + b;
}

// Three parameters
double average(double x, double y, double z) {
    return (x + y + z) / 3.0;
}

// Many parameters
void reportGrade(std::string name, int midterm, int final, int assignments) {
    double grade = (midterm * 0.3) + (final * 0.5) + (assignments * 0.2);
    std::cout << name << ": " << grade << std::endl;
}
```

### Parameter vs. Argument

- **Parameter:** Variable in the function definition
- **Argument:** Value passed when calling the function

```cpp
int add(int a, int b) {  // a and b are PARAMETERS
    return a + b;
}

int result = add(5, 3);  // 5 and 3 are ARGUMENTS
```

---

## Part 5: Local Scope

### What is Scope?

**Scope** is the region of code where a variable exists and can be used.

Variables declared inside a function only exist inside that function:

```cpp
int addAndSquare(int a, int b) {
    int sum = a + b;      // sum exists only in this function
    return sum * sum;
}

int main() {
    int result = addAndSquare(3, 4);
    std::cout << result << std::endl;
    
    // std::cout << sum << std::endl;  // ERROR: sum doesn't exist here
}
```

### Local Variables

Variables declared in a function are **local** to that function:

```cpp
int calculateScore(int exam, int homework) {
    int total = exam + homework;  // local variable
    int percentage = (total / 2);  // local variable
    return percentage;
}

int main() {
    int myScore = calculateScore(85, 90);
    // std::cout << total << std::endl;       // ERROR
    // std::cout << percentage << std::endl;  // ERROR
    std::cout << myScore << std::endl;        // OK
}
```

### Parameters Are Local

Function parameters are also local:

```cpp
void processNumber(int n) {
    // n is a local variable (happens to be a parameter)
    std::cout << "Number is: " << n << std::endl;
}

int main() {
    int value = 42;
    processNumber(value);
    // std::cout << n << std::endl;  // ERROR: n doesn't exist here
}
```

### Benefits of Local Scope

1. **Encapsulation:** Variables are hidden inside functions
2. **No name conflicts:** Two functions can both have a variable named `count`
3. **Memory efficiency:** Local variables are created and destroyed as needed
4. **Clarity:** You know variables are used only within their function

---

## Part 6: Pass-by-Value vs. Pass-by-Reference

### Pass-by-Value (Default)

When you pass a variable by **value**, the function receives a **copy** of the value:

```cpp
void increment(int n) {
    n++;  // Increments the COPY, not the original
}

int main() {
    int x = 5;
    increment(x);
    std::cout << x << std::endl;  // Prints 5 (not 6!)
}
```

The function receives a copy, so changing the parameter doesn't affect the original variable.

### Pass-by-Reference

When you pass a variable by **reference**, the function receives a reference to the **actual variable**:

```cpp
void increment(int &n) {  // & means "pass by reference"
    n++;  // Increments the ACTUAL variable
}

int main() {
    int x = 5;
    increment(x);
    std::cout << x << std::endl;  // Prints 6
}
```

The `&` after the type means "pass by reference." Now the function modifies the original variable.

### When to Use Each

**Pass-by-value (default):**
- When you don't want the function to modify the original
- When passing small values (int, double, char)
- When you want isolation (function can't affect caller)

```cpp
int square(int n) {
    n = n * n;  // We don't care about modifying the parameter
    return n;
}
```

**Pass-by-reference:**
- When you want the function to modify the original
- For input validation (get a value, validate, store back)
- For efficiency with large objects (strings, large data)

```cpp
void getValidAge(int &age) {
    std::cout << "Enter age: ";
    std::cin >> age;
    while (age < 0 || age > 150) {
        std::cout << "Invalid. Enter 0-150: ";
        std::cin >> age;
    }
}

int main() {
    int myAge;
    getValidAge(myAge);  // myAge is modified inside function
    std::cout << "You are " << myAge << std::endl;
}
```

### Comparison

| Pass-by-Value | Pass-by-Reference |
|---|---|
| Receives a **copy** | Receives reference to **actual variable** |
| Changes don't affect original | Changes **do** affect original |
| Safer (can't accidentally modify) | More efficient (no copy) |
| Use for small values | Use for large data or when modification needed |

---

## Part 7: Function Overloading

### What is Overloading?

**Overloading** means having multiple functions with the **same name but different parameters**.

C++ distinguishes them by the **parameter list** (number and type of parameters).

```cpp
// Version 1: add two integers
int add(int a, int b) {
    return a + b;
}

// Version 2: add two doubles
double add(double a, double b) {
    return a + b;
}

// Version 3: add three integers
int add(int a, int b, int c) {
    return a + b + c;
}

int main() {
    std::cout << add(5, 3) << std::endl;              // Uses int version, prints 8
    std::cout << add(5.5, 3.2) << std::endl;          // Uses double version, prints 8.7
    std::cout << add(5, 3, 2) << std::endl;           // Uses three-param version, prints 10
}
```

C++ chooses the right version based on the **types and number of arguments**.

### Why Overloading Matters

Instead of different function names:
```cpp
int addInts(int a, int b) { return a + b; }
double addDoubles(double a, double b) { return a + b; }
int addThreeInts(int a, int b, int c) { return a + b + c; }

// Hard to remember which name to use
addInts(5, 3);
addDoubles(5.5, 3.2);
addThreeInts(5, 3, 2);
```

With overloading:
```cpp
int add(int a, int b) { return a + b; }
double add(double a, double b) { return a + b; }
int add(int a, int b, int c) { return a + b + c; }

// One name, C++ picks the right version
add(5, 3);
add(5.5, 3.2);
add(5, 3, 2);
```

### Function Signature

A **function signature** includes the name, parameter types, and parameter count. The **return type is NOT part of the signature**.

```cpp
// Legal overloads (different signatures)
int getValue();
double getValue(int x);
bool getValue(int x, int y);

// ILLEGAL (same signature, different return)
int getValue();
double getValue();  // ERROR: same parameters, different return type
```

---

## Part 8: Practical Functions from Previous Programs

### Input Validation Function

From Chapter 15, you wrote validation code repeatedly:

```cpp
int getValidAge() {
    int age;
    std::cout << "Enter age (0-150): ";
    std::cin >> age;
    while (age < 0 || age > 150) {
        std::cout << "Error: Enter 0-150: ";
        std::cin >> age;
    }
    return age;
}

int getValidGrade() {
    int grade;
    std::cout << "Enter grade (0-100): ";
    std::cin >> grade;
    while (grade < 0 || grade > 100) {
        std::cout << "Error: Enter 0-100: ";
        std::cin >> grade;
    }
    return grade;
}

int main() {
    int age = getValidAge();
    int grade = getValidGrade();
    std::cout << "Age: " << age << ", Grade: " << grade << std::endl;
}
```

### Menu Display Function

From banking or calculator programs:

```cpp
void displayMenu() {
    std::cout << std::endl;
    std::cout << "=== MENU ===" << std::endl;
    std::cout << "1. Add" << std::endl;
    std::cout << "2. Subtract" << std::endl;
    std::cout << "3. Multiply" << std::endl;
    std::cout << "4. Divide" << std::endl;
    std::cout << "5. Exit" << std::endl;
    std::cout << "Choice: ";
}

int main() {
    int choice;
    do {
        displayMenu();
        std::cin >> choice;
        
        if (choice == 1) {
            // Handle addition
        }
        // ... etc
    } while (choice != 5);
}
```

### Calculation Functions

```cpp
double calculateAverage(double x, double y, double z) {
    return (x + y + z) / 3.0;
}

int findLargest(int a, int b, int c) {
    if (a > b && a > c) return a;
    if (b > a && b > c) return b;
    return c;
}

int findSmallest(int a, int b, int c) {
    if (a < b && a < c) return a;
    if (b < a && b < c) return b;
    return c;
}

int main() {
    double avg = calculateAverage(85, 90, 88);
    int max = findLargest(5, 3, 8);
    int min = findSmallest(5, 3, 8);
    
    std::cout << "Average: " << avg << std::endl;
    std::cout << "Max: " << max << ", Min: " << min << std::endl;
}
```

---

## Part 9: A Complete Refactored Program

### Before (Everything in main, Chapter 15 style)

```cpp
#include <iostream>
int main() {
    double balance = 1000.0;
    int choice;
    double amount;
    
    do {
        std::cout << "\n=== BANKING ===" << std::endl;
        std::cout << "1. Check Balance" << std::endl;
        std::cout << "2. Deposit" << std::endl;
        std::cout << "3. Withdraw" << std::endl;
        std::cout << "4. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            std::cout << "Balance: $" << balance << std::endl;
        } else if (choice == 2) {
            std::cout << "Amount: ";
            std::cin >> amount;
            if (amount <= 0) {
                std::cout << "Invalid." << std::endl;
            } else {
                balance += amount;
                std::cout << "New balance: $" << balance << std::endl;
            }
        } else if (choice == 3) {
            std::cout << "Amount: ";
            std::cin >> amount;
            if (amount <= 0) {
                std::cout << "Invalid." << std::endl;
            } else if (amount > balance) {
                std::cout << "Insufficient funds." << std::endl;
            } else {
                balance -= amount;
                std::cout << "New balance: $" << balance << std::endl;
            }
        }
    } while (choice != 4);
    
    return 0;
}
```

### After (With functions, much clearer)

```cpp
#include <iostream>

// Function declarations
void displayMenu();
double deposit(double balance, double amount);
double withdraw(double balance, double amount);
void displayBalance(double balance);

int main() {
    double balance = 1000.0;
    int choice;
    double amount;
    
    do {
        displayMenu();
        std::cin >> choice;
        
        if (choice == 1) {
            displayBalance(balance);
        } else if (choice == 2) {
            std::cout << "Amount: ";
            std::cin >> amount;
            balance = deposit(balance, amount);
        } else if (choice == 3) {
            std::cout << "Amount: ";
            std::cin >> amount;
            balance = withdraw(balance, amount);
        }
    } while (choice != 4);
    
    return 0;
}

// Function definitions
void displayMenu() {
    std::cout << "\n=== BANKING ===" << std::endl;
    std::cout << "1. Check Balance" << std::endl;
    std::cout << "2. Deposit" << std::endl;
    std::cout << "3. Withdraw" << std::endl;
    std::cout << "4. Exit" << std::endl;
    std::cout << "Choice: ";
}

void displayBalance(double balance) {
    std::cout << "Balance: $" << balance << std::endl;
}

double deposit(double balance, double amount) {
    if (amount <= 0) {
        std::cout << "Error: Amount must be positive." << std::endl;
        return balance;
    }
    balance += amount;
    std::cout << "New balance: $" << balance << std::endl;
    return balance;
}

double withdraw(double balance, double amount) {
    if (amount <= 0) {
        std::cout << "Error: Amount must be positive." << std::endl;
        return balance;
    }
    if (amount > balance) {
        std::cout << "Error: Insufficient funds." << std::endl;
        return balance;
    }
    balance -= amount;
    std::cout << "New balance: $" << balance << std::endl;
    return balance;
}
```

**Improvements:**
- Main is clear and readable (shows overall structure)
- Each function has one responsibility
- Functions are reusable and testable
- Changes to withdrawal logic only need to happen in one place
- Much easier to add new features

---

## Part 10: Common Function Patterns

### Pattern 1: Input Validation

```cpp
int getValidInput(int min, int max) {
    int value;
    std::cout << "Enter value (" << min << "-" << max << "): ";
    std::cin >> value;
    
    while (value < min || value > max) {
        std::cout << "Invalid. Enter " << min << "-" << max << ": ";
        std::cin >> value;
    }
    
    return value;
}

// Usage
int age = getValidInput(0, 150);
int grade = getValidInput(0, 100);
```

### Pattern 2: Menu and Selection

```cpp
int getMenuChoice(int maxChoice) {
    int choice;
    std::cout << "Choice: ";
    std::cin >> choice;
    
    while (choice < 1 || choice > maxChoice) {
        std::cout << "Invalid choice. Enter 1-" << maxChoice << ": ";
        std::cin >> choice;
    }
    
    return choice;
}
```

### Pattern 3: Finding Maximum/Minimum

```cpp
int findMax(int a, int b, int c) {
    int max = a;
    if (b > max) max = b;
    if (c > max) max = c;
    return max;
}

int findMin(int a, int b, int c) {
    int min = a;
    if (b < min) min = b;
    if (c < min) min = c;
    return min;
}
```

### Pattern 4: Calculations

```cpp
double calculateTax(double income, double taxRate) {
    return income * taxRate;
}

double calculateDiscount(double price, double discountPercent) {
    return price * (1.0 - discountPercent / 100.0);
}
```

### Pattern 5: Displaying Results

```cpp
void displayResult(std::string label, double value) {
    std::cout << label << ": " << value << std::endl;
}

void displayGrade(std::string name, int score) {
    char grade;
    if (score >= 90) grade = 'A';
    else if (score >= 80) grade = 'B';
    else if (score >= 70) grade = 'C';
    else grade = 'F';
    
    std::cout << name << ": " << grade << std::endl;
}
```

---

## Part 11: Function Design Principles

### Principle 1: One Function, One Job

Each function should do **one thing well**:

```cpp
// BAD: does multiple things
void processStudent(std::string name, int score) {
    // Validate name
    if (name.length() == 0) {
        std::cout << "Invalid name" << std::endl;
        return;
    }
    // Validate score
    if (score < 0 || score > 100) {
        std::cout << "Invalid score" << std::endl;
        return;
    }
    // Calculate grade
    char grade;
    if (score >= 90) grade = 'A';
    else grade = 'F';
    // Display
    std::cout << name << ": " << grade << std::endl;
}

// GOOD: separate functions
bool isValidName(std::string name) {
    return (name.length() > 0);
}

bool isValidScore(int score) {
    return (score >= 0 && score <= 100);
}

char calculateGrade(int score) {
    return (score >= 90) ? 'A' : 'F';
}

void displayGrade(std::string name, char grade) {
    std::cout << name << ": " << grade << std::endl;
}
```

### Principle 2: Meaningful Names

Function names should describe what they do:

```cpp
// Unclear
void fn1(int x) { ... }
int calc(int a, int b) { ... }

// Clear
void displayMenu() { ... }
int getValidAge() { ... }
double calculateAverage(double a, double b) { ... }
bool isEven(int n) { ... }
```

### Principle 3: Reasonable Function Size

Functions should be readable on one screen (typically 10-30 lines):

```cpp
// Too long (hard to understand)
void handleEverything() {
    // 200 lines of code
}

// Good size (clear purpose, readable)
int getValidAge() {
    int age;
    std::cout << "Enter age: ";
    std::cin >> age;
    while (age < 0 || age > 150) {
        std::cout << "Invalid. Enter 0-150: ";
        std::cin >> age;
    }
    return age;
}
```

---

## Part 12: Thinking in Functions

### Decomposition Strategy

When you have a complex problem:

1. **Identify tasks** (what needs to be done)
2. **Create functions** for each task
3. **Combine functions** in main

**Example: Grade Calculator**

```
Tasks:
- Get number of students (input validation)
- For each student: get name and grade (input with validation)
- Calculate average
- Count grade distribution
- Find highest and lowest
- Display report

Functions needed:
- getValidCount()
- getStudentName()
- getValidGrade()
- calculateAverage()
- countGrades()
- displayReport()

Main orchestrates:
int numStudents = getValidCount();
for each student: ...
average = calculateAverage();
displayReport();
```

### Top-Down Design

Start with the big picture, break into pieces:

```cpp
int main() {
    // High-level overview
    getInput();
    processData();
    displayResults();
}

void getInput() {
    // Details of getting input
}

void processData() {
    // Details of processing
}

void displayResults() {
    // Details of displaying
}
```

---

## Summary: Functions Enable Complexity Management

Functions are your tool for managing growing program complexity. As programs grow from 20 lines to 100 to 1000 lines, functions become essential for:

- **Organizing** code into manageable pieces
- **Reusing** solutions to common problems
- **Testing** individual components
- **Understanding** what code does
- **Maintaining** and modifying code

Without functions, programs become unmaintainable. With functions, you can build programs of any size.

---

## Looking Ahead to Chapter 17

Now that you understand functions, the next chapter introduces **arrays**—how to store and process collections of data. Arrays combine with functions powerfully:

```cpp
// Function that processes an array
double calculateAverage(int numbers[], int count) {
    int sum = 0;
    for (int i = 0; i < count; i++) {
        sum += numbers[i];
    }
    return sum / (double)count;
}
```

Functions + Arrays = powerful data processing capability.

---

## Checklist: Before Writing Functions

Before writing a function, ask yourself:

- [ ] What is the function's single responsibility?
- [ ] What input does it need (parameters)?
- [ ] What should it return?
- [ ] Is the name clear and descriptive?
- [ ] Can I test this function independently?
- [ ] Will I use this function more than once?
- [ ] Is the function a reasonable size (not huge)?
- [ ] Should parameters be pass-by-value or pass-by-reference?

If you can answer these confidently, your function is well-designed.

---

## Key Terminology

| Term | Definition |
|------|-----------|
| **Function** | Named block of code with parameters and return value |
| **Parameter** | Variable in function definition |
| **Argument** | Value passed when calling function |
| **Return value** | Value function sends back to caller |
| **Scope** | Region where variable exists |
| **Local variable** | Variable declared inside function |
| **Pass-by-value** | Function receives copy of parameter |
| **Pass-by-reference** | Function receives reference to actual variable |
| **Overloading** | Multiple functions with same name, different parameters |
| **Prototype** | Function declaration (forward declaration) |
| **Definition** | Function implementation |
