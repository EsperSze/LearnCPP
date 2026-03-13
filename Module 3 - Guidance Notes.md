# Module 3: C++ Basics - Comprehensive Guide

## Course Information
- **Courses**: ENGG1340 Computer Programming II, COMP2113 Programming Technologies
- **Duration**: 3 Hours
- **C++ Standard**: C++11 (compile with: `g++ -pedantic -errors -std=c++11 your_program.cpp`)

---

## Part I: Program Compilation & Execution

### Program Structure & Components

#### The First C++ Program (Hello World)
```cpp
#include <iostream>
using namespace std;
int main () {
  cout << "Hello World! " << endl;
  return 0;
}
```

**Key Components:**
- **Comments** (`//`): Text ignored by the compiler
- **`#include` directive**: Tells compiler where to find library information (e.g., `<iostream>`)
- **Namespace** (`using namespace std;`): Allows use of `cout` and `endl` without `std::` prefix
- **`main()` function**: Starting point of program execution
- **`cout`**: Standard output stream (displays to screen)
- **`endl`**: Inserts newline and flushes output
- **`return 0;`**: Indicates successful program completion

### Compilation & Execution (Linux)
1. **Compile**: `g++ -pedantic -errors -std=c++11 hello.cpp -o hello`
2. **Execute**: `./hello`

### Debugging Hints
- ⚠️ Compiler error line numbers may be incorrect; error may be before reported line
- ⚠️ Error nature may be incorrect due to cascading errors
- ✅ **Always fix the first error first**, then recompile and repeat

---

## Part II: Basic Operations

### Variables & Memory

#### Variable Declaration
```cpp
int width;  // Syntax: type_name variable_name;
```
- Variables store data
- Data stored can change over time
- Computer assigns memory cells based on data type

#### Variable Names (Identifiers) - Rules
- Must start with: letter (A-Z, a-z) or underscore (_)
- Following characters: letters, digits (0-9), underscore
- **Case-sensitive**: `radius`, `RADIUS`, `Radius` are different
- **Cannot be C++ reserved keywords**

#### Valid Identifiers ✓ vs Invalid ✗
- ✓ `a_man`, `program_cc`, `year1`, `_oOOo`, `ABCx123`
- ✗ `2008` (starts with digit), `const` (keyword), `-student` (invalid char)

### Data Types

| Type | Description | Size | Range |
|------|---|---|---|
| `char` | Character or small integer | 1 byte | 0-255 |
| `bool` | Boolean value | 1 byte | true(1) or false(0) |
| `int` | Integer | 4 bytes | -2,147,483,648 to 2,147,483,647 |
| `double` | Double precision float | 8 bytes | ±1.7e-308 to ±1.7e+308 (~15 digits) |

### Declarations & Initialization

#### Declaration Syntax
```cpp
int age, steps;                    // Multiple variables same type
char c;
bool win;
double height, width, length;
```

#### Initialization
```cpp
int age = 5, steps = age + 10;
char c = 'Y';                      // Single quotes for char
bool win = true;
double height = 120.5, length = 1.5e3;  // Scientific notation: 1.5e3 = 1500
```

⚠️ **Uninitialized variables** contain "garbage values" - avoid using them!

### Strings - The Basics

```cpp
#include <string>
using namespace std;

int main() {
  string greeting = "Hi", name = "ENGG1340";
  cout << greeting << " " << name << endl;
  greeting = "Good morning";
  cout << greeting << " " << name << endl;
  return 0;
}
```

**Output:**
```
Hi ENGG1340
Good morning ENGG1340
```

### Constants

```cpp
const double PI = 3.14159265359;
// PI = 3.14159;  // ERROR: cannot change const variable
```

#### Types of Constants
- **Integers**: `65` (decimal), `0101` (octal), `0x41` (hex)
- **Floats**: `3.14159`, `6e23`, `1.6e-19`
- **Characters**: `'A'`, `'\n'` (newline), `'\\'` (backslash)
- **Strings**: `"This is a string"`, `""` (empty)
- **Boolean**: `true`, `false`

---

## Expressions, Operators & Precedence

### Expression Components
```cpp
radius * 2 * 3.1416
// operands: radius, 2, 3.1416
// operators: *, *
```

### Arithmetic Operators

| Operator | Symbol | Example |
|----------|--------|---------|
| Addition | `+` | `5 + 3 = 8` |
| Subtraction | `-` | `5 - 3 = 2` |
| Multiplication | `*` | `5 * 3 = 15` |
| Division | `/` | `5 / 2 = 2` (integer) or `5.0 / 2 = 2.5` |
| Modulus | `%` | `13 % 3 = 1` (remainder) |

**Important**: Division with integers = integer division (truncates)
```cpp
int c = 3 / 2;  // c = 1 (not 1.5)
int d = 8 / 3;  // d = 2
```

⚠️ **Division by zero** generates runtime error (not caught at compile time)

### Modulus Operator
- Returns remainder: `13 % 3 = 1` (because 13 = 3*4 + 1)
- Cannot be applied to `double` values

### Character Arithmetic

```cpp
char c = 'Y';
c = c + ('a' - 'A');     // Convert upper to lower: 'Y' → 'y'
cout << c << endl;       // Output: y
c = c + 1;               // Advance to next char: 'y' → 'z'
cout << c << endl;       // Output: z
```

Uses ASCII code values for characters.

### Relational Operators

| Operator | Symbol | Result |
|----------|--------|--------|
| Greater than | `>` | true(1) or false(0) |
| Greater than or equal | `>=` | true(1) or false(0) |
| Less than | `<` | true(1) or false(0) |
| Less than or equal | `<=` | true(1) or false(0) |
| Equal | `==` | true(1) or false(0) |
| Not equal | `!=` | true(1) or false(0) |

```cpp
int a = 1, b = 2;
cout << (a == b);  // Output: 0 (false)
cout << (a < b);   // Output: 1 (true)
```

### Logical Operators

| Operator | Symbol | Meaning |
|----------|--------|---------|
| AND | `&&` | Both must be true |
| OR | `\|\|` | At least one true |
| NOT | `!` | Reverses truth value |

**Truth Table:**
```
A    B    A&&B    A||B    !A
0    0    0        0       1
0    1    0        1       1
1    0    0        1       0
1    1    1        1       0
```

**Short-circuit Evaluation**: C++ stops evaluating once result is known
```cpp
bool i_am_cool = (gals != 0) && ((gifts / gals) >= 2);
// If gals == 0, second part NOT evaluated (avoids division by zero!)
```

### Operator Precedence & Associativity

| Precedence (High→Low) | Operators | Associativity |
|--|--|--|
| Highest | `+`, `-`, `++`, `--`, `!` (unary) | Right to left |
| | `*`, `/`, `%` (binary) | Left to right |
| | `+`, `-` (binary) | Left to right |
| | `<`, `<=`, `>`, `>=` | Left to right |
| | `==`, `!=` | Left to right |
| | `&&` | Left to right |
| | `\|\|` | Left to right |
| Lowest | `=`, `+=`, `-=`, `*=`, `/=`, `%=` | Right to left |

**Examples:**
```cpp
1 + 2 * 3         // = 1 + (2 * 3) = 7 ✓
12 - 11 % 3       // = 12 - (11 % 3) = 12 - 2 = 10 ✓
(1 + 2) * 3       // = 3 * 3 = 9 (parentheses override)
```

### Increment & Decrement Operators

```cpp
// Prefix (increment/decrement before use)
int i = 0;
int c = ++i;      // i becomes 1, then c = 1

// Postfix (increment/decrement after use)
int i = 0;
int c = i++;      // c = 0, then i becomes 1
```

**Difference:**
```cpp
int c = 0, i = 0;
c = ++i;  // i = 1, c = 1
c = i++;  // c = 1, i = 2
```

### Assignment Operators

```cpp
x = y;        // x = y
x += y;       // x = x + y
x -= y;       // x = x - y
x *= y + 1;   // x = x * (y + 1)
x /= y;       // x = x / y
x %= y;       // x = x % y
```

### Type Conversions

**Implicit (Automatic) Conversion:**
```cpp
3.0 / 2;      // 2 promoted to 2.0 → result 1.5
double x = 5; // 5 converted to 5.0
```

**Explicit Type Casting:**
```cpp
int x = (int) 2.8;  // Truncates decimal → x = 2
int x = 2.8;        // Warning issued (implicit)
```

⚠️ Warning issued for implicit conversions (information loss)

---

## Basic Input/Output (I/O)

### Streams Concept
- **Stream**: Object where program inserts or extracts characters
- **Standard input**: Keyboard (via `cin`)
- **Standard output**: Screen (via `cout`)

### Including I/O Library
```cpp
#include <iostream>
using namespace std;
```

### Standard Output - `cout`

```cpp
cout << "Hello!";           // Insert without newline
cout << "Hello!" << endl;   // Insert with newline
cout << 1 << a;             // No spaces added automatically
cout << "b = " << b << " and c = " << c << endl;
```

**Escape Sequences:**
| Sequence | Description |
|--|--|
| `\n` | Newline |
| `\t` | Horizontal tab |
| `\r` | Carriage return |
| `\\` | Backslash |
| `\'` | Single quote |
| `\"` | Double quote |
| `\?` | Question mark |

### Standard Input - `cin`

```cpp
int x;
cin >> x;           // Reads integer

char x;
cin >> x;           // Reads character

double height, weight;
cin >> height >> weight;  // Multiple inputs
```

⚠️ `cin` waits for RETURN key to process input

### Complete I/O Example

```cpp
#include <iostream>
using namespace std;
int main(){
  int age;
  double height, weight;
  cout << "Please input your age, height and weight: ";
  cin >> age >> height >> weight;
  cout << endl << "Your age is " << age << endl;
  cout << "Your height is " << height << endl;
  cout << "Your weight is " << weight << endl;
  return 0;
}
```

**Sample Run:**
```
Please input your age, height and weight: 20 175.5 132
Your age is 20
Your height is 175.5
Your weight is 132
```

### File Redirection (Testing)
Avoid repeatedly entering input:
```bash
./program < input.txt    # Feed file as input
./program > output.txt   # Redirect output to file
```

---

## Part III: Flow of Control

### Algorithms & Pseudocode

**Algorithm**: Procedure with actions and execution order

**Pseudocode Example** (Problem: Add two integers):
```
Prompt user for 1st integer
Input 1st integer
Prompt user for 2nd integer
Input 2nd integer
Add integers, store result
Display result
```

### Flow of Control Types
1. **Sequential**: Execute statements one after another (default)
2. **Branching**: Choose between alternative actions (if/else, switch)
3. **Looping**: Repeat actions multiple times (while, for)

---

## BRANCHING - Making Decisions

### The `if` Statement

```cpp
if (condition) {
  statement;
}
```

**Example:**
```cpp
if (mark >= 60)
  cout << "passed";

// With block
if (mark >= 60) {
  grade = 'P';
  cout << "passed";
}
```

### The `if...else` Statement

```cpp
if (condition) {
  statement1;
} else {
  statement2;
}
```

**Example:**
```cpp
if (mark >= 60) {
  cout << "passed";
} else {
  cout << "failed";
}
```

### Nested `if...else` Statements

```cpp
if (age >= 18 && age <= 25) {
  if (height >= 180)
    cout << "Yes I do!";
  else
    cout << "I am sorry.";
} else {
  cout << "Bye bye.";
}
```

### Compound Statements & Blocks

```cpp
// Single statement
if (mark >= 60)
  grade = 'P';

// Multiple statements (block)
if (mark >= 60) {
  grade = 'P';
  cout << "passed";
}
```

### Example 1: Find Maximum of Two Numbers

**Pseudocode:**
```
Read a and b
Is a > b?
  YES: max = a
  NO: max = b
Output max
```

**Code:**
```cpp
#include <iostream>
using namespace std;
int main() {
  int a, b, max;
  cin >> a >> b;
  if (a > b)
    max = a;
  else
    max = b;
  cout << max << endl;
  return 0;
}
```

### Example 2: Find Maximum of Three Numbers

```cpp
#include <iostream>
using namespace std;
int main() {
  int a, b, c, max;
  cin >> a >> b >> c;
  if (a > b)
    max = a;
  else
    max = b;
  if (max > c)
    cout << max << endl;
  else
    cout << c << endl;
  return 0;
}
```

---

### The Dangling-Else Problem

⚠️ **Indentation does NOT determine blocks in C++!**

```cpp
// These look different but are treated the same by compiler
if (x > 5)
  if (y > 5)
    cout << "x and y > 5";
else
  cout << "x <= 5";  // Which if does this pair with?
```

**Rule**: `else` pairs with **nearest previous unpaired `if`**

**Solution: Use braces `{}`**
```cpp
if (x > 5) {
  if (y > 5)
    cout << "x and y > 5";
} else {
  cout << "x <= 5";  // Now paired with first if ✓
}
```

### Multi-way `if-else` Statements

**Grade Assignment Example:**
```cpp
if (mark >= 90)
  cout << "A";
else if (mark >= 80)
  cout << "B";
else if (mark >= 70)
  cout << "C";
else if (mark >= 60)
  cout << "D";
else
  cout << "F";
```

**Advantages**:
- ✓ Stops checking once condition is true (faster)
- ✓ More compact than series of separate `if` statements

### The `switch` Statement

```cpp
switch (controlling_expression) {
  case constant_1:
    statement_1;
    break;
  case constant_2:
    statement_2;
    break;
  default:
    default_statement;
}
```

**Requirements:**
- Expression must return: `bool`, `int`, or `char`
- `break` statement exits switch (otherwise continues to next case!)
- `default` case optional (executes if no match)

**Example: Grade Points**
```cpp
char grade;
cin >> grade;
switch (grade) {
  case 'A':
    cout << "grade point is 4.0";
    break;
  case 'B':
    cout << "grade point is 3.0";
    break;
  case 'C':
    cout << "grade point is 2.0";
    break;
  case 'D':
    cout << "grade point is 1.0";
    break;
  case 'F':
    cout << "grade point is 0.0";
    break;
  default:
    cout << "grade is invalid";
}
```

**Multiple Cases for Same Action:**
```cpp
switch (mark / 10) {
  case 0: case 1: case 2: case 3:
  case 4: case 5:
    grade = 'F';
    break;
  case 6:
    grade = 'D';
    break;
  // ... more cases
}
```

⚠️ **Missing `break` Causes Fall-through** (usually wrong!)
```cpp
switch (x) {
  case 1:
    cout << "one";
    // Missing break! Will continue to case 2
  case 2:
    cout << "two";  // This executes too!
    break;
}
```

### The Ternary Operator `?:`

Shorthand for simple `if-else`:
```cpp
// if-else version
if (mark >= 60)
  cout << "passed";
else
  cout << "failed";

// Ternary operator version
cout << ((mark >= 60) ? "passed" : "failed");
```

**Syntax:**
```cpp
condition ? expr1 : expr2;
// If condition true → expr1 (value of expression)
// If condition false → expr2 (value of expression)
```

### Common Mistakes in Boolean Conditions

❌ **Assignment instead of equality:**
```cpp
if (a = 10)    // WRONG: assigns 10 to a
if (a == 10)   // CORRECT
```

❌ **Bitwise operators instead of logical:**
```cpp
if (a != 0 & b > 0)   // WRONG: bitwise AND
if (a != 0 && b > 0)  // CORRECT: logical AND
```

❌ **String inequalities:**
```cpp
if (a < b < c)          // WRONG and confusing
if (a < b && b < c)     // CORRECT
```

---

## LOOPING - Doing Something Repeatedly

### Loop Concepts
- **Loop**: Program construction that repeats statements
- **Loop body**: Statement(s) to be repeated
- **Iteration**: One repetition of loop body

### The `while` Statement

```cpp
while (condition) {
  statement_1;
  statement_2;
  // ...
}
```

**Execution**:
1. Evaluate condition
2. If true: execute loop body → go to step 1
3. If false: exit loop

**Example: Repeated Question**
```cpp
#include <iostream>
using namespace std;
int main() {
  int answer = 0;
  while (answer != 4) {
    cout << "2 * 2 = ";
    cin >> answer;
  }
  cout << "Correct!" << endl;
  return 0;
}
```

### Counter-Controlled `while` Loop

```cpp
#include <iostream>
using namespace std;
int main() {
  int x = 0, total = 0, i, n;
  cout << "Enter # of values to add: ";
  cin >> n;

  i = 0;                    // Initialization
  while (i < n) {           // Condition
    cout << "next number? ";
    cin >> x;
    total += x;
    cout << "Total = " << total << endl;
    i++;                    // Update loop variable
  }
  return 0;
}
```

⚠️ **Must update loop variable** in loop body, or infinite loop!

### Sentinel-Controlled `while` Loop

```cpp
#include <iostream>
using namespace std;
int main() {
  int x = 0, total = 0;
  cout << "Enter a negative number to end." << endl;

  while (x >= 0) {
    total += x;
    cout << "Total = " << total << endl;
    cout << "next number? ";
    cin >> x;  // User enters special "sentinel" value to end
  }
  cout << "Program ends." << endl;
  return 0;
}
```

### Flag-Controlled `while` Loop

```cpp
#include <iostream>
using namespace std;
int main() {
  int num = 23;
  int guess;
  bool isGuessed = false;

  while (!isGuessed) {
    cout << "Make a guess (0-99)? ";
    cin >> guess;
    if (guess == num) {
      cout << "Correct!" << endl;
      isGuessed = true;
    } else if (guess < num)
      cout << "Too small. Guess again!" << endl;
    else
      cout << "Too large. Guess again!" << endl;
  }
  return 0;
}
```

⚠️ **Don't put semicolon after while condition!**
```cpp
while (i < n);  // WRONG: empty loop body!
{
  cout << "This won't execute repeatedly";
}
```

---

### The `for` Statement

```cpp
for (initialization; condition; updating) {
  statement_1;
  statement_2;
  // ...
}
```

**Example:**
```cpp
#include <iostream>
using namespace std;
int main() {
  int i;
  for (i = 1; i <= 20; ++i)
    cout << i << endl;
  return 0;
}
```

**Execution Steps:**
1. **Initialization** (`i = 1`): Executed once before loop starts
2. **Condition** (`i <= 20`): Checked before each iteration
3. **Statement**: Executed if condition true
4. **Updating** (`++i`): Executed after each iteration, then go to step 2

### `for` vs `while`

**Using `while`:**
```cpp
int i = 0, n = 3;
while (i < n) {
  cout << "next number? ";
  cin >> x;
  total += x;
  cout << "Total = " << total << endl;
  i++;
}
```

**Using `for`:**
```cpp
int n = 3;
for (int i = 0; i < n; i++) {
  cout << "next number? ";
  cin >> x;
  total += x;
  cout << "Total = " << total << endl;
}
```

**Advantages of `for`**: Loop control (initialization, condition, update) in one place

### Quick Exercise Solutions

**Exercise 1: Output 1-20 using while loop**
```cpp
#include <iostream>
using namespace std;
int main() {
  int i = 1, n = 20;
  while (i <= n) {
    cout << i << endl;
    i++;
  }
  return 0;
}
```

**Exercise 2: Output 9 8 7 6 5 4 3 2 1 0 using for loop**
```cpp
#include <iostream>
using namespace std;
int main() {
  for (int i = 9; i >= 0; --i)
    cout << i << ' ';
  return 0;
}
```

**Exercise 3: Sum of odd numbers 1-20 using for loop**
```cpp
#include <iostream>
using namespace std;
int main() {
  int i, sum = 0;
  for (i = 1; i <= 20; ++i) {
    if (i % 2 == 1) {
      sum += i;
    }
  }
  cout << sum << endl;  // Output: 100
  return 0;
}
```

---

### The `break` Statement

Exits loop immediately; continues after loop
```cpp
for (int i = 0; i >= 0; i++) {
  if (i == 15) break;     // Exit when i == 15
  cout << i << " ";
}
// Output: 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14
```

**Can also exit `switch` statement:**
```cpp
switch (grade) {
  case 'A':
    cout << "Excellent";
    break;  // Exit switch
}
```

⚠️ **Avoid overusing `break`** - can make code harder to understand

### The `continue` Statement

Skips rest of current iteration; continues with next iteration
```cpp
for (int i = 0; i < 20; ++i) {
  if (i % 2 == 0) continue;  // Skip even numbers
  cout << i << " ";
}
// Output: 1 3 5 7 9 11 13 15 17 19
```

**Differences:**
- `break`: Exits the loop entirely
- `continue`: Skips to next iteration

### Break and Continue Examples

**Using `break`:**
```cpp
for (int count = 1; count <= 10; ++count) {
  if (count == 5) break;
  cout << count << " ";
}
// Output: 1 2 3 4
```

**Using `continue`:**
```cpp
for (int count = 1; count <= 10; ++count) {
  if (count == 5) continue;
  cout << count << " ";
}
// Output: 1 2 3 4 6 7 8 9 10
```

---

## Coding Best Practices

### ✓ Good Habits
1. **Visualize logic** before coding (use flowcharts/pseudocode)
2. **Declare variables** before use
3. **Initialize variables** to avoid garbage values
4. **Use meaningful identifiers** (prefer `radius` over `r`)
5. **Use proper indentation** for readability (even though C++ ignores it)
6. **Use braces `{}`** to avoid dangling-else and ambiguity
7. **Test early** - compile frequently and fix errors one at a time
8. **Use multi-way if-else** instead of multiple separate `if` statements

### ⚠️ Common Mistakes to Avoid
- Using assignment `=` instead of comparison `==`
- Missing `break` in switch statements (causes fall-through)
- Uninitialized variables
- Infinite loops (forgot to update loop variable)
- Semicolon after `while` condition
- Assuming indentation determines blocks (Python vs C++)

---

## Summary of Key Concepts

| Concept | Usage |
|---------|-------|
| `if...else` | Single or double selection |
| `switch` | Multiple selections based on expression value |
| `while` | Repeat while condition true |
| `for` | Repeat fixed or controlled number of times |
| `break` | Exit loop early |
| `continue` | Skip to next iteration |
| `cout` | Display output |
| `cin` | Read input |
| `++i` vs `i++` | Prefix vs postfix increment |
| Type casting | `(int) 2.8` → 2 |

---

**Remember**: Practice all code examples by typing them yourself, running them, and experimenting with modifications to understand the behavior deeply!