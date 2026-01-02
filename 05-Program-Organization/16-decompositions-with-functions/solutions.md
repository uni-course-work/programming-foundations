# Chapter 16: Solutions


## Section A: Understanding Functions and Scope

### Problem 1: Tracing Function Execution

**Solution:**

**Step-by-step trace:**
```
main(): x=5, y=3
Call add(5, 3):
  add(): a=5, b=3
  sum = 5 + 3 = 8
  return 8
result1 = 8

Call multiply(8, 2):
  multiply(): a=8, b=2
  product = 8 * 2 = 16
  return 16
result2 = 16

Output: Result: 16
```

**Parameters vs. Arguments:**
- In `add(x, y)` call: x and y are arguments, a and b are parameters
- In `multiply(result1, 2)` call: result1 and 2 are arguments, a and b are parameters

**Scope of sum:**
- `sum` is declared in `add()` function
- Exists only within `add()`
- Created when function is called, destroyed when returns

**Scope of x:**
- `x` is declared in `main()`
- Exists only within `main()`
- `add()` cannot directly access x (has its own parameter `a`)

**Final output:** `Result: 16`

**Can you access sum from main?**
- No! `sum` only exists inside `add()` function
- It's a local variable with scope limited to that function
- After `add()` returns, `sum` no longer exists

---

### Problem 2: Pass-by-Value vs. Pass-by-Reference

**Solution:**

**Program A output:** `5`
**Program B output:** `6`
**Program C output:** `6`

**Why A and B differ:**
- A: `increment(int n)` - pass by value (n is a copy of x)
  - Incrementing n increments the copy, not the original
  - x remains 5
- B: `increment(int &n)` - pass by reference (n is x itself)
  - Incrementing n increments the actual x
  - x becomes 6

**When to use each:**
- **Pass-by-value:** When you don't want to modify the original, want isolation
  - Example: `square(int n)` doesn't need to change caller's variable
- **Pass-by-reference:** When you want to modify the original or avoid copying large data
  - Example: `getValidAge(int &age)` gets input and stores in caller's variable

**For getValidAge():**
```cpp
void getValidAge(int &age) {
    std::cout << "Enter age (0-150): ";
    std::cin >> age;
    while (age < 0 || age > 150) {
        std::cout << "Invalid. Enter 0-150: ";
        std::cin >> age;
    }
}

// In main:
int age;
getValidAge(age);  // age is modified inside function
std::cout << "You are " << age << std::endl;
```

Use pass-by-reference because we want to store the validated value back in the caller's variable.

---

### Problem 3: Function Overloading

**Solution:**

**Declarations:**
```cpp
int maximum(int a, int b);
int maximum(int a, int b, int c);
double maximum(double a, double b);
```

**Definitions:**
```cpp
int maximum(int a, int b) {
    return (a > b) ? a : b;
}

int maximum(int a, int b, int c) {
    int max = a;
    if (b > max) max = b;
    if (c > max) max = c;
    return max;
}

double maximum(double a, double b) {
    return (a > b) ? a : b;
}
```

**Main:**
```cpp
int main() {
    std::cout << maximum(5, 3) << std::endl;           // 5
    std::cout << maximum(5, 3, 8) << std::endl;        // 8
    std::cout << maximum(5.5, 3.2) << std::endl;       // 5.5
    return 0;
}
```

**How C++ knows which version:**
- `maximum(5, 3)`: Two ints → calls `int maximum(int, int)`
- `maximum(5, 3, 8)`: Three ints → calls `int maximum(int, int, int)`
- `maximum(5.5, 3.2)`: Two doubles → calls `double maximum(double, double)`

C++ chooses based on **number and type of arguments** (the function signature).

---

## Section B: Refactoring Previous Programs

### Problem 4: Refactor the Validation Code

**Solution:**

```cpp
#include <iostream>
#include <string>

int getValidIntegerInRange(int min, int max, std::string prompt) {
    int value;
    std::cout << prompt << " (" << min << "-" << max << "): ";
    std::cin >> value;
    
    while (value < min || value > max) {
        std::cout << "Invalid. Enter " << min << "-" << max << ": ";
        std::cin >> value;
    }
    
    return value;
}

int main() {
    std::cout << "DATA INPUT SYSTEM" << std::endl;
    std::cout << "=================" << std::endl;
    std::cout << std::endl;
    
    int age = getValidIntegerInRange(0, 150, "Enter your age");
    int grade = getValidIntegerInRange(0, 100, "Enter test grade");
    int numStudents = getValidIntegerInRange(1, 100, "Enter number of students");
    
    std::cout << std::endl;
    std::cout << "SUMMARY" << std::endl;
    std::cout << "Age: " << age << std::endl;
    std::cout << "Grade: " << grade << std::endl;
    std::cout << "Students: " << numStudents << std::endl;
    
    return 0;
}
```

**Output:**
```
DATA INPUT SYSTEM
=================

Enter your age (0-150): -5
Invalid. Enter 0-150: 200
Invalid. Enter 0-150: 25

Enter test grade (0-100): 150
Invalid. Enter 0-100: 95

Enter number of students (1-100): 5

SUMMARY
Age: 25
Grade: 95
Students: 5
```

**Benefits over Chapter 15:**
- Validation code written once, reused three times
- If validation logic needs to change, fix one place
- Main is clean and readable
- Pattern is reusable for any future programs

---

### Problem 5: Refactor Menu Code

**Solution:**

```cpp
#include <iostream>
#include <cmath>

int getMenuChoice(int numOptions) {
    int choice;
    std::cout << "Choice: ";
    std::cin >> choice;
    
    while (choice < 1 || choice > numOptions) {
        std::cout << "Invalid. Enter 1-" << numOptions << ": ";
        std::cin >> choice;
    }
    
    return choice;
}

double add(double a, double b) {
    return a + b;
}

double subtract(double a, double b) {
    return a - b;
}

double multiply(double a, double b) {
    return a * b;
}

double divide(double a, double b) {
    if (b == 0) {
        std::cout << "Error: Cannot divide by zero." << std::endl;
        return 0;
    }
    return a / b;
}

int main() {
    int choice;
    double x, y, result;
    
    do {
        std::cout << std::endl;
        std::cout << "=== CALCULATOR ===" << std::endl;
        std::cout << "1. Add" << std::endl;
        std::cout << "2. Subtract" << std::endl;
        std::cout << "3. Multiply" << std::endl;
        std::cout << "4. Divide" << std::endl;
        std::cout << "5. Exit" << std::endl;
        
        choice = getMenuChoice(5);
        
        if (choice >= 1 && choice <= 4) {
            std::cout << "First number: ";
            std::cin >> x;
            
            std::cout << "Second number: ";
            std::cin >> y;
            
            if (choice == 1) {
                result = add(x, y);
            } else if (choice == 2) {
                result = subtract(x, y);
            } else if (choice == 3) {
                result = multiply(x, y);
            } else {
                result = divide(x, y);
            }
            
            std::cout << "Result: " << result << std::endl;
        }
        
    } while (choice != 5);
    
    std::cout << "Goodbye!" << std::endl;
    
    return 0;
}
```

**Key improvements:**
- `getMenuChoice()` handles validation (reusable)
- Each operation is a separate function
- Main loop is clear and readable
- Menu doesn't need to be in main (could move to function too)

---

### Problem 6: Refactor the Banking Program

**Solution:**

```cpp
#include <iostream>
#include <iomanip>

void displayMenu() {
    std::cout << std::endl;
    std::cout << "=== BANKING SYSTEM ===" << std::endl;
    std::cout << "1. Check Balance" << std::endl;
    std::cout << "2. Deposit" << std::endl;
    std::cout << "3. Withdraw" << std::endl;
    std::cout << "4. Exit" << std::endl;
    std::cout << "Choice: ";
}

void displayBalance(double balance) {
    std::cout << "Current balance: $" << std::fixed << std::setprecision(2) << balance << std::endl;
}

double deposit(double balance, double amount) {
    if (amount <= 0) {
        std::cout << "Error: Amount must be positive." << std::endl;
        return balance;
    }
    
    balance += amount;
    std::cout << "Deposited: $" << std::fixed << std::setprecision(2) << amount << std::endl;
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
    std::cout << "Withdrew: $" << std::fixed << std::setprecision(2) << amount << std::endl;
    std::cout << "New balance: $" << balance << std::endl;
    return balance;
}

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
            std::cout << "Enter amount to deposit: $";
            std::cin >> amount;
            balance = deposit(balance, amount);
        } else if (choice == 3) {
            std::cout << "Enter amount to withdraw: $";
            std::cin >> amount;
            balance = withdraw(balance, amount);
        } else if (choice == 4) {
            std::cout << "Goodbye!" << std::endl;
        } else {
            std::cout << "Invalid choice." << std::endl;
        }
        
    } while (choice != 4);
    
    return 0;
}
```

**Comparison:**
- **Chapter 15 version:** 80 lines, all in main, hard to test, hard to modify
- **Refactored version:** 60 lines, clear separation of concerns, testable, maintainable
- **Main focus:** Shows flow, not implementation details

---

## Section C: Building Programs with Functions

### Problem 7: Create a Statistics Calculator

**Solution:**

```cpp
#include <iostream>

int getValidCount() {
    int count;
    std::cout << "How many numbers? ";
    std::cin >> count;
    
    while (count < 1 || count > 100) {
        std::cout << "Invalid. Enter 1-100: ";
        std::cin >> count;
    }
    
    return count;
}

double calculateSum(double n1, double n2, double n3, double n4, double n5) {
    return n1 + n2 + n3 + n4 + n5;
}

double calculateAverage(double sum, int count) {
    return sum / count;
}

double calculateRange(double max, double min) {
    return max - min;
}

double findMax(double n1, double n2, double n3, double n4, double n5) {
    double max = n1;
    if (n2 > max) max = n2;
    if (n3 > max) max = n3;
    if (n4 > max) max = n4;
    if (n5 > max) max = n5;
    return max;
}

double findMin(double n1, double n2, double n3, double n4, double n5) {
    double min = n1;
    if (n2 < min) min = n2;
    if (n3 < min) min = n3;
    if (n4 < min) min = n4;
    if (n5 < min) min = n5;
    return min;
}

void displayStatistics(double sum, double avg, double max, double min, double range) {
    std::cout << std::endl;
    std::cout << "=== STATISTICS ===" << std::endl;
    std::cout << "Sum: " << sum << std::endl;
    std::cout << "Average: " << avg << std::endl;
    std::cout << "Maximum: " << max << std::endl;
    std::cout << "Minimum: " << min << std::endl;
    std::cout << "Range: " << range << std::endl;
}

int main() {
    std::cout << "STATISTICS CALCULATOR" << std::endl;
    std::cout << "=====================" << std::endl;
    std::cout << std::endl;
    
    int count = getValidCount();
    
    // For now, simplified to 5 numbers (arrays coming in Ch 17)
    if (count == 5) {
        double n1, n2, n3, n4, n5;
        
        std::cout << "Enter number 1: ";
        std::cin >> n1;
        std::cout << "Enter number 2: ";
        std::cin >> n2;
        std::cout << "Enter number 3: ";
        std::cin >> n3;
        std::cout << "Enter number 4: ";
        std::cin >> n4;
        std::cout << "Enter number 5: ";
        std::cin >> n5;
        
        double sum = calculateSum(n1, n2, n3, n4, n5);
        double avg = calculateAverage(sum, count);
        double max = findMax(n1, n2, n3, n4, n5);
        double min = findMin(n1, n2, n3, n4, n5);
        double range = calculateRange(max, min);
        
        displayStatistics(sum, avg, max, min, range);
    } else {
        std::cout << "Note: This version handles 5 numbers. Arrays come in Chapter 17!" << std::endl;
    }
    
    return 0;
}
```

**Key points:**
- Multiple small functions, each doing one thing
- Functions are testable independently
- Main is readable
- Note: This demonstrates the need for arrays (hint for Ch 17)

---

### Problem 8: Create a Grade Reporting System with Functions

**Solution:**

```cpp
#include <iostream>
#include <string>

int getValidScore(std::string label) {
    int score;
    std::cout << label << " score (0-100): ";
    std::cin >> score;
    
    while (score < 0 || score > 100) {
        std::cout << "Invalid. Enter 0-100: ";
        std::cin >> score;
    }
    
    return score;
}

int getValidAssignments() {
    int assignments;
    std::cout << "Assignments completed (0-10): ";
    std::cin >> assignments;
    
    while (assignments < 0 || assignments > 10) {
        std::cout << "Invalid. Enter 0-10: ";
        std::cin >> assignments;
    }
    
    return assignments;
}

double calculateTotalGrade(int midterm, int final, int assignments) {
    return (midterm * 0.30) + (final * 0.50) + (assignments * 2.0);
}

char getLetterGrade(double totalGrade) {
    if (totalGrade >= 90) return 'A';
    if (totalGrade >= 80) return 'B';
    if (totalGrade >= 70) return 'C';
    if (totalGrade >= 60) return 'D';
    return 'F';
}

bool getPassStatus(double totalGrade) {
    return (totalGrade >= 60);
}

void displayReport(std::string name, int midterm, int final, int assignments, double grade, char letter, bool passed) {
    std::cout << std::endl;
    std::cout << "=== STUDENT REPORT ===" << std::endl;
    std::cout << "Name: " << name << std::endl;
    std::cout << "Midterm: " << midterm << " (30% weight)" << std::endl;
    std::cout << "Final: " << final << " (50% weight)" << std::endl;
    std::cout << "Assignments: " << assignments << "/10 (20% weight)" << std::endl;
    std::cout << std::endl;
    std::cout << "Total Grade: " << grade << std::endl;
    std::cout << "Letter Grade: " << letter << std::endl;
    std::cout << "Status: " << (passed ? "PASSED" : "FAILED") << std::endl;
}

int main() {
    std::cout << "GRADE REPORTING SYSTEM" << std::endl;
    std::cout << "======================" << std::endl;
    std::cout << std::endl;
    
    std::string name;
    std::cout << "Student name: ";
    std::cin >> name;
    
    int midterm = getValidScore("Midterm");
    int final = getValidScore("Final");
    int assignments = getValidAssignments();
    
    double totalGrade = calculateTotalGrade(midterm, final, assignments);
    char letter = getLetterGrade(totalGrade);
    bool passed = getPassStatus(totalGrade);
    
    displayReport(name, midterm, final, assignments, totalGrade, letter, passed);
    
    return 0;
}
```

**Example output:**
```
GRADE REPORTING SYSTEM
======================

Student name: Alice
Midterm score (0-100): 85
Final score (0-100): 90
Assignments completed (0-10): 9

=== STUDENT REPORT ===
Name: Alice
Midterm: 85 (30% weight)
Final: 90 (50% weight)
Assignments: 9/10 (20% weight)

Total Grade: 87.7
Letter Grade: B
Status: PASSED
```

---

### Problem 9: Create a Temperature Converter with Multiple Units

**Solution:**

```cpp
#include <iostream>
#include <iomanip>

double celsiusToFahrenheit(double c) {
    return (c * 9.0 / 5.0) + 32.0;
}

double celsiusToKelvin(double c) {
    return c + 273.15;
}

double fahrenheitToCelsius(double f) {
    return (f - 32.0) * 5.0 / 9.0;
}

double fahrenheitToKelvin(double f) {
    return celsiusToKelvin(fahrenheitToCelsius(f));
}

double kelvinToCelsius(double k) {
    return k - 273.15;
}

double kelvinToFahrenheit(double k) {
    return celsiusToFahrenheit(kelvinToCelsius(k));
}

void displayMenu() {
    std::cout << std::endl;
    std::cout << "TEMPERATURE CONVERTER" << std::endl;
    std::cout << "1. Celsius" << std::endl;
    std::cout << "2. Fahrenheit" << std::endl;
    std::cout << "3. Kelvin" << std::endl;
    std::cout << "4. Exit" << std::endl;
}

void convertAndDisplay(double temp, int unit) {
    std::cout << std::endl;
    std::cout << "=== CONVERSIONS ===" << std::endl;
    
    if (unit == 1) {  // From Celsius
        std::cout << "Celsius: " << std::fixed << std::setprecision(2) << temp << "°C" << std::endl;
        std::cout << "Fahrenheit: " << celsiusToFahrenheit(temp) << "°F" << std::endl;
        std::cout << "Kelvin: " << celsiusToKelvin(temp) << "K" << std::endl;
    } else if (unit == 2) {  // From Fahrenheit
        std::cout << "Fahrenheit: " << std::fixed << std::setprecision(2) << temp << "°F" << std::endl;
        std::cout << "Celsius: " << fahrenheitToCelsius(temp) << "°C" << std::endl;
        std::cout << "Kelvin: " << fahrenheitToKelvin(temp) << "K" << std::endl;
    } else if (unit == 3) {  // From Kelvin
        std::cout << "Kelvin: " << std::fixed << std::setprecision(2) << temp << "K" << std::endl;
        std::cout << "Celsius: " << kelvinToCelsius(temp) << "°C" << std::endl;
        std::cout << "Fahrenheit: " << kelvinToFahrenheit(temp) << "°F" << std::endl;
    }
}

int main() {
    int choice;
    double temp;
    char again;
    
    do {
        displayMenu();
        std::cout << "Enter temperature unit: ";
        std::cin >> choice;
        
        if (choice >= 1 && choice <= 3) {
            std::cout << "Enter temperature: ";
            std::cin >> temp;
            convertAndDisplay(temp, choice);
        } else if (choice == 4) {
            std::cout << "Goodbye!" << std::endl;
            break;
        } else {
            std::cout << "Invalid choice." << std::endl;
        }
        
    } while (true);
    
    return 0;
}
```

**Key aspects:**
- 6 conversion functions (practice designing functions)
- Functions call other functions (fahrenheitToKelvin calls other functions)
- Clean main loop with clear decision flow

---

### Problem 10: Create a Multi-Function Calculator with History

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <cmath>

std::string history[5];
int historyCount = 0;

double add(double a, double b) {
    return a + b;
}

double subtract(double a, double b) {
    return a - b;
}

double multiply(double a, double b) {
    return a * b;
}

double divide(double a, double b) {
    if (b == 0) {
        std::cout << "Error: Cannot divide by zero." << std::endl;
        return 0;
    }
    return a / b;
}

double power(double base, double exp) {
    return pow(base, exp);
}

double squareRoot(double x) {
    if (x < 0) {
        std::cout << "Error: Cannot take square root of negative number." << std::endl;
        return 0;
    }
    return sqrt(x);
}

void addToHistory(std::string operation) {
    if (historyCount < 5) {
        history[historyCount] = operation;
        historyCount++;
    } else {
        // Shift and add
        for (int i = 0; i < 4; i++) {
            history[i] = history[i + 1];
        }
        history[4] = operation;
    }
}

void displayHistory() {
    if (historyCount == 0) {
        std::cout << "No history yet." << std::endl;
    } else {
        std::cout << std::endl;
        std::cout << "=== HISTORY ===" << std::endl;
        for (int i = 0; i < historyCount; i++) {
            std::cout << (i + 1) << ". " << history[i] << std::endl;
        }
    }
}

void clearHistory() {
    historyCount = 0;
    std::cout << "History cleared." << std::endl;
}

double getValidNumber() {
    double num;
    std::cout << "Enter number: ";
    std::cin >> num;
    return num;
}

void displayMenu() {
    std::cout << std::endl;
    std::cout << "=== CALCULATOR ===" << std::endl;
    std::cout << "1. Add" << std::endl;
    std::cout << "2. Subtract" << std::endl;
    std::cout << "3. Multiply" << std::endl;
    std::cout << "4. Divide" << std::endl;
    std::cout << "5. Power" << std::endl;
    std::cout << "6. Square Root" << std::endl;
    std::cout << "7. View History" << std::endl;
    std::cout << "8. Clear History" << std::endl;
    std::cout << "9. Exit" << std::endl;
    std::cout << "Choice: ";
}

int main() {
    int choice;
    double num1, num2, result;
    
    do {
        displayMenu();
        std::cin >> choice;
        
        if (choice == 1) {
            num1 = getValidNumber();
            std::cout << "Second number: ";
            std::cin >> num2;
            result = add(num1, num2);
            std::cout << "Result: " << result << std::endl;
            addToHistory(std::to_string(num1) + " + " + std::to_string(num2) + " = " + std::to_string(result));
            
        } else if (choice == 2) {
            num1 = getValidNumber();
            std::cout << "Second number: ";
            std::cin >> num2;
            result = subtract(num1, num2);
            std::cout << "Result: " << result << std::endl;
            addToHistory(std::to_string(num1) + " - " + std::to_string(num2) + " = " + std::to_string(result));
            
        } else if (choice == 3) {
            num1 = getValidNumber();
            std::cout << "Second number: ";
            std::cin >> num2;
            result = multiply(num1, num2);
            std::cout << "Result: " << result << std::endl;
            addToHistory(std::to_string(num1) + " * " + std::to_string(num2) + " = " + std::to_string(result));
            
        } else if (choice == 4) {
            num1 = getValidNumber();
            std::cout << "Second number: ";
            std::cin >> num2;
            result = divide(num1, num2);
            if (num2 != 0) {
                std::cout << "Result: " << result << std::endl;
                addToHistory(std::to_string(num1) + " / " + std::to_string(num2) + " = " + std::to_string(result));
            }
            
        } else if (choice == 5) {
            num1 = getValidNumber();
            std::cout << "Exponent: ";
            std::cin >> num2;
            result = power(num1, num2);
            std::cout << "Result: " << result << std::endl;
            addToHistory(std::to_string(num1) + " ^ " + std::to_string(num2) + " = " + std::to_string(result));
            
        } else if (choice == 6) {
            num1 = getValidNumber();
            result = squareRoot(num1);
            if (num1 >= 0) {
                std::cout << "Result: " << result << std::endl;
                addToHistory("sqrt(" + std::to_string(num1) + ") = " + std::to_string(result));
            }
            
        } else if (choice == 7) {
            displayHistory();
            
        } else if (choice == 8) {
            clearHistory();
            
        } else if (choice == 9) {
            std::cout << "Goodbye!" << std::endl;
            
        } else {
            std::cout << "Invalid choice." << std::endl;
        }
        
    } while (choice != 9);
    
    return 0;
}
```

**Key features:**
- 8 operation functions
- History storage (last 5)
- Global history array (motivates better data structures)
- Menu-driven design
- Error handling (division by zero, sqrt of negative)

---