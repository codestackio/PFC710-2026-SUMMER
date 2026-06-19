# Chapter 7: Files and Exceptions

---

## Introduction

This chapter shows how to write programs that read and write text files, process file data, use command-line arguments, and raise and handle exceptions. You will learn to work with files safely and to structure programs that deal with real-world data and error conditions.

---

## Chapter Goals

In this chapter you will learn:

- To read and write text files
- To process collections of data from files
- To process command-line arguments
- To raise and handle exceptions

---

[← Back to Course Index](../table-of-contents.md)

## Contents

- [7.1 Reading and Writing Text Files](#71-reading-and-writing-text-files)
- [7.2 Text Input and Output](#72-text-input-and-output)
- [7.3 Command-Line Arguments](#73-command-line-arguments)
- [7.4 Exception Handling](#74-exception-handling)

---

## 7.1 Reading and Writing Text Files

### Why Text Files?

Text files are very commonly used to store information. They are among the most portable types of data files. Examples include files created with a simple text editor (e.g., Notepad), as well as Python source code and HTML files.

### Opening Files for Reading

To access a file, you must first **open** it. To read from a file named `input.txt` in the same directory as your program, call `open` with the file name and the mode `"r"`:

```python
infile = open("input.txt", "r")
```

**Important points:**

- When opening a file for reading, the file must exist (and be accessible) or an exception is raised.
- Store the file object returned by `open` in a variable.
- All access to the file is done through that file object.

### Opening Files for Writing (Overwrite)

To open a file for writing, use mode `"w"`:

```python
outfile = open("output.txt", "w")
```

- If the file already exists, it is emptied before new data is written.
- If the file does not exist, a new empty file is created.

### Opening Files for Appending

To add data to the end of an existing file, use mode `"a"`:

```python
outfile = open("output.txt", "a")
```

- If the file already exists, new data is appended at the end.
- If the file does not exist, a new empty file is created.

### Closing Files

When you are done with a file, close it using the **`close()`** method:

```python
infile.close()
outfile.close()
```

If the program exits without closing a file that was opened for writing, some output may not be written to disk.

### The with Statement

Python’s **`with`** statement opens a file and automatically closes it when the block ends, even if an exception occurs:

```python
with open("input.txt", "r") as infile:
    line = infile.readline()
    # Process the file...
# infile is closed here
```

Use `with` whenever you can. It is safer than remembering to call `close()` yourself. The examples in this chapter use `with` for file handling.

### Syntax: Opening and Closing Files

The diagram below shows the underlying pattern: open files, read or write data, then close them.

![](media/image2.jpeg "Opening and closing files syntax")

### Reading from a File

To read one line of text, use the **`readline()`** method on the file object:

```python
line = infile.readline()
```

When a file is opened, an internal position (sometimes called an input marker) is at the start of the file. `readline()` reads from that position until the end of the line, then advances the position to the next line.

**Example:** If `input.txt` contains:

```
flying
circus
```

- The first call to `readline()` returns `"flying\n"` (the `\n` is the newline character).
- The second call returns `"circus\n"`.
- A third call returns `""` when the end of the file is reached.
- A blank line in the file produces a string containing only `"\n"`.

### Reading Multiple Lines

Read and process lines until you get the sentinel value (empty string) that indicates end of file:

```python
line = infile.readline()
while line != "":
    # Process the line.
    line = infile.readline()
```

### Converting File Input to Numbers

`readline()` returns strings. If the file contains numbers, convert them with `int()` or `float()`:

```python
value = float(line)
```

The newline at the end of the line is ignored when the string is converted to a number.

### Writing to a File

Use the **`write()`** method. Unlike `print()`, you must include the newline character yourself:

```python
outfile.write("Hello, World!\n")
outfile.write(f"Number of entries: {count}\nTotal: {total:8.2f}\n")
```

### Example: File Reading and Writing

**Task:** Read a text file containing one floating-point value per line; write them to a new file in a column, then the total and average.

**Sample input file:**

```
32.0
54.0
67.5
80.25
115.0
```

**Sample output file:**

```
  32.00
  54.00
  67.50
  80.25
 115.00
--------
Total:   348.75
Average:  69.75
```

**Example code:**

```python
##
#  This program reads a file containing numbers and writes the numbers to
#  another file, lined up in a column and followed by their total and average.
#

input_file_name = input("Input file name: ")
output_file_name = input("Output file name: ")

total = 0.0
count = 0

with open(input_file_name, "r") as infile:
    with open(output_file_name, "w") as outfile:
        line = infile.readline()
        while line != "":
            value = float(line)
            outfile.write(f"{value:15.2f}\n")
            total = total + value
            count = count + 1
            line = infile.readline()

        if count == 0:
            print("Error: input file contains no data.")
        else:
            avg = total / count
            outfile.write(f"{'-'*15}\n")
            outfile.write(f"Total:   {total:8.2f}\n")
            outfile.write(f"Average: {avg:6.2f}\n")
```

### Common Error: Backslashes in File Paths

In a string literal for a Windows path, each backslash must be written twice (because a single backslash starts an escape sequence, e.g. `\n` for newline):

```python
infile = open("c:\\homework\\input.txt", "r")
```

When the user types a path (e.g. from `input()`), they do not double the backslashes.

---

## 7.2 Text Input and Output

### Overview

You will often need to process text by **word**, **line**, or **character**. Python provides methods such as `read()`, `split()`, `strip()`, and `rstrip()` for these tasks.

### Iterating Over Lines

You can treat an open file as a sequence of lines. This loop reads and prints every line:

```python
for line in infile:
    print(line)
```

Each time through the loop, `line` is the next line (including the newline). A file is not like a list: once you have read through it, you must close and reopen it to iterate again.

### Reading Words Example

You can read by line and then split into words. Example input and output:

- **Input:** `Mary had a little lamb`
- **Output:** one word per line: `Mary`, `had`, `a`, `little`, `lamb`

```python
with open("lyrics.txt", "r") as input_file:
    for line in input_file:
        line = line.rstrip()
        # ... process words in line
```

### Removing the Newline

Each line read from a file usually ends with `\n`. Remove it with **`rstrip()`**:

```python
line = line.rstrip()
```

![](media/image3.jpeg "String with newline before rstrip()")

![](media/image4.jpeg "String after rstrip()")

### Character Strip Methods

The string methods **`strip()`**, **`lstrip()`**, and **`rstrip()`** remove characters from the ends of a string:

- **`strip()`** — removes leading and trailing whitespace (or specified characters) from both ends
- **`lstrip()`** — removes from the left end only
- **`rstrip()`** — removes from the right end only

Pass a string of characters to remove, for example `line.rstrip(".,?!")`.

![](media/image5.png "Character strip methods")

### Character Strip Examples

![](media/image6.png "Character strip examples")

### Reading Words

To process a file word by word, read each line, strip the newline, then **split** the line into words:

```python
line = line.rstrip()
word_list = line.split()
```

`split()` returns a list of substrings separated by whitespace. If the last word has punctuation (e.g. `lamb,`), strip it with:

```python
word = word.rstrip(".,?!")
```

![](media/image7.jpeg "Line before split()")

![](media/image8.jpeg "Word list after split()")

### Reading Words: Complete Example

Suppose **lyrics.txt** contains:

```text
Mary had a little lamb,
whose fleece was white as snow.
```

Then the following code reads it word by word:

```python
with open("lyrics.txt", "r") as input_file:
    for line in input_file:
        line = line.rstrip()
        word_list = line.split()
        for word in word_list:
            word = word.rstrip(".,?!")
            print(word)
```


### Additional String Splitting Methods

- **`split()`** — splits on whitespace and returns a list of words
- **`split(sep)`** — splits on the given separator string (for example `":"`)
- **`rsplit(sep, maxsplit)`** — splits from the right, at most `maxsplit` times

![](media/image9.png "String splitting methods")

### Additional String Splitting Examples

![](media/image10.png "String splitting examples")

### Reading Characters

The **`read(n)`** method reads up to `n` characters and returns a string. With `n = 1` you read one character:

```python
char = input_file.read(1)
```

At end of file, `read()` returns `""`.

**Pattern for reading character by character:**

```python
char = input_file.read(1)
while char != "":
    # Process character
    char = input_file.read(1)
```

### Reading Records

A text file can store **records**, each with several **fields** (e.g. ID, name, address, year). In general you read a full record before processing it:

- For each record: read the entire record, then process it.

**Record format 1 — one field per line:**  
Each field on its own line, all fields of one record on consecutive lines:

```
China
1330044605
India
1147995898
United States
303824646
```

Read two lines per record:

```python
line = infile.readline()
while line != "":
    country_name = line.rstrip()
    line = infile.readline()
    population = int(line)
    # Process data record
    line = infile.readline()
```

**Record format 2 — one record per line with delimiter:**  
Fields separated by a delimiter (e.g. `:`):

```
China:1330044605
India:1147995898
United States:303824646
```

Read each line and split on the delimiter:

```python
for line in infile:
    line = line.rstrip()
    parts = line.split(":")
    country_name = parts[0]
    population = int(parts[1])
    # Process data record
```

**Record format 3 — one line, no delimiter:**  
e.g. `China 1330044605`, `United States 303824646`. You cannot split on spaces because country names can have multiple words. Use **`rsplit(" ", 1)`** to split only on the last space:

```python
input_string = "United States 303824646"
result = input_string.rsplit(" ", 1)
print(result)   # ['United States', '303824646']
```

Alternatively, find the first digit and slice the line:

```python
i = 0
while i < len(line) and not line[i].isdigit():
    i = i + 1
country_name = line[:i].rstrip()
population = int(line[i:])
```

![](media/image11.jpeg "Slicing a line into name and number")

### File Operations Summary

| Operation | Purpose |
|-----------|---------|
| `open(filename, mode)` | Open a file for reading (`"r"`), writing (`"w"`), or appending (`"a"`) |
| `readline()` | Read one line of text |
| `read(n)` | Read up to `n` characters |
| `read()` | Read the entire remaining contents |
| `write(text)` | Write a string to the file |
| `close()` | Close the file (automatic with `with`) |

![](media/image12.png "Summary of file operations")

---

## 7.3 Command-Line Arguments

### Running Programs

Programs can be run from an IDE (e.g. Pycharm) or from a terminal. When run from the terminal, you can pass **command-line arguments** after the program name. These are given to the program as strings.

### Using Command-Line Arguments

Text-based programs can be **parameterized** with command-line arguments. A typical invocation:

```text
python program.py -v input.dat
```

- `argv[0]`: `"program.py"`
- `argv[1]`: `"-v"`
- `argv[2]`: `"input.dat"`

Options (switches) often start with a dash. Python exposes the arguments in the **`sys.argv`** list:

```python
import sys

print("This is the name of the program:", sys.argv[0])
print("Argument list:", str(sys.argv))
```

### Reading a Filename from the Command Line

A common use of command-line arguments is to pass the name of an input file:

```python
import sys

if len(sys.argv) < 2:
    print("Usage: python program.py input.txt")
else:
    input_file_name = sys.argv[1]
    with open(input_file_name, "r") as infile:
        for line in infile:
            print(line.rstrip())
```

Run it from the terminal:

```text
python program.py scores.txt
```

Here `sys.argv[0]` is `"program.py"` and `sys.argv[1]` is `"scores.txt"`. Always check `len(sys.argv)` before accessing `sys.argv[1]` — if the user omits the filename, the program would otherwise raise an `IndexError`.

### When to Use Command-Line Arguments

In this course we often use an interactive interface. Command-line arguments are especially useful when you need to **automate** a program (e.g. from scripts or other tools).

---

## 7.4 Exception Handling

### Two Aspects of Errors

- **Detecting errors:** e.g. `open()` can detect that a file does not exist.
- **Handling errors:** The function that detects the error usually does not know how to fix it, so it **raises an exception** and lets another part of the program handle it. Exception handling passes control from the point of the error to a **handler** that can respond appropriately.

### Raising Exceptions

When a condition is wrong (e.g. withdrawal amount exceeds balance), you can **raise** an exception. Execution then jumps to an exception handler instead of continuing with the next statement:

```python
if amount > balance:
    raise ValueError("Amount exceeds balance")
```

### Exception Classes (Subset)

Common built-in exceptions include `ValueError`, `TypeError`, `RuntimeError`, `OSError`, `FileNotFoundError`, and others. Choose (or define) an exception that fits the error.

| Exception | Typical cause |
|-----------|---------------|
| `ValueError` | A value has the wrong form (for example `int("abc")`) |
| `TypeError` | An operation is applied to the wrong type |
| `RuntimeError` | A general run-time problem detected by your program |
| `OSError` | An operating-system error (for example file access) |
| `FileNotFoundError` | A file does not exist (subclass of `OSError`) |

![](media/image13.jpeg "Common exception classes")

### Syntax: Raising an Exception

Use the form `raise ExceptionType("message")` to signal an error:

```python
raise ValueError("Amount exceeds balance")
```

![](media/image14.jpeg "raise statement syntax")

### Handling Exceptions

Every exception should be handled somewhere. You can:

- Handle each possible exception type and react appropriately.
- For recoverable errors: exit the program, or ask the user to correct the input.

### try/except

Put code that might raise an exception in a **`try`** block, and the handler in an **`except`** clause:

```python
try:
    filename = input("Enter filename: ")
    with open(filename, "r") as infile:
        line = infile.readline()
        value = int(line)
    # ...
except OSError:
    print("Error: file not found or could not be opened.")
except ValueError as exception:
    print("Error:", str(exception))
```

- If the file cannot be opened, an `OSError` (or `FileNotFoundError`) is raised and the first `except` runs.
- If `int(line)` fails, a `ValueError` is raised and the second `except` runs.
- If any exception is raised in the `try` block, the rest of the `try` block is skipped.

**Note:** In Python 3, `open()` for a missing file raises `FileNotFoundError` (a subclass of `OSError`). Catching `OSError` will also catch `FileNotFoundError`.

### Example: Reading a File with Error Handling

The following program reads a data file whose first line contains a count, followed by that many floating-point values. If the file is missing or the format is wrong, the user can try again:

**Sample input file (`good.dat`):**

```text
3
10.5
20.0
15.5
```

```python
def main():
    done = False
    while not done:
        try:
            filename = input("Please enter the file name: ")
            data = read_file(filename)

            total = 0
            for value in data:
                total = total + value

            print("The sum is", total)
            done = True

        except OSError:
            print("Error: file not found.")

        except ValueError:
            print("Error: file contents invalid.")

        except RuntimeError as error:
            print("Error:", str(error))


def read_file(filename):
    with open(filename, "r") as infile:
        return read_data(infile)


def read_data(infile):
    line = infile.readline()
    number_of_values = int(line)
    data = []

    for i in range(number_of_values):
        line = infile.readline()
        value = float(line)
        data.append(value)

    line = infile.readline()
    if line != "":
        raise RuntimeError("End of file expected.")

    return data


main()
```

This example combines several ideas from this chapter: reading from a file, converting strings to numbers, raising exceptions when the format is wrong, and using a retry loop so the user can enter a different filename.

### Getting the Exception Message

Store the exception in a variable with **`as`** to print or use its message:

```python
except ValueError as exception:
    print("Error:", str(exception))
```

For `int("35x2")`, the message might be: `invalid literal for int() with base 10: '35x2'`.

### Custom Messages When Raising

When you raise an exception, you can pass a message:

```python
raise ValueError("Amount exceeds balance")
```

That string is the message associated with the exception object.

### The finally Clause

Use **`finally`** when you must run cleanup (e.g. closing a file) whether or not an exception occurred:

```python
outfile = open(filename, "w")
try:
    outfile.write("Hello, World!\n")
finally:
    outfile.close()
```

A **`try`** block may have one or more **`except`** clauses and an optional **`finally`** clause. The `finally` block always runs when the `try` block is exited.

![](media/image15.jpeg "try/except/finally syntax")

### Programming Tips

**Raise early.** If a function detects a problem it cannot fix, raise an exception rather than applying a bad fix.

**Catch late.** Only catch an exception where you can actually handle it. Otherwise, let it propagate to a caller that can.

**Avoid mixing `except` and `finally` in one `try`.** The `finally` block runs when the `try` is exited: by finishing normally, by an `except` handling an exception, or by an unhandled exception. If the file object might be `None` (e.g. `open` failed), calling `close()` in `finally` could raise another exception. Prefer nested `try` blocks:

```python
try:
    outfile = open(filename, "w")
    try:
        # Write output to outfile
    finally:
        outfile.close()
except OSError:
    # Handle exception
```

### The with Statement and Exceptions

Section 7.1 introduced the **`with`** statement for automatic file closing. It works well with exception handling — the file is closed when the block is left, whether or not an exception occurred:

```python
try:
    with open(filename, "r") as infile:
        line = infile.readline()
        value = int(line)
except OSError:
    print("Error: file not found.")
except ValueError as exception:
    print("Error:", str(exception))
```

When you use `with`, you usually do not need a separate `finally` block just to close the file.

---

## Summary

### File Input/Output

- Open a file with `open(filename, mode)` where mode is `"r"`, `"w"`, or `"a"`.
- Prefer `with open(...) as f:` so the file is closed automatically.
- Use `readline()` to read one line; use a loop or iterate with `for line in file` to read all lines.
- Write with `write()` (remember to add `\n` for new lines) or with `print(..., file=f)`.

### Processing Text Files

- Iterate over a file to read line by line.
- Use `rstrip()` to remove the newline (and optionally other characters).
- Use `split()` or `rsplit()` to break a line into words or fields.
- Use `read(n)` to read a fixed number of characters.

### Command-Line Arguments

- Use `sys.argv` to get the list of command-line arguments (including the script name in `argv[0]`).
- Check `len(sys.argv)` before accessing arguments beyond the program name.

### Exceptions

- Use **`raise`** to signal an error; execution continues in a matching **`except`** block.
- Put risky code in a **`try`** block and handlers in **`except`** clauses.
- Use **`finally`** for cleanup that must run whether or not an exception occurred.
- Combine **`with`** and **`try`/`except`** when reading or writing files that may fail.
- Raise as soon as you detect a problem; catch only where you can handle it. For each possible exception, decide which part of the program should handle it.
