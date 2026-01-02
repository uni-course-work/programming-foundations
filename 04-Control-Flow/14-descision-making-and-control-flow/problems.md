# Chapter 14: Problems

## Section A: Understanding Control Flow and Branching

### Problem 1: Tracing Control Flow Execution

Consider this program:

```cpp
#include <iostream>

int main() {
    int score = 75;
    
    if (score >= 90) {
        std::cout << "A" << std::endl;
    } else if (score >= 80) {
        std::cout << "B" << std::endl;
    } else if (score >= 70) {
        std::cout << "C" << std::endl;
    } else {
        std::cout << "F" << std::endl;
    }
    
    std::cout << "Done" << std::endl;
    
    return 0;
}
```

1. Trace through the program step-by-step with `score = 75`
2. Which conditions are checked?
3. Which block executes?
4. What is the complete output?
5. Change `score` to 95. What changes in the execution path?
6. Change `score` to 55. What changes in the execution path?
7. Explain why this is mutually exclusive (only one grade prints)

---

### Problem 2: The Danger of Multiple if Statements

Compare these two versions:

**Version A:**
```cpp
int score = 95;

if (score >= 90) {
    std::cout << "Excellent!" << std::endl;
}
if (score >= 80) {
    std::cout << "Good!" << std::endl;
}
if (score >= 70) {
    std::cout << "Satisfactory." << std::endl;
}
```

**Version B:**
```cpp
int score = 95;

if (score >= 90) {
    std::cout << "Excellent!" << std::endl;
} else if (score >= 80) {
    std::cout << "Good!" << std::endl;
} else if (score >= 70) {
    std::cout << "Satisfactory." << std::endl;
}
```

1. What does Version A output?
2. What does Version B output?
3. Why does Version A print multiple messages?
4. Which version is correct for displaying a single grade comment?
5. When would you intentionally use multiple separate if statements (like Version A)?

---

## Section B: Boolean Logic and Complex Conditions

### Problem 3: Building Complex Eligibility Checks

You're writing a program to check if someone is eligible to rent a car. The rules are:
- Must be at least 25 years old
- Must have a valid driver's license
- Must have insurance OR be willing to purchase insurance from rental company

Given these variables:
```cpp
int age = 30;
bool hasLicense = true;
bool hasInsurance = false;
bool willingToBuyInsurance = true;
```

1. Write a boolean expression that checks eligibility
2. Break down the expression into parts and explain each
3. Trace the evaluation with the given values
4. What if `willingToBuyInsurance` were false? Would they be eligible?
5. Rewrite using nested if statements instead of a complex boolean expression
6. Which version (complex boolean vs. nested) is clearer?

---

### Problem 4: Understanding Exhaustive Handling

Consider this program that converts numeric grades to letter grades:

```cpp
int score;
std::cin >> score;

if (score >= 90) {
    std::cout << "A" << std::endl;
} else if (score >= 80) {
    std::cout << "B" << std::endl;
} else if (score >= 70) {
    std::cout << "C" << std::endl;
}
```

1. What happens if the user enters 85?
2. What happens if the user enters 65?
3. What happens if the user enters 150?
4. Is this exhaustive? Why or why not?
5. Fix the program to handle all cases (including invalid scores)
6. What's the appropriate action for scores above 100 or below 0?

---

## Section C: Program Design and Creation

### Problem 5: Create a BMI Calculator with Categorization

**Design and write a complete program** that:
1. Asks the user for their height in inches
2. Asks the user for their weight in pounds
3. Calculates BMI using the formula: BMI = (weight × 703) / (height²)
4. Categorizes the BMI:
   - BMI < 18.5: Underweight
   - 18.5 ≤ BMI < 25: Normal weight
   - 25 ≤ BMI < 30: Overweight
   - BMI ≥ 30: Obese
5. Displays the BMI value and category

Requirements:
- Validate that height and weight are positive
- Use if-else if-else for categorization
- Format output clearly
- Test with at least 3 different inputs

**Write the complete program and show example outputs.**

---

### Problem 6: Create a Grade Calculator with Pass/Fail Logic

**Design and write a complete program** that:
1. Asks for a student's midterm score (0-100)
2. Asks for their final exam score (0-100)
3. Asks how many assignments they completed (0-10)
4. Calculates total grade: (midterm × 30%) + (final × 50%) + (assignments × 2% each)
5. Determines letter grade (A, B, C, D, F) based on standard scale
6. Determines if student passed (grade >= 60)
7. Displays complete report

Requirements:
- Validate all inputs (scores 0-100, assignments 0-10)
- Use constants for weighting percentages
- Clear output formatting
- Handle edge cases (e.g., perfect scores, zero scores)

**Write the complete program and show example outputs.**

---

### Problem 7: Create a Ticket Pricing System

**Design and write a complete program** that:
1. Asks for visitor's age
2. Asks if they are a student (yes/no)
3. Asks if they are a military member (yes/no)
4. Determines ticket price based on these rules:
   - Children (under 12): $8
   - Seniors (65+): $10
   - Students (with ID): $12
   - Military (active duty): $10
   - Adults (everyone else): $15
   - Note: If someone qualifies for multiple discounts, use the lowest price

Requirements:
- Validate age is positive
- Handle yes/no input for student/military
- Explain which discount applies (if any)
- Display final price clearly
- Use good decision structure

**Write the complete program and show example outputs.**

---

## Section D: Complex Decision Making

### Problem 8: Create a Tax Calculator with Multiple Brackets

**Design and write a complete program** that:
1. Asks for annual income
2. Asks for filing status (single, married, head of household)
3. Calculates tax based on simplified brackets:

   **Single:**
   - $0 - $10,000: 10%
   - $10,001 - $40,000: 12%
   - $40,001+: 22%

   **Married:**
   - $0 - $20,000: 10%
   - $20,001 - $80,000: 12%
   - $80,001+: 22%

   **Head of Household:**
   - $0 - $15,000: 10%
   - $15,001 - $50,000: 12%
   - $50,001+: 22%

4. Displays income, filing status, tax bracket, tax rate, and tax owed

Requirements:
- Validate income is non-negative
- Validate filing status (only accept valid options)
- Use nested if statements or complex conditions
- Clear output with all details

**Write the complete program and show example outputs.**

---

### Problem 9: Create a Password Strength Checker

**Design and write a complete program** that:
1. Asks the user to enter a password (as a string)
2. Checks password strength based on:
   - Length: At least 8 characters (use `password.length()`)
   - Contains uppercase letter (you can simplify by checking one specific letter)
   - Contains lowercase letter (you can simplify by checking one specific letter)
   - Contains a digit (you can simplify by checking one specific digit)
3. Determines strength:
   - Weak: Fewer than 2 criteria met
   - Medium: 2-3 criteria met
   - Strong: All 4 criteria met
4. Displays which criteria are met and overall strength

Requirements:
- Check all four criteria
- Count how many criteria are met
- Display clear feedback for each criterion
- Give overall strength rating

**Simplified approach:** Since we haven't learned loops or character checking, you can:
- Use `password.length() >= 8` for length
- Use `password.find('A') != std::string::npos` to check for 'A' (or any uppercase)
- Use `password.find('a') != std::string::npos` to check for 'a' (or any lowercase)
- Use `password.find('0') != std::string::npos` to check for '0' (or any digit)

**Write the complete program and show example outputs.**

---

### Problem 10: Create a Shipping Cost Calculator

**Design and write a complete program** that:
1. Asks for package weight (in pounds)
2. Asks for shipping distance (in miles)
3. Asks for shipping speed (standard, express, overnight)
4. Calculates shipping cost based on:
   
   **Base cost by weight:**
   - 0-5 lbs: $5
   - 6-10 lbs: $10
   - 11-20 lbs: $15
   - 21+ lbs: $25
   
   **Distance multiplier:**
   - 0-100 miles: 1.0×
   - 101-500 miles: 1.5×
   - 501-1000 miles: 2.0×
   - 1001+ miles: 3.0×
   
   **Speed multiplier:**
   - Standard: 1.0×
   - Express: 1.5×
   - Overnight: 2.5×
   
   Final cost = base cost × distance multiplier × speed multiplier

5. Displays breakdown and total cost

Requirements:
- Validate weight and distance are positive
- Validate speed option
- Show calculation breakdown
- Clear, formatted output

**Write the complete program and show example outputs.**

---

## Challenge Problems (Optional)

### Challenge 1: Create a Leap Year Calculator with Full Logic

**Design and write a complete program** that:
1. Asks for a year
2. Determines if it's a leap year using the complete rules:
   - If divisible by 400: leap year
   - Else if divisible by 100: NOT a leap year
   - Else if divisible by 4: leap year
   - Else: NOT a leap year
3. Explains why (which rule applied)
4. Displays result clearly

Requirements:
- Implement exact leap year algorithm
- Explain which rule determined the result
- Test with: 2000 (leap), 1900 (not leap), 2024 (leap), 2023 (not leap)

**Write the complete program and show example outputs for all test cases.**

---

### Challenge 2: Create a Quadratic Equation Solver

**Design and write a complete program** that:
1. Asks for coefficients a, b, c for equation ax² + bx + c = 0
2. Calculates discriminant: b² - 4ac
3. Determines number and type of solutions:
   - If discriminant > 0: Two real solutions
   - If discriminant = 0: One real solution
   - If discriminant < 0: No real solutions (complex)
4. If real solutions exist, calculates and displays them using quadratic formula:
   x = (-b ± √discriminant) / (2a)
5. Handles edge case where a = 0 (not a quadratic equation)

Requirements:
- Validate a ≠ 0 (if a=0, it's linear)
- Use `#include <cmath>` and `sqrt()` function
- Calculate and display actual solution values
- Clear explanation of solution type

**Write the complete program and show example outputs.**

---

### Challenge 3: Create a Date Validator

**Design and write a complete program** that:
1. Asks for a month (1-12)
2. Asks for a day (1-31)
3. Asks for a year
4. Validates if the date is valid:
   - Month must be 1-12
   - Day must be valid for that month:
     - January, March, May, July, August, October, December: 1-31
     - April, June, September, November: 1-30
     - February: 1-29 if leap year, 1-28 otherwise
5. If valid, displays the date; if invalid, explains why

Requirements:
- Check leap year (use Challenge 1 logic)
- Validate month range
- Validate day range for specific month
- Clear error messages for invalid dates
- Test edge cases (Feb 29 on leap/non-leap years)

**Write the complete program and show example outputs.**

---