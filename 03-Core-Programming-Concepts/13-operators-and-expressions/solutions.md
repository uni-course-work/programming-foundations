# Chapter 13: Solutions

## Section A: Understanding Expressions and Computation

### Problem 1: Expressions, Types, and Values

**Solution:**

1. **`42`**
   - Valid expression: Yes
   - Type: `int`
   - Value: `42`
   - **Explanation:** Integer literal; directly represents the value 42.

2. **`5 + 3`**
   - Valid expression: Yes
   - Type: `int`
   - Value: `8`
   - **Explanation:** Addition of two integers produces an integer. 5 + 3 = 8.

3. **`10 / 3`**
   - Valid expression: Yes
   - Type: `int`
   - Value: `3`
   - **Explanation:** Integer division. 10 ÷ 3 = 3 remainder 1. The remainder is discarded, so result is 3.

4. **`10.0 / 3`**
   - Valid expression: Yes
   - Type: `double`
   - Value: `3.333...`
   - **Explanation:** Floating-point division. 10.0 is double, so 3 is converted to 3.0, then 10.0 ÷ 3.0 = 3.333...

5. **`10 / 3.0`**
   - Valid expression: Yes
   - Type: `double`
   - Value: `3.333...`
   - **Explanation:** Floating-point division. 3.0 is double, so 10 is converted to 10.0, then 10.0 ÷ 3.0 = 3.333...

6. **`x + y` (where `int x = 5;` and `int y = 3;`)**
   - Valid expression: Yes
   - Type: `int`
   - Value: `8`
   - **Explanation:** Both x and y are int. Addition of two ints produces an int. 5 + 3 = 8.

7. **`15 % 4`**
   - Valid expression: Yes
   - Type: `int`
   - Value: `3`
   - **Explanation:** Modulo (remainder). 15 ÷ 4 = 3 remainder 3. Result is the remainder: 3.

8. **`10 > 5`**
   - Valid expression: Yes
   - Type: `bool`
   - Value: `true`
   - **Explanation:** Relational operator. 10 is greater than 5, so the comparison is true.

9. **`(3 + 4) * 2`**
   - Valid expression: Yes
   - Type: `int`
   - Value: `14`
   - **Explanation:** Parentheses first: 3 + 4 = 7. Then multiplication: 7 * 2 = 14.

10. **`3 + 4 * 2`**
    - Valid expression: Yes
    - Type: `int`
    - Value: `11`
    - **Explanation:** Multiplication has higher precedence than addition. 4 * 2 = 8 first, then 3 + 8 = 11.

**Why #9 and #10 differ:**
- #9: Parentheses force `(3 + 4)` to evaluate first, giving `7 * 2 = 14`
- #10: Without parentheses, precedence rules apply: `*` before `+`, so `4 * 2 = 8` first, then `3 + 8 = 11`
- **Lesson:** Parentheses override precedence and make intent explicit.

---

### Problem 2: Integer Division vs. Floating-Point Division

**Solution:**

**For `result1 = a / b;` where `int a = 7;` and `int b = 2;`:**
1. **Value:** `3.0` (not 3.5!)
2. **Why:** `a / b` is integer division (both operands are int). 7 ÷ 2 = 3 (truncated). Then 3 is converted to 3.0 when stored in double.
3. **Type conversion:** The integer result 3 is converted to double 3.0 during assignment.
4. **Surprising:** This is the most surprising! Beginners expect 3.5, but integer division happens BEFORE the conversion to double.

**For `result2 = c / d;` where `double c = 7.0;` and `int d = 2;`:**
1. **Value:** `3.5`
2. **Why:** `c` is double, so `d` is converted to 2.0. Then 7.0 ÷ 2.0 = 3.5 (floating-point division).
3. **Type conversion:** `d` (int) is converted to double before division.
4. **Not surprising:** This works as expected; one operand is double.

**For `result3 = e / f;` where `int e = 7;` and `double f = 2.0;`:**
1. **Value:** `3.5`
2. **Why:** `f` is double, so `e` is converted to 7.0. Then 7.0 ÷ 2.0 = 3.5 (floating-point division).
3. **Type conversion:** `e` (int) is converted to double before division.
4. **Not surprising:** This works as expected; one operand is double.

**Fix for result1 to get 3.5:**
```cpp
double result1 = (double)a / b;  // Cast a to double first
// Or:
double result1 = a / (double)b;  // Cast b to double first
// Or:
double result1 = a / 2.0;        // Use literal 2.0 instead of b
```

**Key insight:** When both operands are int, integer division happens regardless of the result variable's type. Cast at least one operand to force floating-point division.

---

## Section B: Relational and Logical Operators

### Problem 3: Building Complex Conditions

**Solution:**

**Expression:**
```cpp
bool qualifies = (gpa >= 3.5) && (age >= 18 && age <= 25) && (hasFinancialNeed || hasCompletedService);
```

**Breaking down the expression:**

1. **`(gpa >= 3.5)`**: Checks if GPA is at least 3.5
   - With gpa = 3.8: `3.8 >= 3.5` → `true`

2. **`(age >= 18 && age <= 25)`**: Checks if age is in range 18-25 inclusive
   - With age = 22: `22 >= 18` → `true` AND `22 <= 25` → `true`
   - Result: `true && true` → `true`

3. **`(hasFinancialNeed || hasCompletedService)`**: Checks if either condition is true
   - With values: `false || true` → `true`

4. **Final:** `true && true && true` → `true`

**Step-by-step evaluation:**
```
(3.8 >= 3.5) && (22 >= 18 && 22 <= 25) && (false || true)
     true    &&     (true && true)      &&      true
     true    &&          true           &&      true
                        true
```

**If `hasCompletedService` were false:**
- `(hasFinancialNeed || hasCompletedService)` would be `false || false` → `false`
- Final: `true && true && false` → `false`
- **They would NOT qualify**

**Alternative parentheses (same logic):**
```cpp
bool qualifies = ((gpa >= 3.5) && (age >= 18 && age <= 25)) && (hasFinancialNeed || hasCompletedService);
```

---

### Problem 4: The Difference Between = and ==

**Solution:**

**Snippet A:**
```cpp
int x = 5;
if (x == 10) {
    std::cout << "Equal!" << std::endl;
}
```
1. **What happens:** Checks if x equals 10. It doesn't, so condition is false.
2. **Value of x after:** `5` (unchanged)
3. **Does "Equal!" print?** No
4. **This is correct behavior**

**Snippet B:**
```cpp
int x = 5;
if (x = 10) {
    std::cout << "Equal!" << std::endl;
}
```
1. **What happens:** ASSIGNS 10 to x, then checks if 10 is true (non-zero is true, so yes).
2. **Value of x after:** `10` (changed!)
3. **Does "Equal!" print?** YES (always, because 10 is always true)
4. **Why dangerous:** This is a bug! The programmer meant to compare (==) but instead assigned (=). The condition is always true (any non-zero value is true). `x` is modified unintentionally.

**Snippet C:**
```cpp
int x = 5;
bool result = (x = 10);
std::cout << result << std::endl;
```
1. **What happens:** Assigns 10 to x, then converts 10 to bool (true).
2. **Value of x after:** `10`
3. **Does "Equal!" print?** N/A (different snippet)
4. **Output:** `1` (true)
5. **Why confusing:** `result` is true not because x equals something, but because the assignment produces a non-zero value.

**Why Snippet B is dangerous:**
- It's a silent bug; the code compiles without error
- The condition is always true (unless assigning 0)
- The variable is modified when it shouldn't be
- This mistake is extremely common

**How compiler warnings help:**
- Many compilers warn: "Assignment in condition; did you mean ==?"
- Using `-Wall` or similar flags catches this
- Some code styles require: `if ((x = 10))` with extra parentheses if assignment is intentional

---

## Section C: Operator Precedence and Order

### Problem 5: Precedence and Associativity

**Solution:**

1. **`5 + 3 * 2`**
   - Step 1: `3 * 2` = `6` (multiplication before addition)
   - Step 2: `5 + 6` = `11`
   - **With parentheses:** `5 + (3 * 2)` = `11`
   - **Result:** `11` (type: int)

2. **`10 - 5 - 2`**
   - Step 1: `10 - 5` = `5` (left-to-right associativity)
   - Step 2: `5 - 2` = `3`
   - **With parentheses:** `(10 - 5) - 2` = `3`
   - **Result:** `3` (type: int)
   - **Not:** `10 - (5 - 2)` = `7`

3. **`20 / 4 / 2`**
   - Step 1: `20 / 4` = `5` (left-to-right associativity)
   - Step 2: `5 / 2` = `2` (integer division)
   - **With parentheses:** `(20 / 4) / 2` = `2`
   - **Result:** `2` (type: int)
   - **Not:** `20 / (4 / 2)` = `10`

4. **`true || false && false`**
   - Step 1: `false && false` = `false` (&& before ||)
   - Step 2: `true || false` = `true`
   - **With parentheses:** `true || (false && false)` = `true`
   - **Result:** `true` (type: bool)

5. **`!false && true || false`**
   - Step 1: `!false` = `true` (! has highest precedence)
   - Step 2: `true && true` = `true` (&& before ||)
   - Step 3: `true || false` = `true`
   - **With parentheses:** `((!false) && true) || false` = `true`
   - **Result:** `true` (type: bool)

6. **`5 > 3 && 10 < 20 || 8 == 8`**
   - Step 1: `5 > 3` = `true`
   - Step 2: `10 < 20` = `true`
   - Step 3: `8 == 8` = `true`
   - Step 4: `true && true` = `true` (&& before ||)
   - Step 5: `true || true` = `true`
   - **With parentheses:** `((5 > 3) && (10 < 20)) || (8 == 8)` = `true`
   - **Result:** `true` (type: bool)

7. **`2 + 3 * 4 > 10 && 15 / 3 == 5`**
   - Step 1: `3 * 4` = `12`
   - Step 2: `2 + 12` = `14`
   - Step 3: `14 > 10` = `true`
   - Step 4: `15 / 3` = `5`
   - Step 5: `5 == 5` = `true`
   - Step 6: `true && true` = `true`
   - **With parentheses:** `((2 + (3 * 4)) > 10) && ((15 / 3) == 5)` = `true`
   - **Result:** `true` (type: bool)

---

### Problem 6: The Power of Parentheses

**Solution:**

**Original expression:**
```cpp
bool access = age >= 18 && hasID || isStaff && hasPermission;
```

1. **Order of evaluation (based on precedence):**
   - Relational: `age >= 18`
   - Logical AND: `(age >= 18) && hasID`
   - Logical AND: `isStaff && hasPermission`
   - Logical OR: `((age >= 18) && hasID) || (isStaff && hasPermission)`

2. **With explicit parentheses showing default precedence:**
   ```cpp
   bool access = ((age >= 18) && hasID) || (isStaff && hasPermission);
   ```

3. **Could this be misunderstood?** YES! Without parentheses, it's unclear if the intent is:
   - "Adult with ID, OR staff with permission" (what it actually means)
   - "Adult, AND (either has ID OR is staff with permission)" (different meaning)

4. **Rewrite: "Either (adult with ID) or (staff with permission)"**
   ```cpp
   bool access = ((age >= 18) && hasID) || (isStaff && hasPermission);
   ```
   This is what the default precedence already means, but parentheses make it explicit.

5. **Rewrite: "(Adult) and (either has ID or is staff with permission)"**
   ```cpp
   bool access = (age >= 18) && (hasID || (isStaff && hasPermission));
   ```
   Very different meaning! Adult is required, and they must have ID OR be staff with permission.

6. **Why parentheses crucial:** Without them, the reader must:
   - Remember precedence rules (&& before ||)
   - Mentally parse the expression
   - Risk misunderstanding the intent
   
   With parentheses, the intent is immediately clear to any reader.

---

## Section D: Type Conversion and Pitfalls

### Problem 7: Understanding Type Conversion

**Solution:**

**Program output:**
```
Dollars: 1
Pennies: 57
Exact: $1.57
```

1. **Output explained:**
   - `dollars`: `1` (157 / 100 = 1 with integer division)
   - `remainingPennies`: `57` (157 % 100 = 57)
   - `exactDollars`: `1.57` (157 / 100.0 = 1.57 with floating-point division)

2. **Why `dollars` is 1 (not 1.57):**
   - `pennies / 100` is integer division (both operands are int)
   - 157 ÷ 100 = 1 with remainder 57
   - Integer division truncates, so result is 1

3. **Role of modulo in `remainingPennies`:**
   - Modulo (`%`) gives the remainder after division
   - 157 % 100 = 57 (the part that doesn't fit in whole dollars)
   - This separates the whole dollars from the remaining pennies

4. **Why `exactDollars` gives 1.57:**
   - `100.0` is a double literal
   - `pennies` (int 157) is converted to 157.0 (double)
   - 157.0 ÷ 100.0 = 1.57 (floating-point division preserves decimal)

5. **When integer division vs. floating-point:**
   - **Integer division useful:** When you want whole units (dollars, complete groups, pages)
   - **Floating-point useful:** When you want exact proportions (percentages, rates, measurements)
   - **Combined (as here):** Use integer division and modulo to separate whole and fractional parts

6. **Program to convert cents to dollar format:**
   ```cpp
   #include <iostream>
   
   int main() {
       int totalCents;
       
       std::cout << "Enter total cents: ";
       std::cin >> totalCents;
       
       int dollars = totalCents / 100;
       int cents = totalCents % 100;
       
       std::cout << "$" << dollars << ".";
       
       // Print cents with leading zero if needed
       if (cents < 10) {
           std::cout << "0";
       }
       std::cout << cents << std::endl;
       
       return 0;
   }
   ```
   
   **Example runs:**
   - Input: 157 → Output: $1.57
   - Input: 305 → Output: $3.05
   - Input: 5 → Output: $0.05

---

### Problem 8: Complete Program Design with Expressions

**Solution:**

**Complete program:**
```cpp
#include <iostream>

int main() {
    // Get input
    double fahrenheit;
    std::cout << "Enter temperature in Fahrenheit: ";
    std::cin >> fahrenheit;
    
    // Convert to Celsius
    double celsius = (fahrenheit - 32) * 5.0 / 9.0;
    
    // Display results
    std::cout << std::endl;
    std::cout << "TEMPERATURE CONVERSION" << std::endl;
    std::cout << "======================" << std::endl;
    std::cout << "Fahrenheit: " << fahrenheit << "°F" << std::endl;
    std::cout << "Celsius: " << celsius << "°C" << std::endl;
    std::cout << std::endl;
    
    // Determine condition
    if (celsius < 0) {
        std::cout << "Status: FREEZING (below 0°C)" << std::endl;
    } else if (celsius > 100) {
        std::cout << "Status: BOILING (above 100°C)" << std::endl;
    } else {
        std::cout << "Status: COMFORTABLE (0-100°C)" << std::endl;
    }
    
    return 0;
}
```

**All expressions used:**
1. `fahrenheit - 32` (arithmetic: subtraction)
2. `(fahrenheit - 32) * 5.0` (arithmetic: multiplication)
3. `(fahrenheit - 32) * 5.0 / 9.0` (arithmetic: division)
4. `celsius < 0` (relational: less than)
5. `celsius > 100` (relational: greater than)

**Type conversions:**
- `fahrenheit` is double, so all arithmetic produces double
- `5.0` and `9.0` are double literals (not `5` and `9` integers)
- Comparisons with `0` and `100` convert these ints to doubles automatically

**Example input/output:**
```
Enter temperature in Fahrenheit: 32

TEMPERATURE CONVERSION
======================
Fahrenheit: 32°F
Celsius: 0°C

Status: COMFORTABLE (0-100°C)
```

**Why integer division would break the formula:**
If we wrote `(fahrenheit - 32) * 5 / 9`:
- `5` and `9` are integers
- Even though `fahrenheit` is double, `5 / 9` would be integer division
- `5 / 9 = 0` (integer division truncates)
- `(fahrenheit - 32) * 0 = 0` (always!)
- Result would always be 0°C, regardless of input

**Fix:** Use `5.0` and `9.0` to force floating-point division.

---

## Challenge Problems (Optional)

### Challenge 1: Short-Circuit Evaluation

**Solution:**

**Code:**
```cpp
int x = 0;
int y = 10;

bool result = (x != 0) && (y / x > 5);
```

1. **What happens:** The expression is evaluated, result is set to false, no crash.

2. **Does it crash or work safely?** Works safely due to short-circuit evaluation.

3. **Value of `result`:** `false`

4. **How short-circuit prevents division-by-zero:**
   - First part: `(x != 0)` evaluates to `false` (x is 0)
   - Since the first part of `&&` is false, the entire expression MUST be false
   - The second part `(y / x > 5)` is NEVER evaluated
   - Division by zero never happens
   - **This is safe by design**

5. **Similar example using `||`:**
   ```cpp
   int x = 0;
   int y = 10;
   
   bool result = (x == 0) || (y / x > 5);
   // (x == 0) is true
   // Since || needs only one true, second part is never evaluated
   // No division by zero
   // result is true
   ```

6. **When to rely on short-circuit vs. explicit if:**
   - **Rely on short-circuit:** When it's a natural guard condition
     ```cpp
     if (pointer != nullptr && pointer->value > 0) { ... }
     ```
   - **Use explicit if:** When the logic is complex or nested
     ```cpp
     if (x != 0) {
         if (y / x > 5) { ... }
     }
     ```
   - **Short-circuit is elegant but can be subtle; use when intent is clear**

---

### Challenge 2: Operator Precedence Puzzle

**Solution:**

**Code:**
```cpp
int a = 5;
int b = 3;
int c = 2;

bool result1 = a > b && b > c;
bool result2 = a > b || b < c && a == 5;
bool result3 = !(a < b) && b * c > a || c + c == b + 1;
```

**result1:** `a > b && b > c`
- Step 1: `5 > 3` → `true`
- Step 2: `3 > 2` → `true`
- Step 3: `true && true` → `true`
- **Output:** `1` (true)
- **Parentheses:** `(a > b) && (b > c)` = `true`

**result2:** `a > b || b < c && a == 5`
- Step 1: `a > b` → `5 > 3` → `true`
- Step 2: `b < c` → `3 < 2` → `false`
- Step 3: `a == 5` → `5 == 5` → `true`
- Step 4: `false && true` → `false` (&& before ||)
- Step 5: `true || false` → `true`
- **Output:** `1` (true)
- **Parentheses:** `(a > b) || ((b < c) && (a == 5))` = `true`
- **Due to short-circuit:** Since first part is true, second part (`b < c && a == 5`) is never evaluated

**result3:** `!(a < b) && b * c > a || c + c == b + 1`
- Step 1: `a < b` → `5 < 3` → `false`
- Step 2: `!(false)` → `true`
- Step 3: `b * c` → `3 * 2` → `6`
- Step 4: `6 > a` → `6 > 5` → `true`
- Step 5: `c + c` → `2 + 2` → `4`
- Step 6: `b + 1` → `3 + 1` → `4`
- Step 7: `4 == 4` → `true`
- Step 8: `true && true` → `true` (&& before ||)
- Step 9: `true || true` → `true`
- **Output:** `1` (true)
- **Parentheses:** `(!(a < b) && (b * c > a)) || (c + c == b + 1)` = `true`

**Which is most confusing:** `result3` is most confusing because:
- Multiple operators at different precedence levels
- Negation, arithmetic, relational, and logical all mixed
- Hard to trace without careful precedence knowledge
- Would benefit greatly from explicit parentheses

---

### Challenge 3: Type Conversion Chain

**Solution:**

**Expression:**
```cpp
int x = 5;
double y = 2.5;
int z = 3;

double result = (x / z) + y * (double)z - x % z;
```

**Sub-expressions:**

1. **`x / z`**
   - Type: `int` (both operands int)
   - Value: `5 / 3 = 1` (integer division)

2. **`(double)z`**
   - Type: `double`
   - Value: `3.0` (z cast to double)

3. **`y * (double)z`**
   - Type: `double` (y is double, (double)z is double)
   - Value: `2.5 * 3.0 = 7.5`

4. **`x % z`**
   - Type: `int` (both operands int)
   - Value: `5 % 3 = 2`

5. **`(x / z) + y * (double)z`**
   - Type: `double` (1 converted to 1.0, then 1.0 + 7.5)
   - Value: `1.0 + 7.5 = 8.5`

6. **`(x / z) + y * (double)z - x % z`**
   - Type: `double` (8.5 - 2 becomes 8.5 - 2.0)
   - Value: `8.5 - 2.0 = 6.5`

**Order of evaluation:**
1. `x / z` → `1` (division)
2. `(double)z` → `3.0` (cast)
3. `y * 3.0` → `7.5` (multiplication)
4. `x % z` → `2` (modulo)
5. `1 + 7.5` → `8.5` (addition, 1 converts to 1.0)
6. `8.5 - 2` → `6.5` (subtraction, 2 converts to 2.0)

**Final result:** `6.5`

**If `(double)z` cast removed:**
```cpp
double result = (x / z) + y * z - x % z;
```
- `y * z` would be `2.5 * 3`, where z (int 3) converts to 3.0
- Result would still be `2.5 * 3.0 = 7.5`
- **Final result would be the same: 6.5**
- The cast is redundant here since y is already double

**Why understanding type matters:**
- Different types produce different operations (int division vs. float)
- Type conversions can happen implicitly and change results
- Mixing types requires understanding precedence AND type rules
- Debugging requires tracing both values AND types

---
