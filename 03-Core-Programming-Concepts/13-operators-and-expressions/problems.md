# Chapter 13: Problems

## Section A: Understanding Expressions and Computation

### Problem 1: Expressions, Types, and Values

For each of the following, identify:
- Whether it's a valid expression
- If valid, what **type** it produces
- If valid, what **value** it produces

1. `42`
2. `5 + 3`
3. `10 / 3`
4. `10.0 / 3`
5. `10 / 3.0`
6. `x + y` (where `int x = 5;` and `int y = 3;`)
7. `15 % 4`
8. `10 > 5`
9. `(3 + 4) * 2`
10. `3 + 4 * 2`

For each expression:
- Explain why the type is what it is
- Explain how the value is computed
- For #9 and #10, explain why they produce different results

---

### Problem 2: Integer Division vs. Floating-Point Division

Consider this code:

```cpp
int a = 7;
int b = 2;
double result1 = a / b;

double c = 7.0;
int d = 2;
double result2 = c / d;

int e = 7;
double f = 2.0;
double result3 = e / f;
```

For each result variable:
1. What value does it hold?
2. Why does it hold that value?
3. Explain the type conversion that happens (if any)
4. Which result is most surprising to beginners and why?
5. How would you fix `result1` to get 3.5 instead of 3.0?

---

## Section B: Relational and Logical Operators

### Problem 3: Building Complex Conditions

You're writing a program to check if a student qualifies for a scholarship. The rules are:
- GPA must be at least 3.5
- Age must be between 18 and 25 (inclusive)
- Must either have financial need OR have completed community service

Given these variables:
```cpp
double gpa = 3.8;
int age = 22;
bool hasFinancialNeed = false;
bool hasCompletedService = true;
```

1. Write a boolean expression that checks if the student qualifies
2. Break down the expression into sub-parts and explain each
3. Trace through the evaluation step-by-step
4. What if `hasCompletedService` were false? Would they still qualify?
5. Rewrite the expression using different parentheses (but same logic)

---

### Problem 4: The Difference Between = and ==

Consider these three code snippets:

**Snippet A:**
```cpp
int x = 5;
if (x == 10) {
    std::cout << "Equal!" << std::endl;
}
```

**Snippet B:**
```cpp
int x = 5;
if (x = 10) {
    std::cout << "Equal!" << std::endl;
}
```

**Snippet C:**
```cpp
int x = 5;
bool result = (x = 10);
std::cout << result << std::endl;
```

For each snippet:
1. What happens when it executes?
2. What value does `x` have after execution?
3. Does "Equal!" print?
4. Explain why Snippet B is dangerous
5. How would a compiler warning help catch this error?

---

## Section C: Operator Precedence and Order

### Problem 5: Precedence and Associativity

Evaluate these expressions step-by-step, showing the order of operations:

1. `5 + 3 * 2`
2. `10 - 5 - 2`
3. `20 / 4 / 2`
4. `true || false && false`
5. `!false && true || false`
6. `5 > 3 && 10 < 20 || 8 == 8`
7. `2 + 3 * 4 > 10 && 15 / 3 == 5`

For each:
- Show the step-by-step evaluation
- Explain which operator is evaluated first and why
- Show how parentheses could make the intent clearer
- Give the final result (value and type)

---

### Problem 6: The Power of Parentheses

Consider this expression:

```cpp
bool access = age >= 18 && hasID || isStaff && hasPermission;
```

1. What is the order of evaluation based on precedence?
2. Write the expression with explicit parentheses showing the default precedence
3. Could this expression be misunderstood? How?
4. Rewrite it with parentheses to mean: "Either (adult with ID) or (staff with permission)"
5. Rewrite it with parentheses to mean: "(Adult) and (either has ID or is staff with permission)"
6. Why are parentheses crucial for readability here?

---

## Section D: Type Conversion and Pitfalls

### Problem 7: Understanding Type Conversion

Analyze this program:

```cpp
#include <iostream>

int main() {
    int pennies = 157;
    int dollars = pennies / 100;
    int remainingPennies = pennies % 100;
    
    std::cout << "Dollars: " << dollars << std::endl;
    std::cout << "Pennies: " << remainingPennies << std::endl;
    
    // Now with floating-point
    double exactDollars = pennies / 100.0;
    std::cout << "Exact: $" << exactDollars << std::endl;
    
    return 0;
}
```

1. What does the program output for each line?
2. Explain why `dollars` is 1 (not 1.57)
3. Explain the role of modulo in computing `remainingPennies`
4. Why does `exactDollars` give 1.57?
5. When is integer division (as in `dollars`) useful vs. floating-point division?
6. Design a program that converts total cents to dollars and cents format (e.g., 157 cents = $1.57)

---

### Problem 8: Complete Program Design with Expressions

Design a complete program that:
1. Asks the user for a temperature in Fahrenheit
2. Converts it to Celsius using the formula: C = (F - 32) × 5/9
3. Displays both temperatures
4. Determines if the temperature is freezing (C < 0) or boiling (C > 100) or comfortable
5. Displays an appropriate message

For this program:
- Write the complete code
- Identify all expressions used
- Explain any type conversion needed
- Show example input and output
- Explain why integer division would break the conversion formula

---

## Challenge Problems (Optional)

### Challenge 1: Short-Circuit Evaluation

Consider this code:

```cpp
int x = 0;
int y = 10;

bool result = (x != 0) && (y / x > 5);
```

1. What happens when this executes?
2. Does it crash or work safely? Why?
3. What is the value of `result`?
4. Explain how short-circuit evaluation prevents a division-by-zero error
5. Write a similar example using `||` where short-circuit prevents an error
6. When should you rely on short-circuit evaluation vs. using explicit if-statements?

---

### Challenge 2: Operator Precedence Puzzle

Without running this code, predict the output:

```cpp
int a = 5;
int b = 3;
int c = 2;

bool result1 = a > b && b > c;
bool result2 = a > b || b < c && a == 5;
bool result3 = !(a < b) && b * c > a || c + c == b + 1;

std::cout << result1 << std::endl;
std::cout << result2 << std::endl;
std::cout << result3 << std::endl;
```

For each result:
1. Trace through the complete evaluation
2. Show which operators are evaluated in which order
3. Explain the final boolean value
4. Rewrite each expression with full parentheses to clarify
5. Explain which expression is most confusing and why

---

### Challenge 3: Type Conversion Chain

Analyze this complex expression:

```cpp
int x = 5;
double y = 2.5;
int z = 3;

double result = (x / z) + y * (double)z - x % z;
```

1. Identify every sub-expression
2. For each sub-expression, identify its type and value
3. Show the order of evaluation
4. Trace through all type conversions that occur
5. What is the final result?
6. How would the result change if you removed the `(double)z` cast?
7. Why is understanding the type of each sub-expression important?

---