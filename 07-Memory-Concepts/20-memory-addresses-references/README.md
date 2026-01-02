# Chapter 20: Memory, Addresses, and References

## Introduction: Where Does Data Live?

Every variable you've created exists somewhere. When you write:

```cpp
int x = 5;
```

The integer `5` is stored in the computer's memory. But *where* exactly? And how does the program find it?

This chapter answers these questions with **conceptual clarity** rather than low-level technical details.

Think of memory like a row of mailboxes:

```
Mailbox 1000: [empty]
Mailbox 1001: [ 5 ]        ← x is here
Mailbox 1002: [empty]
Mailbox 1003: [ 'A' ]      ← c is here
Mailbox 1004: [empty]
Mailbox 1005: [ 3.14 ]     ← pi is here
```

Each mailbox has:
- A **location** (address: 1001, 1003, 1005)
- **Contents** (data: 5, 'A', 3.14)

Variables are names for these mailboxes. When you use `x`, the program knows to look in mailbox 1001 and find the value `5`.

In this chapter, we'll explore:
- **References as aliases** (another name for the same mailbox)
- **Memory addresses** (where data is located)
- **How data is accessed** (location → value)
- **Pointers** (advanced: storing addresses)
- **Conceptual clarity** (understanding without overwhelming complexity)

By the end of this chapter, you'll understand how memory works and how references give you powerful tools for working with data.

---

## Part 1: Memory and Storage

### Memory is Sequential Storage

Computer memory is like a long sequence of storage locations, each with:
- A **unique address** (location number)
- **Contents** (the data stored there)
- A **size** (how many bytes it occupies)

```
Address | Contents | Size
--------|----------|------
1000    | unused   | 1 byte
1001    | 5        | 4 bytes (int)
1005    | unused   | 2 bytes
1007    | 'A'      | 1 byte (char)
1008    | 3.14     | 8 bytes (double)
1016    | ...      | ...
```

When you create a variable, the program:
1. Finds available memory space
2. Stores your data there
3. Associates a name (variable name) with that location

### Each Data Type Uses Different Space

```cpp
int x = 5;           // Uses 4 bytes
double pi = 3.14;    // Uses 8 bytes
char letter = 'A';   // Uses 1 byte
bool flag = true;    // Uses 1 byte
std::string name = "Alice";  // Uses more (variable, includes text)
```

**Key insight:** The program doesn't care about the address number. It uses the *variable name* as a user-friendly way to refer to the location.

### Variables Are Memory Locations With Names

```cpp
int age = 25;
```

This creates:
- A memory location (say, address 1001)
- Stores the value 25 there
- Associates the name `age` with address 1001

When you write:
```cpp
std::cout << age << std::endl;
```

The program:
1. Looks up the address for `age` (1001)
2. Reads the contents (25)
3. Displays it (25)

---

## Part 2: References - Aliases to Data

### What is a Reference?

A **reference** is an **alias**—another name for the same data.

```cpp
int x = 5;
int& ref = x;  // ref is a reference to x
```

Now `x` and `ref` refer to the **same storage location**:

```
Address 1001: [5]
             ↑
           x and ref both point here
```

Both `x` and `ref` are names for the same mailbox. Changing one changes the other:

```cpp
int x = 5;
int& ref = x;

ref = 10;
std::cout << x << std::endl;  // Prints 10 (x changed too!)
```

### References vs. Copies

**Without a reference (copy):**
```cpp
int x = 5;
int copy = x;  // Creates a NEW location with value 5

copy = 10;
std::cout << x << std::endl;  // Prints 5 (x unchanged)
```

Memory after `int copy = x;`:
```
Address 1001: [5]    ← x
Address 1002: [5]    ← copy (separate location)
```

Change copy, x is unaffected.

**With a reference (alias):**
```cpp
int x = 5;
int& ref = x;  // Creates an ALIAS to the same location

ref = 10;
std::cout << x << std::endl;  // Prints 10 (x changed)
```

Memory after `int& ref = x;`:
```
Address 1001: [5]
             ↑
           x and ref (same location)
```

Change ref, x is automatically changed (it's the same storage).

### Reference Syntax

```cpp
int x = 5;
int& ref = x;  // & means "reference to"
```

Read this as: "ref is a reference to int x"

**Once created, a reference cannot be changed:**
```cpp
int x = 5;
int y = 10;
int& ref = x;  // ref refers to x

ref = y;  // This assigns y's VALUE (10) to x, not rebinding ref!
// ref still refers to x, which is now 10
```

---

## Part 3: Why References Matter

### References in Function Parameters

In Chapter 16, you learned about pass-by-reference:

```cpp
void addBonus(double& salary, double bonus) {
    salary += bonus;  // salary is an alias, modifies original
}

int main() {
    double pay = 50000;
    addBonus(pay, 5000);
    std::cout << pay << std::endl;  // Prints 55000 (modified!)
}
```

How it works:
- `salary` (in function) is a reference to `pay` (in main)
- Both names refer to the same storage location
- Modifying `salary` modifies `pay`

**Without reference (pass-by-value):**
```cpp
void addBonus(double salary, double bonus) {
    salary += bonus;  // salary is a COPY, doesn't modify original
}

int main() {
    double pay = 50000;
    addBonus(pay, 5000);
    std::cout << pay << std::endl;  // Prints 50000 (unchanged)
}
```

### Efficiency: Avoiding Copies

When passing large structures:

**Pass-by-value (inefficient):**
```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
    std::vector<int> grades;  // Large data
};

void printStudent(Student s) {  // Creates a COPY
    std::cout << s.name << std::endl;
}
```

Every call creates a full copy of the entire structure (expensive).

**Pass-by-reference (efficient):**
```cpp
void printStudent(const Student& s) {  // Just a reference (cheap)
    std::cout << s.name << std::endl;
}
```

Just passes a reference (no copy needed).

### Returning References

Functions can return references:

```cpp
int& findElement(std::vector<int>& v, int index) {
    return v[index];  // Return reference to element
}

int main() {
    std::vector<int> numbers = {10, 20, 30};
    int& ref = findElement(numbers, 1);  // ref refers to numbers[1]
    ref = 99;  // Modifies the vector
    std::cout << numbers[1] << std::endl;  // Prints 99
}
```

---

## Part 4: Addresses and the `&` Operator

### The Address-of Operator `&`

The `&` operator (in an expression, not a declaration) gets the **address** of a variable:

```cpp
int x = 5;
std::cout << &x << std::endl;  // Prints the address, e.g., 0x7ffc2b3d4a44
```

The address is just a number representing where in memory the variable is stored.

**Memory visualization:**
```cpp
int x = 5;
std::cout << x << std::endl;   // Prints 5 (the value)
std::cout << &x << std::endl;  // Prints 0x... (the address)
```

Output:
```
5
0x7ffc2b3d4a44
```

### Addresses Are Hardware-Dependent

The actual address numbers vary:
- Different computers have different memory layouts
- Different program runs might use different addresses
- We don't usually care about the specific number
- We care about the **concept**: data is stored at some location

**Key insight:** You rarely need to know the actual address. You use variable names instead. The `&` operator is mainly useful for understanding how references work.

### The `*` Operator (Pointer Dereference)

If you have an address, you can get the value at that address using `*`:

```cpp
int x = 5;
int* ptr = &x;        // ptr stores the address of x
std::cout << *ptr;    // * gets the value at that address (5)
```

Read `*ptr` as "the value at the address stored in ptr" (5).

**Don't worry about pointers for now.** This is mentioned for completeness. References are the tool you'll use most often.

---

## Part 5: Memory Layout and Data Organization

### Stack Memory (Where Variables Live)

Local variables are stored on the **stack**—a region of memory for temporary storage:

```cpp
int main() {
    int x = 5;           // Stack address: 1000, value: 5
    double y = 3.14;     // Stack address: 1004, value: 3.14
    std::string name = "Alice";  // Stack address: 1012, value: "Alice"
}
// When main() ends, all stack memory is freed
```

**Stack characteristics:**
- Fast access
- Automatic memory management (freed when scope ends)
- Limited size
- Variables disappear when function exits

### Heap Memory (Dynamic Storage)

Sometimes you need memory that lasts longer. **Heap** memory is allocated dynamically:

```cpp
int* ptr = new int(5);  // Allocate memory on heap
std::cout << *ptr << std::endl;
delete ptr;  // Must free memory manually
```

**Don't use this yet.** Vectors handle heap memory automatically. We mention it for understanding.

**Heap characteristics:**
- Slower access than stack
- Manual memory management (you must `delete`)
- Large size available
- Memory persists until you explicitly free it

### Smart Pointers and Vectors

Modern C++ uses **smart pointers** and **containers** (like vectors) to manage heap memory automatically:

```cpp
std::vector<int> v = {1, 2, 3};  // Memory automatically managed
```

You don't need to think about addresses or `new`/`delete`. The vector handles it.

---

## Part 6: Understanding Function Parameters

### Pass-by-Value Parameter

```cpp
void modify(int x) {
    x = 99;  // Modifies the COPY
}

int main() {
    int a = 5;
    modify(a);
    std::cout << a << std::endl;  // Prints 5 (unchanged)
}
```

**Memory:**
```
main() stack:        modify() stack:
a = 5 (addr 1000)    x = 5 (addr 2000, a COPY)
                     x = 99 (now changed to 99)
After modify():
a = 5 (unchanged)
```

The function works with a copy. Original unchanged.

### Pass-by-Reference Parameter

```cpp
void modify(int& x) {
    x = 99;  // Modifies the ORIGINAL
}

int main() {
    int a = 5;
    modify(a);
    std::cout << a << std::endl;  // Prints 99 (changed!)
}
```

**Memory:**
```
main() stack:        modify() sees:
a = 5 (addr 1000)    x is an alias to a (also addr 1000)
                     x = 99 (modifies the original)
After modify():
a = 99 (changed!)
```

The function works with the original data. Changes persist.

### Pass-by-Const-Reference (Best Practice)

```cpp
void display(const Student& s) {
    std::cout << s.name << std::endl;
    // s.name = "Bob";  // ERROR: can't modify (const)
}
```

**Benefits:**
- Efficient (no copy)
- Safe (can't accidentally modify)
- Clear intent (we're not modifying this)

**Use this for:**
- Functions that just read data
- Large structures that shouldn't be copied
- Anything in the standard library uses this pattern

---

## Part 7: Memory and Collections

### How Vectors Store Data

A **vector** is a container that stores multiple values. Internally, it:
1. Allocates memory on the heap
2. Stores elements contiguously (next to each other)
3. Automatically grows when needed

```cpp
std::vector<int> v = {10, 20, 30};

// Internal memory:
// [10] [20] [30] ...
//  ↑    ↑    ↑
// v[0] v[1] v[2]
```

When you access an element:
```cpp
v[0] = 99;  // Go to address of v[0], write 99
```

### References to Vector Elements

```cpp
std::vector<int> v = {10, 20, 30};
int& ref = v[1];  // Reference to the element (20)
ref = 99;         // Modifies v[1]
std::cout << v[1] << std::endl;  // Prints 99
```

`ref` is an alias to the same storage location as `v[1]`.

### Vectors Handle Memory Automatically

You don't think about addresses when using vectors:

```cpp
std::vector<Student> roster;
roster.push_back(student1);
roster.push_back(student2);

// Vector handles memory automatically
// You just use it like an array
std::cout << roster[0].name << std::endl;
```

The vector:
- Allocates memory for each element
- Expands when you add more
- Frees everything when the vector is destroyed

You don't worry about addresses or `new`/`delete`.

---

## Part 8: Scope and Lifetime

### Variables Have Limited Lifetimes

Variables exist only within their **scope**:

```cpp
{
    int x = 5;
    std::cout << x << std::endl;  // OK
}
// x no longer exists here
std::cout << x << std::endl;  // ERROR
```

When scope ends:
- Memory is freed
- Variable no longer exists
- References to it become invalid

### References Can Outlive Data (Danger)

```cpp
int& createRef() {
    int x = 5;
    return x;  // DANGER: returns reference to local variable!
    // x is destroyed when function exits
}

int main() {
    int& ref = createRef();  // ref points to destroyed memory!
    std::cout << *ref << std::endl;  // Undefined behavior!
}
```

**Never return a reference to a local variable.** The memory is freed when the function exits.

### Safe References

References to data that outlives the function:

```cpp
int& findElement(std::vector<int>& v, int index) {
    return v[index];  // OK: v outlives this function
}

int main() {
    std::vector<int> v = {10, 20, 30};
    int& ref = findElement(v, 1);  // Safe: v still exists
    ref = 99;
    std::cout << v[1] << std::endl;  // Prints 99
}
```

---

## Part 9: Common Patterns With References

### Pattern 1: Modify Parameter In Place

```cpp
void addBonus(double& salary, double bonus) {
    salary += bonus;
}

int main() {
    double pay = 50000;
    addBonus(pay, 5000);
    // pay is now 55000
}
```

Used for: Modifying data passed to function.

### Pattern 2: Avoid Copying Large Data

```cpp
void printStudent(const Student& s) {
    std::cout << s.name << std::endl;
}

int main() {
    Student alice = {"Alice", 101, 3.85, ...};
    printStudent(alice);  // No copy made
}
```

Used for: Efficiency with large structures.

### Pattern 3: Return Value That's Being Modified

```cpp
Student& findStudent(std::vector<Student>& roster, int id) {
    for (int i = 0; i < roster.size(); i++) {
        if (roster[i].id == id) {
            return roster[i];
        }
    }
}

int main() {
    std::vector<Student> roster = {...};
    Student& s = findStudent(roster, 101);
    s.gpa = 4.0;  // Modifies the actual student in roster
}
```

Used for: Returning something that should be modifiable.

### Pattern 4: Iteration With Modification

```cpp
std::vector<int> scores = {85, 90, 78};

for (int& score : scores) {  // Reference
    score += 5;  // Modifies each score in vector
}

// scores are now {90, 95, 83}
```

Used for: Modifying all elements in a collection.

---

## Part 10: Pointers (Advanced Preview)

### What Are Pointers?

A **pointer** is a variable that **stores an address**:

```cpp
int x = 5;
int* ptr = &x;  // ptr stores the address of x
```

Read `int*` as "pointer to int."

### Dereferencing a Pointer

Use `*` to get the value at an address:

```cpp
int x = 5;
int* ptr = &x;
std::cout << *ptr << std::endl;  // Prints 5
```

Read `*ptr` as "the int at the address stored in ptr."

### Pointers vs. References

| References | Pointers |
|---|---|
| Always refers to valid data | Can be null or invalid |
| Can't be reassigned | Can point to different data |
| Automatic dereferencing | Manual dereferencing with `*` |
| Safer | More flexible but dangerous |

**Use references** (you already know this).

**For now:** Understand that pointers exist, but you'll use references instead. Modern C++ prefers references and smart pointers.

---

## Part 11: Common Misconceptions

### Misconception 1: References Are Pointers

**Wrong:** References are different from pointers.
- References: Always valid, can't change what they refer to
- Pointers: Can be null or invalid, can point to different data

References are simpler and safer.

### Misconception 2: `&` Always Means Reference

Wrong. The `&` symbol means different things in different contexts:

```cpp
int x = 5;
int& ref = x;      // & declares a reference
std::cout << &x;   // & gets the address

void modify(int& x) {  // & declares a reference parameter
    x++;
}
```

Context matters.

### Misconception 3: Memory Addresses Matter

**Wrong:** You rarely need to know actual address numbers.

```cpp
int x = 5;
std::cout << &x << std::endl;  // Prints 0x7ffc..., but we don't care
```

The address exists, but you use variable names instead. Understanding that data *has* an address is important. Knowing the exact number is not.

### Misconception 4: References Are Slower Than Copies

**Wrong:** References are more efficient.
- Copying a large structure: copy all data (slow)
- Passing a reference: pass one address (fast)

Use references for large data!

---

## Part 12: Summary - Memory Model

### Simple Mental Model

Think of memory as a series of numbered mailboxes:

```
Mailbox 1000: [empty]
Mailbox 1001: [5]      ← x lives here
Mailbox 1002: [empty]
...
```

**Variable name** (e.g., `x`) = user-friendly name for a mailbox.
**Address** (e.g., 1001) = the mailbox number.
**Reference** (e.g., `int& ref = x`) = another name for the same mailbox.

When you use `x`, the program looks in mailbox 1001 and finds 5.
When you use `ref`, the program also looks in mailbox 1001 and finds 5.
They're the same mailbox with two names.

### Data Movement

**Copy (pass-by-value):**
- Creates a new mailbox with a copy of the data
- Changes to copy don't affect original
- Expensive for large data

**Reference (pass-by-reference):**
- Two names for the same mailbox
- Changes visible to both names
- Efficient for large data

### Scope and Lifetime

Variables exist in their scope:
```cpp
{
    int x = 5;  // x created
    // x exists here
}
// x destroyed, memory freed
```

References must refer to data that outlives them.

---

## Looking Ahead

Understanding memory and references prepares you for:
- **Chapter 21:** Pointers (advanced: storing addresses)
- **Chapter 22:** Dynamic memory (advanced: new/delete)
- **Future:** Advanced data structures (trees, graphs)

For now: References are your tool for efficient, safe data access.

---

## Checklist: Memory Concepts

- [ ] Do I understand memory is sequential storage?
- [ ] Do I understand variables are stored at addresses?
- [ ] Can I explain what a reference is?
- [ ] Can I use references in function parameters?
- [ ] Do I know when to use pass-by-value vs. pass-by-reference?
- [ ] Can I use `const` with references?
- [ ] Do I understand references avoid copying?
- [ ] Do I understand scope and lifetime?
- [ ] Do I avoid returning references to local variables?
- [ ] Can I use references in loops to modify elements?

If you answer "yes" to most, you understand memory and references.

---

## Key Terminology

| Term | Definition |
|------|-----------|
| **Memory** | Storage where data lives |
| **Address** | Location number of data in memory |
| **Variable** | Named storage location |
| **Reference** | Alias to another variable |
| **Alias** | Another name for the same data |
| **Stack** | Memory for local variables (temporary) |
| **Heap** | Memory for dynamic allocation (persistent) |
| **Scope** | Region where variable exists |
| **Lifetime** | How long a variable exists |
| **Pass-by-value** | Function receives a copy |
| **Pass-by-reference** | Function receives an alias |
| **Pointer** | Variable storing an address (advanced) |
| **Dereference** | Getting value from an address |
| **Address-of operator** (`&`) | Gets address of a variable |
| **Dereference operator** (`*`) | Gets value at an address |
