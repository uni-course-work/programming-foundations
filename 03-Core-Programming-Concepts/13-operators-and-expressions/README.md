# Chapter 13: Operators and Expressions

## Introduction: Moving Beyond Single Values

So far, you've learned about **types** (Chapter 10), which constrain how data is represented. You've learned about **variables** (Chapter 11), which hold that data. You've learned about **I/O** (Chapter 12), which communicates that data to and from the outside world.

But programming isn't just about storing and displaying data. It's about **computing new values from existing ones**. This is where **operators** and **expressions** come in.

An **expression** is a piece of code that **computes a value**. The simplest expression is just a literal: `42` is an expression that computes the value 42. But more complex expressions combine values using **operators**: `5 + 3` is an expression that computes 8.

In this chapter, we'll explore:
- How expressions work as computations
- **Arithmetic operators** (addition, subtraction, etc.)
- **Relational operators** (greater than, less than, equals)
- **Logical operators** (and, or, not)
- **Operator precedence** (the rules that determine which operation happens first)
- **Type conversion** (what happens when you mix types)

By the end of this chapter, you'll understand how to write complex expressions that compute meaningful results. You'll see that every expression has a **type** and a **value**—fundamental concepts that will serve you for the rest of your programming journey.

---

## Part 1: Expressions as Computations

### What is an Expression?

An **expression** is code that **evaluates to a value**. Every expression has:
1. A **type** (from Chapter 10)
2. A **value** (the result of evaluation)

Examples of expressions:

```cpp
42                    // Type: int, Value: 42
3.14                  // Type: double, Value: 3.14
5 + 3                 // Type: int, Value: 8
10 > 5                // Type: bool, Value: true
x + y                 // Type: depends on x and y, Value: depends on current values
```

### Simple vs. Complex Expressions

**Simple expressions** are literals or variables:
```cpp
25              // Expression: literal
age             // Expression: variable
'A'             // Expression: character literal
```

**Complex expressions** combine values using operators:
```cpp
5 + 3           // Expression: addition
x * y           // Expression: multiplication
age > 18        // Expression: comparison
```

### Expressions Produce Values

This is crucial: **expressions always produce values**. You can use an expression anywhere a value is expected:

```cpp
int sum = 5 + 3;              // Expression "5 + 3" produces value 8
std::cout << 2 * 4 << std::endl;  // Expression "2 * 4" produces value 8
if (x > 10) { ... }           // Expression "x > 10" produces true or false
```

---

## Part 2: Arithmetic Operators

### The Five Basic Arithmetic Operators

C++ provides five operators for mathematical computation:

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `10 - 3` | `7` |
| `*` | Multiplication | `4 * 3` | `12` |
| `/` | Division | `12 / 3` | `4` |
| `%` | Modulo (remainder) | `13 % 5` | `3` |

### Addition and Subtraction

These are straightforward:

```cpp
int x = 5 + 3;      // x = 8
int y = 10 - 3;     // y = 7
double z = 3.5 + 2.1;  // z = 5.6
```

### Multiplication

```cpp
int area = 5 * 4;   // area = 20 (rectangle area)
double price = 19.99 * 3;  // price = 59.97 (three items)
```

### Division: Integer vs. Floating-Point

Division behaves differently depending on types:

**Integer division (truncates):**
```cpp
int result = 13 / 5;    // result = 2 (not 2.6!)
// The decimal part is discarded
```

**Floating-point division (preserves decimal):**
```cpp
double result = 13.0 / 5;   // result = 2.6
// The decimal part is preserved
```

**Important:** If either operand is `double`, the result is `double`. If both are `int`, the result is `int` (with truncation).

### Modulo: The Remainder Operator

The `%` operator returns the **remainder** after division:

```cpp
13 % 5      // 13 divided by 5 is 2 remainder 3, so result is 3
10 % 3      // 10 divided by 3 is 3 remainder 1, so result is 1
20 % 4      // 20 divided by 4 is 5 remainder 0, so result is 0
```

Modulo is useful for:
- Checking if a number is even or odd: `if (x % 2 == 0) { /* even */ }`
- Cycling through ranges: `next_index = (current_index + 1) % array_size;`
- Converting time: `seconds = total_seconds % 60;`

### Combining Arithmetic Operations

You can combine multiple operations:

```cpp
int result = 5 + 3 * 2;    // What is this?
```

The order matters! This is where **operator precedence** comes in (we'll explore this in detail later). For now: `3 * 2` happens first (multiplication before addition), so the result is `5 + 6 = 11`, not `8 * 2 = 16`.

---

## Part 3: Relational Operators

### Comparing Values

**Relational operators** compare two values and produce a **boolean result** (true or false):

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| `==` | Equal to | `5 == 5` | `true` |
| `!=` | Not equal to | `5 != 3` | `true` |
| `<` | Less than | `3 < 5` | `true` |
| `>` | Greater than | `5 > 3` | `true` |
| `<=` | Less than or equal | `5 <= 5` | `true` |
| `>=` | Greater than or equal | `5 >= 3` | `true` |

### Important: == vs. =

**Critical mistake:** Confusing `=` (assignment) with `==` (equality check).

```cpp
// WRONG: Assignment (not comparison)
if (age = 18) { ... }  // Sets age to 18, then checks if 18 is true (it is)

// CORRECT: Comparison
if (age == 18) { ... }  // Checks if age equals 18
```

This is one of the most common bugs in programming!

### Using Relational Operators

```cpp
int age = 25;

bool isAdult = age >= 18;         // isAdult = true
bool isTeenager = age < 20;       // isTeenager = false
bool isRetiringAge = age >= 65;   // isRetiringAge = false

if (score > 90) {
    std::cout << "Excellent!" << std::endl;
}
```

### Relational Operators Produce Booleans

Every relational operator produces a boolean value. You can store it:

```cpp
bool result = x > y;
std::cout << result << std::endl;  // Prints 1 (true) or 0 (false)
```

---

## Part 4: Logical Operators

### Combining Boolean Conditions

**Logical operators** combine boolean values to produce new boolean values:

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| `&&` | AND (both true) | `true && true` | `true` |
| `\|\|` | OR (at least one true) | `true \|\| false` | `true` |
| `!` | NOT (opposite) | `!true` | `false` |

### AND (&&): Both Must Be True

```cpp
int age = 25;
int income = 50000;

bool canBuyHouse = (age >= 21) && (income >= 40000);
// age >= 21 is true
// income >= 40000 is true
// true && true = true
// canBuyHouse = true
```

AND requires **both** conditions to be true. If either is false, the result is false.

```cpp
bool result1 = true && true;       // true
bool result2 = true && false;      // false
bool result3 = false && true;      // false
bool result4 = false && false;     // false
```

### OR (||): At Least One Must Be True

```cpp
int temperature = 35;  // Celsius
bool isTooExtreme = (temperature < -10) || (temperature > 30);
// temperature < -10 is false
// temperature > 30 is true
// false || true = true
// isTooExtreme = true
```

OR requires **at least one** condition to be true. Only false if both are false.

```cpp
bool result1 = true || true;       // true
bool result2 = true || false;      // true
bool result3 = false || true;      // true
bool result4 = false || false;     // false
```

### NOT (!): Flip the Value

```cpp
bool isRaining = true;
bool isDry = !isRaining;           // isDry = false

bool isEven = (x % 2 == 0);
bool isOdd = !isEven;              // Opposite of even
```

NOT flips the boolean value. `true` becomes `false`, and `false` becomes `true`.

### Complex Logical Expressions

You can combine multiple operators:

```cpp
int age = 25;
int income = 50000;
bool hasJobOffer = true;

bool canRelocate = (age < 65) && (income > 30000 || hasJobOffer);
// Evaluates as:
// (age < 65) is true
// (income > 30000) is true
// (true || true) is true
// (true && true) is true
// canRelocate = true
```

### Short-Circuit Evaluation

Logical operators use **short-circuit evaluation**: they stop evaluating once the result is determined.

**AND short-circuit:**
```cpp
if (x != 0 && 10 / x > 2) {
    // If x == 0, first part is false
    // Since && result can't be true, second part is never evaluated
    // This prevents division by zero!
}
```

**OR short-circuit:**
```cpp
if (x == 0 || compute()) {
    // If x == 0, first part is true
    // Since || result is already true, compute() is never called
    // This prevents unnecessary computation
}
```

---

## Part 5: Operator Precedence and Parsing

### Why Precedence Matters

Consider this expression:

```cpp
int result = 5 + 3 * 2;
```

Is this `(5 + 3) * 2 = 16` or `5 + (3 * 2) = 11`?

The answer is **precedence rules**. They determine the order of operations.

### The Operator Precedence Hierarchy

Here's the basic precedence (highest to lowest):

1. **Unary operators** (act on single value): `!`, `-` (negation)
2. **Multiplication/Division/Modulo**: `*`, `/`, `%`
3. **Addition/Subtraction**: `+`, `-`
4. **Relational operators**: `<`, `>`, `<=`, `>=`
5. **Equality operators**: `==`, `!=`
6. **Logical AND**: `&&`
7. **Logical OR**: `||`
8. **Assignment**: `=`

### Examples of Precedence

```cpp
// Multiplication before addition
5 + 3 * 2 = 5 + 6 = 11  (not 16)

// Relational before logical AND
x > 5 && y < 10  means  (x > 5) && (y < 10)

// Logical AND before OR
true || false && false = true || (false && false) = true || false = true

// Negation before AND
!x && y  means  (!x) && y  (not !(x && y))
```

### Using Parentheses to Override Precedence

When in doubt, use parentheses:

```cpp
// Unclear
int result = 5 + 3 * 2;

// Clear
int result = 5 + (3 * 2);  // 11

// Or if you meant the other way
int result = (5 + 3) * 2;  // 16
```

Parentheses make intent explicit and prevent bugs.

### Associativity: Left to Right

When operators have the same precedence, **associativity** determines the order:

```cpp
// Subtraction (left-to-right associativity)
10 - 5 - 2 = (10 - 5) - 2 = 5 - 2 = 3  (not 10 - (5 - 2) = 7)

// Division (left-to-right associativity)
20 / 4 / 2 = (20 / 4) / 2 = 5 / 2 = 2  (not 20 / (4 / 2) = 10)
```

Most operators are left-to-right. Assignment is right-to-left:

```cpp
// Assignment (right-to-left)
int x, y, z;
x = y = z = 5;  // Evaluates as x = (y = (z = 5))
// z is set to 5, then y to 5, then x to 5
```

---

## Part 6: Type Conversion in Expressions

### Implicit Type Conversion

When you mix types in an expression, C++ automatically converts them to a common type:

```cpp
int x = 5;
double y = 2.5;
double result = x + y;  // x is converted to 5.0, then 5.0 + 2.5 = 7.5
```

**Conversion rule:** int is converted to double (the "wider" type).

### Integer vs. Floating-Point Conversion

```cpp
int a = 5;
double b = 2.0;

double result1 = a + b;   // a converts to 5.0, result is 7.0
double result2 = a / b;   // a converts to 5.0, result is 2.5
int result3 = a + b;      // 7.0 converts back to 7 (truncation!)
```

**Key point:** When storing a `double` in an `int`, the decimal is **truncated** (lost).

### Division Pitfall: Integer Division Surprise

```cpp
int numerator = 5;
int denominator = 2;
double result = numerator / denominator;  // What is this?
```

You might expect `2.5`, but you get `2.0`!

Why? Because `5 / 2` is **integer division** (both operands are `int`), which truncates to `2`. Only after that is the result converted to `double` (becoming `2.0`).

**Fix:**
```cpp
double result = (double)numerator / denominator;  // Converts 5 to 5.0 first
// Now 5.0 / 2 is floating-point division, producing 2.5
```

### Bool to Int Conversion

```cpp
bool flag = true;
int x = flag;           // x = 1
int y = false;          // y = 0
bool result = 5;        // result = true (any non-zero converts to true)
```

Booleans convert to 0 (false) or 1 (true) when used as integers.

### Explicit Type Casting

You can force type conversion using a **cast**:

```cpp
int x = 5;
double y = (double)x;   // Explicitly convert x to double

int truncated = (int)3.7;   // Explicitly convert 3.7 to int (becomes 3)
```

The syntax `(type)value` casts the value to the specified type.

### Type Conversion and Loss of Information

Be aware of conversions that lose information:

```cpp
double precise = 3.14159;
int rounded = (int)precise;     // rounded = 3 (loses decimal)

int large = 1000000;
short smaller = (short)large;   // smaller might overflow
```

---

## Part 7: Combining It All Together

### A Complete Expression

```cpp
int age = 25;
int income = 50000;
double tax_rate = 0.22;

bool qualifies = (age >= 21) && (income >= 40000) && !(income > 200000);
// (25 >= 21) is true
// (50000 >= 40000) is true
// (50000 > 200000) is false, so !(false) is true
// true && true && true = true
// qualifies = true

double tax = income * tax_rate;
// 50000 * 0.22 = 11000.0
// Type: double (because tax_rate is double)
```

### Complex Expressions with Multiple Types

```cpp
int count = 10;
double value = 3.5;
bool isValid = true;

// Mixed-type expression
double total = (count + 1) * value;
// (10 + 1) is 11 (int)
// 11 is converted to 11.0 (double)
// 11.0 * 3.5 = 38.5
// total = 38.5

// With logical operations
bool condition = (count > 0) && (value > 0.0) && isValid;
// (10 > 0) is true
// (3.5 > 0.0) is true
// true && true && true = true
// condition = true
```

---

## Part 8: Common Expression Pitfalls

### Pitfall 1: Integer Division Truncation

```cpp
int result = 5 / 2;  // result = 2, not 2.5
```

**Fix:** Convert to double first
```cpp
double result = 5.0 / 2;  // result = 2.5
```

### Pitfall 2: Confusing = and ==

```cpp
// WRONG: Assignment instead of comparison
if (x = 5) { ... }  // Sets x to 5, then checks if true (always true!)

// CORRECT: Comparison
if (x == 5) { ... }  // Checks if x equals 5
```

### Pitfall 3: Not Using Parentheses

```cpp
// UNCLEAR (relies on precedence)
bool result = x < 5 && y > 10;

// CLEAR (parentheses make intent obvious)
bool result = (x < 5) && (y > 10);
```

### Pitfall 4: Unexpected Type Conversion

```cpp
int x = 5;
int y = 2;
double result = x / y;  // result = 2.0, not 2.5!
// Integer division happens first (5 / 2 = 2)
// Then converted to 2.0

// FIX: Cast one operand first
double result = x / (double)y;  // 5 / 2.0 = 2.5
```

### Pitfall 5: Operator Precedence Surprises

```cpp
bool result = !x > 5;  // Means (!x) > 5, not !(x > 5)

// CLEAR: Use parentheses
bool result = !(x > 5);  // Better intent
```

---

## Part 9: Order of Operations Summary

### PEMDAS in Programming (sort of)

You learned PEMDAS (Parentheses, Exponents, Multiplication, Division, Addition, Subtraction) in math. C++ is similar but with subtle differences:

1. **Parentheses** (highest priority)
2. **Unary operators** (`!`, `-`)
3. **Multiplication/Division/Modulo** (`*`, `/`, `%`)
4. **Addition/Subtraction** (`+`, `-`)
5. **Relational** (`<`, `>`, `<=`, `>=`)
6. **Equality** (`==`, `!=`)
7. **Logical AND** (`&&`)
8. **Logical OR** (`||`)
9. **Assignment** (`=`, lowest priority)

### Rule of Thumb

When in doubt, use parentheses. They make code clearer and prevent bugs.

---

## Part 10: Expressions and Variables

### Expressions Produce Values That Go Into Variables

```cpp
int x = 5 + 3;              // Expression "5 + 3" produces 8, stored in x
int y = x * 2;              // Expression "x * 2" produces 16, stored in y
bool isPositive = y > 0;    // Expression "y > 0" produces true, stored in isPositive
```

### Variables Can Be Part of Expressions

```cpp
int a = 5;
int b = 3;

int sum = a + b;            // Variables in expression
int product = a * b;        // Variables in expression
bool isSumLarger = sum > product;  // Variables in another expression
```

### The Complete Picture

```cpp
#include <iostream>

int main() {
    int principal = 1000;           // Initial investment
    double rate = 0.05;             // Interest rate (5%)
    int years = 3;                  // Time period
    
    // Expression: computing interest
    double interest = principal * rate * years;
    
    // Expression: computing total
    double total = principal + interest;
    
    // Expression: checking if doubled
    bool doubled = total >= principal * 2;
    
    // Output using expressions
    std::cout << "Principal: $" << principal << std::endl;
    std::cout << "Interest: $" << interest << std::endl;
    std::cout << "Total: $" << total << std::endl;
    std::cout << "Doubled: " << doubled << std::endl;
    
    return 0;
}
```

Output:
```
Principal: $1000
Interest: $150
Total: $1150
Doubled: 0
```

---

## Part 11: Building Complex Programs

Now you have all the pieces:
- **Types** constrain values
- **Variables** store values
- **I/O** communicates values
- **Operators** compute new values from existing ones
- **Expressions** combine all of this

You can write complete programs that:
1. Read input from users
2. Store it in variables
3. Compute results using expressions
4. Display output

Example:

```cpp
#include <iostream>

int main() {
    // Read input
    double price;
    int quantity;
    double discount_rate;
    
    std::cout << "Enter price per item: $";
    std::cin >> price;
    
    std::cout << "Enter quantity: ";
    std::cin >> quantity;
    
    std::cout << "Enter discount rate (0-1): ";
    std::cin >> discount_rate;
    
    // Compute using expressions
    double subtotal = price * quantity;
    double discount = subtotal * discount_rate;
    double total = subtotal - discount;
    
    // Display output
    std::cout << std::endl;
    std::cout << "INVOICE" << std::endl;
    std::cout << "================" << std::endl;
    std::cout << "Subtotal: $" << subtotal << std::endl;
    std::cout << "Discount: $" << discount << std::endl;
    std::cout << "Total: $" << total << std::endl;
    
    return 0;
}
```

This program demonstrates I/O, variables, and expressions working together.

---

## Part 12: Key Concepts Reference

| Concept | Definition | Example |
|---------|-----------|---------|
| **Expression** | Code that evaluates to a value | `5 + 3`, `x > 10` |
| **Operator** | Symbol for computation | `+`, `*`, `>`, `&&` |
| **Arithmetic operator** | Performs math (+, -, *, /, %) | `5 * 2 = 10` |
| **Relational operator** | Compares values (==, !=, <, >) | `5 > 3` = `true` |
| **Logical operator** | Combines booleans (&&, \|\|, !) | `true && false` = `false` |
| **Precedence** | Order in which operators evaluate | `*` before `+` |
| **Associativity** | Left-to-right (or right-to-left) order | `5 - 2 - 1` = `2` |
| **Type conversion** | Auto-conversion between types | `5 + 2.5` = `7.5` |
| **Casting** | Explicit type conversion | `(int)3.7` = `3` |

---

## Mistakes to Avoid

### Mistake 1: Confusing = and ==

```cpp
// WRONG: Assignment
if (x = 5) { ... }

// CORRECT: Comparison
if (x == 5) { ... }
```

### Mistake 2: Integer Division Truncation

```cpp
// WRONG: Integer division
double result = 5 / 2;  // result = 2.0

// CORRECT: Force floating-point
double result = 5.0 / 2;  // result = 2.5
```

### Mistake 3: Not Using Parentheses

```cpp
// UNCLEAR
bool result = x < 5 && y > 10;

// CLEAR
bool result = (x < 5) && (y > 10);
```

### Mistake 4: Relying on Precedence

```cpp
// HARD TO READ
bool result = a > b && c < d || e == f && !g;

// CLEAR
bool result = ((a > b) && (c < d)) || ((e == f) && (!g));
```

### Mistake 5: Type Conversion Loss

```cpp
// LOSES DECIMAL
int truncated = 3.7;  // truncated = 3

// PRESERVE WITH DOUBLE
double kept = 3.7;    // kept = 3.7
```

---

## Looking Ahead to Chapter 14

Now that you understand how to create expressions and compute values, the next chapter will introduce **control flow**. You'll learn how to:
- Use boolean expressions to make decisions (`if` statements)
- Repeat computations using loops
- Build programs that react differently based on computed values

Expressions enable computation. Control flow enables decision-making based on those computations. Together, they enable intelligent programs.

---

## Checklist: Before Using Operators

Before writing expressions, ask yourself:

- [ ] Do I understand what each operator does?
- [ ] Have I checked operator precedence?
- [ ] Are parentheses needed for clarity?
- [ ] Do the types make sense (no unexpected conversions)?
- [ ] Could I use parentheses to make intent clearer?
- [ ] Am I using == for comparison (not = for assignment)?
- [ ] Will integer division truncate unexpectedly?

If you can answer these confidently, your expressions are well-designed.
