# Chapter 19: Persistent Data and File Interaction

## Introduction: Programs That Disappear

Every program you've written so far has had a critical flaw: when it ends, all data disappears.

Run your student grade system, add 50 students, calculate statistics, close the program... everything is gone. Next time you run it, you start with zero students.

Run your banking system, process transactions, check balances, close the program... all accounts are gone. The next customer starts fresh.

This is **not real software**. Real software remembers data between runs.

A **file** is data stored on disk that persists after the program ends. When you start the program again, you can read that file and get your data back.

Consider the possibilities:
- **Student System:** Save all students to a file so data survives program restarts
- **Banking:** Save account data so transactions persist
- **Game:** Save player progress so you can resume later
- **Configuration:** Save user settings so they're remembered next time

This is **persistence**—data that outlives the program.

In this chapter, we'll explore:
- **Why persistence matters** (real software needs it)
- **Reading from files** (input from disk)
- **Writing to files** (output to disk)
- **Text-based data formats** (simple, human-readable)
- **File processing patterns** (common structures)
- **Error handling** (files might not exist, be unreadable, etc.)

By the end of this chapter, you'll understand how to make programs that remember data between runs.

---

## Part 1: Why Persistence Matters

### Programs Need Memory

Consider a student grade system without persistence:

**Session 1:**
```
Add 10 students
Calculate statistics
Exit program
```

**Session 2:**
```
Program starts with ZERO students
All previous work is lost!
```

**With persistence:**

**Session 1:**
```
Add 10 students
Calculate statistics
Save to file "students.txt"
Exit program
```

**Session 2:**
```
Program starts
Load from file "students.txt"
All 10 students are back!
```

### Real-World Examples

**Banking:**
- Every transaction is saved to a file
- Balance persists between sessions
- History of all transactions is maintained
- Multiple customers' accounts are all saved

**Gaming:**
- Player progress is saved to a file
- Level, inventory, achievements persist
- Multiple save files for multiple games
- Can resume exactly where you left off

**Business:**
- Employee records saved to files
- Inventory data persists
- Sales history archived
- Reports generated from stored data

**Social Media:**
- User profiles saved
- Posts saved
- Messages saved
- Comments and likes saved

**Without persistent data, these systems wouldn't work.** A bank that lost all account data every time the system restarted would be catastrophic.

### Types of Persistence

**In-memory (what you've done):**
- Data in variables and structures
- Lost when program ends
- Fast but temporary

**Persistent (what we're learning):**
- Data saved to files on disk
- Survives program restart
- Slower but permanent
- The foundation of all real software

---

## Part 2: Files and File Streams

### What is a File?

A **file** is a sequence of data stored on disk. It can contain:
- Text (human-readable)
- Binary data (machine-readable)
- Mixed content

For now, we'll focus on **text files**—files containing readable text with one piece of data per line or in a structured format.

### Text Files vs. Binary Files

**Text Files:**
- Human readable
- Easy to create/edit manually
- Larger file size
- Slower to read/write
- **Best for:** Configuration, data export, learning

**Binary Files:**
- Machine readable only
- Compact representation
- Faster to read/write
- Harder to edit manually
- **Best for:** Large datasets, databases, executables

For this chapter, we focus on **text files** because they're easier to understand and debug.

### File Streams

A **stream** is a connection to a file. C++ provides two main file streams:

**Input Stream (ifstream):** Read from a file
```cpp
#include <fstream>

std::ifstream inputFile("filename.txt");
```

**Output Stream (ofstream):** Write to a file
```cpp
std::ofstream outputFile("filename.txt");
```

**Both directions (fstream):** Read and write (advanced)
```cpp
std::fstream file("filename.txt");
```

---

## Part 3: Writing to Files (Output)

### Basic File Writing

```cpp
#include <iostream>
#include <fstream>
using std::ofstream;

int main() {
    ofstream outputFile("grades.txt");
    
    outputFile << "85" << std::endl;
    outputFile << "90" << std::endl;
    outputFile << "78" << std::endl;
    
    outputFile.close();
    
    return 0;
}
```

This creates a file `grades.txt` containing:
```
85
90
78
```

**Key points:**
- `ofstream` creates a stream for writing
- `<<` operator writes to the stream (same as `std::cout`)
- `.close()` closes the file (important!)
- File is created in current directory

### Checking if File Opened

Files can fail to open (permission denied, disk full, invalid path):

```cpp
#include <iostream>
#include <fstream>
using std::ofstream;

int main() {
    ofstream outputFile("grades.txt");
    
    if (!outputFile.is_open()) {
        std::cout << "Error: Could not open file!" << std::endl;
        return 1;
    }
    
    outputFile << "85" << std::endl;
    outputFile << "90" << std::endl;
    
    outputFile.close();
    return 0;
}
```

**Always check** if the file opened successfully.

### Writing Different Data Types

```cpp
ofstream outputFile("data.txt");

outputFile << "Alice" << std::endl;      // String
outputFile << 85 << std::endl;           // Integer
outputFile << 3.75 << std::endl;         // Double
outputFile << "Engineering" << std::endl; // String
```

File contains:
```
Alice
85
3.75
Engineering
```

### Writing Formatted Data

```cpp
ofstream outputFile("scores.txt");

outputFile << "Name,Score,Grade" << std::endl;
outputFile << "Alice,85,B" << std::endl;
outputFile << "Bob,92,A" << std::endl;
outputFile << "Charlie,78,C" << std::endl;

outputFile.close();
```

File contains (CSV format):
```
Name,Score,Grade
Alice,85,B
Bob,92,A
Charlie,78,C
```

---

## Part 4: Reading from Files (Input)

### Basic File Reading

```cpp
#include <iostream>
#include <fstream>
using std::ifstream;

int main() {
    ifstream inputFile("grades.txt");
    
    if (!inputFile.is_open()) {
        std::cout << "Error: Could not open file!" << std::endl;
        return 1;
    }
    
    int grade;
    while (inputFile >> grade) {
        std::cout << "Grade: " << grade << std::endl;
    }
    
    inputFile.close();
    return 0;
}
```

**Key points:**
- `ifstream` creates a stream for reading
- `>>` operator reads from the stream (same as `std::cin`)
- `while (inputFile >> grade)` reads until end of file
- Loop automatically exits at EOF (end of file)

### Reading Different Data Types

```cpp
ifstream inputFile("students.txt");

std::string name;
int id;
double gpa;

while (inputFile >> name >> id >> gpa) {
    std::cout << name << ": " << id << " (" << gpa << ")" << std::endl;
}

inputFile.close();
```

If file contains:
```
Alice 101 3.85
Bob 102 3.72
Charlie 103 3.91
```

Output:
```
Alice: 101 (3.85)
Bob: 102 (3.72)
Charlie: 103 (3.91)
```

### Reading Line by Line

The `>>` operator stops at whitespace. To read entire lines, use `getline()`:

```cpp
#include <string>
using std::string;
using std::getline;

ifstream inputFile("names.txt");

string line;
while (getline(inputFile, line)) {
    std::cout << "Line: " << line << std::endl;
}

inputFile.close();
```

If file contains:
```
Alice Smith
Bob Johnson
Charlie Brown
```

Output:
```
Line: Alice Smith
Line: Bob Johnson
Line: Charlie Brown
```

### Reading with Delimiter

For CSV (comma-separated values), read fields separated by commas:

```cpp
#include <sstream>
using std::stringstream;

ifstream inputFile("students.csv");

string line;
while (getline(inputFile, line)) {
    stringstream ss(line);
    string name;
    int id;
    double gpa;
    char comma;
    
    ss >> name >> comma >> id >> comma >> gpa;
    
    std::cout << name << ": " << id << " (" << gpa << ")" << std::endl;
}

inputFile.close();
```

---

## Part 5: End of File Detection

### Loop Until EOF

When you read past the end of file, the stream fails:

```cpp
ifstream inputFile("data.txt");

int value;
while (inputFile >> value) {  // Fails when EOF reached
    std::cout << value << std::endl;
}

inputFile.close();
```

The condition `inputFile >> value` is false when:
- Reading fails (not a valid integer)
- Reached end of file

### Checking EOF Explicitly

```cpp
ifstream inputFile("data.txt");

int value;
while (!inputFile.eof()) {
    inputFile >> value;
    if (!inputFile.eof()) {
        std::cout << value << std::endl;
    }
}

inputFile.close();
```

**Better approach:** Just use the implicit check in the while condition.

### Line Counting

```cpp
ifstream inputFile("data.txt");

string line;
int lineCount = 0;

while (getline(inputFile, line)) {
    lineCount++;
    std::cout << lineCount << ": " << line << std::endl;
}

inputFile.close();
```

---

## Part 6: Common File Patterns

### Pattern 1: Write a Vector to File

```cpp
#include <vector>
#include <fstream>

std::vector<int> scores = {85, 90, 78, 92, 88};

std::ofstream outputFile("scores.txt");

for (int score : scores) {
    outputFile << score << std::endl;
}

outputFile.close();
```

Creates `scores.txt`:
```
85
90
78
92
88
```

### Pattern 2: Read File into Vector

```cpp
#include <vector>
#include <fstream>

std::vector<int> scores;
std::ifstream inputFile("scores.txt");

int score;
while (inputFile >> score) {
    scores.push_back(score);
}

inputFile.close();

// Now scores contains: 85, 90, 78, 92, 88
```

### Pattern 3: Save Structures to File

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
};

std::vector<Student> roster = {
    {"Alice", 101, 3.85},
    {"Bob", 102, 3.72}
};

std::ofstream outputFile("students.txt");

for (Student s : roster) {
    outputFile << s.name << " " << s.id << " " << s.gpa << std::endl;
}

outputFile.close();
```

Creates `students.txt`:
```
Alice 101 3.85
Bob 102 3.72
```

### Pattern 4: Load Structures from File

```cpp
struct Student {
    std::string name;
    int id;
    double gpa;
};

std::vector<Student> roster;
std::ifstream inputFile("students.txt");

std::string name;
int id;
double gpa;

while (inputFile >> name >> id >> gpa) {
    roster.push_back({name, id, gpa});
}

inputFile.close();

// Now roster contains all students from file
```

### Pattern 5: Append to Existing File

```cpp
#include <fstream>

std::ofstream outputFile("log.txt", std::ios::app);  // app = append mode

outputFile << "Log entry at 10:30 AM" << std::endl;

outputFile.close();
```

File modes:
- Default (or `std::ios::out`): Overwrite if file exists
- `std::ios::app`: Append to existing file

### Pattern 6: Count Lines in File

```cpp
#include <fstream>

std::ifstream inputFile("data.txt");

std::string line;
int count = 0;

while (getline(inputFile, line)) {
    count++;
}

inputFile.close();

std::cout << "File has " << count << " lines" << std::endl;
```

---

## Part 7: Text Data Formats

### Format 1: Simple One Per Line

**Format:**
```
value1
value2
value3
```

**Code:**
```cpp
// Write
std::ofstream out("file.txt");
out << 85 << std::endl;
out << 90 << std::endl;

// Read
std::ifstream in("file.txt");
int value;
while (in >> value) {
    std::cout << value << std::endl;
}
```

### Format 2: Space-Separated (Fixed Width)

**Format:**
```
Alice 101 3.85
Bob 102 3.72
```

**Code:**
```cpp
// Write
std::ofstream out("file.txt");
out << "Alice " << 101 << " " << 3.85 << std::endl;

// Read
std::ifstream in("file.txt");
std::string name;
int id;
double gpa;
while (in >> name >> id >> gpa) {
    std::cout << name << ": " << gpa << std::endl;
}
```

### Format 3: CSV (Comma-Separated Values)

**Format:**
```
Name,ID,GPA
Alice,101,3.85
Bob,102,3.72
```

**Code:**
```cpp
// Write
std::ofstream out("file.csv");
out << "Name,ID,GPA" << std::endl;
out << "Alice," << 101 << "," << 3.85 << std::endl;

// Read (simple, ignoring commas)
std::ifstream in("file.csv");
std::string line;
getline(in, line);  // Skip header
while (getline(in, line)) {
    std::cout << line << std::endl;
}
```

### Format 4: Key-Value Pairs

**Format:**
```
name Alice
id 101
gpa 3.85
```

**Code:**
```cpp
// Write
std::ofstream out("file.txt");
out << "name Alice" << std::endl;
out << "id 101" << std::endl;
out << "gpa 3.85" << std::endl;

// Read
std::ifstream in("file.txt");
std::string key, value;
while (in >> key >> value) {
    std::cout << key << " = " << value << std::endl;
}
```

---

## Part 8: Error Handling and Edge Cases

### File Not Found

```cpp
#include <fstream>

std::ifstream inputFile("nonexistent.txt");

if (!inputFile.is_open()) {
    std::cout << "Error: File not found!" << std::endl;
    return 1;
}

// Process file...
inputFile.close();
```

### Empty File

```cpp
std::ifstream inputFile("data.txt");

int count = 0;
int value;

while (inputFile >> value) {
    count++;
}

if (count == 0) {
    std::cout << "File is empty!" << std::endl;
}

inputFile.close();
```

### File Permission Denied

```cpp
std::ofstream outputFile("data.txt");

if (!outputFile.is_open()) {
    std::cout << "Error: Cannot write to file (permission denied?)" << std::endl;
    return 1;
}

outputFile << "data" << std::endl;
outputFile.close();
```

### Disk Full

```cpp
std::ofstream outputFile("data.txt");

if (!outputFile) {
    std::cout << "Error: Cannot open file!" << std::endl;
    return 1;
}

outputFile << "large data" << std::endl;

if (outputFile.fail()) {
    std::cout << "Error: Write failed (disk full?)" << std::endl;
    return 1;
}

outputFile.close();
```

### Invalid Format in File

```cpp
std::ifstream inputFile("data.txt");

int value;
while (inputFile >> value) {
    if (inputFile.fail()) {
        std::cout << "Error: Invalid number in file!" << std::endl;
        inputFile.clear();  // Clear error state
        std::string dummy;
        inputFile >> dummy;  // Skip invalid entry
    }
}

inputFile.close();
```

---

## Part 9: File Management Best Practices

### Always Close Files

```cpp
// WRONG: File not closed
std::ofstream outputFile("data.txt");
outputFile << "data" << std::endl;
return 0;  // File not closed!

// CORRECT: File closed
std::ofstream outputFile("data.txt");
outputFile << "data" << std::endl;
outputFile.close();
return 0;
```

### Use Scope to Auto-Close

```cpp
{
    std::ofstream outputFile("data.txt");
    outputFile << "data" << std::endl;
}  // File automatically closed here
```

### Check Before Overwriting

```cpp
#include <fstream>

// Check if file exists before overwriting
std::ifstream checkFile("data.txt");
if (checkFile.is_open()) {
    std::cout << "Warning: File already exists. Overwrite? (y/n): ";
    char response;
    std::cin >> response;
    if (response != 'y') {
        return 0;
    }
}
checkFile.close();

// Now safe to write
std::ofstream outputFile("data.txt");
outputFile << "new data" << std::endl;
outputFile.close();
```

### Use Meaningful Filenames

```cpp
// WRONG: Generic names
std::ofstream out1("data.txt");
std::ofstream out2("info.txt");

// CORRECT: Descriptive names
std::ofstream studentFile("students.txt");
std::ofstream gradeFile("grades.txt");
std::ofstream logFile("system_log.txt");
```

---

## Part 10: Complete Example: Persistent Student System

### Without Persistence (Old Way)

```cpp
#include <iostream>
#include <vector>
#include <string>
using std::vector;
using std::string;

struct Student {
    string name;
    int id;
    double gpa;
};

int main() {
    vector<Student> roster;
    
    // Add students (lost on exit!)
    roster.push_back({"Alice", 101, 3.85});
    roster.push_back({"Bob", 102, 3.72});
    
    // Display students
    for (Student s : roster) {
        std::cout << s.name << ": " << s.gpa << std::endl;
    }
    
    // All data lost when program exits!
    return 0;
}
```

### With Persistence (New Way)

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
using std::vector;
using std::string;
using std::ifstream;
using std::ofstream;

struct Student {
    string name;
    int id;
    double gpa;
};

void loadStudents(vector<Student>& roster, string filename) {
    ifstream inputFile(filename);
    
    string name;
    int id;
    double gpa;
    
    while (inputFile >> name >> id >> gpa) {
        roster.push_back({name, id, gpa});
    }
    
    inputFile.close();
}

void saveStudents(vector<Student>& roster, string filename) {
    ofstream outputFile(filename);
    
    for (Student s : roster) {
        outputFile << s.name << " " << s.id << " " << s.gpa << std::endl;
    }
    
    outputFile.close();
}

void displayRoster(vector<Student>& roster) {
    for (Student s : roster) {
        std::cout << s.name << ": " << s.gpa << std::endl;
    }
}

int main() {
    vector<Student> roster;
    
    // Load previous students from file
    loadStudents(roster, "students.txt");
    
    // Add new student
    std::string name;
    int id;
    double gpa;
    
    std::cout << "New student name: ";
    std::cin >> name;
    std::cout << "ID: ";
    std::cin >> id;
    std::cout << "GPA: ";
    std::cin >> gpa;
    
    roster.push_back({name, id, gpa});
    
    // Display all
    std::cout << "\nRoster:" << std::endl;
    displayRoster(roster);
    
    // Save to file (data survives program exit!)
    saveStudents(roster, "students.txt");
    
    std::cout << "\nData saved!" << std::endl;
    return 0;
}
```

**Key difference:** Data is saved to `students.txt`. Next run of the program, `loadStudents()` reads the file and gets all previous students back.

---

## Part 11: File Formats and Data Organization

### Flat File (Simple)

**students.txt:**
```
Alice 101 3.85
Bob 102 3.72
Charlie 103 3.91
```

**Advantages:** Simple, human-readable
**Disadvantages:** No structure, hard to parse complex data

### CSV Format

**students.csv:**
```
Name,ID,GPA
Alice,101,3.85
Bob,102,3.72
Charlie,103,3.91
```

**Advantages:** Standard format, compatible with Excel
**Disadvantages:** Requires parsing delimiters

### Structured Format (One Record Per Block)

**students.txt:**
```
Student
Name: Alice
ID: 101
GPA: 3.85
---
Student
Name: Bob
ID: 102
GPA: 3.72
---
```

**Advantages:** Clear structure, easy to find records
**Disadvantages:** More complex parsing

### JSON-like (Future Learning)

```json
[
  {"name": "Alice", "id": 101, "gpa": 3.85},
  {"name": "Bob", "id": 102, "gpa": 3.72}
]
```

**Advantages:** Standard format, hierarchical structure
**Disadvantages:** Complex to parse manually (libraries exist)

For now, stick with simple formats (one value per line or space-separated).

---

## Summary: Programs That Remember

With file I/O, programs become **persistent**:
- Data survives program shutdown
- Users can resume where they left off
- History is maintained
- Multiple runs build on each other

**Real software always saves data.** Without persistence:
- Banks couldn't exist
- Games couldn't save progress
- Documents couldn't be preserved
- User settings would reset every session

File I/O is fundamental to making programs useful beyond a single execution.

---

## Looking Ahead

Now that you understand file I/O, you can:
- Save data from your programs
- Load data between sessions
- Create backup copies
- Export data for analysis
- Implement undo/redo (save states)

Files are how data persists. Databases (future learning) build on this concept with more sophisticated storage.

---

## Checklist: Before Using File I/O

- [ ] Do I need data to persist between runs?
- [ ] Have I chosen an appropriate file format?
- [ ] Do I check if files opened successfully?
- [ ] Do I close files when done?
- [ ] Have I handled empty files?
- [ ] Have I handled file not found?
- [ ] Do I validate data when reading?
- [ ] Is my filename descriptive?
- [ ] Does my format make sense for the data?
- [ ] Can I manually verify the file format?

If you answer "yes" to most of these, you're ready to use files effectively.

---

## Key Terminology

| Term | Definition |
|------|-----------|
| **File** | Data persisted on disk |
| **Stream** | Connection to a file for reading/writing |
| **ifstream** | Input file stream (read from file) |
| **ofstream** | Output file stream (write to file) |
| **EOF** | End of file marker |
| **Text file** | Human-readable file with text data |
| **Binary file** | Machine-readable file with binary data |
| **Delimiter** | Character separating data (comma, space, etc.) |
| **Persistence** | Data surviving program shutdown |
| **Append mode** | Add to existing file instead of overwriting |
| **Error handling** | Detecting and handling file operation failures |
