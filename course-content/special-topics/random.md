# Special Topic: Random Choices

[← Back to Course Index](../table-of-contents.md)

Python's built-in **`random`** module lets you introduce unpredictability into a program — picking a value from a list, simulating a die roll, or branching with a given probability.

No installation is needed; `random` is part of the standard library.

---

## Picking One Item from a List

Use **`random.choice()`** to select a single item from a sequence (such as a list):

```python
import random

colors = ["red", "green", "blue", "yellow"]
picked = random.choice(colors)
print(picked)  # e.g. "blue"
```

Each call may return a different item. Every item in the list has an equal chance of being chosen.

**Example: choice from a comma-separated string**

If you have a string like `"apple,banana,cherry"`, split it into a list first:

```python
import random

fruits_string = "apple, banana, cherry"
fruits = [fruit.strip() for fruit in fruits_string.split(",")]
picked = random.choice(fruits)
print(picked)  # e.g. "banana"
```

`strip()` removes extra spaces around each name.

---

## Random Integers in a Range

Use **`random.randint(a, b)`** to get a random whole number from `a` through `b` (both ends included):

```python
import random

roll = random.randint(1, 6)
print(roll)  # simulates a six-sided die: 1, 2, 3, 4, 5, or 6
```

---

## Choosing Between Options with a Probability

Use **`random.random()`** to get a random decimal between `0.0` and `1.0` (including 0.0, but not including 1.0). Compare that value to a threshold to branch with a percentage chance.

**Example: roughly 25% chance**

```python
import random

if random.random() < 0.25:
    print("Lucky!")
else:
    print("Not this time.")
```

`random.random() < 0.25` is true about one quarter of the time.

**Example: two branches with unequal odds**

```python
import random

if random.random() < 1 / 3:
    print("Outcome A")   # about one third of the time
else:
    print("Outcome B")   # about two thirds of the time
```

You will not get exactly the target ratio on every single run; the split evens out over many runs.

---

## Quick Reference

| Task | Function |
|------|----------|
| Pick one item from a list | `random.choice(my_list)` |
| Random integer from `a` through `b` (inclusive) | `random.randint(a, b)` |
| Random float from 0.0 up to (but not including) 1.0 | `random.random()` |

---

[← Back to Course Index](../table-of-contents.md)
