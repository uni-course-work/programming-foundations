# Chapter 11: Problems

## Section A: Understanding Variables as Named Bindings

### Problem 1: The Nature of Variables

Consider this code:

```cpp
#include <iostream>

int main() {
    int age = 25;
    age = 30;
    age = 35;
    
    return 0;
}
```

1. When the variable `age` is created, what happens in computer memory?
2. When `age = 30;` executes, what changes and what stays the same?
3. Why is the term "binding" (rather than just "variable") meaningful for understanding what `age` represents?
4. If the memory address of `age` is `0x7fff5fbff8c4`, does knowing this address matter to the programmer? Why or why not?
5. Explain: What is the relationship between the **name** `age`, the **type** `int`, and the **value** 25?

---

### Problem 2: Declaration vs. Initialization—Understanding the Difference

Below are three scenarios. For each, explain what happens and what the risks are:

**Scenario A:**
```cpp
int score;
score = 95;
std::cout << score << std::endl;
```

**Scenario B:**
```cpp
int score = 0;
score = 95;
std::cout << score << std::endl;
```

**Scenario C:**
```cpp
int score = 95;
std::cout << score << std::endl;
```

For each scenario:
1. When is the variable created?
2. When does it get a value?
3. What is the risk or danger (if any)?
4. Which scenario represents best practice and why?

---

### Problem 3: The Critical Danger of Uninitialized Variables

Imagine you're writing a banking application:

```cpp
#include <iostream>

int main() {
    double accountBalance;  // Not initialized
    
    std::cout << "Your balance: $" << accountBalance << std::endl;
    
    return 0;
}
```

1. What value will `accountBalance` display?
2. Why can't the compiler guarantee what value is displayed?
3. Write a concrete scenario where this bug could cause a serious problem for users.
4. How would you fix this code?
5. If you fixed it by initializing to 0, might that create a different problem? Explain.

---

## Section B: Scope, Lifetime, and Variable Existence

### Problem 4: Understanding Scope and Lifetime

Study this code:

```cpp
#include <iostream>

int main() {
    int x = 5;
    std::cout << x << std::endl;    // Line 1
    
    int y = 10;
    std::cout << x << y << std::endl; // Line 2
    
    return 0;
}
```

1. At what point in the program does variable `x` come into existence?
2. At what point does variable `x` cease to exist?
3. At what point does variable `y` come into existence?
4. Why can you use both `x` and `y` on Line 2?
5. If you tried to use `x` or `y` after the closing `}` of main(), what would happen?

---

### Problem 5: Scope Prevents Problems

Without scope, this code would be problematic:

```cpp
void calculateSales() {
    int total = 1000;  // total in this context
    // ...use total...
}

void calculateExpenses() {
    int total = 500;   // total in this context
    // ...use total...
}
```

1. If both functions existed in the same global scope (instead of separate function scopes), what problem would occur?
2. How does scope solve this problem?
3. Why is it safe for both functions to have a variable named `total`?
4. Explain: How does scope enable code organization and prevent name collisions?

---

## Section C: Professional Naming and Communication

### Problem 6: The Power of Good Naming

Compare these two versions of the same program:

**Version A (Poor Naming):**
```cpp
int x = 20;
int y = 8;
int z = x + y;
std::cout << z << std::endl;
```

**Version B (Good Naming):**
```cpp
int hoursWorked = 20;
int hourlyRate = 8;
int totalWages = hoursWorked + hourlyRate;
std::cout << totalWages << std::endl;
```

Wait—Version B has a bug (should be multiplication, not addition). But:

1. Which version would make the bug more obvious to a reader?
2. How does the name `totalWages` communicate something that `z` doesn't?
3. Write a third version that fixes the calculation. Why is it even clearer?
4. Explain: How do good names function as documentation?
5. In what way do good names make code maintainability easier?

---

### Problem 7: Naming Boolean Variables

For each of these variables, decide:
- Is the name good for a boolean?
- If not, suggest a better name

1. `bool complete;`
2. `bool isLoggedIn;`
3. `bool student;`
4. `bool hasPermission;`
5. `bool valid;`
6. `bool shouldRetry;`
7. `bool enrolled;`
8. `bool activeUser;`

For each one you consider "not good," explain what's unclear and why your suggestion is better.

---

## Section D: Practical Variable Design and Mistakes

### Problem 8: Choosing Types, Names, and Initial Values

You're building a student information system. For each piece of data, design a variable by specifying:
- The type
- The name (following camelCase)
- The initialization value

The data elements are:
1. Number of students in the class (currently 0)
2. Whether a student is currently enrolled (currently yes)
3. A student's GPA (you don't have the actual value yet, so use a safe default)
4. The letter grade received in the course (currently unknown, use safe default)
5. The semester (currently Spring 2024)

For each one, explain your choices for type, name, and initial value. What would be a **bad** choice for each element?

---

### Problem 9: Avoiding Common Variable Mistakes

Identify the mistake(s) in each code snippet and explain why it's problematic:

**Snippet 1:**
```cpp
int age;
std::cout << age << std::endl;
```

**Snippet 2:**
```cpp
int x = 5;
int x = 10;
std::cout << x << std::endl;
```

**Snippet 3:**
```cpp
int count;
std::cout << "Enter count: ";
// (no input code here)
std::cout << count << std::endl;
```

**Snippet 4:**
```cpp
double temperature = 98;
bool isEmpty = 0;
int isActive = true;
```

**Snippet 5:**
```cpp
int myVariable = 42;
int myvariable = 100;
std::cout << myVariable << std::endl;
```

For each, explain the mistake and show the corrected version.

---

## Section E: Integration and Design

### Problem 10: Complete Variable Design for a Real Program

You're building a program that tracks book information for a library. Design variables for:

1. The book's ISBN (fixed 13-digit number)
2. The book's title (we'll pretend this is a character for now)
3. The number of copies in stock
4. Whether the book is available for checkout
5. The average reader rating (0.0 to 5.0)
6. The year it was published
7. Whether it's a bestseller

For each variable, specify:
- Type
- Name (camelCase)
- Initial value
- **Why this is the best choice** (2-3 sentences explaining your reasoning)
- **What would go wrong** with a different type/name choice

---

### Problem 11: Scope and Program Organization

Consider this incomplete program:

```cpp
#include <iostream>

int main() {
    int studentID = 12345;
    char grade = 'A';
    
    // Point 1: studentID and grade both accessible here
    
    // Imagine more code here that uses studentID and grade
    
    // Point 2: studentID and grade still accessible here
    
    return 0;
}
// Point 3: What happens here?
```

1. At Point 1, are `studentID` and `grade` accessible? Explain.
2. At Point 2, are they still accessible? Explain.
3. At Point 3 (after the closing `}`), are they still accessible? What happens if you try to use them?
4. Why is the closing `}` of main() significant for variable lifetime?
5. In a larger program with multiple functions, why is it important that each function has its own scope?

---

### Problem 12: Type, Name, and Initialization Working Together

Analyze this poorly written code and rewrite it well:

```cpp
#include <iostream>

int main() {
    int x;
    double y;
    bool z;
    char a;
    
    // Later in the program (after some other code):
    x = 25;
    y = 98.6;
    z = 1;
    a = 'J';
    
    std::cout << x << std::endl;
    std::cout << y << std::endl;
    std::cout << z << std::endl;
    std::cout << a << std::endl;
    
    return 0;
}
```

Rewrite this code to follow best practices. In your rewritten version:
1. Use meaningful variable names
2. Initialize all variables at declaration
3. Group related variables together
4. Add a comment explaining the program's purpose
5. Explain why your version is better than the original

---

## Challenge Problems (Optional)

### Challenge 1: Variable Lifetime and Memory

At the machine level, memory is a sequence of bytes. Consider this scenario:

```cpp
int main() {
    int x = 42;      // 4 bytes allocated for x
    double y = 3.14; // 8 bytes allocated for y
    
    // ... use x and y ...
    
    return 0;
}
```

1. When `x` is declared, what happens to those 4 bytes?
2. When `x` goes out of scope, what happens to those 4 bytes?
3. Could those 4 bytes be reused by a different program?
4. If a second program later runs and uses the same memory location, would it see the value 42? Explain.
5. Why is initialization important in light of this memory reuse?

---

### Challenge 2: Scope and Name Reuse

You're writing a library management system. Multiple functions need a variable named `count`:

```cpp
void countStudents() {
    int count = 0;      // count of students
    // ...
}

void countBooks() {
    int count = 0;      // count of books
    // ...
}

void countStaff() {
    int count = 0;      // count of staff
    // ...
}
```

1. Is it safe for all three functions to have a variable named `count`? Why?
2. What would happen if all three `count` variables were in the same scope instead of separate functions?
3. Design an alternative naming scheme where each `count` has a more specific name. When might this be better than reusing `count`?
4. Explain: When is reusing a name (via scope) good practice, and when should you use different names?

---

### Challenge 3: The Cost of Uninitialized Variables—Real Consequences

Suppose you're writing the score tracking system for a large online game:

```cpp
int playerScore;  // Not initialized

if (playerScore > 1000) {
    std::cout << "High score achieved!" << std::endl;
}
```

1. What could happen when this code runs?
2. Describe a scenario where an uninitialized variable could:
   - Give false hope (player thinks they have a high score when they don't)
   - Cause false alarm (player thinks score is dangerously low when it isn't)
   - Cause a security vulnerability (player somehow exploits the uninitialized value)
3. How widespread would this bug be? (Would it happen every time, sometimes, or randomly?)
4. What's particularly dangerous about uninitialized variables in decision-making code (if/else)?

---