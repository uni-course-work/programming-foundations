# Chapter 15: Repetition and Iteration

## Introduction: Programs That Repeat

So far, every program has executed each statement **at most once**. Input is read, variables are set, decisions are made, output is displayed. But what if you need to do something multiple times?

Consider:
- Reading 100 test scores (would you write 100 separate input statements?)
- Repeating a menu until the user chooses to exit
- Validating input until the user enters something correct
- Processing a list of numbers until done

These scenarios require **repetition**—executing statements multiple times. This is where **loops** come in.

A **loop** is a control flow structure that **repeats a block of code multiple times** based on a condition. Loops are one of the most powerful programming constructs because they let you handle massive amounts of data and complex workflows without writing repetitive code.

In this chapter, we'll explore:
- **Loops as controlled repetition** (what loops do)
- **Loop purpose and termination conditions** (why loops repeat and when they stop)
- **Loop invariants** (truths that remain constant during repetition)
- **Choosing the right looping construct** (different loops for different situations)
- **Practical applications** (input validation, menus, data processing)

By the end of this chapter, you'll understand how to write programs that handle repetition elegantly and efficiently.

---

## Part 1: Loops as Controlled Repetition

### What is a Loop?

A **loop** is code that **executes repeatedly** based on a condition. Each repetition is called an **iteration**.

**Without a loop:**
```cpp
std::cout << "1" << std::endl;
std::cout << "2" << std::endl;
std::cout << "3" << std::endl;
std::cout << "4" << std::endl;
std::cout << "5" << std::endl;
```

Output: 1, 2, 3, 4, 5 (but requires 5 lines of code)

**With a loop:**
```cpp
for (int i = 1; i <= 5; i++) {
    std::cout << i << std::endl;
}
```

Output: Same, but with 3 lines of code. And if you need to print 1000 numbers, the loop still requires 3 lines.

### Why Loops Matter

Loops enable:
1. **Handling repetitive tasks** without duplicating code
2. **Processing large amounts of data** (thousands of values, not 10)
3. **Flexible repetition** (repeat N times, where N comes from input)
4. **User interaction** (repeat menu until user exits)
5. **Validation** (repeat input request until valid)

Without loops, practical programs would be impossibly long.

### The Loop Metaphor

Think of a loop like a recipe instruction: "Mix ingredients for 1 minute." That means: repeat the "mix" action over and over until 1 minute has elapsed. Loop iterations are like that: repeat the block until the condition says to stop.

---

## Part 2: Loop Purpose and Termination Conditions

### Why Loops Repeat

A loop repeats because its **termination condition** is not yet satisfied. The loop **continues executing** until the condition becomes true (or false, depending on loop type).

**Example: Print while counter < 5**
```cpp
int counter = 0;
while (counter < 5) {
    std::cout << counter << std::endl;
    counter++;
}
```

**Execution trace:**
```
Iteration 1: counter=0, check 0<5 (true), print 0, increment to 1
Iteration 2: counter=1, check 1<5 (true), print 1, increment to 2
Iteration 3: counter=2, check 2<5 (true), print 2, increment to 3
Iteration 4: counter=3, check 3<5 (true), print 3, increment to 4
Iteration 5: counter=4, check 4<5 (true), print 4, increment to 5
Check: counter=5, check 5<5 (false), exit loop
```

**Output:** 0 1 2 3 4

### Termination Conditions

A loop must have a **termination condition**—the condition that, when satisfied, causes the loop to stop. Without it, the loop runs forever (**infinite loop**).

**Example of infinite loop:**
```cpp
while (true) {
    std::cout << "This runs forever!" << std::endl;
    // No condition ever becomes false, so loop never stops
}
```

**Fixing it:**
```cpp
int count = 0;
while (count < 10) {
    std::cout << "This runs 10 times" << std::endl;
    count++;  // Eventually count reaches 10, and condition becomes false
}
```

### The Loop Must Make Progress

For a loop to terminate, something inside it must move toward the termination condition.

**Wrong (doesn't make progress):**
```cpp
int i = 0;
while (i < 5) {
    std::cout << i << std::endl;
    // i never changes! Loop runs forever
}
```

**Correct (makes progress):**
```cpp
int i = 0;
while (i < 5) {
    std::cout << i << std::endl;
    i++;  // Increment i; eventually i reaches 5
}
```

---

## Part 3: Three Types of Loops

### The while Loop

The **while loop** is the simplest and most flexible loop. It repeats as long as a condition is true.

**Syntax:**
```cpp
while (condition) {
    // Body: executes repeatedly while condition is true
}
```

**Execution:**
1. Check condition
2. If true, execute body, then go back to step 1
3. If false, exit loop

**Example:**
```cpp
int number;
std::cout << "Enter a positive number: ";
std::cin >> number;

while (number <= 0) {
    std::cout << "Invalid. Enter a positive number: ";
    std::cin >> number;
}
```

This loop repeats the input request until the user enters a positive number.

### The do-while Loop

The **do-while loop** is like while, but it **always executes at least once** before checking the condition.

**Syntax:**
```cpp
do {
    // Body: executes at least once
} while (condition);
```

**Key difference:** Condition is checked AFTER the body, not before.

**Example:**
```cpp
int choice;
do {
    std::cout << "Menu:" << std::endl;
    std::cout << "1. Start" << std::endl;
    std::cout << "2. Settings" << std::endl;
    std::cout << "3. Exit" << std::endl;
    std::cout << "Enter choice: ";
    std::cin >> choice;
} while (choice != 3);
```

This loop shows the menu at least once, then repeats until the user chooses 3.

**When to use do-while:** When the body must execute at least once (like menu loops or input validation where you need at least one attempt).

### The for Loop

The **for loop** is specialized for counting iterations. It combines initialization, condition, and increment in one line.

**Syntax:**
```cpp
for (initialization; condition; increment) {
    // Body: executes repeatedly
}
```

**Parts:**
- **initialization:** Set up variables (runs once, before loop starts)
- **condition:** Check if loop should continue (checked before each iteration)
- **increment:** Update variables (runs after each iteration)

**Example:**
```cpp
for (int i = 0; i < 5; i++) {
    std::cout << i << std::endl;
}
```

**Execution:**
1. Initialize: `int i = 0`
2. Check: `0 < 5` (true)
3. Execute body: print 0
4. Increment: `i++` (now i = 1)
5. Check: `1 < 5` (true)
6. Execute body: print 1
7. Increment: `i++` (now i = 2)
8. Continue...

**When to use for:** When you know how many times to loop (fixed count or count from input).

### Comparing the Three Loops

| Loop | When to use | Must execute once? |
|------|-------------|-------------------|
| `while` | Unknown repetitions, flexible | No |
| `do-while` | Unknown repetitions, flexible | Yes |
| `for` | Known repetitions, counting | No |

---

## Part 4: Loop Invariants

### What is a Loop Invariant?

A **loop invariant** is a statement about the program's state that is **true before, during, and after each iteration**. It's an "invariant"—a truth that doesn't change.

**Example:**
```cpp
int sum = 0;
for (int i = 1; i <= 5; i++) {
    sum += i;  // Add i to sum
}
```

**Invariant:** "After each iteration, `sum` contains the total of all integers from 1 to i."

- Before loop: `sum = 0`, and "sum of 1 to 0" is 0 ✓
- After iteration 1: `sum = 1`, "sum of 1 to 1" is 1 ✓
- After iteration 2: `sum = 3`, "sum of 1 to 2" is 1+2=3 ✓
- After iteration 3: `sum = 6`, "sum of 1 to 3" is 1+2+3=6 ✓
- After iteration 5: `sum = 15`, "sum of 1 to 5" is 1+2+3+4+5=15 ✓

The invariant is **always true**.

### Why Loop Invariants Matter

Loop invariants help you:
1. **Understand what the loop does** (the invariant describes its purpose)
2. **Verify correctness** (if invariant holds, loop is correct)
3. **Debug loops** (if invariant breaks, you found the bug)
4. **Design loops** (start by stating the invariant)

### More Examples

**Example 2: Counting iterations**
```cpp
int count = 0;
for (int i = 1; i <= 10; i++) {
    count++;
}
```

**Invariant:** "After each iteration, `count` equals the number of iterations completed."

**Example 3: Finding a maximum**
```cpp
int max_value = values[0];
for (int i = 1; i < num_values; i++) {
    if (values[i] > max_value) {
        max_value = values[i];
    }
}
```

**Invariant:** "After each iteration, `max_value` contains the largest value seen so far."

---

## Part 5: Choosing the Right Looping Construct

### Decision Tree: Which Loop?

**Question 1: Do you know how many iterations?**
- **Yes:** Use `for` loop
- **No:** Go to Question 2

**Question 2: Must the body execute at least once?**
- **Yes:** Use `do-while` loop
- **No:** Use `while` loop

### Practical Examples

**For loop (known iterations):**
```cpp
// Process 100 test scores
for (int i = 0; i < 100; i++) {
    int score;
    std::cin >> score;
    // Process score
}
```

**While loop (unknown iterations, might not execute):**
```cpp
// Read until user enters -1 (sentinel value)
int score;
std::cin >> score;

while (score != -1) {
    // Process score
    std::cin >> score;
}
```

**Do-while loop (unknown iterations, must execute at least once):**
```cpp
// Menu that always displays at least once
int choice;
do {
    std::cout << "Menu: 1=Start, 2=Exit" << std::endl;
    std::cin >> choice;
} while (choice != 2);
```

---

## Part 6: Practical Application: Input Validation

### The Input Validation Loop

One of the most common uses of loops is **input validation**—repeatedly asking the user to enter data until they enter something valid.

**Pattern:**
```cpp
type variable;
std::cout << "Enter [description]: ";
std::cin >> variable;

while (/* variable is invalid */) {
    std::cout << "Invalid. Enter [description]: ";
    std::cin >> variable;
}
```

### Example 1: Validate Range

```cpp
int age;
std::cout << "Enter your age (0-150): ";
std::cin >> age;

while (age < 0 || age > 150) {
    std::cout << "Invalid age. Enter 0-150: ";
    std::cin >> age;
}

std::cout << "You are " << age << " years old." << std::endl;
```

The loop repeats until `age` is in the valid range.

### Example 2: Validate Type or Range

```cpp
int grade;
std::cout << "Enter test grade (0-100): ";
std::cin >> grade;

while (grade < 0 || grade > 100) {
    std::cout << "Invalid grade. Enter 0-100: ";
    std::cin >> grade;
}

if (grade >= 90) {
    std::cout << "Grade: A" << std::endl;
}
// ... etc
```

### Example 3: Validate with Multiple Conditions

```cpp
double weight;
std::cout << "Enter weight (lbs): ";
std::cin >> weight;

while (weight <= 0 || weight > 1000) {
    std::cout << "Invalid. Weight must be 1-1000 lbs: ";
    std::cin >> weight;
}
```

### Better Error Messages

Give specific feedback for each invalid case:

```cpp
int choice;
do {
    std::cout << "Menu:" << std::endl;
    std::cout << "1. Start" << std::endl;
    std::cout << "2. Settings" << std::endl;
    std::cout << "3. Exit" << std::endl;
    std::cout << "Enter choice (1-3): ";
    std::cin >> choice;
    
    if (choice < 1 || choice > 3) {
        std::cout << "Invalid choice. Please enter 1, 2, or 3." << std::endl;
    }
} while (choice < 1 || choice > 3);
```

---

## Part 7: Practical Application: Menu Loops

### The Menu Pattern

A **menu loop** presents options, gets user input, and repeats until the user chooses to exit.

**Basic pattern:**
```cpp
int choice;
do {
    std::cout << "\n=== MENU ===" << std::endl;
    std::cout << "1. Option A" << std::endl;
    std::cout << "2. Option B" << std::endl;
    std::cout << "3. Exit" << std::endl;
    std::cout << "Enter choice: ";
    std::cin >> choice;
    
    if (choice == 1) {
        // Handle option A
    } else if (choice == 2) {
        // Handle option B
    } else if (choice == 3) {
        std::cout << "Goodbye!" << std::endl;
    } else {
        std::cout << "Invalid choice." << std::endl;
    }
} while (choice != 3);
```

### Why do-while for Menus?

Do-while is perfect for menus because:
1. **Menu always displays at least once** (user needs to see options)
2. **Loop repeats until user chooses exit** (choice != exit_option)
3. **Natural structure** (show menu, get input, repeat)

### Example: Complete Calculator Menu

```cpp
#include <iostream>

int main() {
    double x, y, result;
    int operation;
    
    do {
        // Display menu
        std::cout << "\n=== CALCULATOR ===" << std::endl;
        std::cout << "1. Add" << std::endl;
        std::cout << "2. Subtract" << std::endl;
        std::cout << "3. Multiply" << std::endl;
        std::cout << "4. Divide" << std::endl;
        std::cout << "5. Exit" << std::endl;
        std::cout << "Choose operation: ";
        std::cin >> operation;
        
        // Handle choice
        if (operation >= 1 && operation <= 4) {
            std::cout << "Enter first number: ";
            std::cin >> x;
            
            std::cout << "Enter second number: ";
            std::cin >> y;
            
            if (operation == 1) {
                result = x + y;
                std::cout << "Result: " << result << std::endl;
            } else if (operation == 2) {
                result = x - y;
                std::cout << "Result: " << result << std::endl;
            } else if (operation == 3) {
                result = x * y;
                std::cout << "Result: " << result << std::endl;
            } else if (operation == 4) {
                if (y != 0) {
                    result = x / y;
                    std::cout << "Result: " << result << std::endl;
                } else {
                    std::cout << "Error: Cannot divide by zero." << std::endl;
                }
            }
        } else if (operation == 5) {
            std::cout << "Goodbye!" << std::endl;
        } else {
            std::cout << "Invalid operation." << std::endl;
        }
        
    } while (operation != 5);
    
    return 0;
}
```

---

## Part 8: Combining Loops and Decisions

### Loops with Decisions Inside

Often you have a loop that processes different items based on conditions:

```cpp
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        std::cout << i << " is even" << std::endl;
    } else {
        std::cout << i << " is odd" << std::endl;
    }
}
```

Output:
```
1 is odd
2 is even
3 is odd
4 is even
5 is odd
6 is even
7 is odd
8 is even
9 is odd
10 is even
```

### Loops with break and continue

**break:** Exit the loop immediately

```cpp
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break;  // Exit loop when i reaches 5
    }
    std::cout << i << " ";
}
// Output: 0 1 2 3 4
```

**continue:** Skip to the next iteration

```cpp
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        continue;  // Skip iteration when i == 5
    }
    std::cout << i << " ";
}
// Output: 0 1 2 3 4 6 7 8 9  (5 is skipped)
```

---

## Part 9: Nested Loops

### Loops Inside Loops

You can put a loop inside another loop:

```cpp
// Print a multiplication table
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        std::cout << i * j << " ";
    }
    std::cout << std::endl;
}
```

Output:
```
1 2 3
2 4 6
3 6 9
```

**Execution:**
- Outer loop iteration 1: i=1
  - Inner loop: j=1,2,3 (prints 1 2 3)
- Outer loop iteration 2: i=2
  - Inner loop: j=1,2,3 (prints 2 4 6)
- Outer loop iteration 3: i=3
  - Inner loop: j=1,2,3 (prints 3 6 9)

Nested loops are useful for:
- **2D structures** (rows and columns)
- **Patterns** (triangles, grids)
- **Combinations** (every item paired with every other)

---

## Part 10: Common Loop Mistakes

### Mistake 1: Infinite Loop

```cpp
// WRONG: condition never becomes false
while (true) {
    std::cout << "Enter number: ";
    int x;
    std::cin >> x;
    // Loop never exits!
}

// CORRECT
while (x != -1) {
    std::cout << "Enter number (-1 to exit): ";
    std::cin >> x;
}
```

### Mistake 2: Off-by-One Error

```cpp
// WRONG: prints 1-4, not 1-5
for (int i = 1; i < 5; i++) {
    std::cout << i << std::endl;
}

// CORRECT: prints 1-5
for (int i = 1; i <= 5; i++) {
    std::cout << i << std::endl;
}
```

### Mistake 3: Loop Doesn't Progress

```cpp
// WRONG: counter never changes
int i = 0;
while (i < 10) {
    std::cout << "Hello";
    // i never increments! Infinite loop
}

// CORRECT
int i = 0;
while (i < 10) {
    std::cout << "Hello";
    i++;
}
```

### Mistake 4: Wrong Loop Type

```cpp
// INEFFICIENT: could be a for loop
int count = 0;
while (count < 100) {
    // Process
    count++;
}

// BETTER: for loop is clearer
for (int count = 0; count < 100; count++) {
    // Process
}
```

---

## Part 11: Loops and Variables

### Loop Variables (Counter)

The loop counter (like `i` in `for (int i = 0; i < 5; i++)`) has scope limited to the loop:

```cpp
for (int i = 0; i < 5; i++) {
    std::cout << i << std::endl;
}
// i no longer exists here; error if you try to use it
```

### Accumulators (Running Totals)

Variables that accumulate values across iterations:

```cpp
int sum = 0;  // Must be outside loop
for (int i = 1; i <= 10; i++) {
    sum += i;  // Add to sum in each iteration
}
std::cout << "Sum: " << sum << std::endl;  // Prints 55
```

### State Variables (Tracking Changes)

Variables that track information across iterations:

```cpp
int max_score = 0;
for (int i = 0; i < 10; i++) {
    int score;
    std::cin >> score;
    
    if (score > max_score) {
        max_score = score;  // Update maximum
    }
}
std::cout << "Highest score: " << max_score << std::endl;
```

---

## Part 12: Loop Summary and Patterns

### Quick Reference: Three Loop Types

| Type | Syntax | When to use |
|------|--------|------------|
| **for** | `for (init; cond; inc)` | Known repetitions, counting |
| **while** | `while (condition)` | Unknown reps, might not execute |
| **do-while** | `do { } while (condition)` | Unknown reps, must execute once |

### Common Loop Patterns

**Pattern 1: Count from 0 to N-1**
```cpp
for (int i = 0; i < n; i++) { ... }
```

**Pattern 2: Count from 1 to N**
```cpp
for (int i = 1; i <= n; i++) { ... }
```

**Pattern 3: Repeat until condition**
```cpp
while (condition) { ... }
```

**Pattern 4: Input validation**
```cpp
while (invalid) { get input; }
```

**Pattern 5: Menu loop**
```cpp
do { show menu; } while (not exit);
```

---

## Looking Ahead to Chapter 17

Now that you understand repetition, the 17th chapter will introduce **arrays and data structures**. You'll learn:
- How to store multiple values efficiently
- How to process collections of data with loops
- How arrays enable powerful programs

Loops enable repetition. Arrays enable managing large amounts of data. Together, they're the foundation of practical programming.

---

## Checklist: Before Using Loops

Before writing a loop, ask yourself:

- [ ] Do I need repetition? (Could a simple if solve this?)
- [ ] Do I know how many repetitions? (for) or unknown? (while/do-while)
- [ ] Must the body execute at least once? (do-while) or might not? (while)
- [ ] What's my termination condition?
- [ ] Does my loop make progress toward termination?
- [ ] Are there any edge cases (0 iterations, 1 iteration, many iterations)?
- [ ] Have I tested with boundary values?
- [ ] Is there an infinite loop risk?

If you can answer these confidently, your loop is well-designed.
