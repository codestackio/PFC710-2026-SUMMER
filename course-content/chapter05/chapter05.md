# Chapter Five: Functions

---

## Introduction

In this chapter, you will learn how to design and implement your own functions.

---

## Chapter Goals

In this chapter you will learn:

- To implement functions
- To become familiar with the concept of parameter passing
- To design functions as black boxes with clear inputs and outputs
- To be able to determine the scope of a variable

---

[← Back to Course Index](../table-of-contents.md)

## 5.1 Functions as Black Boxes

A **function** is a sequence of instructions with a name. For example, the `round` function, which was introduced in Chapter 2, contains instructions to round a floating-point value to a specified number of decimal places.



## Calling Functions

You **call** a function in order to execute its instructions. For example, by using the expression `round(6.8275, 2)`, your program calls the `round` function and asks it to round 6.8275 to two decimal digits:

```python
price = round(6.8275, 2)  # Sets result to 6.83
```

When the function finishes, it **returns** its result back to the place where it was called, and your program resumes execution.

![](media/image2.png "Picture 5")



## Function Arguments

When you call a function, you pass it *inputs*—the values it needs to do its job. In the call `round(6.8275, 2)`, the values `6.8275` and `2` are the **arguments** of the function call. Note that arguments are not necessarily input from a human user; they are simply the values for which we want the function to compute a result. Functions can receive multiple arguments, or they may have no arguments at all.



## Function Return Values

The *output* that a function computes is called its **return value**. A function returns at most one value, and that value is sent back to the point in your program where the function was called. For example:

```python
price = round(6.8275, 2)
```

When `round` returns, its result is stored in the variable `price`. Do not confuse *returning* a value with *printing* output: `return` sends a value back to the caller; `print()` displays text to the user.



## Black Box Analogy

Think of a thermostat: you set a desired temperature and it turns the heater or A/C on as needed. You don't have to know how it measures the current temp or what signals it sends—you just give it what it needs and get the result. **Use functions the same way:** treat them as black boxes. Pass in what the function needs (its arguments), and receive the answer (its return value).

## The round Function as a Black Box

When you call `round(6.8275, 2)`, you pass the necessary arguments and get back the result (6.83). As a user of the function, you don't need to know how it is implemented; you only need to know its **specification**: if you provide arguments `x` and `n`, the function returns `x` rounded to `n` decimal digits.

<img src="media/image3.png" alt="Picture 5" width="400">



## Designing Your Own Functions

When you design your own functions, make them behave like black boxes: clear inputs, clear output, and no need for the caller to know the internals. Even if you are the only person on the project, this habit keeps the program easier to understand and maintain.



## 5.2 Implementing and Testing Functions

Suppose we want a function that computes the volume of a cube. To define it we need to decide: what does it need (one number, the side length) and what does it return (the volume). When writing the function:

### Naming functions

As with variables (Chapter 2), Python style recommends **`snake_case`** for function names: lowercase words separated by underscores (for example, `cube_volume`, not `cubeVolume`). Use the same convention for parameter and local variable names (`side_length`, not `sideLength`). The name `main` is a common exception—many programs use that exact name for the starting function.

When writing the function:

1. **Pick a name** for the function (e.g. `cube_volume`).
2. **Declare a variable** for each value the function receives (e.g. `side_length`). These are called **parameter variables**.
3. **Put this together** with the `def` keyword to form the first line—the **header** of the function:

   ```python
   def cube_volume(side_length):
   ```

### Testing a Function

If you run a program that only defines a function and does not call it, nothing visible happens—the function is never executed. To test a function, your program should contain both the function definition and statements that call the function and print (or use) the result.



## Calling/Testing the Cube Function

```python
def cube_volume(side_length):
    volume = side_length ** 3
    return volume

result1 = cube_volume(2)
result2 = cube_volume(10)
print("A cube with side length 2 has volume", result1)
print("A cube with side length 10 has volume", result2)
```

- The first block is the **function definition** (implementing the function).
- The last three lines **call** the function and print the results (testing).



## Syntax: Function Definition

<img src="media/image4.jpeg" width="800">



## Programming Tip: Function Comments

Whenever you write a function, add a short comment (or docstring) that describes what it does, what its parameters mean, and what it returns. Comments are for human readers, not the compiler—they make the code easier to understand and maintain.

**Example:** A documented function. The comments (or docstring) explain the purpose, the parameters, and the return value:

```python
## Computes the volume of a cube.
#  @param side_length the length of a side of the cube
#  @return the volume of the cube
#
def cube_volume(side_length):
    volume = side_length ** 3
    return volume
```



## cubes.py with Documentation

```python
##
#  This program computes the volumes of two cubes.
#

def main():
    result1 = cube_volume(2)
    result2 = cube_volume(10)
    print("A cube with side length 2 has volume", result1)
    print("A cube with side length 10 has volume", result2)

## Computes the volume of a cube.
#  @param side_length the length of a side of the cube
#  @return the volume of the cube
#
def cube_volume(side_length):
    volume = side_length ** 3
    return volume

# Start the program.
main()
```

![](media/image5.png "image5")



## The main Function

When you structure a program with functions, it is good practice to put the main logic inside a function and treat that as the **starting point**. Any legal name is allowed for that function; we use `main` because it is the conventional name in many languages. You must have at least one statement in the program that **calls** this function (e.g. `main()`) so that execution actually starts there.



## Syntax: The main Function

![](media/image7.png "Content Placeholder 5")



## Using Functions: Order

In Python, a function must be **defined before it is called** at the top level. For example, the following will produce an error:

```python
print(cube_volume(10))

def cube_volume(side_length):
    volume = side_length ** 3
    return volume
```

Python does not know that `cube_volume` will be defined later, so the call fails.

If the call is *inside* another function (e.g. inside `main`), the callee can be defined later in the file. When `main()` runs, Python has already seen the definition of `cube_volume`. So the following is valid:

```python
def main():
    result = cube_volume(2)
    print("A cube with side length 2 has volume", result)

def cube_volume(side_length):
    volume = side_length ** 3
    return volume

main()
```


## 5.3 Parameter Passing

When you **call** a function, you pass **arguments**. The function receives those values in its **parameters**. Parameter passing is how the values get from the caller into the function.

### Arguments and parameters

- **Argument** — The value you pass in the function call (e.g. `cube_volume(2)` or `cube_volume(side)`). It can be a literal like `2` or the value of a variable.
- **Parameter** — The variable declared in the function definition (e.g. `side_length` in `def cube_volume(side_length):`). It is initialized with the argument value when the function is called and used like any other variable inside the function.

So in `result1 = cube_volume(2)`, the argument is `2`. Inside `cube_volume`, the parameter `side_length` is set to `2` for that call.

### Default parameter values

A parameter can have a **default value**. If the caller does not supply an argument for that parameter, Python uses the default instead.

```python
def greet(name="World"):
    print(f"Hello, {name}!")

greet()           # Hello, World!
greet("Alice")    # Hello, Alice!
```

In `def greet(name="World"):`, the parameter `name` defaults to `"World"`. A call with no arguments uses that default; a call with one argument replaces it.

Parameters **with** defaults must come **after** parameters **without** defaults:

```python
def power(base, exponent=2):
    return base ** exponent

print(power(5))      # 25 — exponent defaults to 2
print(power(5, 3))   # 125
```

### Keyword arguments

When you **call** a function, you can match arguments to parameters **by name** using the form `name=value`. These are **keyword arguments**. They make calls easier to read, especially when a function has several parameters.

You already saw positional arguments in Section 5.1:

```python
round(6.8275, 2)   # positional: first arg → number, second → ndigits
```

The same call with keyword arguments:

```python
round(number=6.8275, ndigits=2)   # keyword arguments
```

Both forms produce `6.83`. Keyword arguments are useful when you want to be explicit about which value goes where, or when you skip parameters that have defaults.

Combining positional and keyword arguments in one call:

```python
greet(name="Bob")              # keyword only
power(5, exponent=3)         # positional first, then keyword
round(6.8275, ndigits=2)     # mix positional and keyword
```

**Rule:** Positional arguments must come **before** keyword arguments in a call. This is invalid:

```python
greet(name="Bob", "Alice")   # Error — positional argument after keyword
```

### What happens when you call a function

Python passes the **value** of the argument into the parameter. For numbers and other simple types, the function gets a copy of the value. So when the function runs, the parameter variable holds that value and can be used in expressions—but changing the parameter variable does not change the variable in the caller.

**Example:** Steps when `result1 = cube_volume(2)` is executed:

1. The argument `2` is evaluated.
2. `cube_volume` is called and its parameter `side_length` is set to `2`.
3. The function body runs (e.g. `volume = side_length ** 3` → 8).
4. The return value is sent back and assigned to `result1`.

```python
result1 = cube_volume(2)

def cube_volume(side_length):
    volume = side_length ** 3
    return volume
```

![Parameter passing diagram](media/image8.png "Parameter passing")

### ⚠️ Common Error: Modifying parameter variables

Because the parameter holds a copy of the value (for numbers), the caller’s variables are not changed when you assign to the parameter. But **assigning to a parameter inside the function is confusing**—readers expect parameters to be “inputs” that are not reassigned. Avoid modifying parameter variables; use a local variable instead.

**Confusing (modifies parameter):**

```python
def total_cents(dollars, cents):
    cents = dollars * 100 + cents   # Modifies parameter variable
    return cents
```

**Clear (use a separate variable):**

```python
def total_cents(dollars, cents):
    result = dollars * 100 + cents
    return result
```

## 5.4 Return Values

Functions can optionally **return** a value to the caller. You use a `return` statement to send that value back. A `return` statement does two things: it immediately **terminates** the function, and it **passes** the return value back to the place where the function was called. The return value can be a literal, a variable, or any expression (e.g. a calculation).

```python
def cube_volume(side_length):
    volume = side_length ** 3
    return volume
```

### Returning multiple values

A function can return **more than one value** by listing them separated by commas in a single `return` statement:

```python
def min_max(a, b):
    if a <= b:
        return a, b
    else:
        return b, a
```

Python groups the returned values together and sends them back as **one combined result**. You will learn about **tuples**—the data type Python uses for this—in a later chapter. For now, focus on **unpacking**: assigning each returned value to its own variable.

**Unpacking** assigns each returned value to its own variable:

```python
low, high = min_max(10, 3)
print(low)    # 3
print(high)   # 10
```

The number of variables on the left must match the number of values returned.

You can also store the combined result in one variable (the value is a tuple, which you will study later):

```python
result = min_max(10, 3)
print(result)   # (3, 10) — parentheses show it is one grouped value
```

Returning multiple values is useful when a function computes several related results—for example, returning both a minimum and a maximum in one call.

A function that finds both the minimum and maximum in a short list of grades (using ideas from Chapter 4):

```python
def min_max_grade(grade1, grade2, grade3):
    minimum = grade1
    maximum = grade1
    if grade2 < minimum:
        minimum = grade2
    if grade2 > maximum:
        maximum = grade2
    if grade3 < minimum:
        minimum = grade3
    if grade3 > maximum:
        maximum = grade3
    return minimum, maximum

low, high = min_max_grade(88, 55, 72)
print(f"Min: {low}, Max: {high}")   # Min: 55, Max: 88
```

## Multiple return Statements

![](media/image13.png "Picture 8")

A function can have more than one `return` statement (for example, one per branch of an `if`). The important rule: **every path** through the function that should produce a result must hit a `return` statement.


```python
def cube_volume(side_length):
    if side_length < 0:
        return 0
    return side_length ** 3
```

**Alternative:** Instead of multiple `return` statements, you can compute the result into a variable and have a single `return` at the end of the function. For example:

```python
def cube_volume(side_length):
    if side_length >= 0:
        volume = side_length ** 3
    else:
        volume = 0
    return volume
```

## Make Sure A Return Catches All Cases

If some path through the function does not execute a `return` statement, the function still returns—Python returns the special value `None`. That can cause confusing bugs (e.g. when the caller expects a number). So make sure **every logical branch** that should produce a value has a `return`. For example, if `side_length` could be negative, zero, or positive, each case should be handled and return an appropriate value. The compiler does not warn you when a branch is missing a `return`.

```python
def cube_volume(side_length):
    if side_length >= 0:
        return side_length ** 3
    # Error — no return value if side_length < 0
```

A correct implementation:

```python
def cube_volume(side_length):
    if side_length >= 0:
        return side_length ** 3
    else:
        return 0
```

## pyramids.py

In the file `pyramids.py`, the `main` function calls `pyramid_volume` with different arguments and prints the results so you can check them against the expected values.

```python
##
#  This program defines a function for calculating a pyramid's volume and
#  provides a unit test for the function.
#

def main():
    print("Volume:", pyramid_volume(9, 10))
    print("Expected: 300")
    print("Volume:", pyramid_volume(0, 10))
    print("Expected: 0")

## Computes the volume of a pyramid whose base is a square.
#  @param height a float indicating the height of the pyramid
#  @param base_length a float indicating the length of one side of
#  the pyramid's base
#  @return the volume of the pyramid as a float
#
def pyramid_volume(height, base_length):
    base_area = base_length * base_length
    return height * base_area / 3

# Start the program.
main()
```

## 5.5 Functions Without Return Values

A function does not have to return a value. If it only needs to do something (e.g. print output or change state), you can omit the `return` statement or use `return` with no value. The function can still produce output with `print()` or other side effects.

```python
def box_string(contents):
    n = len(contents)
    print("-" * (n + 2))
    print("!" + contents + "!")
    print("-" * (n + 2))
```
![](media/image14.png "Picture 7")

```python
box_string("Hello")
```

## Using return Without a Value

You can use `return` with no value (just `return`). That **ends the function immediately** and returns `None` to the caller. This is useful to exit early when a condition is met (e.g. invalid input) without returning a result.

```python
def box_string(contents):
    n = len(contents)
    if n == 0:
        return  # Return immediately
    print("-" * (n + 2))
    print("!" + contents + "!")
    print("-" * (n + 2))
```

## 5.6 Variable Scope

The **scope** of a variable is the part of the program in which that variable is visible and can be used.

### Where variables can be declared

- **Inside a function** — These are **local variables** (including parameter variables). They are only available inside that function and are not visible to other functions.
- **Outside all functions** — These are **global variables**. They are in *global scope* and can be used (and changed) by code in any function that declares `global name` for that name.

### Local variables

Variables declared inside one function are not visible to other functions. In the following example, `side_length` is local to `main`. Using it inside `cube_volume` would be an error because `cube_volume` has no access to `main`'s locals—and here `cube_volume()` takes no parameters, so `side_length` is undefined there.

```python
def main():
    side_length = 10
    result = cube_volume()   # cube_volume has no parameters
    print(result)

def cube_volume():
    return side_length * side_length * side_length   # ERROR: side_length not defined here
```

### Reusing names in different functions

You can use the same variable name in different functions. Each function has its own local variables, so `result` in one function and `result` in another are two different variables. Their scopes do not overlap.

### Global variables

Global variables are defined at the top level of the file (outside any function). A global variable is visible to all functions defined after it in the file. Any function that wants to **assign** to a global variable must include a `global` declaration for that name; otherwise the name is treated as a new local variable.

**Example:**

```python
balance = 10000    # Global variable

def withdraw(amount):
    global balance
    if balance >= amount:
        balance = balance - amount
```

If you omit `global balance`, then `balance` inside `withdraw` would be treated as a local variable, and the assignment would not change the global `balance`.

### Programming tip: Prefer parameters and return values

There are a few cases where globals are reasonable (e.g. constants like `pi` in the `math` module), but they are rare. Programs that rely on global variables are harder to maintain and extend, because you can no longer treat each function as a *black box* that gets inputs via parameters and sends back a result via `return`. Prefer passing data in with parameters and returning results; use globals only when necessary.

---

## Key Takeaways

1. **Functions** are named sequences of instructions; they receive **arguments** and can **return** a value.
2. **Parameter passing** supplies the values for a function’s parameters; parameters can have **default values**, and callers can pass **keyword arguments** (`name=value`); avoid modifying parameter variables.
3. **Return values** are sent back with the `return` statement; a function can return **multiple values** at once; every code path that should produce a value must return.
4. **Functions without return values** can still use `return` to exit early, or omit it to fall off the end.
5. **Variable scope** is the part of the program where a variable is visible; local and parameter variables are confined to their function.

---

*End of Chapter Five*

[← Back to Course Index](../table-of-contents.md)
