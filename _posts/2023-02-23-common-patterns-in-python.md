---
title: Common in python
author: subodh kamble 
By : subodh kamble
date: 2023-02-23 09:38:31
categories: [Tutorial]
tags: [Diy, Tutorial, pattern, python]
math: true
mermaid: true
---

### pattern 1

```terminal
|--| j1| j2| j3| j4| j5|
|i1| * | * | * | * | * |
|i2| * | * | * | * | * |
|i3| * | * | * | * | * |
|i4| * | * | * | * | * |
|i5| * | * | * | * | * |
```
```python
for i in range(1,6):
    for j in range(1,6):
        print("*",end="")
    print()
```

The outer loop `for i in range(1,6):` runs five times, each time assigning a new value to `i`. The inner loop `for j in range(1,6):` also runs five times, each time assigning a new value to `j`.

Within the inner loop, the `print("*", end="")` statement is executed five times, which prints an asterisk character (*) on the same line without adding a newline character (\n) at the end. The `end=""` parameter in the print statement specifies that the output should end with an empty string instead of a newline character.

After the inner loop has completed, the `print()` statement is executed, which prints a newline character and moves the cursor to the next line, resulting in a new line being printed for each iteration of the outer loop.

---

### pattern 2

```terminal
|--| j1| j2| j3| j4| j5|
|i1| 5 | 4 | 3 | 2 | 1 |
|i2| 5 | 4 | 3 | 2 | 1 |
|i3| 5 | 4 | 3 | 2 | 1 |
|i4| 5 | 4 | 3 | 2 | 1 |
|i5| 5 | 4 | 3 | 2 | 1 |
```

```python
for i in range(5,0,-1):
    for j in range(5,0,-1):
        print(j,end="")
    print()
```

### pattern 3

```terminal
|--| j1| j2| j3| j4| j5|
|i1| i | 2 | 3 | 4 | 5 |
|i2| 2 | 4 | 6 | 8 | 10|
|i3| 3 | 6 | 9 | 12| 15|
|i4| 4 | 8 | 12| 16| 20|
|i5| 5 | 10| 15| 20| 25|
```

```python
for i in range(5,0,-1):
    for j in range(5,0,-1):
        print(j,end="")
    print()
```

*more to updated...*