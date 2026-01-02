# Chapter 21: Solutions

## Section A: Understanding Pointers

### Problem 1: Pointer Basics and Memory

**Solution:**

**1. Values:**
```cpp
int x = 5;           // x = 5 (the value)
int y = 10;
int* ptr = &x;       // ptr = 0x7ffc... (the address)
```

`x` contains 5 (a value).
`ptr` contains 1000 (or some address, not the value).

**2. Dereferencing:**
```cpp
*ptr  // = 5 (the value at the address ptr stores)
```

`*ptr` evaluates to 5 (what's stored at address 1000).

**3. Memory diagram:**
```
Address | Variable | Value
--------|----------|-------
1000    | x        | 5
1004    | y        | 10
1008    | ptr      | 1000
```

`ptr` stores 1000 (the address of x).

**4. After `ptr = &y;`:**
```cpp
ptr = &y;     // ptr now stores address of y (1004)
*ptr          // = 10 (value at address 1004)
```

**5. Output difference:**
```cpp
std::cout << ptr;   // Prints: 0x7ffc... (the address)
std::cout << *ptr;  // Prints: 10 (the value at that address)
```

**6. Uninitialized pointer:**
```cpp
int* ptr;  // DANGEROUS! ptr points to random memory
*ptr = 5;  // UNDEFINED BEHAVIOR! Modifying unknown memory
```

Should always initialize:
```cpp
int* ptr = nullptr;  // Safe default
```

---

### Problem 2: Dynamic Allocation and Cleanup

**Solution:**

**Scenario A: Memory Leak**
```cpp
int* ptr = new int(5);
std::cout << *ptr;
// (program ends - memory NEVER freed!)
```

**Error:** Memory leak. Allocated but not deleted.

**Fix:**
```cpp
int* ptr = new int(5);
std::cout << *ptr;
delete ptr;  // Free the memory!
ptr = nullptr;
```

---

**Scenario B: Use-After-Free**
```cpp
int* ptr = new int(5);
delete ptr;
std::cout << *ptr;  // UNDEFINED BEHAVIOR! ptr is dangling
```

**Error:** `ptr` is invalid after delete. Using it is undefined behavior.

**Fix:**
```cpp
int* ptr = new int(5);
std::cout << *ptr;
delete ptr;
ptr = nullptr;
// Don't use ptr after this!
```

---

**Scenario C: Wrong Delete**
```cpp
int* arr = new int[5];  // Use new[]
arr[0] = 10;
delete arr;  // WRONG! Should use delete[]
```

**Error:** Allocated with `new[]` but deleted with `delete`. Corruption.

**Fix:**
```cpp
int* arr = new int[5];
arr[0] = 10;
delete[] arr;  // Match with new[]
arr = nullptr;
```

---

**Scenario D: Deleting nullptr**
```cpp
int* ptr = new int(5);
ptr = nullptr;
delete ptr;  // Deleting nullptr is OK, but now ptr points nowhere and memory leaked
```

**Error:** Lost the address before deleting. Memory leaked.

**Fix:**
```cpp
int* ptr = new int(5);
delete ptr;  // Delete while we still have the address
ptr = nullptr;  // Then mark as null
```

---

**Scenario E: Uninitialized Pointer**
```cpp
int* ptr;  // Uninitialized! Random address
*ptr = 5;  // UNDEFINED BEHAVIOR! Modifying unknown memory
```

**Error:** `ptr` points to random memory. Modifying it corrupts something.

**Fix:**
```cpp
int* ptr = nullptr;  // Initialize to null
if (ptr == nullptr) {
    ptr = new int(5);  // Now allocate
}
*ptr = 5;  // Safe
```

---

## Section B: Building Programs with Pointers

### Problem 3: Dynamic Integer Operations

**Solution:**

```cpp
#include <iostream>
#include <iomanip>

// Create number on heap
int* createNumber(int value) {
    int* ptr = new int(value);
    return ptr;
}

// Double the value
void doubleValue(int* ptr) {
    if (ptr != nullptr) {
        *ptr *= 2;
    }
}

// Display safely
void displayNumber(int* ptr) {
    if (ptr != nullptr) {
        std::cout << "Value: " << *ptr << std::endl;
    } else {
        std::cout << "Pointer is null" << std::endl;
    }
}

// Delete and mark null
void freeNumber(int*& ptr) {
    if (ptr != nullptr) {
        delete ptr;
        ptr = nullptr;
        std::cout << "Memory freed" << std::endl;
    }
}

int main() {
    std::cout << "=== DYNAMIC INTEGER ===" << std::endl;
    
    // Create
    std::cout << "Creating number: 5" << std::endl;
    int* num = createNumber(5);
    displayNumber(num);
    
    // Use
    std::cout << "\nDoubling..." << std::endl;
    doubleValue(num);
    displayNumber(num);
    
    // Free
    std::cout << "\nFreeing..." << std::endl;
    freeNumber(num);
    
    // Try to use after free (safely)
    std::cout << "Attempting to use after free: ";
    displayNumber(num);
    
    std::cout << "\nProgram complete." << std::endl;
    return 0;
}
```

**Output:**
```
=== DYNAMIC INTEGER ===
Creating number: 5
Value: 5

Doubling...
Value: 10

Freeing...
Memory freed
Attempting to use after free: Pointer is null

Program complete.
```

---

### Problem 4: Linked List Implementation

**Solution:**

```cpp
#include <iostream>
#include <string>

struct Node {
    int data;
    Node* next;
};

// Create new node
Node* createNode(int value) {
    Node* node = new Node;
    node->data = value;
    node->next = nullptr;
    return node;
}

// Add to front
void pushFront(Node*& head, int value) {
    Node* newNode = createNode(value);
    newNode->next = head;
    head = newNode;
}

// Remove from front
void popFront(Node*& head) {
    if (head == nullptr) {
        std::cout << "List is empty!" << std::endl;
        return;
    }
    
    Node* temp = head;
    head = head->next;
    delete temp;
}

// Display list
void print(Node* head) {
    std::cout << "List: ";
    Node* current = head;
    while (current != nullptr) {
        std::cout << current->data << " → ";
        current = current->next;
    }
    std::cout << "null" << std::endl;
}

// Count nodes
int getSize(Node* head) {
    int count = 0;
    Node* current = head;
    while (current != nullptr) {
        count++;
        current = current->next;
    }
    return count;
}

// Find value
bool find(Node* head, int value) {
    Node* current = head;
    while (current != nullptr) {
        if (current->data == value) {
            return true;
        }
        current = current->next;
    }
    return false;
}

// Delete entire list
void deleteList(Node*& head) {
    while (head != nullptr) {
        Node* temp = head;
        head = head->next;
        delete temp;
    }
}

int main() {
    std::cout << "=== LINKED LIST ===" << std::endl;
    
    Node* head = nullptr;  // Empty list
    int choice;
    
    do {
        std::cout << "\n1. Push to front" << std::endl;
        std::cout << "2. Pop from front" << std::endl;
        std::cout << "3. Display" << std::endl;
        std::cout << "4. Size" << std::endl;
        std::cout << "5. Find" << std::endl;
        std::cout << "6. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            int value;
            std::cout << "Value: ";
            std::cin >> value;
            pushFront(head, value);
            std::cout << "Added!" << std::endl;
        }
        else if (choice == 2) {
            popFront(head);
            std::cout << "Removed from front" << std::endl;
        }
        else if (choice == 3) {
            print(head);
        }
        else if (choice == 4) {
            std::cout << "Size: " << getSize(head) << std::endl;
        }
        else if (choice == 5) {
            int value;
            std::cout << "Find value: ";
            std::cin >> value;
            if (find(head, value)) {
                std::cout << "Found!" << std::endl;
            } else {
                std::cout << "Not found!" << std::endl;
            }
        }
        else if (choice == 6) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 6);
    
    // Clean up
    deleteList(head);
    
    return 0;
}
```

---

### Problem 5: Dynamic Student Records with Pointers

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <iomanip>
using std::string;

struct Student {
    string name;
    int id;
    double gpa;
};

// Create student on heap
Student* createStudent(const string& name, int id, double gpa) {
    Student* s = new Student;
    s->name = name;
    s->id = id;
    s->gpa = gpa;
    return s;
}

// Display one student
void displayStudent(Student* s) {
    if (s != nullptr) {
        std::cout << s->name << " (ID: " << s->id << ") - GPA: " 
                  << std::fixed << std::setprecision(2) << s->gpa << std::endl;
    }
}

// Update GPA
void updateGPA(Student* s, double newGPA) {
    if (s != nullptr && newGPA >= 0.0 && newGPA <= 4.0) {
        s->gpa = newGPA;
    }
}

// Find highest GPA
Student* findHighestGPA(Student** students, int count) {
    if (count <= 0 || students == nullptr) return nullptr;
    
    Student* highest = students[0];
    for (int i = 1; i < count; i++) {
        if (students[i] != nullptr && students[i]->gpa > highest->gpa) {
            highest = students[i];
        }
    }
    return highest;
}

// Free all students
void freeStudents(Student** students, int count) {
    for (int i = 0; i < count; i++) {
        if (students[i] != nullptr) {
            delete students[i];
            students[i] = nullptr;
        }
    }
}

int main() {
    std::cout << "=== STUDENT RECORDS ===" << std::endl;
    
    Student** students = new Student*[10];  // Array of pointers
    int count = 0;
    
    int choice;
    
    do {
        std::cout << "\n1. Add Student" << std::endl;
        std::cout << "2. Display All" << std::endl;
        std::cout << "3. Update GPA" << std::endl;
        std::cout << "4. Highest GPA" << std::endl;
        std::cout << "5. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        std::cin.ignore();
        
        if (choice == 1) {
            if (count >= 10) {
                std::cout << "Roster full!" << std::endl;
                continue;
            }
            
            string name;
            int id;
            double gpa;
            
            std::cout << "Name: ";
            std::getline(std::cin, name);
            std::cout << "ID: ";
            std::cin >> id;
            std::cout << "GPA: ";
            std::cin >> gpa;
            
            students[count] = createStudent(name, id, gpa);
            count++;
            std::cout << "Added!" << std::endl;
        }
        else if (choice == 2) {
            std::cout << "\n=== ALL STUDENTS ===" << std::endl;
            for (int i = 0; i < count; i++) {
                displayStudent(students[i]);
            }
        }
        else if (choice == 3) {
            int id;
            double newGPA;
            
            std::cout << "Find student by ID: ";
            std::cin >> id;
            
            bool found = false;
            for (int i = 0; i < count; i++) {
                if (students[i]->id == id) {
                    std::cout << "Update GPA to: ";
                    std::cin >> newGPA;
                    updateGPA(students[i], newGPA);
                    std::cout << "Updated!" << std::endl;
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Student not found!" << std::endl;
        }
        else if (choice == 4) {
            Student* highest = findHighestGPA(students, count);
            if (highest != nullptr) {
                std::cout << "Highest GPA: ";
                displayStudent(highest);
            }
        }
        else if (choice == 5) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 5);
    
    // Clean up
    freeStudents(students, count);
    delete[] students;
    
    return 0;
}
```

---

### Problem 6: Pointer Arithmetic and Arrays

**Solution:**

```cpp
#include <iostream>
#include <iomanip>

// Fill array with values
void fillArray(int* arr, int size) {
    for (int i = 0; i < size; i++) {
        *(arr + i) = (i + 1) * 10;  // Pointer arithmetic
    }
}

// Display using pointers
void displayArray(int* arr, int size) {
    std::cout << "Array: ";
    for (int i = 0; i < size; i++) {
        std::cout << *(arr + i) << " ";  // Dereference with arithmetic
    }
    std::cout << std::endl;
}

// Find max element (return pointer)
int* findMax(int* arr, int size) {
    int* max = arr;
    for (int i = 1; i < size; i++) {
        if (*(arr + i) > *max) {
            max = arr + i;
        }
    }
    return max;
}

// Reverse array in place
void reverseArray(int* arr, int size) {
    int* start = arr;
    int* end = arr + (size - 1);
    
    while (start < end) {
        // Swap
        int temp = *start;
        *start = *end;
        *end = temp;
        
        start++;
        end--;
    }
}

// Sum all elements
int sum(int* arr, int size) {
    int total = 0;
    for (int i = 0; i < size; i++) {
        total += *(arr + i);
    }
    return total;
}

// Double all elements
void doubleAll(int* arr, int size) {
    for (int i = 0; i < size; i++) {
        *(arr + i) *= 2;
    }
}

int main() {
    std::cout << "=== POINTER ARITHMETIC ===" << std::endl;
    
    int size = 10;
    int* arr = new int[size];
    
    // Fill
    fillArray(arr, size);
    displayArray(arr, size);
    
    // Operations
    std::cout << "Sum: " << sum(arr, size) << std::endl;
    
    int* maxPtr = findMax(arr, size);
    int maxIndex = maxPtr - arr;  // Pointer difference
    std::cout << "Max: " << *maxPtr << " (at position " << maxIndex << ")" << std::endl;
    
    // Reverse
    std::cout << "\nReversed: ";
    reverseArray(arr, size);
    displayArray(arr, size);
    
    // Double
    std::cout << "Double all: ";
    doubleAll(arr, size);
    displayArray(arr, size);
    
    // Clean up
    delete[] arr;
    
    return 0;
}
```

**Output:**
```
=== POINTER ARITHMETIC ===
Array: 10 20 30 40 50 60 70 80 90 100 
Sum: 550
Max: 100 (at position 9)

Reversed: Array: 100 90 80 70 60 50 40 30 20 10 
Double all: Array: 200 180 160 140 120 100 80 60 40 20 
```

---
