# Chapter 17: Problems

## Section A: Understanding Collections and Indexing

### Problem 1: Array Basics and Indexing

Consider this program:

```cpp
#include <iostream>

int main() {
    int scores[5] = {85, 90, 78, 92, 88};
    
    std::cout << scores[0] << std::endl;  // Line A
    std::cout << scores[2] << std::endl;  // Line B
    std::cout << scores[4] << std::endl;  // Line C
    
    scores[1] = 95;
    std::cout << scores[1] << std::endl;  // Line D
    
    return 0;
}
```

1. What does each line (A, B, C, D) output?
2. Why does the first element use index 0, not index 1?
3. What would `scores[5]` do? Why is it dangerous?
4. What is stored in the array after the program modifies scores[1]?
5. How would you modify the program to print all five scores?
6. What is the relationship between array size and valid indices?

---

### Problem 2: Iteration Patterns with Arrays

For each scenario, identify which iteration pattern from Chapter 17 you would use:

1. Print all test scores
2. Find the highest score
3. Add 5 points to every score
4. Count how many scores are A's (90+)
5. Create a curved array (add 5 points to each)
6. Find the index of a specific score
7. Calculate the average of all scores

For each:
- Name the pattern
- Write the code
- Explain why that pattern is appropriate

---

## Section B: Building Programs with Arrays

### Problem 3: Create a Simple Grade Tracker

**Write a complete program** that:

1. Declares an array to store up to 10 test scores
2. Asks the user how many scores they want to enter (1-10)
3. Reads that many scores with validation (0-100)
4. Calculates and displays:
   - Sum of all scores
   - Average
   - Highest score (and which position)
   - Lowest score (and which position)
   - Count of A's (90-100), B's (80-89), C's (70-79), D's (60-69), F's (<60)

**Example run:**
```
GRADE TRACKER
==============
How many scores? 5
Enter score 1: 85
Enter score 2: 92
Enter score 3: 78
Enter score 4: 88
Enter score 5: 95

=== ANALYSIS ===
Sum: 438
Average: 87.6
Highest: 95 (at position 5)
Lowest: 78 (at position 3)

Grade Distribution:
A's (90-100): 2
B's (80-89): 2
C's (70-79): 1
D's (60-69): 0
F's (<60): 0
```

Requirements:
- Use array (not vector yet)
- Validate scores 0-100
- Validate count 1-10
- Find max and min with their positions
- Count grades in each category

---

### Problem 4: Create a Student Grade Report System

**Write a complete program** that:

1. Uses two parallel arrays:
   - `names[]` to store student names
   - `grades[]` to store their grades
2. Asks how many students (2-20)
3. For each student: gets name and grade (0-100)
4. Finds and displays:
   - Class average
   - Highest grade (student name and grade)
   - Lowest grade (student name and grade)
   - All students sorted by performance (best to worst)
   - Count of passing students (60+)

**Example run:**
```
STUDENT GRADE REPORT
====================
How many students? 3

Student 1 name: Alice
Student 1 grade: 85

Student 2 name: Bob
Student 2 grade: 72

Student 3 name: Charlie
Student 3 grade: 91

=== CLASS REPORT ===
Class Average: 82.67

Highest Grade: Charlie (91)
Lowest Grade: Bob (72)

Students by Performance:
1. Charlie: 91
2. Alice: 85
3. Bob: 72

Passing: 3 students

```

Requirements:
- Use two parallel arrays (names and grades)
- Validate grades 0-100
- Find max and min with names
- Sort/display in order (best to worst)
- Count students passing (60+)

---

## Section C: Vectors and Dynamic Collections

### Problem 5: Create a Dynamic Grade Manager with Vectors

**Write a complete program** that:

1. Uses vectors to store names and grades dynamically
2. Displays a menu:
   - 1. Add Student
   - 2. Display All Students
   - 3. Find Student by Name
   - 4. Display Statistics
   - 5. Exit
3. For each option:
   - Add: Get name and grade, add to vectors
   - Display: Show all students with grades
   - Find: Search by name, display grade
   - Statistics: Average, highest, lowest, count
   - Exit: Quit program

**Example run:**
```
=== STUDENT GRADE MANAGER ===
1. Add Student
2. Display All Students
3. Find Student
4. Display Statistics
5. Exit
Choice: 1

Student name: Alice
Student grade: 85
Added successfully!

[Menu repeats]
Choice: 1
Student name: Bob
Student grade: 92
Added successfully!

Choice: 2

=== ALL STUDENTS ===
1. Alice: 85
2. Bob: 92

Choice: 3
Enter name to find: Alice
Alice: 85

Choice: 4

=== STATISTICS ===
Count: 2
Average: 88.5
Highest: Bob (92)
Lowest: Alice (85)

Choice: 5
Goodbye!
```

Requirements:
- Use vectors (not arrays)
- Dynamic adding (no size limit)
- Search by name functionality
- Menu-driven interface
- Range-based loops where appropriate

---

### Problem 6: Create a Word Frequency Counter

**Write a complete program** that:

1. Asks the user to enter words (one per line, enter "stop" to finish)
2. Counts frequency of each word (how many times it appears)
3. Stores in parallel vectors: `words[]` and `counts[]`
4. Displays results sorted by frequency (most to least)

**Example run:**
```
WORD FREQUENCY COUNTER
======================
Enter words (type "stop" to finish):

Enter word 1: apple
Enter word 2: banana
Enter word 3: apple
Enter word 4: cherry
Enter word 5: banana
Enter word 6: apple
Enter word 7: stop

=== FREQUENCY ANALYSIS ===
Word: apple, Frequency: 3
Word: banana, Frequency: 2
Word: cherry, Frequency: 1
```

Requirements:
- Use vectors for dynamic storage
- Count occurrences of each word
- Display sorted by frequency (high to low)
- Handle case sensitivity (apple and Apple are different)
- Show unique words only

---

### Problem 7: Create a Temperature Trend Analyzer

**Write a complete program** that:

1. Uses a vector to store daily temperatures
2. Asks how many days of data (1-30)
3. Reads temperatures for each day
4. Calculates and displays:
   - Average temperature
   - Highest temperature (day and value)
   - Lowest temperature (day and value)
   - Temperature trend (rising, falling, stable)
   - Days above average, below average, at average
   - Day-by-day change (+/- from previous day)

**Example run:**
```
TEMPERATURE TREND ANALYZER
==========================
How many days? 5

Day 1 temperature: 72
Day 2 temperature: 75
Day 3 temperature: 78
Day 4 temperature: 74
Day 5 temperature: 76

=== ANALYSIS ===
Average: 75.0°F

Highest: Day 3, 78.0°F
Lowest: Day 1, 72.0°F

Days above average: 3
Days below average: 1
Days at average: 1

Temperature Changes:
Day 1: 72.0°F
Day 2: 75.0°F (+3.0°)
Day 3: 78.0°F (+3.0°)
Day 4: 74.0°F (-4.0°)
Day 5: 76.0°F (+2.0°)
```

Requirements:
- Use vector for temperatures
- Calculate average, max, min
- Track day numbers for highs/lows
- Calculate day-by-day changes
- Count days relative to average

---

### Problem 8: Create a 2D Matrix Calculator

**Write a complete program** that:

1. Asks for matrix dimensions (rows and columns, max 5x5)
2. Creates a 2D array to store the matrix
3. Asks user to enter values for the matrix
4. Calculates and displays:
   - Sum of all elements
   - Average of all elements
   - Sum of each row
   - Sum of each column
   - Diagonal sum (top-left to bottom-right)
   - Transpose (swap rows and columns)

**Example run:**
```
2D MATRIX CALCULATOR
====================
How many rows? 3
How many columns? 3

Enter matrix values:
Row 1: 1 2 3
Row 2: 4 5 6
Row 3: 7 8 9

=== ORIGINAL MATRIX ===
1 2 3
4 5 6
7 8 9

=== CALCULATIONS ===
Sum of all elements: 45
Average: 5.0

Row Sums:
Row 1: 6
Row 2: 15
Row 3: 24

Column Sums:
Column 1: 12
Column 2: 15
Column 3: 18

Diagonal Sum (top-left to bottom-right): 15

=== TRANSPOSE ===
1 4 7
2 5 8
3 6 9
```

Requirements:
- Use 2D array
- Input validation (1-5 rows/columns)
- Row and column sum calculations
- Diagonal sum
- Display transpose matrix
- Professional formatting

---

## Challenge Problems (Optional)

### Challenge 1: Create an Inventory Management System

**Write a complete program** that:

1. Uses parallel vectors for:
   - Product names
   - Quantities
   - Unit prices
2. Menu options:
   - Add item
   - Remove item
   - Update quantity
   - Calculate total inventory value
   - Display inventory
   - Find item by name
   - Exit
3. Displays current inventory value at each step

Requirements:
- Dynamic vectors
- Complex menu with validation
- Inventory value calculation
- Search functionality
- Update operations

---

### Challenge 2: Create a Student Grade Distribution Analyzer

**Write a complete program** that:

1. Reads grades from user input (vector)
2. Creates a histogram showing grade distribution
3. Displays:
   - Grade distribution chart
   - Statistical analysis (mean, median, standard deviation)
   - Pass rate
   - Class performance summary

Requirements:
- Process grades into buckets (A, B, C, D, F)
- Display visual histogram
- Calculate statistical measures
- Interpret results

---