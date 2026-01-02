# Chapter 21: Problems

## Section A: Understanding Pointers

### Problem 1: Pointer Basics and Memory

Consider this code:

```cpp
int x = 5;
int y = 10;
int* ptr = &x;
```

1. What is the value of `x`? What is the value of `ptr`?
2. What does `*ptr` evaluate to?
3. Can you draw a memory diagram showing addresses, variables, and values?
4. If you change `ptr = &y;`, what does `*ptr` now evaluate to?
5. What's the difference between:
   - `std::cout << ptr;` (prints what?)
   - `std::cout << *ptr;` (prints what?)
6. What happens if you write `int* ptr;` without initialization?

---

### Problem 2: Dynamic Allocation and Cleanup

For each scenario, identify the error and fix it:

**Scenario A:**
```cpp
int* ptr = new int(5);
std::cout << *ptr;
// (program ends without delete)
```

**Scenario B:**
```cpp
int* ptr = new int(5);
delete ptr;
std::cout << *ptr;  // What's wrong?
```

**Scenario C:**
```cpp
int* arr = new int[5];
arr[0] = 10;
delete arr;  // What's wrong?
```

**Scenario D:**
```cpp
int* ptr = new int(5);
ptr = nullptr;
delete ptr;  // What's wrong?
```

**Scenario E:**
```cpp
int* ptr;
*ptr = 5;  // What's wrong?
```

For each:
- Identify the error
- Explain why it's a problem
- Show the correct code

---

## Section B: Building Programs with Pointers

### Problem 3: Dynamic Integer Operations

**Write a complete program** that:

1. Defines functions:
   - `createNumber(int value)` - allocate int on heap, return pointer
   - `doubleValue(int* ptr)` - double the value at pointer
   - `displayNumber(int* ptr)` - display with safety check
   - `freeNumber(int*& ptr)` - delete and set to null

2. In `main()`:
   - Create a number dynamically
   - Display it
   - Double it
   - Display it again
   - Free it
   - Show what happens if you try to use after free (safely check for null)

3. Demonstrates:
   - Creating pointers
   - Passing pointers to functions
   - Modifying through pointers
   - Cleanup and null safety

**Example run:**
```
=== DYNAMIC INTEGER ===
Creating number: 5
Value: 5

Doubling...
Value: 10

Freeing...
Attempting to use after free: Pointer is null

Program complete.
```

Requirements:
- Four functions using pointers
- Safe dereferencing (check for null)
- Clear before/after display
- Demonstrates creation, use, cleanup

---

### Problem 4: Linked List Implementation

**Write a complete program** that implements a simple linked list:

1. Define Node structure:
   ```cpp
   struct Node {
       int data;
       Node* next;
   };
   ```

2. Implement functions:
   - `createNode(int value)` - allocate new node
   - `pushFront(Node*& head, int value)` - add to front
   - `popFront(Node*& head)` - remove from front
   - `print(Node* head)` - display all values
   - `getSize(Node* head)` - count nodes
   - `find(Node* head, int value)` - search for value
   - `deleteList(Node*& head)` - free all nodes

3. Menu system:
   - Add value to front
   - Remove from front
   - Display list
   - Get size
   - Find value
   - Exit

4. Demonstrates:
   - Dynamic allocation
   - Pointer chains (node → next → next)
   - Traversal
   - Complex cleanup

**Example run:**
```
=== LINKED LIST ===
1. Push to front
2. Pop from front
3. Display
4. Size
5. Find
6. Exit
Choice: 1
Value: 10
Added!

Choice: 1
Value: 20
Added!

Choice: 1
Value: 15
Added!

Choice: 3
List: 15 → 20 → 10 → null

Choice: 4
Size: 3

Choice: 5
Find value: 20
Found!

Choice: 2
Removed from front
List: 20 → 10 → null

Choice: 6
Goodbye!
```

Requirements:
- Complete working linked list
- Six operations
- Dynamic node creation/deletion
- Safe traversal (check for null)
- Menu-driven interface

---

### Problem 5: Dynamic Student Records with Pointers

**Write a complete program** that:

1. Define Student structure:
   ```cpp
   struct Student {
       std::string name;
       int id;
       double gpa;
   };
   ```

2. Implement functions using pointers:
   - `createStudent(const std::string& name, int id, double gpa)` - allocate on heap
   - `displayStudent(Student* s)` - show one student (check for null)
   - `updateGPA(Student* s, double newGPA)` - modify through pointer
   - `findHighestGPA(Student** students, int count)` - return pointer to highest
   - `freeStudents(Student** students, int count)` - delete all

3. Menu system:
   - Add student (create on heap)
   - Display all
   - Update GPA (by pointer)
   - Find highest GPA
   - Exit

4. Demonstrates:
   - Dynamic allocation of structures
   - Array of pointers
   - Passing pointers to functions
   - Modifying through pointers
   - Cleanup array of pointers

**Example run:**
```
=== STUDENT RECORDS ===
1. Add Student
2. Display All
3. Update GPA
4. Highest GPA
5. Exit
Choice: 1
Name: Alice
ID: 101
GPA: 3.85
Added!

Choice: 1
Name: Bob
ID: 102
GPA: 3.72
Added!

Choice: 1
Name: Charlie
ID: 103
GPA: 3.91
Added!

Choice: 2
=== ALL STUDENTS ===
Alice (ID: 101) - GPA: 3.85
Bob (ID: 102) - GPA: 3.72
Charlie (ID: 103) - GPA: 3.91

Choice: 4
Highest GPA: Charlie (3.91)

Choice: 3
Find student by ID: 102
Update GPA to: 3.80
Updated!

Choice: 5
Goodbye!
```

Requirements:
- Dynamic student allocation
- Pointers to structures
- Array of pointers
- Safe operations (null checks)
- Complete cleanup

---

### Problem 6: Pointer Arithmetic and Arrays (Advanced)

**Write a complete program** that demonstrates pointer operations with arrays:

1. Create dynamic array:
   ```cpp
   int* arr = new int[10];
   ```

2. Implement functions:
   - `fillArray(int* arr, int size)` - fill with values
   - `displayArray(int* arr, int size)` - print using pointer arithmetic
   - `findMax(int* arr, int size)` - return pointer to max element
   - `reverseArray(int* arr, int size)` - reverse in place
   - `sum(int* arr, int size)` - sum of all elements

3. Demonstrate:
   - Pointer arithmetic (`ptr++`, `ptr[i]`, `*(ptr + i)`)
   - Traversal with pointers
   - Finding elements
   - Modifying through pointers

**Example run:**
```
=== POINTER ARITHMETIC ===
Array: 10 20 30 40 50 60 70 80 90 100

Sum: 550
Max: 100 (at position 9)

Reversed: 100 90 80 70 60 50 40 30 20 10

Double all: 200 180 160 140 120 100 80 60 40 20
```

Requirements:
- Dynamic array
- Pointer arithmetic operations
- Safe traversal
- Multiple operations
- Clear output

---