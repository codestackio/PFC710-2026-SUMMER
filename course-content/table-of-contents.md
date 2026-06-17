# Course Content Index

Course materials for PFC710 - Summer 2026

## Chapter 1
- [Chapter One: Introduction](chapter01/chapter01.md)
  - Hardware, CPU, memory, RAM vs persistent storage (SSD, USB, cloud-synced files)
  - Algorithms, pseudocode, and planning before code
  - Python, the interpreter, bytecode, and the virtual machine (no separate compile step)
  - Programming environments (for example PyCharm), first program, `print`, strings, functions
  - Errors: syntax, run-time exceptions (tracebacks), and logic; optional Zen of Python and official tutorial

## Chapter 2
- [Chapter Two: Programming with Numbers and Strings](chapter02/chapter02.md)
  - Variables, assignment, constants, comments, and naming conventions
  - Arithmetic operators, operator precedence, floor division (`//`) and modulus (`%`)
  - The `math` module, built-in numeric functions, and type conversions (`int`, `float`, `str`)
  - Problem solving: working an example by hand before coding it
  - Strings: length, concatenation, repetition, indexing, slicing, and useful methods
  - Reading input with `input()` and formatting output with f-strings

## Chapter 3
- [Chapter Three: Decisions and Relational Operators](chapter03/chapter03.md)
  - The `if` statement, compound blocks, `pass`, and flowcharts
  - Relational operators (`==`, `!=`, `<`, `>`, `<=`, `>=`); comparing numbers and strings (lexicographic order)
  - Nested `if` statements and multiway branching with `elif`
  - Boolean values, truthy/falsy conditions, and operators (`and`, `or`, `not`) with precedence
  - String analysis: `in`, `not in`, and methods such as `startswith`, `endswith`, `find`, `count`, `isdigit`, `isalpha`
  - Input validation and guarding against invalid user data

## Chapter 4
- [Chapter Four: Loops](chapter04/chapter04.md)
  - The `while` loop: count-controlled and event-controlled loops, `+=`, `break`, `continue`, and `while True`
  - Hand-tracing loops to predict behavior and find logic errors (for example sum of digits with `%` and `//`)
  - Sentinel values, priming and modification reads, and boolean flag variables
  - Common loop algorithms: sum, average, counting matches, min/max, and input validation
  - The `for` loop over strings and `range`
  - Nested loops (for example a multiplication table) and string processing with loops

## Chapter 5
- [Chapter Five: Functions](chapter05/chapter05.md)
  - Functions as black boxes: arguments, return values, and specifications
  - Defining and testing functions, `main()`, and definition order
  - Parameter passing, default parameter values, keyword arguments, return values (including returning multiple values), and functions without return values
  - Variable scope: local, global, and preferring parameters over globals

## Chapter 6
- [Chapter Six: Lists](chapter06/chapter06.md)
  - List basics: creation, indexing, mutability, length, iteration, references, aliases, copying, and negative indices
  - List operations: `append`, `insert`, `pop`, `remove`, concatenation, replication, equality, `sum`/`max`/`min`, `sort`, and slicing
  - Common list algorithms: filling a list, combining elements, linear search, collecting and counting matches, removing matches, swapping, and reading input
  - Lists with functions: passing lists as arguments, mutating lists in place, parameter passing, and returning lists
  - Tuples: immutability, use cases, returning multiple values, and operations that work or do not work compared to lists

## Special Topics
- [External Libraries](special-topics/external-libraries.md)
  - Third-party packages, PyPI, pip, `requirements.txt`, and the `requests` library
- [Random Choices](special-topics/random.md)
  - The `random` module: `choice`, `randint`, and `random()` for probability-based branching
- [Timestamps](special-topics/timestamp.md)
  - The `datetime` module, `strftime` format codes, and using timestamps in file names and logs

---
