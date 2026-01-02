# Chapter 16: Problems

## Section A: Understanding Functions and Scope

### Problem 1: Tracing Function Execution

Consider this program:

```cpp
#include <iostream>

int add(int a, int b) {
    int sum = a + b;
    return sum;
}

int multiply(int a, int b) {
    int product = a * b;
    return product;
}

int main() {
    int x = 5;
    int y = 3;
    
    int result1 = add(x, y);
    int result2 = multiply(result1, 2);
    
    std::cout << "Result: " << result2 << std::endl;
    
    return 0;
}
```

1. Trace through the program step-by-step
2. What are the parameters and arguments in each function call?
3. What is the scope of variable `sum`? Where can it be used?
4. What is the scope of variable `x`? Can `add()` access it directly?
5. What is the final output?
6. Can you access `sum` from main? Why or why not?

---

### Problem 2: Pass-by-Value vs. Pass-by-Reference

Consider these three programs:

**Program A:**
```cpp
void increment(int n) {
    n++;
}

int main() {
    int x = 5;
    increment(x);
    std::cout << x << std::endl;
}
```

**Program B:**
```cpp
void increment(int &n) {
    n++;
}

int main() {
    int x = 5;
    increment(x);
    std::cout << x << std::endl;
}
```

**Program C:**
```cpp
int increment(int n) {
    return n + 1;
}

int main() {
    int x = 5;
    x = increment(x);
    std::cout << x << std::endl;
}
```

1. What does each program output?
2. Why do A and B produce different outputs?
3. When would you use pass-by-value vs. pass-by-reference?
4. When would you use a return value instead of pass-by-reference?
5. Which approach would you use for `getValidAge()` and why?

---

### Problem 3: Function Overloading

You want three versions of a `maximum()` function:
- Find maximum of two integers
- Find maximum of three integers
- Find maximum of two doubles

1. Write all three function declarations
2. Write all three function definitions
3. Write main() that calls all three versions
4. Explain how C++ knows which version to use

---

## Section B: Refactoring Previous Programs

### Problem 4: Refactor the Validation Code

In Chapter 15, you wrote validation code like this repeatedly:

```cpp
int age;
std::cout << "Enter age (0-150): ";
std::cin >> age;
while (age < 0 || age > 150) {
    std::cout << "Invalid. Enter 0-150: ";
    std::cin >> age;
}
```

**Task:**
1. Write a `getValidIntegerInRange()` function that:
   - Takes parameters: min value, max value, prompt text
   - Gets input from user
   - Validates it's in range
   - Returns the valid value
2. Write a complete program that uses this function to get age, grade, and number of students
3. Show how this eliminates code duplication

---

### Problem 5: Refactor Menu Code

In Chapter 15, every menu-driven program had this pattern:

```cpp
do {
    std::cout << "1. Option A" << std::endl;
    std::cout << "2. Option B" << std::endl;
    std::cout << "3. Exit" << std::endl;
    std::cout << "Choice: ";
    std::cin >> choice;
} while (choice != 3);
```

**Task:**
1. Write a `getMenuChoice()` function that:
   - Takes parameter: number of menu options
   - Displays a generic menu (1 through N)
   - Gets validated choice from user
   - Returns the choice
2. Write a complete calculator program using this function
3. Show how the main loop becomes much simpler

---

### Problem 6: Refactor the Banking Program

Take the banking program from Chapter 15 and refactor it with functions:

**Write functions for:**
- `displayMenu()` - shows the menu
- `displayBalance(double balance)` - shows current balance
- `deposit(double balance, double amount)` - processes deposit, returns new balance
- `withdraw(double balance, double amount)` - processes withdrawal, returns new balance

**Task:**
1. Write all four functions with validation
2. Rewrite main() to use these functions
3. Compare to the Chapter 15 version (is it clearer?)
4. Show how the program is now more maintainable

---

## Section C: Building Programs with Functions

### Problem 7: Create a Statistics Calculator

**Write a complete program with functions** that:

1. Gets N numbers from the user (N is validated 1-100)
2. Calculates and displays:
   - Sum
   - Average
   - Maximum value
   - Minimum value
   - Median (middle value when sorted)
   - Range (max - min)

**Required functions:**
- `getValidCount()` - get N (1-100)
- `readNumbers(int count)` - read N numbers and return... (we'll use arrays in Ch 17, for now just read and calculate)
- `calculateSum(double x1, double x2, ...)` - actually, redesign this
- Better: read numbers in main, pass to calculation functions
- `calculateAverage(double sum, int count)` - average
- `calculateRange(double max, double min)` - range
- `displayStatistics(double sum, double avg, double max, double min, double range)` - display results

**Example run:**
```
STATISTICS CALCULATOR
How many numbers? 5
Enter number 1: 10
Enter number 2: 25
Enter number 3: 5
Enter number 4: 30
Enter number 5: 15

STATISTICS
Sum: 85
Average: 17
Maximum: 30
Minimum: 5
Range: 25
```

Requirements:
- Use at least 4 functions
- Main is clear and readable
- Each function has one responsibility
- All inputs validated

---

### Problem 8: Create a Grade Reporting System with Functions

**Write a complete program with functions** that:

1. Gets student information: name, midterm, final, assignments (all with validation)
2. Calculates total grade (30% midterm + 50% final + 20% assignments)
3. Determines letter grade
4. Determines pass/fail status
5. Displays complete report

**Required functions:**
- `getValidScore(std::string label)` - get score (0-100)
- `getValidAssignments()` - get assignments (0-10)
- `calculateTotalGrade(int mid, int final, int assignments)` - weighted grade
- `getLetterGrade(double totalGrade)` - return char (A, B, C, D, F)
- `getPassStatus(double totalGrade)` - return bool (true if >= 60)
- `displayReport(std::string name, double grade, char letter, bool passed)` - display results

**Example run:**
```
GRADE REPORTING SYSTEM
Student name: Alice
Midterm score (0-100): 85
Final score (0-100): 90
Assignments completed (0-10): 9

STUDENT REPORT
Name: Alice
Midterm: 85 (30% weight)
Final: 90 (50% weight)
Assignments: 9/10 (20% weight)

Total Grade: 87.7
Letter Grade: B
Status: PASSED

Would you like to report another student? (y/n): n
```

Requirements:
- Main shows overall flow (get input, calculate, report)
- Functions handle specific tasks
- All inputs validated with specific ranges
- Display is clean and professional

---

### Problem 9: Create a Temperature Converter with Multiple Units

**Write a complete program with functions** that:

1. Shows conversion menu (Celsius ↔ Fahrenheit, Celsius ↔ Kelvin, etc.)
2. Gets temperature from user
3. Converts to all other units
4. Displays results
5. Repeats until user quits

**Required functions:**
- `celsiusToFahrenheit(double c)` - return double
- `celsiusToKelvin(double c)` - return double
- `fahrenheitToCelsius(double f)` - return double
- `fahrenheitToKelvin(double f)` - return double
- `kelvinToCelsius(double k)` - return double
- `kelvinToFahrenheit(double k)` - return double
- `displayMenu()` - show menu options
- `convertAndDisplay(double temp, int unit)` - do conversion and show all results

**Example run:**
```
TEMPERATURE CONVERTER
1. Celsius
2. Fahrenheit
3. Kelvin
4. Exit
Enter temperature unit: 1

Enter temperature in Celsius: 25

=== CONVERSIONS ===
Celsius: 25.0°C
Fahrenheit: 77.0°F
Kelvin: 298.15K

Convert again? (y/n): y

Enter temperature unit: 2
...
```

Requirements:
- 6 conversion functions (practice function design)
- Menu-driven with validation
- Overloading: have versions that convert between any two units
- Clear formatting

---

### Problem 10: Create a Multi-Function Calculator with History

**Write a complete program with functions** that:

1. Displays menu with operations:
   - Add, Subtract, Multiply, Divide
   - Power (x^y)
   - Square root
   - View History
   - Exit
2. For each operation, validates input and performs calculation
3. Maintains history of last 5 operations
4. Can display and clear history

**Required functions (minimum 8):**
- `add(double a, double b)` - return sum
- `subtract(double a, double b)` - return difference
- `multiply(double a, double b)` - return product
- `divide(double a, double b)` - return quotient (check for zero)
- `power(double base, double exp)` - return base^exp
- `squareRoot(double x)` - return sqrt (check for negative)
- `addToHistory(std::string operation)` - store operation
- `displayHistory()` - show last 5 operations
- `getValidNumber()` - get number from user
- `displayMenu()` - show menu options

**Example run:**
```
=== CALCULATOR ===
1. Add
2. Subtract
3. Multiply
4. Divide
5. Power
6. Square Root
7. View History
8. Clear History
9. Exit
Choice: 1

First number: 10
Second number: 5
Result: 15
Added to history.

Choice: 2
First number: 20
Second number: 3
Result: 17
Added to history.

Choice: 7

=== HISTORY ===
1. 10 + 5 = 15
2. 20 - 3 = 17

Choice: 9
Goodbye!
```

Requirements:
- Multiple functions for different operations
- Input validation
- History tracking (5 operations)
- Menu-driven design
- At least 8 functions

---

## Challenge Problems (Optional)

### Challenge 1: Refactor Chapter 15 Grade Management Program

Take the Grade Management System from Chapter 15 and refactor it with functions:

**Write functions for:**
- `getValidCount()` - get number of students (1-10)
- `getValidGrade()` - get individual grade (0-100)
- `addGrade(int gradeIndex, double grade)` - store grade (we'll use arrays in Ch 17)
- `calculateAverage(...)` - calculate class average
- `countByGrade(int &a, int &b, int &c, int &d, int &f)` - count grades in each category
- `findHighest(..., double &maxValue, int &maxIndex)` - find highest
- `findLowest(..., double &minValue, int &minIndex)` - find lowest
- `displayMenu()` - show menu
- `displayReport(...)` - display statistics

**Task:**
1. Refactor the program with 8+ functions
2. Show how much clearer it is than the Chapter 15 version
3. Each function should have one clear responsibility
4. Main should focus on the menu loop

---

### Challenge 2: Create a Scientific Calculator

**Write a complete program with functions** that supports:
- Basic operations (add, subtract, multiply, divide)
- Trigonometric (sin, cos, tan) - use `#include <cmath>`
- Exponential (e^x)
- Logarithm (log, ln)
- Factorial
- GCD and LCM
- Memory operations (M+, M-, MR, MC)

**Challenge aspects:**
- Multiple operation categories (8+ functions)
- Function overloading (multiple ways to do same operation)
- Complex menu with submenus
- Persistent memory across operations
- Error handling (domain errors, etc.)

**Requirements:**
- At least 10 functions
- Use function overloading where appropriate
- Professional menu system
- Memory storage between calculations

---

### Challenge 3: Create a Utility Library with Functions

**Write a complete program with functions** that provides:

**String utilities:**
- `reverseString(std::string s)` - return reversed
- `countVowels(std::string s)` - return count
- `toUppercase(std::string s)` - return uppercase
- `toLowercase(std::string s)` - return lowercase
- `isPalindrome(std::string s)` - return bool

**Math utilities:**
- `factorial(int n)` - return factorial
- `isPrime(int n)` - return bool
- `gcd(int a, int b)` - return GCD
- `lcm(int a, int b)` - return LCM

**Menu system:**
- Menu for each category
- Get appropriate input
- Display results
- Repeat until exit

**Requirements:**
- At least 10 functions
- Two distinct categories (string and math)
- Menu-driven interface
- Clear organization

---