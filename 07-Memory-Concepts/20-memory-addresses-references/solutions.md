# Chapter 20: Solutions

## Section A: Understanding Memory and References

### Problem 1: Memory and Variable Storage

**Solution:**

**1. Memory usage:**
```cpp
int x = 5;           // 4 bytes
int y = 10;          // 4 bytes
double z = 3.14;     // 8 bytes
```

**2. Memory layout diagram:**
```
Address | Variable | Value  | Size
--------|----------|--------|-------
1000    | x        | 5      | 4 bytes
1004    | y        | 10     | 4 bytes
1008    | z        | 3.14   | 8 bytes
```

**3. Addresses (example):**
- If `x` is at 1000:
  - `y` might be at 1004 (4 bytes later)
  - `z` might be at 1008 (4 bytes later)
  - Depends on memory layout and alignment

**4. Output difference:**
```cpp
std::cout << x << std::endl;   // Prints: 5 (the value)
std::cout << &x << std::endl;  // Prints: 0x7ffc... (the address)
```

- `x` = the value stored at the address (5)
- `&x` = the address itself (like 0x7ffc2b3d4a44)

**5. Can you create a variable at a specific address?**
No. The compiler decides where to place variables. You can't specify addresses in standard C++ (except with advanced pointer techniques, beyond this course).

---

### Problem 2: References vs. Copies

**Solution:**

**Scenario A: Copy**
```cpp
int a = 5;
int b = a;  // b = COPY of a
b = 10;
std::cout << a << std::endl;  // Prints: 5
```

**Memory:**
```
a: address 1000, value 5
b: address 1004, value 5 (copy)
```

After `b = 10;`:
```
a: address 1000, value 5 (unchanged)
b: address 1004, value 10 (changed)
```

**Explanation:** `b` is a separate variable with its own storage. Changing `b` doesn't affect `a`.

---

**Scenario B: Reference**
```cpp
int a = 5;
int& b = a;  // b is ALIAS to a
b = 10;
std::cout << a << std::endl;  // Prints: 10
```

**Memory:**
```
a / b: address 1000, value 5
```

After `b = 10;`:
```
a / b: address 1000, value 10 (both changed)
```

**Explanation:** `b` is an alias for `a` (same storage location). Changing `b` changes the storage location, so `a` also reflects the change.

---

**Scenario C: Reference then Copy**
```cpp
int a = 5;
int& b = a;  // b refers to a
int c = b;   // c = COPY of value at b (which is a's value)
c = 20;
std::cout << a << " " << b << std::endl;  // Prints: 5 5
```

**Memory:**
```
a / b: address 1000, value 5 (same storage)
c: address 1008, value 5 (copy)
```

After `c = 20;`:
```
a / b: address 1000, value 5 (unchanged)
c: address 1008, value 20 (changed)
```

**Explanation:** `b` is a reference to `a`. But `c = b` creates a copy of the current value. `c` is a separate variable. Changing `c` doesn't affect `a` or `b`.

---

**Scenario D: Chain of References**
```cpp
int a = 5;
int& b = a;  // b refers to a
int& c = b;  // c refers to b, which refers to a (all same location)
c = 30;
std::cout << a << std::endl;  // Prints: 30
```

**Memory:**
```
a / b / c: address 1000, value 5
```

After `c = 30;`:
```
a / b / c: address 1000, value 30 (all changed)
```

**Explanation:** `b` is a reference to `a`. `c` is a reference to `b`, which means `c` refers to the same storage as `a`. All three names refer to the same location. Changing any of them changes all of them.

---

## Section B: Building Programs with References

### Problem 3: Reference Functions - Swapping and Modification

**Solution:**

```cpp
#include <iostream>
#include <vector>
#include <string>
using std::string;
using std::vector;

// Function 1: Swap two integers
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

// Function 2: Add bonus percentage to salary
void addBonus(double& salary, double percent) {
    salary += salary * (percent / 100.0);
}

// Function 3: Increment all elements in vector
void incrementAll(vector<int>& v) {
    for (int& num : v) {  // Reference to each element
        num++;  // Modifies the actual element
    }
}

// Function 4: Find element and return reference
int& findElement(vector<int>& v, int value) {
    for (int i = 0; i < v.size(); i++) {
        if (v[i] == value) {
            return v[i];  // Return reference to the element
        }
    }
    // (Simplified - error handling omitted)
    return v[0];
}

int main() {
    std::cout << "=== SWAPPING ===" << std::endl;
    int a = 5, b = 10;
    std::cout << "Before: a=" << a << ", b=" << b << std::endl;
    swap(a, b);
    std::cout << "After swap: a=" << a << ", b=" << b << std::endl;
    std::cout << "(Both changed because swap receives references)" << std::endl;
    
    std::cout << "\n=== BONUS ===" << std::endl;
    double salary = 50000;
    std::cout << "Before: salary=$" << salary << std::endl;
    addBonus(salary, 10);
    std::cout << "After 10% bonus: salary=$" << salary << std::endl;
    std::cout << "(Salary changed because function receives reference)" << std::endl;
    
    std::cout << "\n=== INCREMENT ALL ===" << std::endl;
    vector<int> scores = {85, 90, 78};
    std::cout << "Before: [";
    for (int s : scores) std::cout << s << " ";
    std::cout << "]" << std::endl;
    incrementAll(scores);
    std::cout << "After increment: [";
    for (int s : scores) std::cout << s << " ";
    std::cout << "]" << std::endl;
    std::cout << "(All incremented because loop uses references)" << std::endl;
    
    std::cout << "\n=== FIND AND MODIFY ===" << std::endl;
    vector<int> numbers = {10, 20, 30};
    std::cout << "Before: [";
    for (int n : numbers) std::cout << n << " ";
    std::cout << "]" << std::endl;
    int& found = findElement(numbers, 20);
    std::cout << "Found element 20" << std::endl;
    found = 99;
    std::cout << "Changed it to 99" << std::endl;
    std::cout << "After: [";
    for (int n : numbers) std::cout << n << " ";
    std::cout << "]" << std::endl;
    std::cout << "(Element changed because we returned a reference)" << std::endl;
    
    return 0;
}
```

**Output:**
```
=== SWAPPING ===
Before: a=5, b=10
After swap: a=10, b=5
(Both changed because swap receives references)

=== BONUS ===
Before: salary=$50000
After 10% bonus: salary=$55000
(Salary changed because function receives reference)

=== INCREMENT ALL ===
Before: [85 90 78 ]
After increment: [86 91 79 ]
(All incremented because loop uses references)

=== FIND AND MODIFY ===
Before: [10 20 30 ]
Found element 20
Changed it to 99
After: [10 99 30 ]
(Element changed because we returned a reference)
```

---

### Problem 4: Understanding Function Parameters and Scope

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <iomanip>
using std::string;
using std::vector;

// ===== MULTIPLY FUNCTIONS =====

// Version A: Pass-by-value (creates copy)
void multiplyA(int num, int factor) {
    num *= factor;  // Modifies the COPY, not the original
}

// Version B: Pass-by-reference (modifying)
void multiplyB(int& num, int factor) {
    num *= factor;  // Modifies the ORIGINAL
}

// Version C: Pass-by-const-reference (read-only)
void multiplyC(const int& num, int factor) {
    // Can't modify because const
    // int result = num * factor;  (if we needed to do something)
}

// ===== PRODUCT STRUCTURE =====

struct Product {
    string name;
    double price;
    int quantity;
    vector<double> reviews;
};

// ===== PRODUCT DISPLAY FUNCTIONS =====

// Version A: Pass-by-value (INEFFICIENT!)
void displayA(Product p) {
    std::cout << "By-Value: " << p.name << " ($" << p.price << "), Qty: " 
              << p.quantity << ", Reviews: " << p.reviews.size() << std::endl;
}

// Version B: Pass-by-const-reference (EFFICIENT, SAFE)
void displayB(const Product& p) {
    std::cout << "By-Const-Ref: " << p.name << " ($" << p.price << "), Qty: " 
              << p.quantity << ", Reviews: " << p.reviews.size() << std::endl;
}

// Version C: Pass-by-reference (FOR MODIFICATION)
void displayC(Product& p) {
    std::cout << "By-Ref: " << p.name << " ($" << p.price << "), Qty: " 
              << p.quantity << ", Reviews: " << p.reviews.size() << std::endl;
}

// Function that modifies through reference
void updatePrice(Product& p, double newPrice) {
    p.price = newPrice;
}

int main() {
    std::cout << "===== MULTIPLY FUNCTIONS =====" << std::endl;
    
    std::cout << "\n=== VERSION A: PASS-BY-VALUE ===" << std::endl;
    int value = 5;
    std::cout << "Original value: " << value << std::endl;
    std::cout << "Call multiplyA(value, 2)" << std::endl;
    std::cout << "  Function receives a COPY of value" << std::endl;
    multiplyA(value, 2);
    std::cout << "Original after function: " << value << " (UNCHANGED!)" << std::endl;
    std::cout << "Why: Function modified the copy, not the original" << std::endl;
    
    std::cout << "\n=== VERSION B: PASS-BY-REFERENCE ===" << std::endl;
    value = 5;
    std::cout << "Original value: " << value << std::endl;
    std::cout << "Call multiplyB(value, 2)" << std::endl;
    std::cout << "  Function receives a REFERENCE to value" << std::endl;
    multiplyB(value, 2);
    std::cout << "Original after function: " << value << " (CHANGED!)" << std::endl;
    std::cout << "Why: Function modified the original through the reference" << std::endl;
    
    std::cout << "\n=== VERSION C: PASS-BY-CONST-REFERENCE ===" << std::endl;
    value = 5;
    std::cout << "Original value: " << value << std::endl;
    std::cout << "Call multiplyC(value, 2)" << std::endl;
    std::cout << "  Function receives a CONST REFERENCE to value" << std::endl;
    std::cout << "  (Can't modify because of const)" << std::endl;
    multiplyC(value, 2);
    std::cout << "Original after function: " << value << " (UNCHANGED)" << std::endl;
    std::cout << "Why: const prevents modification" << std::endl;
    
    // ===== PRODUCT DISPLAY COMPARISON =====
    
    std::cout << "\n\n===== PRODUCT DISPLAY FUNCTIONS =====" << std::endl;
    
    Product laptop;
    laptop.name = "Gaming Laptop";
    laptop.price = 1299.99;
    laptop.quantity = 5;
    for (int i = 0; i < 50; i++) {  // Many reviews
        laptop.reviews.push_back(4.5);
    }
    
    std::cout << "\nProduct with 50 reviews\n" << std::endl;
    
    std::cout << "VERSION A (by-value):\n";
    std::cout << "  Creates a FULL COPY of the entire product\n";
    std::cout << "  INEFFICIENT for large data!\n";
    displayA(laptop);
    
    std::cout << "\nVERSION B (by-const-reference):\n";
    std::cout << "  Passes just a REFERENCE (no copy)\n";
    std::cout << "  EFFICIENT and SAFE\n";
    displayB(laptop);
    
    std::cout << "\nVERSION C (by-reference):\n";
    std::cout << "  Passes a REFERENCE for MODIFICATION\n";
    std::cout << "  Used when function needs to change the product\n";
    displayC(laptop);
    
    // ===== MODIFYING THROUGH REFERENCE =====
    
    std::cout << "\n=== MODIFYING THROUGH REFERENCE ===" << std::endl;
    std::cout << "Original price: $" << std::fixed << std::setprecision(2) 
              << laptop.price << std::endl;
    updatePrice(laptop, 999.99);
    std::cout << "After updatePrice(): $" << laptop.price << std::endl;
    std::cout << "Price changed because updatePrice receives a reference\n" << std::endl;
    
    // ===== SUMMARY =====
    
    std::cout << "===== SUMMARY =====" << std::endl;
    std::cout << "\nWhen to use each approach:\n";
    std::cout << "1. Pass-by-value: Small data, function doesn't modify\n";
    std::cout << "2. Pass-by-const-reference: Large data, function only reads\n";
    std::cout << "3. Pass-by-reference: Function needs to modify the data\n";
    std::cout << "\nBest practice for large structures: Use const-reference!\n";
    
    return 0;
}
```

**Output:**
```
===== MULTIPLY FUNCTIONS =====

=== VERSION A: PASS-BY-VALUE ===
Original value: 5
Call multiplyA(value, 2)
  Function receives a COPY of value
Original after function: 5 (UNCHANGED!)
Why: Function modified the copy, not the original

=== VERSION B: PASS-BY-REFERENCE ===
Original value: 5
Call multiplyB(value, 2)
  Function receives a REFERENCE to value
Original after function: 10 (CHANGED!)
Why: Function modified the original through the reference

=== VERSION C: PASS-BY-CONST-REFERENCE ===
Original value: 5
Call multiplyC(value, 2)
  Function receives a CONST REFERENCE to value
  (Can't modify because of const)
Original after function: 5 (UNCHANGED)
Why: const prevents modification


===== PRODUCT DISPLAY FUNCTIONS =====

Product with 50 reviews

VERSION A (by-value):
  Creates a FULL COPY of the entire product
  INEFFICIENT for large data!
By-Value: Gaming Laptop ($1299.99), Qty: 5, Reviews: 50

VERSION B (by-const-reference):
  Passes just a REFERENCE (no copy)
  EFFICIENT and SAFE
By-Const-Ref: Gaming Laptop ($1299.99), Qty: 5, Reviews: 50

VERSION C (by-reference):
  Passes a REFERENCE for MODIFICATION
  Used when function needs to change the product
By-Ref: Gaming Laptop ($1299.99), Qty: 5, Reviews: 50

=== MODIFYING THROUGH REFERENCE ===
Original price: $1299.99
After updatePrice(): $999.99
Price changed because updatePrice receives a reference

===== SUMMARY =====

When to use each approach:

1. Pass-by-value: Small data, function doesn't modify
2. Pass-by-const-reference: Large data, function only reads
3. Pass-by-reference: Function needs to modify the data

Best practice for large structures: Use const-reference!
```

---

### Problem 5: Vector References and Iteration

**Solution:**

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
using std::string;
using std::vector;

struct Student {
    string name;
    int id;
    double gpa;
};

// Add bonus GPA to all students
void addBonus(vector<Student>& roster, double bonusGPA) {
    for (Student& s : roster) {  // Reference to each student
        s.gpa += bonusGPA;
        if (s.gpa > 4.0) s.gpa = 4.0;  // Cap at 4.0
    }
}

// Find student by ID and return reference
Student& findStudent(vector<Student>& roster, int id) {
    for (int i = 0; i < roster.size(); i++) {
        if (roster[i].id == id) {
            return roster[i];  // Return reference to the student
        }
    }
    // (Simplified - error handling omitted)
    return roster[0];
}

// Display all students (read-only with const-reference)
void displayRoster(const vector<Student>& roster) {
    std::cout << "\n=== ALL STUDENTS ===" << std::endl;
    for (const Student& s : roster) {  // Const reference (read-only)
        std::cout << s.name << " (ID: " << s.id << ") - GPA: " 
                  << std::fixed << std::setprecision(2) << s.gpa << std::endl;
    }
}

// Update one student's GPA
void updateGPA(Student& s, double newGPA) {
    if (newGPA < 0.0 || newGPA > 4.0) {
        std::cout << "Invalid GPA!" << std::endl;
        return;
    }
    s.gpa = newGPA;
}

int main() {
    std::cout << "=== STUDENT ROSTER ===" << std::endl;
    
    // Create sample roster
    vector<Student> roster = {
        {"Alice", 101, 3.85},
        {"Bob", 102, 3.72},
        {"Charlie", 103, 3.91}
    };
    
    std::cout << "\nSample students loaded.\n" << std::endl;
    
    int choice;
    
    do {
        std::cout << "1. Display All" << std::endl;
        std::cout << "2. Add GPA Bonus" << std::endl;
        std::cout << "3. Find and Modify" << std::endl;
        std::cout << "4. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            displayRoster(roster);
            std::cout << "(Using const-reference to display - efficient, read-only)" << std::endl;
        } 
        else if (choice == 2) {
            double bonus;
            std::cout << "Add bonus (e.g., 0.1): ";
            std::cin >> bonus;
            std::cout << "Adding " << bonus << " to all GPAs..." << std::endl;
            addBonus(roster, bonus);
            std::cout << "(Using reference in loop to modify each student)" << std::endl;
        } 
        else if (choice == 3) {
            int id;
            std::cout << "Enter student ID: ";
            std::cin >> id;
            
            Student& student = findStudent(roster, id);
            std::cout << "Found: " << student.name << std::endl;
            
            double newGPA;
            std::cout << "Enter new GPA: ";
            std::cin >> newGPA;
            
            updateGPA(student, newGPA);
            std::cout << "Updated!" << std::endl;
            std::cout << "(Using returned reference to modify the student)" << std::endl;
        } 
        else if (choice == 4) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 4);
    
    return 0;
}
```

**Example Run:**
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
(Using const-reference to display - efficient, read-only)

1. Display All
2. Add GPA Bonus
3. Find and Modify
4. Exit
Choice: 2
Add bonus (e.g., 0.1): 0.1
Adding 0.1 to all GPAs...
(Using reference in loop to modify each student)

1. Display All
2. Add GPA Bonus
3. Find and Modify
4. Exit
Choice: 1

=== ALL STUDENTS ===
Alice (ID: 101) - GPA: 3.95
Bob (ID: 102) - GPA: 3.82
Charlie (ID: 103) - GPA: 4.00
(Using const-reference to display - efficient, read-only)

1. Display All
2. Add GPA Bonus
3. Find and Modify
4. Exit
Choice: 3
Enter student ID: 102
Found: Bob
Enter new GPA: 3.50
Updated!
(Using returned reference to modify the student)

1. Display All
2. Add GPA Bonus
3. Find and Modify
4. Exit
Choice: 1

=== ALL STUDENTS ===
Alice (ID: 101) - GPA: 3.95
Bob (ID: 102) - GPA: 3.50
Charlie (ID: 103) - GPA: 4.00
(Using const-reference to display - efficient, read-only)

1. Display All
2. Add GPA Bonus
3. Find and Modify
4. Exit
Choice: 4
Goodbye!
```

---
