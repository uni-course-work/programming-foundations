# Chapter 14: Decision Making and Control Flow

## Introduction: Programs That Think and React

So far, every program you've written has been **linear**: it reads input, computes values, and displays output, following the same sequence every time. The order of statements is always: statement 1, then statement 2, then statement 3, and so on.

But real programs need to **react to situations**. They need to make decisions. If a student's grade is above 90, congratulate them. If it's below 60, suggest tutoring. If it's in between, offer encouragement. Different situations require different responses.

This is where **control flow** comes in. Control flow is how a program **alters its execution path based on conditions**. Instead of always following the same sequence, the program follows one of multiple paths depending on what data it has and what decisions it makes.

In this chapter, we'll explore:
- **Conditional execution** (if statements and their variations)
- **Boolean logic** (how conditions are evaluated)
- **Mutually exclusive paths** (when only one path can execute)
- **Exhaustive decision handling** (covering all cases)
- **Branching logic** (how programs think about situations)

By the end of this chapter, you'll understand how to write programs that aren't just executors of fixed sequences, but intelligent decision-makers that adapt to circumstances.

---

## Part 1: Branching Logic and Program State

### What is Control Flow?

**Control flow** determines **which statements execute and in what order**. Without control flow, every program is just a sequence. With control flow, a program can:
- Skip statements
- Repeat statements
- Choose between alternatives
- Make decisions based on data

### Program State and Decisions

Every program at any moment has **state**: the current values of its variables. Decisions are made **based on this state**.

```cpp
int age = 25;
double gpa = 3.8;
bool hasInternship = true;

// The program's state is: age=25, gpa=3.8, hasInternship=true
// Based on this state, different things should happen
```

A **decision** is a question asked about the state:
- "Is age >= 21?"
- "Is GPA > 3.5 AND has internship?"
- "Is score in range 90-100?"

Based on the answer (true or false), the program takes different paths.

### The Branching Metaphor

Think of a program like a tree with branches:

```
        START
         |
    Is age >= 21?
       /     \
     YES      NO
     /         \
  Print        Print
  "Adult"      "Minor"
     |           |
    END
```

The program evaluates the condition ("Is age >= 21?"), then follows ONE of the two branches depending on the answer. Different data leads to different paths.

### Why Control Flow Matters

Without control flow:
```cpp
std::cout << "Congratulations!";  // Always shows
std::cout << "Try harder next time";  // Always shows
```

Nonsensical—both print regardless of performance.

With control flow:
```cpp
if (score >= 90) {
    std::cout << "Congratulations!";  // Only if score is high
} else {
    std::cout << "Try harder next time";  // Only if score is low
}
```

Now the output matches the situation. The program **reacts intelligently**.

---

## Part 2: Conditional Execution with if

### The Basic if Statement

An **if statement** executes a block of code **only if a condition is true**:

```cpp
if (age >= 21) {
    std::cout << "You can vote." << std::endl;
}
```

**How it works:**
1. Evaluate the condition: `age >= 21`
2. If true, execute the block inside braces `{ ... }`
3. If false, skip the block entirely
4. Continue with the rest of the program

### if-else: Two Paths

An **if-else statement** provides **two mutually exclusive paths**: one for true, one for false:

```cpp
if (score >= 60) {
    std::cout << "You passed!" << std::endl;
} else {
    std::cout << "You failed." << std::endl;
}
```

**Execution:**
- If `score >= 60` is true: execute first block, skip second block
- If `score >= 60` is false: skip first block, execute second block
- **Exactly one path executes** (never both, never neither)

### Chaining: if-else if-else if-else

When you have **multiple mutually exclusive cases**, use chaining:

```cpp
if (grade == 'A') {
    std::cout << "Excellent!" << std::endl;
} else if (grade == 'B') {
    std::cout << "Good!" << std::endl;
} else if (grade == 'C') {
    std::cout << "Satisfactory." << std::endl;
} else {
    std::cout << "Needs improvement." << std::endl;
}
```

**How it works:**
1. Check first condition (`grade == 'A'`)
2. If true, execute that block and skip the rest
3. If false, check next condition (`grade == 'B'`)
4. Keep checking until one is true or all fail
5. If all fail, execute the final `else` block (if present)

**Key point:** Only ONE block executes. As soon as a condition is true, that block runs and the rest are skipped.

### The Structure of if Statements

```cpp
if (condition) {
    // Block 1: executes if condition is true
} else if (other_condition) {
    // Block 2: executes if condition is false AND other_condition is true
} else {
    // Block 3: executes if all conditions are false
}
```

- `if` is **required**
- `else if` is **optional** (can have zero or many)
- `else` is **optional** (at most one)

### Nested if Statements

You can put if statements inside other if statements:

```cpp
if (age >= 21) {
    if (hasLicense) {
        std::cout << "You can drive." << std::endl;
    } else {
        std::cout << "You need a license." << std::endl;
    }
} else {
    std::cout << "You're too young to drive." << std::endl;
}
```

This checks **two conditions in sequence**: first age, then license.

---

## Part 3: Boolean Logic and Conditions

### Simple Conditions

A **simple condition** is a single comparison:

```cpp
age >= 21
score == 100
name != "John"
```

Each produces a boolean (true or false).

### Complex Conditions: Combining with && and ||

You can combine simple conditions using **logical operators**:

**AND (&&): Both must be true**
```cpp
if (age >= 21 && hasLicense) {
    std::cout << "You can drive." << std::endl;
}
```

Executes only if BOTH conditions are true. If either is false, the whole condition is false.

**OR (||): At least one must be true**
```cpp
if (isStudent || hasHardship) {
    std::cout << "You qualify for assistance." << std::endl;
}
```

Executes if at least one condition is true. Only skips if both are false.

**NOT (!): Opposite**
```cpp
if (!isRaining) {
    std::cout << "Go outside." << std::endl;
}
```

Executes if the condition is false. Flips the meaning.

### Order of Conditions and De Morgan's Laws

These two expressions are equivalent:

```cpp
// Version 1
if (age < 18) {
    std::cout << "Cannot vote." << std::endl;
}

// Version 2
if (!(age >= 18)) {
    std::cout << "Cannot vote." << std::endl;
}
```

**De Morgan's Laws** describe how to negate complex conditions:
- `!(A && B)` is equivalent to `(!A || !B)`
- `!(A || B)` is equivalent to `(!A && !B)`

In other words, when you negate AND, it becomes OR (and vice versa), and each condition flips.

### Reading Complex Conditions

For complex conditions, use parentheses to clarify:

```cpp
// HARD TO READ
if (age >= 21 && hasLicense && !isDistracted || hasPermission) {
    // What does this mean?
}

// CLEAR
if ((age >= 21 && hasLicense && !isDistracted) || hasPermission) {
    // Either: adult with license and not distracted
    // Or: has special permission
}
```

Parentheses make logic explicit and prevent bugs.

---

## Part 4: Mutually Exclusive Paths

### What Are Mutually Exclusive Paths?

**Mutually exclusive paths** are branches where **only one can execute**. No two paths run simultaneously; exactly one path always executes (or possibly zero if all conditions are false).

### if-else: Two Mutually Exclusive Paths

```cpp
if (score >= 60) {
    std::cout << "Passed." << std::endl;       // Path 1
} else {
    std::cout << "Failed." << std::endl;       // Path 2
}
```

**Mutually exclusive:** Exactly one path executes.
- If score is 75: first path (Passed)
- If score is 45: second path (Failed)
- **Never both**

### if-else if-else if-else: Multiple Mutually Exclusive Paths

```cpp
if (score >= 90) {
    std::cout << "A" << std::endl;             // Path 1
} else if (score >= 80) {
    std::cout << "B" << std::endl;             // Path 2
} else if (score >= 70) {
    std::cout << "C" << std::endl;             // Path 3
} else if (score >= 60) {
    std::cout << "D" << std::endl;             // Path 4
} else {
    std::cout << "F" << std::endl;             // Path 5
}
```

**Mutually exclusive:** Exactly one path executes (assuming scores are 0-100).
- Score 95: Path 1
- Score 85: Path 2
- Score 75: Path 3
- etc.
- **Exactly one always executes**

### The Danger of Non-Mutually-Exclusive Code

If you use multiple separate if statements (not if-else), they're **not mutually exclusive**:

```cpp
// WRONG: Multiple separate if statements
if (score >= 90) {
    std::cout << "Great job!" << std::endl;
}
if (score >= 80) {
    std::cout << "Good effort." << std::endl;  // Might print even if score is 95!
}
```

If score is 95:
- First if: true, prints "Great job!"
- Second if: also true, prints "Good effort!"
- **Both print!** That's probably not intended.

**Fix: Use if-else if:**
```cpp
if (score >= 90) {
    std::cout << "Great job!" << std::endl;
} else if (score >= 80) {
    std::cout << "Good effort." << std::endl;  // Only if score is 80-89
}
```

Now score 95 only prints "Great job!", not both.

---

## Part 5: Exhaustive Decision Handling

### What is Exhaustive Handling?

**Exhaustive handling** means your if-else-if chain **covers all possible cases**. No matter what the input is, exactly one path executes (or you intentionally handle that some cases skip).

### Example: Grade Letter to Points

```cpp
char grade;
std::cin >> grade;

int points;
if (grade == 'A') {
    points = 4;
} else if (grade == 'B') {
    points = 3;
} else if (grade == 'C') {
    points = 2;
} else if (grade == 'D') {
    points = 1;
} else if (grade == 'F') {
    points = 0;
} else {
    std::cout << "Invalid grade." << std::endl;
    points = -1;  // Sentinel value indicating error
}

std::cout << "Points: " << points << std::endl;
```

This covers:
- A → 4
- B → 3
- C → 2
- D → 1
- F → 0
- Anything else → Error

**All cases are handled.** No matter what character the user enters, one of the branches executes.

### Exhaustive vs. Non-Exhaustive

**Non-exhaustive (has unhandled cases):**
```cpp
int age;
std::cin >> age;

if (age < 13) {
    std::cout << "Child." << std::endl;
} else if (age < 18) {
    std::cout << "Teen." << std::endl;
}
// What if age >= 18? No branch handles it!
```

**Exhaustive (all cases handled):**
```cpp
int age;
std::cin >> age;

if (age < 13) {
    std::cout << "Child." << std::endl;
} else if (age < 18) {
    std::cout << "Teen." << std::endl;
} else {
    std::cout << "Adult." << std::endl;  // Catches age >= 18
}
```

### When to Leave Cases Unhandled

Sometimes you **intentionally don't handle all cases**:

```cpp
if (itemPrice < 0) {
    std::cout << "Error: Invalid price." << std::endl;
    return;  // Exit early if invalid
}

// Rest of program only runs if price is valid
// We don't have an else; we just skip everything if condition is false
```

This is fine when the unhandled case means "skip this processing" or "error condition."

---

## Part 6: Common Decision Patterns

### Pattern 1: Validation and Early Exit

Check for invalid input and exit early:

```cpp
int age;
std::cin >> age;

if (age < 0 || age > 150) {
    std::cout << "Invalid age." << std::endl;
    return;  // Exit the program
}

// Rest of program only executes if age is valid
std::cout << "You are " << age << " years old." << std::endl;
```

This pattern **fails fast**—immediately reject invalid input before processing.

### Pattern 2: Nested Decisions (Step by Step)

Check conditions in sequence:

```cpp
if (age >= 21) {
    if (hasLicense) {
        if (hasInsurance) {
            std::cout << "You can drive." << std::endl;
        } else {
            std::cout << "You need insurance." << std::endl;
        }
    } else {
        std::cout << "You need a license." << std::endl;
    }
} else {
    std::cout << "You're too young." << std::endl;
}
```

Each level checks a prerequisite. All requirements must be met.

**Alternative using AND:**
```cpp
if (age >= 21 && hasLicense && hasInsurance) {
    std::cout << "You can drive." << std::endl;
} else {
    std::cout << "You cannot drive." << std::endl;
}
```

Simpler for checking "all of these must be true."

### Pattern 3: Range Checking

Check if a value falls in a range:

```cpp
int score;
std::cin >> score;

if (score >= 90 && score <= 100) {
    std::cout << "A" << std::endl;
} else if (score >= 80 && score < 90) {
    std::cout << "B" << std::endl;
} else if (score >= 70 && score < 80) {
    std::cout << "C" << std::endl;
} else if (score >= 60 && score < 70) {
    std::cout << "D" << std::endl;
} else if (score >= 0 && score < 60) {
    std::cout << "F" << std::endl;
} else {
    std::cout << "Invalid score." << std::endl;
}
```

Each branch checks if the value is in a specific range.

### Pattern 4: Status-Based Actions

Different actions based on current state:

```cpp
std::string status;
std::cin >> status;

if (status == "pending") {
    std::cout << "Request is waiting." << std::endl;
} else if (status == "approved") {
    std::cout << "Request was approved!" << std::endl;
} else if (status == "rejected") {
    std::cout << "Request was denied." << std::endl;
} else {
    std::cout << "Unknown status." << std::endl;
}
```

Match input to appropriate response.

---

## Part 7: Decision Trees and Planning

### What is a Decision Tree?

A **decision tree** is a visual representation of decision logic:

```
                    START
                     |
            Is score >= 90?
               /          \
            YES             NO
             |               |
          Print "A"   Is score >= 80?
             |           /        \
             |        YES          NO
             |         |            |
             |      Print "B"   Is score >= 70?
             |         |           /      \
             |         |        YES        NO
             |         |         |          |
             |         |     Print "C"  Print "F"
             |         |         |         |
             +----+----+----+----+         |
                  |                       |
                 END                      |
                                         END
```

Tree shows:
- Each decision point (diamond shape)
- Possible outcomes (rectangles)
- The order of checking

### Building a Decision Tree

Before writing code, sketch the decision tree:

1. **What's the first question?**
2. **If true, what's the next question?**
3. **If false, what's the next question?**
4. **Continue until all paths end**

Example: "Can you buy alcohol?"

```
                START
                 |
        Age >= 21?
           /     \
         YES      NO
          |        |
        Yes       No
          |        |
         END      END
```

Simple tree, simple code.

More complex: "Are you eligible for student loan?"

```
                    START
                     |
            Citizen or resident?
               /           \
             YES             NO
              |               |
              No             END
              |
         GPA >= 2.0?
           /        \
         YES         NO
          |           |
          Yes         No
          |           |
         END         END
```

More complex tree, more complex code.

---

## Part 8: The Complete Decision Structure

### Anatomy of an if-else if-else Chain

```cpp
if (condition1) {
    // Path 1: Execute if condition1 is TRUE
    // ...
} else if (condition2) {
    // Path 2: Execute if condition1 is FALSE AND condition2 is TRUE
    // ...
} else if (condition3) {
    // Path 3: Execute if conditions 1-2 are FALSE AND condition3 is TRUE
    // ...
} else {
    // Path N: Execute if ALL conditions are FALSE
    // ...
}

// Code here ALWAYS executes (after exactly one path above)
```

Key points:
- **Each condition is only checked if previous conditions were false**
- **Only one path executes** (the first true condition)
- **All conditions below are never reached once one is true**
- **Code after the entire structure always executes**

### Short-Circuiting in Decision Making

Remember short-circuit evaluation from Chapter 13:

```cpp
if (x != 0 && 10 / x > 2) {
    // If x == 0, first part is false
    // Second part is NEVER evaluated
    // So 10/0 never happens
}
```

This is useful in decision-making:

```cpp
if (age >= 18 && canVote()) {
    // If age < 18, canVote() is never called
    // Saves computation
}
```

---

## Part 9: Debugging Control Flow

### Common Control Flow Bugs

**Bug 1: Wrong operator**
```cpp
// WRONG: Using = instead of ==
if (age = 18) {  // Sets age to 18!
    std::cout << "Adult";
}

// CORRECT
if (age == 18) {  // Checks if age equals 18
    std::cout << "Adult";
}
```

**Bug 2: Missing else if**
```cpp
// WRONG: Multiple independent if statements
if (score >= 90) {
    std::cout << "A" << std::endl;
}
if (score >= 80) {  // This runs even if score is 95!
    std::cout << "B" << std::endl;
}

// CORRECT
if (score >= 90) {
    std::cout << "A" << std::endl;
} else if (score >= 80) {  // Only if score is 80-89
    std::cout << "B" << std::endl;
}
```

**Bug 3: Logic errors**
```cpp
// WRONG: Impossible condition
if (age > 18 && age < 10) {
    // Never true; age can't be both > 18 and < 10
}

// CORRECT: Depends on intent
if (age > 18 && age < 65) {  // Working age
    std::cout << "Employed" << std::endl;
}
```

**Bug 4: Unhandled cases**
```cpp
// WRONG: What if input is invalid?
if (grade == 'A') {
    points = 4;
} else if (grade == 'B') {
    points = 3;
}
// If grade is 'X', points is undefined!

// CORRECT
if (grade == 'A') {
    points = 4;
} else if (grade == 'B') {
    points = 3;
} else {
    points = -1;  // Error value
}
```

### Testing Control Flow

Test all paths:

```cpp
// If you have:
if (age >= 18) {
    std::cout << "Adult";
} else {
    std::cout << "Minor";
}

// Test with:
// age = 18 (boundary: adult)
// age = 17 (boundary: minor)
// age = 50 (inside adult)
// age = 5 (inside minor)
```

Test multiple independent conditions:

```cpp
if (age >= 21 && hasLicense) {
    std::cout << "Can drive";
}

// Test with:
// age=25, hasLicense=true (both true)
// age=25, hasLicense=false (one false)
// age=15, hasLicense=true (other false)
// age=15, hasLicense=false (both false)
```

---

## Part 10: A Complete Example Program

Here's a complete program showing all concepts:

```cpp
#include <iostream>

int main() {
    // Read student info
    std::string name;
    int score;
    int absences;
    
    std::cout << "Enter student name: ";
    std::cin >> name;
    
    std::cout << "Enter exam score (0-100): ";
    std::cin >> score;
    
    std::cout << "Enter number of absences: ";
    std::cin >> absences;
    
    // Validate input
    if (score < 0 || score > 100) {
        std::cout << "Invalid score." << std::endl;
        return;
    }
    
    if (absences < 0) {
        std::cout << "Invalid absences." << std::endl;
        return;
    }
    
    // Determine if student passed
    bool passedExam = score >= 60;
    bool attendanceGood = absences <= 5;
    
    // Make decision based on state
    std::cout << std::endl;
    std::cout << "STUDENT REPORT" << std::endl;
    std::cout << "==============" << std::endl;
    std::cout << "Name: " << name << std::endl;
    std::cout << "Score: " << score << std::endl;
    std::cout << "Absences: " << absences << std::endl;
    std::cout << std::endl;
    
    // Decision: determine grade
    if (score >= 90) {
        std::cout << "Grade: A" << std::endl;
    } else if (score >= 80) {
        std::cout << "Grade: B" << std::endl;
    } else if (score >= 70) {
        std::cout << "Grade: C" << std::endl;
    } else if (score >= 60) {
        std::cout << "Grade: D" << std::endl;
    } else {
        std::cout << "Grade: F" << std::endl;
    }
    
    // Decision: determine status
    if (passedExam && attendanceGood) {
        std::cout << "Status: PASS" << std::endl;
    } else if (passedExam && !attendanceGood) {
        std::cout << "Status: PASS (but improve attendance)" << std::endl;
    } else if (!passedExam && attendanceGood) {
        std::cout << "Status: FAIL (but attendance good)" << std::endl;
    } else {
        std::cout << "Status: FAIL" << std::endl;
    }
    
    // Decision: determine recommendation
    if (score < 60 && absences > 10) {
        std::cout << "Recommendation: Retake course" << std::endl;
    } else if (score >= 60 && score < 70) {
        std::cout << "Recommendation: Consider tutoring" << std::endl;
    } else if (score >= 90 && attendanceGood) {
        std::cout << "Recommendation: Excellent work!" << std::endl;
    }
    
    return 0;
}
```

This program demonstrates:
- Input validation (early exit)
- Boolean logic (combining conditions)
- Mutually exclusive paths (grades)
- Complex conditions (status determination)
- Multiple independent decisions (recommendation)
- Complete handling of various cases

---

## Part 11: Key Concepts Reference

| Concept | Definition | Example |
|---------|-----------|---------|
| **Control flow** | Order and selection of which statements execute | if statement changes execution |
| **Condition** | Expression that evaluates to true or false | `age >= 21` |
| **Branch** | Path through program (true or false) | One path of if-else |
| **Mutually exclusive** | Only one can be true/execute | if-else guarantees this |
| **Exhaustive** | All cases covered | else clause catches remaining |
| **Nested** | if inside if | Complex multi-level decisions |
| **Short-circuit** | Stops evaluating when result determined | && and \|\| behavior |
| **Decision tree** | Visual of decision logic | Diagram showing all paths |

---

## Part 12: Best Practices

### 1. Always Use else if, Not Multiple if Statements

```cpp
// WRONG: Both might execute
if (score >= 90) { ... }
if (score >= 80) { ... }

// CORRECT: Only one executes
if (score >= 90) { ... }
else if (score >= 80) { ... }
```

### 2. Use Parentheses to Clarify Complex Conditions

```cpp
// HARD TO READ
if (age >= 21 && hasLicense || hasPermission && !isDistracted) {

// CLEAR
if ((age >= 21 && hasLicense) || (hasPermission && !isDistracted)) {
```

### 3. Guard Against Invalid Input Early

```cpp
// Validate and exit early
if (age < 0 || age > 150) {
    std::cout << "Invalid age." << std::endl;
    return;
}
// Rest of program knows age is valid
```

### 4. Keep if Statements Simple

```cpp
// TOO COMPLEX
if (a > b && c < d && e != f || g == h && !i && j) {

// SIMPLER
bool condition1 = (a > b) && (c < d) && (e != f);
bool condition2 = (g == h) && (!i) && (j);

if (condition1 || condition2) {
```

### 5. Use Meaningful Variable Names

```cpp
// UNCLEAR
bool x = score >= 60;

// CLEAR
bool passedExam = score >= 60;

if (passedExam) {
    // Intent is obvious
}
```

---

## Summary: Decision Making Fundamentals

1. **Control flow** directs program execution based on conditions
2. **Conditions** are boolean expressions (true or false)
3. **if-else** creates mutually exclusive paths
4. **if-else if-else if-else** handles multiple cases
5. **Boolean logic** (&&, ||, !) combines conditions
6. **Exhaustive handling** covers all possible inputs
7. **Decision trees** help plan complex logic
8. **Short-circuit evaluation** optimizes conditions

---

## Looking Ahead to Chapter 15

Now that you understand how to make decisions, the next chapter introduces **loops**—how to repeat statements based on conditions. You'll learn:
- How to repeat code multiple times
- When to use loops
- Different loop structures
- How loops combine with decisions

Decision-making (this chapter) lets programs react to different situations. Looping (next chapter) lets programs handle repetition efficiently. Together, they're the foundation of all real programs.

---

## Checklist: Before Writing Decision Code

Before writing control flow code, ask yourself:

- [ ] What are all possible cases?
- [ ] Are my conditions mutually exclusive?
- [ ] Have I covered all cases (exhaustive)?
- [ ] Am I using else if (not multiple if)?
- [ ] Should I validate input early?
- [ ] Are my conditions clear with parentheses?
- [ ] Have I tested boundary cases?
- [ ] Could I draw a decision tree for this logic?
- [ ] Is the logic readable and maintainable?

If you can answer these confidently, your control flow is well-designed.
