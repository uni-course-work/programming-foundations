# Chapter 15: Solutions

## Section A: Understanding Loop Mechanics

### Problem 1: Tracing Loop Execution

**Solution:**

1. **What does each loop output?**

**Loop A:** `0 1 2 3 4` 
**Loop B:** `0 1 2 3 4`
**Loop C:** `0 1 2 3 4`

2. **Are they equivalent?**

Yes, when starting with `i = 0`, all three produce identical output. They demonstrate that for, while, and do-while can accomplish the same task.

3. **Change starting value to i = 5. What does each output?**

**Loop A:** (nothing - loop doesn't execute because 5 < 5 is false)
**Loop B:** (nothing - condition 5 < 5 is false before first iteration)
**Loop C:** `5` (executes once, then condition 6 < 5 is false)

4. **Why does Loop C behave differently?**

Loop C is a `do-while` loop, which **always executes the body at least once** before checking the condition. Loops A and B check the condition first, so if it's initially false, they never execute.

5. **When would you choose each loop type?**

- **for loop (A):** When you know exactly how many iterations (counting from 0 to N)
- **while loop (B):** When iterations are unknown and body might not execute at all (input validation, sentinel loops)
- **do-while loop (C):** When iterations are unknown but body must execute at least once (menus, confirmations)

---

### Problem 2: Identifying Loop Invariants

**Solution:**

**Loop 1:**
```cpp
int sum = 0;
for (int i = 1; i <= 10; i++) {
    sum += i;
}
```

1. **Loop Invariant:** "After each iteration, `sum` contains the total of all integers from 1 to `i`."

2. **Verification:**
   - Before loop: i=0, sum=0 (sum of 1 to 0 is 0) ✓
   - After iteration 1: i=1, sum=1 (sum of 1 to 1 is 1) ✓
   - After iteration 2: i=2, sum=3 (sum of 1 to 2 is 1+2=3) ✓
   - After iteration 3: i=3, sum=6 (sum of 1 to 3 is 1+2+3=6) ✓

3. **How it helps:** The invariant tells us exactly what the loop computes—the sum of integers from 1 to 10.

**Loop 2:**
```cpp
int count = 0;
for (int i = 1; i <= 100; i++) {
    if (i % 2 == 0) {
        count++;
    }
}
```

1. **Loop Invariant:** "After each iteration, `count` contains the number of even integers from 1 to `i`."

2. **Verification:**
   - After i=1: count=0 (no evens in [1]) ✓
   - After i=2: count=1 (one even: 2) ✓
   - After i=3: count=1 (still one even) ✓
   - After i=4: count=2 (two evens: 2, 4) ✓

3. **How it helps:** The invariant reveals the loop counts even numbers from 1 to 100.

**Loop 3:**
```cpp
int max = numbers[0];
for (int i = 1; i < n; i++) {
    if (numbers[i] > max) {
        max = numbers[i];
    }
}
```

1. **Loop Invariant:** "After each iteration, `max` contains the largest value among `numbers[0]` through `numbers[i]`."

2. **Verification (assume numbers = [5, 3, 9, 2, 7]):**
   - Before loop: max=5 (largest of numbers[0..0]) ✓
   - After i=1: max=5 (largest of [5,3]) ✓
   - After i=2: max=9 (largest of [5,3,9]) ✓
   - After i=3: max=9 (largest of [5,3,9,2]) ✓

3. **How it helps:** The invariant shows the loop finds the maximum value in an array.

---

## Section B: Basic Loop Programming

### Problem 3: Create a Number Pyramid Program

**Solution:**

```cpp
#include <iostream>

int main() {
    int n;
    
    std::cout << "NUMBER PYRAMID GENERATOR" << std::endl;
    std::cout << "========================" << std::endl;
    std::cout << std::endl;
    
    // Validate input
    std::cout << "Enter a number (1-10): ";
    std::cin >> n;
    
    while (n < 1 || n > 10) {
        std::cout << "Invalid. Enter a number between 1 and 10: ";
        std::cin >> n;
    }
    
    std::cout << std::endl;
    
    // Generate pyramid
    for (int row = 1; row <= n; row++) {
        for (int col = 1; col <= row; col++) {
            std::cout << col << " ";
        }
        std::cout << std::endl;
    }
    
    return 0;
}
```

**Example runs:**

**Test 1: N=3**
```
NUMBER PYRAMID GENERATOR
========================

Enter a number (1-10): 3

1 
1 2 
1 2 3 
```

**Test 2: N=5**
```
Enter a number (1-10): 5

1 
1 2 
1 2 3 
1 2 3 4 
1 2 3 4 5 
```

**Test 3: Invalid input**
```
Enter a number (1-10): 15
Invalid. Enter a number between 1 and 10: 0
Invalid. Enter a number between 1 and 10: 4

1 
1 2 
1 2 3 
1 2 3 4 
```

---

### Problem 4: Create a Sum Calculator

**Solution:**

```cpp
#include <iostream>

int main() {
    int count;
    double number, sum, max, min, average;
    
    std::cout << "SUM CALCULATOR" << std::endl;
    std::cout << "==============" << std::endl;
    std::cout << std::endl;
    
    std::cout << "How many numbers? ";
    std::cin >> count;
    
    if (count < 1) {
        std::cout << "Error: Must enter at least 1 number." << std::endl;
        return 0;
    }
    
    // Read first number to initialize max and min
    std::cout << "Enter number 1: ";
    std::cin >> number;
    
    sum = number;
    max = number;
    min = number;
    
    // Read remaining numbers
    for (int i = 2; i <= count; i++) {
        std::cout << "Enter number " << i << ": ";
        std::cin >> number;
        
        sum += number;
        
        if (number > max) {
            max = number;
        }
        
        if (number < min) {
            min = number;
        }
    }
    
    average = sum / count;
    
    // Display results
    std::cout << std::endl;
    std::cout << "RESULTS" << std::endl;
    std::cout << "=======" << std::endl;
    std::cout << "Sum: " << sum << std::endl;
    std::cout << "Average: " << average << std::endl;
    std::cout << "Largest: " << max << std::endl;
    std::cout << "Smallest: " << min << std::endl;
    
    return 0;
}
```

**Example output:**
```
SUM CALCULATOR
==============

How many numbers? 5
Enter number 1: 10
Enter number 2: 25
Enter number 3: 5
Enter number 4: 30
Enter number 5: 15

RESULTS
=======
Sum: 85
Average: 17
Largest: 30
Smallest: 5
```

---

### Problem 5: Create a Multiplication Table Generator

**Solution:**

```cpp
#include <iostream>

int main() {
    int n;
    
    std::cout << "MULTIPLICATION TABLE GENERATOR" << std::endl;
    std::cout << "==============================" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter a number (1-12): ";
    std::cin >> n;
    
    while (n < 1 || n > 12) {
        std::cout << "Invalid. Enter a number between 1 and 12: ";
        std::cin >> n;
    }
    
    std::cout << std::endl;
    std::cout << "MULTIPLICATION TABLE FOR " << n << std::endl;
    std::cout << "==========================" << std::endl;
    
    for (int i = 1; i <= 12; i++) {
        std::cout << n << " x " << i << " = " << (n * i) << std::endl;
    }
    
    return 0;
}
```

**Example output:**
```
MULTIPLICATION TABLE GENERATOR
==============================

Enter a number (1-12): 7

MULTIPLICATION TABLE FOR 7
==========================
7 x 1 = 7
7 x 2 = 14
7 x 3 = 21
7 x 4 = 28
7 x 5 = 35
7 x 6 = 42
7 x 7 = 49
7 x 8 = 56
7 x 9 = 63
7 x 10 = 70
7 x 11 = 77
7 x 12 = 84
```

---

## Section C: Input Validation and Error Handling

### Problem 6: Create a Robust Age Input System

**Solution:**

```cpp
#include <iostream>

int main() {
    int age;
    
    std::cout << "AGE INPUT SYSTEM" << std::endl;
    std::cout << "================" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter your age: ";
    std::cin >> age;
    
    while (age < 0 || age > 150) {
        if (age < 0) {
            std::cout << "Error: Age cannot be negative. Try again." << std::endl;
        } else {
            std::cout << "Error: Age must be 0-150. Try again." << std::endl;
        }
        std::cout << "Enter your age: ";
        std::cin >> age;
    }
    
    std::cout << std::endl;
    std::cout << "You are " << age << " years old." << std::endl;
    
    // Categorize
    std::string category;
    if (age <= 12) {
        category = "Child";
    } else if (age <= 17) {
        category = "Teen";
    } else if (age <= 64) {
        category = "Adult";
    } else {
        category = "Senior";
    }
    
    std::cout << "Category: " << category << std::endl;
    
    return 0;
}
```

**Example output:**
```
AGE INPUT SYSTEM
================

Enter your age: -5
Error: Age cannot be negative. Try again.
Enter your age: 200
Error: Age must be 0-150. Try again.
Enter your age: 25

You are 25 years old.
Category: Adult
```

---

### Problem 7: Create a Password Validator

**Solution:**

```cpp
#include <iostream>
#include <string>

int main() {
    std::string password, confirm;
    bool validPassword = false;
    
    std::cout << "PASSWORD VALIDATOR" << std::endl;
    std::cout << "==================" << std::endl;
    std::cout << std::endl;
    
    // Password validation loop
    while (!validPassword) {
        std::cout << "Create a password: ";
        std::cin >> password;
        
        bool hasLength = (password.length() >= 8);
        bool hasDigit = (password.find('0') != std::string::npos ||
                        password.find('1') != std::string::npos ||
                        password.find('2') != std::string::npos ||
                        password.find('3') != std::string::npos ||
                        password.find('4') != std::string::npos ||
                        password.find('5') != std::string::npos ||
                        password.find('6') != std::string::npos ||
                        password.find('7') != std::string::npos ||
                        password.find('8') != std::string::npos ||
                        password.find('9') != std::string::npos);
        
        if (!hasLength) {
            std::cout << "Invalid: Must be at least 8 characters" << std::endl;
        }
        if (!hasDigit) {
            std::cout << "Invalid: Must contain a digit" << std::endl;
        }
        
        if (hasLength && hasDigit) {
            validPassword = true;
            std::cout << "Valid password!" << std::endl;
        }
        
        std::cout << std::endl;
    }
    
    // Confirmation loop
    bool confirmed = false;
    while (!confirmed) {
        std::cout << "Confirm your password: ";
        std::cin >> confirm;
        
        if (confirm == password) {
            confirmed = true;
            std::cout << "Password set successfully!" << std::endl;
        } else {
            std::cout << "Passwords don't match. Try again." << std::endl;
            std::cout << std::endl;
        }
    }
    
    return 0;
}
```

**Example output:**
```
PASSWORD VALIDATOR
==================

Create a password: hello
Invalid: Must be at least 8 characters
Invalid: Must contain a digit

Create a password: hello123
Valid password!

Confirm your password: hello124
Passwords don't match. Try again.

Confirm your password: hello123
Password set successfully!
```

---

## Section D: Menu-Driven Programs

### Problem 8: Create a Simple Banking System

**Solution:**

```cpp
#include <iostream>
#include <iomanip>

int main() {
    double balance = 1000.0;
    int choice;
    double amount;
    
    do {
        std::cout << std::endl;
        std::cout << "=== BANKING SYSTEM ===" << std::endl;
        std::cout << "Balance: $" << std::fixed << std::setprecision(2) << balance << std::endl;
        std::cout << std::endl;
        std::cout << "1. Check Balance" << std::endl;
        std::cout << "2. Deposit" << std::endl;
        std::cout << "3. Withdraw" << std::endl;
        std::cout << "4. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        std::cout << std::endl;
        
        if (choice == 1) {
            std::cout << "Current balance: $" << std::fixed << std::setprecision(2) << balance << std::endl;
            
        } else if (choice == 2) {
            std::cout << "Enter deposit amount: $";
            std::cin >> amount;
            
            if (amount <= 0) {
                std::cout << "Error: Amount must be positive." << std::endl;
            } else {
                balance += amount;
                std::cout << "Deposited $" << std::fixed << std::setprecision(2) << amount;
                std::cout << ". New balance: $" << balance << std::endl;
            }
            
        } else if (choice == 3) {
            std::cout << "Enter withdrawal amount: $";
            std::cin >> amount;
            
            if (amount <= 0) {
                std::cout << "Error: Amount must be positive." << std::endl;
            } else if (amount > balance) {
                std::cout << "Error: Insufficient funds." << std::endl;
            } else {
                balance -= amount;
                std::cout << "Withdrew $" << std::fixed << std::setprecision(2) << amount;
                std::cout << ". New balance: $" << balance << std::endl;
            }
            
        } else if (choice == 4) {
            std::cout << "Goodbye!" << std::endl;
            
        } else {
            std::cout << "Invalid choice." << std::endl;
        }
        
    } while (choice != 4);
    
    return 0;
}
```

**Note:** Requires `#include <iomanip>` for `std::fixed` and `std::setprecision()` to format currency.

---

### Problem 9: Create a Grade Management System

**Solution:**

```cpp
#include <iostream>

int main() {
    // Storage for up to 50 grades
    double grade1, grade2, grade3, grade4, grade5, grade6, grade7, grade8, grade9, grade10;
    int gradeCount = 0;
    int choice;
    
    do {
        std::cout << std::endl;
        std::cout << "=== GRADE MANAGEMENT ===" << std::endl;
        std::cout << "Grades stored: " << gradeCount << std::endl;
        std::cout << std::endl;
        std::cout << "1. Add Grade" << std::endl;
        std::cout << "2. Display All Grades" << std::endl;
        std::cout << "3. Calculate Average" << std::endl;
        std::cout << "4. Find Highest" << std::endl;
        std::cout << "5. Find Lowest" << std::endl;
        std::cout << "6. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        std::cout << std::endl;
        
        if (choice == 1) {
            if (gradeCount >= 10) {
                std::cout << "Error: Maximum 10 grades." << std::endl;
            } else {
                double grade;
                std::cout << "Enter grade (0-100): ";
                std::cin >> grade;
                
                if (grade < 0 || grade > 100) {
                    std::cout << "Invalid grade." << std::endl;
                } else {
                    gradeCount++;
                    if (gradeCount == 1) grade1 = grade;
                    else if (gradeCount == 2) grade2 = grade;
                    else if (gradeCount == 3) grade3 = grade;
                    else if (gradeCount == 4) grade4 = grade;
                    else if (gradeCount == 5) grade5 = grade;
                    else if (gradeCount == 6) grade6 = grade;
                    else if (gradeCount == 7) grade7 = grade;
                    else if (gradeCount == 8) grade8 = grade;
                    else if (gradeCount == 9) grade9 = grade;
                    else if (gradeCount == 10) grade10 = grade;
                    
                    std::cout << "Grade added! Total grades: " << gradeCount << std::endl;
                }
            }
            
        } else if (choice == 2) {
            if (gradeCount == 0) {
                std::cout << "No grades entered yet." << std::endl;
            } else {
                std::cout << "All Grades:" << std::endl;
                if (gradeCount >= 1) std::cout << "1. " << grade1 << std::endl;
                if (gradeCount >= 2) std::cout << "2. " << grade2 << std::endl;
                if (gradeCount >= 3) std::cout << "3. " << grade3 << std::endl;
                if (gradeCount >= 4) std::cout << "4. " << grade4 << std::endl;
                if (gradeCount >= 5) std::cout << "5. " << grade5 << std::endl;
                if (gradeCount >= 6) std::cout << "6. " << grade6 << std::endl;
                if (gradeCount >= 7) std::cout << "7. " << grade7 << std::endl;
                if (gradeCount >= 8) std::cout << "8. " << grade8 << std::endl;
                if (gradeCount >= 9) std::cout << "9. " << grade9 << std::endl;
                if (gradeCount >= 10) std::cout << "10. " << grade10 << std::endl;
            }
            
        } else if (choice == 3) {
            if (gradeCount == 0) {
                std::cout << "No grades to average." << std::endl;
            } else {
                double sum = 0;
                if (gradeCount >= 1) sum += grade1;
                if (gradeCount >= 2) sum += grade2;
                if (gradeCount >= 3) sum += grade3;
                if (gradeCount >= 4) sum += grade4;
                if (gradeCount >= 5) sum += grade5;
                if (gradeCount >= 6) sum += grade6;
                if (gradeCount >= 7) sum += grade7;
                if (gradeCount >= 8) sum += grade8;
                if (gradeCount >= 9) sum += grade9;
                if (gradeCount >= 10) sum += grade10;
                
                double average = sum / gradeCount;
                std::cout << "Average: " << average << std::endl;
            }
            
        } else if (choice == 4) {
            if (gradeCount == 0) {
                std::cout << "No grades entered." << std::endl;
            } else {
                double highest = grade1;
                if (gradeCount >= 2 && grade2 > highest) highest = grade2;
                if (gradeCount >= 3 && grade3 > highest) highest = grade3;
                if (gradeCount >= 4 && grade4 > highest) highest = grade4;
                if (gradeCount >= 5 && grade5 > highest) highest = grade5;
                if (gradeCount >= 6 && grade6 > highest) highest = grade6;
                if (gradeCount >= 7 && grade7 > highest) highest = grade7;
                if (gradeCount >= 8 && grade8 > highest) highest = grade8;
                if (gradeCount >= 9 && grade9 > highest) highest = grade9;
                if (gradeCount >= 10 && grade10 > highest) highest = grade10;
                
                std::cout << "Highest grade: " << highest << std::endl;
            }
            
        } else if (choice == 5) {
            if (gradeCount == 0) {
                std::cout << "No grades entered." << std::endl;
            } else {
                double lowest = grade1;
                if (gradeCount >= 2 && grade2 < lowest) lowest = grade2;
                if (gradeCount >= 3 && grade3 < lowest) lowest = grade3;
                if (gradeCount >= 4 && grade4 < lowest) lowest = grade4;
                if (gradeCount >= 5 && grade5 < lowest) lowest = grade5;
                if (gradeCount >= 6 && grade6 < lowest) lowest = grade6;
                if (gradeCount >= 7 && grade7 < lowest) lowest = grade7;
                if (gradeCount >= 8 && grade8 < lowest) lowest = grade8;
                if (gradeCount >= 9 && grade9 < lowest) lowest = grade9;
                if (gradeCount >= 10 && grade10 < lowest) lowest = grade10;
                
                std::cout << "Lowest grade: " << lowest << std::endl;
            }
            
        } else if (choice == 6) {
            std::cout << "Goodbye!" << std::endl;
            
        } else {
            std::cout << "Invalid choice." << std::endl;
        }
        
    } while (choice != 6);
    
    return 0;
}
```

**Note:** This version uses individual variables (grade1-grade10) since arrays haven't been introduced yet. This is intentionally cumbersome to motivate learning arrays in Chapter 16.

---

### Problem 10: Create a Number Guessing Game

**Solution:**

```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>

int main() {
    srand(time(0));  // Seed random generator
    char playAgain;
    
    do {
        int secret = rand() % 100 + 1;  // Random 1-100
        int guess;
        int attempts = 0;
        
        std::cout << std::endl;
        std::cout << "=== NUMBER GUESSING GAME ===" << std::endl;
        std::cout << "I'm thinking of a number between 1 and 100." << std::endl;
        std::cout << std::endl;
        
        while (guess != secret) {
            std::cout << "Guess: ";
            std::cin >> guess;
            attempts++;
            
            if (guess < secret) {
                std::cout << "Too low!" << std::endl;
            } else if (guess > secret) {
                std::cout << "Too high!" << std::endl;
            } else {
                std::cout << "Correct! You got it in " << attempts << " attempts." << std::endl;
            }
            std::cout << std::endl;
        }
        
        std::cout << "Play again? (y/n): ";
        std::cin >> playAgain;
        
    } while (playAgain == 'y' || playAgain == 'Y');
    
    std::cout << "Thanks for playing!" << std::endl;
    
    return 0;
}
```

---

## Section E: Pattern Generation and Nested Loops

### Problem 11: Create a Pattern Generator Menu

**Solution:**

```cpp
#include <iostream>

int main() {
    int choice, n;
    
    do {
        std::cout << std::endl;
        std::cout << "=== PATTERN GENERATOR ===" << std::endl;
        std::cout << "1. Right Triangle" << std::endl;
        std::cout << "2. Inverted Triangle" << std::endl;
        std::cout << "3. Pyramid" << std::endl;
        std::cout << "4. Diamond" << std::endl;
        std::cout << "5. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        std::cout << std::endl;
        
        if (choice >= 1 && choice <= 4) {
            std::cout << "Enter size (1-10): ";
            std::cin >> n;
            
            while (n < 1 || n > 10) {
                std::cout << "Invalid. Enter 1-10: ";
                std::cin >> n;
            }
            
            std::cout << std::endl;
        }
        
        if (choice == 1) {
            // Right Triangle
            for (int i = 1; i <= n; i++) {
                for (int j = 1; j <= i; j++) {
                    std::cout << "*";
                }
                std::cout << std::endl;
            }
            
        } else if (choice == 2) {
            // Inverted Triangle
            for (int i = n; i >= 1; i--) {
                for (int j = 1; j <= i; j++) {
                    std::cout << "*";
                }
                std::cout << std::endl;
            }
            
        } else if (choice == 3) {
            // Pyramid
            for (int i = 1; i <= n; i++) {
                // Print spaces
                for (int j = 1; j <= n - i; j++) {
                    std::cout << " ";
                }
                // Print stars
                for (int j = 1; j <= 2 * i - 1; j++) {
                    std::cout << "*";
                }
                std::cout << std::endl;
            }
            
        } else if (choice == 4) {
            // Diamond - upper half
            for (int i = 1; i <= n; i++) {
                for (int j = 1; j <= n - i; j++) {
                    std::cout << " ";
                }
                for (int j = 1; j <= 2 * i - 1; j++) {
                    std::cout << "*";
                }
                std::cout << std::endl;
            }
            // Diamond - lower half
            for (int i = n - 1; i >= 1; i--) {
                for (int j = 1; j <= n - i; j++) {
                    std::cout << " ";
                }
                for (int j = 1; j <= 2 * i - 1; j++) {
                    std::cout << "*";
                }
                std::cout << std::endl;
            }
            
        } else if (choice == 5) {
            std::cout << "Goodbye!" << std::endl;
            
        } else {
            std::cout << "Invalid choice." << std::endl;
        }
        
    } while (choice != 5);
    
    return 0;
}
```

---

## Section F: Data Processing and Accumulation

### Problem 12: Create a Student Grade Processor

**Solution:**

```cpp
#include <iostream>
#include <string>

int main() {
    int numStudents;
    
    std::cout << "STUDENT GRADE PROCESSOR" << std::endl;
    std::cout << "=======================" << std::endl;
    std::cout << std::endl;
    
    std::cout << "How many students (1-30)? ";
    std::cin >> numStudents;
    
    while (numStudents < 1 || numStudents > 30) {
        std::cout << "Invalid. Enter 1-30: ";
        std::cin >> numStudents;
    }
    
    std::cout << std::endl;
    
    // Variables to track statistics
    double sum = 0;
    int countA = 0, countB = 0, countC = 0, countD = 0, countF = 0;
    double maxScore = -1;
    double minScore = 101;
    std::string maxName, minName;
    
    // Input loop
    for (int i = 1; i <= numStudents; i++) {
        std::string name;
        double score;
        
        std::cout << "Student " << i << " name: ";
        std::cin >> name;
        
        std::cout << "Score: ";
        std::cin >> score;
        
        while (score < 0 || score > 100) {
            std::cout << "Invalid. Enter 0-100: ";
            std::cin >> score;
        }
        
        std::cout << std::endl;
        
        // Update statistics
        sum += score;
        
        if (score >= 90) countA++;
        else if (score >= 80) countB++;
        else if (score >= 70) countC++;
        else if (score >= 60) countD++;
        else countF++;
        
        if (score > maxScore) {
            maxScore = score;
            maxName = name;
        }
        
        if (score < minScore) {
            minScore = score;
            minName = name;
        }
    }
    
    double average = sum / numStudents;
    
    // Display report
    std::cout << "=== CLASS REPORT ===" << std::endl;
    std::cout << "Class Average: " << average << std::endl;
    std::cout << std::endl;
    std::cout << "Grade Distribution:" << std::endl;
    std::cout << "A (90-100): " << countA << " student(s)" << std::endl;
    std::cout << "B (80-89): " << countB << " student(s)" << std::endl;
    std::cout << "C (70-79): " << countC << " student(s)" << std::endl;
    std::cout << "D (60-69): " << countD << " student(s)" << std::endl;
    std::cout << "F (0-59): " << countF << " student(s)" << std::endl;
    std::cout << std::endl;
    std::cout << "Highest: " << maxName << " (" << maxScore << ")" << std::endl;
    std::cout << "Lowest: " << minName << " (" << minScore << ")" << std::endl;
    
    return 0;
}
```

---

### Problem 13: Create a Prime Number Finder

**Solution:**

```cpp
#include <iostream>

int main() {
    int n;
    char again;
    
    do {
        std::cout << std::endl;
        std::cout << "PRIME NUMBER FINDER" << std::endl;
        std::cout << "===================" << std::endl;
        std::cout << std::endl;
        
        std::cout << "Enter a number: ";
        std::cin >> n;
        
        std::cout << std::endl;
        std::cout << "Prime numbers from 2 to " << n << ":" << std::endl;
        
        int primeCount = 0;
        
        for (int num = 2; num <= n; num++) {
            bool isPrime = true;
            
            // Check if num is prime
            for (int divisor = 2; divisor < num; divisor++) {
                if (num % divisor == 0) {
                    isPrime = false;
                    break;
                }
            }
            
            if (isPrime) {
                std::cout << num << " ";
                primeCount++;
            }
        }
        
        std::cout << std::endl << std::endl;
        std::cout << "Total: " << primeCount << " prime numbers found." << std::endl;
        std::cout << std::endl;
        
        std::cout << "Try another number? (y/n): ";
        std::cin >> again;
        
    } while (again == 'y' || again == 'Y');
    
    std::cout << "Goodbye!" << std::endl;
    
    return 0;
}
```

**Example output:**
```
PRIME NUMBER FINDER
===================

Enter a number: 20

Prime numbers from 2 to 20:
2 3 5 7 11 13 17 19 

Total: 8 prime numbers found.

Try another number? (y/n): n
Goodbye!
```

---

## Challenge Problems Solutions

### Challenge 1: Create a Fibonacci Sequence Generator

**Solution:**

```cpp
#include <iostream>

int main() {
    int count;
    char again;
    
    do {
        std::cout << std::endl;
        std::cout << "FIBONACCI SEQUENCE GENERATOR" << std::endl;
        std::cout << "============================" << std::endl;
        std::cout << std::endl;
        
        std::cout << "How many Fibonacci numbers (1-50)? ";
        std::cin >> count;
        
        while (count < 1 || count > 50) {
            std::cout << "Invalid. Enter 1-50: ";
            std::cin >> count;
        }
        
        std::cout << std::endl;
        std::cout << "Fibonacci Sequence:" << std::endl;
        
        if (count >= 1) {
            long long first = 0;
            std::cout << first << " ";
        }
        
        if (count >= 2) {
            long long second = 1;
            std::cout << second << " ";
        }
        
        if (count > 2) {
            long long prev1 = 0;
            long long prev2 = 1;
            
            for (int i = 3; i <= count; i++) {
                long long next = prev1 + prev2;
                std::cout << next << " ";
                prev1 = prev2;
                prev2 = next;
            }
        }
        
        std::cout << std::endl << std::endl;
        
        std::cout << "Would you like to generate more? (y/n): ";
        std::cin >> again;
        
    } while (again == 'y' || again == 'Y');
    
    return 0;
}
```

---

### Challenge 2: Create a Collatz Conjecture Checker

**Solution:**

```cpp
#include <iostream>

int main() {
    long long num;
    char again;
    
    do {
        std::cout << std::endl;
        std::cout << "COLLATZ CONJECTURE CHECKER" << std::endl;
        std::cout << "==========================" << std::endl;
        std::cout << std::endl;
        
        std::cout << "Enter starting number: ";
        std::cin >> num;
        
        std::cout << std::endl;
        std::cout << "Collatz Sequence:" << std::endl;
        
        int steps = 0;
        
        while (num != 1) {
            std::cout << num << " → ";
            
            if (num % 2 == 0) {
                num = num / 2;
            } else {
                num = num * 3 + 1;
            }
            
            steps++;
        }
        
        std::cout << "1" << std::endl;
        std::cout << std::endl;
        std::cout << "Steps: " << steps << std::endl;
        std::cout << std::endl;
        
        std::cout << "Try another number? (y/n): ";
        std::cin >> again;
        
    } while (again == 'y' || again == 'Y');
    
    return 0;
}
```

---

### Challenge 3: Create a Text-Based Calculator with History

**Solution:**

```cpp
#include <iostream>
#include <string>

int main() {
    // History storage (up to 10 entries)
    std::string hist1, hist2, hist3, hist4, hist5, hist6, hist7, hist8, hist9, hist10;
    int histCount = 0;
    
    int choice;
    
    do {
        std::cout << std::endl;
        std::cout << "=== CALCULATOR WITH HISTORY ===" << std::endl;
        std::cout << std::endl;
        std::cout << "1. Add" << std::endl;
        std::cout << "2. Subtract" << std::endl;
        std::cout << "3. Multiply" << std::endl;
        std::cout << "4. Divide" << std::endl;
        std::cout << "5. View History" << std::endl;
        std::cout << "6. Clear History" << std::endl;
        std::cout << "7. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        std::cout << std::endl;
        
        if (choice >= 1 && choice <= 4) {
            double x, y, result;
            char op;
            std::string opSymbol;
            
            std::cout << "Enter first number: ";
            std::cin >> x;
            
            std::cout << "Enter second number: ";
            std::cin >> y;
            
            if (choice == 1) {
                result = x + y;
                opSymbol = " + ";
            } else if (choice == 2) {
                result = x - y;
                opSymbol = " - ";
            } else if (choice == 3) {
                result = x * y;
                opSymbol = " × ";
            } else if (choice == 4) {
                if (y == 0) {
                    std::cout << "Error: Cannot divide by zero." << std::endl;
                    continue;
                }
                result = x / y;
                opSymbol = " ÷ ";
            }
            
            std::cout << "Result: " << x << opSymbol << y << " = " << result << std::endl;
            
            // Add to history (shift if full)
            if (histCount < 10) {
                histCount++;
            } else {
                // Shift history (remove oldest)
                hist1 = hist2;
                hist2 = hist3;
                hist3 = hist4;
                hist4 = hist5;
                hist5 = hist6;
                hist6 = hist7;
                hist7 = hist8;
                hist8 = hist9;
                hist9 = hist10;
            }
            
            // Build history string
            std::string entry = std::to_string(x) + opSymbol + std::to_string(y) + " = " + std::to_string(result);
            
            if (histCount == 1) hist1 = entry;
            else if (histCount == 2) hist2 = entry;
            else if (histCount == 3) hist3 = entry;
            else if (histCount == 4) hist4 = entry;
            else if (histCount == 5) hist5 = entry;
            else if (histCount == 6) hist6 = entry;
            else if (histCount == 7) hist7 = entry;
            else if (histCount == 8) hist8 = entry;
            else if (histCount == 9) hist9 = entry;
            else if (histCount == 10) hist10 = entry;
            
        } else if (choice == 5) {
            if (histCount == 0) {
                std::cout << "No history yet." << std::endl;
            } else {
                std::cout << "=== CALCULATION HISTORY ===" << std::endl;
                if (histCount >= 1) std::cout << "1. " << hist1 << std::endl;
                if (histCount >= 2) std::cout << "2. " << hist2 << std::endl;
                if (histCount >= 3) std::cout << "3. " << hist3 << std::endl;
                if (histCount >= 4) std::cout << "4. " << hist4 << std::endl;
                if (histCount >= 5) std::cout << "5. " << hist5 << std::endl;
                if (histCount >= 6) std::cout << "6. " << hist6 << std::endl;
                if (histCount >= 7) std::cout << "7. " << hist7 << std::endl;
                if (histCount >= 8) std::cout << "8. " << hist8 << std::endl;
                if (histCount >= 9) std::cout << "9. " << hist9 << std::endl;
                if (histCount >= 10) std::cout << "10. " << hist10 << std::endl;
            }
            
        } else if (choice == 6) {
            histCount = 0;
            std::cout << "History cleared." << std::endl;
            
        } else if (choice == 7) {
            std::cout << "Goodbye!" << std::endl;
            
        } else {
            std::cout << "Invalid choice." << std::endl;
        }
        
    } while (choice != 7);
    
    return 0;
}
```

---