# Chapter 18: Solutions

## Section A: Understanding Structures

### Problem 1: Structure Basics and Initialization

**Solution:**

**1. Creating an instance:**
```cpp
Book myBook;  // Uninitialized (garbage values)
Book myBook = {};  // Default initialized (empty string, 0, 0.0)
Book myBook = {"Title", "Author", 2023, 19.99};  // With values
```

**2. Three initialization methods:**

Method 1: Create then assign
```cpp
Book b;
b.title = "The Hobbit";
b.author = "J.R.R. Tolkien";
b.year = 1937;
b.price = 15.99;
```

Method 2: Initialize with values
```cpp
Book b = {"The Hobbit", "J.R.R. Tolkien", 1937, 15.99};
```

Method 3: Named initialization
```cpp
Book b = {.title = "The Hobbit", .author = "J.R.R. Tolkien", .year = 1937, .price = 15.99};
```

**3. Accessing members:**
```cpp
std::cout << myBook.title << std::endl;  // Prints the title
std::string t = myBook.title;  // Store in variable
```

**4. Uninitialized book:**
- Contains garbage values
- `title` is empty string
- `author` is empty string
- `year` is undefined (0 or random)
- `price` is undefined (0.0 or random)
- **Don't do this** - always initialize

**5. Modifying a member:**
```cpp
Book b = {"The Hobbit", "J.R.R. Tolkien", 1937, 15.99};
b.price = 12.99;  // Change price
b.author = "Tolkien";  // Change author
```

**6. Why structure is better:**
- Five variables scattered vs. one organized object
- Easier to pass to functions (one parameter vs. five)
- Easier to track (all book info together)
- Can create collections (vector<Book>)
- Clear relationships (all belong to same book)
- No naming confusion (which title goes with which author?)

---

### Problem 2: Structures and Functions

**Solution:**

**1. Display a book:**
```cpp
void displayBook(Book b) {
    std::cout << b.title << " by " << b.author << " (" << b.year << ") - $" << b.price << std::endl;
}
```
**Pass-by-value:** Displaying doesn't modify, no need for reference.

**2. Change price:**
```cpp
void applyDiscount(Book &b, double discountPercent) {
    b.price *= (1 - discountPercent / 100.0);
}
```
**Pass-by-reference:** Modifying the actual book.

**3. Return discounted price:**
```cpp
double getDiscountedPrice(Book b, double discountPercent) {
    return b.price * (1 - discountPercent / 100.0);
}
```
**Pass-by-value:** Calculating doesn't modify original.

**4. Find most expensive:**
```cpp
Book getMostExpensive(std::vector<Book>& books) {
    Book most = books[0];
    for (Book b : books) {
        if (b.price > most.price) {
            most = b;
        }
    }
    return most;
}
```
**Pass-by-reference:** Efficient (no copy of vector), returns copy of book.

**5. Count books from year:**
```cpp
int countByYear(std::vector<Book>& books, int year) {
    int count = 0;
    for (Book b : books) {
        if (b.year == year) count++;
    }
    return count;
}
```
**Pass-by-reference:** Efficient, no modification needed.

**6. Create and return book:**
```cpp
Book createBook(std::string title, std::string author, int year, double price) {
    Book b = {title, author, year, price};
    return b;
}
```
**No reference:** Creating new object, returns it.

**When to use each:**
- **By-value:** Read-only operations, small structures, simple functions
- **By-reference:** Modifications, large structures, efficiency, vector operations

---

## Section B: Building Programs with Structures

### Problem 3: Create a Book Catalog System

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <vector>
using std::string;
using std::vector;

struct Book {
    string title;
    string author;
    int year;
    double price;
    string isbn;
};

void displayBook(Book b) {
    std::cout << "\"" << b.title << "\" by " << b.author 
              << " (" << b.year << ") - $" << b.price << std::endl;
}

void displayAllBooks(Book books[], int count) {
    std::cout << "\n=== ALL BOOKS ===" << std::endl;
    for (int i = 0; i < count; i++) {
        std::cout << (i + 1) << ". ";
        displayBook(books[i]);
    }
}

void findByAuthor(Book books[], int count, string author) {
    std::cout << "\n=== BOOKS BY " << author << " ===" << std::endl;
    bool found = false;
    for (int i = 0; i < count; i++) {
        if (books[i].author == author) {
            displayBook(books[i]);
            found = true;
        }
    }
    if (!found) std::cout << "No books found." << std::endl;
}

void findByYear(Book books[], int count, int year) {
    std::cout << "\n=== BOOKS FROM " << year << " ===" << std::endl;
    bool found = false;
    for (int i = 0; i < count; i++) {
        if (books[i].year == year) {
            displayBook(books[i]);
            found = true;
        }
    }
    if (!found) std::cout << "No books found." << std::endl;
}

double getTotalValue(Book books[], int count) {
    double total = 0;
    for (int i = 0; i < count; i++) {
        total += books[i].price;
    }
    return total;
}

double getAveragePrice(Book books[], int count) {
    if (count == 0) return 0;
    return getTotalValue(books, count) / count;
}

Book getMostExpensive(Book books[], int count) {
    Book most = books[0];
    for (int i = 1; i < count; i++) {
        if (books[i].price > most.price) {
            most = books[i];
        }
    }
    return most;
}

int main() {
    Book catalog[5] = {
        {"The Hobbit", "J.R.R. Tolkien", 1937, 15.99, "ISBN001"},
        {"1984", "George Orwell", 1949, 13.99, "ISBN002"},
        {"Brave New World", "Aldous Huxley", 1932, 14.99, "ISBN003"},
        {"The Great Gatsby", "F. Scott Fitzgerald", 1925, 12.99, "ISBN004"},
        {"To Kill a Mockingbird", "Harper Lee", 1960, 16.99, "ISBN005"}
    };
    
    std::cout << "=== BOOK CATALOG ===" << std::endl;
    
    displayAllBooks(catalog, 5);
    
    findByAuthor(catalog, 5, "J.R.R. Tolkien");
    
    findByYear(catalog, 5, 1949);
    
    std::cout << "\nTotal Value: $" << getTotalValue(catalog, 5) << std::endl;
    std::cout << "Average Price: $" << getAveragePrice(catalog, 5) << std::endl;
    
    Book expensive = getMostExpensive(catalog, 5);
    std::cout << "Most Expensive: \"" << expensive.title << "\" by " 
              << expensive.author << " ($" << expensive.price << ")" << std::endl;
    
    return 0;
}
```

---

### Problem 4: Create a Student Grade System with Structures

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <vector>
using std::string;
using std::vector;

struct Student {
    string name;
    int id;
    vector<int> grades;
    double gpa;
};

void addStudent(vector<Student>& students) {
    Student s;
    std::cout << "Student name: ";
    std::cin.ignore();
    std::getline(std::cin, s.name);
    
    std::cout << "Student ID: ";
    std::cin >> s.id;
    
    s.gpa = 0.0;
    students.push_back(s);
    std::cout << "Student added!" << std::endl;
}

void addGrade(vector<Student>& students, int id, int grade) {
    for (int i = 0; i < students.size(); i++) {
        if (students[i].id == id) {
            students[i].grades.push_back(grade);
            std::cout << "Grade added!" << std::endl;
            return;
        }
    }
    std::cout << "Student not found." << std::endl;
}

void calculateGPA(Student &s) {
    if (s.grades.empty()) {
        s.gpa = 0.0;
        return;
    }
    
    int sum = 0;
    for (int grade : s.grades) {
        sum += grade;
    }
    s.gpa = sum / (double)s.grades.size() / 30.0;  // Convert to 4.0 scale
}

Student* findStudentByID(vector<Student>& students, int id) {
    for (int i = 0; i < students.size(); i++) {
        if (students[i].id == id) {
            return &students[i];
        }
    }
    return nullptr;
}

Student* findStudentByName(vector<Student>& students, string name) {
    for (int i = 0; i < students.size(); i++) {
        if (students[i].name == name) {
            return &students[i];
        }
    }
    return nullptr;
}

void displayStudent(Student s) {
    std::cout << s.name << " (ID: " << s.id << ")" << std::endl;
    std::cout << "  Grades: ";
    for (int g : s.grades) {
        std::cout << g << " ";
    }
    if (s.grades.empty()) std::cout << "None";
    std::cout << std::endl;
    std::cout << "  GPA: " << s.gpa << std::endl;
}

double classAverage(vector<Student>& students) {
    if (students.empty()) return 0;
    
    double sum = 0;
    int count = 0;
    for (Student s : students) {
        sum += s.gpa;
        count++;
    }
    return sum / count;
}

int main() {
    vector<Student> students;
    int choice;
    
    do {
        std::cout << "\n=== STUDENT GRADE SYSTEM ===" << std::endl;
        std::cout << "1. Add Student" << std::endl;
        std::cout << "2. Add Grade to Student" << std::endl;
        std::cout << "3. Calculate GPA" << std::endl;
        std::cout << "4. Display All Students" << std::endl;
        std::cout << "5. Find Student" << std::endl;
        std::cout << "6. Statistics" << std::endl;
        std::cout << "7. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            addStudent(students);
        } else if (choice == 2) {
            int id, grade;
            std::cout << "Enter student ID: ";
            std::cin >> id;
            std::cout << "Enter grade: ";
            std::cin >> grade;
            addGrade(students, id, grade);
        } else if (choice == 3) {
            int id;
            std::cout << "Enter student ID: ";
            std::cin >> id;
            Student* s = findStudentByID(students, id);
            if (s) {
                calculateGPA(*s);
                std::cout << "GPA: " << s->gpa << std::endl;
            } else {
                std::cout << "Student not found." << std::endl;
            }
        } else if (choice == 4) {
            std::cout << "\n=== ALL STUDENTS ===" << std::endl;
            for (Student s : students) {
                displayStudent(s);
            }
        } else if (choice == 5) {
            std::cout << "Search by: 1. ID  2. Name : ";
            int searchChoice;
            std::cin >> searchChoice;
            
            if (searchChoice == 1) {
                int id;
                std::cout << "Enter ID: ";
                std::cin >> id;
                Student* s = findStudentByID(students, id);
                if (s) {
                    displayStudent(*s);
                } else {
                    std::cout << "Not found." << std::endl;
                }
            } else {
                string name;
                std::cout << "Enter name: ";
                std::cin.ignore();
                std::getline(std::cin, name);
                Student* s = findStudentByName(students, name);
                if (s) {
                    displayStudent(*s);
                } else {
                    std::cout << "Not found." << std::endl;
                }
            }
        } else if (choice == 6) {
            std::cout << "Class Average GPA: " << classAverage(students) << std::endl;
        } else if (choice == 7) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 7);
    
    return 0;
}
```

---

### Problem 5: Create an Employee Payroll System

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <iomanip>
using std::string;
using std::vector;

struct Employee {
    string name;
    int id;
    string department;
    double salary;
    double hourlyRate;
    double hoursWorked;
};

Employee createEmployee() {
    Employee e;
    std::cout << "Name: ";
    std::cin.ignore();
    std::getline(std::cin, e.name);
    
    std::cout << "ID: ";
    std::cin >> e.id;
    std::cin.ignore();
    
    std::cout << "Department: ";
    std::getline(std::cin, e.department);
    
    std::cout << "Salary (annual): ";
    std::cin >> e.salary;
    
    std::cout << "Hourly Rate: ";
    std::cin >> e.hourlyRate;
    
    std::cout << "Hours Worked (this week): ";
    std::cin >> e.hoursWorked;
    
    return e;
}

double calculateWeeklyPay(Employee e) {
    return e.salary / 52.0;
}

double calculateOvertimePay(Employee e) {
    if (e.hoursWorked <= 40) return 0;
    
    double overtimeHours = e.hoursWorked - 40;
    double overtimeRate = e.hourlyRate * 1.5;
    return overtimeHours * overtimeRate;
}

double calculateTotalPay(Employee e) {
    double basePay = calculateWeeklyPay(e);
    double regularPay = std::min(e.hoursWorked, 40.0) * e.hourlyRate;
    double overtimePay = calculateOvertimePay(e);
    
    return regularPay + overtimePay;
}

void giveRaise(Employee &e, double percent) {
    e.salary *= (1 + percent / 100.0);
}

void displayPaycheck(Employee e) {
    double basePay = calculateWeeklyPay(e);
    double regularPay = std::min(e.hoursWorked, 40.0) * e.hourlyRate;
    double overtimePay = calculateOvertimePay(e);
    double totalPay = regularPay + overtimePay;
    
    std::cout << "\n=== PAYCHECK ===" << std::endl;
    std::cout << "Employee: " << e.name << std::endl;
    std::cout << "Weekly Base Pay: $" << std::fixed << std::setprecision(2) << basePay << std::endl;
    std::cout << "Hours Worked: " << e.hoursWorked << std::endl;
    
    if (e.hoursWorked > 40) {
        std::cout << "Overtime Hours: " << (e.hoursWorked - 40) << std::endl;
        std::cout << "Overtime Pay: $" << overtimePay << std::endl;
    }
    
    std::cout << "Total Weekly Pay: $" << totalPay << std::endl;
}

void displayAllEmployees(vector<Employee>& employees) {
    std::cout << "\n=== ALL EMPLOYEES ===" << std::endl;
    for (Employee e : employees) {
        std::cout << e.name << " (" << e.department << ") - $" 
                  << std::fixed << std::setprecision(2) << e.salary << std::endl;
    }
}

Employee* findHighestPaid(vector<Employee>& employees) {
    if (employees.empty()) return nullptr;
    
    Employee* highest = &employees[0];
    for (int i = 1; i < employees.size(); i++) {
        if (employees[i].salary > highest->salary) {
            highest = &employees[i];
        }
    }
    return highest;
}

double calculatePayrollTotal(vector<Employee>& employees) {
    double total = 0;
    for (Employee e : employees) {
        total += calculateTotalPay(e);
    }
    return total;
}

int main() {
    vector<Employee> employees;
    int choice;
    
    do {
        std::cout << "\n=== PAYROLL SYSTEM ===" << std::endl;
        std::cout << "1. Add Employee" << std::endl;
        std::cout << "2. View Paycheck" << std::endl;
        std::cout << "3. Give Raise" << std::endl;
        std::cout << "4. Display All" << std::endl;
        std::cout << "5. Find Highest Paid" << std::endl;
        std::cout << "6. Total Payroll" << std::endl;
        std::cout << "7. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            employees.push_back(createEmployee());
            std::cout << "Employee added!" << std::endl;
        } else if (choice == 2) {
            int id;
            std::cout << "Enter Employee ID: ";
            std::cin >> id;
            
            bool found = false;
            for (Employee e : employees) {
                if (e.id == id) {
                    displayPaycheck(e);
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Employee not found." << std::endl;
        } else if (choice == 3) {
            int id;
            double percent;
            std::cout << "Enter Employee ID: ";
            std::cin >> id;
            std::cout << "Raise percent: ";
            std::cin >> percent;
            
            bool found = false;
            for (int i = 0; i < employees.size(); i++) {
                if (employees[i].id == id) {
                    giveRaise(employees[i], percent);
                    std::cout << "Raise applied! New salary: $" 
                              << std::fixed << std::setprecision(2) 
                              << employees[i].salary << std::endl;
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Employee not found." << std::endl;
        } else if (choice == 4) {
            displayAllEmployees(employees);
        } else if (choice == 5) {
            Employee* highest = findHighestPaid(employees);
            if (highest) {
                std::cout << "\nHighest Paid: " << highest->name << " ($" 
                          << std::fixed << std::setprecision(2) 
                          << highest->salary << ")" << std::endl;
            }
        } else if (choice == 6) {
            std::cout << "\nTotal Weekly Payroll: $" 
                      << std::fixed << std::setprecision(2) 
                      << calculatePayrollTotal(employees) << std::endl;
        } else if (choice == 7) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 7);
    
    return 0;
}
```

---

### Problem 6: Create a Banking System with Structures

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <iomanip>
#include <sstream>
using std::string;
using std::vector;

struct BankAccount {
    string holder;
    int accountNumber;
    string type;
    double balance;
    vector<string> history;
};

BankAccount createAccount(string holder, string type, double initialBalance) {
    static int nextAccountNum = 1001;
    BankAccount acc;
    acc.holder = holder;
    acc.type = type;
    acc.balance = initialBalance;
    acc.accountNumber = nextAccountNum++;
    
    std::stringstream ss;
    ss << "Initial deposit: $" << std::fixed << std::setprecision(2) 
       << initialBalance << " (Balance: $" << initialBalance << ")";
    acc.history.push_back(ss.str());
    
    return acc;
}

void deposit(BankAccount &acc, double amount) {
    if (amount <= 0) {
        std::cout << "Error: Amount must be positive." << std::endl;
        return;
    }
    
    acc.balance += amount;
    
    std::stringstream ss;
    ss << "Deposit: $" << std::fixed << std::setprecision(2) << amount 
       << " (Balance: $" << acc.balance << ")";
    acc.history.push_back(ss.str());
    
    std::cout << "Deposit successful! New balance: $" << std::fixed 
              << std::setprecision(2) << acc.balance << std::endl;
}

void withdraw(BankAccount &acc, double amount) {
    if (amount <= 0) {
        std::cout << "Error: Amount must be positive." << std::endl;
        return;
    }
    
    if (amount > acc.balance) {
        std::cout << "Error: Insufficient funds." << std::endl;
        return;
    }
    
    acc.balance -= amount;
    
    std::stringstream ss;
    ss << "Withdrawal: $" << std::fixed << std::setprecision(2) << amount 
       << " (Balance: $" << acc.balance << ")";
    acc.history.push_back(ss.str());
    
    std::cout << "Withdrawal successful! New balance: $" << std::fixed 
              << std::setprecision(2) << acc.balance << std::endl;
}

void transfer(BankAccount &from, BankAccount &to, double amount) {
    if (amount <= 0) {
        std::cout << "Error: Amount must be positive." << std::endl;
        return;
    }
    
    if (amount > from.balance) {
        std::cout << "Error: Insufficient funds." << std::endl;
        return;
    }
    
    from.balance -= amount;
    to.balance += amount;
    
    std::stringstream ss1, ss2;
    ss1 << "Transfer out: $" << std::fixed << std::setprecision(2) << amount 
        << " (Balance: $" << from.balance << ")";
    ss2 << "Transfer in: $" << std::fixed << std::setprecision(2) << amount 
        << " (Balance: $" << to.balance << ")";
    
    from.history.push_back(ss1.str());
    to.history.push_back(ss2.str());
    
    std::cout << "Transfer successful!" << std::endl;
}

void displayAccount(BankAccount acc) {
    std::cout << "\n=== ACCOUNT INFO ===" << std::endl;
    std::cout << "Account Holder: " << acc.holder << std::endl;
    std::cout << "Account Number: " << acc.accountNumber << std::endl;
    std::cout << "Type: " << acc.type << std::endl;
    std::cout << "Balance: $" << std::fixed << std::setprecision(2) 
              << acc.balance << std::endl;
}

void displayTransactionHistory(BankAccount acc) {
    std::cout << "\n=== TRANSACTION HISTORY ===" << std::endl;
    for (int i = 0; i < acc.history.size(); i++) {
        std::cout << (i + 1) << ". " << acc.history[i] << std::endl;
    }
}

void displayAllAccounts(vector<BankAccount>& accounts) {
    std::cout << "\n=== ALL ACCOUNTS ===" << std::endl;
    for (BankAccount acc : accounts) {
        std::cout << acc.accountNumber << " - " << acc.holder << " (" 
                  << acc.type << ") - $" << std::fixed << std::setprecision(2) 
                  << acc.balance << std::endl;
    }
}

int main() {
    vector<BankAccount> accounts;
    int choice;
    
    do {
        std::cout << "\n=== BANKING SYSTEM ===" << std::endl;
        std::cout << "1. Create Account" << std::endl;
        std::cout << "2. Deposit" << std::endl;
        std::cout << "3. Withdraw" << std::endl;
        std::cout << "4. Transfer" << std::endl;
        std::cout << "5. View Account" << std::endl;
        std::cout << "6. View History" << std::endl;
        std::cout << "7. View All Accounts" << std::endl;
        std::cout << "8. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            string holder, type;
            double initial;
            std::cout << "Account holder: ";
            std::cin.ignore();
            std::getline(std::cin, holder);
            std::cout << "Account type (Checking/Savings): ";
            std::getline(std::cin, type);
            std::cout << "Initial balance: ";
            std::cin >> initial;
            
            accounts.push_back(createAccount(holder, type, initial));
            std::cout << "Account created! Account number: " 
                      << accounts.back().accountNumber << std::endl;
        } else if (choice == 2) {
            int accNum;
            double amount;
            std::cout << "Account number: ";
            std::cin >> accNum;
            std::cout << "Amount: ";
            std::cin >> amount;
            
            bool found = false;
            for (int i = 0; i < accounts.size(); i++) {
                if (accounts[i].accountNumber == accNum) {
                    deposit(accounts[i], amount);
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Account not found." << std::endl;
        } else if (choice == 3) {
            int accNum;
            double amount;
            std::cout << "Account number: ";
            std::cin >> accNum;
            std::cout << "Amount: ";
            std::cin >> amount;
            
            bool found = false;
            for (int i = 0; i < accounts.size(); i++) {
                if (accounts[i].accountNumber == accNum) {
                    withdraw(accounts[i], amount);
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Account not found." << std::endl;
        } else if (choice == 4) {
            int fromNum, toNum;
            double amount;
            std::cout << "From account: ";
            std::cin >> fromNum;
            std::cout << "To account: ";
            std::cin >> toNum;
            std::cout << "Amount: ";
            std::cin >> amount;
            
            int fromIdx = -1, toIdx = -1;
            for (int i = 0; i < accounts.size(); i++) {
                if (accounts[i].accountNumber == fromNum) fromIdx = i;
                if (accounts[i].accountNumber == toNum) toIdx = i;
            }
            
            if (fromIdx != -1 && toIdx != -1) {
                transfer(accounts[fromIdx], accounts[toIdx], amount);
            } else {
                std::cout << "One or both accounts not found." << std::endl;
            }
        } else if (choice == 5) {
            int accNum;
            std::cout << "Account number: ";
            std::cin >> accNum;
            
            bool found = false;
            for (BankAccount acc : accounts) {
                if (acc.accountNumber == accNum) {
                    displayAccount(acc);
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Account not found." << std::endl;
        } else if (choice == 6) {
            int accNum;
            std::cout << "Account number: ";
            std::cin >> accNum;
            
            bool found = false;
            for (BankAccount acc : accounts) {
                if (acc.accountNumber == accNum) {
                    displayTransactionHistory(acc);
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Account not found." << std::endl;
        } else if (choice == 7) {
            displayAllAccounts(accounts);
        } else if (choice == 8) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 8);
    
    return 0;
}
```

---

### Problem 7: Create a Restaurant Ordering System

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <iomanip>
using std::string;
using std::vector;

struct MenuItem {
    int id;
    string name;
    string description;
    double price;
    bool available;
};

struct Order {
    int orderId;
    string customerName;
    vector<MenuItem> items;
    double total;
    string status;
};

void displayMenu(vector<MenuItem>& menu) {
    std::cout << "\n=== MENU ===" << std::endl;
    for (MenuItem item : menu) {
        if (item.available) {
            std::cout << item.id << ". " << item.name << " - " 
                      << item.description << " - $" << std::fixed 
                      << std::setprecision(2) << item.price << std::endl;
        }
    }
}

Order createOrder(string customerName) {
    static int nextOrderId = 1001;
    Order o;
    o.orderId = nextOrderId++;
    o.customerName = customerName;
    o.total = 0.0;
    o.status = "Pending";
    return o;
}

void addItemToOrder(Order &o, vector<MenuItem>& menu, int itemId) {
    for (MenuItem item : menu) {
        if (item.id == itemId && item.available) {
            o.items.push_back(item);
            o.total += item.price;
            std::cout << item.name << " added!" << std::endl;
            return;
        }
    }
    std::cout << "Item not found or unavailable." << std::endl;
}

void removeItemFromOrder(Order &o, int itemIndex) {
    if (itemIndex >= 0 && itemIndex < o.items.size()) {
        o.total -= o.items[itemIndex].price;
        o.items.erase(o.items.begin() + itemIndex);
        std::cout << "Item removed!" << std::endl;
    } else {
        std::cout << "Invalid item index." << std::endl;
    }
}

void displayOrder(Order o) {
    std::cout << "\n=== ORDER #" << o.orderId << " ===" << std::endl;
    std::cout << "Customer: " << o.customerName << std::endl;
    std::cout << "Status: " << o.status << std::endl;
    std::cout << "Items:" << std::endl;
    for (int i = 0; i < o.items.size(); i++) {
        std::cout << (i + 1) << ". " << o.items[i].name << ": $" 
                  << std::fixed << std::setprecision(2) 
                  << o.items[i].price << std::endl;
    }
    std::cout << "Total: $" << std::fixed << std::setprecision(2) 
              << o.total << std::endl;
}

void displayAllOrders(vector<Order>& orders) {
    std::cout << "\n=== ALL ORDERS ===" << std::endl;
    for (Order o : orders) {
        std::cout << "Order #" << o.orderId << " - " << o.customerName 
                  << " (" << o.status << ") - $" << std::fixed 
                  << std::setprecision(2) << o.total << std::endl;
    }
}

double calculateRevenue(vector<Order>& orders) {
    double revenue = 0;
    for (Order o : orders) {
        if (o.status == "Delivered") {
            revenue += o.total;
        }
    }
    return revenue;
}

int main() {
    vector<MenuItem> menu = {
        {1, "Burger", "Juicy beef burger", 12.99, true},
        {2, "Pizza", "Cheese pizza", 14.99, true},
        {3, "Pasta", "Spaghetti with sauce", 11.99, true},
        {4, "Salad", "Fresh garden salad", 8.99, true}
    };
    
    vector<Order> orders;
    int choice;
    
    do {
        std::cout << "\n=== RESTAURANT ORDERING ===" << std::endl;
        std::cout << "1. View Menu" << std::endl;
        std::cout << "2. Create Order" << std::endl;
        std::cout << "3. Add Item to Order" << std::endl;
        std::cout << "4. Remove Item from Order" << std::endl;
        std::cout << "5. Update Order Status" << std::endl;
        std::cout << "6. View Order" << std::endl;
        std::cout << "7. View All Orders" << std::endl;
        std::cout << "8. Calculate Revenue" << std::endl;
        std::cout << "9. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            displayMenu(menu);
        } else if (choice == 2) {
            string name;
            std::cout << "Customer name: ";
            std::cin.ignore();
            std::getline(std::cin, name);
            
            orders.push_back(createOrder(name));
            std::cout << "Order created! Order #" << orders.back().orderId << std::endl;
        } else if (choice == 3) {
            int orderId, itemId;
            std::cout << "Order ID: ";
            std::cin >> orderId;
            std::cout << "Item ID (1-4): ";
            std::cin >> itemId;
            
            bool found = false;
            for (int i = 0; i < orders.size(); i++) {
                if (orders[i].orderId == orderId) {
                    addItemToOrder(orders[i], menu, itemId);
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Order not found." << std::endl;
        } else if (choice == 4) {
            int orderId, itemIndex;
            std::cout << "Order ID: ";
            std::cin >> orderId;
            std::cout << "Item index (1-based): ";
            std::cin >> itemIndex;
            
            bool found = false;
            for (int i = 0; i < orders.size(); i++) {
                if (orders[i].orderId == orderId) {
                    removeItemFromOrder(orders[i], itemIndex - 1);
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Order not found." << std::endl;
        } else if (choice == 5) {
            int orderId;
            std::string status;
            std::cout << "Order ID: ";
            std::cin >> orderId;
            std::cout << "New status (Pending/Cooking/Ready/Delivered): ";
            std::cin.ignore();
            std::getline(std::cin, status);
            
            bool found = false;
            for (int i = 0; i < orders.size(); i++) {
                if (orders[i].orderId == orderId) {
                    orders[i].status = status;
                    std::cout << "Status updated!" << std::endl;
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Order not found." << std::endl;
        } else if (choice == 6) {
            int orderId;
            std::cout << "Order ID: ";
            std::cin >> orderId;
            
            bool found = false;
            for (Order o : orders) {
                if (o.orderId == orderId) {
                    displayOrder(o);
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Order not found." << std::endl;
        } else if (choice == 7) {
            displayAllOrders(orders);
        } else if (choice == 8) {
            std::cout << "Total Revenue: $" << std::fixed << std::setprecision(2) 
                      << calculateRevenue(orders) << std::endl;
        } else if (choice == 9) {
            std::cout << "Goodbye!" << std::endl;
        }
        
    } while (choice != 9);
    
    return 0;
}
```

---

### Problem 8: Create a Game Character System

**Solution:**

```cpp
#include <iostream>
#include <string>
#include <vector>
using std::string;
using std::vector;

struct Character {
    string name;
    string charClass;
    int level;
    int experience;
    int health;
    int maxHealth;
    int mana;
    int maxMana;
    vector<string> inventory;
};

Character createCharacter(string name, string charClass) {
    Character c;
    c.name = name;
    c.charClass = charClass;
    c.level = 1;
    c.experience = 0;
    
    if (charClass == "Warrior") {
        c.health = 100;
        c.maxHealth = 100;
        c.mana = 30;
        c.maxMana = 30;
    } else if (charClass == "Mage") {
        c.health = 60;
        c.maxHealth = 60;
        c.mana = 80;
        c.maxMana = 80;
    } else {  // Rogue
        c.health = 80;
        c.maxHealth = 80;
        c.mana = 50;
        c.maxMana = 50;
    }
    
    return c;
}

void gainExperience(Character &c, int xp) {
    c.experience += xp;
    std::cout << "Gained " << xp << " XP!" << std::endl;
    
    if (c.experience >= 100) {
        levelUp(c);
    }
}

void levelUp(Character &c) {
    c.level++;
    c.experience = 0;
    c.maxHealth += 20;
    c.health = c.maxHealth;
    c.maxMana += 15;
    c.mana = c.maxMana;
    std::cout << "LEVEL UP! Now level " << c.level << std::endl;
}

void takeDamage(Character &c, int damage) {
    c.health -= damage;
    if (c.health < 0) c.health = 0;
    std::cout << c.name << " takes " << damage << " damage!" << std::endl;
    std::cout << "Health: " << c.health << "/" << c.maxHealth << std::endl;
}

void heal(Character &c, int amount) {
    c.health = std::min(c.health + amount, c.maxHealth);
    std::cout << c.name << " heals for " << amount << " HP!" << std::endl;
    std::cout << "Health: " << c.health << "/" << c.maxHealth << std::endl;
}

void castSpell(Character &c, int manaCost) {
    if (c.mana >= manaCost) {
        c.mana -= manaCost;
        std::cout << "Spell cast! Mana: " << c.mana << "/" << c.maxMana << std::endl;
    } else {
        std::cout << "Not enough mana!" << std::endl;
    }
}

void restoreMana(Character &c, int amount) {
    c.mana = std::min(c.mana + amount, c.maxMana);
    std::cout << "Mana restored! Mana: " << c.mana << "/" << c.maxMana << std::endl;
}

void addItem(Character &c, string item) {
    c.inventory.push_back(item);
    std::cout << item << " added to inventory!" << std::endl;
}

void dropItem(Character &c, int index) {
    if (index >= 0 && index < c.inventory.size()) {
        std::cout << c.inventory[index] << " dropped!" << std::endl;
        c.inventory.erase(c.inventory.begin() + index);
    } else {
        std::cout << "Invalid item index." << std::endl;
    }
}

void displayCharacter(Character c) {
    std::cout << "\n=== " << c.name << " (Level " << c.level << " " 
              << c.charClass << ") ===" << std::endl;
    std::cout << "Health: " << c.health << "/" << c.maxHealth << std::endl;
    std::cout << "Mana: " << c.mana << "/" << c.maxMana << std::endl;
    std::cout << "Experience: " << c.experience << "/100" << std::endl;
}

void displayInventory(Character c) {
    std::cout << "\n=== INVENTORY ===" << std::endl;
    if (c.inventory.empty()) {
        std::cout << "Empty" << std::endl;
    } else {
        for (int i = 0; i < c.inventory.size(); i++) {
            std::cout << (i + 1) << ". " << c.inventory[i] << std::endl;
        }
    }
}

int main() {
    Character hero;
    bool characterCreated = false;
    int choice;
    
    do {
        std::cout << "\n=== CHARACTER SYSTEM ===" << std::endl;
        std::cout << "1. Create Character" << std::endl;
        std::cout << "2. View Stats" << std::endl;
        std::cout << "3. View Inventory" << std::endl;
        std::cout << "4. Gain Experience" << std::endl;
        std::cout << "5. Take Damage" << std::endl;
        std::cout << "6. Heal" << std::endl;
        std::cout << "7. Cast Spell" << std::endl;
        std::cout << "8. Add Item" << std::endl;
        std::cout << "9. Drop Item" << std::endl;
        std::cout << "10. Exit" << std::endl;
        std::cout << "Choice: ";
        std::cin >> choice;
        
        if (choice == 1) {
            if (!characterCreated) {
                string name, charClass;
                std::cout << "Character name: ";
                std::cin.ignore();
                std::getline(std::cin, name);
                
                std::cout << "Choose class:\n1. Warrior\n2. Mage\n3. Rogue\nChoice: ";
                int classChoice;
                std::cin >> classChoice;
                
                if (classChoice == 1) charClass = "Warrior";
                else if (classChoice == 2) charClass = "Mage";
                else charClass = "Rogue";
                
                hero = createCharacter(name, charClass);
                characterCreated = true;
                std::cout << hero.name << " the " << hero.charClass << " created!" << std::endl;
            } else {
                std::cout << "Character already created!" << std::endl;
            }
        } else if (characterCreated) {
            if (choice == 2) {
                displayCharacter(hero);
            } else if (choice == 3) {
                displayInventory(hero);
            } else if (choice == 4) {
                int xp;
                std::cout << "Gain XP: ";
                std::cin >> xp;
                gainExperience(hero, xp);
            } else if (choice == 5) {
                int damage;
                std::cout << "Damage taken: ";
                std::cin >> damage;
                takeDamage(hero, damage);
            } else if (choice == 6) {
                int amount;
                std::cout << "Heal amount: ";
                std::cin >> amount;
                heal(hero, amount);
            } else if (choice == 7) {
                int manaCost;
                std::cout << "Mana cost: ";
                std::cin >> manaCost;
                castSpell(hero, manaCost);
            } else if (choice == 8) {
                string item;
                std::cout << "Item to add: ";
                std::cin.ignore();
                std::getline(std::cin, item);
                addItem(hero, item);
            } else if (choice == 9) {
                int index;
                std::cout << "Item index to drop: ";
                std::cin >> index;
                dropItem(hero, index - 1);
            } else if (choice == 10) {
                std::cout << "Goodbye!" << std::endl;
            }
        } else if (choice != 1 && choice != 10) {
            std::cout << "Create a character first!" << std::endl;
        }
        
    } while (choice != 10);
    
    return 0;
}
```
