# Special Topic: Timestamps

[← Back to Course Index](../table-of-contents.md)

A **timestamp** is a string that records when something happened — for example, when a log entry was written or when a backup file was created. Putting a timestamp in a file name helps keep each version unique so a new run does not overwrite an older one.

Python's **`datetime`** module (standard library) provides the current date and time and lets you format it as a string.

---

## Getting the Current Date and Time

```python
from datetime import datetime

now = datetime.now()
print(now)  # e.g. 2026-03-14 10:30:45.123456
```

`datetime.now()` returns a `datetime` object with the current local date and time.

---

## Formatting a Timestamp String

Use **`strftime()`** (string format time) to turn a `datetime` into a custom string:

```python
from datetime import datetime

now = datetime.now()
timestamp = now.strftime("%Y-%m-%dT%H-%M-%S")
print(timestamp)  # e.g. 2026-03-14T10-30-45
```

Common format codes:

| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | 4-digit year | `2026` |
| `%m` | Month (01–12) | `03` |
| `%d` | Day (01–31) | `14` |
| `%H` | Hour, 24-hour (00–23) | `10` |
| `%M` | Minute (00–59) | `30` |
| `%S` | Second (00–59) | `45` |

The `T` between date and time in the example above is a literal character in the format string — it is not a special code. It is a common way to separate the date from the time in a single string.

Other useful patterns:

```python
now.strftime("%Y-%m-%d")           # 2026-03-14
now.strftime("%H:%M:%S")           # 10:30:45
now.strftime("%B %d, %Y")          # March 14, 2026
```

---

## Using a Timestamp in a File Name

Combine a prefix and a formatted timestamp to build a unique file name:

```python
from datetime import datetime

timestamp = datetime.now().strftime("%Y-%m-%dT%H-%M-%S")
filename = f"backup-{timestamp}.txt"
print(filename)  # e.g. backup-2026-03-14T10-30-45.txt
```

Each time your program runs, `datetime.now()` produces a new value, so the file name is different unless two runs happen in the same second.

**Example: a simple log line**

```python
from datetime import datetime

timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
print(f"[{timestamp}] Program started.")
# e.g. [2026-03-14 10:30:45] Program started.
```

---

[← Back to Course Index](../table-of-contents.md)
