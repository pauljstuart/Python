# Python Interview Memory Sheet

**Purpose:** Core Python syntax and reusable patterns to reproduce confidently in a live coding interview.  
**Priority:** ⭐ Memorise = practise without notes; ◇ Recognise = know when to use it.

## 1. Data types ⭐

```python
names = ["Alice", "Bob"]              # list: ordered, mutable, duplicates allowed
point = (10, 20)                    # tuple: ordered, immutable
unique = {"Alice", "Bob"}           # set: unique items, mutable
person = {"name": "Alice", "age": 30}  # dict: key/value pairs
empty_set = set()                   # {} creates an empty dict, NOT a set
```

- Lists, dictionaries and sets are mutable; tuples, strings and `frozenset`s are immutable.
- `in` tests membership: `"Alice" in names`, `"name" in person`.
- Dictionary keys must be hashable (for example, strings, numbers or suitable tuples).

## 2. Operators and conditions ⭐

```python
x += 1       # add and assign
x -= 1       # subtract and assign
x ** 2      # power
10 / 4      # 2.5: true division
10 // 4     # 2: floor division
10 % 4      # 2: remainder
x == 5      # value equality
x != 5      # not equal
x > 0 and x < 10
x in [1, 2, 3]
```

**Remember:** `==` compares values; `is` tests identity. Use `value is None` for a `None` check. `|` means set union, dictionary merge (Python 3.9+) or bitwise OR, depending on operands.

## 3. Strings ⭐

```python
s = "  Hello,Python  "
s.strip()                    # "Hello,Python"
s.lower()                    # "  hello,python  "
s.upper()
s.replace("Python", "World")
s.split(",")                 # ["  Hello", "Python  "]
"-".join(["a", "b", "c"])     # "a-b-c"
"hello".startswith("he")     # True
"file.csv".endswith(".csv")  # True
"Python" in s               # True
len(s)

name = "Alice"
message = f"Hello, {name}!"  # f-string
```

**Know string indexing and slicing:**

```python
s = "Python"
s[0]       # "P"
s[-1]      # "n"
s[1:4]     # "yth" (end excluded)
s[::-1]    # "nohtyP" (reverse)
```

◇ Recognise: `find()`, `count()`, `title()`, `capitalize()`, `isdigit()`, `isalpha()`, `isalnum()`.

**Important:** Strings are immutable: `s.replace(...)` returns a new string; it does not change `s`.

## 4. Lists ⭐

Definition : A list is an ordered, mutable collection

```python
nums = [3, 1, 2]
nums.append(4)         				# add one item
nums.extend([5, 6])					# add several items
nums.remove(3)         				# remove first matching value
last = nums.pop()      				# remove and return last item
nums.sort()             			# sort list IN PLACE
ordered = sorted(nums, reverse = True)  # return NEW sorted list

nums[0]                				# first element
nums[-1]             				# last element
nums[:2]             				# first two elements
nums[::-1]              			# reversed copy
# in place methods (returns None)
lst.sort()
lst.reverse()
lst.clear()
lst.append()
lst.extend(iterable)
lst.insert(i, x)
lst.remove(x)
lst.pop(i)    # this returns the popped (removed) value
lst.copy      # shallow copy
```

`append`, `sort` and `reverse` mutate the list and return `None`. `sorted()` works with other iterables too.

## 5. Dictionaries ⭐ — especially important

```python
person = {"name": "Alice", "age": 30}

person["name"]                  # "Alice"; KeyError if missing
person.get("city")              # None if missing
person.get("city", "Unknown")   # provide default
person["city"] = "London"      # add or update
person.update({"age": 31})     # update from another dict
person.pop("city", None)        # remove key safely

person.keys()                   # view of keys
person.values()                 # view of values
person.items()                  # view of (key, value) pairs
"name" in person               # checks KEYS, not values

d[key] = value   # replaces the existing value.
d[key] = d.get(key, 0) + value # accumulates value
```

### A. Iterate over a dictionary ⭐

```python
for key, value in person.items():
    print(key, value)
```

### B. Count frequencies ⭐

```python
words = ["cat", "dog", "cat", "bird", "dog", "cat"]
counts = {}

for word in words:
    counts[word] = counts.get(word, 0) + 1

# {'cat': 3, 'dog': 2, 'bird': 1}
```

### C. Group records by a field ⭐

```python
rows = [
    {"name": "Alice", "team": "IT"},
    {"name": "Bob", "team": "HR"},
    {"name": "Cara", "team": "IT"},
]

groups = {}
for row in rows:
    team = row["team"]
    groups.setdefault(team, []).append(row["name"])

# {'IT': ['Alice', 'Cara'], 'HR': ['Bob']}
```

### D. Sort by a dictionary value ⭐

```python
scores = {"Alice": 80, "Bob": 95, "Cara": 75}
ranked = sorted(scores.items(), key=lambda pair: pair[1], reverse=True)
# [('Bob', 95), ('Alice', 80), ('Cara', 75)]
```

### E. Merge two dictionaries ◇

```python
a = {"x": 1, "y": 2}
b = {"y": 9, "z": 3}
merged = a | b      # {'x': 1, 'y': 9, 'z': 3}
```

**Remember:** Later values win when keys overlap.

### F Converting a Dictionary to a list of tuples

 ``` python
# items() always returns a tuple

freqs = {'to': 2, 'be': 2, 'or': 1, 'not': 1}
pairs = list(freqs.items())
print(pairs)
```
### G Adding Duplicate keys

in Python, if you add a duplicate key to a dictionary, you do not get an error.
Instead, the new value simply overwrites the old one.



## 6. Sets ⭐

```python
a = {1, 2, 3}
b = {3, 4, 5}
a | b      # union: {1, 2, 3, 4, 5}
a & b      # intersection: {3}
a - b      # difference: {1, 2}
a ^ b      # symmetric difference: {1, 2, 4, 5}
a.add(6)
a.discard(10)   # safely ignores missing value

unique_names = list(set(["Alice", "Bob", "Alice"]))
When comparing two collections to find missing or additional elements, consider set difference.

```

**Caution:** Converting to a set removes duplicates but doesn't preserve original order. To deduplicate while keeping order:

```python
names = ["Alice", "Bob", "Alice"]
unique_in_order = list(dict.fromkeys(names))  # ['Alice', 'Bob']
```

## 7. Loops and comprehensions ⭐

```python
for i, name in enumerate(["Alice", "Bob"]):
    print(i, name)                  # index and value

for name, score in zip(["A", "B"], [90, 80]):
    print(name, score)              # pair values

squares = [x * x for x in range(5)]
evens = [x for x in range(10) if x % 2 == 0]
unique_lengths = {len(s) for s in ["cat", "bird", "dog"]}
lookup = {name: len(name) for name in ["Alice", "Bob"]}
generator = (x * x for x in range(5))
# multidimension comprehension
pairs = [(x, y) for x in [1, 2, 3] for y in [4, 5, 6]]
```

Comprehension patterns:

```python
[expression for x in iterable if condition]   # list
{expression for x in iterable if condition}   # set
{k: v for x in iterable}                      # dictionary
(expression for x in iterable)                 # generator
```

## 8. Sorting and selecting ⭐

```python
# sorting a list
numbers = [5, 2, 9]
sorted(numbers)                  # ascending, new list
sorted(numbers, reverse=True)    # descending
sum(numbers)
min(numbers)
max(numbers)
len(numbers)

# sorting a dictionary
scores = {"Charlie": 85, "Alice": 92, "Bob": 78}
sorted(score)                                      # Sorted keys (list)
sorted(scores.items())                              # Sort by keys (list of tuples)
sorted(scores.items(), key=lambda x: x[1])          # Sort by values (ascending)
sorted(scores.items(), key=lambda x: x[1], reverse=True)  # Sort by values (descending)
dict(sorted(scores.items()))                        # Dictionary sorted by keys
dict(sorted(scores.items(), key=lambda x: x[1]))    # Dictionary sorted by values

# sorting a list of dictionaries
people = [
    {"name": "Alice", "age": 30},
    {"name": "Bob", "age": 25},
]
by_age = sorted(people, key=lambda p: p["age"])  # note that sorted() returns a list of tuples not a dict!

oldest = max(people, key=lambda p: p["age"])
sorted(students, key=lambda s: s["score"])         # lowest score first
max(students, key=lambda s: s["score"])            # dict with highest score
# sorting by 2 fields :
by_name = sorted(students, key=lambda s: s["name"])
result = sorted(by_name, key=lambda s: s["score"], reverse=True)

Remember:
- sorted(d) → list of keys
- sorted(d.items()) → list of (key, value) tuples
- dict(sorted(d.items())) → dictionary with sorted insertion order
- sorted() always returns a list, never a dictionary, and does not modify the original dictionary.

# Important interview rule: When using multiple stable sorts, sort by the secondary criterion first and the primary criterion last.

```

## 9. Functions, type hints and error handling ⭐

```python
def total_prices(prices: list[float]) -> float:
    return sum(prices)

try:
    value = int("123")
except ValueError as exc:
    print(f"Invalid number: {exc}")
 
# anonymous functions
 def get_name(X):
    return X["name"]

lambda X: X["name"]    # same thing
```

Understand positional arguments, keyword arguments and default arguments. Avoid mutable defaults such as `def f(items=[])`.

## 10. JSON and records ⭐ — useful for AI/backend engineering

```python
import json

payload = '{"name": "Alice", "active": true}'
record = json.loads(payload)       # JSON string -> Python dict
text = json.dumps(record)          # Python object -> JSON string

# list of dictionaries: very common API response shape
users = [{"id": 1, "active": True}, {"id": 2, "active": False}]
active_ids = [u["id"] for u in users if u["active"]]
```

`json.load(file)` reads a file; `json.loads(string)` reads a string. `json.dump(obj, file)` writes a file; `json.dumps(obj)` returns a string.

## 11. Useful standard-library shortcuts ◇

```python
from collections import Counter, defaultdict

Counter(["a", "b", "a"])       # Counter({'a': 2, 'b': 1})

by_team = defaultdict(list)
by_team["IT"].append("Alice")
```

`Counter` handles frequency counting; `defaultdict(list)` handles grouping. Be able to write the equivalent manually first.



## 12. Datetime


``` python
# imports
from datetime import datetime, date, timedelta

# Getting now
now = datetime.now()   # current date & time
today = date.today()   # just the date

# Difference between two dates / times
d1 = datetime(2023, 5, 1, 14, 30)
d2 = datetime(2023, 5, 3, 16, 45)
delta = d2 - d1  # type: timedelta
print(delta)   # 2 days, 2:15:00

# Access parts of the interval:
delta.days    # 2
delta.seconds   # 8100  (2h 15m = 8100 sec)
delta.total_seconds() # 183900.0

#Adding / subtracting intervals
future = now + timedelta(days=7, hours=3)
past  = now - timedelta(days=1)

# Only working with date (no time)

d1 = date(2023, 1, 1)
d2 = date(2023, 1, 15)
print((d2 - d1).days) # 14

# Parsing & formatting date strings
# String → datetime
dt = datetime.strptime("2025-08-26 14:30", "%Y-%m-%d %H:%M")
# Datetime → string
print(dt.strftime("%d %B %Y, %H:%M")) # "26 August 2025, 14:30"

 

#Common patterns
# Days between two dates:
(date2 - date1).days
# Hours/minutes between two datetimes:
(dt2 - dt1).total_seconds() / 3600
(dt2 - dt1).total_seconds() / 60


Pro tip for interviews:

Always remember datetime - datetime → timedelta, and timedelta gives you .days, .seconds, and .total_seconds() for flexibility.
```

####  Python Date & Time Format Codes (strftime/strptime)

| Code | Meaning            | Code | Meaning        |
| ---- | ------------------ | ---- | -------------- |
| `%Y` | Year (2025)        | `%y` | Year (25)      |
| `%m` | Month (08)         | `%B` | August         |
| `%b` | Aug                | `%d` | Day (26)       |
| `%A` | Tuesday            | `%a` | Tue            |
| `%H` | Hour 24h (14)      | `%I` | Hour 12h (02)  |
| `%p` | AM/PM              | `%M` | Minute (30)    |
| `%S` | Second (05)        | `%f` | Microseconds   |
| `%z` | UTC offset (+0100) | `%Z` | Timezone (UTC) |

Common examples:

- `%Y-%m-%d` → `2025-08-26`
- `%d/%m/%Y` → `26/08/2025`
- `%H:%M:%S` → `14:30:05`
- `%I:%M %p` → `02:30 PM`
- `%A, %d %B %Y` → `Tuesday, 26 August 2025`



## 13. Five interview patterns to practise ⭐

**1 — Count occurrences:** loop over values and update `counts[x] = counts.get(x, 0) + 1`.

**2 — Filter records:** `[r for r in records if r["active"]]`.

**3 — Group records:** use `groups.setdefault(key, []).append(value)`.

**4 — Deduplicate while preserving order:** `list(dict.fromkeys(values))` (for hashable values).

**5 — Sort records:** `sorted(records, key=lambda r: r["score"], reverse=True)`.

### Mini practice questions (write from memory)

1. Given a list of words, return a dictionary containing each word's count.
2. Given a list of employee dictionaries, return the names of employees with salary above 60,000.
3. Group a list of employee dictionaries by department.
4. Remove duplicate names from a list while retaining their original order.
5. Return the three highest-scoring records from a list of dictionaries.
6. Given a JSON string containing users, extract the IDs of active users.

---

## How to revise this sheet

- **First pass:** Memorise sections **3–8 and 10**; these cover most small data-manipulation exercises.
- **Second pass:** Practise the five patterns without looking at their examples.
- **Third pass:** Reproduce one mini challenge while narrating your approach aloud.
- **Later:** Review the ◇ items; don't let them crowd out the fundamentals.

**Interview habit:** Explain the data shape → state your approach → write a small working solution → test an edge case (empty input, missing key, duplicates, etc.).



## Python Snippets

 


``` python
# Counting occurences
items = ["a", "b", "a", "c", "b", "a"]
freq = {}

for x in items:
  freq[x] = freq.get(x, 0) + 1

print(freq)
{'a': 3, 'b': 2, 'c': 1}
```


 ``` python
# de-duplicating a list :

items = [ 3,2,34,12,]
seen = set()
unique = []

for x in items:
  if x not in seen:
    seen.add(x)
    unique.append(x)
 ```

``` python
#sorting a list of tuples

list = [('be', 2), ('not', 1), ('or', 1), ('to', 2)]

sorted(list) → sorts by first element, then second, etc.
sorted(list, key=lambda x: x[0]) → sort by first element
sorted(list, key=lambda x: x[1]) → sort by second element
```

``` python
#Creating a multidimensional list of 3 integers

  array = [[i, j, k] for i in range(x+1) for j in range(y+1) for k in range(z+1) i]

  print( array)
```

``` python
# Using RE groups
import re

 
s = "Name: Alice, Age: 30"
match = re.search(r"Name: (\w+), Age: (\d+)", s)

if match:
  print(match.group(1)) # 'Alice'
  print(match.group(2)) # '30'
```



