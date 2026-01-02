# Chapter 10: Problems

## Section A: Understanding Data Types as Constraints

### Problem 1: Types as Multi-Faceted Constraints

The byte sequence `01000001` exists in computer memory. Depending on the type used to interpret it, it could represent different things.

1. If this byte sequence is interpreted as an `int`, what value is it?
2. If this byte sequence is interpreted as a `char`, what character is it? (Hint: Consider ASCII codes)
3. If this byte sequence is interpreted as a `bool`, what value would it represent?
4. Explain why the SAME bytes in memory can mean different things.
5. How does declaring a type in C++ prevent confusion about what these bytes mean?

---

### Problem 2: Constraints and Consequences

Consider the `int` type. It typically occupies 4 bytes and can store values from -2,147,483,648 to 2,147,483,647.

1. What is the practical consequence of this range limitation?
2. What would happen if you tried to store the number 3,000,000,000 in an `int` variable?
3. Write a scenario where this limitation would cause a real problem in a program.
4. Why do you think `int` has this specific range instead of being unlimited?
5. How is this constraint different from a logical constraint (like "age should be positive")?

---

### Problem 3: The Float vs. Double Trade-off

C++ provides two floating-point types: `float` (4 bytes) and `double` (8 bytes). Yet many programmers default to `double`.

1. What is the primary trade-off between choosing `float` and `double`?
2. Explain: Why would `double` be preferred for scientific calculations?
3. Explain: When might `float` be preferred despite lower precision?
4. Consider a program that stores temperature readings from 10,000 sensors. How would choosing `float` vs. `double` affect memory usage?
5. Given this calculation: 0.1 + 0.2 = ? (Consider IEEE 754 representation). Why might the answer surprise you?

---

## Section B: Values, Variables, and Interpretation

### Problem 4: Distinguishing Values, Literals, and Variables

For each item below, identify whether it is a **value**, a **literal**, or a **variable**:

1. `42`
2. `int x = 42;` (specifically, what is `x`?)
3. The number 42 as a concept in mathematics
4. `'A'`
5. The character A in the alphabet
6. `true`
7. A storage location in computer memory holding the character 'Z'

For each one, explain your reasoning in one or two sentences.

---

### Problem 5: The Power of Type Interpretation

Consider these four storage locations in memory. Each contains the exact same bytes: `01000001`.

- Location 1 is declared as `int`
- Location 2 is declared as `char`
- Location 3 is declared as `float`
- Location 4 is declared as `bool`

1. What value would each location appear to hold?
2. If you printed each value to the screen, what would you see?
3. Why is the `type` declaration crucial for interpreting memory correctly?
4. Is there a "correct" interpretation of `01000001`, or does it depend on context?
5. How does this explain why type safety is so important in programming?

---

## Section C: Primitive Types in Practice

### Problem 6: Choosing Types for Real Data

You're building a program to track book inventory for a library. For each piece of information below, choose the most appropriate C++ primitive type and explain your reasoning:

1. Book ISBN number (unique identifier, like 978-0-123456-78-9, but stored as a whole number)
2. Number of copies of each book in stock
3. Average rating of each book (on a scale of 0.0 to 5.0)
4. Whether a book has been checked out or not
5. First letter of the book's author's last name
6. Year the book was published

For each choice, explain:
- Why this type is appropriate
- What range or precision is needed
- Any constraints or risks with your choice

---

### Problem 7: Understanding Type Conversion and Loss

Consider this code:

```cpp
double temperature = 98.6;
int roundedTemp = (int)temperature;  // Type conversion
```

1. What is the value of `roundedTemp`?
2. What happened to the decimal portion (.6)?
3. Is this loss of precision unavoidable, or could you handle it differently?
4. When would this type of loss be a critical bug?
5. When would this type of loss be acceptable?
6. How is this different from rounding?

---

## Section D: Type Constraints and Real-World Scenarios

### Problem 8: Overflow and Its Consequences

An `int` in C++ can store values from -2,147,483,648 to 2,147,483,647 (approximately -2.1 billion to +2.1 billion).

1. Consider a program that counts how many milliseconds have passed since the computer started. If you use an `int` and the computer runs continuously, how many days until overflow occurs?
2. This actually happened in real computer systems. Research or think about: What problems does overflow cause?
3. How could you prevent overflow in such a scenario?
4. Why is this a particular concern for programs that run continuously (like operating systems)?
5. Write a hypothetical scenario where overflow causes a serious bug in a real application.

---

### Problem 9: ASCII and Character Representation

The `char` type stores integers that are interpreted as ASCII codes. For example:
- 65 = 'A'
- 97 = 'a'
- 48 = '0' (the digit zero, not the number zero)

1. What is the ASCII code for the space character ' '?
2. What is the difference between the character '5' and the integer 5?
3. If you store the integer 65 in a `char` variable and print it, what appears on screen?
4. Can you store the number 1000 in a `char`? Why or why not?
5. Why might confusing `'5'` (character) with `5` (integer) create bugs in a program?

---

## Section E: Type Systems and Program Correctness

### Problem 10: Type Mismatches and Compiler Errors

Consider this program:

```cpp
#include <iostream>

int main() {
    int age = 25;
    char grade = 'A';
    
    // Line X: int combined = age + grade;
    
    return 0;
}
```

1. What happens if you compile and run line X as written?
2. Why would the compiler potentially object to this operation?
3. Is the operation mathematically valid? Why or why not?
4. How does type checking by the compiler help prevent bugs?
5. What scenarios could this type mismatch cause problems in a real program?

---

### Problem 11: Floating-Point Precision and Real Consequences

In IEEE 754 representation, some decimal numbers cannot be represented perfectly. For example, 0.1 and 0.2 are approximated.

```cpp
double result = 0.1 + 0.2;
```

1. What would you expect `result` to equal?
2. Why might the actual computed value differ from your expectation?
3. In what types of programs would this small error be irrelevant?
4. In what types of programs would this small error be catastrophic?
5. How do professional programmers handle floating-point precision concerns?

---

## Section F: Synthesis and Application

### Problem 12: Complete Type Analysis

You are designing a program to track student grades. For each data element, you need to:
- Choose an appropriate type
- Explain the constraints that type provides
- Identify potential problems with that choice
- Suggest alternatives if appropriate

The data elements are:
1. Student ID number (8 digits)
2. Midterm exam score (0-100)
3. Final exam score (0-100)
4. GPA (0.0 to 4.0)
5. Letter grade (A, B, C, D, or F)
6. Class participation flag (attended or didn't attend)
7. Number of assignments completed (0-30)

For each element, write 3-4 sentences explaining your type choice and its implications.

---

### Problem 13: Designing with Type Constraints

You need to write a program that monitors a spacecraft's altitude during landing. The altitude is measured in feet with decimal precision (e.g., 1,234.5 feet).

1. Would `int` be appropriate? Why or why not?
2. Would `float` be appropriate? Why or why not? (Hint: Consider precision needs)
3. Would `double` be appropriate? Why or why not?
4. What happens if the spacecraft's altitude exceeds the maximum value your chosen type can represent?
5. Why might this be a safety-critical decision?
6. Design a solution that accounts for the constraints you've identified.

---

## Challenge Problems (Optional)

### Challenge 1: Understanding Memory and Types

At the machine level, all data is just bytes. Types add meaning. Consider this hypothetical scenario:

You have a file that contains 1,000,000 bytes of unknown data. A colleague insists it contains:
- 250,000 `int` values (each 4 bytes)
- 1,000,000 `char` values (each 1 byte)
- 125,000 `double` values (each 8 bytes)

1. Could the file contain all three interpretations simultaneously? Explain.
2. Why does the "correct" interpretation depend on context?
3. What information would you need to know to interpret the data correctly?
4. How does this illustrate why types are essential in programming?

---

### Challenge 2: The Limits of Representation

Numbers in mathematics are infinite and can have arbitrary precision. But in computer memory:
- `int` is limited to roughly -2.1B to +2.1B
- `float` is limited to roughly -3.4e38 to +3.4e38
- `double` is limited to roughly -1.7e308 to +1.7e308

1. Can you represent every integer between -2.1B and +2.1B with an `int`? Yes, but what's the catch?
2. Can you represent every decimal number between 0.0 and 1.0 with a `float`? Explain.
3. Why do we accept these limitations in programming?
4. For what purposes would these limitations be unacceptable, requiring different solutions?

---

### Challenge 3: Type Conversion Pitfalls

Different type conversions have different consequences:

```cpp
double x = 3.9;
int a = (int)x;          // Conversion 1
int b = (int)(x + 0.5);  // Conversion 2
int c = (int)(x - 0.5);  // Conversion 3
```

1. What are the values of a, b, and c?
2. Why do these three conversions produce different results?
3. Which conversion is closest to rounding?
4. Why might a programmer use each approach?
5. What's the lesson about type conversion here?

---
