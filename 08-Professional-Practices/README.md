# Chapter 22: Program Correctness and Style

## Introduction: Code is Communication

When you write a program, you're not just communicating with the computer. You're communicating with humans—including your future self.

**Reality check:**
- You write code once
- Others read it many times
- You'll read your own code months later and won't remember it
- Bugs are found during reading
- Maintenance happens through understanding code

**The truth:** Code is read far more often than it is written. A program that works but is unreadable is a **failure**. A program that is clear and maintainable is **success**.

This chapter focuses on:
- **Commenting strategy** (explaining why, not what)
- **Code readability** (clear structure, meaningful names)
- **Consistent formatting** (style that doesn't distract)
- **Defensive programming** (preventing errors before they happen)
- **Testing mindset** (verifying correctness)
- **Programs as artifacts** (meant to be maintained)

By the end, you'll understand that correctness isn't just about working code—it's about code others (and you) can understand, maintain, and trust.

---

## Part 1: The Nature of Code

### Code Has Two Audiences

**Computer:**
- Must be syntactically correct
- Must run
- Must produce right output

**Humans:**
- Must understand intent
- Must follow logic
- Must find bugs
- Must maintain and extend

Most of your effort should go to the human audience. The computer doesn't care about style; humans do.

### Programs Are Artifacts

A program is like a bridge:
- It must work (structural integrity)
- It must be clear (so engineers understand it)
- It must be maintainable (so it can be repaired)
- It must document assumptions (so future engineers know constraints)

**Bad programs:**
- Work but are incomprehensible
- Break when modified
- Hide bugs until they fail catastrophically
- Document nothing about intent

**Good programs:**
- Work and are clear
- Can be extended safely
- Bugs are obvious
- Document reasoning

### The Cost of Unclear Code

**Example: Unclear Code**
```cpp
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        if (a[i][j] != 0) {
            b[i]++;
        }
    }
}
```

What does this do? Why? Is it correct?

**Example: Clear Code**
```cpp
// Count non-zero elements in each row
// b[i] = number of non-zero values in row i of matrix a
for (int row = 0; row < rows; row++) {
    for (int col = 0; col < cols; col++) {
        if (a[row][col] != 0) {
            b[row]++;
        }
    }
}
```

Same logic, but clear intent and naming.

---

## Part 2: Commenting Strategy

### Comments Explain Why, Not What

**Bad comments (explaining what code does):**
```cpp
x = x + 1;  // Increment x
y = x * 2;  // Multiply x by 2 and store in y
```

The code already says what it does! Comments should explain **why**.

**Good comments (explaining why):**
```cpp
// Shift index forward (we need the next element)
x = x + 1;

// Calculate double (for distance scaling in physics simulation)
y = x * 2;
```

Now we understand the intent.

### Types of Comments

**1. Purpose Comments (function/block level)**
```cpp
// Count how many students have GPA >= 3.5
int honors_count = 0;
for (const Student& s : students) {
    if (s.gpa >= 3.5) {
        honors_count++;
    }
}
```

Explains what the block does at a high level.

**2. Reasoning Comments (when logic isn't obvious)**
```cpp
// We check size > 2 instead of size > 1 because the last element
// is always a sentinel value we added
if (v.size() > 2) {
    // ...
}
```

Explains **why** this condition makes sense.

**3. Warning Comments (when something is tricky)**
```cpp
// WARNING: This modifies the vector in-place!
// Must be called before iterating elsewhere
void removeNegatives(std::vector<int>& v) {
    // ...
}
```

Alerts future readers to non-obvious behavior.

**4. TODO Comments (incomplete work)**
```cpp
// TODO: Add bounds checking for array access
int getValue(int index) {
    return data[index];  // Dangerous without bounds check
}
```

Marks known issues for future work.

### When NOT to Comment

**Don't comment obvious code:**
```cpp
// BAD: Comment adds nothing
x = 5;  // Set x to 5
```

**Don't duplicate documentation:**
```cpp
// BAD: Comment restates code
if (x > 0) {  // If x is greater than 0
    // ...
}
```

**Don't comment bad code—fix it:**
```cpp
// BAD: Comment trying to explain bad code
// This variable is used for temporary storage of student data
// before we process it in the next section
int tmp;  // Should be named better!

// GOOD: Rename the variable
int student_temp_data;
```

### Function Comments

Comment every function (except trivial ones):

```cpp
// Find the student with the highest GPA
// Returns: pointer to student, or nullptr if list is empty
// Notes: Does not modify the roster
Student* findTop(std::vector<Student>& roster) {
    if (roster.empty()) return nullptr;
    // ...
}
```

**What to include:**
- What the function does (one line)
- What it returns
- What it modifies (side effects)
- Assumptions or preconditions
- Any non-obvious behavior

---

## Part 3: Code Readability

### Meaningful Names

**Names are the most important form of documentation.**

**Bad names:**
```cpp
int x, y, z;              // What do these represent?
int a, b, c;              // No information
void f(int n, double d);  // Purpose unclear
```

**Good names:**
```cpp
int student_count, class_size, year;
double gpa, average_score;
void processStudent(int id, double grade);
```

**Naming conventions:**
- Variables and functions: `snake_case` (lowercase with underscores)
- Classes and structures: `PascalCase` (uppercase first letter)
- Constants: `ALL_CAPS` (uppercase with underscores)

```cpp
class StudentRecord { };           // Class
int current_gpa = 3.5;            // Variable
const int MAX_GRADE = 100;        // Constant
void displayStudent() { }          // Function
```

### Variables Should Be Specific

**Not specific:**
```cpp
int data;      // What kind of data?
double val;    // Value of what?
string temp;   // Temporary what?
```

**Specific:**
```cpp
int student_count;     // How many students
double average_gpa;    // Average grade point average
string student_name;   // Name of the student
```

### Function Names Should Describe Action

```cpp
// Good: Verb-based names
void calculateAverage() { }
bool isValid() { }
int findMaximum() { }
void displayResults() { }

// Unclear: Non-descriptive names
void process() { }   // Process what?
void run() { }       // Run what?
void do_stuff() { }  // Do what?
```

### Variable Scope Should Match Name Length

**Rule:** Wider scope = longer, clearer name.

```cpp
// OK: Loop variable, narrow scope
for (int i = 0; i < 10; i++) { }

// NOT OK: Loop variable with unclear scope
for (int i = 0; i < students.size(); i++) {
    // ... 50 lines of code ...
    // What's i again?
}

// Better: Descriptive name for complex loop
for (int student_index = 0; student_index < students.size(); student_index++) {
    // Still clear 50 lines later
}
```

---

## Part 4: Consistent Formatting

### Indentation

**Standard: 4 spaces (or 1 tab set to 4 spaces)**

```cpp
int main() {
    if (x > 5) {
        for (int i = 0; i < 10; i++) {
            std::cout << i << std::endl;
        }
    }
    return 0;
}
```

**Why it matters:**
- Shows code structure immediately
- Makes nesting depth obvious
- Helps catch unmatched braces

### Line Length

**Guideline: Keep lines under 80-100 characters**

**Why:**
- Fits on most displays
- Easier to read
- Prevents horizontal scrolling
- Prevents long-line mistakes

**If line is too long, break it:**

```cpp
// Too long on one line
result = calculateComplexValue(parameter1, parameter2, parameter3, parameter4);

// Better: Break at logical points
result = calculateComplexValue(
    parameter1,
    parameter2,
    parameter3,
    parameter4
);
```

### Spacing

**Add spaces around operators:**
```cpp
// Bad: Hard to read
x=y+z*2;
if(x>5){

// Good: Clear to read
x = y + z * 2;
if (x > 5) {
```

**Add spaces after commas:**
```cpp
// Bad: Cramped
func(a,b,c);
array[1,2,3];

// Good: Clear separation
func(a, b, c);
array[1, 2, 3];
```

### Brace Style

**Common style (Allman):**
```cpp
if (condition) {
    statement;
} else {
    statement;
}
```

**Pick one style and stick with it.** Don't mix styles in the same program.

### Blank Lines

**Use blank lines to separate logical sections:**

```cpp
// Read input
std::string name;
std::cin >> name;

// Process data
int count = countVowels(name);

// Display result
std::cout << "Vowels: " << count << std::endl;
```

Blank lines act like paragraphs in prose.

---

## Part 5: Defensive Programming

### Check Inputs

**Bad: Assumes input is valid**
```cpp
int divide(int a, int b) {
    return a / b;  // What if b is 0?
}
```

**Good: Validates inputs**
```cpp
int divide(int a, int b) {
    if (b == 0) {
        std::cout << "Error: Division by zero!" << std::endl;
        return 0;  // Or throw exception
    }
    return a / b;
}
```

### Check Array Bounds

**Bad: Assumes index is valid**
```cpp
int getValue(std::vector<int>& v, int index) {
    return v[index];  // What if index is out of bounds?
}
```

**Good: Uses safe access**
```cpp
int getValue(const std::vector<int>& v, int index) {
    if (index < 0 || index >= v.size()) {
        std::cout << "Error: Index out of bounds!" << std::endl;
        return -1;  // Or throw exception
    }
    return v[index];
}

// Or use safe accessor
return v.at(index);  // Throws exception if out of bounds
```

### Check Pointer Validity

**Bad: Assumes pointer is valid**
```cpp
void display(int* ptr) {
    std::cout << *ptr << std::endl;  // What if ptr is nullptr?
}
```

**Good: Checks for null**
```cpp
void display(int* ptr) {
    if (ptr == nullptr) {
        std::cout << "Error: Null pointer!" << std::endl;
        return;
    }
    std::cout << *ptr << std::endl;
}
```

### Use Assertions for Logic Assumptions

**Assertions document assumptions and catch violations during development:**

```cpp
#include <cassert>

int getStudent(int id) {
    // We assume id is always positive
    assert(id > 0);
    
    // Find and return student
    // ...
}
```

Assertions:
- Document assumptions clearly
- Catch bugs in development
- Disappear in release builds (no performance cost)
- Make intent explicit

### Const Correctness

**Mark things const when they shouldn't change:**

```cpp
// Bad: Suggests gpa might be modified (but it isn't)
void displayStudent(Student s) { }

// Good: Clear that student data isn't modified
void displayStudent(const Student& s) { }

// Bad: Suggests value might be modified
int getValue(std::vector<int>& v, int index) { }

// Good: Clear that vector isn't modified
int getValue(const std::vector<int>& v, int index) { }
```

**const** is a form of defensive documentation.

---

## Part 6: Testing Mindset

### Test as You Code

Don't write 1000 lines, then test. Test as you go.

**For each function:**
1. Write the function
2. Test with simple case
3. Test with edge cases
4. Test with invalid input

### Test Edge Cases

**For a function processing numbers:**

```cpp
// Test normal case
assert(getAverage({10, 20, 30}) == 20);

// Test edge cases
assert(getAverage({1}) == 1);        // Single element
assert(getAverage({}) == 0);         // Empty (or throw)
assert(getAverage({-5, -10}) == -7.5); // Negative
assert(getAverage({0, 0, 0}) == 0);  // All zeros
```

**For a function with user input:**

```cpp
// Test valid input
processStudentGrade("Alice", 95);

// Test edge cases
processStudentGrade("", 95);         // Empty name
processStudentGrade("Alice", -5);    // Invalid grade
processStudentGrade("Alice", 200);   // Out of range
```

### Print Debug Information

**During development, print what's happening:**

```cpp
void addStudent(Student s) {
    std::cout << "DEBUG: Adding student " << s.name << std::endl;
    students.push_back(s);
    std::cout << "DEBUG: Now have " << students.size() << " students" << std::endl;
}
```

This helps you understand what's happening. Remove before release.

### Use Assertions in Tests

```cpp
void testAverage() {
    // Test case 1: Normal average
    std::vector<int> scores = {80, 90, 100};
    int avg = calculateAverage(scores);
    assert(avg == 90);
    std::cout << "✓ Test 1 passed: Normal average" << std::endl;
    
    // Test case 2: Single score
    scores = {75};
    avg = calculateAverage(scores);
    assert(avg == 75);
    std::cout << "✓ Test 2 passed: Single score" << std::endl;
    
    // Test case 3: Empty (should return 0 or throw)
    scores = {};
    avg = calculateAverage(scores);
    assert(avg == 0);
    std::cout << "✓ Test 3 passed: Empty array" << std::endl;
}
```

### Types of Tests

**1. Unit Tests** (test individual functions)
```cpp
testCalculateAverage();
testFindStudent();
testAddStudent();
```

**2. Integration Tests** (test functions working together)
```cpp
void testStudentManagement() {
    // Create student, add to roster, search, modify
    Student s = createStudent("Alice", 101, 3.5);
    addStudent(s);
    Student* found = findStudent(101);
    assert(found != nullptr);
    // ...
}
```

**3. Edge Case Tests** (test boundary conditions)
```cpp
void testEdgeCases() {
    // Empty inputs, negative numbers, maximum values, etc.
}
```

---

## Part 7: Program Organization

### Logical Structure

**Organize code logically:**

```cpp
// Declarations (structures, constants)
struct Student { };
const int MAX_STUDENTS = 100;

// Helper functions (simple operations)
bool isValidGPA(double gpa) { }
std::string formatName(const std::string& name) { }

// Main functions (complex operations)
Student createStudent(/*...*/);
void addStudent(std::vector<Student>&, Student);
void displayAllStudents(const std::vector<Student>&);

// Menu system
void displayMenu() { }
int main() { }
```

Groups related code together.

### Function Size

**Keep functions small and focused:**

**Bad: 100-line function doing many things**
```cpp
void processStudents() {
    // Read input
    // Validate
    // Process
    // Save
    // Display
    // Calculate statistics
    // Generate report
}
```

**Good: Small functions doing one thing**
```cpp
void readStudents(std::vector<Student>&);
void validateStudents(std::vector<Student>&);
void processStudents(std::vector<Student>&);
void saveStudents(const std::vector<Student>&);
void displayStudents(const std::vector<Student>&);
void calculateStatistics(const std::vector<Student>&);
void generateReport(const std::vector<Student>&);
```

**Rule of thumb:** If a function doesn't fit on one screen (40-50 lines), it's too long.

### Helper Functions

Create helper functions for repeated code:

**Bad: Copy-pasted code**
```cpp
if (gpa >= 3.8) cout << name << " - Honors\n";
// ... later ...
if (gpa >= 3.8) cout << name << " - Honors\n";
// ... again ...
if (gpa >= 3.8) cout << name << " - Honors\n";
```

**Good: Helper function**
```cpp
bool isHonorsStudent(double gpa) {
    return gpa >= 3.8;
}

// Then use it:
if (isHonorsStudent(s.gpa)) cout << s.name << " - Honors\n";
```

---

## Part 8: Error Handling

### Exit Codes

**Return meaningful exit codes:**

```cpp
int main() {
    if (!readInput()) {
        std::cerr << "Error: Failed to read input" << std::endl;
        return 1;  // Non-zero = error
    }
    
    if (!processData()) {
        std::cerr << "Error: Failed to process data" << std::endl;
        return 2;  // Different error code
    }
    
    std::cout << "Success!" << std::endl;
    return 0;  // Zero = success
}
```

The shell can check the exit code:
```bash
$ ./program
$ echo $?  # Prints exit code
```

### Error Messages

**Be specific about what went wrong:**

**Bad: Vague error**
```cpp
std::cout << "Error!" << std::endl;
```

**Good: Specific error**
```cpp
std::cout << "Error: Invalid GPA " << gpa << " (must be 0.0-4.0)" << std::endl;
```

**Good: Include context**
```cpp
std::cout << "Error: Cannot add student (roster full at " << students.size() << ")" << std::endl;
```

### Exceptions (Advanced)

For more complex programs, exceptions handle errors:

```cpp
#include <stdexcept>

int divide(int a, int b) {
    if (b == 0) {
        throw std::invalid_argument("Division by zero!");
    }
    return a / b;
}

int main() {
    try {
        int result = divide(10, 0);
        std::cout << result << std::endl;
    } catch (const std::invalid_argument& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
}
```

For this course, input validation and return codes are sufficient.

---

## Part 9: Version Control and History

### Why Version Control Matters

Version control tracks changes:
- See who changed what and when
- Revert mistakes
- Understand code evolution
- Backup work

**Every program should be version controlled.**

### Meaningful Commit Messages

**Bad commit message:**
```
git commit -m "Fix stuff"
```

**Good commit message:**
```
git commit -m "Fix off-by-one error in student roster iteration

The loop was accessing one element past the end of the vector.
Changed i < count to i <= count-1 for clarity.

Fixes: Segmentation fault when displaying students"
```

**What to include:**
- What changed
- Why it changed
- What problem it solves

---

## Part 10: Documentation

### README Files

Every program should have a README:

```markdown
# Student Management System

## Description
A program for managing student records, calculating GPAs, and generating reports.

## How to Build
g++ -std=c++17 main.cpp -o student_manager

## How to Run
./student_manager

## Features
- Add/remove students
- Calculate class statistics
- Search by name or ID
- Generate honor roll

## Known Issues
- None currently

## Future Improvements
- Save/load from database
- Generate PDF reports
- Email notifications
```

### Code Headers

Include header comment at top of file:

```cpp
/*
 * student_manager.cpp
 * 
 * Implements student record management system.
 * Handles creation, searching, and reporting on student data.
 * 
 * Author: [Your Name]
 * Date: 2026-01-02
 * Status: Complete
 */

#include <iostream>
#include <vector>
#include <string>
```

---

## Part 11: The Full Picture

### A Well-Written Program

A well-written program has:

1. **Clear Intent**
   - Meaningful names
   - Good comments
   - Logical structure

2. **Defensive**
   - Input validation
   - Bounds checking
   - Error handling

3. **Testable**
   - Tested as written
   - Edge cases considered
   - Assertions verify assumptions

4. **Maintainable**
   - Consistent style
   - Reasonable function size
   - Self-documenting code

5. **Documented**
   - README file
   - Function comments
   - Inline explanations of non-obvious code

### Example: Before and After

**Before (Unclear, Unsafe, Untestable):**
```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<int> v;
    int n;
    std::cin >> n;
    for (int i = 0; i < n; i++) {
        int x;
        std::cin >> x;
        v.push_back(x);
    }
    
    int s = 0;
    for (int i = 0; i < n; i++) {
        s += v[i];
    }
    
    std::cout << s / n << std::endl;
    return 0;
}
```

**After (Clear, Safe, Tested):**
```cpp
/*
 * average_calculator.cpp
 * Reads a list of grades and calculates the average.
 */

#include <iostream>
#include <vector>
#include <cassert>

// Calculate average of a list of grades
// Precondition: grades is not empty
// Returns: average grade, or 0 if empty
double calculateAverage(const std::vector<int>& grades) {
    if (grades.empty()) {
        return 0.0;
    }
    
    int sum = 0;
    for (int grade : grades) {
        sum += grade;
    }
    return sum / static_cast<double>(grades.size());
}

// Read grades from user input
// Returns: vector of grades, or empty vector on error
std::vector<int> readGrades() {
    std::vector<int> grades;
    
    int count;
    std::cout << "How many grades? ";
    std::cin >> count;
    
    if (count <= 0) {
        std::cout << "Error: Must enter at least 1 grade" << std::endl;
        return grades;  // Empty vector
    }
    
    for (int i = 0; i < count; i++) {
        int grade;
        std::cout << "Grade " << (i + 1) << ": ";
        std::cin >> grade;
        
        if (grade < 0 || grade > 100) {
            std::cout << "Warning: Grade should be 0-100" << std::endl;
        }
        
        grades.push_back(grade);
    }
    
    return grades;
}

// Test the calculator
void testCalculateAverage() {
    assert(calculateAverage({100}) == 100.0);
    assert(calculateAverage({80, 90}) == 85.0);
    assert(calculateAverage({}) == 0.0);
    std::cout << "✓ All tests passed" << std::endl;
}

int main() {
    testCalculateAverage();
    
    std::vector<int> grades = readGrades();
    
    if (grades.empty()) {
        std::cout << "No grades entered" << std::endl;
        return 1;  // Error
    }
    
    double average = calculateAverage(grades);
    std::cout << "Average: " << average << std::endl;
    
    return 0;  // Success
}
```

**Improvements:**
- Clear variable names (`grades` not `v`)
- Comments explain intent
- Input validation
- Error messages
- Test cases
- Safe casting
- Logical organization

---

## Part 12: Summary - Correctness and Style

### Code Quality Has Layers

**Layer 1: Correctness** - Does it work?
**Layer 2: Clarity** - Can others understand it?
**Layer 3: Safety** - Does it handle errors?
**Layer 4: Maintainability** - Can it be modified?
**Layer 5: Documentation** - Is it explained?

All layers matter.

### Key Principles

1. **Names Matter** - Use clear, specific names
2. **Comments Explain Why** - Not what the code does
3. **Format for Humans** - Consistent spacing and indentation
4. **Defend Your Code** - Check inputs, handle errors
5. **Test Early** - Test as you code, not after
6. **Keep Functions Small** - One job per function
7. **Document Intent** - Comments and structure make intent clear
8. **Treat Code as Artifact** - It will be maintained by others

### The Professional Mindset

**Amateur:** "Does it work?"
**Professional:** "Can it be understood, maintained, and extended safely?"

Professional programmers care about:
- Other people reading their code
- Future maintenance
- Preventing bugs before they happen
- Making intent clear
- Creating reliable, trustworthy code

---

## Checklist: Code Quality

- [ ] Are all variable names clear and specific?
- [ ] Do all functions have comments explaining their purpose?
- [ ] Are comments explaining *why*, not *what*?
- [ ] Is indentation consistent (4 spaces)?
- [ ] Are lines under 100 characters?
- [ ] Is there space around operators?
- [ ] Do all functions validate their inputs?
- [ ] Are array/vector accesses bounds-checked?
- [ ] Are pointers checked for null?
- [ ] Are functions tested with normal and edge cases?
- [ ] Are test cases documented?
- [ ] Is there a README explaining the program?
- [ ] Is code organized logically (related code together)?
- [ ] Are functions reasonably sized (< 50 lines)?
- [ ] Is error handling clear and informative?

If you answer "yes" to most, your code is well-written.

---

## Key Terminology

| Term | Definition |
|------|-----------|
| **Code Style** | Consistent formatting and naming conventions |
| **Readability** | How easily code can be understood |
| **Defensive Programming** | Checking assumptions and handling errors |
| **Edge Case** | Boundary condition or unusual input |
| **Assertion** | Statement verifying an assumption |
| **Const Correctness** | Using const to mark unchanged data |
| **Comments** | Explanations of intent and reasoning |
| **Refactor** | Reorganize code without changing behavior |
| **Exit Code** | Return value indicating success/failure |
| **Version Control** | System tracking code changes over time |
| **Documentation** | Explanation of how to use the program |
| **Maintainability** | How easily code can be modified |
| **Unit Test** | Test of individual function |
| **Integration Test** | Test of functions working together |
| **Code Artifact** | Program as a piece meant to be maintained |

---

## Looking Ahead

As you progress:
- Chapter 23 would cover more advanced testing (frameworks, automation)
- Data Structures and Algorithms course emphasizes efficiency
- Software Engineering course focuses on large projects
- But the principles in this chapter apply everywhere

**Quality code isn't about complexity—it's about clarity.**
