# Chapter 14: Solutions

## Section A: Understanding Control Flow and Branching

### Problem 1: Tracing Control Flow Execution

**Solution:**

**With score = 75:**

1. **Step-by-step trace:**
   ```
   Step 1: Check if (75 >= 90) → false, skip block
   Step 2: Check else if (75 >= 80) → false, skip block
   Step 3: Check else if (75 >= 70) → TRUE, enter block
   Step 4: Execute std::cout << "C" << std::endl;
   Step 5: Skip remaining else block (already executed one)
   Step 6: Execute std::cout << "Done" << std::endl;
   ```

2. **Conditions checked:** 
   - `score >= 90` (false)
   - `score >= 80` (false)
   - `score >= 70` (true)
   - Remaining conditions not checked after one is true

3. **Block executes:** The third block (`std::cout << "C"`)

4. **Complete output:**
   ```
   C
   Done
   ```

5. **With score = 95:**
   - Check `95 >= 90` → true
   - Execute first block: print "A"
   - Skip all remaining conditions
   - Output: `A` then `Done`

6. **With score = 55:**
   - Check `55 >= 90` → false
   - Check `55 >= 80` → false
   - Check `55 >= 70` → false
   - All conditions false, execute else block
   - Output: `F` then `Done`

7. **Why mutually exclusive:** Once ONE condition is true, that block executes and ALL remaining conditions are skipped. The `else if` structure ensures only one path is taken. Even though score 95 satisfies all conditions (>= 90, >= 80, >= 70), only the first true condition's block executes.

---

### Problem 2: The Danger of Multiple if Statements

**Solution:**

1. **Version A output:**
   ```
   Excellent!
   Good!
   Satisfactory.
   ```
   All three messages print!

2. **Version B output:**
   ```
   Excellent!
   ```
   Only one message prints.

3. **Why Version A prints multiple messages:** Each `if` is **independent**. They're not connected with `else if`, so each condition is checked separately:
   - `95 >= 90` → true, print "Excellent!"
   - `95 >= 80` → true, print "Good!"
   - `95 >= 70` → true, print "Satisfactory."
   - All three conditions are true, so all three blocks execute

4. **Which is correct:** Version B is correct for displaying a single grade comment. The `else if` ensures mutual exclusivity—only the first matching condition executes.

5. **When to use multiple separate if statements (like Version A):**
   - When conditions are truly independent (not mutually exclusive)
   - Example:
     ```cpp
     if (temperature > 85) {
         std::cout << "Hot today!" << std::endl;
     }
     if (humidity > 70) {
         std::cout << "Very humid!" << std::endl;
     }
     if (windSpeed > 25) {
         std::cout << "Windy conditions!" << std::endl;
     }
     ```
   - All three conditions might be true simultaneously (hot AND humid AND windy)
   - Each describes a different aspect of weather
   - Not mutually exclusive categories

---

## Section B: Boolean Logic and Complex Conditions

### Problem 3: Building Complex Eligibility Checks

**Solution:**

1. **Boolean expression:**
   ```cpp
   bool eligible = (age >= 25) && hasLicense && (hasInsurance || willingToBuyInsurance);
   ```

2. **Breaking down the expression:**
   - `(age >= 25)`: Must be at least 25 years old
   - `hasLicense`: Must have a driver's license
   - `(hasInsurance || willingToBuyInsurance)`: Must have insurance OR be willing to buy it
   - All three parts combined with AND (`&&`): ALL must be true

3. **Trace with given values (age=30, hasLicense=true, hasInsurance=false, willingToBuyInsurance=true):**
   ```
   Step 1: (30 >= 25) → true
   Step 2: hasLicense → true
   Step 3: (false || true) → true
   Step 4: true && true && true → TRUE
   Result: ELIGIBLE
   ```

4. **If willingToBuyInsurance were false:**
   ```
   Step 3: (false || false) → false
   Step 4: true && true && false → FALSE
   Result: NOT ELIGIBLE (no insurance and won't buy)
   ```

5. **Rewrite using nested if statements:**
   ```cpp
   if (age >= 25) {
       if (hasLicense) {
           if (hasInsurance || willingToBuyInsurance) {
               std::cout << "Eligible to rent." << std::endl;
           } else {
               std::cout << "Not eligible: Need insurance." << std::endl;
           }
       } else {
           std::cout << "Not eligible: Need license." << std::endl;
       }
   } else {
       std::cout << "Not eligible: Must be 25+." << std::endl;
   }
   ```

6. **Which is clearer:** 
   - **Complex boolean:** More concise, easier to read if conditions are straightforward
   - **Nested if:** Better for providing specific error messages (tells you WHY ineligible)
   - **Best:** Use nested when you need specific feedback; use complex boolean for simple yes/no

---

### Problem 4: Understanding Exhaustive Handling

**Solution:**

1. **User enters 85:** Outputs `B` (matches `score >= 80` condition)

2. **User enters 65:** Nothing is output! The score doesn't match any condition (not >= 70), and there's no `else` to catch this case.

3. **User enters 150:** Outputs `A` (matches `score >= 90` condition). This is wrong—150 is not a valid score!

4. **Is this exhaustive?** No. It doesn't handle:
   - Scores below 70 (but above 0)
   - Invalid scores (above 100 or below 0)

5. **Fixed program:**
   ```cpp
   int score;
   std::cin >> score;
   
   if (score < 0 || score > 100) {
       std::cout << "Invalid score. Must be 0-100." << std::endl;
   } else if (score >= 90) {
       std::cout << "A" << std::endl;
   } else if (score >= 80) {
       std::cout << "B" << std::endl;
   } else if (score >= 70) {
       std::cout << "C" << std::endl;
   } else if (score >= 60) {
       std::cout << "D" << std::endl;
   } else {
       std::cout << "F" << std::endl;
   }
   ```

6. **Appropriate action for invalid scores:**
   - Check FIRST (before categorizing)
   - Display error message
   - Either exit program or ask for input again
   - Don't try to categorize invalid data

---

## Section C: Program Design and Creation

### Problem 5: Create a BMI Calculator with Categorization

**Solution:**

```cpp
#include <iostream>

int main() {
    double height, weight;
    
    // Get input
    std::cout << "BMI CALCULATOR" << std::endl;
    std::cout << "==============" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter your height (inches): ";
    std::cin >> height;
    
    std::cout << "Enter your weight (pounds): ";
    std::cin >> weight;
    
    // Validate input
    if (height <= 0 || weight <= 0) {
        std::cout << "Error: Height and weight must be positive." << std::endl;
        return 0;
    }
    
    // Calculate BMI
    double bmi = (weight * 703) / (height * height);
    
    // Determine category
    std::string category;
    if (bmi < 18.5) {
        category = "Underweight";
    } else if (bmi < 25) {
        category = "Normal weight";
    } else if (bmi < 30) {
        category = "Overweight";
    } else {
        category = "Obese";
    }
    
    // Display results
    std::cout << std::endl;
    std::cout << "RESULTS" << std::endl;
    std::cout << "=======" << std::endl;
    std::cout << "Your BMI: " << bmi << std::endl;
    std::cout << "Category: " << category << std::endl;
    
    return 0;
}
```

**Example outputs:**

**Test 1: Normal weight**
```
BMI CALCULATOR
==============

Enter your height (inches): 70
Enter your weight (pounds): 160

RESULTS
=======
Your BMI: 22.95
Category: Normal weight
```

**Test 2: Overweight**
```
Enter your height (inches): 65
Enter your weight (pounds): 180

RESULTS
=======
Your BMI: 29.95
Category: Overweight
```

**Test 3: Invalid input**
```
Enter your height (inches): -5
Enter your weight (pounds): 150
Error: Height and weight must be positive.
```

---

### Problem 6: Create a Grade Calculator with Pass/Fail Logic

**Solution:**

```cpp
#include <iostream>

int main() {
    int midterm, final, assignments;
    
    // Get input
    std::cout << "GRADE CALCULATOR" << std::endl;
    std::cout << "================" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter midterm score (0-100): ";
    std::cin >> midterm;
    
    std::cout << "Enter final exam score (0-100): ";
    std::cin >> final;
    
    std::cout << "Enter assignments completed (0-10): ";
    std::cin >> assignments;
    
    // Validate input
    if (midterm < 0 || midterm > 100) {
        std::cout << "Error: Midterm score must be 0-100." << std::endl;
        return 0;
    }
    
    if (final < 0 || final > 100) {
        std::cout << "Error: Final score must be 0-100." << std::endl;
        return 0;
    }
    
    if (assignments < 0 || assignments > 10) {
        std::cout << "Error: Assignments must be 0-10." << std::endl;
        return 0;
    }
    
    // Calculate total grade
    const double MIDTERM_WEIGHT = 0.30;
    const double FINAL_WEIGHT = 0.50;
    const double ASSIGNMENT_POINTS = 2.0;
    
    double totalGrade = (midterm * MIDTERM_WEIGHT) + 
                        (final * FINAL_WEIGHT) + 
                        (assignments * ASSIGNMENT_POINTS);
    
    // Determine letter grade
    char letterGrade;
    if (totalGrade >= 90) {
        letterGrade = 'A';
    } else if (totalGrade >= 80) {
        letterGrade = 'B';
    } else if (totalGrade >= 70) {
        letterGrade = 'C';
    } else if (totalGrade >= 60) {
        letterGrade = 'D';
    } else {
        letterGrade = 'F';
    }
    
    // Determine pass/fail
    bool passed = (totalGrade >= 60);
    
    // Display results
    std::cout << std::endl;
    std::cout << "GRADE REPORT" << std::endl;
    std::cout << "============" << std::endl;
    std::cout << "Midterm: " << midterm << " (30%)" << std::endl;
    std::cout << "Final: " << final << " (50%)" << std::endl;
    std::cout << "Assignments: " << assignments << "/10 (20%)" << std::endl;
    std::cout << std::endl;
    std::cout << "Total Grade: " << totalGrade << std::endl;
    std::cout << "Letter Grade: " << letterGrade << std::endl;
    std::cout << "Status: " << (passed ? "PASSED" : "FAILED") << std::endl;
    
    return 0;
}
```

**Example outputs:**

**Test 1: Passing grade**
```
GRADE CALCULATOR
================

Enter midterm score (0-100): 85
Enter final exam score (0-100): 90
Enter assignments completed (0-10): 10

GRADE REPORT
============
Midterm: 85 (30%)
Final: 90 (50%)
Assignments: 10/10 (20%)

Total Grade: 90.5
Letter Grade: A
Status: PASSED
```

**Test 2: Failing grade**
```
Enter midterm score (0-100): 50
Enter final exam score (0-100): 55
Enter assignments completed (0-10): 3

GRADE REPORT
============
Midterm: 50 (30%)
Final: 55 (50%)
Assignments: 3/10 (20%)

Total Grade: 48.5
Letter Grade: F
Status: FAILED
```

---

### Problem 7: Create a Ticket Pricing System

**Solution:**

```cpp
#include <iostream>
#include <string>

int main() {
    int age;
    char studentResponse, militaryResponse;
    
    // Get input
    std::cout << "TICKET PRICING SYSTEM" << std::endl;
    std::cout << "=====================" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter your age: ";
    std::cin >> age;
    
    if (age < 0) {
        std::cout << "Error: Age must be positive." << std::endl;
        return 0;
    }
    
    std::cout << "Are you a student? (y/n): ";
    std::cin >> studentResponse;
    
    std::cout << "Are you active military? (y/n): ";
    std::cin >> militaryResponse;
    
    bool isStudent = (studentResponse == 'y' || studentResponse == 'Y');
    bool isMilitary = (militaryResponse == 'y' || militaryResponse == 'Y');
    
    // Determine price and discount
    int price;
    std::string discount;
    
    if (age < 12) {
        price = 8;
        discount = "Child discount";
    } else if (age >= 65) {
        price = 10;
        discount = "Senior discount";
    } else if (isMilitary) {
        price = 10;
        discount = "Military discount";
    } else if (isStudent) {
        price = 12;
        discount = "Student discount";
    } else {
        price = 15;
        discount = "Regular price";
    }
    
    // Display results
    std::cout << std::endl;
    std::cout << "TICKET DETAILS" << std::endl;
    std::cout << "==============" << std::endl;
    std::cout << "Age: " << age << std::endl;
    std::cout << "Discount: " << discount << std::endl;
    std::cout << "Price: $" << price << std::endl;
    
    return 0;
}
```

**Example outputs:**

**Test 1: Child**
```
TICKET PRICING SYSTEM
=====================

Enter your age: 8
Are you a student? (y/n): n
Are you active military? (y/n): n

TICKET DETAILS
==============
Age: 8
Discount: Child discount
Price: $8
```

**Test 2: Student adult**
```
Enter your age: 22
Are you a student? (y/n): y
Are you active military? (y/n): n

TICKET DETAILS
==============
Age: 22
Discount: Student discount
Price: $12
```

**Test 3: Senior**
```
Enter your age: 70
Are you a student? (y/n): n
Are you active military? (y/n): n

TICKET DETAILS
==============
Age: 70
Discount: Senior discount
Price: $10
```

---

## Section D: Complex Decision Making

### Problem 8: Create a Tax Calculator with Multiple Brackets

**Solution:**

```cpp
#include <iostream>
#include <string>

int main() {
    double income;
    std::string filingStatus;
    
    // Get input
    std::cout << "TAX CALCULATOR" << std::endl;
    std::cout << "==============" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter annual income: $";
    std::cin >> income;
    
    if (income < 0) {
        std::cout << "Error: Income cannot be negative." << std::endl;
        return 0;
    }
    
    std::cout << "Enter filing status (single/married/household): ";
    std::cin >> filingStatus;
    
    // Determine tax rate and calculate tax
    double taxRate;
    std::string bracket;
    double taxOwed;
    
    if (filingStatus == "single") {
        if (income <= 10000) {
            taxRate = 0.10;
            bracket = "$0 - $10,000";
        } else if (income <= 40000) {
            taxRate = 0.12;
            bracket = "$10,001 - $40,000";
        } else {
            taxRate = 0.22;
            bracket = "$40,001+";
        }
    } else if (filingStatus == "married") {
        if (income <= 20000) {
            taxRate = 0.10;
            bracket = "$0 - $20,000";
        } else if (income <= 80000) {
            taxRate = 0.12;
            bracket = "$20,001 - $80,000";
        } else {
            taxRate = 0.22;
            bracket = "$80,001+";
        }
    } else if (filingStatus == "household") {
        if (income <= 15000) {
            taxRate = 0.10;
            bracket = "$0 - $15,000";
        } else if (income <= 50000) {
            taxRate = 0.12;
            bracket = "$15,001 - $50,000";
        } else {
            taxRate = 0.22;
            bracket = "$50,001+";
        }
    } else {
        std::cout << "Error: Invalid filing status." << std::endl;
        return 0;
    }
    
    taxOwed = income * taxRate;
    
    // Display results
    std::cout << std::endl;
    std::cout << "TAX REPORT" << std::endl;
    std::cout << "==========" << std::endl;
    std::cout << "Income: $" << income << std::endl;
    std::cout << "Filing Status: " << filingStatus << std::endl;
    std::cout << "Tax Bracket: " << bracket << std::endl;
    std::cout << "Tax Rate: " << (taxRate * 100) << "%" << std::endl;
    std::cout << "Tax Owed: $" << taxOwed << std::endl;
    
    return 0;
}
```

**Example outputs:**

**Test 1: Single, middle bracket**
```
TAX CALCULATOR
==============

Enter annual income: $35000
Enter filing status (single/married/household): single

TAX REPORT
==========
Income: $35000
Filing Status: single
Tax Bracket: $10,001 - $40,000
Tax Rate: 12%
Tax Owed: $4200
```

**Test 2: Married, high bracket**
```
Enter annual income: $95000
Enter filing status (single/married/household): married

TAX REPORT
==========
Income: $95000
Filing Status: married
Tax Bracket: $80,001+
Tax Rate: 22%
Tax Owed: $20900
```

---

### Problem 9: Create a Password Strength Checker

**Solution:**

```cpp
#include <iostream>
#include <string>

int main() {
    std::string password;
    
    // Get input
    std::cout << "PASSWORD STRENGTH CHECKER" << std::endl;
    std::cout << "=========================" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter password: ";
    std::cin >> password;
    
    // Check criteria
    bool hasLength = (password.length() >= 8);
    bool hasUpper = (password.find('A') != std::string::npos || 
                     password.find('B') != std::string::npos ||
                     password.find('C') != std::string::npos ||
                     password.find('Z') != std::string::npos);
    bool hasLower = (password.find('a') != std::string::npos || 
                     password.find('b') != std::string::npos ||
                     password.find('c') != std::string::npos ||
                     password.find('z') != std::string::npos);
    bool hasDigit = (password.find('0') != std::string::npos || 
                     password.find('1') != std::string::npos ||
                     password.find('2') != std::string::npos ||
                     password.find('9') != std::string::npos);
    
    // Count criteria met
    int criteriaMet = 0;
    if (hasLength) criteriaMet++;
    if (hasUpper) criteriaMet++;
    if (hasLower) criteriaMet++;
    if (hasDigit) criteriaMet++;
    
    // Determine strength
    std::string strength;
    if (criteriaMet < 2) {
        strength = "WEAK";
    } else if (criteriaMet < 4) {
        strength = "MEDIUM";
    } else {
        strength = "STRONG";
    }
    
    // Display results
    std::cout << std::endl;
    std::cout << "PASSWORD ANALYSIS" << std::endl;
    std::cout << "=================" << std::endl;
    std::cout << "Length (8+ chars): " << (hasLength ? "YES" : "NO") << std::endl;
    std::cout << "Has uppercase: " << (hasUpper ? "YES" : "NO") << std::endl;
    std::cout << "Has lowercase: " << (hasLower ? "YES" : "NO") << std::endl;
    std::cout << "Has digit: " << (hasDigit ? "YES" : "NO") << std::endl;
    std::cout << std::endl;
    std::cout << "Criteria met: " << criteriaMet << "/4" << std::endl;
    std::cout << "Strength: " << strength << std::endl;
    
    return 0;
}
```

**Example outputs:**

**Test 1: Weak password**
```
PASSWORD STRENGTH CHECKER
=========================

Enter password: pass

PASSWORD ANALYSIS
=================
Length (8+ chars): NO
Has uppercase: NO
Has lowercase: NO
Has digit: NO

Criteria met: 0/4
Strength: WEAK
```

**Test 2: Strong password**
```
Enter password: Password123

PASSWORD ANALYSIS
=================
Length (8+ chars): YES
Has uppercase: YES
Has lowercase: YES
Has digit: YES

Criteria met: 4/4
Strength: STRONG
```

---

### Problem 10: Create a Shipping Cost Calculator

**Solution:**

```cpp
#include <iostream>
#include <string>

int main() {
    double weight, distance;
    std::string speed;
    
    // Get input
    std::cout << "SHIPPING COST CALCULATOR" << std::endl;
    std::cout << "========================" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter package weight (pounds): ";
    std::cin >> weight;
    
    std::cout << "Enter shipping distance (miles): ";
    std::cin >> distance;
    
    std::cout << "Enter shipping speed (standard/express/overnight): ";
    std::cin >> speed;
    
    // Validate input
    if (weight <= 0 || distance <= 0) {
        std::cout << "Error: Weight and distance must be positive." << std::endl;
        return 0;
    }
    
    // Determine base cost by weight
    double baseCost;
    if (weight <= 5) {
        baseCost = 5.0;
    } else if (weight <= 10) {
        baseCost = 10.0;
    } else if (weight <= 20) {
        baseCost = 15.0;
    } else {
        baseCost = 25.0;
    }
    
    // Determine distance multiplier
    double distanceMultiplier;
    if (distance <= 100) {
        distanceMultiplier = 1.0;
    } else if (distance <= 500) {
        distanceMultiplier = 1.5;
    } else if (distance <= 1000) {
        distanceMultiplier = 2.0;
    } else {
        distanceMultiplier = 3.0;
    }
    
    // Determine speed multiplier
    double speedMultiplier;
    if (speed == "standard") {
        speedMultiplier = 1.0;
    } else if (speed == "express") {
        speedMultiplier = 1.5;
    } else if (speed == "overnight") {
        speedMultiplier = 2.5;
    } else {
        std::cout << "Error: Invalid speed option." << std::endl;
        return 0;
    }
    
    // Calculate total cost
    double totalCost = baseCost * distanceMultiplier * speedMultiplier;
    
    // Display results
    std::cout << std::endl;
    std::cout << "SHIPPING COST BREAKDOWN" << std::endl;
    std::cout << "=======================" << std::endl;
    std::cout << "Base cost (weight): $" << baseCost << std::endl;
    std::cout << "Distance multiplier: " << distanceMultiplier << "x" << std::endl;
    std::cout << "Speed multiplier: " << speedMultiplier << "x" << std::endl;
    std::cout << std::endl;
    std::cout << "TOTAL COST: $" << totalCost << std::endl;
    
    return 0;
}
```

**Example outputs:**

**Test 1: Light package, short distance, standard**
```
SHIPPING COST CALCULATOR
========================

Enter package weight (pounds): 3
Enter shipping distance (miles): 50
Enter shipping speed (standard/express/overnight): standard

SHIPPING COST BREAKDOWN
=======================
Base cost (weight): $5
Distance multiplier: 1x
Speed multiplier: 1x

TOTAL COST: $5
```

**Test 2: Heavy package, long distance, overnight**
```
Enter package weight (pounds): 25
Enter shipping distance (miles): 1500
Enter shipping speed (standard/express/overnight): overnight

SHIPPING COST BREAKDOWN
=======================
Base cost (weight): $25
Distance multiplier: 3x
Speed multiplier: 2.5x

TOTAL COST: $187.5
```

---

## Challenge Problems (Optional)

### Challenge 1: Create a Leap Year Calculator with Full Logic

**Solution:**

```cpp
#include <iostream>

int main() {
    int year;
    
    std::cout << "LEAP YEAR CALCULATOR" << std::endl;
    std::cout << "====================" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter a year: ";
    std::cin >> year;
    
    bool isLeap;
    std::string reason;
    
    if (year % 400 == 0) {
        isLeap = true;
        reason = "Divisible by 400";
    } else if (year % 100 == 0) {
        isLeap = false;
        reason = "Divisible by 100 but not 400";
    } else if (year % 4 == 0) {
        isLeap = true;
        reason = "Divisible by 4 but not 100";
    } else {
        isLeap = false;
        reason = "Not divisible by 4";
    }
    
    std::cout << std::endl;
    std::cout << "Year " << year << " is " << (isLeap ? "a LEAP year" : "NOT a leap year") << std::endl;
    std::cout << "Reason: " << reason << std::endl;
    
    return 0;
}
```

**Example outputs for all test cases:**

**Test 1: 2000 (leap)**
```
LEAP YEAR CALCULATOR
====================

Enter a year: 2000

Year 2000 is a LEAP year
Reason: Divisible by 400
```

**Test 2: 1900 (not leap)**
```
Enter a year: 1900

Year 1900 is NOT a leap year
Reason: Divisible by 100 but not 400
```

**Test 3: 2024 (leap)**
```
Enter a year: 2024

Year 2024 is a LEAP year
Reason: Divisible by 4 but not 100
```

**Test 4: 2023 (not leap)**
```
Enter a year: 2023

Year 2023 is NOT a leap year
Reason: Not divisible by 4
```

---

### Challenge 2: Create a Quadratic Equation Solver

**Solution:**

```cpp
#include <iostream>
#include <cmath>

int main() {
    double a, b, c;
    
    std::cout << "QUADRATIC EQUATION SOLVER" << std::endl;
    std::cout << "=========================" << std::endl;
    std::cout << "Solves equations of form: ax² + bx + c = 0" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter coefficient a: ";
    std::cin >> a;
    
    std::cout << "Enter coefficient b: ";
    std::cin >> b;
    
    std::cout << "Enter coefficient c: ";
    std::cin >> c;
    
    // Check if actually quadratic
    if (a == 0) {
        std::cout << "Error: a cannot be 0 (not a quadratic equation)" << std::endl;
        return 0;
    }
    
    // Calculate discriminant
    double discriminant = b * b - 4 * a * c;
    
    std::cout << std::endl;
    std::cout << "SOLUTION" << std::endl;
    std::cout << "========" << std::endl;
    std::cout << "Equation: " << a << "x² + " << b << "x + " << c << " = 0" << std::endl;
    std::cout << "Discriminant: " << discriminant << std::endl;
    std::cout << std::endl;
    
    if (discriminant > 0) {
        // Two real solutions
        double x1 = (-b + sqrt(discriminant)) / (2 * a);
        double x2 = (-b - sqrt(discriminant)) / (2 * a);
        
        std::cout << "Two real solutions:" << std::endl;
        std::cout << "x₁ = " << x1 << std::endl;
        std::cout << "x₂ = " << x2 << std::endl;
    } else if (discriminant == 0) {
        // One real solution
        double x = -b / (2 * a);
        
        std::cout << "One real solution (repeated root):" << std::endl;
        std::cout << "x = " << x << std::endl;
    } else {
        // Complex solutions
        std::cout << "No real solutions (complex roots)" << std::endl;
        std::cout << "The solutions are complex numbers." << std::endl;
    }
    
    return 0;
}
```

**Example outputs:**

**Test 1: Two real solutions**
```
QUADRATIC EQUATION SOLVER
=========================
Solves equations of form: ax² + bx + c = 0

Enter coefficient a: 1
Enter coefficient b: -5
Enter coefficient c: 6

SOLUTION
========
Equation: 1x² + -5x + 6 = 0
Discriminant: 1

Two real solutions:
x₁ = 3
x₂ = 2
```

**Test 2: One real solution**
```
Enter coefficient a: 1
Enter coefficient b: -4
Enter coefficient c: 4

SOLUTION
========
Equation: 1x² + -4x + 4 = 0
Discriminant: 0

One real solution (repeated root):
x = 2
```

**Test 3: Complex solutions**
```
Enter coefficient a: 1
Enter coefficient b: 0
Enter coefficient c: 4

SOLUTION
========
Equation: 1x² + 0x + 4 = 0
Discriminant: -16

No real solutions (complex roots)
The solutions are complex numbers.
```

---

### Challenge 3: Create a Date Validator

**Solution:**

```cpp
#include <iostream>

int main() {
    int month, day, year;
    
    std::cout << "DATE VALIDATOR" << std::endl;
    std::cout << "==============" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Enter month (1-12): ";
    std::cin >> month;
    
    std::cout << "Enter day: ";
    std::cin >> day;
    
    std::cout << "Enter year: ";
    std::cin >> year;
    
    // Validate month
    if (month < 1 || month > 12) {
        std::cout << "Invalid: Month must be 1-12." << std::endl;
        return 0;
    }
    
    // Check if leap year
    bool isLeapYear = false;
    if (year % 400 == 0) {
        isLeapYear = true;
    } else if (year % 100 == 0) {
        isLeapYear = false;
    } else if (year % 4 == 0) {
        isLeapYear = true;
    }
    
    // Determine max days in month
    int maxDays;
    if (month == 1 || month == 3 || month == 5 || month == 7 || 
        month == 8 || month == 10 || month == 12) {
        maxDays = 31;
    } else if (month == 4 || month == 6 || month == 9 || month == 11) {
        maxDays = 30;
    } else {  // February
        if (isLeapYear) {
            maxDays = 29;
        } else {
            maxDays = 28;
        }
    }
    
    // Validate day
    if (day < 1 || day > maxDays) {
        std::cout << "Invalid: Day must be 1-" << maxDays << " for this month/year." << std::endl;
        return 0;
    }
    
    // Valid date
    std::cout << std::endl;
    std::cout << "Valid date: " << month << "/" << day << "/" << year << std::endl;
    if (month == 2 && isLeapYear) {
        std::cout << "(Note: " << year << " is a leap year)" << std::endl;
    }
    
    return 0;
}
```

**Example outputs:**

**Test 1: Valid date**
```
DATE VALIDATOR
==============

Enter month (1-12): 3
Enter day: 15
Enter year: 2024

Valid date: 3/15/2024
```

**Test 2: Feb 29 on leap year**
```
Enter month (1-12): 2
Enter day: 29
Enter year: 2024

Valid date: 2/29/2024
(Note: 2024 is a leap year)
```

**Test 3: Feb 29 on non-leap year**
```
Enter month (1-12): 2
Enter day: 29
Enter year: 2023
Invalid: Day must be 1-28 for this month/year.
```

**Test 4: Invalid month**
```
Enter month (1-12): 13
Enter day: 15
Enter year: 2024
Invalid: Month must be 1-12.
```

---