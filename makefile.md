# Comprehensive Makefile and Make Tool Summary

## Overview
This module covers the make tool, separate compilation, and how to use Makefiles to manage compilation dependencies in C++ projects. The make tool smartly recompiles and links files only when necessary, avoiding time-consuming unnecessary recompilation.

---

## 1. Separate Compilation

### 1.1 Why Separate Compilation?

**Key Benefits:**
- **Code Organization:** Separate features from the main program for better readability and organization
- **Code Reusability:** Allow other programs to reuse functions without duplicating code
- **Faster Rebuilds:** When only a few source files are modified, only those files need recompilation
- **Implementation Hiding:** Provide class implementations as object code without exposing source files

### 1.2 Multiple Source Files - Example

**Problem:** A monolithic GCD program

```cpp
// gcd_single.cpp
#include <iostream>
using namespace std;

int gcd(int a, int b);

int main() {
  int a, b, c;
  cout << "Please input two positive numbers: ";
  cin >> a >> b;
  c = gcd(a, b);
  cout << "GCD is " << c << endl;
}

int gcd(int a, int b) {
  while(a != b) {
    if(a > b) {
      a -= b;
    } else {
      b -= a;
    }
  }
  return a;
}
```

**Compile and run:**
```bash
$ g++ gcd_single.cpp -o gcd_single
$ ./gcd_single
Please input two positive numbers: 18 24
GCD is 6
```

### 1.3 Separating into Multiple Files

**Separate out the GCD function into its own file:**

```cpp
// gcd.cpp
#include <iostream>
#include "gcd.h"
using namespace std;

int gcd(int a, int b) {
  while(a != b) {
    if(a > b) {
      a -= b;
    } else {
      b -= a;
    }
  }
  return a;
}
```

**Create a header file for function declaration:**

```cpp
// gcd.h
#ifndef GCD_H
#define GCD_H

int gcd(int a, int b);

#endif
```

**Include guard explanation:**
- `#ifndef GCD_H` — Check if `GCD_H` is not defined
- `#define GCD_H` — Define the macro
- `#endif` — End of guard
- **Purpose:** Prevents the same file from being included multiple times, avoiding redeclaration errors

**Main program using the header:**

```cpp
// gcd_main.cpp
#include <iostream>
#include "gcd.h"
using namespace std;

int main() {
  int a, b, c;
  cout << "Please input two positive numbers: ";
  cin >> a >> b;
  c = gcd(a, b);
  cout << "GCD is " << c << endl;
}
```

**Compile together:**
```bash
$ g++ gcd_main.cpp gcd.cpp -o gcd
```

---

## 2. The Compilation Process

### Four Stages of Compilation

1. **Pre-processing:** Process preprocessor directives (lines starting with `#`) to generate expanded source code
2. **Compilation:** Compile expanded source code to assembly code (machine-specific)
3. **Assembly:** Convert assembly code into machine code, producing object code files (`.o`)
4. **Linker:** Link object codes with libraries to create the final executable

### Multiple File Compilation Flow

`````````
gcd.cpp  ─────┐
              ├──> Pre-process ──> gcd.i
              ├──> Compilation ──> gcd.s
              ├──> Assembly ────> gcd.o
                                    │
                                    └──> Linker ──> gcd (executable)
                                    │
gcd_main.cpp ─┐                     │
              ├──> Pre-process ──> gcd_main.i
              ├──> Compilation ──> gcd_main.s
              ├──> Assembly ────> gcd_main.o
                                    │
```

---

## 3. Separate Compilation in Practice

### Compiling Separately to Object Code

Use the `-c` flag to compile to object code without linking:

```bash
$ g++ -c gcd.cpp
$ g++ -c gcd_main.cpp
```

This generates:
- `gcd.o` (object code from gcd.cpp)
- `gcd_main.o` (object code from gcd_main.cpp)

### Linking Object Code

```bash
$ g++ gcd.o gcd_main.o -o gcd
```

### Advantages of Separate Compilation

1. **Independent development:** Programmers can write and compile files separately
2. **Faster rebuilds:** Only recompile modified files; link existing object code
3. **Implementation privacy:** Distribute object code without source files
4. **Example:** Recompiling Linux OS takes 8 hours; recompiling only modified files takes much less

---

## 4. File Dependencies

In large projects with many source files, dependencies form a dependency graph:

```
lcm.h ──┐
lcm.cpp ┤──> lcm.o ──┐
                     ├──> calc (executable)
gcd.h ──┐            │
gcd.cpp ┤──> gcd.o ──┤
                     │
calc.cpp ────────> calc.o
```

**When `calc.cpp` is modified:**
- Recompile `calc.cpp` → `calc.o`
- **DO NOT** recompile `gcd.cpp` or `lcm.cpp` (unchanged dependencies)
- Link: `calc.o + gcd.o + lcm.o` → `calc`

**Need:** A tool to manage these dependencies and avoid unnecessary recompilation

---

## 5. The Make Tool

### 5.1 What is Make?

The `make` tool:
- Automatically recompiles and links files when source files change
- Avoids unnecessary recompilation by tracking dependency modification times
- Reads instructions from a file called **Makefile** (or makefile)
- Only regenerates files that are outdated (older than their dependencies)

### 5.2 Makefile Structure

A **Makefile** contains rules with three parts:

**Syntax:**
```makefile
target: dependency1 dependency2 ...
	command1
	command2
```

**Important:** Each command line must start with a **TAB character** (not spaces).

### 5.3 Rule Components

| Component | Description |
|-----------|-------------|
| **Target** | File to be generated (followed by `:`) |
| **Dependencies** | Files that target depends on |
| **Commands** | Instructions to generate the target (must start with TAB) |

### 5.4 Example Makefile

```makefile
# This file must be named Makefile
# Comments start with #

gcd.o: gcd.cpp gcd.h
	g++ -c gcd.cpp

gcd_main.o: gcd_main.cpp gcd.h
	g++ -c gcd_main.cpp

gcd_main: gcd.o gcd_main.o
	g++ gcd.o gcd_main.o -o gcd_main
```

---

## 6. How Make Works

### Execution Flow

When you run `make gcd_main`, the make tool:

1. **Check dependency recursively:** Look at the dependency list of the target. If any dependency doesn't exist or is older than its own dependencies, recursively make those files first.

2. **Check if target is up-to-date:** Compare modification time of target with its dependencies.
   - If target doesn't exist → execute commands
   - If any dependency is newer than target → execute commands
   - If target is newer than all dependencies → **skip** (already up-to-date)

3. **Execute minimum commands:** Only recompile what's necessary

### Example Execution

**Initial compilation:**
```bash
$ make gcd_main
g++ -c gcd.cpp
g++ -c gcd_main.cpp
g++ gcd.o gcd_main.o -o gcd_main
```

**After modifying `gcd_main.cpp` only:**
```bash
$ make gcd_main
g++ -c gcd_main.cpp
g++ gcd.o gcd_main.o -o gcd_main
```

Make detects that:
- ✅ `gcd.o` is up-to-date (gcd.cpp and gcd.h unchanged)
- ❌ `gcd_main.o` is **outdated** (gcd_main.cpp is newer)
- ❌ `gcd_main` is **outdated** (depends on gcd_main.o)

Result: Only `gcd_main.cpp` is recompiled, then linking occurs.

---

## 7. Makefile Tricks and Advanced Features

### 7.1 Variables

**Define and use variables to reduce repetition:**

```makefile
TARGET = gcd_main
OBJECTS = gcd.o gcd_main.o

$(TARGET): $(OBJECTS)
	g++ $(OBJECTS) -o $(TARGET)
```

**Equivalent to:**
```makefile
gcd_main: gcd.o gcd_main.o
	g++ gcd.o gcd_main.o -o gcd_main
```

**Syntax:** Use `$(variable_name)` to retrieve the value

### 7.2 Special Variables

Make automatically defines three special variables:

| Variable | Description | Example Usage |
|----------|-------------|----------------|
| `$@` | The target name | `g++ $^ -o $@` → `g++ gcd.o gcd_main.o -o gcd_main` |
| `$^` | All dependencies | `g++ -c $<` → `g++ -c gcd_main.cpp` |
| `$<` | Leftmost dependency | Used when only first dependency is needed |

**Example with special variables:**

```makefile
# Without special variables
gcd_main.o: gcd_main.cpp gcd.h
	g++ -c gcd_main.cpp

gcd_main: gcd.o gcd_main.o
	g++ gcd.o gcd_main.o -o gcd_main

# With special variables (equivalent)
gcd_main.o: gcd_main.cpp gcd.h
	g++ -c $<

gcd_main: gcd.o gcd_main.o
	g++ $^ -o $@
```

### 7.3 Phony Targets

**Phony targets** are "fake" targets that don't generate actual files. They execute commands when invoked.

**Example use cases:** `clean`, `tar`, `install`

```makefile
clean:
	rm -f gcd_main gcd.o gcd_main.o gcd.tgz

tar:
	tar -czvf gcd.tgz *.cpp *.h

.PHONY: clean tar
```

**Usage:**
```bash
$ make clean      # Executes: rm -f gcd_main gcd.o gcd_main.o gcd.tgz
$ make tar        # Executes: tar -czvf gcd.tgz *.cpp *.h
```

**Important:** Declare phony targets with `.PHONY:` to force execution even if a file with that name exists

### 7.4 Realistic Complete Makefile

```makefile
FLAGS = -pedantic -errors -std=c++11

gcd.o: gcd.cpp gcd.h
	g++ $(FLAGS) -c $<

gcd_main.o: gcd_main.cpp gcd.h
	g++ $(FLAGS) -c $<

gcd_main: gcd.o gcd_main.o
	g++ $(FLAGS) $^ -o $@

clean:
	rm -f gcd_main gcd.o gcd_main.o gcd.tgz

tar:
	tar -czvf gcd.tgz *.cpp *.h

.PHONY: clean tar
```

### 7.5 Complete Sample Workflow

```bash
$ ls
Makefile  gcd.cpp  gcd.h  gcd_main.cpp

$ make gcd_main
g++ -pedantic -errors -std=c++11 -c gcd.cpp
g++ -pedantic -errors -std=c++11 -c gcd_main.cpp
g++ -pedantic -errors -std=c++11 gcd.o gcd_main.o -o gcd_main

$ ls
Makefile  gcd_main  gcd.cpp  gcd.h  gcd.o  gcd_main.cpp  gcd_main.o

$ make tar
tar -czvf gcd.tgz *.cpp *.h
gcd.cpp
gcd_main.cpp
gcd.h

$ ls
Makefile  gcd_main  gcd.cpp  gcd.h  gcd.o  gcd.tgz  gcd_main.cpp  gcd_main.o

$ make clean
rm -f gcd_main gcd.o gcd_main.o gcd.tgz

$ ls
Makefile  gcd.cpp  gcd.h  gcd_main.cpp
```

---

## 8. Key Concepts Summary

| Concept | Key Point |
|---------|-----------|
| **Separate Compilation** | Compile each source file to object code independently, then link |
| **Object Code** | Intermediate machine code (`.o` files) from compilation stage |
| **Make Tool** | Automatically manages dependencies and avoids unnecessary recompilation |
| **Makefile** | Configuration file with rules defining targets, dependencies, and build commands |
| **Up-to-date** | A file is up-to-date if it's newer than all its dependencies |
| **Include Guard** | `#ifndef`, `#define`, `#endif` to prevent multiple inclusions |
| **Phony Target** | Fake target that executes commands regardless of file state |
| **Variables** | Reusable values (e.g., `TARGET`, `OBJECTS`) accessed via `$(name)` |
| **Special Variables** | `$@` (target), `$^` (all deps), `$<` (first dep) |

---

## 9. Quick Reference

### Common Make Commands

```bash
make target        # Build specific target
make               # Build first target in Makefile
make clean         # Clean up (if defined)
make tar           # Create archive (if defined)
```

### Makefile Template

```makefile
# Compiler flags
FLAGS = -pedantic -errors -std=c++11

# Compilation rules
filename.o: filename.cpp header.h
	g++ $(FLAGS) -c $<

# Linking rule
executable: file1.o file2.o
	g++ $(FLAGS) $^ -o $@

# Utility targets
clean:
	rm -f executable *.o *.tgz

.PHONY: clean
```

---

## 10. Additional Resources

- [Introduction to Makefiles by Johannes Franken](http://www.jfranken.de/homepages/johannes/vortraege/make_inhalt.en.html)
- [A Simple Makefile Tutorial](http://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/)
- [Tutorial on Writing Makefiles](http://makepp.sourceforge.net/1.19/makepp_tutorial.html)

---

This comprehensive guide covers all essential aspects of the make tool and separate compilation. You now understand how separate compilation improves code organization and build times, and how makefiles automate the dependency management and build process.