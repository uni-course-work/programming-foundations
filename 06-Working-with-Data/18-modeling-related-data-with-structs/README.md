# Chapter 18: Modeling Related Data with Structures

## Introduction: Beyond Individual Variables and Collections

So far, you've learned:
- **Variables** store individual values
- **Collections** store multiple values of the same type

But what if you need to store related values of **different types**?

Consider a student record. You need:
- Name (string)
- ID (int)
- GPA (double)
- Year (int)
- Major (string)

You could use five separate variables:
```cpp
std::string studentName = "Alice";
int studentID = 12345;
double studentGPA = 3.85;
int studentYear = 2;
std::string studentMajor = "Computer Science";
```

Or a vector of mixed types... but that's impossible in C++ (vectors hold one type).

The solution is a **structure**—a custom data type that groups related values of different types together.

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
    int year;
    std::string major;
};

Student alice = {"Alice", 12345, 3.85, 2, "Computer Science"};
```

Now all of Alice's data is organized in one place, accessible as `alice.name`, `alice.id`, etc.

In this chapter, we'll explore:
- **Structures as data models** (grouping related data)
- **Defining and using structures** (syntax and access)
- **Collections of structures** (vector of students)
- **Structures with functions** (passing, returning)
- **Real-world data modeling** (designing structures for problems)
- **Boundary with OOP** (where structures lead to classes)

By the end of this chapter, you'll understand how to model complex, real-world data with structures.

---

## Part 1: What is a Structure?

### The Problem With Separate Variables

Without structures, related data is scattered:

```cpp
// Student 1
std::string name1 = "Alice";
int id1 = 12345;
double gpa1 = 3.85;

// Student 2
std::string name2 = "Bob";
int id2 = 12346;
double gpa2 = 3.72;

// Student 3
std::string name3 = "Charlie";
int id3 = 12347;
double gpa3 = 3.91;
```

**Problems:**
- Data is scattered (not organized)
- Hard to pass to functions (three parameters each)
- Easy to lose track (which gpa goes with which name?)
- Can't easily store 100 students

### The Solution: Structures

A **structure** is a user-defined data type that groups related variables (called **members** or **fields**) of different types.

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
};
```

This defines a blueprint. Now you can create **instances** (actual students):

```cpp
Student alice = {"Alice", 12345, 3.85};
Student bob = {"Bob", 12346, 3.72};
Student charlie = {"Charlie", 12347, 3.91};
```

All related data is now together. A structure is a **container for related data**.

---

## Part 2: Defining Structures

### Structure Definition Syntax

```cpp
struct StructureName {
    type member1;
    type member2;
    type member3;
    // ... more members
};
```

**Key points:**
- `struct` keyword starts the definition
- Members can be any type (int, double, string, bool, other structures)
- Semicolon at the end (common mistake to forget this)
- Definition goes at top of file, before main()

### Common Structure Examples

**A point in 2D space:**
```cpp
struct Point {
    double x;
    double y;
};
```

**A book:**
```cpp
struct Book {
    std::string title;
    std::string author;
    int year;
    double price;
};
```

**A date:**
```cpp
struct Date {
    int day;
    int month;
    int year;
};
```

**A person:**
```cpp
struct Person {
    std::string firstName;
    std::string lastName;
    int age;
    std::string email;
};
```

**A product:**
```cpp
struct Product {
    std::string name;
    int quantity;
    double price;
    std::string category;
};
```

### Structure Naming Convention

- Use **CamelCase** (Student, not student)
- Capitalize first letter
- Meaningful names (Point, not Pt)
- Singular (Student, not Students)

---

## Part 3: Creating and Accessing Structure Members

### Creating Instances

Once defined, create instances like any variable:

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
};

int main() {
    Student alice;  // Create an instance
    return 0;
}
```

### Accessing Members

Use the dot (`.`) operator to access members:

```cpp
Student alice;
alice.name = "Alice";
alice.id = 12345;
alice.gpa = 3.85;

std::cout << alice.name << std::endl;  // Prints: Alice
std::cout << alice.gpa << std::endl;   // Prints: 3.85
```

### Initializing Structures

**Initialize empty, then assign:**
```cpp
Student alice;
alice.name = "Alice";
alice.id = 12345;
alice.gpa = 3.85;
```

**Initialize with values (order matters):**
```cpp
Student alice = {"Alice", 12345, 3.85};
```

**Initialize with named members (modern C++):**
```cpp
Student alice = {.name = "Alice", .id = 12345, .gpa = 3.85};
```

**Initialize some members:**
```cpp
Student alice = {"Alice"};  // name = "Alice", id = 0, gpa = 0.0
```

### Default Values

Uninitialized members contain garbage:

```cpp
Student alice;
std::cout << alice.gpa << std::endl;  // Garbage value!
```

**Always initialize:**
```cpp
Student alice = {};  // Zero-initialize all members
Student alice = {"Alice", 12345, 3.85};  // Initialize with values
```

---

## Part 4: Structures in Functions

### Passing Structures to Functions

Pass by value (copy):
```cpp
void printStudent(Student s) {
    std::cout << s.name << ": " << s.gpa << std::endl;
}

int main() {
    Student alice = {"Alice", 12345, 3.85};
    printStudent(alice);  // Pass the structure
}
```

Pass by reference (no copy):
```cpp
void modifyStudent(Student &s) {
    s.gpa = 4.0;  // Modify the actual student
}

int main() {
    Student alice = {"Alice", 12345, 3.85};
    modifyStudent(alice);
    std::cout << alice.gpa << std::endl;  // Prints 4.0
}
```

**When to use each:**
- **By value:** For display functions, read-only operations
- **By reference:** For modifications, efficiency with large structures

### Returning Structures

Functions can return structures:

```cpp
Student createStudent(std::string name, int id, double gpa) {
    Student s = {name, id, gpa};
    return s;
}

int main() {
    Student alice = createStudent("Alice", 12345, 3.85);
    std::cout << alice.name << std::endl;
}
```

### Structure Operations in Functions

```cpp
// Check if student is honors (GPA >= 3.8)
bool isHonors(Student s) {
    return s.gpa >= 3.8;
}

// Get letter grade from GPA
char getLetterGrade(Student s) {
    if (s.gpa >= 3.7) return 'A';
    if (s.gpa >= 3.3) return 'B';
    return 'C';
}

// Display student info
void displayStudent(Student s) {
    std::cout << "Name: " << s.name << std::endl;
    std::cout << "ID: " << s.id << std::endl;
    std::cout << "GPA: " << s.gpa << std::endl;
}

int main() {
    Student alice = {"Alice", 12345, 3.85};
    
    if (isHonors(alice)) {
        std::cout << "Honors student!" << std::endl;
    }
    
    displayStudent(alice);
}
```

---

## Part 5: Collections of Structures

### Array of Structures

Store multiple instances in an array:

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
};

int main() {
    Student roster[3] = {
        {"Alice", 12345, 3.85},
        {"Bob", 12346, 3.72},
        {"Charlie", 12347, 3.91}
    };
    
    // Access
    std::cout << roster[0].name << std::endl;  // Alice
    std::cout << roster[1].gpa << std::endl;   // 3.72
    
    // Iterate
    for (int i = 0; i < 3; i++) {
        std::cout << roster[i].name << ": " << roster[i].gpa << std::endl;
    }
}
```

### Vector of Structures

Dynamic array of structures:

```cpp
#include <vector>
using std::vector;

struct Student {
    std::string name;
    int id;
    double gpa;
};

int main() {
    vector<Student> roster;
    
    // Add students
    roster.push_back({"Alice", 12345, 3.85});
    roster.push_back({"Bob", 12346, 3.72});
    roster.push_back({"Charlie", 12347, 3.91});
    
    // Access
    std::cout << roster[0].name << std::endl;
    
    // Iterate
    for (Student s : roster) {
        std::cout << s.name << ": " << s.gpa << std::endl;
    }
}
```

### Processing Collections of Structures

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
};

// Find highest GPA student
Student findTopStudent(vector<Student>& roster) {
    Student top = roster[0];
    for (Student s : roster) {
        if (s.gpa > top.gpa) {
            top = s;
        }
    }
    return top;
}

// Count honors students
int countHonors(vector<Student>& roster) {
    int count = 0;
    for (Student s : roster) {
        if (s.gpa >= 3.8) count++;
    }
    return count;
}

// Print all students
void printRoster(vector<Student>& roster) {
    for (Student s : roster) {
        std::cout << s.name << ": " << s.gpa << std::endl;
    }
}

int main() {
    vector<Student> roster = {
        {"Alice", 12345, 3.85},
        {"Bob", 12346, 3.72},
        {"Charlie", 12347, 3.91}
    };
    
    Student top = findTopStudent(roster);
    std::cout << "Top student: " << top.name << std::endl;
    
    std::cout << "Honors: " << countHonors(roster) << std::endl;
    
    printRoster(roster);
}
```

---

## Part 6: Structures with Nested Data

### Nested Structures

Structures can contain other structures:

```cpp
struct Date {
    int day;
    int month;
    int year;
};

struct Person {
    std::string name;
    int age;
    Date birthDate;  // Structure inside structure
};

int main() {
    Person alice = {
        "Alice",
        22,
        {15, 3, 2002}  // Nested initialization
    };
    
    std::cout << alice.name << std::endl;
    std::cout << alice.birthDate.year << std::endl;  // Access nested member
}
```

### Structures with Arrays/Vectors

Structures can contain collections:

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
    int grades[5];  // Array of grades
};

int main() {
    Student alice = {
        "Alice",
        12345,
        3.85,
        {90, 85, 92, 88, 95}
    };
    
    std::cout << alice.grades[0] << std::endl;  // 90
}
```

Even better with vector:

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
    std::vector<int> grades;  // Dynamic grades
};

int main() {
    Student alice = {"Alice", 12345, 3.85, {90, 85, 92}};
    
    alice.grades.push_back(88);  // Add more grades
}
```

---

## Part 7: Real-World Data Models

### Example 1: Bank Account

```cpp
struct BankAccount {
    std::string accountHolder;
    int accountNumber;
    double balance;
    std::string type;  // "Checking" or "Savings"
};

void deposit(BankAccount &account, double amount) {
    account.balance += amount;
}

void withdraw(BankAccount &account, double amount) {
    if (amount <= account.balance) {
        account.balance -= amount;
    }
}

void displayAccount(BankAccount account) {
    std::cout << "Account: " << account.accountHolder << std::endl;
    std::cout << "Balance: $" << account.balance << std::endl;
}

int main() {
    BankAccount alice = {"Alice", 1001, 1500.0, "Checking"};
    
    deposit(alice, 500);
    withdraw(alice, 200);
    
    displayAccount(alice);
}
```

### Example 2: Product Inventory

```cpp
struct Product {
    int id;
    std::string name;
    int quantity;
    double price;
    std::string category;
};

double getTotalValue(Product p) {
    return p.quantity * p.price;
}

void restock(Product &p, int amount) {
    p.quantity += amount;
}

void sell(Product &p, int amount) {
    if (amount <= p.quantity) {
        p.quantity -= amount;
    }
}

int main() {
    Product laptop = {101, "Laptop", 15, 999.99, "Electronics"};
    
    restock(laptop, 5);
    sell(laptop, 3);
    
    std::cout << "Stock: " << laptop.quantity << std::endl;
    std::cout << "Value: $" << getTotalValue(laptop) << std::endl;
}
```

### Example 3: Employee Record

```cpp
struct Employee {
    int id;
    std::string name;
    double salary;
    int yearsEmployed;
    std::string department;
};

void giveRaise(Employee &emp, double percent) {
    emp.salary *= (1 + percent / 100.0);
}

bool isEligibleForPromotion(Employee emp) {
    return emp.yearsEmployed >= 2 && emp.salary < 150000;
}

void displayEmployee(Employee emp) {
    std::cout << emp.name << " - $" << emp.salary << std::endl;
}

int main() {
    std::vector<Employee> staff = {
        {1, "Alice", 75000, 3, "Engineering"},
        {2, "Bob", 65000, 1, "Marketing"},
        {3, "Charlie", 80000, 2, "Engineering"}
    };
    
    giveRaise(staff[0], 10);  // Give Alice 10% raise
    
    for (Employee emp : staff) {
        displayEmployee(emp);
        if (isEligibleForPromotion(emp)) {
            std::cout << "  -> Eligible for promotion" << std::endl;
        }
    }
}
```

### Example 4: Game Character

```cpp
struct Character {
    std::string name;
    int level;
    int health;
    int maxHealth;
    int mana;
    int maxMana;
    std::vector<std::string> spells;
};

void heal(Character &c, int amount) {
    c.health = std::min(c.health + amount, c.maxHealth);
}

void takeDamage(Character &c, int damage) {
    c.health -= damage;
}

bool isAlive(Character c) {
    return c.health > 0;
}

void levelUp(Character &c) {
    c.level++;
    c.maxHealth += 50;
    c.health = c.maxHealth;
    c.maxMana += 20;
    c.mana = c.maxMana;
}

int main() {
    Character hero = {
        "Aragorn",
        10,
        100,
        100,
        50,
        50,
        {"Fireball", "Heal", "Lightning"}
    };
    
    takeDamage(hero, 30);
    heal(hero, 20);
    levelUp(hero);
    
    std::cout << hero.name << " Level " << hero.level << std::endl;
    std::cout << "Health: " << hero.health << "/" << hero.maxHealth << std::endl;
}
```

---

## Part 8: Comparing Structures to Other Approaches

### Without Structures (Scattered Variables)

```cpp
// Five students
std::string name1, name2, name3, name4, name5;
int id1, id2, id3, id4, id5;
double gpa1, gpa2, gpa3, gpa4, gpa5;

// Nightmare to manage!
```

**Problems:**
- Dozens of variables
- Hard to pass to functions
- Easy to lose track
- No organization

### With Separate Collections (Not Ideal)

```cpp
std::string names[5];
int ids[5];
double gpas[5];

// Still not well-organized
// Relationship between parallel arrays not explicit
```

**Problems:**
- Parallel arrays must stay in sync
- Relationship not explicit
- Error-prone

### With Structures (Best)

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
};

Student roster[5];

// Organized, clear, safe
```

**Benefits:**
- All data for one student together
- Relationship is explicit
- Easy to pass/return
- Type-safe

---

## Part 9: Structures vs. Classes (Boundary)

Structures are the foundation for understanding **classes**, which you'll learn in future chapters.

### Similarities

Both structures and classes:
- Group related data
- Access members with dot operator
- Can contain functions
- Support inheritance

### Key Difference

**Structures:** Focus on **data** (simple containers)
```cpp
struct Point {
    double x;
    double y;
};
```

**Classes:** Focus on **behavior** (data + operations)
```cpp
class Point {
private:
    double x, y;
public:
    Point(double x, double y);
    double distance();
    // ...
};
```

### When to Use Structures

Use structures when:
- You need a simple data container
- Data has no complex operations
- You want simplicity (classes are more complex)
- Grouping related values is the main goal

Use classes when:
- You need encapsulation (hiding data)
- You need multiple operations on the data
- You need inheritance and polymorphism
- You want a more sophisticated design

For this course, structures are sufficient. Classes come later.

---

## Part 10: Common Structure Patterns

### Pattern 1: Simple Data Container

```cpp
struct Point {
    double x;
    double y;
};

struct Color {
    int red;
    int green;
    int blue;
};
```

### Pattern 2: Record with Operations

```cpp
struct Book {
    std::string title;
    std::string author;
    int year;
    double price;
};

double getDiscountedPrice(Book b, double discount) {
    return b.price * (1 - discount / 100.0);
}
```

### Pattern 3: Nested Data

```cpp
struct Address {
    std::string street;
    std::string city;
    std::string zip;
};

struct Person {
    std::string name;
    int age;
    Address address;
};
```

### Pattern 4: Collection of Related Data

```cpp
struct Car {
    std::string make;
    std::string model;
    int year;
    double mileage;
    std::vector<std::string> serviceHistory;
};
```

---

## Part 11: Example: Complete Student Management System

```cpp
#include <iostream>
#include <vector>
#include <string>
using std::vector;
using std::string;

struct Student {
    int id;
    string name;
    double gpa;
    int year;
};

// Add a student
void addStudent(vector<Student>& roster) {
    Student s;
    std::cout << "Enter ID: ";
    std::cin >> s.id;
    std::cin.ignore();
    std::cout << "Enter name: ";
    std::getline(std::cin, s.name);
    std::cout << "Enter GPA: ";
    std::cin >> s.gpa;
    std::cout << "Enter year: ";
    std::cin >> s.year;
    
    roster.push_back(s);
}

// Display all students
void displayRoster(vector<Student>& roster) {
    for (Student s : roster) {
        std::cout << s.id << " | " << s.name << " | " << s.gpa << " | " << s.year << std::endl;
    }
}

// Find student by ID
Student* findStudent(vector<Student>& roster, int id) {
    for (int i = 0; i < roster.size(); i++) {
        if (roster[i].id == id) {
            return &roster[i];
        }
    }
    return nullptr;  // Not found
}

// Calculate class average
double classAverage(vector<Student>& roster) {
    if (roster.empty()) return 0;
    
    double sum = 0;
    for (Student s : roster) {
        sum += s.gpa;
    }
    return sum / roster.size();
}

// Count honors students
int countHonors(vector<Student>& roster) {
    int count = 0;
    for (Student s : roster) {
        if (s.gpa >= 3.8) count++;
    }
    return count;
}

int main() {
    vector<Student> roster;
    int choice;
    
    do {
        std::cout << "\n=== STUDENT MANAGEMENT ===" << std::endl;
        std::cout << "1. Add Student" << std::endl;
        std::cout << "2. Display All" << std::endl;
        std::cout << "3. Find Student" << std::endl;
        std::cout << "4. Statistics" << std::endl;
        std::cout << "5. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            addStudent(roster);
        } else if (choice == 2) {
            displayRoster(roster);
        } else if (choice == 3) {
            int id;
            std::cout << "Enter ID: ";
            std::cin >> id;
            Student* found = findStudent(roster, id);
            if (found) {
                std::cout << "Found: " << found->name << std::endl;
            } else {
                std::cout << "Not found." << std::endl;
            }
        } else if (choice == 4) {
            std::cout << "Class Average: " << classAverage(roster) << std::endl;
            std::cout << "Honors Students: " << countHonors(roster) << std::endl;
        }
    } while (choice != 5);
    
    return 0;
}
```

---

## Summary: Structures Model Real-World Data

Structures are the bridge between:
- **Simple values** (integers, strings)
- **Collections** (arrays, vectors)
- **Complex systems** (classes, object-oriented design)

With structures, you can:
- Group related data meaningfully
- Pass complete records to functions
- Store collections of complex entities
- Model real-world objects (students, products, employees, characters)

Structures make programs:
- **Organized** (data logically grouped)
- **Clear** (relationships explicit)
- **Maintainable** (easy to understand and modify)
- **Scalable** (handle growing complexity)

---

## Looking Ahead

Now that you understand structures:
- **Chapter 19:** Add functions to structures (methods)
- **Chapter 20:** Classes (more sophisticated design)
- **Future:** Object-oriented programming

Structures are the foundation for understanding objects and classes.

---

## Checklist: Before Using Structures

- [ ] Do I have related values to group?
- [ ] Are they different types?
- [ ] Would this be clearer than separate variables?
- [ ] Can I design a meaningful structure?
- [ ] Should I store collections of this structure?
- [ ] What operations will this structure need?
- [ ] Will functions operate on this structure?

If you answer "yes" to most of these, structures are the right choice.

---

## Key Terminology

| Term | Definition |
|------|-----------|
| **Structure** | User-defined type grouping related variables |
| **Member** | Individual variable within a structure |
| **Instance** | Actual object created from a structure definition |
| **Dot operator** (`.`) | Access members of a structure |
| **Nested structure** | Structure containing another structure |
| **Array of structures** | Multiple instances stored in array |
| **Vector of structures** | Dynamic collection of structures |
| **Pass by value** | Function receives copy of structure |
| **Pass by reference** | Function receives access to actual structure |
| **Data model** | Design of a structure to represent real-world data |
