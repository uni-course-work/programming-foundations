# Chapter 21: Pointers as a Model of Indirection

## Introduction: Indirection - Accessing Data Indirectly

In the real world, indirection means accessing something through another thing.

**Example:** If you want to visit a friend, you could:
1. **Direct:** Walk directly to their house
2. **Indirect:** Look up their address, then go to that address

Both get you to the same place, but the indirect method involves a level of indirection: you use an **address** to find them.

In C++, **pointers** are addresses. They let you store and use addresses to access data indirectly.

```cpp
int x = 5;          // Direct: x contains the value 5

int* ptr = &x;      // Indirect: ptr contains the ADDRESS of x
std::cout << *ptr;  // Use the address to access the value (5)
```

This chapter explores:
- **Understanding indirection** (addresses as intermediate step)
- **Dynamic allocation** (creating data at runtime)
- **Ownership concepts** (who manages memory?)
- **Risks of misuse** (dangers and pitfalls)
- **Alternatives** (safer approaches that replace pointers)

By the end, you'll understand pointers, their power, their danger, and when to use alternatives instead.

---

## Part 1: Understanding Pointers

### What is a Pointer?

A **pointer** is a variable that stores an **address**.

```cpp
int x = 5;           // x is an int, contains value 5
int* ptr = &x;       // ptr is a pointer to int, contains address of x
```

**Reading the syntax:**
- `int*` = "pointer to int" (the `*` means "pointer")
- `&x` = "address of x" (the `&` operator gets the address)
- `ptr = &x;` = "ptr stores the address of x"

**Using a pointer:**
```cpp
int x = 5;
int* ptr = &x;

std::cout << ptr;     // Prints address (e.g., 0x7ffc...)
std::cout << *ptr;    // Prints value (5) - * is "dereference"
```

The `*` operator (in an expression) means "the value at this address":
- `*ptr` = "the int at the address stored in ptr" = 5

### Pointer vs. Reference

You've learned about references. How are pointers different?

| References | Pointers |
|---|---|
| Always refer to valid data | Can be null (nullptr) or invalid |
| Can't be reassigned | Can point to different data |
| Automatic dereferencing | Manual dereferencing with `*` |
| Safer | More flexible but more dangerous |

**References (you know these):**
```cpp
int x = 5;
int& ref = x;  // Always refers to x
ref = 10;      // Use directly, no special syntax
```

**Pointers (learning now):**
```cpp
int x = 5;
int* ptr = &x;  // Can point to x, or other data later
*ptr = 10;      // Dereference with *, then use
```

### Memory Model for Pointers

Think of a pointer as storing a mailbox number:

```
Memory:
Address 1000: x = 5
Address 1004: ptr = 1000  ← ptr stores the address 1000

Using the pointer:
*ptr = the value at address 1000 = 5
```

The pointer `ptr` doesn't contain the value 5. It contains the **address 1000** where the value 5 is stored.

### Null Pointers

A pointer can be null (doesn't point to anything):

```cpp
int* ptr = nullptr;  // ptr is null (doesn't point anywhere)

// Don't dereference a null pointer!
std::cout << *ptr;   // UNDEFINED BEHAVIOR!
```

Always check if a pointer is valid before using it:

```cpp
int* ptr = nullptr;

if (ptr != nullptr) {
    std::cout << *ptr;  // Safe: only if ptr is valid
}

// Or more concise:
if (ptr) {
    std::cout << *ptr;  // ptr is valid
}
```

---

## Part 2: Dynamic Allocation

### Stack vs. Heap Memory (Review)

**Stack:** Local variables (automatic cleanup)
```cpp
{
    int x = 5;  // Stack
    // x exists here
}
// x destroyed, memory freed automatically
```

**Heap:** Dynamic allocation (manual management)
```cpp
{
    int* ptr = new int(5);  // Heap - creates new data
    std::cout << *ptr;      // Access the data
    delete ptr;             // Must free manually
}
```

### The `new` Operator

`new` allocates memory on the heap and returns a pointer:

```cpp
int* ptr = new int(5);  // Allocate int on heap, initialize to 5
                        // ptr stores the address
```

Memory layout:
```
Stack:               Heap:
ptr = 0x5000         0x5000: [5]
```

`ptr` (on stack) stores the address (0x5000) where the data lives on the heap.

### The `delete` Operator

`delete` frees heap memory:

```cpp
int* ptr = new int(5);
std::cout << *ptr;  // 5
delete ptr;         // Free the memory
// ptr now points to freed memory (invalid)
```

**Important:** After `delete`, the pointer is **dangling** (points to freed memory). Don't use it:

```cpp
int* ptr = new int(5);
delete ptr;
std::cout << *ptr;  // UNDEFINED BEHAVIOR! ptr is invalid
```

### Dynamic Arrays

Allocate arrays dynamically:

```cpp
int n = 5;
int* arr = new int[n];  // Array of 5 integers on heap

arr[0] = 10;
arr[1] = 20;

delete[] arr;  // delete[] for arrays (with brackets!)
```

### Example: Dynamic Strings

```cpp
std::string* str = new std::string("Hello");
std::cout << *str << std::endl;  // Dereference to use
delete str;
```

### When to Use Dynamic Allocation

Use `new`/`delete` when:
- Size unknown at compile time (coming soon in data structures)
- Data must outlive the function
- Need more memory than stack provides

**But:** Modern C++ prefers smart pointers (coming later) or vectors.

---

## Part 3: Ownership and Memory Management

### The Ownership Problem

When you allocate memory with `new`, someone must be responsible for `delete`.

**Who owns the memory?**

```cpp
int* createNumber() {
    int* ptr = new int(42);
    return ptr;  // Caller now owns this memory
}

int main() {
    int* p = createNumber();
    std::cout << *p << std::endl;
    delete p;  // Caller must delete
}
```

The function creates data on the heap, but the caller must remember to delete it. **This is error-prone.**

### Memory Leaks

**Leak:** Allocate memory but forget to `delete`.

```cpp
int* ptr = new int(5);
ptr = nullptr;  // Oops! Address lost, memory never freed
// Memory "leaks" - still allocated but inaccessible
```

Every `new` must have a corresponding `delete`. If you forget:
- Memory accumulates over time
- Program uses more memory each run
- Eventually crashes due to memory exhaustion

### Double Delete

**Error:** Delete the same memory twice.

```cpp
int* ptr = new int(5);
delete ptr;
delete ptr;  // UNDEFINED BEHAVIOR! Freeing freed memory
```

The second delete operates on freed memory (corruption).

### Use-After-Free

**Error:** Use a pointer after deleting.

```cpp
int* ptr = new int(5);
delete ptr;
std::cout << *ptr;  // UNDEFINED BEHAVIOR! ptr is invalid
```

After `delete`, the pointer becomes **dangling**. Using it is unsafe.

### Null-Pointer Assignment (Best Practice)

Assign `nullptr` after deleting to catch use-after-free:

```cpp
int* ptr = new int(5);
delete ptr;
ptr = nullptr;  // Mark as invalid

if (ptr) {
    std::cout << *ptr;  // Won't execute: ptr is nullptr
}
```

This prevents accidental use-after-free (at least the dereference would be null).

---

## Part 4: Risks and Pitfalls

### Common Pointer Errors

**Error 1: Uninitialized Pointer**
```cpp
int* ptr;  // Uninitialized! Points to random memory
*ptr = 5;  // UNDEFINED BEHAVIOR!
```

Always initialize:
```cpp
int* ptr = nullptr;  // Initialize to null
```

**Error 2: Dangling Pointer**
```cpp
int* ptr;
{
    int x = 5;
    ptr = &x;  // Points to local variable
}
// x destroyed, ptr now dangles
std::cout << *ptr;  // UNDEFINED BEHAVIOR!
```

Don't point to stack variables from outside the scope.

**Error 3: Memory Leak**
```cpp
int* ptr = new int(5);
// ... use ptr ...
// Forget to delete!
return;  // Memory leaked
```

Every `new` must have corresponding `delete`.

**Error 4: Pointer Arithmetic (Dangerous)**
```cpp
int* ptr = new int(5);
ptr = ptr + 1;  // Move to next address (what's there?)
*ptr = 10;      // Modifying unknown memory!
delete ptr;     // Wrong address!
```

Pointer arithmetic is dangerous without arrays.

**Error 5: Invalid Dereferencing**
```cpp
int* ptr = nullptr;
std::cout << *ptr;  // UNDEFINED BEHAVIOR!
```

Always check for null before dereferencing.

---

## Part 5: Why Alternatives Are Better

### The Pointer Problem

Pointers are powerful but risky:
- Easy to create memory leaks
- Easy to create dangling pointers
- Easy to access freed memory
- Hard to track ownership
- Error-prone manual management

### Alternative 1: Smart Pointers

**Smart pointers** automatically manage memory:

```cpp
std::unique_ptr<int> ptr(new int(5));
std::cout << *ptr << std::endl;
// Automatically deleted when ptr goes out of scope
```

Benefits:
- Automatic deletion (no leak)
- Clear ownership (unique_ptr owns it)
- Safe dereferencing
- No manual `delete` needed

**Coming in Chapter 22:** We'll learn smart pointers properly.

### Alternative 2: Vectors and Collections

Instead of dynamic arrays with pointers:

```cpp
// Bad: Manual array
int* arr = new int[5];
// ...use array...
delete[] arr;

// Good: Vector handles memory
std::vector<int> arr(5);
// ...use vector...
// Automatically cleaned up
```

Vectors:
- Automatic memory management
- Safe access (bounds checking with `.at()`)
- No manual `new`/`delete`
- Easier to use

### Alternative 3: Return by Value

Instead of returning pointers:

```cpp
// Bad: Caller must delete
int* createNumber() {
    return new int(42);  // Caller responsible for delete
}

// Good: Return by value
int createNumber() {
    return 42;  // Simple, safe
}
```

### Alternative 4: Pass by Reference

Instead of passing pointers:

```cpp
// Bad: Pointer parameter
void modify(int* ptr) {
    if (ptr) *ptr = 10;  // Check for null
}

// Good: Reference parameter
void modify(int& ref) {
    ref = 10;  // Always valid
}
```

References are safer: always valid, no null checks needed.

---

## Part 6: When Pointers Are Necessary

### Pointers Are Still Needed For:

**1. Data Structures**
- Linked lists (node points to next node)
- Trees (node points to children)
- Graphs (node points to neighbors)

**2. Advanced Patterns**
- Polymorphism (pointers to base class)
- Callbacks and function pointers
- C interop (C uses pointers)

**3. Legacy Code**
- Older C++ codebases use pointers
- Need to understand existing code

### Pointers in Data Structures

**Example: Linked List Node**

```cpp
struct Node {
    int data;
    Node* next;  // Points to next node
};

// Create nodes
Node* head = new Node{5, nullptr};
head->next = new Node{10, nullptr};
head->next->next = new Node{15, nullptr};

// Traverse
Node* current = head;
while (current != nullptr) {
    std::cout << current->data << " ";
    current = current->next;
}

// Cleanup (must delete all nodes!)
delete head->next->next;
delete head->next;
delete head;
```

Pointers enable flexible data structures, but ownership gets complex (must delete entire structure).

---

## Part 7: Safe Pointer Usage Guidelines

### Rule 1: Initialize All Pointers

```cpp
int* ptr = nullptr;  // Good: starts as null
int* ptr;            // Bad: uninitialized
```

### Rule 2: Check Before Dereferencing

```cpp
int* ptr = getSomePointer();

if (ptr != nullptr) {
    std::cout << *ptr;
}
```

Or use smart pointers that guarantee validity.

### Rule 3: Match `new` with `delete`

```cpp
int* ptr = new int;
delete ptr;  // Matches

int* arr = new int[10];
delete[] arr;  // Matches (with brackets for arrays)
```

### Rule 4: Don't Point to Stack Variables

```cpp
int* badPtr;
{
    int x = 5;
    badPtr = &x;  // Bad: x will be destroyed
}
// badPtr now dangles

int* goodPtr = new int(5);  // Good: heap allocation
```

### Rule 5: Avoid Dangling Pointers

```cpp
int* ptr = getSomePointer();
delete ptr;
ptr = nullptr;  // Mark invalid

if (ptr) {  // This check prevents use-after-free
    std::cout << *ptr;
}
```

### Rule 6: Use Alternatives When Possible

```cpp
// Instead of: int* arr = new int[10]; delete[] arr;
std::vector<int> arr(10);  // Safer!

// Instead of: int* ptr = new int(5); delete ptr;
std::unique_ptr<int> ptr(new int(5));  // Safer!

// Instead of: int* ptr = new int(); return ptr;
int value = 42; return value;  // Simpler!
```

---

## Part 8: Pointer Syntax and Operators

### Declaration

```cpp
int* ptr;           // Pointer to int
double* ptr;        // Pointer to double
std::string* ptr;   // Pointer to string
int** ptr;          // Pointer to pointer to int
```

The `*` is part of the type name: `int*` = "pointer to int".

### Address-of Operator `&`

```cpp
int x = 5;
int* ptr = &x;  // ptr = address of x
```

`&x` = "get the address of x"

### Dereference Operator `*`

```cpp
int x = 5;
int* ptr = &x;
std::cout << *ptr;  // Dereference: get value at address
```

`*ptr` = "the int at the address stored in ptr"

### Member Access Through Pointer

Two ways to access struct members through pointers:

```cpp
struct Point {
    int x;
    int y;
};

Point p = {3, 4};
Point* ptr = &p;

// Method 1: Dereference then access
std::cout << (*ptr).x << std::endl;  // 3

// Method 2: Arrow operator (same thing)
std::cout << ptr->x << std::endl;    // 3 (preferred)
```

The arrow `->` is shorthand for `(*ptr).`.

---

## Part 9: Conceptual Model - Indirection

### Direct Access (No Indirection)

```cpp
int x = 5;
std::cout << x;  // Access directly
```

Program finds `x` in memory and uses the value (5).

### Indirect Access (With Indirection)

```cpp
int x = 5;
int* ptr = &x;
std::cout << *ptr;  // Access through pointer
```

Program finds `ptr`, reads the address, goes to that address, finds the value (5).

**One extra step:** Address lookup.

### Why Indirection Matters

**Benefits of Indirection:**
- Flexibility: can change what pointer refers to
- Dynamic: create/destroy at runtime
- Polymorphism: point to different types
- Data structures: nodes point to other nodes

**Costs of Indirection:**
- Complexity: harder to reason about
- Danger: null, dangling, leaks
- Performance: extra memory lookup (negligible in practice)

---

## Part 10: Pointers vs. Modern Alternatives

### Scenario 1: Temporary Dynamic Data

**Old way (pointers):**
```cpp
int* ptr = new int(42);
std::cout << *ptr;
delete ptr;
```

**Modern way (smart pointers):**
```cpp
auto ptr = std::make_unique<int>(42);
std::cout << *ptr;
// Automatically deleted
```

### Scenario 2: Storing Collection of Data

**Old way (pointers):**
```cpp
int* arr = new int[n];
// ...use...
delete[] arr;
```

**Modern way (vector):**
```cpp
std::vector<int> arr(n);
// ...use...
// Automatically cleaned up
```

### Scenario 3: Returning Dynamically Created Data

**Old way (pointer):**
```cpp
int* createData() {
    return new int(42);  // Caller must delete
}
```

**Modern way (return by value):**
```cpp
int createData() {
    return 42;  // Simple, safe
}
```

### Scenario 4: Flexible Data Structure

**Old way (raw pointers):**
```cpp
struct Node {
    int data;
    Node* next;
};
// Manual management of links
```

**Modern way (smart pointers):**
```cpp
struct Node {
    int data;
    std::unique_ptr<Node> next;
};
// Automatic cleanup through hierarchy
```

---

## Part 11: Common Misconceptions

### Misconception 1: Pointers Are Always Needed

**False.** Modern C++ provides alternatives:
- Vectors for arrays
- Smart pointers for dynamic data
- References for parameters
- Return by value for simplicity

Pointers are a tool, not a requirement.

### Misconception 2: Pointers Are Too Dangerous to Use

**False.** Pointers are safe when used correctly:
- Initialize to null
- Check before dereferencing
- Understand ownership
- Prefer alternatives when available

Dangers exist, but are manageable.

### Misconception 3: Dereferencing Pointer = Getting Address

**False.** Opposites:
- `&x` = get the address of x
- `*ptr` = get the value at the address stored in ptr

They're inverse operations.

### Misconception 4: Null Pointers Are Useless

**False.** Null pointers are useful:
- Indicate "no data"
- Detect invalid pointers
- Mark end of linked lists
- Return error conditions

Null pointers are feature, not flaw.

---

## Part 12: Summary - Pointers as Indirection

### What Pointers Are

Pointers store addresses. They enable **indirection**: accessing data through a level of indirection (the address).

### Why Indirection Matters

- **Flexibility:** Can point to different data
- **Dynamic:** Allocate at runtime
- **Structures:** Nodes point to other nodes
- **Polymorphism:** Point to base classes

### Risks

- **Leaks:** Forget to delete
- **Dangling:** Use after delete
- **Null:** Dereference nullptr
- **Complexity:** Hard to track ownership

### Modern Alternatives

- **Smart Pointers:** Automatic cleanup
- **Vectors:** Dynamic arrays
- **Return by Value:** Simple, safe
- **References:** Always valid, no null

### When to Use Pointers

- Building complex data structures
- Understanding legacy code
- Specific algorithmic needs

### When to Use Alternatives

- Simple dynamic allocation
- Storing collections
- Function parameters
- Returning data

---

## Checklist: Pointer Concepts

- [ ] Do I understand what a pointer is?
- [ ] Can I declare pointers correctly?
- [ ] Do I understand `&` (address-of)?
- [ ] Do I understand `*` (dereference)?
- [ ] Do I understand `new` and `delete`?
- [ ] Do I know the risks (leaks, dangling, null)?
- [ ] Can I avoid common pointer errors?
- [ ] Do I know when alternatives are better?
- [ ] Can I read pointer syntax correctly?
- [ ] Do I understand indirection conceptually?

If you answer "yes" to most, you understand pointers.

---

## Key Terminology

| Term | Definition |
|------|-----------|
| **Pointer** | Variable storing an address |
| **Indirection** | Accessing data through an address |
| **Address-of** (`&`) | Operator getting address of variable |
| **Dereference** (`*`) | Operator getting value at address |
| **Dynamic allocation** | Creating memory at runtime with `new` |
| **Heap** | Memory pool for dynamic allocation |
| **Stack** | Memory for local variables (automatic cleanup) |
| **Ownership** | Who is responsible for `delete` |
| **Memory leak** | Allocated memory never freed |
| **Dangling pointer** | Pointer to freed memory |
| **Null pointer** | Pointer not pointing anywhere (nullptr) |
| **Smart pointer** | Pointer with automatic cleanup (modern) |
| **Use-after-free** | Using pointer after delete (error) |
| **Double delete** | Deleting same memory twice (error) |
| **Arrow operator** (`->`) | Shorthand for `(*ptr).member` |

---

## Looking Ahead

This chapter explains pointers as they exist in C++. The next chapter will show:
- **Smart Pointers:** Safe alternatives with automatic cleanup
- **Unique Ownership:** `unique_ptr` for one owner
- **Shared Ownership:** `shared_ptr` for multiple owners
- **RAII:** Resource Acquisition Is Initialization (automatic management)

Pointers are powerful, but smart pointers are the modern way to use their power safely.
