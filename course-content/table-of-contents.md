# Course Content Index

Course materials for PFC710 - Summer 2026

## Chapter 1
- [Chapter One: Introduction](chapter01%20-%20Introduction/chapter01.md)
  - Hardware, CPU, memory, RAM vs persistent storage (SSD, USB, cloud-synced files)
  - Algorithms, pseudocode, and planning before code
  - Python, the interpreter, bytecode, and the virtual machine (no separate compile step)
  - Programming environments (for example PyCharm), first program, `print`, strings, functions
  - Errors: syntax, run-time exceptions (tracebacks), and logic; optional Zen of Python and official tutorial

## Chapter 2
- [Chapter Two: Programming with Numbers and Strings](chapter02%20-%20Programming%20with%20Numbers%20and%20Strings/chapter02.md)
  - Variables, assignment, constants, comments, and naming conventions
  - Arithmetic operators, operator precedence, floor division (`//`) and modulus (`%`)
  - The `math` module, built-in numeric functions, and type conversions (`int`, `float`, `str`)
  - Problem solving: working an example by hand before coding it
  - Strings: length, concatenation, repetition, indexing, slicing, and useful methods
  - Reading input with `input()` and formatting output with f-strings

## Chapter 3
- [Chapter Three: Decisions and Relational Operators](chapter03%20-%20Decisions%20and%20Relational%20Operators/chapter03.md)
  - The `if` statement, compound blocks, `pass`, and flowcharts
  - Relational operators (`==`, `!=`, `<`, `>`, `<=`, `>=`); comparing numbers and strings (lexicographic order)
  - Nested `if` statements and multiway branching with `elif`
  - Boolean values, truthy/falsy conditions, and operators (`and`, `or`, `not`) with precedence
  - String analysis: `in`, `not in`, and methods such as `startswith`, `endswith`, `find`, `count`, `isdigit`, `isalpha`
  - Input validation and guarding against invalid user data

## Chapter 4
- [Chapter Four: Loops](chapter04%20-%20Loops/chapter04.md)
  - The `while` loop: count-controlled and event-controlled loops, `+=`, `break`, `continue`, and `while True`
  - Hand-tracing loops to predict behavior and find logic errors (for example sum of digits with `%` and `//`)
  - Sentinel values, priming and modification reads, and boolean flag variables
  - Common loop algorithms: sum, average, counting matches, min/max, and input validation
  - The `for` loop over strings and `range`
  - Nested loops (for example a multiplication table) and string processing with loops

## Chapter 5
- [Chapter Five: Functions](chapter05%20-%20Functions/chapter05.md)
  - Functions as black boxes: arguments, return values, and specifications
  - Defining and testing functions, `main()`, and definition order
  - Parameter passing, default parameter values, keyword arguments, return values (including returning multiple values), and functions without return values
  - Variable scope: local, global, and preferring parameters over globals

## Chapter 6
- [Chapter Six: Lists](chapter06%20-%20Lists/chapter06.md)
  - List basics: creation, indexing, mutability, length, iteration, references, aliases, copying, and negative indices
  - List operations: `append`, `insert`, `pop`, `remove`, concatenation, replication, equality, `sum`/`max`/`min`, `sort`, and slicing
  - Common list algorithms: filling a list, combining elements, linear search, collecting and counting matches, removing matches, swapping, and reading input
  - Lists with functions: passing lists as arguments, mutating lists in place, parameter passing, and returning lists
  - Tuples: immutability, use cases, returning multiple values, and operations that work or do not work compared to lists

## Chapter 7
- [Chapter Seven: Files and Exceptions](chapter07%20-%20Files%20and%20Exceptions/chapter07.md)
  - Opening, reading, writing, and closing text files; modes `"r"`, `"w"`, and `"a"`
  - The `with` statement for safe file handling
  - Processing text by line, word, and character; `rstrip()`, `split()`, and `rsplit()`
  - Reading structured records from files
  - Command-line arguments with `sys.argv`
  - Raising and handling exceptions with `try`, `except`, and `finally`

## Chapter 8
- [Chapter Eight: Sets and Dictionaries](chapter08%20-%20Sets%20and%20Dictionaries/chapter08.md)
  - Sets: creation, membership, `add`/`discard`/`remove`/`clear`, subsets, union, intersection, and difference
  - Counting unique words with a set
  - Dictionaries: creation, `len`, membership, `get`, add/update, `pop`/`del`/`clear`, and traversal (`keys`, `values`, `items`)
  - Dictionaries as data records and reading structured file records
  - Complex structures (for example a dictionary of sets for a book index)
  - Modules: driver vs supplemental files and `if __name__ == "__main__"`

## Chapter 9
- [Chapter Nine: S3 and DynamoDB](chapter09%20-%20S3%20and%20DynamoDB/chapter09.md)
  - AWS Academy credentials in the Windows `.aws` folder (`credentials` and `config`)
  - Installing and using **boto3**
  - S3: `put_object`, `get_object`, JSON bodies, and unique keys with `uuid`
  - DynamoDB: tables, items, **partition key** only, `put_item`, and `get_item`
  - Reading bucket and table names from environment variables

## Special Topics
- [External Libraries](special-topics/external-libraries.md)
  - Third-party packages, PyPI, pip, `requirements.txt`, and the `requests` library
- [AWS Lambda](special-topics/lambda.md)
  - Serverless Python functions, handlers, environment variables, EventBridge scheduling, S3, packaging, and CloudWatch logging
- [Random Choices](special-topics/random.md)
  - The `random` module: `choice`, `randint`, and `random()` for probability-based branching
- [Timestamps](special-topics/timestamp.md)
  - The `datetime` module, `strftime` format codes, and using timestamps in file names and logs


---
