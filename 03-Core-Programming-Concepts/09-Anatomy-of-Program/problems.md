# Chapter 9: Problems

## Section A: Program Structure

### Problem 1: Deconstructing a Complete Program

Look at this program and answer the following questions:

```cpp
#include <iostream>

int main() {
    std::cout << "Welcome to C++!" << std::endl;
    std::cout << "Your first program." << std::endl;
    return 0;
}
```

1. What would happen if you removed the line `#include <iostream>`? Why?
2. Why is `int` before `main()`? What does it mean?
3. If you changed `return 0;` to `return 1;`, what would change in how the program executes? What would stay the same?
4. Can you move the `return 0;` statement to before the `std::cout` statements? Why or why not?
5. What is the purpose of the curly braces `{ }` in the `int main() { }`?

---

### Problem 2: Building a Program from Scratch

Write a complete, working C++ program that:
- Includes the necessary library for output
- Has a main() function that returns an integer
- Outputs exactly three lines:
  - "Line 1"
  - "Line 2"
  - "Line 3"

After writing it, explain what each part of your program does.

---

### Problem 3: Understanding Namespaces and Modern Practice

Compare these two programs:

**Program A:**
```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Hello" << endl;
    return 0;
}
```

**Program B:**
```cpp
#include <iostream>

int main() {
    std::cout << "Hello" << std::endl;
    return 0;
}
```

Both compile and produce the same output. Explain:
1. What is the practical difference between them?
2. Why would a professional prefer Program B?
3. When might you see Program A used?
4. Write a third version using selective `using` declarations. When would you use this approach?

---

## Section B: Statements and Expressions

### Problem 4: The Statement-Expression Boundary

For each of the following, determine if it is a statement or an expression. Then explain what makes it one or the other:

1. `10 + 5`
2. `10 + 5;`
3. `int x = 10 + 5;`
4. `std::cout << 10 + 5;`
5. `std::cout << 10 + 5;` (with semicolon)
6. `x = 10;`

For the ones that are statements, explain what they do. For the ones that are expressions, explain what they evaluate to.

---

### Problem 5: When Does an Expression Matter?

Consider these two code snippets:

**Snippet A:**
```cpp
#include <iostream>

int main() {
    5 + 3;
    std::cout << "Done" << std::endl;
    return 0;
}
```

**Snippet B:**
```cpp
#include <iostream>

int main() {
    int result = 5 + 3;
    std::cout << result << std::endl;
    return 0;
}
```

1. What is the output of each program?
2. In Snippet A, why is `5 + 3;` essentially useless?
3. In Snippet B, why does wrapping the expression in a variable assignment make it useful?
4. Rewrite Snippet A to actually use the result of `5 + 3` in a meaningful way.

---

## Section C: Linear Execution and Program State

### Problem 6: Tracing Execution and State Changes

Without running this program, predict its exact output and trace the state of the variable `x` at each step:

```cpp
#include <iostream>

int main() {
    std::cout << "Start" << std::endl;
    int x = 10;
    std::cout << x << std::endl;
    x = x - 3;
    std::cout << x << std::endl;
    x = x + 5;
    std::cout << x << std::endl;
    return 0;
}
```

Create a table showing:
- Each line of code
- The state of `x` after that line (if changed)
- Any output produced

Then predict the final output of the program.

---

### Problem 7: The Critical Importance of Order

Here is a program with a logic error:

```cpp
#include <iostream>

int main() {
    std::cout << "The value is: " << x << std::endl;
    int x = 5;
    return 0;
}
```

This program will not compile. Answer:
1. Why does it fail to compile? (Hint: It's not a typo)
2. How would you fix it?
3. Explain why the ORDER of statements matters in this case.
4. What principle of linear execution does this demonstrate?

---

## Section D: Comments as Communication

### Problem 8: Writing Effective Comments

Below is code with poor comments. Rewrite it with better comments that actually help a reader understand the PURPOSE of the code, not just what it does:

```cpp
#include <iostream>

int main() {
    // Create an integer
    int value = 100;
    
    // Print the value
    std::cout << value << std::endl;
    
    // Subtract 25 from the value
    value = value - 25;
    
    // Print the value again
    std::cout << value << std::endl;
    
    return 0;
}
```

After rewriting, explain what makes your version better than the original.

---

### Problem 9: When Comments Are (and Aren't) Necessary

For each of these code snippets, decide: Does this code need a comment? If yes, write a helpful comment. If no, explain why the code is self-explanatory:

1. 
```cpp
int age = 25;
```

2. 
```cpp
int daysInWeek = 7;
```

3. 
```cpp
int offsetValue = 3;  // Why is this 3?
```

4. 
```cpp
std::cout << "Enter your name: ";
```

5. 
```cpp
// This loops 10 times
for (int i = 0; i < 10; i++) {
    std::cout << i << std::endl;
}
```

---

## Section E: Complete Program Analysis and Construction

### Problem 10: Analyzing a Real Program

Study this program carefully:

```cpp
#include <iostream>

int main() {
    std::cout << "Step 1" << std::endl;
    int counter = 5;
    std::cout << "Counter is: " << counter << std::endl;
    counter = counter + 1;
    std::cout << "Counter is now: " << counter << std::endl;
    return 0;
}
```

Answer these questions:
1. List every statement in this program (in order)
2. Identify which lines modify the program's state and how
3. Predict the complete output
4. If you moved the line `int counter = 5;` to after the first `std::cout`, what would happen? Why?
5. This program demonstrates three key concepts from the chapter. What are they?

---

### Problem 11: Building Programs from Specification

Write a complete C++ program that:
- Includes the iostream library
- Defines main() with the correct return type
- Outputs the following exactly:
  ```
  Starting calculation
  Result: 42
  Program complete
  ```
- Has at least one comment explaining what the program does
- Returns 0 to the operating system

Explain your design choices: Why did you structure it this way?

---

## Section F: Understanding Compilation and Errors

### Problem 12: Diagnosing Compilation Errors

Each of these programs has ONE compilation error. For each:
1. Identify what the error is (be specific)
2. Explain why the compiler rejects it
3. Show the corrected code
4. Explain what you changed and why that fixes it

**Program A:**
```cpp
#include <iostream>

int main() {
    std::cout << "Hello World!" << std::endl
    return 0;
}
```

**Program B:**
```cpp
int main() {
    std::cout << "Hello World!" << std::endl;
    return 0;
}
```

**Program C:**
```cpp
#include <iostream>

int main() {
    cout << "Hello World!" << endl;
    return 0;
}
```

---

### Problem 13: From Concept to Working Code

You want to write a program that demonstrates these concepts:
- Linear execution (multiple statements in sequence)
- Program state (a variable that changes)
- Output (displaying information)
- Comments (explaining the purpose)

Design and write this program. It should:
- Have at least 4 statements (not counting the return)
- Modify a variable at least once
- Have meaningful comments
- Produce clear, readable output

After writing it, explain how your program demonstrates each of the four concepts above.

---

## Challenge Problems (Optional)

### Challenge 1: Reverse Engineering

This program has been intentionally written in a confusing way:

```cpp
#include <iostream>

int main() {
    std::cout << "C" << std::endl;
    std::cout << "B" << std::endl;
    std::cout << "A" << std::endl;
    return 0;
}
```

1. What is the output?
2. Rewrite this program to output A, B, C in that order (without changing the logic, only moving statements)
3. Why does changing the order of statements change the output? What does this tell you about how C++ works?

---

### Challenge 2: Understanding State

Write a program that:
1. Starts with a variable set to 0
2. Adds 5 to it
3. Multiplies by 2
4. Subtracts 3
5. Displays the final result

Trace through your program to verify the math is correct. Explain what the final value should be BEFORE you write the code.

---

### Challenge 3: The Minimal Complete Program

What is the absolute minimum code you need to write a C++ program that:
- Compiles without errors
- Runs without errors
- Does nothing else

Write it. Then explain why each part (including each character) is necessary.

---

## Summary of Problem Themes

| Problem # | Theme | Key Learning |
|-----------|-------|--------------|
| 1-3 | Program structure | What goes where and why |
| 4-5 | Statement vs. expression | Fundamental difference in meaning |
| 6-7 | Linear execution & state | Order matters; declare before use |
| 8-9 | Comments | When and how to communicate with readers |
| 10-11 | Analysis & construction | Apply all concepts together |
| 12-13 | Compilation & common errors | Understand what compiler expects |
| C1-C3 | Deep understanding | Master core concepts through challenge |

---

## Progression

- **Problems 1-3:** Understand structure and syntax
- **Problems 4-5:** Distinguish fundamental concepts
- **Problems 6-7:** Understand program behavior
- **Problems 8-9:** Develop good communication habits
- **Problems 10-11:** Combine everything
- **Problems 12-13:** Troubleshoot and fix
- **Challenges:** Master the material deeply
