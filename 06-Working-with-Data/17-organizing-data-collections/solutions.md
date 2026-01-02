# Chapter 17: Solutions

## Section A: Understanding Collections and Indexing

### Problem 1: Array Basics and Indexing

**Solution:**

**Outputs:**
- Line A: `85` (scores[0] is the first element)
- Line B: `78` (scores[2] is the third element)
- Line C: `88` (scores[4] is the fifth element)
- Line D: `95` (after modification, scores[1] is now 95)

**Why index 0 for first element:**
- Computer scientists count from 0
- Array memory starts at position 0
- This becomes natural with practice
- All programming languages follow this convention

**What scores[5] would do:**
- Access invalid memory (array only has indices 0-4)
- Undefined behavior
- Might crash, corrupt data, or appear to work
- Very dangerous

**After modification:**
- Array contains: {85, 95, 78, 92, 88}
- Only scores[1] changed

**Modify to print all five scores:**
```cpp
for (int i = 0; i < 5; i++) {
    std::cout << scores[i] << std::endl;
}
```

**Relationship between size and indices:**
- Array of size 5 has valid indices: 0, 1, 2, 3, 4
- Valid indices range from 0 to (size - 1)
- Any index >= size is out of bounds

---

### Problem 2: Iteration Patterns with Arrays

**Solution:**

1. **Print all test scores** → **Pattern 1: Process All Elements**
```cpp
for (int i = 0; i < 5; i++) {
    std::cout << scores[i] << std::endl;
}
```

2. **Find the highest score** → **Pattern 4: Accumulate (Find Max)**
```cpp
int max = scores[0];
for (int i = 1; i < 5; i++) {
    if (scores[i] > max) max = scores[i];
}
```

3. **Add 5 points to every score** → **Pattern 3: Transform**
```cpp
for (int i = 0; i < 5; i++) {
    scores[i] += 5;
}
```

4. **Count how many scores are A's (90+)** → **Pattern 5: Count**
```cpp
int countA = 0;
for (int i = 0; i < 5; i++) {
    if (scores[i] >= 90) countA++;
}
```

5. **Create a curved array (add 5 points to each)** → **Pattern 6: Build Another Array**
```cpp
int curved[5];
for (int i = 0; i < 5; i++) {
    curved[i] = scores[i] + 5;
}
```

6. **Find the index of a specific score** → **Pattern 2: Find Specific Element**
```cpp
int target = 90;
for (int i = 0; i < 5; i++) {
    if (scores[i] == target) {
        std::cout << "Found at index " << i << std::endl;
        break;
    }
}
```

7. **Calculate the average of all scores** → **Pattern 4: Accumulate (Sum and Average)**
```cpp
int sum = 0;
for (int i = 0; i < 5; i++) {
    sum += scores[i];
}
double average = sum / 5.0;
```

---

## Section B: Building Programs with Arrays

### Problem 3: Create a Simple Grade Tracker

**Solution:**

```cpp
#include <iostream>

int main() {
    int scores[10];
    int count;
    
    std::cout << "GRADE TRACKER" << std::endl;
    std::cout << "==============" << std::endl;
    std::cout << std::endl;
    
    // Get count
    std::cout << "How many scores? ";
    std::cin >> count;
    
    while (count < 1 || count > 10) {
        std::cout << "Invalid. Enter 1-10: ";
        std::cin >> count;
    }
    
    std::cout << std::endl;
    
    // Get scores
    for (int i = 0; i < count; i++) {
        std::cout << "Enter score " << (i + 1) << ": ";
        std::cin >> scores[i];
        
        while (scores[i] < 0 || scores[i] > 100) {
            std::cout << "Invalid. Enter 0-100: ";
            std::cin >> scores[i];
        }
    }
    
    // Calculate statistics
    int sum = 0;
    int max = scores[0];
    int min = scores[0];
    int maxPos = 0;
    int minPos = 0;
    int countA = 0, countB = 0, countC = 0, countD = 0, countF = 0;
    
    for (int i = 0; i < count; i++) {
        sum += scores[i];
        
        if (scores[i] > max) {
            max = scores[i];
            maxPos = i;
        }
        
        if (scores[i] < min) {
            min = scores[i];
            minPos = i;
        }
        
        if (scores[i] >= 90) countA++;
        else if (scores[i] >= 80) countB++;
        else if (scores[i] >= 70) countC++;
        else if (scores[i] >= 60) countD++;
        else countF++;
    }
    
    double average = sum / (double)count;
    
    // Display results
    std::cout << std::endl;
    std::cout << "=== ANALYSIS ===" << std::endl;
    std::cout << "Sum: " << sum << std::endl;
    std::cout << "Average: " << average << std::endl;
    std::cout << "Highest: " << max << " (at position " << (maxPos + 1) << ")" << std::endl;
    std::cout << "Lowest: " << min << " (at position " << (minPos + 1) << ")" << std::endl;
    std::cout << std::endl;
    std::cout << "Grade Distribution:" << std::endl;
    std::cout << "A's (90-100): " << countA << std::endl;
    std::cout << "B's (80-89): " << countB << std::endl;
    std::cout << "C's (70-79): " << countC << std::endl;
    std::cout << "D's (60-69): " << countD << std::endl;
    std::cout << "F's (<60): " << countF << std::endl;
    
    return 0;
}
```

---

### Problem 4: Create a Student Grade Report System

**Solution:**

```cpp
#include <iostream>
#include <string>

int main() {
    std::string names[20];
    int grades[20];
    int count;
    
    std::cout << "STUDENT GRADE REPORT" << std::endl;
    std::cout << "====================" << std::endl;
    std::cout << std::endl;
    
    // Get count
    std::cout << "How many students? ";
    std::cin >> count;
    std::cin.ignore();  // Clear input buffer after number
    
    while (count < 1 || count > 20) {
        std::cout << "Invalid. Enter 1-20: ";
        std::cin >> count;
        std::cin.ignore();
    }
    
    std::cout << std::endl;
    
    // Get student data
    for (int i = 0; i < count; i++) {
        std::cout << "Student " << (i + 1) << " name: ";
        std::getline(std::cin, names[i]);
        
        std::cout << "Student " << (i + 1) << " grade: ";
        std::cin >> grades[i];
        
        while (grades[i] < 0 || grades[i] > 100) {
            std::cout << "Invalid. Enter 0-100: ";
            std::cin >> grades[i];
        }
        
        std::cin.ignore();  // Clear input buffer
        std::cout << std::endl;
    }
    
    // Calculate statistics
    int sum = 0;
    int max = grades[0];
    int min = grades[0];
    int maxIdx = 0;
    int minIdx = 0;
    int passing = 0;
    
    for (int i = 0; i < count; i++) {
        sum += grades[i];
        
        if (grades[i] > max) {
            max = grades[i];
            maxIdx = i;
        }
        
        if (grades[i] < min) {
            min = grades[i];
            minIdx = i;
        }
        
        if (grades[i] >= 60) passing++;
    }
    
    double average = sum / (double)count;
    
    // Display results
    std::cout << std::endl;
    std::cout << "=== CLASS REPORT ===" << std::endl;
    std::cout << "Class Average: " << average << std::endl;
    std::cout << std::endl;
    std::cout << "Highest Grade: " << names[maxIdx] << " (" << max << ")" << std::endl;
    std::cout << "Lowest Grade: " << names[minIdx] << " (" << min << ")" << std::endl;
    std::cout << std::endl;
    
    // Sort and display (bubble sort for simplicity)
    std::string tempName;
    int tempGrade;
    
    for (int i = 0; i < count - 1; i++) {
        for (int j = 0; j < count - i - 1; j++) {
            if (grades[j] < grades[j + 1]) {
                tempGrade = grades[j];
                grades[j] = grades[j + 1];
                grades[j + 1] = tempGrade;
                
                tempName = names[j];
                names[j] = names[j + 1];
                names[j + 1] = tempName;
            }
        }
    }
    
    std::cout << "Students by Performance:" << std::endl;
    for (int i = 0; i < count; i++) {
        std::cout << (i + 1) << ". " << names[i] << ": " << grades[i] << std::endl;
    }
    
    std::cout << std::endl;
    std::cout << "Passing: " << passing << " students" << std::endl;
    
    return 0;
}
```

---

## Section C: Vectors and Dynamic Collections

### Problem 5: Create a Dynamic Grade Manager with Vectors

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <vector>
using std::vector;
using std::string;

void addStudent(vector<string>& names, vector<int>& grades) {
    string name;
    int grade;
    
    std::cout << "Student name: ";
    std::cin.ignore();
    std::getline(std::cin, name);
    
    std::cout << "Student grade: ";
    std::cin >> grade;
    
    while (grade < 0 || grade > 100) {
        std::cout << "Invalid. Enter 0-100: ";
        std::cin >> grade;
    }
    
    names.push_back(name);
    grades.push_back(grade);
    std::cout << "Added successfully!" << std::endl;
}

void displayAll(vector<string>& names, vector<int>& grades) {
    if (names.empty()) {
        std::cout << "No students yet." << std::endl;
    } else {
        std::cout << std::endl;
        std::cout << "=== ALL STUDENTS ===" << std::endl;
        for (int i = 0; i < names.size(); i++) {
            std::cout << (i + 1) << ". " << names[i] << ": " << grades[i] << std::endl;
        }
    }
}

void findStudent(vector<string>& names, vector<int>& grades) {
    string searchName;
    
    std::cout << "Enter name to find: ";
    std::cin.ignore();
    std::getline(std::cin, searchName);
    
    bool found = false;
    for (int i = 0; i < names.size(); i++) {
        if (names[i] == searchName) {
            std::cout << searchName << ": " << grades[i] << std::endl;
            found = true;
            break;
        }
    }
    
    if (!found) {
        std::cout << "Student not found." << std::endl;
    }
}

void displayStatistics(vector<string>& names, vector<int>& grades) {
    if (grades.empty()) {
        std::cout << "No grades yet." << std::endl;
        return;
    }
    
    int sum = 0;
    int max = grades[0];
    int min = grades[0];
    int maxIdx = 0;
    int minIdx = 0;
    
    for (int grade : grades) {
        sum += grade;
        if (grade > max) {
            max = grade;
            maxIdx = &grade - &grades[0];
        }
        if (grade < min) {
            min = grade;
            minIdx = &grade - &grades[0];
        }
    }
    
    // Recalculate max/min indices properly
    for (int i = 0; i < grades.size(); i++) {
        if (grades[i] == max) maxIdx = i;
        if (grades[i] == min) minIdx = i;
    }
    
    double average = sum / (double)grades.size();
    
    std::cout << std::endl;
    std::cout << "=== STATISTICS ===" << std::endl;
    std::cout << "Count: " << grades.size() << std::endl;
    std::cout << "Average: " << average << std::endl;
    std::cout << "Highest: " << names[maxIdx] << " (" << max << ")" << std::endl;
    std::cout << "Lowest: " << names[minIdx] << " (" << min << ")" << std::endl;
}

int main() {
    vector<string> names;
    vector<int> grades;
    int choice;
    
    do {
        std::cout << std::endl;
        std::cout << "=== STUDENT GRADE MANAGER ===" << std::endl;
        std::cout << "1. Add Student" << std::endl;
        std::cout << "2. Display All Students" << std::endl;
        std::cout << "3. Find Student" << std::endl;
        std::cout << "4. Display Statistics" << std::endl;
        std::cout << "5. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            addStudent(names, grades);
        } else if (choice == 2) {
            displayAll(names, grades);
        } else if (choice == 3) {
            findStudent(names, grades);
        } else if (choice == 4) {
            displayStatistics(names, grades);
        } else if (choice == 5) {
            std::cout << "Goodbye!" << std::endl;
        } else {
            std::cout << "Invalid choice." << std::endl;
        }
        
    } while (choice != 5);
    
    return 0;
}
```

---

### Problem 6: Create a Word Frequency Counter

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <vector>
using std::vector;
using std::string;

int main() {
    vector<string> words;
    vector<int> counts;
    string word;
    
    std::cout << "WORD FREQUENCY COUNTER" << std::endl;
    std::cout << "=====================" << std::endl;
    std::cout << "Enter words (type \"stop\" to finish):" << std::endl;
    std::cout << std::endl;
    
    int count = 0;
    do {
        std::cout << "Enter word " << (count + 1) << ": ";
        std::cin >> word;
        
        if (word != "stop") {
            // Check if word already exists
            bool found = false;
            for (int i = 0; i < words.size(); i++) {
                if (words[i] == word) {
                    counts[i]++;
                    found = true;
                    break;
                }
            }
            
            // If word not found, add it
            if (!found) {
                words.push_back(word);
                counts.push_back(1);
            }
            
            count++;
        }
        
    } while (word != "stop");
    
    if (words.empty()) {
        std::cout << "No words entered." << std::endl;
        return 0;
    }
    
    // Sort by frequency (bubble sort, highest to lowest)
    for (int i = 0; i < words.size() - 1; i++) {
        for (int j = 0; j < words.size() - i - 1; j++) {
            if (counts[j] < counts[j + 1]) {
                int tempCount = counts[j];
                counts[j] = counts[j + 1];
                counts[j + 1] = tempCount;
                
                string tempWord = words[j];
                words[j] = words[j + 1];
                words[j + 1] = tempWord;
            }
        }
    }
    
    // Display results
    std::cout << std::endl;
    std::cout << "=== FREQUENCY ANALYSIS ===" << std::endl;
    for (int i = 0; i < words.size(); i++) {
        std::cout << "Word: " << words[i] << ", Frequency: " << counts[i] << std::endl;
    }
    
    return 0;
}
```

---

### Problem 7: Create a Temperature Trend Analyzer

**Solution:**

```cpp
#include <iostream>
#include <vector>
using std::vector;

int main() {
    vector<double> temps;
    int days;
    
    std::cout << "TEMPERATURE TREND ANALYZER" << std::endl;
    std::cout << "==========================" << std::endl;
    std::cout << std::endl;
    
    std::cout << "How many days? ";
    std::cin >> days;
    
    while (days < 1 || days > 30) {
        std::cout << "Invalid. Enter 1-30: ";
        std::cin >> days;
    }
    
    std::cout << std::endl;
    
    // Get temperatures
    for (int i = 0; i < days; i++) {
        double temp;
        std::cout << "Day " << (i + 1) << " temperature: ";
        std::cin >> temp;
        temps.push_back(temp);
    }
    
    // Calculate average
    double sum = 0;
    for (double temp : temps) {
        sum += temp;
    }
    double average = sum / days;
    
    // Find max and min
    double maxTemp = temps[0];
    double minTemp = temps[0];
    int maxDay = 0;
    int minDay = 0;
    
    for (int i = 1; i < days; i++) {
        if (temps[i] > maxTemp) {
            maxTemp = temps[i];
            maxDay = i;
        }
        if (temps[i] < minTemp) {
            minTemp = temps[i];
            minDay = i;
        }
    }
    
    // Count days relative to average
    int aboveAvg = 0;
    int belowAvg = 0;
    int atAvg = 0;
    
    for (double temp : temps) {
        if (temp > average) aboveAvg++;
        else if (temp < average) belowAvg++;
        else atAvg++;
    }
    
    // Display results
    std::cout << std::endl;
    std::cout << "=== ANALYSIS ===" << std::endl;
    std::cout << "Average: " << average << "°F" << std::endl;
    std::cout << std::endl;
    std::cout << "Highest: Day " << (maxDay + 1) << ", " << maxTemp << "°F" << std::endl;
    std::cout << "Lowest: Day " << (minDay + 1) << ", " << minTemp << "°F" << std::endl;
    std::cout << std::endl;
    std::cout << "Days above average: " << aboveAvg << std::endl;
    std::cout << "Days below average: " << belowAvg << std::endl;
    std::cout << "Days at average: " << atAvg << std::endl;
    std::cout << std::endl;
    
    // Display day-by-day changes
    std::cout << "Temperature Changes:" << std::endl;
    std::cout << "Day 1: " << temps[0] << "°F" << std::endl;
    
    for (int i = 1; i < days; i++) {
        double change = temps[i] - temps[i - 1];
        std::cout << "Day " << (i + 1) << ": " << temps[i] << "°F ";
        if (change > 0) {
            std::cout << "(+" << change << "°)";
        } else if (change < 0) {
            std::cout << "(" << change << "°)";
        } else {
            std::cout << "(no change)";
        }
        std::cout << std::endl;
    }
    
    return 0;
}
```

---

### Problem 8: Create a 2D Matrix Calculator

**Solution:**

```cpp
#include <iostream>

int main() {
    int rows, cols;
    int matrix[5][5];
    
    std::cout << "2D MATRIX CALCULATOR" << std::endl;
    std::cout << "====================" << std::endl;
    std::cout << std::endl;
    
    // Get dimensions
    std::cout << "How many rows? ";
    std::cin >> rows;
    
    std::cout << "How many columns? ";
    std::cin >> cols;
    
    while (rows < 1 || rows > 5 || cols < 1 || cols > 5) {
        std::cout << "Invalid. Enter 1-5 for each: ";
        std::cin >> rows >> cols;
    }
    
    std::cout << std::endl;
    std::cout << "Enter matrix values:" << std::endl;
    
    // Get matrix values
    for (int i = 0; i < rows; i++) {
        std::cout << "Row " << (i + 1) << ": ";
        for (int j = 0; j < cols; j++) {
            std::cin >> matrix[i][j];
        }
    }
    
    // Calculate statistics
    int sum = 0;
    int max = matrix[0][0];
    int min = matrix[0][0];
    
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            sum += matrix[i][j];
            if (matrix[i][j] > max) max = matrix[i][j];
            if (matrix[i][j] < min) min = matrix[i][j];
        }
    }
    
    double average = sum / (double)(rows * cols);
    
    // Calculate row sums
    int rowSum[5];
    for (int i = 0; i < rows; i++) {
        rowSum[i] = 0;
        for (int j = 0; j < cols; j++) {
            rowSum[i] += matrix[i][j];
        }
    }
    
    // Calculate column sums
    int colSum[5];
    for (int j = 0; j < cols; j++) {
        colSum[j] = 0;
        for (int i = 0; i < rows; i++) {
            colSum[j] += matrix[i][j];
        }
    }
    
    // Calculate diagonal sum
    int diagSum = 0;
    int minDim = (rows < cols) ? rows : cols;
    for (int i = 0; i < minDim; i++) {
        diagSum += matrix[i][i];
    }
    
    // Display original matrix
    std::cout << std::endl;
    std::cout << "=== ORIGINAL MATRIX ===" << std::endl;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            std::cout << matrix[i][j] << " ";
        }
        std::cout << std::endl;
    }
    
    // Display calculations
    std::cout << std::endl;
    std::cout << "=== CALCULATIONS ===" << std::endl;
    std::cout << "Sum of all elements: " << sum << std::endl;
    std::cout << "Average: " << average << std::endl;
    std::cout << std::endl;
    
    std::cout << "Row Sums:" << std::endl;
    for (int i = 0; i < rows; i++) {
        std::cout << "Row " << (i + 1) << ": " << rowSum[i] << std::endl;
    }
    
    std::cout << std::endl;
    std::cout << "Column Sums:" << std::endl;
    for (int j = 0; j < cols; j++) {
        std::cout << "Column " << (j + 1) << ": " << colSum[j] << std::endl;
    }
    
    std::cout << std::endl;
    std::cout << "Diagonal Sum (top-left to bottom-right): " << diagSum << std::endl;
    
    // Display transpose
    std::cout << std::endl;
    std::cout << "=== TRANSPOSE ===" << std::endl;
    for (int j = 0; j < cols; j++) {
        for (int i = 0; i < rows; i++) {
            std::cout << matrix[i][j] << " ";
        }
        std::cout << std::endl;
    }
    
    return 0;
}
```

---