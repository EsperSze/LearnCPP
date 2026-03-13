# Module 5: Functions - Comprehensive Summary

## Course Information
- **Courses**: ENGG1340 Computer Programming II / COMP2113 Programming Technologies
- **Estimated Time of Completion**: 2.5 Hours

---

## Table of Contents
1. [Top-Down Design (Divide and Conquer)](#top-down-design)
2. [Predefined Functions vs. Self-Defined Functions](#predefined-functions)
3. [Function Definition, Call & Declaration](#function-definition)
4. [Flow of Control](#flow-of-control)
5. [Scope of Variables](#scope-of-variables)
6. [Parameter Passing Mechanism](#parameter-passing)

---

## Top-Down Design (Divide and Conquer) {#top-down-design}

### Core Concept
Program design breaks down complex tasks into smaller, manageable sub-tasks. This hierarchical decomposition continues until each sub-task is simple enough to implement directly.

### Key Principle
- Each module should perform a **single, well-defined task**
- Preserves structure making programs easier to understand and modify

### Benefits of Using Functions
1. **Focus**: Easy to concentrate on particular tasks and debug
2. **Collaboration**: Multiple developers can work on different functions simultaneously
3. **Reusability**: Functions written once can be used multiple times within the same program or in different programs
4. **Readability**: Reduces complexity of `main()` by organizing code into logical units
5. **Maintenance**: Easier to test and modify individual components

### Example: Simple GPA Calculator
```
┌─ Read in data
├─ Output result to screen
└─ Compute GPA
   ├─ Convert marks to grade and grade point
   ├─ Compute sum of weighted grade points
   ├─ Compute sum of credit units
   └─ Compute GPA and round to 2 decimal places
```

---

## Predefined Functions vs. Self-Defined Functions {#predefined-functions}

### What are Predefined Functions?
**Definition**: Pre-built, tested functions provided by C++ libraries that implement common tasks.

**Key Advantages**:
- No need to rewrite common functionality
- Already tested and optimized
- Saves development time

**What You Need to Know**:
1. The required inputs (function parameters)
2. The output produced (return value)

### Using Predefined Functions

#### Function Libraries in C++
A C++ library contains:
1. **Function definitions**: Pre-compiled object codes stored in separate files
2. **Function declarations**: Stored in header files (`.h` or included via `#include`)

#### Common C++ Math Functions

| Function Declaration | Description | Library | Header |
|---|---|---|---|
| `double sqrt(double x)` | Square root of x | cmath | `<cmath>` |
| `double pow(double x, double y)` | x to the power of y (x^y) | cmath | `<cmath>` |
| `double fabs(double x)` | Absolute value of x | cmath | `<cmath>` |
| `double ceil(double x)` | Round up x | cmath | `<cmath>` |
| `double floor(double x)` | Round down x | cmath | `<cmath>` |
| `int abs(int x)` | Absolute value of x | cstdlib | `<cstdlib>` |
| `int rand()` | Random integer [0, RAND_MAX] | cstdlib | `<cstdlib>` |

### How to Use Predefined Functions

**Step 1**: Include the header file with the `#include` directive
```cpp
#include <iostream>
#include <cmath>
using namespace std;
```

**Step 2**: Refer to function documentation to understand parameters and return type

**Step 3**: Call the function with correct arguments

#### Example: Root Mean Square Calculation
```cpp
#include <iostream>
#include <cmath>
using namespace std;

int main() {
    // Compute the root mean square of 10 input numbers
    int i;
    double n, sq_sum = 0;
    
    for(i = 0; i < 10; i++) {
        cout << i+1 << ": ";
        cin >> n;
        sq_sum += pow(n, 2.0);  // pow(n, 2.0) raises n to power 2
    }
    
    cout << "The root mean square is " << sqrt(sq_sum/10) << endl;
    return 0;
}
```

**Key Points**:
- Function parameter order and type matter
- Consult function reference for each parameter's meaning
- Function overloading: Same function name with different parameter types

### Random Number Generation

#### The Challenge
Random numbers are essential for games and simulations. However, computers use algorithms that depend on a **seed value**. Same seed = same sequence.

#### Solution: Using `srand()` and `time()`

**Problem**: Generate truly random numbers each program run
```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

int main() {
    for (int i = 0; i < 10; i++) 
        cout << rand() % 100 + 1 << endl;  // Same output every run!
    return 0;
}
```

**Solution**: Seed with current system time
```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

int main() {
    srand(time(NULL));  // Initialize with current time
    for (int i = 0; i < 10; i++) 
        cout << rand() % 100 + 1 << endl;  // Different each run
    return 0;
}
```

#### Generating Random Numbers in Ranges
- Range [0, 9]: `rand() % 10`
- Range [0, 100]: `rand() % 101`
- Range [1, 100]: `rand() % 100 + 1`

**Important Note**: For debugging, sometimes you DO want the same sequence. In that case, fix the seed:
```cpp
srand(0);  // Always generates the same sequence
```

---

## Function Definition, Call & Declaration {#function-definition}

### Defining Your Own Functions

#### Questions to Ask When Creating a Function
1. **What are the inputs?** (And their data types)
2. **What is the output?** (And its data type)
3. **What operations are needed?**

#### Example: Function to Find Larger of Two Numbers

**Step 1: Identify Inputs & Output**
- Inputs: Two floating-point numbers (type: `double`)
- Output: One floating-point number (type: `double`)

**Step 2: Design Function Header**
```cpp
double larger(double x, double y)
    //    ↑       ↑            ↑
    //  return  name      parameters
    //  type
```

**Step 3: Implement Function Body**
```cpp
double larger(double x, double y) {
    double max;
    if (x >= y)
        max = x;
    else
        max = y;
    return max;  // Returns computed value
}
```

### Function Definition Syntax
```cpp
type_ret func_name(type1 par1, type2 par2, ...) {
    // Variable declarations
    ...
    // Executable statements
    ...
}
```

**Components**:
- **`type_ret`**: Return type (data type of the result)
- **`func_name`**: Function name (identifier)
- **`type1 par1, type2 par2`**: Parameter list (formal parameters)
- **`{}`**: Function body

### Void Functions

**Definition**: Functions that perform operations but return NO value

```cpp
void print_msg(int x) {
    cout << "This is a void function " << x << endl;
    return;  // Optional - just returns control
}

void print_msg(int x) {
    cout << "This is a void function " << x << endl;
    // No return statement needed
}
```

**Key Points**:
- Use `void` as return type
- `return;` statement (without value) is optional
- Function returns to caller after last statement if no explicit return

### Function Call (Invocation)

**Syntax**: `function_name(argument1, argument2, ...);`

**Example**:
```cpp
double z = larger(2.5, 5.0);
      ↑      ↑        ↑      ↑
   stores  function   arguments (actual parameters)
   return value  name
```

#### Arguments vs. Parameters
- **Formal Parameters**: Placeholders in function definition (e.g., `x`, `y`)
- **Actual Parameters/Arguments**: Actual values passed in function call (e.g., `2.5`, `5.0`)

#### Types of Arguments
A function call can use:
1. **Constants**: `larger(2.5, 5.0);`
2. **Variables**: `larger(one, two);`
3. **Expressions**: `larger(one - 2, two);`
4. **Other function calls**: `larger(2.5, larger(3, 5.0));`

Expressions and nested function calls are evaluated BEFORE the function call.

### Function Declaration (Forward Declaration)

**Problem**: Compiler needs to know function prototype BEFORE it's called

**Solution 1**: Define function before calling it
```cpp
#include <iostream>
using namespace std;

double larger(double x, double y) {
    if (x >= y)
        return x;
    else
        return y;
}

int main() {
    double c = larger(a, b);
    return 0;
}
```

**Solution 2**: Declare function, define it later
```cpp
#include <iostream>
using namespace std;

double larger(double x, double y);  // Declaration ends with ;

int main() {
    double c = larger(a, b);
    return 0;
}

double larger(double x, double y) {  // Definition
    if (x >= y)
        return x;
    else
        return y;
}
```

#### Function Declaration Syntax
```cpp
type_ret func_name(type1 par1, type2 par2, ...);
//                                           ↑
//                                      semicolon required

// Parameter names can be omitted in declaration
type_ret func_name(type1, type2, ...);
```

**Key Difference from Definition**:
- Declaration: Ends with `;`
- Definition: Ends with `{}` containing function body
- Declaration parameter names are optional (just for documentation)

---

## Flow of Control {#flow-of-control}

### Execution Sequence

1. **Program starts**: Always begins at `main()` function
2. **Sequential execution**: Statements in `main()` execute top to bottom
3. **Function call**: When encountered, main() suspends, calls function
4. **Function execution**: Called function executes its statements top to bottom
5. **Return**: `return` statement terminates function, passes control + value back
6. **Resume**: `main()` continues from the calling statement

### Step-by-Step Example

```cpp
#include <iostream>
using namespace std;

double larger(double x, double y) {
    if (x >= y)
        return x;
    else
        return y;
}

int main() {
    double a = 2.5, b = 5.0, c;
    c = larger(a, b);  // Function call
    cout << c << " is larger." << endl;
    return 0;
}
```

**Execution Flow**:

| Step | Location | Action |
|------|----------|--------|
| 1 | `main()` line 11 | Declare variables: a=2.5, b=5.0, c=? |
| 2 | `main()` line 12 | Encounter function call |
|  | → | Arguments copied: x=2.5, y=5.0 |
| 3 | `larger()` | Execute: x >= y? (2.5 >= 5.0? NO) |
| 4 | `larger()` | Execute else: return y (5.0) |
| 5 | `larger()` | Terminate and return 5.0 to caller |
| 6 | `main()` line 12 | c = 5.0 |
| 7 | `main()` line 13 | Execute cout: output "5 is larger." |
| 8 | `main()` line 14 | `return 0` → program ends |

---

## Scope of Variables {#scope-of-variables}

### Local Variables

**Definition**: Variables declared within a function are private/local to that function.

**Properties**:
- No other function can access them directly
- Created when function is called
- Destroyed when function exits
- Do NOT retain values between function calls
- Must be explicitly set upon each function entry

**Key Rule**: Local variables from different functions can have the same name (they're independent)

#### Example: Local Variable with Same Name
```cpp
#include <iostream>
using namespace std;

double larger(double x, double y) {
    double max;
    max = (x >= y) ? x : y;  // max is local to larger()
    return max;
}

int main() {
    double a = 2.5, b = 5.0, max;  // Different max!
    max = larger(a, b);
    cout << max << " is larger." << endl;
    return 0;
}
```

**Important**: `max` in `larger()` and `max` in `main()` are COMPLETELY SEPARATE variables.

### Global Variables

**Definition**: Variables declared outside ALL functions, accessible by all functions.

**Properties**:
- Visible/accessible to all functions
- Remain in existence permanently
- Retain values even after functions exit
- Can be modified by ANY function

**Syntax**:
```cpp
#include <iostream>
using namespace std;

double a, b;  // Global variables
const double PI = 3.1415;  // Global constant

double larger() {
    return (a >= b) ? a : b;
}

int main() {
    cout << "Input two numbers: ";
    cin >> a >> b;
    cout << larger() << " is larger." << endl;
    return 0;
}
```

### Scope Rules

**Scope**: The portion of a program where a variable is defined and usable.

**Rules**:
1. A variable's scope starts at its declaration and extends to the end of its block
2. A block is delimited by braces `{}`
3. Variables in outer blocks can be referred to in inner blocks
4. Variables can share the same name if in different scopes
5. Inner block variables **HIDE** identically named outer block variables

#### Detailed Scope Example
```cpp
double a;  // Scope of global a: entire file

int func(int x, int y) {
    // Scope of x, y: entire function
    
    if (x > y) {
        int k;  // Scope of k: only within if block
        ...
    }
    
    int z;  // Scope of z: entire function func()
    ...
}

double b;  // Scope of global b: from declaration to end of file

int main() {
    int x, y, z;  // Scope: entire main()
    
    if (...) {
        int x;  // This x HIDES the outer x (inner scope only)
        ...
    }  // End of if block - inner x destroyed
    
    ...
}
```

#### Practical Example: Variable Shadowing
```cpp
#include <iostream>
using namespace std;

int main() {
    int i = 0;
    cout << "Outer block: i = " << i << endl;  // Output: 0
    
    {
        int i = 100;  // Inner i shadows outer i
        cout << "Inner block: i = " << i << endl;  // Output: 100
    }  // Inner i goes out of scope
    
    cout << "Outer block: i = " << i << endl;  // Output: 0
    return 0;
}

// Output:
// Outer block: i = 0
// Inner block: i = 100
// Outer block: i = 0
```

---

## Parameter Passing Mechanism {#parameter-passing}

### Pass-by-Value

**Definition**: Copies the value of the argument to the formal parameter. The formal parameter is a local variable of the function.

**Key Characteristics**:
1. Argument values are COPIED to formal parameters
2. Any changes to formal parameters are LOCAL to the function
3. Changes do NOT affect the original arguments
4. Formal parameters disappear when function exits
5. Only the return value comes back to the caller

#### Example: Why Pass-by-Value Doesn't Work for Swapping
```cpp
#include <iostream>
using namespace std;

void swap(int a, int b) {
    cout << "a = " << a << ", b = " << b << endl;
    int temp = a;
    a = b;
    b = temp;
    cout << "a = " << a << ", b = " << b << endl;
}

int main() {
    int x = 0, y = 100;
    cout << "x = " << x << ", y = " << y << endl;
    swap(x, y);
    cout << "x = " << x << ", y = " << y << endl;
    return 0;
}

// Output:
// x = 0, y = 100
// a = 0, b = 100
// a = 100, b = 0        <- Only local variables changed!
// x = 0, y = 100        <- Original variables unchanged!
```

**Why it fails**: Values are copied; changes to `a` and `b` don't affect `x` and `y`.

#### Visualization: Pass-by-Value
```
Caller's main():          Function's swap():
┌─────────────┐           ┌─────────────┐
│ x = 0       │  copy →   │ a = 0       │
│ y = 100     │  copy →   │ b = 100     │
└─────────────┘           │             │
     ↓                    │ temp = a    │
  (unchanged)            │ a = b       │
                         │ b = temp    │
                         │ a = 100     │
                         │ b = 0       │
                         └─────────────┘
                         (deleted when
                          function exits)
```

### Pass-by-Reference

**Definition**: Formal parameter refers to the SAME memory location as the argument, not a copy.

**Key Characteristics**:
1. Formal parameter and argument occupy the SAME memory cell
2. Any changes to formal parameter are reflected in the argument
3. Arguments MUST be variables (not constants or expressions)
4. Changes persist after function returns

**Syntax**: Use ampersand `&` in the parameter declaration
```cpp
void func(type &par) {
    // par refers to the same memory as the argument
}
```

#### Example: Successful Swap with Pass-by-Reference
```cpp
#include <iostream>
using namespace std;

void swap(int &a, int &b) {
    cout << "a = " << a << ", b = " << b << endl;
    int temp = a;
    a = b;
    b = temp;
    cout << "a = " << a << ", b = " << b << endl;
}

int main() {
    int x = 0, y = 100;
    cout << "x = " << x << ", y = " << y << endl;
    swap(x, y);
    cout << "x = " << x << ", y = " << y << endl;
    return 0;
}

// Output:
// x = 0, y = 100
// a = 0, b = 100
// a = 100, b = 0
// x = 100, y = 0        <- Original variables ARE changed!
```

#### Visualization: Pass-by-Reference
```
Caller's main():          Function's swap():
┌─────────────┐           
│ x = 0  ─────┼──→ &a points to same location
│ y = 100────┼──→ &b points to same location
└─────────────┘           
     ↓                    
x = 100, y = 0            
(CHANGED via              
 references)              
```

#### Example: Pass-by-Reference for Square Root
```cpp
void square(int &x) {
    x *= x;  // Modifies the actual argument
}

int main() {
    int a = 10;
    cout << a << " squared: ";
    square(a);
    cout << a << endl;  // Output: 100
    return 0;
}

// Output: 10 squared: 100
```

### Pass-by-Reference vs. Value-Returning Functions

Both can achieve similar results but with different approaches:

```cpp
// Approach 1: Value-Returning Function
int squareByValue(int number) {
    return number *= number;  // Return the result
}

// Usage:
int x = 2;
x = squareByValue(x);  // Must assign return value

// Approach 2: Pass-by-Reference
void squareByReference(int &number) {
    number *= number;  // Modify argument directly
}

// Usage:
int z = 4;
squareByReference(z);  // No assignment needed
```

#### Example: Complete Comparison
```cpp
int squareByValue(int);
void squareByReference(int &);

int main() {
    int x = 2;
    int z = 4;
    
    cout << "x = " << x << " before squareByValue\n";
    cout << "Value returned by squareByValue: " 
         << squareByValue(x) << endl;
    cout << "x = " << x << " after squareByValue\n\n";
    
    cout << "z = " << z << " before squareByReference" << endl;
    squareByReference(z);
    cout << "z = " << z << " after squareByReference" << endl;
    return 0;
}

int squareByValue(int number) {
    return number *= number;
}

void squareByReference(int &number) {
    number *= number;
}

// Output:
// x = 2 before squareByValue
// Value returned by squareByValue: 4
// x = 2 after squareByValue           <- Not modified
//
// z = 4 before squareByReference
// z = 16 after squareByReference      <- Modified directly
```

### Functions Returning Multiple Values

**Challenge**: Function can only return ONE value via `return` statement

**Solution**: Use `void` function with reference parameters
```cpp
const double CONVERSION = 2.54;
const int INCHES_IN_FOOT = 12;
const int CENTIMETERS_IN_METER = 100;

void metersAndCentTofeetAndInches(int mt, int ct, int &f, int &in) {
    int centimeters;
    centimeters = mt * CENTIMETERS_IN_METER + ct;
    in = (int)(centimeters / CONVERSION);
    f = in / INCHES_IN_FOOT;
    in = in % INCHES_IN_FOOT;
}

// Usage:
int feet, inches;
metersAndCentTofeetAndInches(1, 50, feet, inches);
// Results stored in feet and inches
```

**Good Programming Practice**: 
- Use pass-by-value for input parameters (read-only)
- Use pass-by-reference for output parameters (return multiple values)

### Quick Exercise & Answer

**Exercise**: What's the output?
```cpp
#include <iostream>
using namespace std;

void figureMeOut(int &x, int y, int &z) {
    cout << x << ' ' << y << ' ' << z << endl;
    x = 1;
    y = 2;
    z = 3;
    cout << x << ' ' << y << ' ' << z << endl;
}

int main() {
    int a = 10, b = 20, c = 30;
    figureMeOut(a, b, c);
    cout << a << ' ' << b << ' ' << c << endl;
}
```

**Answer**:
```
10 20 30          // First cout in figureMeOut
1 2 3             // Second cout in figureMeOut
1 20 3            // cout in main (a and c modified by references!)
```

**Explanation**:
- `x` is pass-by-reference → changes to `x` affect `a`
- `y` is pass-by-value → changes to `y` don't affect `b`
- `z` is pass-by-reference → changes to `z` affect `c`

---

## Summary of Key Concepts

| Concept | Key Points |
|---------|-----------|
| **Top-Down Design** | Break problems into smaller sub-tasks; implement each as a function |
| **Functions** | Reusable blocks of code that perform specific tasks |
| **Predefined Functions** | Use `#include <header>`, check documentation for parameters/returns |
| **Function Definition** | `return_type name(param1, param2) { body }` |
| **Function Declaration** | `return_type name(param1, param2);` (forward declaration) |
| **Function Call** | `name(arg1, arg2);` with arguments |
| **Local Variables** | Exist only within function; different functions can use same names |
| **Global Variables** | Exist entire program; accessible to all functions (use carefully!) |
| **Variable Scope** | Region where variable is defined; inner scopes shadow outer scopes |
| **Pass-by-Value** | Copy values; original arguments unchanged; good for input parameters |
| **Pass-by-Reference** | Reference same memory; changes affect arguments; good for outputs/multiple returns |

---

## Best Practices

✅ **DO**:
- Use functions to organize code into logical modules
- Use pass-by-value for input parameters
- Use pass-by-reference for output parameters or multiple return values
- Include function declarations before `main()` for clarity
- Document function purpose and parameters
- Use descriptive function names

❌ **DON'T**:
- Use global variables for communication between functions
- Forget to include necessary header files
- Mix pass-by-value and pass-by-reference without clear documentation
- Create functions that do multiple unrelated tasks
- Reuse variable names in inner scopes without good reason

---

This comprehensive summary covers all essential material from Module 5 on Functions, including practical examples and best practices for C++ programming!