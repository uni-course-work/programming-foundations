# Chapter 19: Problems

## Section A: Understanding File I/O

### Problem 1: File Reading and Writing Basics

Consider these scenarios:

1. Write a program that creates a file with five numbers and then reads them back
2. What happens if you try to read from a file that doesn't exist?
3. How do you check if a file opened successfully?
4. What's the difference between these two approaches:
   ```cpp
   // Approach A
   std::ofstream out("file.txt");
   out << "data";
   
   // Approach B
   std::ofstream out("file.txt", std::ios::app);
   out << "data";
   ```
5. When does the `while (inputFile >> value)` loop end?
6. How would you read an entire line (with spaces) from a file?

---

### Problem 2: File Processing Patterns

For each scenario, identify what pattern you would use and write the code:

1. Load 100 student names from a file into a vector
2. Save a vector of grades to a file (one per line)
3. Count how many lines are in a file
4. Read a file and process it line-by-line
5. Check if a file exists before overwriting it
6. Read CSV data (Name,ID,Score) and parse it

---

## Section B: Building Programs with Files

### Problem 3: Create a Persistent Student Grade System

**Write a complete program** that:

1. Defines a Student structure:
   - Name (string)
   - ID (int)
   - Grade (double)

2. Implements persistence functions:
   - `loadStudents()` - read students from "students.txt"
   - `saveStudents()` - write students to "students.txt"

3. Implements operations:
   - `addStudent()` - get input, add to vector
   - `displayAllStudents()` - show all loaded students
   - `findStudent()` - search by name or ID
   - `calculateAverage()` - average grade of all students
   - `saveToFile()` - save current roster

4. Menu system:
   - Load Students
   - Add Student
   - Display All
   - Find Student
   - Calculate Average
   - Save to File
   - Exit

5. On startup: automatically load from "students.txt" if it exists

**Example run:**
```
=== STUDENT SYSTEM ===
Press Enter to load students from file...
Loaded 0 students.

1. Add Student
2. Display All
3. Find Student
4. Calculate Average
5. Save to File
6. Exit
Choice: 1

Enter student name: Alice
Enter ID: 101
Enter grade: 85
Student added!

Choice: 1
Enter student name: Bob
Enter ID: 102
Enter grade: 92
Student added!

Choice: 2
=== ALL STUDENTS ===
1. Alice (ID: 101) - Grade: 85
2. Bob (ID: 102) - Grade: 92

Choice: 4
Class Average: 88.5

Choice: 5
Students saved to file!

Choice: 6
Goodbye!
```

**On next run:**
```
=== STUDENT SYSTEM ===
Press Enter to load students from file...
Loaded 2 students.

1. Add Student
2. Display All
[Previous students are back!]
```

Requirements:
- Load students from file at startup
- Save students to file
- Menu-driven interface
- File format: one student per line (name id grade)
- Handle file not found gracefully

---

### Problem 4: Create a Personal Expense Tracker

**Write a complete program** that:

1. Defines an Expense structure:
   - Description (string)
   - Amount (double)
   - Category (string)
   - Date (string)

2. Implements persistence:
   - `loadExpenses()` - load from "expenses.txt"
   - `saveExpenses()` - save to "expenses.txt"

3. Implements operations:
   - `addExpense()` - get input, add expense
   - `displayAllExpenses()` - show all expenses
   - `displayByCategory()` - show expenses for one category
   - `calculateTotal()` - sum of all expenses
   - `calculateByCategory()` - sum per category

4. Menu system:
   - Load Expenses
   - Add Expense
   - Display All
   - Display by Category
   - Summary (total, by category)
   - Save to File
   - Exit

5. Tracks expenses across multiple runs

**Example run:**
```
=== EXPENSE TRACKER ===
Loaded 0 expenses.

1. Add Expense
2. Display All
3. Display by Category
4. Summary
5. Save to File
6. Exit
Choice: 1

Description: Lunch
Amount: 12.50
Category: Food
Date: 2026-01-02
Expense added!

Choice: 1
Description: Gas
Amount: 45.00
Category: Transportation
Date: 2026-01-02
Expense added!

Choice: 4
=== SUMMARY ===
Total Spent: $57.50

By Category:
Food: $12.50
Transportation: $45.00

Choice: 5
Expenses saved!

Choice: 6
```

Requirements:
- Load/save to file
- Menu-driven
- Calculate totals
- Category filtering
- File format: description|amount|category|date

---

### Problem 5: Create a Simple Note-Taking Application

**Write a complete program** that:

1. Stores notes in a file ("notes.txt")
2. Each note has:
   - Title (string)
   - Content (string, can be multiple lines)
   - Date created (string)

3. Implements functions:
   - `loadNotes()` - load all notes from file
   - `saveNotes()` - save all notes to file
   - `createNote()` - get title and content, add to vector
   - `displayAllNotes()` - list all note titles and dates
   - `displayNote()` - show full content of one note
   - `deleteNote()` - remove a note
   - `searchNotes()` - find notes with keyword

4. Menu system:
   - Create Note
   - Display All Notes
   - View Note
   - Search Notes
   - Delete Note
   - Save to File
   - Exit

5. On startup: load existing notes

**Example run:**
```
=== NOTE TAKER ===
Loaded 0 notes.

1. Create Note
2. Display All
3. View Note
4. Search
5. Delete
6. Save
7. Exit
Choice: 1

Note title: My First Note
Enter content (type END on new line when done):
This is my first note in the system.
It can have multiple lines.
This is line 3.
END

Note created!

Choice: 1
Note title: Shopping List
Enter content (type END on new line when done):
- Milk
- Bread
- Eggs
END

Note created!

Choice: 2
=== ALL NOTES ===
1. My First Note (2026-01-02)
2. Shopping List (2026-01-02)

Choice: 3
Enter note number: 1

=== MY FIRST NOTE ===
(2026-01-02)
This is my first note in the system.
It can have multiple lines.
This is line 3.

Choice: 6
Notes saved!

Choice: 7
```

Requirements:
- Load/save notes
- Multi-line note content
- Search by keyword
- Delete notes
- File format: handle multi-line content (use separator like "---")

---

### Problem 6: Create a Student Grade Analyzer with File Export

**Write a complete program** that:

1. Uses Student structure (from Problem 3):
   - Name, ID, grade vector

2. Loads students from "students.txt" (name id grade grade grade...)

3. Implements analysis functions:
   - Calculate individual GPA
   - Find highest/lowest students
   - Create grade distribution (A, B, C, D, F counts)
   - Generate class statistics

4. Exports analysis to file:
   - "class_report.txt" - full analysis
   - "honors_list.txt" - students with GPA >= 3.8
   - "at_risk.txt" - students with GPA < 2.0

5. Menu system:
   - Load Students
   - Display All
   - Class Statistics (display on screen)
   - Generate Reports (save to files)
   - View Honors List
   - View At-Risk List
   - Exit

**Example run:**
```
=== GRADE ANALYZER ===
Loaded 5 students.

1. Display All
2. Class Statistics
3. Generate Reports
4. View Honors List
5. View At-Risk List
6. Exit
Choice: 2

=== CLASS STATISTICS ===
Total Students: 5
Class Average: 87.4
Highest: Alice (92.5)
Lowest: Charlie (71.0)

Grade Distribution:
A (90-100): 2 students
B (80-89): 2 students
C (70-79): 1 student
D (60-69): 0 students
F (<60): 0 students

Choice: 3
Reports generated!
- class_report.txt
- honors_list.txt
- at_risk.txt

Choice: 4
=== HONORS LIST ===
Alice (3.95)
Bob (3.87)

Choice: 6
```

Requirements:
- Load students with multiple grades
- Calculate statistics
- Export to multiple files
- File format preservation
- Professional report format

---