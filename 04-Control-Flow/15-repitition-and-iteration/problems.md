# Chapter 15: Problems

## Section A: Understanding Loop Mechanics

### Problem 1: Tracing Loop Execution

Consider these three loops:

**Loop A:**
```cpp
for (int i = 0; i < 5; i++) {
    std::cout << i << " ";
}
```

**Loop B:**
```cpp
int i = 0;
while (i < 5) {
    std::cout << i << " ";
    i++;
}
```

**Loop C:**
```cpp
int i = 0;
do {
    std::cout << i << " ";
    i++;
} while (i < 5);
```

1. What does each loop output?
2. Are they equivalent? Why or why not?
3. Change the starting value to `i = 5`. What does each output now?
4. Why does Loop C behave differently?
5. When would you choose each loop type?

---

### Problem 2: Identifying Loop Invariants

For each loop, identify the loop invariant:

**Loop 1:**
```cpp
int sum = 0;
for (int i = 1; i <= 10; i++) {
    sum += i;
}
```

**Loop 2:**
```cpp
int count = 0;
for (int i = 1; i <= 100; i++) {
    if (i % 2 == 0) {
        count++;
    }
}
```

**Loop 3:**
```cpp
int max = numbers[0];
for (int i = 1; i < n; i++) {
    if (numbers[i] > max) {
        max = numbers[i];
    }
}
```

For each:
1. State the loop invariant in plain English
2. Verify the invariant holds after 2-3 iterations
3. Explain how the invariant helps you understand what the loop does

---

## Section B: Basic Loop Programming

### Problem 3: Create a Number Pyramid Program

**Write a complete program** that:
1. Asks the user for a number N (1-10)
2. Validates that N is in range 1-10
3. Prints a pyramid of numbers from 1 to N

Example output for N=5:
```
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5
```

Requirements:
- Use nested loops
- Validate input with a loop
- Show example runs with N=3, N=5, and invalid input

---

### Problem 4: Create a Sum Calculator

**Write a complete program** that:
1. Asks the user how many numbers they want to enter
2. Reads that many numbers using a loop
3. Calculates and displays:
   - The sum of all numbers
   - The average
   - The largest number
   - The smallest number

Example run:
```
How many numbers? 5
Enter number 1: 10
Enter number 2: 25
Enter number 3: 5
Enter number 4: 30
Enter number 5: 15

Results:
Sum: 85
Average: 17
Largest: 30
Smallest: 5
```

Requirements:
- Use a for loop to read numbers
- Track sum, max, and min during the loop
- Handle edge case (1 number)

---

### Problem 5: Create a Multiplication Table Generator

**Write a complete program** that:
1. Asks the user for a number N (1-12)
2. Validates the input
3. Displays the multiplication table for N from 1 to 12

Example output for N=7:
```
MULTIPLICATION TABLE FOR 7
==========================
7 x 1 = 7
7 x 2 = 14
7 x 3 = 21
...
7 x 12 = 84
```

Requirements:
- Validate N is in range 1-12
- Use a for loop to generate the table
- Format output clearly

---

## Section C: Input Validation and Error Handling

### Problem 6: Create a Robust Age Input System

**Write a complete program** that:
1. Asks the user for their age
2. Validates that:
   - Age is a positive integer
   - Age is between 0 and 150
3. If invalid, displays specific error message and asks again
4. Continues until valid input is received
5. Categorizes age:
   - 0-12: Child
   - 13-17: Teen
   - 18-64: Adult
   - 65+: Senior

Example run with errors:
```
Enter your age: -5
Error: Age cannot be negative. Try again.
Enter your age: 200
Error: Age must be 0-150. Try again.
Enter your age: 25

You are 25 years old.
Category: Adult
```

Requirements:
- Use while loop for validation
- Give specific error messages
- Test with multiple invalid inputs

---

### Problem 7: Create a Password Validator

**Write a complete program** that:
1. Asks user to create a password
2. Validates the password meets requirements:
   - At least 8 characters long
   - Contains at least one digit
3. If invalid, explains which requirements are not met
4. Repeats until valid password is entered
5. Asks user to confirm password (enter it again)
6. Repeats confirmation until passwords match

Example run:
```
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

Requirements:
- Use do-while or while loops
- Check password length with `.length()`
- Check for digit with `.find('0')` through `.find('9')`
- Use nested validation (password, then confirmation)

---

## Section D: Menu-Driven Programs

### Problem 8: Create a Simple Banking System

**Write a complete program** that:
1. Starts with a balance of $1000
2. Displays a menu with options:
   - 1. Check Balance
   - 2. Deposit
   - 3. Withdraw
   - 4. Exit
3. Handles each option:
   - Check Balance: Shows current balance
   - Deposit: Asks amount, validates (positive), adds to balance
   - Withdraw: Asks amount, validates (positive, <= balance), subtracts
   - Exit: Says goodbye and exits
4. Loops until user chooses Exit

Example run:
```
=== BANKING SYSTEM ===
Balance: $1000

1. Check Balance
2. Deposit
3. Withdraw
4. Exit
Choice: 1

Current balance: $1000.00

[Menu repeats]
Choice: 2
Enter deposit amount: $250
Deposited $250.00. New balance: $1250.00

[Menu repeats]
Choice: 3
Enter withdrawal amount: $2000
Error: Insufficient funds.

Choice: 3
Enter withdrawal amount: $100
Withdrew $100.00. New balance: $1150.00

Choice: 4
Goodbye!
```

Requirements:
- Use do-while for menu loop
- Validate all inputs
- Don't allow negative amounts
- Don't allow withdrawal > balance
- Display balance with 2 decimal places

---

### Problem 9: Create a Grade Management System

**Write a complete program** that:
1. Displays a menu:
   - 1. Add Grade
   - 2. Display All Grades
   - 3. Calculate Average
   - 4. Find Highest Grade
   - 5. Find Lowest Grade
   - 6. Exit
2. Stores up to 50 grades (use counter to track how many)
3. Implements each option
4. Validates grades are 0-100
5. Loops until user exits

Example run:
```
=== GRADE MANAGEMENT ===
Grades stored: 0

1. Add Grade
2. Display All Grades
3. Calculate Average
4. Find Highest
5. Find Lowest
6. Exit
Choice: 1

Enter grade (0-100): 85
Grade added! Total grades: 1

[Menu repeats]
Choice: 1
Enter grade (0-100): 92
Grade added! Total grades: 2

Choice: 3
Average: 88.5

Choice: 6
Goodbye!
```

Requirements:
- Store grades in variables (grade1, grade2, etc.) or declare 50 variables
- Track count of grades entered
- Validate each grade is 0-100
- Handle "no grades entered" case
- Use loops to calculate average, max, min

---

### Problem 10: Create a Number Guessing Game

**Write a complete program** that:
1. Generates a random number between 1 and 100
2. Asks the user to guess the number
3. After each guess, says "Too high" or "Too low"
4. Continues until correct guess
5. Displays number of attempts
6. Asks if user wants to play again

Example run:
```
=== NUMBER GUESSING GAME ===
I'm thinking of a number between 1 and 100.

Guess: 50
Too low!

Guess: 75
Too high!

Guess: 60
Too low!

Guess: 67
Correct! You got it in 4 attempts.

Play again? (y/n): y

[New game starts]
```

Requirements:
- Use `rand() % 100 + 1` for random number (include `<cstdlib>` and `<ctime>`)
- Use `srand(time(0))` at start to seed random generator
- Track number of guesses with counter
- Use while loop for guessing
- Use do-while or while for "play again" loop

---

## Section E: Pattern Generation and Nested Loops

### Problem 11: Create a Pattern Generator Menu

**Write a complete program** with a menu offering different patterns:
1. Right Triangle
2. Inverted Triangle
3. Pyramid
4. Diamond
5. Exit

For each pattern, ask for size N and display the pattern.

**Right Triangle (N=5):**
```
*
**
***
****
*****
```

**Inverted Triangle (N=5):**
```
*****
****
***
**
*
```

**Pyramid (N=5):**
```
    *
   ***
  *****
 *******
*********
```

**Diamond (N=5):**
```
    *
   ***
  *****
 *******
*********
 *******
  *****
   ***
    *
```

Requirements:
- Use nested loops for each pattern
- Menu-driven with do-while loop
- Validate N is between 1 and 10
- Show complete patterns

---

## Section F: Data Processing and Accumulation

### Problem 12: Create a Student Grade Processor

**Write a complete program** that:
1. Asks how many students (1-30)
2. For each student:
   - Asks for name
   - Asks for test score (0-100)
   - Validates score
3. After all input, displays:
   - Class average
   - Number of A's (90+)
   - Number of B's (80-89)
   - Number of C's (70-79)
   - Number of D's (60-69)
   - Number of F's (below 60)
   - Highest scoring student
   - Lowest scoring student

Example run:
```
How many students? 3

Student 1 name: Alice
Score: 95

Student 2 name: Bob
Score: 78

Student 3 name: Charlie
Score: 88

=== CLASS REPORT ===
Class Average: 87.0

Grade Distribution:
A (90-100): 1 student(s)
B (80-89): 1 student(s)
C (70-79): 1 student(s)
D (60-69): 0 student(s)
F (0-59): 0 student(s)

Highest: Alice (95)
Lowest: Bob (78)
```

Requirements:
- Use for loop to input student data
- Track counts for each grade category
- Track max/min with names
- Calculate average with accumulator
- Validate all inputs

---

### Problem 13: Create a Prime Number Finder

**Write a complete program** that:
1. Asks user for a number N
2. Finds and displays all prime numbers from 2 to N
3. Counts how many primes were found
4. Asks if user wants to try another number

A prime number is only divisible by 1 and itself.

Example run:
```
Enter a number: 20

Prime numbers from 2 to 20:
2 3 5 7 11 13 17 19

Total: 8 prime numbers found.

Try another number? (y/n): n
Goodbye!
```

Requirements:
- Use nested loops (outer: each number, inner: check divisibility)
- To check if number `n` is prime:
  - Try dividing by all numbers from 2 to n-1
  - If any divide evenly (remainder 0), not prime
  - If none divide evenly, it's prime
- Use do-while for "try again" loop

---

## Challenge Problems (Optional)

### Challenge 1: Create a Fibonacci Sequence Generator

**Write a complete program** that:
1. Asks user how many Fibonacci numbers to generate (1-50)
2. Generates and displays that many Fibonacci numbers
3. The Fibonacci sequence: 0, 1, 1, 2, 3, 5, 8, 13, 21, ...
   - First number is 0
   - Second number is 1
   - Each subsequent number is sum of previous two

Example run:
```
How many Fibonacci numbers? 10

Fibonacci Sequence:
0 1 1 2 3 5 8 13 21 34

Would you like to generate more? (y/n): n
```

Requirements:
- Use loop to generate sequence
- Track previous two numbers
- Handle edge cases (1 or 2 numbers requested)

---

### Challenge 2: Create a Collatz Conjecture Checker

**Write a complete program** that:
1. Asks for a starting number
2. Applies the Collatz conjecture rules:
   - If even: divide by 2
   - If odd: multiply by 3 and add 1
   - Repeat until reaching 1
3. Displays each step
4. Counts how many steps it took

Example run:
```
Enter starting number: 10

Collatz Sequence:
10 → 5 → 16 → 8 → 4 → 2 → 1

Steps: 6

Try another number? (y/n): y

Enter starting number: 27
27 → 82 → 41 → 124 → 62 → 31 → 94 → 47 → 142 → 71 → 214 → 107 → 322 → 161 → 484 → 242 → 121 → 364 → 182 → 91 → 274 → 137 → 412 → 206 → 103 → 310 → 155 → 466 → 233 → 700 → 350 → 175 → 526 → 263 → 790 → 395 → 1186 → 593 → 1780 → 890 → 445 → 1336 → 668 → 334 → 167 → 502 → 251 → 754 → 377 → 1132 → 566 → 283 → 850 → 425 → 1276 → 638 → 319 → 958 → 479 → 1438 → 719 → 2158 → 1079 → 3238 → 1619 → 4858 → 2429 → 7288 → 3644 → 1822 → 911 → 2734 → 1367 → 4102 → 2051 → 6154 → 3077 → 9232 → 4616 → 2308 → 1154 → 577 → 1732 → 866 → 433 → 1300 → 650 → 325 → 976 → 488 → 244 → 122 → 61 → 184 → 92 → 46 → 23 → 70 → 35 → 106 → 53 → 160 → 80 → 40 → 20 → 10 → 5 → 16 → 8 → 4 → 2 → 1

Steps: 111
```

Requirements:
- Use while loop (unknown number of steps)
- Track and count steps
- Display sequence
- Allow multiple tries

---

### Challenge 3: Create a Text-Based Calculator with History

**Write a complete program** that:
1. Displays calculator menu:
   - 1. Add
   - 2. Subtract
   - 3. Multiply
   - 4. Divide
   - 5. View History
   - 6. Clear History
   - 7. Exit
2. For operations, asks for two numbers and displays result
3. Stores last 10 calculations in history
4. View History shows all previous calculations
5. Continues until exit

Example run:
```
=== CALCULATOR WITH HISTORY ===

1. Add
2. Subtract
3. Multiply
4. Divide
5. View History
6. Clear History
7. Exit
Choice: 1

Enter first number: 10
Enter second number: 5
Result: 10 + 5 = 15

[Menu repeats]
Choice: 3
First number: 4
Second number: 7
Result: 4 × 7 = 28

Choice: 5

=== CALCULATION HISTORY ===
1. 10 + 5 = 15
2. 4 × 7 = 28

Choice: 7
Goodbye!
```

Requirements:
- Store up to 10 history entries
- Track count of history entries
- Clear history resets to empty
- Handle division by zero

---