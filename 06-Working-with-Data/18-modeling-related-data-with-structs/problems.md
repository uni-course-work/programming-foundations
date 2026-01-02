# Chapter 18: Problems

## Section A: Understanding Structures

### Problem 1: Structure Basics and Initialization

Consider this structure definition:

```cpp
struct Book {
    std::string title;
    std::string author;
    int year;
    double price;
};
```

1. How would you create an instance of Book?
2. Write three different ways to initialize a Book with values
3. How do you access the `title` member of a book?
4. What happens if you create a Book without initialization?
5. How would you modify the author of an existing book?
6. Why is a structure better than four separate variables?

---

### Problem 2: Structures and Functions

For each scenario, write the function signature:

1. A function that takes a Book and displays it
2. A function that takes a Book by reference and changes its price
3. A function that takes a Book and returns the discounted price (10% off)
4. A function that takes a vector of Books and returns the most expensive
5. A function that takes a vector of Books and counts how many are from a certain year
6. A function that creates and returns a new Book

For each, explain when to use pass-by-value vs. pass-by-reference.

---

## Section B: Building Programs with Structures

### Problem 3: Create a Book Catalog System

**Write a complete program** that:

1. Defines a Book structure with:
   - Title (string)
   - Author (string)
   - Year (int)
   - Price (double)
   - ISBN (string)

2. Creates an array of 5 books and initializes them

3. Implements functions for:
   - `displayBook()` - show one book's details
   - `displayAllBooks()` - show all books
   - `findByAuthor()` - search for books by author name
   - `findByYear()` - search for books from a certain year
   - `getTotalValue()` - calculate total value of all books
   - `getAveragePrice()` - calculate average price
   - `getMostExpensive()` - find most expensive book

4. In main(), demonstrate all functions

**Example run:**
```
=== BOOK CATALOG ===

All Books:
1. "The Hobbit" by J.R.R. Tolkien (1937) - $15.99
2. "1984" by George Orwell (1949) - $13.99
3. "Brave New World" by Aldous Huxley (1932) - $14.99
4. "The Great Gatsby" by F. Scott Fitzgerald (1925) - $12.99
5. "To Kill a Mockingbird" by Harper Lee (1960) - $16.99

Books by Tolkien:
- "The Hobbit" (1937) - $15.99

Books from 1949:
- "1984" by George Orwell - $13.99

Total Value: $74.95
Average Price: $14.99
Most Expensive: "To Kill a Mockingbird" by Harper Lee ($16.99)
```

Requirements:
- Use structure for Book
- Array of 5 books
- Six functions demonstrating different operations
- Clear, professional output

---

### Problem 4: Create a Student Grade System with Structures

**Write a complete program** that:

1. Defines a Student structure with:
   - Name (string)
   - ID (int)
   - Grades (vector of int, up to 10 grades)
   - GPA (double)

2. Uses a vector of Student structures (dynamic)

3. Implements a menu system:
   - Add Student (name and ID)
   - Add Grade to Student (find by ID, add grade)
   - Calculate GPA (for one student)
   - Display All Students (with their grades and GPA)
   - Find Student (by ID or name)
   - Statistics (class average, highest/lowest GPA)
   - Exit

4. Functions for:
   - `addStudent(vector<Student>&)` - add new student
   - `addGrade(vector<Student>&, int id, int grade)` - add grade to student
   - `calculateGPA(Student&)` - compute GPA from grades
   - `findStudentByID(vector<Student>&, int)` - search by ID
   - `findStudentByName(vector<Student>&, string)` - search by name
   - `displayStudent(Student)` - show one student
   - `classAverage(vector<Student>&)` - average of all GPAs

**Example run:**
```
=== STUDENT GRADE SYSTEM ===
1. Add Student
2. Add Grade to Student
3. Calculate GPA
4. Display All Students
5. Find Student
6. Statistics
7. Exit
Choice: 1

Student name: Alice
Student ID: 101
Student added!

Choice: 2
Enter student ID: 101
Enter grade: 85
Grade added!

Choice: 2
Enter student ID: 101
Enter grade: 90
Grade added!

Choice: 3
Enter student ID: 101
GPA: 3.68

Choice: 4
=== ALL STUDENTS ===
Alice (ID: 101)
  Grades: 85 90
  GPA: 3.68

Choice: 7
Goodbye!
```

Requirements:
- Student structure with vector of grades
- Dynamic vector of students
- Grade management
- GPA calculation
- Search functionality
- Menu-driven interface

---

### Problem 5: Create an Employee Payroll System

**Write a complete program** that:

1. Defines Employee structure with:
   - Name (string)
   - ID (int)
   - Department (string)
   - Salary (double)
   - Hourly Rate (double)
   - Hours Worked (double)

2. Implements functions:
   - `createEmployee()` - create new employee with input
   - `calculateWeeklyPay()` - base salary/52 (weekly)
   - `calculateOvertimePay()` - extra for hours > 40
   - `calculateTotalPay()` - weekly + overtime
   - `giveRaise()` - increase salary by percent
   - `displayPaycheck()` - show detailed pay breakdown
   - `displayAllEmployees()` - list all employees
   - `findHighestPaid()` - employee with highest salary
   - `calculatePayrollTotal()` - total company payroll

3. Menu system for:
   - Add Employee
   - View Employee Paycheck
   - Give Raise
   - Display All
   - Find Highest Paid
   - Calculate Total Payroll
   - Exit

**Example run:**
```
=== PAYROLL SYSTEM ===
1. Add Employee
2. View Paycheck
3. Give Raise
4. Display All
5. Find Highest Paid
6. Total Payroll
7. Exit

Choice: 1
Name: Alice
ID: 101
Department: Engineering
Salary: 80000
Hourly Rate: 0 (if using salary)

Choice: 2
Enter Employee ID: 101

=== PAYCHECK ===
Employee: Alice
Weekly Base Pay: $1538.46
Hours Worked: 45
Overtime Hours: 5
Overtime Pay: $184.53
Total Weekly Pay: $1723.00
```

Requirements:
- Employee structure
- Salary and hourly pay calculation
- Overtime handling
- Raise functionality
- Complete payroll reporting

---

### Problem 6: Create a Banking System with Structures

**Write a complete program** that:

1. Defines BankAccount structure with:
   - Account Holder (string)
   - Account Number (int)
   - Account Type (string) - "Checking" or "Savings"
   - Balance (double)
   - Transaction History (vector of strings)

2. Implements functions:
   - `createAccount()` - new account with starting balance
   - `deposit()` - add money, record transaction
   - `withdraw()` - remove money, validate funds, record transaction
   - `transfer()` - move between two accounts
   - `getInterest()` - calculate interest (for savings)
   - `displayAccount()` - show account details
   - `displayTransactionHistory()` - show all transactions
   - `displayAllAccounts()` - list all accounts (vector)

3. Menu system:
   - Create Account
   - Deposit
   - Withdraw
   - Transfer Between Accounts
   - View Account
   - View Transaction History
   - Calculate Interest
   - Exit

**Example run:**
```
=== BANKING SYSTEM ===
1. Create Account
2. Deposit
3. Withdraw
4. Transfer
5. View Account
6. View History
7. Exit

Choice: 1
Account holder: Alice
Account type: Checking
Starting balance: 1000

Choice: 2
Account number: 1001
Amount: 500
Deposit successful! New balance: $1500.00

Choice: 3
Account number: 1001
Amount: 200
Withdrawal successful! New balance: $1300.00

Choice: 6
Account number: 1001

=== TRANSACTION HISTORY ===
1. Deposit: $500 (Balance: $1500.00)
2. Withdrawal: $200 (Balance: $1300.00)
```

Requirements:
- BankAccount structure with history vector
- Multiple accounts (vector)
- Deposit, withdraw, transfer
- Transaction logging
- Interest calculation
- Comprehensive menu

---

### Problem 7: Create a Restaurant Ordering System

**Write a complete program** that:

1. Defines MenuItem structure:
   - Name (string)
   - Description (string)
   - Price (double)
   - Availability (bool)

2. Defines Order structure:
   - Order ID (int)
   - Customer Name (string)
   - Items Ordered (vector of MenuItem)
   - Total (double)
   - Status (string) - "Pending", "Cooking", "Ready", "Delivered"

3. Implements functions:
   - `displayMenu()` - show all available items
   - `createOrder()` - new order with customer name
   - `addItemToOrder()` - add menu item to order
   - `removeItemFromOrder()` - remove item from order
   - `calculateTotal()` - sum of items
   - `updateOrderStatus()` - change status
   - `displayOrder()` - show order details
   - `displayAllOrders()` - list all orders with status
   - `displayRevenue()` - total from completed orders

4. Menu system:
   - View Menu
   - Create Order
   - Add Item to Order
   - Remove Item from Order
   - Update Order Status
   - View Order
   - View All Orders
   - Calculate Revenue
   - Exit

**Example run:**
```
=== RESTAURANT ORDERING SYSTEM ===

MENU:
1. Burger - Juicy beef burger - $12.99
2. Pizza - Cheese pizza - $14.99
3. Pasta - Spaghetti with sauce - $11.99
4. Salad - Fresh garden salad - $8.99

Choice: 2
Customer name: Alice
Order #1001 created!

Choice: 3
Order ID: 1001
Item number (1-4): 1
Burger added!

Choice: 3
Order ID: 1001
Item number (1-4): 2
Pizza added!

Choice: 7
=== ORDER #1001 ===
Customer: Alice
Items:
  - Burger: $12.99
  - Pizza: $14.99
Total: $27.98
Status: Pending
```

Requirements:
- MenuItem and Order structures
- Menu system with items
- Order management (add/remove items)
- Status tracking
- Revenue calculation
- Complete ordering workflow

---

### Problem 8: Create a Game Character System

**Write a complete program** that:

1. Defines Character structure with:
   - Name (string)
   - Class (string) - "Warrior", "Mage", "Rogue"
   - Level (int)
   - Experience (int, toward next level)
   - Health (int)
   - Max Health (int)
   - Mana (int)
   - Max Mana (int)
   - Inventory (vector of strings - items)

2. Implements functions:
   - `createCharacter()` - new character with class
   - `gainExperience()` - add XP, level up if needed
   - `takeDamage()` - reduce health
   - `heal()` - restore health
   - `castSpell()` - use mana
   - `restoreMana()` - recover mana
   - `addItem()` - pick up item
   - `dropItem()` - remove item
   - `displayCharacter()` - show full stats
   - `displayInventory()` - list items
   - `levelUp()` - increase level, restore health/mana

3. Battle system:
   - Two characters attack each other
   - Damage varies by class (Warrior: 10-15, Mage: 15-20, Rogue: 8-12)
   - Can cast spell to dodge (uses mana)
   - Battle ends when one reaches 0 health

**Example run:**
```
=== CHARACTER SYSTEM ===

Character Name: Aragorn
Choose class:
1. Warrior (high health, medium damage)
2. Mage (low health, high damage, mana)
3. Rogue (medium health, medium damage)
Choice: 1

Aragorn the Warrior created!
Level: 1, Health: 100/100, Mana: 30/30

=== GAME MENU ===
1. View Stats
2. View Inventory
3. Gain Experience
4. Take Damage
5. Heal
6. Battle Another Character
7. Exit

Choice: 3
Gain 50 XP!
Experience: 50/100

Choice: 5
Heal for 25 HP
Health: 100/100

Choice: 1
=== ARAGORN (Level 1 Warrior) ===
Health: 100/100
Mana: 30/30
Experience: 50/100
Inventory: Empty
```

Requirements:
- Character structure with dynamic inventory
- Class-based mechanics
- Level/experience system
- Health/mana management
- Inventory management
- Battle system (optional extension)

---

## Challenge Problems (Optional)

### Challenge 1: Create a Library Management System

**Write a complete program** that manages a library with:
- Book structure (title, author, ISBN, available count)
- Patron structure (name, ID, borrowed books vector)
- Functions for:
  - Add book to library
  - Borrow book (check availability, update patron)
  - Return book (update availability)
  - Find book by title/author
  - List all books
  - List patron's borrowed books
  - Fine calculation (late books)
- Menu-driven interface

---

### Challenge 2: Create a Hotel Reservation System

**Write a complete program** that manages:
- Room structure (number, type, price per night, available dates)
- Guest structure (name, email, reservations vector)
- Reservation structure (room number, guest, check-in, check-out, total cost)
- Functions for:
  - Add room to hotel
  - Create reservation (check availability)
  - Cancel reservation
  - Find available rooms (for dates)
  - Calculate total revenue
  - List all reservations
  - Check-in/check-out operations

---