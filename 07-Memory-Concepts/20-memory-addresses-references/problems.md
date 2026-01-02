# Chapter 20: Problems

## Section A: Understanding Memory and References

### Problem 1: Memory and Variable Storage

Consider this code:

```cpp
int x = 5;
int y = 10;
double z = 3.14;
```

1. How much memory does each variable use?
   - `x` uses ___ bytes
   - `y` uses ___ bytes
   - `z` uses ___ bytes

2. Draw a memory layout diagram showing all three variables (show address, variable name, value)

3. If `x` is at address 1000, what addresses might `y` and `z` occupy?

4. What's the difference between these two operations:
   ```cpp
   std::cout << x << std::endl;      // Prints: ?
   std::cout << &x << std::endl;     // Prints: ?
   ```

5. Can you create a variable at a specific address? Why or why not?

---

### Problem 2: References vs. Copies

For each scenario, determine if it's a copy or reference, and predict the output:

**Scenario A:**
```cpp
int a = 5;
int b = a;
b = 10;
std::cout << a << std::endl;  // Prints: ?
```

**Scenario B:**
```cpp
int a = 5;
int& b = a;
b = 10;
std::cout << a << std::endl;  // Prints: ?
```

**Scenario C:**
```cpp
int a = 5;
int& b = a;
int c = b;
c = 20;
std::cout << a << " " << b << std::endl;  // Prints: ?
```

**Scenario D:**
```cpp
int a = 5;
int& b = a;
int& c = b;
c = 30;
std::cout << a << std::endl;  // Prints: ?
```

For each scenario:
- Draw a memory diagram
- Explain why the output is what it is
- Identify what changed and what didn't

---

## Section B: Building Programs with References

### Problem 3: Reference Functions - Swapping and Modification

**Write a complete program** that:

1. Defines helper functions:
   - `swap(int& a, int& b)` - swap two integers
   - `addBonus(double& salary, double percent)` - add percentage to salary
   - `incrementAll(std::vector<int>& v)` - add 1 to each element
   - `findElement(std::vector<int>& v, int value)` - return reference to element

2. In `main()`:
   - Create two integers, swap them, display
   - Create a salary, apply bonus, display
   - Create a vector of numbers, increment all, display
   - Create a vector, find an element, modify through reference

3. Display memory concepts:
   - Show values before and after operations
   - Demonstrate which variables changed (and which didn't)

**Example run:**
```
=== SWAPPING ===
Before: a=5, b=10
After swap: a=10, b=5

=== BONUS ===
Before: salary=$50000
After 10% bonus: salary=$55000

=== INCREMENT ALL ===
Before: [85, 90, 78]
After increment: [86, 91, 79]

=== FIND AND MODIFY ===
Before: [10, 20, 30]
Find element 20 and set to 99
After: [10, 99, 30]
```

Requirements:
- Four functions using references
- Clear before/after display
- Demonstrate reference behavior in each case
- Comments explaining what changed and why

---

### Problem 4: Understanding Function Parameters and Scope

**Write a complete program** that demonstrates:

1. Three versions of a "multiply value" function:
   - `multiplyA(int num, int factor)` - pass-by-value
   - `multiplyB(int& num, int factor)` - pass-by-reference (modifying)
   - `multiplyC(const int& num, int factor)` - pass-by-const-reference (reading)

2. For each function:
   - Call with same input value
   - Display the value before and after
   - Show what changed in the original variable

3. Create a structure:
   ```cpp
   struct Product {
       std::string name;
       double price;
       int quantity;
       std::vector<double> reviews;  // Large data
   };
   ```

4. Create three display functions:
   - `displayA(Product p)` - pass-by-value
   - `displayB(const Product& p)` - pass-by-const-reference
   - `displayC(Product& p)` - pass-by-reference (for modification)

5. Explain which is most efficient for reading, which for modifying

**Example run:**
```
=== PASS-BY-VALUE ===
Original value: 5
Multiply by 2: function receives 5
Function result: 10
Original after function: 5 (unchanged!)

=== PASS-BY-REFERENCE ===
Original value: 5
Multiply by 2: function receives reference to 5
Function result: 10
Original after function: 10 (changed!)

=== PASS-BY-CONST-REFERENCE ===
Original value: 5
Can't modify through const reference
Original after function: 5 (unchanged!)

=== DISPLAYING STRUCTURE ===
By-value: Creates full copy (inefficient!)
By-const-reference: Just references (efficient!)
By-reference: Can modify (for updates)
```

Requirements:
- Six functions demonstrating different parameter types
- Before/after display showing what changed
- Comments explaining why each approach is used
- Comparison of efficiency (value vs. reference vs. const-ref)

---

### Problem 5: Vector References and Iteration

**Write a complete program** that:

1. Creates a Student structure:
   ```cpp
   struct Student {
       std::string name;
       int id;
       double gpa;
   };
   ```

2. Implements functions:
   - `addBonus(std::vector<Student>& roster, double bonusGPA)` - add to all GPAs
   - `findStudent(std::vector<Student>& roster, int id)` - return reference to student
   - `displayRoster(const std::vector<Student>& roster)` - display all (read-only)
   - `updateGPA(Student& s, double newGPA)` - update one student

3. Demonstrates:
   - Iterate through vector with references (modify in place)
   - Iterate through vector without references (read-only)
   - Return reference to vector element
   - Modify through returned reference

4. Menu system:
   - Load sample students
   - Display all
   - Add GPA bonus
   - Find student by ID and modify
   - Display all (show changes)
   - Exit

**Example run:**
```
=== STUDENT ROSTER ===

Sample students loaded.

1. Display All
2. Add GPA Bonus
3. Find and Modify
4. Exit
Choice: 1

=== ALL STUDENTS ===
Alice (ID: 101) - GPA: 3.85
Bob (ID: 102) - GPA: 3.72
Charlie (ID: 103) - GPA: 3.91

Choice: 2
Add bonus (0.1): 0.1
Adding 0.1 to all GPAs...

Choice: 1
=== ALL STUDENTS ===
Alice (ID: 101) - GPA: 3.95
Bob (ID: 102) - GPA: 3.82
Charlie (ID: 103) - GPA: 4.00 (capped at 4.0)

Choice: 3
Enter student ID: 102
Found: Bob
Enter new GPA: 3.50
Updated!

Choice: 1
=== ALL STUDENTS ===
Alice (ID: 101) - GPA: 3.95
Bob (ID: 102) - GPA: 3.50
Charlie (ID: 103) - GPA: 4.00

Choice: 4
Goodbye!
```

Requirements:
- Functions using reference parameters
- Functions returning references
- Iteration with and without references
- Clear before/after display
- Menu-driven interface
- Comments explaining reference behavior

---
