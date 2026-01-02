# Chapter 12: Problems

## Section A: Understanding Streams and I/O Concepts

### Problem 1: The Stream Abstraction and Its Power

Consider these three scenarios:

**Scenario A:** Output to screen
```cpp
std::cout << "Hello, World!" << std::endl;
```

**Scenario B:** Output to a file (using advanced features)
```cpp
std::ofstream file("output.txt");
file << "Hello, World!" << std::endl;
```

**Scenario C:** Output to a network socket (using advanced libraries)
```cpp
socket << "Hello, World!" << std::endl;
```

1. What is the same about all three scenarios?
2. What is different?
3. Explain why the stream abstraction is powerful.
4. Why would a programmer prefer writing `std::cout << value` instead of dealing with memory addresses and device-specific code?
5. Describe a real-world scenario where this abstraction would save time and effort.

---

### Problem 2: Input and Output as Communication

You're writing a program that helps a user calculate their body mass index (BMI). The program needs to:
1. Ask the user for their height in inches
2. Ask the user for their weight in pounds
3. Calculate BMI = (weight × 703) / (height²)
4. Tell the user their BMI

Write the complete I/O for this program (just the prompts and output; don't worry about the calculation):

1. What prompts would you use? Write them.
2. What output would you display? Write it.
3. For each prompt, explain why you chose that wording.
4. For each output, explain why that formatting/context is important.

---

## Section B: Output Design and Communication

### Problem 3: The Power of Good Formatting

Compare these two programs that display the same data:

**Program A (Poor Formatting):**
```cpp
int students = 42;
double avgGPA = 3.65;
int honor_students = 18;

std::cout << students << " " << avgGPA << " " << honor_students << std::endl;
```

**Program B (Good Formatting):**
```cpp
int students = 42;
double avgGPA = 3.65;
int honor_students = 18;

std::cout << "CLASS REPORT" << std::endl;
std::cout << "============" << std::endl;
std::cout << "Total Students: " << students << std::endl;
std::cout << "Average GPA: " << avgGPA << std::endl;
std::cout << "Honor Students: " << honor_students << std::endl;
```

1. What does Program A output? Is it clear?
2. What does Program B output? Is it clearer?
3. Which version would a user prefer and why?
4. Which version would be easier to maintain or extend?
5. Write a third version that improves on Program B even further.

---

### Problem 4: Prompts and User Guidance

For each of these attempted I/O sequences, identify what's wrong and fix it:

**Sequence A:**
```cpp
int age;
std::cin >> age;
std::cout << "You are " << age << " years old." << std::endl;
```

**Sequence B:**
```cpp
std::cout << "Enter name";
std::string name;
std::cin >> name;
std::cout << name << std::endl;
```

**Sequence C:**
```cpp
std::cout << "Cost: " << std::endl;
double price;
std::cin >> price;
std::cout << "You will pay: " << price << std::endl;
```

For each, answer:
1. What's the problem?
2. How would a user experience this?
3. What's the corrected version?
4. Why is your correction better?

---

## Section C: Input Design and Type Considerations

### Problem 5: Understanding Input Type Matching

Consider this program:

```cpp
#include <iostream>

int main() {
    int count;
    std::cout << "Enter a number: ";
    std::cin >> count;
    
    std::cout << "You entered: " << count << std::endl;
    
    return 0;
}
```

1. If the user types "42" and presses Enter, what happens?
2. If the user types "hello" and presses Enter, what happens?
3. If the user types "42.5" and presses Enter, what happens?
4. If the user types nothing and just presses Enter, what happens?
5. Why is the mismatch between input and expected type dangerous?

---

### Problem 6: Multiple Input Values

You're writing a program that asks a user for three test scores (integers between 0 and 100):

1. Write the I/O code to prompt for and read three scores.
2. How should the user enter them (on one line, separate lines, etc.)? Why?
3. What if a user types "abc" for one score? What happens to your program?
4. Design a user experience where it's clear how to enter the three scores.
5. Why would the user experience be better if you read all three at once (rather than separately)?

---

## Section D: The Relationship Between I/O and Computation

### Problem 7: Data Flow Through a Program

Trace the complete data flow for this program:

```cpp
#include <iostream>

int main() {
    int hourlyRate;
    int hoursWorked;
    
    std::cout << "Enter your hourly rate ($): ";
    std::cin >> hourlyRate;
    
    std::cout << "Enter hours worked: ";
    std::cin >> hoursWorked;
    
    int totalPay = hourlyRate * hoursWorked;
    
    std::cout << std::endl;
    std::cout << "PAYCHECK SUMMARY" << std::endl;
    std::cout << "=================" << std::endl;
    std::cout << "Hourly Rate: $" << hourlyRate << std::endl;
    std::cout << "Hours Worked: " << hoursWorked << std::endl;
    std::cout << "Total Pay: $" << totalPay << std::endl;
    
    return 0;
}
```

1. Trace the data flow: where does data come from, where does it go?
2. What is the role of input?
3. What is the role of computation?
4. What is the role of output?
5. Draw a diagram showing: Input → Variables → Computation → Output

---

### Problem 8: Understanding I/O Performance Implications

You're writing a program that processes 1,000,000 numbers. For each number, you:
1. Read it from the user
2. Multiply it by 2
3. Display the result

1. Why would this program be very slow?
2. What's the performance bottleneck (input, computation, or output)?
3. How much slower is I/O than computation (refer to the speed hierarchy)?
4. Is there a way to redesign this program to be faster?
5. When is the slow speed acceptable, and when would you need to optimize?

---

## Section E: Complete Program Design

### Problem 9: Designing an Interactive Program

Design a program that:
- Asks the user for their name
- Asks the user for their birth year
- Calculates their approximate age (current year 2026)
- Displays a personalized greeting with their name and age

For this program:
1. Write all the I/O code (prompts and output displays)
2. Explain the order in which you do things
3. Why is the order important?
4. How could poor formatting confuse the user?
5. Write the complete program (including variable declarations)

---

### Problem 10: Analyzing I/O Design in Real Programs

Imagine you're using a program that asks for information like this:

```cpp
std::cout << "Name: ";
std::cin >> name;
std::cout << "Age: ";
std::cin >> age;
std::cout << "City: ";
std::cin >> city;
```

Now imagine an alternative design:

```cpp
std::cout << "Please enter your information:" << std::endl;
std::cout << "  Full name: ";
std::cin >> name;
std::cout << "  Your age (in years): ";
std::cin >> age;
std::cout << "  City of residence: ";
std::cin >> city;
std::cout << std::endl;
std::cout << "Thank you! We have recorded:" << std::endl;
std::cout << "  Name: " << name << std::endl;
std::cout << "  Age: " << age << std::endl;
std::cout << "  City: " << city << std::endl;
```

1. How does the second design differ from the first?
2. Which would be easier to use and why?
3. Which would be better for important applications?
4. What principles of good I/O design does the second version follow?
5. Design your own improved version that combines best practices.

---

## Challenge Problems (Optional)

### Challenge 1: The Relationship Between Buffering and Timing

Consider this program:

```cpp
std::cout << "Processing...";  // No endl
// Imagine a long computation happens here
std::cout << " Done!" << std::endl;
```

1. What might the user see on their screen?
2. What if the computation takes 5 seconds? Might the user think nothing is happening?
3. What's the problem with not using `std::endl` after "Processing..."?
4. How would you fix this?
5. Explain the role of buffering and flushing in this scenario.

---

### Challenge 2: Designing for User Error

Write I/O code that reads two numbers and displays their sum. Consider:

1. How should you prompt the user?
2. What if the user enters text instead of numbers?
3. What if the user doesn't enter anything?
4. What feedback should you give the user?
5. Design a version that's as user-friendly as possible (even if you can't fully validate input yet).

---

### Challenge 3: Understanding the Stream Concept at Deeper Level

Explain why these three pieces of code all work, even though they output to different destinations:

1. `std::cout << "Screen" << std::endl;` (outputs to screen)
2. `std::cerr << "Error" << std::endl;` (outputs to error stream)
3. With file streams: `file << "File" << std::endl;` (outputs to file)

1. What do all three have in common?
2. What does the stream abstraction hide from the programmer?
3. Why is this abstraction valuable?
4. Could you write a single piece of code that works with all three? How?

---