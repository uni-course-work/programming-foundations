# Chapter 19: Solutions

## Section A: Understanding File I/O

### Problem 1: File Reading and Writing Basics

**Solution:**

**1. Write and read five numbers:**
```cpp
#include <iostream>
#include <fstream>

int main() {
    // Write
    std::ofstream out("numbers.txt");
    out << 10 << std::endl;
    out << 20 << std::endl;
    out << 30 << std::endl;
    out << 40 << std::endl;
    out << 50 << std::endl;
    out.close();
    
    // Read
    std::ifstream in("numbers.txt");
    int value;
    while (in >> value) {
        std::cout << value << std::endl;
    }
    in.close();
    
    return 0;
}
```

**2. If file doesn't exist:**
- `ifstream` opens but `.is_open()` returns false
- Reading from closed stream produces garbage or undefined behavior
- **Always check** if file opened successfully

**3. Check file opened:**
```cpp
std::ifstream in("file.txt");
if (!in.is_open()) {
    std::cout << "Error: Could not open file!" << std::endl;
    return 1;
}
```

**4. Difference between approaches:**
- Approach A: Creates new file, overwrites if exists
- Approach B: Appends to existing file, creates if doesn't exist
- Use A for initial save, B for adding more data

**5. Loop ends when:**
- `inputFile >> value` returns false
- Happens at EOF or if read fails (not a valid integer)
- Loop automatically exits

**6. Read entire line:**
```cpp
#include <string>
std::string line;
while (std::getline(inputFile, line)) {
    std::cout << line << std::endl;
}
```
`getline()` reads entire line including spaces.

---

### Problem 2: File Processing Patterns

**Solution:**

**1. Load 100 student names into vector:**
```cpp
std::vector<std::string> names;
std::ifstream in("names.txt");
std::string name;
while (in >> name) {
    names.push_back(name);
}
in.close();
```

**2. Save vector of grades to file:**
```cpp
std::vector<double> grades = {85, 90, 78, 92};
std::ofstream out("grades.txt");
for (double g : grades) {
    out << g << std::endl;
}
out.close();
```

**3. Count lines in file:**
```cpp
std::ifstream in("data.txt");
std::string line;
int count = 0;
while (std::getline(in, line)) {
    count++;
}
in.close();
std::cout << "Lines: " << count << std::endl;
```

**4. Process line-by-line:**
```cpp
std::ifstream in("data.txt");
std::string line;
while (std::getline(in, line)) {
    std::cout << "Processing: " << line << std::endl;
}
in.close();
```

**5. Check if file exists:**
```cpp
std::ifstream check("file.txt");
if (check.is_open()) {
    std::cout << "File exists. Overwrite? (y/n): ";
    char response;
    std::cin >> response;
    if (response != 'y') return;
}
check.close();

// Now safe to overwrite
std::ofstream out("file.txt");
```

**6. Read and parse CSV:**
```cpp
std::ifstream in("data.csv");
std::string name, line;
int id, score;
char comma;

while (std::getline(in, line)) {
    std::stringstream ss(line);
    ss >> name >> comma >> id >> comma >> score;
    std::cout << name << ": " << score << std::endl;
}
in.close();
```

---

## Section B: Building Programs with Files

### Problem 3: Create a Persistent Student Grade System

**Solution:**

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <iomanip>
using std::string;
using std::vector;
using std::ifstream;
using std::ofstream;

struct Student {
    string name;
    int id;
    double grade;
};

vector<Student> students;

void loadStudents() {
    ifstream inputFile("students.txt");
    
    if (!inputFile.is_open()) {
        std::cout << "No previous students found." << std::endl;
        return;
    }
    
    string name;
    int id;
    double grade;
    int count = 0;
    
    while (inputFile >> name >> id >> grade) {
        students.push_back({name, id, grade});
        count++;
    }
    
    inputFile.close();
    std::cout << "Loaded " << count << " students." << std::endl;
}

void saveStudents() {
    ofstream outputFile("students.txt");
    
    if (!outputFile.is_open()) {
        std::cout << "Error: Could not open file!" << std::endl;
        return;
    }
    
    for (Student s : students) {
        outputFile << s.name << " " << s.id << " " << s.grade << std::endl;
    }
    
    outputFile.close();
    std::cout << "Students saved to file!" << std::endl;
}

void addStudent() {
    Student s;
    std::cout << "Enter student name: ";
    std::cin >> s.name;
    std::cout << "Enter ID: ";
    std::cin >> s.id;
    std::cout << "Enter grade: ";
    std::cin >> s.grade;
    
    students.push_back(s);
    std::cout << "Student added!" << std::endl;
}

void displayAllStudents() {
    if (students.empty()) {
        std::cout << "No students loaded." << std::endl;
        return;
    }
    
    std::cout << "\n=== ALL STUDENTS ===" << std::endl;
    for (int i = 0; i < students.size(); i++) {
        std::cout << (i + 1) << ". " << students[i].name 
                  << " (ID: " << students[i].id << ") - Grade: " 
                  << students[i].grade << std::endl;
    }
}

void findStudent() {
    std::cout << "Search by: 1. Name  2. ID : ";
    int choice;
    std::cin >> choice;
    
    if (choice == 1) {
        string name;
        std::cout << "Enter name: ";
        std::cin >> name;
        
        bool found = false;
        for (Student s : students) {
            if (s.name == name) {
                std::cout << s.name << " (ID: " << s.id 
                          << ") - Grade: " << s.grade << std::endl;
                found = true;
                break;
            }
        }
        if (!found) std::cout << "Not found." << std::endl;
    } else {
        int id;
        std::cout << "Enter ID: ";
        std::cin >> id;
        
        bool found = false;
        for (Student s : students) {
            if (s.id == id) {
                std::cout << s.name << " (ID: " << s.id 
                          << ") - Grade: " << s.grade << std::endl;
                found = true;
                break;
            }
        }
        if (!found) std::cout << "Not found." << std::endl;
    }
}

void calculateAverage() {
    if (students.empty()) {
        std::cout << "No students to average." << std::endl;
        return;
    }
    
    double sum = 0;
    for (Student s : students) {
        sum += s.grade;
    }
    
    double average = sum / students.size();
    std::cout << "Class Average: " << std::fixed << std::setprecision(2) 
              << average << std::endl;
}

int main() {
    std::cout << "=== STUDENT SYSTEM ===" << std::endl;
    std::cout << "Press Enter to load students from file...";
    std::cin.ignore();
    
    loadStudents();
    
    int choice;
    
    do {
        std::cout << "\n1. Add Student" << std::endl;
        std::cout << "2. Display All" << std::endl;
        std::cout << "3. Find Student" << std::endl;
        std::cout << "4. Calculate Average" << std::endl;
        std::cout << "5. Save to File" << std::endl;
        std::cout << "6. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            addStudent();
        } else if (choice == 2) {
            displayAllStudents();
        } else if (choice == 3) {
            findStudent();
        } else if (choice == 4) {
            calculateAverage();
        } else if (choice == 5) {
            saveStudents();
        } else if (choice == 6) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 6);
    
    return 0;
}
```

---

### Problem 4: Create a Personal Expense Tracker

**Solution:**

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <sstream>
#include <iomanip>
using std::string;
using std::vector;

struct Expense {
    string description;
    double amount;
    string category;
    string date;
};

vector<Expense> expenses;

void loadExpenses() {
    std::ifstream in("expenses.txt");
    
    if (!in.is_open()) {
        std::cout << "No previous expenses found." << std::endl;
        return;
    }
    
    string line;
    int count = 0;
    
    while (std::getline(in, line)) {
        if (line.empty()) continue;
        
        std::stringstream ss(line);
        string desc, cat, date;
        double amt;
        char pipe;
        
        ss >> desc >> pipe >> amt >> pipe >> cat >> pipe >> date;
        expenses.push_back({desc, amt, cat, date});
        count++;
    }
    
    in.close();
    std::cout << "Loaded " << count << " expenses." << std::endl;
}

void saveExpenses() {
    std::ofstream out("expenses.txt");
    
    for (Expense e : expenses) {
        out << e.description << "|" << e.amount << "|" 
            << e.category << "|" << e.date << std::endl;
    }
    
    out.close();
    std::cout << "Expenses saved!" << std::endl;
}

void addExpense() {
    Expense e;
    std::cout << "Description: ";
    std::cin >> e.description;
    std::cout << "Amount: ";
    std::cin >> e.amount;
    std::cout << "Category: ";
    std::cin >> e.category;
    std::cout << "Date (YYYY-MM-DD): ";
    std::cin >> e.date;
    
    expenses.push_back(e);
    std::cout << "Expense added!" << std::endl;
}

void displayAllExpenses() {
    if (expenses.empty()) {
        std::cout << "No expenses." << std::endl;
        return;
    }
    
    std::cout << "\n=== ALL EXPENSES ===" << std::endl;
    for (Expense e : expenses) {
        std::cout << e.description << " ($" << std::fixed 
                  << std::setprecision(2) << e.amount << ") - " 
                  << e.category << " [" << e.date << "]" << std::endl;
    }
}

void displayByCategory() {
    if (expenses.empty()) {
        std::cout << "No expenses." << std::endl;
        return;
    }
    
    string cat;
    std::cout << "Enter category: ";
    std::cin >> cat;
    
    std::cout << "\n=== " << cat << " ===" << std::endl;
    bool found = false;
    for (Expense e : expenses) {
        if (e.category == cat) {
            std::cout << e.description << " ($" << std::fixed 
                      << std::setprecision(2) << e.amount << ") [" 
                      << e.date << "]" << std::endl;
            found = true;
        }
    }
    if (!found) std::cout << "No expenses in this category." << std::endl;
}

void displaySummary() {
    if (expenses.empty()) {
        std::cout << "No expenses." << std::endl;
        return;
    }
    
    double total = 0;
    for (Expense e : expenses) {
        total += e.amount;
    }
    
    std::cout << "\n=== SUMMARY ===" << std::endl;
    std::cout << "Total Spent: $" << std::fixed << std::setprecision(2) 
              << total << std::endl;
    
    std::cout << "\nBy Category:" << std::endl;
    
    vector<string> categories;
    for (Expense e : expenses) {
        bool found = false;
        for (string c : categories) {
            if (c == e.category) {
                found = true;
                break;
            }
        }
        if (!found) categories.push_back(e.category);
    }
    
    for (string c : categories) {
        double catTotal = 0;
        for (Expense e : expenses) {
            if (e.category == c) {
                catTotal += e.amount;
            }
        }
        std::cout << c << ": $" << std::fixed << std::setprecision(2) 
                  << catTotal << std::endl;
    }
}

int main() {
    std::cout << "=== EXPENSE TRACKER ===" << std::endl;
    loadExpenses();
    
    int choice;
    
    do {
        std::cout << "\n1. Add Expense" << std::endl;
        std::cout << "2. Display All" << std::endl;
        std::cout << "3. Display by Category" << std::endl;
        std::cout << "4. Summary" << std::endl;
        std::cout << "5. Save to File" << std::endl;
        std::cout << "6. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            addExpense();
        } else if (choice == 2) {
            displayAllExpenses();
        } else if (choice == 3) {
            displayByCategory();
        } else if (choice == 4) {
            displaySummary();
        } else if (choice == 5) {
            saveExpenses();
        } else if (choice == 6) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 6);
    
    return 0;
}
```

---

### Problem 5: Create a Simple Note-Taking Application

**Solution:**

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <ctime>
#include <iomanip>
using std::string;
using std::vector;

struct Note {
    string title;
    string content;
    string dateCreated;
};

vector<Note> notes;

string getCurrentDate() {
    time_t now = time(0);
    struct tm* timeinfo = localtime(&now);
    char buffer[11];
    strftime(buffer, sizeof(buffer), "%Y-%m-%d", timeinfo);
    return string(buffer);
}

void loadNotes() {
    std::ifstream in("notes.txt");
    
    if (!in.is_open()) {
        std::cout << "No previous notes found." << std::endl;
        return;
    }
    
    string line;
    int count = 0;
    
    while (std::getline(in, line)) {
        if (line == "---") {
            Note n;
            std::getline(in, n.title);
            std::getline(in, n.dateCreated);
            
            string contentLine;
            n.content = "";
            while (std::getline(in, contentLine)) {
                if (contentLine == "---") break;
                if (!n.content.empty()) n.content += "\n";
                n.content += contentLine;
            }
            
            notes.push_back(n);
            count++;
        }
    }
    
    in.close();
    std::cout << "Loaded " << count << " notes." << std::endl;
}

void saveNotes() {
    std::ofstream out("notes.txt");
    
    for (Note n : notes) {
        out << "---" << std::endl;
        out << n.title << std::endl;
        out << n.dateCreated << std::endl;
        out << n.content << std::endl;
    }
    
    out.close();
    std::cout << "Notes saved!" << std::endl;
}

void createNote() {
    Note n;
    std::cout << "Note title: ";
    std::cin.ignore();
    std::getline(std::cin, n.title);
    
    std::cout << "Enter content (type END on new line when done):" << std::endl;
    n.content = "";
    string line;
    
    while (std::getline(std::cin, line)) {
        if (line == "END") break;
        if (!n.content.empty()) n.content += "\n";
        n.content += line;
    }
    
    n.dateCreated = getCurrentDate();
    notes.push_back(n);
    std::cout << "Note created!" << std::endl;
}

void displayAllNotes() {
    if (notes.empty()) {
        std::cout << "No notes." << std::endl;
        return;
    }
    
    std::cout << "\n=== ALL NOTES ===" << std::endl;
    for (int i = 0; i < notes.size(); i++) {
        std::cout << (i + 1) << ". " << notes[i].title 
                  << " (" << notes[i].dateCreated << ")" << std::endl;
    }
}

void displayNote() {
    int num;
    std::cout << "Enter note number: ";
    std::cin >> num;
    
    if (num < 1 || num > notes.size()) {
        std::cout << "Invalid note number." << std::endl;
        return;
    }
    
    Note n = notes[num - 1];
    std::cout << "\n=== " << n.title << " ===" << std::endl;
    std::cout << "(" << n.dateCreated << ")" << std::endl;
    std::cout << n.content << std::endl;
}

void searchNotes() {
    string keyword;
    std::cout << "Search keyword: ";
    std::cin.ignore();
    std::getline(std::cin, keyword);
    
    std::cout << "\n=== SEARCH RESULTS ===" << std::endl;
    int count = 0;
    
    for (int i = 0; i < notes.size(); i++) {
        if (notes[i].title.find(keyword) != string::npos ||
            notes[i].content.find(keyword) != string::npos) {
            std::cout << (i + 1) << ". " << notes[i].title 
                      << " (" << notes[i].dateCreated << ")" << std::endl;
            count++;
        }
    }
    
    if (count == 0) std::cout << "No notes found." << std::endl;
}

void deleteNote() {
    int num;
    std::cout << "Enter note number to delete: ";
    std::cin >> num;
    
    if (num < 1 || num > notes.size()) {
        std::cout << "Invalid note number." << std::endl;
        return;
    }
    
    notes.erase(notes.begin() + (num - 1));
    std::cout << "Note deleted!" << std::endl;
}

int main() {
    std::cout << "=== NOTE TAKER ===" << std::endl;
    loadNotes();
    
    int choice;
    
    do {
        std::cout << "\n1. Create Note" << std::endl;
        std::cout << "2. Display All" << std::endl;
        std::cout << "3. View Note" << std::endl;
        std::cout << "4. Search" << std::endl;
        std::cout << "5. Delete" << std::endl;
        std::cout << "6. Save" << std::endl;
        std::cout << "7. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            createNote();
        } else if (choice == 2) {
            displayAllNotes();
        } else if (choice == 3) {
            displayNote();
        } else if (choice == 4) {
            searchNotes();
        } else if (choice == 5) {
            deleteNote();
        } else if (choice == 6) {
            saveNotes();
        } else if (choice == 7) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 7);
    
    return 0;
}
```

---

### Problem 6: Create a Student Grade Analyzer with File Export

**Solution:**

```cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <iomanip>
using std::string;
using std::vector;

struct Student {
    string name;
    int id;
    vector<double> grades;
    double gpa;
};

vector<Student> students;

double calculateGPA(vector<double>& grades) {
    if (grades.empty()) return 0.0;
    
    double sum = 0;
    for (double g : grades) {
        sum += g;
    }
    return sum / grades.size() / 30.0;  // Convert to 4.0 scale
}

void loadStudents() {
    std::ifstream in("students.txt");
    
    if (!in.is_open()) {
        std::cout << "No students file found." << std::endl;
        return;
    }
    
    string name;
    int id;
    double grade;
    int count = 0;
    
    while (in >> name >> id) {
        Student s;
        s.name = name;
        s.id = id;
        
        while (in.peek() != '\n' && in >> grade) {
            s.grades.push_back(grade);
        }
        
        s.gpa = calculateGPA(s.grades);
        students.push_back(s);
        count++;
    }
    
    in.close();
    std::cout << "Loaded " << count << " students." << std::endl;
}

void displayAllStudents() {
    std::cout << "\n=== ALL STUDENTS ===" << std::endl;
    for (Student s : students) {
        std::cout << s.name << " (ID: " << s.id << ") GPA: " 
                  << std::fixed << std::setprecision(2) << s.gpa << std::endl;
    }
}

void displayStatistics() {
    if (students.empty()) {
        std::cout << "No students." << std::endl;
        return;
    }
    
    double total = 0;
    double highest = students[0].gpa;
    double lowest = students[0].gpa;
    int countA = 0, countB = 0, countC = 0, countD = 0, countF = 0;
    
    string highName = students[0].name;
    string lowName = students[0].name;
    
    for (Student s : students) {
        total += s.gpa;
        
        if (s.gpa > highest) {
            highest = s.gpa;
            highName = s.name;
        }
        if (s.gpa < lowest) {
            lowest = s.gpa;
            lowName = s.name;
        }
        
        if (s.gpa >= 3.7) countA++;
        else if (s.gpa >= 3.3) countB++;
        else if (s.gpa >= 2.7) countC++;
        else if (s.gpa >= 2.0) countD++;
        else countF++;
    }
    
    double average = total / students.size();
    
    std::cout << "\n=== CLASS STATISTICS ===" << std::endl;
    std::cout << "Total Students: " << students.size() << std::endl;
    std::cout << "Class Average: " << std::fixed << std::setprecision(2) 
              << average << std::endl;
    std::cout << "Highest: " << highName << " (" << highest << ")" << std::endl;
    std::cout << "Lowest: " << lowName << " (" << lowest << ")" << std::endl;
    std::cout << "\nGrade Distribution:" << std::endl;
    std::cout << "A (3.7-4.0): " << countA << " students" << std::endl;
    std::cout << "B (3.3-3.6): " << countB << " students" << std::endl;
    std::cout << "C (2.7-3.2): " << countC << " students" << std::endl;
    std::cout << "D (2.0-2.6): " << countD << " students" << std::endl;
    std::cout << "F (<2.0): " << countF << " students" << std::endl;
}

void generateReports() {
    // Class report
    std::ofstream classReport("class_report.txt");
    classReport << "=== CLASS REPORT ===" << std::endl;
    
    double total = 0;
    for (Student s : students) {
        total += s.gpa;
        classReport << s.name << " (ID: " << s.id << ") GPA: " 
                    << std::fixed << std::setprecision(2) << s.gpa << std::endl;
    }
    
    classReport << "\nClass Average: " << std::fixed << std::setprecision(2) 
                << (total / students.size()) << std::endl;
    classReport.close();
    
    // Honors list
    std::ofstream honors("honors_list.txt");
    honors << "=== HONORS LIST ===" << std::endl;
    
    for (Student s : students) {
        if (s.gpa >= 3.8) {
            honors << s.name << " (" << std::fixed << std::setprecision(2) 
                   << s.gpa << ")" << std::endl;
        }
    }
    honors.close();
    
    // At-risk list
    std::ofstream atRisk("at_risk.txt");
    atRisk << "=== AT-RISK STUDENTS ===" << std::endl;
    
    for (Student s : students) {
        if (s.gpa < 2.0) {
            atRisk << s.name << " (" << std::fixed << std::setprecision(2) 
                   << s.gpa << ")" << std::endl;
        }
    }
    atRisk.close();
    
    std::cout << "Reports generated!" << std::endl;
    std::cout << "- class_report.txt" << std::endl;
    std::cout << "- honors_list.txt" << std::endl;
    std::cout << "- at_risk.txt" << std::endl;
}

void viewHonorsList() {
    std::cout << "\n=== HONORS LIST ===" << std::endl;
    int count = 0;
    
    for (Student s : students) {
        if (s.gpa >= 3.8) {
            std::cout << s.name << " (" << std::fixed << std::setprecision(2) 
                      << s.gpa << ")" << std::endl;
            count++;
        }
    }
    
    if (count == 0) std::cout << "No honors students." << std::endl;
}

void viewAtRiskList() {
    std::cout << "\n=== AT-RISK STUDENTS ===" << std::endl;
    int count = 0;
    
    for (Student s : students) {
        if (s.gpa < 2.0) {
            std::cout << s.name << " (" << std::fixed << std::setprecision(2) 
                      << s.gpa << ")" << std::endl;
            count++;
        }
    }
    
    if (count == 0) std::cout << "No at-risk students." << std::endl;
}

int main() {
    std::cout << "=== GRADE ANALYZER ===" << std::endl;
    loadStudents();
    
    int choice;
    
    do {
        std::cout << "\n1. Display All" << std::endl;
        std::cout << "2. Class Statistics" << std::endl;
        std::cout << "3. Generate Reports" << std::endl;
        std::cout << "4. View Honors List" << std::endl;
        std::cout << "5. View At-Risk List" << std::endl;
        std::cout << "6. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            displayAllStudents();
        } else if (choice == 2) {
            displayStatistics();
        } else if (choice == 3) {
            generateReports();
        } else if (choice == 4) {
            viewHonorsList();
        } else if (choice == 5) {
            viewAtRiskList();
        } else if (choice == 6) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 6);
    
    return 0;
}
```

---
