# Chapter 17: Organizing Data Collections

## Introduction: The Limitation of Individual Variables

In Chapter 16, you wrote a statistics calculator that could handle exactly 5 numbers. In the Grade Management refactoring challenge, you stored exactly 10 grades using individual variables (grade1, grade2, ... grade10).

But what if you need 100 grades? 1000 test scores? A list of student names?

The answer is **collections**—data structures that store multiple values of the same type.

A **collection** is a single variable that holds multiple values, typically accessed by position. Instead of writing:
```cpp
int score1, score2, score3, score4, score5;  // 5 separate variables
```

You write:
```cpp
int scores[5];  // One variable holding 5 integers
```

Instead of:
```cpp
std::string name1, name2, name3, name4;
```

You write:
```cpp
std::string names[4];  // One variable holding 4 strings
```

In this chapter, we'll explore:
- **Collections of values** (what they are and why they matter)
- **Arrays: the fundamental collection** (fixed-size, efficient)
- **Iteration patterns** (how to process all elements)
- **Dynamic containers** (vectors: safety and flexibility)
- **When to use each** (arrays vs. vectors)

By the end of this chapter, you'll understand how to organize and process collections of data efficiently.

---

## Part 1: Collections of Values

### What is a Collection?

A **collection** is a data structure that holds multiple values under one name. Each value is accessed by its **position** (called an **index**).

**Without collections:**
```cpp
int test1 = 85;
int test2 = 90;
int test3 = 78;
int test4 = 92;

double average = (test1 + test2 + test3 + test4) / 4.0;
```

**With collections:**
```cpp
int tests[4] = {85, 90, 78, 92};

double average = (tests[0] + tests[1] + tests[2] + tests[3]) / 4.0;
```

Or better, with a loop:
```cpp
int tests[4] = {85, 90, 78, 92};
double sum = 0;
for (int i = 0; i < 4; i++) {
    sum += tests[i];
}
double average = sum / 4.0;
```

### Why Collections Matter

1. **Handle variable amounts of data** (100 scores, 1000 names)
2. **Process data with loops** (instead of duplicate code)
3. **Pass multiple values to functions** (single parameter instead of many)
4. **Organize related data** (all grades together, not scattered)

Collections are **essential for real programs**. Almost every program processes collections of data.

### Index-Based Access

Collections are accessed by **index**—the position of an element:

```cpp
int scores[5] = {85, 90, 78, 92, 88};

scores[0]  // First element: 85
scores[1]  // Second element: 90
scores[2]  // Third element: 78
scores[3]  // Fourth element: 92
scores[4]  // Fifth element: 88
scores[5]  // ERROR: out of bounds!
```

**Key point:** Indexing starts at 0, not 1. An array of 5 elements has indices 0-4.

---

## Part 2: Arrays—The Fundamental Collection

### What is an Array?

An **array** is a fixed-size collection of elements of the same type, stored in consecutive memory locations, accessed by index.

**Key characteristics:**
- **Fixed size:** Determined at creation, cannot change
- **Same type:** All elements are the same type (all ints, or all strings)
- **Contiguous:** Elements stored next to each other in memory
- **Efficient:** Fast access to any element by index

### Declaring Arrays

**Syntax:**
```cpp
type name[size];
```

**Examples:**
```cpp
int scores[5];           // Array of 5 integers
double temperatures[10]; // Array of 10 doubles
std::string names[20];   // Array of 20 strings
char letters[26];        // Array of 26 characters
bool flags[100];         // Array of 100 booleans
```

**Uninitialized arrays contain garbage values:**
```cpp
int scores[5];  // Contains unknown values!
std::cout << scores[0];  // Might print garbage
```

### Initializing Arrays

**Initialize with values:**
```cpp
int scores[5] = {85, 90, 78, 92, 88};
std::string names[3] = {"Alice", "Bob", "Charlie"};
double temperatures[4] = {98.6, 99.2, 97.8, 100.1};
```

**Initialize with default values:**
```cpp
int scores[5] = {};       // All zeros
std::string names[3] = {}; // All empty strings
bool flags[10] = {};      // All false
```

**Partial initialization:**
```cpp
int scores[5] = {85, 90};  // scores[0]=85, scores[1]=90, rest are 0
```

**Array size from initializer:**
```cpp
int scores[] = {85, 90, 78, 92, 88};  // Size is 5 (inferred)
```

### Accessing Array Elements

**Read:**
```cpp
int scores[5] = {85, 90, 78, 92, 88};
std::cout << scores[2] << std::endl;  // Prints 78
```

**Write:**
```cpp
scores[2] = 95;  // Change element at index 2
std::cout << scores[2] << std::endl;  // Prints 95
```

**In loops:**
```cpp
for (int i = 0; i < 5; i++) {
    std::cout << scores[i] << std::endl;  // Print all scores
}
```

### The Danger: Out of Bounds

Arrays don't check bounds. Accessing outside the array is undefined behavior:

```cpp
int scores[5] = {85, 90, 78, 92, 88};
std::cout << scores[10] << std::endl;  // Out of bounds! Undefined behavior!
scores[5] = 100;  // Out of bounds! Overwrites memory!
```

**Out of bounds access:**
- Might crash the program
- Might corrupt data
- Might appear to work (most dangerous)

**Protection:** Always keep track of array size and validate indices.

---

## Part 3: Iteration Patterns

### Pattern 1: Process All Elements (For Loop)

The most common pattern—loop through every element:

```cpp
int scores[5] = {85, 90, 78, 92, 88};

for (int i = 0; i < 5; i++) {
    std::cout << scores[i] << std::endl;
}
```

**Key:** Loop from 0 to size-1.

### Pattern 2: Find a Specific Element (Loop with Condition)

Search for an element meeting a condition:

```cpp
int scores[5] = {85, 90, 78, 92, 88};
int target = 90;
bool found = false;

for (int i = 0; i < 5; i++) {
    if (scores[i] == target) {
        std::cout << "Found at index " << i << std::endl;
        found = true;
        break;  // Exit loop once found
    }
}

if (!found) {
    std::cout << "Not found" << std::endl;
}
```

### Pattern 3: Transform All Elements

Modify every element:

```cpp
int scores[5] = {85, 90, 78, 92, 88};

// Add 5 points to all scores
for (int i = 0; i < 5; i++) {
    scores[i] += 5;
}
```

### Pattern 4: Accumulate (Sum, Average, Max, Min)

Calculate a single result from all elements:

```cpp
int scores[5] = {85, 90, 78, 92, 88};

// Sum
int sum = 0;
for (int i = 0; i < 5; i++) {
    sum += scores[i];
}

// Average
double average = sum / 5.0;

// Maximum
int max = scores[0];
for (int i = 1; i < 5; i++) {
    if (scores[i] > max) {
        max = scores[i];
    }
}

// Minimum
int min = scores[0];
for (int i = 1; i < 5; i++) {
    if (scores[i] < min) {
        min = scores[i];
    }
}
```

### Pattern 5: Count Elements Matching Condition

Count how many elements meet a condition:

```cpp
int scores[10] = {85, 92, 78, 88, 76, 95, 82, 91, 73, 87};

int countA = 0;  // 90+
int countB = 0;  // 80-89
int countF = 0;  // <60

for (int i = 0; i < 10; i++) {
    if (scores[i] >= 90) {
        countA++;
    } else if (scores[i] >= 80) {
        countB++;
    } else if (scores[i] < 60) {
        countF++;
    }
}
```

### Pattern 6: Build Another Array (Transformation)

Create a new array based on an existing one:

```cpp
int scores[5] = {85, 90, 78, 92, 88};
int curved[5];  // New array for curved scores

// Add 5 points to each
for (int i = 0; i < 5; i++) {
    curved[i] = scores[i] + 5;
}
```

---

## Part 4: Arrays with Functions

Arrays are particularly useful when passed to functions.

### Passing Arrays to Functions

**Arrays are passed by reference automatically** (different from other types):

```cpp
double calculateAverage(int scores[], int count) {
    int sum = 0;
    for (int i = 0; i < count; i++) {
        sum += scores[i];
    }
    return sum / (double)count;
}

int main() {
    int scores[5] = {85, 90, 78, 92, 88};
    double avg = calculateAverage(scores, 5);
    std::cout << "Average: " << avg << std::endl;
}
```

**Important:** You must pass the count separately. Arrays don't know their own size.

### Functions That Modify Arrays

Since arrays are passed by reference, functions can modify them:

```cpp
void addBonus(int scores[], int count, int bonus) {
    for (int i = 0; i < count; i++) {
        scores[i] += bonus;
    }
}

int main() {
    int scores[5] = {85, 90, 78, 92, 88};
    addBonus(scores, 5, 5);  // Add 5 points to all
    
    // Now scores are: 90, 95, 83, 97, 93
}
```

### Finding Values in Arrays

```cpp
// Returns index if found, -1 if not found
int findScore(int scores[], int count, int target) {
    for (int i = 0; i < count; i++) {
        if (scores[i] == target) {
            return i;  // Found at index i
        }
    }
    return -1;  // Not found
}

int main() {
    int scores[5] = {85, 90, 78, 92, 88};
    int index = findScore(scores, 5, 90);
    
    if (index != -1) {
        std::cout << "Found at index " << index << std::endl;
    } else {
        std::cout << "Not found" << std::endl;
    }
}
```

### Statistics Functions

```cpp
double findMaximum(int scores[], int count) {
    int max = scores[0];
    for (int i = 1; i < count; i++) {
        if (scores[i] > max) {
            max = scores[i];
        }
    }
    return max;
}

double findMinimum(int scores[], int count) {
    int min = scores[0];
    for (int i = 1; i < count; i++) {
        if (scores[i] < min) {
            min = scores[i];
        }
    }
    return min;
}

int countGrade(int scores[], int count, int minScore, int maxScore) {
    int count_in_range = 0;
    for (int i = 0; i < count; i++) {
        if (scores[i] >= minScore && scores[i] <= maxScore) {
            count_in_range++;
        }
    }
    return count_in_range;
}
```

---

## Part 5: Dynamic Containers (Vectors)

### The Limitation of Fixed Arrays

Arrays have a major limitation: **their size is fixed at compile time**.

```cpp
int scores[5];  // Always exactly 5 scores, no more, no less
```

What if you need:
- Read N scores, where N comes from the user?
- Add more scores as the program runs?
- Handle variable amounts of data?

Arrays can't do this. **Vectors** can.

### What is a Vector?

A **vector** is a dynamic array—a collection that grows and shrinks as needed. It's from the Standard Library (in `<vector>`).

**Key characteristics:**
- **Dynamic size:** Grows as you add elements
- **Safe:** Bounds checking available
- **Flexible:** Add/remove elements easily
- **Same efficiency:** Fast access by index

### Using Vectors

**Include the library:**
```cpp
#include <vector>
using std::vector;  // Or std::vector each time
```

**Declare:**
```cpp
vector<int> scores;           // Empty vector of ints
vector<std::string> names;    // Empty vector of strings
vector<double> temperatures;  // Empty vector of doubles
```

**Add elements:**
```cpp
vector<int> scores;
scores.push_back(85);   // Add 85 to the end
scores.push_back(90);   // Add 90 to the end
scores.push_back(78);   // Add 78 to the end

// Now scores contains: 85, 90, 78
```

**Access elements:**
```cpp
std::cout << scores[0] << std::endl;  // Print 85
std::cout << scores[1] << std::endl;  // Print 90
```

**Get size:**
```cpp
int size = scores.size();  // Returns 3

for (int i = 0; i < scores.size(); i++) {
    std::cout << scores[i] << std::endl;
}
```

**Check if empty:**
```cpp
if (scores.empty()) {
    std::cout << "No scores yet" << std::endl;
}
```

**Initialize with values:**
```cpp
vector<int> scores = {85, 90, 78, 92, 88};
```

### Vector vs. Array Comparison

| Feature | Array | Vector |
|---------|-------|--------|
| **Size** | Fixed at creation | Dynamic, grows as needed |
| **Declaration** | `int arr[5]` | `vector<int> v` |
| **Adding elements** | Can't (fixed size) | `.push_back()` |
| **Bounds checking** | None (dangerous) | Optional `.at()` |
| **Access** | `arr[i]` (fast) | `v[i]` (fast) |
| **Size function** | None (must track) | `.size()` (built-in) |
| **Memory** | Stack (small arrays) | Heap (can be large) |

### When to Use Each

**Use Arrays when:**
- Size is known and small
- You need maximum efficiency
- Legacy code compatibility
- Simple, fixed-size data

**Use Vectors when:**
- Size is unknown or variable
- You need to add/remove elements
- You want safety (bounds checking)
- You're writing modern C++

**Real recommendation:** Use vectors by default. Arrays only when there's a specific reason.

---

## Part 6: Common Vector Operations

### Adding and Removing Elements

```cpp
vector<int> scores;

// Add elements
scores.push_back(85);
scores.push_back(90);
scores.push_back(78);

// Remove last element
scores.pop_back();  // Removes 78

// Now scores: 85, 90
```

### Accessing Elements Safely

**Unsafe (like arrays):**
```cpp
scores[10];  // Out of bounds! Undefined behavior if vector has < 11 elements
```

**Safe (vectors only):**
```cpp
scores.at(10);  // Throws exception if out of bounds (safer)
```

### Clearing a Vector

```cpp
scores.clear();  // Remove all elements
std::cout << scores.size() << std::endl;  // Prints 0
```

### Iterating Through a Vector

**Index-based (like arrays):**
```cpp
for (int i = 0; i < scores.size(); i++) {
    std::cout << scores[i] << std::endl;
}
```

**Range-based (modern C++, easier):**
```cpp
for (int score : scores) {
    std::cout << score << std::endl;
}
```

---

## Part 7: Practical Example: Grade Collection and Analysis

### Using Arrays

```cpp
#include <iostream>

int main() {
    int scores[100];
    int count = 0;
    
    // Read scores
    std::cout << "Enter scores (enter -1 to stop):" << std::endl;
    int score;
    do {
        std::cin >> score;
        if (score != -1 && count < 100) {
            scores[count] = score;
            count++;
        }
    } while (score != -1 && count < 100);
    
    if (count == 0) {
        std::cout << "No scores entered." << std::endl;
        return 0;
    }
    
    // Calculate statistics
    int sum = 0;
    int max = scores[0];
    int min = scores[0];
    
    for (int i = 0; i < count; i++) {
        sum += scores[i];
        if (scores[i] > max) max = scores[i];
        if (scores[i] < min) min = scores[i];
    }
    
    double average = sum / (double)count;
    
    // Display results
    std::cout << "\n=== STATISTICS ===" << std::endl;
    std::cout << "Count: " << count << std::endl;
    std::cout << "Sum: " << sum << std::endl;
    std::cout << "Average: " << average << std::endl;
    std::cout << "Maximum: " << max << std::endl;
    std::cout << "Minimum: " << min << std::endl;
    
    return 0;
}
```

### Using Vectors (Simpler)

```cpp
#include <iostream>
#include <vector>
using std::vector;

int main() {
    vector<int> scores;
    
    // Read scores
    std::cout << "Enter scores (enter -1 to stop):" << std::endl;
    int score;
    do {
        std::cin >> score;
        if (score != -1) {
            scores.push_back(score);  // Just add it, no size limit!
        }
    } while (score != -1);
    
    if (scores.empty()) {
        std::cout << "No scores entered." << std::endl;
        return 0;
    }
    
    // Calculate statistics
    int sum = 0;
    int max = scores[0];
    int min = scores[0];
    
    for (int score : scores) {  // Range-based loop, simpler!
        sum += score;
        if (score > max) max = score;
        if (score < min) min = score;
    }
    
    double average = sum / (double)scores.size();
    
    // Display results
    std::cout << "\n=== STATISTICS ===" << std::endl;
    std::cout << "Count: " << scores.size() << std::endl;
    std::cout << "Sum: " << sum << std::endl;
    std::cout << "Average: " << average << std::endl;
    std::cout << "Maximum: " << max << std::endl;
    std::cout << "Minimum: " << min << std::endl;
    
    return 0;
}
```

**Notice:** The vector version is:
- Shorter
- No manual count tracking
- Automatically handles any number of scores
- No risk of exceeding array bounds
- Cleaner loop syntax

---

## Part 8: 2D Collections (Arrays of Arrays)

### What is a 2D Array?

Sometimes you need a table or grid—a collection of collections.

```cpp
int matrix[3][4];  // 3 rows, 4 columns
```

This is like:
```
[0,0] [0,1] [0,2] [0,3]
[1,0] [1,1] [1,2] [1,3]
[2,0] [2,1] [2,2] [2,3]
```

### Using 2D Arrays

**Access:**
```cpp
int matrix[3][4];

matrix[0][0] = 5;     // First row, first column
matrix[1][2] = 10;    // Second row, third column
matrix[2][3] = 15;    // Third row, fourth column
```

**Initialize:**
```cpp
int matrix[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
```

**Iterate (nested loops):**
```cpp
for (int row = 0; row < 3; row++) {
    for (int col = 0; col < 4; col++) {
        std::cout << matrix[row][col] << " ";
    }
    std::cout << std::endl;
}
```

### 2D Vectors

```cpp
#include <vector>
using std::vector;

vector<vector<int>> matrix;

// Add a row
matrix.push_back({1, 2, 3});
matrix.push_back({4, 5, 6});

// Access
matrix[0][0] = 10;  // First row, first column

// Iterate
for (int row = 0; row < matrix.size(); row++) {
    for (int col = 0; col < matrix[row].size(); col++) {
        std::cout << matrix[row][col] << " ";
    }
    std::cout << std::endl;
}
```

---

## Part 9: Common Collection Mistakes

### Mistake 1: Going Out of Bounds

```cpp
// WRONG
int scores[5] = {85, 90, 78, 92, 88};
scores[5] = 100;  // Out of bounds!
```

### Mistake 2: Forgetting Indices Start at 0

```cpp
// WRONG: Thinking first element is at index 1
int scores[5] = {85, 90, 78, 92, 88};
std::cout << scores[1] << std::endl;  // Prints 90, not 85!

// CORRECT
std::cout << scores[0] << std::endl;  // Prints 85
```

### Mistake 3: Forgetting Array Size in Loops

```cpp
// WRONG: Hard-coded size, easy to make mistakes
int scores[5] = {85, 90, 78, 92, 88};
for (int i = 0; i < 10; i++) {  // Oops! Array only has 5 elements!
    std::cout << scores[i] << std::endl;
}

// CORRECT: Use actual size
for (int i = 0; i < 5; i++) {
    std::cout << scores[i] << std::endl;
}
```

### Mistake 4: Not Tracking Vector Size

```cpp
vector<int> scores;
scores.push_back(85);
scores.push_back(90);

// WRONG: Assuming we know the size
int third = scores[2];  // Out of bounds!

// CORRECT: Use size()
if (scores.size() > 2) {
    int third = scores[2];
}
```

### Mistake 5: Modifying Vector While Iterating

```cpp
vector<int> scores = {85, 90, 78, 92, 88};

// WRONG: Modifying while iterating
for (int i = 0; i < scores.size(); i++) {
    if (scores[i] < 80) {
        scores.erase(scores.begin() + i);  // Dangerous!
    }
}

// BETTER: Collect indices to remove, then remove
```

---

## Part 10: Collections in Functions

### Passing to Functions

```cpp
// Array version (must pass size separately)
double calculateAverage(int scores[], int count) {
    int sum = 0;
    for (int i = 0; i < count; i++) {
        sum += scores[i];
    }
    return sum / (double)count;
}

// Vector version (size included)
double calculateAverage(vector<int>& scores) {
    int sum = 0;
    for (int score : scores) {
        sum += score;
    }
    return sum / (double)scores.size();
}

int main() {
    // Array usage
    int arr[5] = {85, 90, 78, 92, 88};
    double avg1 = calculateAverage(arr, 5);
    
    // Vector usage
    vector<int> vec = {85, 90, 78, 92, 88};
    double avg2 = calculateAverage(vec);
}
```

### Processing Collections with Functions

```cpp
void printScores(vector<int>& scores) {
    for (int score : scores) {
        std::cout << score << " ";
    }
    std::cout << std::endl;
}

void addBonus(vector<int>& scores, int bonus) {
    for (int i = 0; i < scores.size(); i++) {
        scores[i] += bonus;
    }
}

int findHighest(vector<int>& scores) {
    int max = scores[0];
    for (int score : scores) {
        if (score > max) max = score;
    }
    return max;
}

int main() {
    vector<int> scores = {85, 90, 78, 92, 88};
    
    printScores(scores);      // 85 90 78 92 88
    addBonus(scores, 5);      // Add 5 points to all
    printScores(scores);      // 90 95 83 97 93
    std::cout << "Highest: " << findHighest(scores) << std::endl;  // 97
}
```

---

## Summary: Collections Enable Practical Programming

Collections are **essential for real programming**. Without them, you can't:
- Process variable amounts of data
- Use loops to handle repetition
- Write reusable functions
- Build realistic programs

**The progression:**
- **Arrays:** Fixed-size, efficient, low-level control
- **Vectors:** Dynamic-size, flexible, modern C++

**Best practice:** Use vectors for most new code. Use arrays for specific reasons (embedded systems, competitive programming, interfacing with C code).

---

## Looking Ahead

Now that you understand collections, you can:
- **Chapter 18:** Process files (which contain collections of data)
- **Chapter 19:** Use standard library algorithms (sort, find, etc.)
- **Future chapters:** Advanced data structures (linked lists, trees, graphs)

Collections + functions = **the foundation of all real programs**.

---

## Checklist: Before Using Collections

- [ ] Do I need to store multiple values?
- [ ] Are they the same type?
- [ ] Do I know the size in advance (use array) or not (use vector)?
- [ ] Have I initialized my collection properly?
- [ ] Do I understand indexing starts at 0?
- [ ] Will I stay within bounds?
- [ ] Have I considered what data structure fits my use case?
- [ ] Can I write a loop to process all elements?
- [ ] Can I extract common operations into functions?

If you can answer these confidently, you're ready to use collections effectively.

---

## Key Terminology

| Term | Definition |
|------|-----------|
| **Collection** | Data structure holding multiple values of same type |
| **Array** | Fixed-size collection with indices 0 to size-1 |
| **Index** | Position of element in collection (starts at 0) |
| **Element** | Individual value in a collection |
| **Bounds** | Range of valid indices (0 to size-1) |
| **Out of bounds** | Accessing invalid index (undefined behavior) |
| **Vector** | Dynamic collection from Standard Library |
| **push_back()** | Add element to end of vector |
| **pop_back()** | Remove element from end of vector |
| **size()** | Number of elements in collection |
| **Iteration** | Processing all elements in a collection |
| **2D Array** | Array of arrays (rows and columns) |
