# Chapter 7: Debugging as a Core Skill

## Introduction: The Heart of Programming

**You will spend more time debugging than writing code.**

This is not a failure on your part. This is the reality of programming.

Professional developers spend 35–50% of their time debugging and testing. Not learning. Not designing. Actually finding and fixing broken code. Debugging is not something that happens after you finish programming. Debugging *is* programming.

Yet most beginners treat debugging like a chore, something to avoid if possible. They would rather write new code than fix broken code. They see debugging as evidence of failure.

This is backwards. **Debugging is where you learn the most about how your code actually works.** Debugging is where you develop deep understanding. Debugging is a core skill, not a side effect.

This chapter teaches you to debug effectively: systematically, methodically, and with purpose.

---

## Part 1: What Bugs Are and Why They're Inevitable

### The Nature of Bugs

**A bug is a discrepancy between what your code does and what it should do.**

There are three categories of bugs:

**Syntax Errors**
- Your code violates the rules of the programming language
- Example: Missing semicolon, unmatched bracket, using undefined variable
- Severity: Critical—prevents the program from running
- Detection: Compiler catches these immediately
- Good news: These are the easiest bugs because the computer tells you exactly where they are

**Runtime Errors**
- Your code is syntactically correct, but something goes wrong when it runs
- Example: Dividing by zero, accessing array out of bounds, dereferencing null pointer
- Severity: Critical—the program crashes
- Detection: Only caught when you run the program with problematic input
- Good news: The program usually gives you an error message telling you what went wrong

**Logic Errors**
- Your code is syntactically correct and doesn't crash, but it produces the wrong result
- Example: Algorithm calculates the wrong answer, loop terminates too early, condition is inverted
- Severity: Critical—the program does not do what it should
- Detection: Only found by testing and observing incorrect output
- Bad news: These are the hardest bugs because the computer does not tell you anything is wrong

**Which bugs are most dangerous?** Logic errors. Because at least with syntax and runtime errors, you know something is broken. Logic errors hide. They produce plausible-looking output. They pass some tests and fail others in ways that seem random. They are the bugs that ship to production and cause real damage.

### Why Bugs Are Inevitable

You might think bugs are a sign of carelessness. If you were just more careful, more precise, bugs would not happen.

This is wrong.

Bugs are inevitable for three fundamental reasons:

**Reason 1: Complexity**

Programs are complex. Even a small program has many moving parts. A program with 100 lines of code has hundreds of potential interactions between those lines. A program with 10,000 lines has billions of potential interactions.

Your brain cannot hold all of these interactions in mind simultaneously. Even the most careful programmer will miss something.

**Reason 2: Changing Requirements**

When you write code, you write it with certain assumptions about how it will be used. Later, someone uses it in a way you did not anticipate. Suddenly, it breaks. This is not your fault. It is the nature of software.

**Reason 3: Cascading Effects**

One bug can hide another. You fix bug A, and suddenly bug B (which has always been there) becomes visible. You fix bug B, and bug C appears. Bugs interact in unpredictable ways.

### The Inevitability Mindset

**Accept that bugs are inevitable.** Not as failure, but as normal.

The question is not "How do I write code without bugs?" The question is "How do I find and fix bugs efficiently when they appear?"

This mental shift is crucial. When you accept bugs as inevitable, debugging stops feeling like failure and starts feeling like investigation. You switch from defensive (hoping to avoid bugs) to proactive (expecting bugs and preparing to find them).

---

## Part 2: Systematic Debugging vs. Guesswork

### The Wrong Way: Guessing

Many beginners debug by guessing. They see a bug, they do not understand it, so they change something and see if it helps. If it does, great. If not, they change something else. Change, test, change, test, repeat.

This approach is called "random debugging" or "spray and pray debugging." And it is a waste of time.

Why? Because you are not learning anything. You might fix the bug by accident, but you do not understand what caused it. You do not develop intuition about where bugs hide. You do not build skill in finding similar bugs in the future.

Worse, changing code randomly often introduces new bugs while fixing the original one. You end up with a broken program that is broken in a different way.

**Never debug by guessing.**

### The Right Way: Systematic Debugging

Systematic debugging is a process. It is the application of scientific method to broken code.

**The Systematic Debugging Process:**

```
1. UNDERSTAND THE SYMPTOM
   ├── What is the unexpected behavior?
   ├── When does it happen?
   ├── When does it NOT happen?
   └── What data triggers it?

2. REPRODUCE THE BUG
   ├── Create a test case that reliably triggers the bug
   ├── Verify the bug happens every time with that input
   └── Write down the exact steps to reproduce

3. UNDERSTAND THE SYSTEM
   ├── What is the code supposed to do?
   ├── Where is the relevant code?
   ├── What are the key functions involved?
   └── What is the data flow?

4. FORM A HYPOTHESIS
   ├── Based on the symptom, where could the bug be?
   ├── Make a narrow, testable hypothesis
   ├── Do NOT guess; base it on evidence
   └── Try to eliminate as much of the system as possible

5. TEST THE HYPOTHESIS
   ├── Design a test to validate or invalidate the hypothesis
   ├── Observe the results carefully
   ├── Record what you learned
   └── Adjust your hypothesis based on results

6. REPEAT STEPS 4-5
   ├── Keep forming narrower hypotheses
   ├── Keep testing until you find the bug
   └── Use binary search: eliminate 50% each iteration

7. FIX THE BUG
   ├── Once you find the bug, understand why it happened
   ├── Make the minimal necessary fix
   ├── Do NOT over-engineer
   └── Test to ensure the fix works

8. VERIFY THE FIX
   ├── Test with the original failing case
   ├── Test with edge cases
   ├── Test related functionality
   └── Ensure the fix does not introduce new bugs
```

The key difference: systematic debugging is iterative. You gain information with each cycle. Each hypothesis is narrower than the last. Each test eliminates possibilities.

### The Binary Search Technique

The most powerful debugging technique is binary search. It works like this:

1. You have a program with 1,000 lines of code. One of them has a bug.
2. Instead of reading all 1,000 lines, you test the first 500.
3. If the bug is there, you now know the bug is in lines 1-500 (not 501-1000).
4. You test the first 250. This narrows it to either 1-250 or 251-500.
5. You keep halving the search space.
6. After just 10 tests, you have narrowed the problem to about 1 line.

This is much faster than reading and analyzing code sequentially.

**How to apply binary search:**

Comment out half the code and test. If the bug disappears, the bug is in the commented-out code. If the bug persists, the bug is in the code that is still running.

Repeat with whichever half contains the bug.

### Forming Good Hypotheses

The quality of your debugging depends on the quality of your hypotheses. Good hypotheses are:

**Specific**: Not "something is wrong with the loop" but "the loop is incrementing the counter twice per iteration"

**Testable**: You can design a test to prove or disprove it

**Evidence-based**: Grounded in what you have observed, not in guesses

**Narrow**: Eliminates as much of the system as possible

Bad hypotheses are vague ("the algorithm is broken"), untestable ("something bad happened"), or based on guesses rather than evidence.

When you form a hypothesis, ask: "What single test would prove or disprove this?"

If you cannot answer that question, your hypothesis is too vague. Refine it.

---

## Part 3: Mental Tracing and Reasoning About Program State

### What Is Program Tracing?

**Program tracing (or mental tracing) is the process of mentally simulating a program execution step by step.**

You read through the code. For each line, you execute it in your head, keeping track of what the variables are, what the flow is, what the results would be.

This is one of the most important debugging skills. It is also one of the hardest because it requires you to hold multiple pieces of information in your working memory.

Research in cognitive psychology shows that humans can hold about 7 pieces of information in working memory at a time. When you are tracing a program, you need to hold:

- Current line number
- Current values of relevant variables
- The call stack (which functions have called which)
- The condition being evaluated
- Loop counters and loop conditions
- Accumulated results

That is 6 things already, and you only have 1 slot left.

This is why tracing large programs is hard. You run out of working memory.

### How to Trace Efficiently

**Technique 1: Write it down**

Do not try to hold everything in your head. Use paper. Write down the values of variables as they change. Write down the order of operations.

This sounds tedious, but it is much faster than trying to hold everything in your head and getting confused.

**Technique 2: Start with concrete examples**

Do not trace abstractly. Pick specific input values and trace through with those values.

Example: Instead of tracing "a function that finds the maximum," trace "the function with input [3, 1, 4, 1, 5]."

Concrete values let you actually calculate results and see if they are correct. Abstract reasoning leads to confusion.

**Technique 3: Mark the important parts**

Not every variable matters. Focus on the variables that are relevant to the suspected bug.

If the bug is about iteration, focus on loop variables and loop conditions. If the bug is about calculation, focus on the values being computed.

**Technique 4: Use a trace table**

Create a table showing how variables change with each iteration:

```
Iteration | i | counter | total | Action
----------|---|---------|-------|------------------
Start     | 0 | 0       | 0     | Initialize
1         | 1 | 1       | 1     | i++, counter++, total=1
2         | 2 | 2       | 3     | i++, counter++, total=3
3         | 3 | 3       | 6     | i++, counter++, total=6
End       | 4 | 3       | 6     | Loop exits (i >= 3)
```

This makes it easy to see patterns and catch errors.

**Technique 5: Trace backwards from the error**

If you know the incorrect output, trace backwards. What value would have had to be wrong to produce that output? Work backwards until you find where the wrong value came from.

### A Worked Example of Mental Tracing

Let's trace this pseudocode to find the bug:

```
FUNCTION CountToFive()
    counter = 0
    FOR i = 1 to 5
        counter = counter + i
    END FOR
    RETURN counter
END FUNCTION
```

**Trace Table:**

| Iteration | i | counter before | counter after | Expected after |
|-----------|---|----------------|---------------|-----------------|
| Start     | - | 0              | -             | 0               |
| 1         | 1 | 0              | 1             | 1               |
| 2         | 2 | 1              | 3             | 3               |
| 3         | 3 | 3              | 6             | 6               |
| 4         | 4 | 6              | 10            | 10              |
| 5         | 5 | 10             | 15            | 15              |
| End       | - | 15             | -             | 15              |

Result: The function returns 15, which is 1+2+3+4+5. This is correct!

**Now let's trace a buggy version:**

```
FUNCTION CountToFive()
    counter = 0
    FOR i = 1 to 5
        counter = counter + 1
    END FOR
    RETURN counter
END FUNCTION
```

| Iteration | i | counter before | counter after | Expected after |
|-----------|---|----------------|---------------|-----------------|
| Start     | - | 0              | -             | 0               |
| 1         | 1 | 0              | 1             | 1               |
| 2         | 2 | 1              | 2             | 3               |
| 3         | 3 | 2              | 3             | 6               |
| 4         | 4 | 3              | 4             | 10              |
| 5         | 5 | 4              | 5             | 15              |
| End       | - | 5              | -             | 15              |

Result: The function returns 5, but we expected 15. **Bug found!** The code is adding 1 each iteration instead of adding the value of i.

By tracing through with a table, we found the exact mistake and understand exactly what to fix.

### Program State as a Mental Model

When you trace a program, you are building a mental model of the program's state: what is happening at each step.

The better you can maintain this mental model, the better you can debug. And the key to maintaining a good mental model is to be explicit:

- Write down the variables
- Write down their values
- Write down how they change
- Update your model after each step

Do not try to hold this all in your head.

---

## Part 4: Fault Isolation

### What Is Fault Isolation?

**Fault isolation is the process of identifying exactly where a bug is located in the code.**

You know your program is broken. You might even know what the symptom is. But you do not know which part of the code is responsible. Fault isolation is the process of narrowing down the location.

The goal: from "something in this program is wrong" to "this specific line is wrong."

### Techniques for Fault Isolation

**Technique 1: Divide and Conquer**

Split the system into two halves. Determine whether the bug is in the first half or the second half. Repeat with whichever half contains the bug.

```
Program (1000 lines)
├── Lines 1-500 (test: does bug appear?)
│   ├── Lines 1-250 (test: does bug appear?)
│   ├── Lines 251-500
├── Lines 501-1000
```

After each test, you eliminate one half.

**Technique 2: Instrumentation**

Add diagnostic output to your code to track what is happening.

Pseudocode example:

```
FUNCTION CalculateAverage(numbers)
    OUTPUT "CalculateAverage called with " + numbers
    
    sum = 0
    FOR each number in numbers
        OUTPUT "  Adding " + number + " to sum"
        sum = sum + number
        OUTPUT "  Sum is now " + sum
    END FOR
    
    count = length(numbers)
    OUTPUT "  Count is " + count
    
    average = sum / count
    OUTPUT "  Average calculated as " + average
    OUTPUT "Returning " + average
    
    RETURN average
END FUNCTION
```

When you run this code, the diagnostic output tells you exactly what is happening at each step. You can see where things go wrong.

**Technique 3: Test Isolation**

Create small test cases that exercise specific parts of your code in isolation.

Instead of testing the entire program, test individual functions with specific inputs.

Example test cases:

```
Test 1: Empty list
  Input: []
  Expected: Error or 0
  
Test 2: Single item
  Input: [42]
  Expected: 42
  
Test 3: Two items
  Input: [2, 4]
  Expected: 3
  
Test 4: Negative numbers
  Input: [-5, 5]
  Expected: 0
```

Run each test and observe. If test 3 fails but tests 1 and 2 pass, the bug is specific to lists with multiple items.

**Technique 4: Hypothesis Testing**

Form a hypothesis about where the bug is. Design a test to confirm or refute it.

Example:

**Hypothesis**: "The bug is in the calculation step, not in the input gathering."

**Test**: Create a function that takes pre-computed values and calculates the average. If it works, the bug is not in the calculation. If it fails, the bug might be.

**Technique 5: Dependency Mapping**

Draw a diagram showing which parts of your program depend on which other parts.

```
main()
├── GetInput()
│   └── ReadFile()
├── ProcessData()
│   ├── ValidateData()
│   └── TransformData()
└── DisplayResults()
    └── FormatOutput()
```

If GetInput() is broken, then ProcessData() might also look broken (because it received bad data). If you find bad data, trace it backwards to see where it came from.

### A Systematic Fault Isolation Process

```
1. OBSERVE THE SYMPTOM
   └── "Program returns 42 instead of 100"

2. REPRODUCE WITH SMALL INPUT
   └── Find the smallest input that triggers the bug

3. BINARY SEARCH FOR LOCATION
   ├── Test first half of code
   ├── Determine which half has the bug
   └── Repeat with that half

4. NARROW DOWN WITH TESTS
   ├── Test individual functions
   ├── Test with edge cases
   └── Identify which function fails first

5. TRACE THAT FUNCTION
   └── Use mental tracing to find exact line

6. INSTRUMENT IF NEEDED
   └── Add diagnostic output if tracing is too complex

7. IDENTIFY THE BUG
   └── "Line 47: off-by-one error in loop"
```

### Why Isolation Matters

Fault isolation is not just about finding the bug. It is about:

- **Confidence**: Once you have isolated the bug to a specific location, you can be confident about your fix
- **Verification**: After fixing, you know exactly what to test to ensure the fix worked
- **Learning**: You understand exactly what went wrong and why
- **Prevention**: Understanding the bug prevents similar bugs in the future

Developers who skip isolation and just guess at fixes often fix the symptom while leaving the root cause. Then the bug appears in a different context later.

---

## Part 5: Building Effective Debugging Habits

### Habit 1: Always Reproduce First

Before you do anything else, reproduce the bug reliably.

Write down:
- Exactly what input causes it
- What you did to trigger it
- What happens when you do it again

If you cannot reproduce it consistently, the bug is hiding. Keep experimenting until you can reproduce it.

**Why**: A bug you cannot reproduce is a bug you cannot fix. And a bug you cannot fix might appear again later.

### Habit 2: Do Not Change Code Yet

When you find a bug, your instinct is to immediately start changing code hoping to fix it.

Resist this instinct.

First, understand. First, trace. First, form a hypothesis. Only then change code.

**Why**: If you change code before understanding the bug, you might fix it by accident while introducing new bugs. And you will not learn anything.

### Habit 3: Make Minimal Changes

When you finally fix the bug, make the smallest possible change that fixes it.

Do not refactor the entire function. Do not rewrite the algorithm. Do not add "improvements."

Make one tiny change. Test. If it works, you are done.

**Why**: Minimal changes are easier to verify. They are less likely to introduce new bugs. And they are easier to understand later.

### Habit 4: Keep a Debug Journal

When you fix a bug, write it down:

- What was the bug?
- What was the symptom?
- How did you find it?
- What was the fix?
- What did you learn?

Over time, you will notice patterns. You will recognize bugs you have seen before. You will develop intuition.

**Why**: Debugging is a skill that improves with practice and reflection. A debug journal accelerates that practice.

### Habit 5: Test After Fixing

After you fix a bug, test thoroughly:

- Test the case that triggered the bug
- Test related cases
- Test edge cases
- Test cases that pass different paths through the code

Do not assume the fix is complete until you have tested thoroughly.

**Why**: Bugs often have more than one manifestation. Your fix might solve one but miss another. Only thorough testing gives you confidence.

---

## Part 6: Debugging Recipes

### Recipe 1: Off-by-One Error

**Symptom**: Loop processes 1 too many or 1 too few items. Array access gets the wrong element.

**Common causes**:
- Loop condition uses `<=` instead of `<`
- Loop starts at 1 instead of 0
- Loop ends at n instead of n-1
- Array indexing starts at 1 instead of 0

**Debugging process**:

```
TRACE: Loop with boundary values
├── What happens on first iteration?
├── What happens on last iteration?
└── Does the last iteration process the right element?

TEST: Small input
├── With 1 item: Does it process that 1 item?
├── With 2 items: Does it process exactly 2?
└── With 0 items: Does it exit without error?
```

**Example**:
```
FOR i = 0 to length-1    (CORRECT: processes all items)
FOR i = 0 to length      (WRONG: processes one too many)
FOR i = 1 to length-1    (WRONG: skips first item)
```

### Recipe 2: Uninitialized Variable

**Symptom**: Variable has garbage value. Result is random or undefined.

**Common causes**:
- Variable declared but never assigned
- Variable assigned conditionally, but not all paths assign it
- Variable assigned in one scope, used in another

**Debugging process**:

```
TRACE: When is the variable first used?
├── Has it been assigned before that use?
├── Are all code paths that reach it assigning it first?
└── Is it being used outside its scope?

TEST: Simple case
├── Create a test with minimal code
├── Output the variable immediately
└── See if it has a value or garbage
```

**Example**:
```
FUNCTION ProcessNumber(x)
    result        (WRONG: not initialized)
    IF x > 0
        result = x * 2
    END IF
    RETURN result  (Wrong if x is not > 0)

FUNCTION ProcessNumber(x)
    result = 0    (CORRECT: initialized)
    IF x > 0
        result = x * 2
    END IF
    RETURN result
```

### Recipe 3: Wrong Operator

**Symptom**: Condition evaluates opposite of what expected. Calculation gives wrong result.

**Common causes**:
- `=` instead of `==`
- `>` instead of `>=`
- `AND` instead of `OR`
- Operator precedence misunderstanding

**Debugging process**:

```
TRACE: The problematic condition
├── What is the condition?
├── What should it evaluate to?
├── What is it actually evaluating to?
└── Which part is wrong?

TEST: Isolate the condition
├── Extract just the condition
├── Test with specific values
└── See what it evaluates to
```

**Example**:
```
IF age > 18      (CORRECT: over 18 is allowed)
IF age >= 18     (Different semantics: exactly 18 is different)
IF age = 18      (WRONG: only age 18, not a range)
```

### Recipe 4: Loop Never Exits

**Symptom**: Program hangs. Loop runs forever.

**Common causes**:
- Loop condition is always true
- Loop variable is never updated
- Loop update is in the wrong place
- Exit condition is impossible to reach

**Debugging process**:

```
TRACE: The loop condition
├── What makes the loop exit?
├── Is that condition ever true?
├── Does the loop variable change each iteration?
└── Does it change in the right direction?

TEST: Manual iteration
├── Start with initial values
├── Check condition: is it true?
├── Update values
├── Repeat a few times
└── Does condition ever become false?
```

**Example**:
```
i = 0
WHILE i < 10
    OUTPUT i
    i = i + 1        (CORRECT: i increases)
END WHILE

i = 0
WHILE i < 10
    OUTPUT i
    i = i - 1        (WRONG: i decreases, will never reach 10)
END WHILE

i = 0
WHILE i < 10
    OUTPUT i
    (no update)      (WRONG: i never changes)
END WHILE
```

### Recipe 5: Unexpected Data Type

**Symptom**: Function works with strings but fails with numbers. Type mismatch error.

**Common causes**:
- Data conversion is missing
- Function expects one type but receives another
- Type comparison uses wrong operator

**Debugging process**:

```
TRACE: Data flow
├── Where does the data come from?
├── What type is it initially?
├── Does it get converted?
├── What type does it have when it reaches the problem?
└── Is that the type the function expects?

TEST: Type checking
├── Output the type at each step
├── Verify assumptions about types
└── Check conversions
```

---

## Part 7: The Debugging Mindset

### Debugging is Not Punishment

Many beginners see debugging as something that happens when they have "done something wrong." They feel ashamed of bugs and want to hide them.

Change this. **Bugs are not evidence of failure. Bugs are opportunities to learn.**

Every bug teaches you something:
- A pattern you did not account for
- An edge case you did not consider
- A misunderstanding you had about the language
- A careless mistake to avoid in the future

### Debugging Builds Intuition

Over time, as you debug more, you build intuition about where bugs hide.

You start to recognize patterns. "This looks like an off-by-one error." "This looks like an uninitialized variable." "This looks like a type mismatch."

This intuition comes from experience. Every bug you fix adds to that experience.

### Debugging Reveals How Code Really Works

Reading code is one thing. Understanding how code actually executes is another.

When you trace through code carefully, you see things you would not see by just reading. You see the order of execution. You see how data flows. You see where assumptions break down.

This deep understanding is invaluable.

### Debugging is a Superpower

Developers who debug systematically are vastly more effective than those who guess.

When something goes wrong, they do not panic. They do not randomly change code. They methodically trace, isolate, and fix.

This skill is rarer than you might think. Many developers have been programming for years without developing systematic debugging skills. They stumble through, fixing by luck more than anything else.

If you develop systematic debugging skills now, you will be exceptional.

---

## Part 8: Reflection Questions

### On Bugs and Their Inevitability

1. **How do you currently feel when you encounter a bug?** Frustrated? Ashamed? Curious?

2. **What bugs have you encountered so far?** Syntax errors? Runtime errors? Logic errors? Which are most common?

3. **Can you think of a complex program you use?** How many bugs do you think it has? Why might such complex programs still have bugs?

### On Systematic Debugging

4. **When you find a bug, is your first instinct to start changing code, or to understand the bug first?** How could you shift that impulse?

5. **Have you used binary search to isolate a bug?** How would it feel to use that approach instead of guessing?

6. **Can you describe a specific bug and trace through your approach to finding it?** Was it systematic or did you guess?

### On Mental Tracing

7. **Have you ever traced through code on paper?** How did it feel compared to just reading code?

8. **When you trace code, do you keep track of variable values?** What helps you remember all the state?

9. **Can you create a trace table for a simple pseudocode function?** How does seeing the values change help you understand the code?

### On Fault Isolation

10. **Have you isolated a bug to a specific function or line?** How did you narrow it down?

11. **What testing techniques do you know?** Could you use them to isolate bugs more systematically?

12. **If a function is broken, how would you test its sub-components to find which one is wrong?** Can you think of an example?

### On Debugging Habits

13. **Do you always reproduce a bug before trying to fix it?** Why is that important?

14. **When you fix a bug, do you make minimal changes or do you often refactor?** What are the pros and cons of each?

15. **Do you keep track of bugs you have found and fixed?** What would you learn if you reviewed those patterns?

---

## Summary

**Debugging is the core of programming.**

The debugging process:
1. Understand the symptom
2. Reproduce the bug
3. Understand the system
4. Form a hypothesis
5. Test the hypothesis
6. Repeat until you find the bug
7. Fix it minimally
8. Verify the fix

Key techniques:
- **Binary search**: Eliminate 50% of the system each iteration
- **Mental tracing**: Simulate execution step-by-step on paper
- **Fault isolation**: Narrow down exactly where the bug lives
- **Instrumentation**: Add diagnostic output to understand execution
- **Test isolation**: Test individual functions with specific inputs

The mindset shift:
- Bugs are inevitable, not evidence of failure
- Systematic debugging beats guessing every time
- Every bug is a learning opportunity
- Debugging skill is rarer than programming skill
- Good debuggers are exceptional programmers

Build these habits:
1. Always reproduce first
2. Understand before changing
3. Make minimal changes
4. Keep a debug journal
5. Test thoroughly after fixing

When you master debugging, you master programming.

---

## Looking Forward

In the next chapter, we will explore the **Development Environment and Toolchain**—the tools and systems that support your programming work.

For now, practice debugging. The next time something breaks, do not guess. Do not randomly change code. Do not panic.

Trace. Isolate. Understand. Then fix.

This discipline is what separates effective programmers from those who merely survive.
[Development Environment and Toolchain](./08-dev-environ-and-toolchain.md)