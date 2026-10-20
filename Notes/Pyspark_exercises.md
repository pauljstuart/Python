# Pyspark Exercises



## Exercise 1

For example, suppose the interviewer gives you the following dataset:



"Using PySpark, find the average salary for each department, and return the departments ordered by average salary, highest first."



employees

| name    | department | salary |
| ------- | ---------- | ------ |
| Alice   | IT         | 60,000 |
| Bob     | HR         | 45,000 |
| Charlie | IT         | 75,000 |
| David   | HR         | 55,000 |
| Eve     | IT         | 80,000 |

They ask:



Here's the solution:

```
from pyspark.sql.functions import avg, colresult = (    df.groupBy("department")      .agg(avg("salary").alias("avg_salary"))      .orderBy(col("avg_salary").desc()))result.show()
```

Expected result:

| department | avg_salary |
| ---------- | ---------- |
| IT         | 71,666.67  |
| HR         | 50,000     |

This one exercise uses three critical operations:

`groupBy()` → `agg()` → `orderBy()`



## Exercise 2





Question:

You have two lists stored in variables `First` and `Last`. The length of the first list is five whereas the length of the second list is six. How can you add the first five elements from each list?

Given code:

```
First = [8, 5, 7, 8, 2]
Last = [77, 41, 22, 65, 7, 5]
# Output# [85, 46, 29, 73, 9]
```

### Answer options

A.

```
[i = First[i] + j = Last[j] for i, j in map(First, Last)]
```

B. ✅ Correct answer

```
[i + j for i, j in zip(First, Last)]
```





 

### Fizz Buzz

 

\# # Complete the 'fizzBuzz' function below. # 

\# The function accepts INTEGER n as parameter. 

 

\# def fizzBuzz(n): # Write your code here 

  range_list = range(1,n+1) 

  for n in range_list: 

​    output = "" 

​    if n%3 == 0: 

​      output = "Fizz" 

​    if n % 5 == 0: 

​      output += "Buzz" 

​    if output == "": 

​      print( n ) 

​    else: 

​      print( output ) 

 

if __name__ == '__main__':

 

 

 

 

and list comprehension 

 

 

def fizzBuzz(n):

  results = [

​    ("Fizz" * (i % 3 == 0) + "Buzz" * (i % 5 == 0)) or str(i)

​    for i in range(1, n+1)

  ]

  print("\n".join(results))

 

 

### The “Maximum Subarray Sum” challenge

 

On HackerRank it usually appears under “Algorithms → Dynamic Programming” (sometimes in the warmup/interview prep kit).

 

It’s the one solved with Kadane’s Algorithm.

 

🔹 Problem (typical wording)

 

Given an integer array, find the maximum possible sum of a contiguous subarray.

 

Input: [-2, 1, -3, 4, -1, 2, 1, -5, 4]

Output: 6

 

Kadane's Algorithm

 

 

def max_subarray_sum(arr):

  current_sum = max_sum = arr[0]

  for x in arr[1:]:

​    \# Either extend the current subarray OR start fresh from x

​    current_sum = max(x, current_sum + x)

​    \# Track global maximum

​    max_sum = max(max_sum, current_sum)

  return max_sum

 

 

 

 

### bigocheatsheet.com

 

 

 

 

### Prime Numbers

 

Find all prime numbers up to 100

 

 

### Fibonacci

 

 

First Fibonacci :

 

def fibonacci(N):

  """Return all fibonacci numbers up to N. """

  result = [0]

  next_n = 1

  while next_n <= N:

​    result.append(next_n)

​    next_n = sum(result[-2:])

  return result

 

 

print(fibonacci(0))  # [0]

print(fibonacci(1))  # [0, 1, 1]

print(fibonacci(50)) # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

 

 

Second Fibonacci with Generator

 

def fibonacci(N):

  """Return all fibonacci numbers up to N. """

  yield 0

  if N == 0:

​    return

  a = 0

  b = 1

  while b <= N:

​    yield b

​    a, b = b, a + b

 

 

print(list(fibonacci(0)))  # [0]

print(list(fibonacci(1)))  # [0, 1, 1]

print(list(fibonacci(50))) # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

 

 

Third Fibonacci

 

def fibonacci():

  a, b = 0, 1

  while True:

​    yield a

​    a, b = b, a + b

 

### ML Task #3 

 

\# 1 = Positive (Good), 0 = Negative (Bad) 

 

texts = [ 

"this is a good movie", # positive

"this is a bad movie", # Negative 

"I love this movie", # Positive 

"I do not like this movie" # Negative 

] 

 

 

new_reviews = [ 

"I really enjoyed this movie", # Expected Positive 

"This movie was not great" # Expected Negative 

] 

 

for review, sentiment in zip(new_reviews, predictions): 

 

sentiment_label = "Good Review" if sentiment == 1 else "Bad Review"

 

### Task #1

 

 

if __name__ == "__main__":

  document = [ "the cat in the hat!", 

 "the quick, brown fox.", 

 "jumped over the lazy dog?",

 "the dog sat in the hat." 

 ]

  print("Task 1 Output:", word_frequency(document) 

 

 

 

Flatten into list of words, and get rid of characters

 

1. Using list comprehension

 

import re

 

document = [

  "the cat in the hat!",

  "the quick, brown fox.",

  "jumped over the lazy dog?",

  "the dog sat in the hat."

]

 

\# Flatten into words, strip punctuation, lowercase

words = [re.sub(r'\W+', '', w).lower()

​     for line in document

​     for w in line.split()]

 

print(words)

\# ['the', 'cat', 'in', 'the', 'hat', 'the', 'quick', 'brown', 'fox', 'jumped', ...]

 

2. Using map and split

 

import re

 

\# map(str.split, document) gives a list of lists of words

\# then flatten with a comprehension

words = [re.sub(r'\W+', '', w).lower()

​     for line in map(str.split, document)

​     for w in line]

 

print(words)

 

 

Next step: frequency with comprehension + count

 

You mentioned you wanted to try without a dictionary. Once you’ve got words, you could do:

 

freq = {w: words.count(w) for w in set(words)}

print(freq)

 

 

### Task #2

 

I have the following data

 

data = {

'Name' : ['Alice', 'Bob', np.nan, 'David', 'Alice']

'Age' : [24, np.nan, 21,34,24' ],

'Score' : [ 90, 85, np.nan, 88, 90 ]

}

 

Task : return the number of people over 55 considering that score over x

 

 

from pyspark.sql import SparkSession

from pyspark.sql import functions as F

 

spark = SparkSession.builder.getOrCreate()

 

df = spark.createDataFrame([

  ("Alice", 24, 90),

  ("Bob", None, 85),

  (None, 21, None),

  ("David", 34, 88),

  ("Alice", 24, 90)

], ["Name", "Age", "Score"])

 

df.show()

 

+-----+----+-----+

| Name| Age|Score|

+-----+----+-----+

|Alice| 24|  90|

| Bob|null|  85|

| null| 21| null|

|David| 34|  88|

|Alice| 24|  90|

+-----+----+-----+

 

 

cleaning the data :

 

df_clean = df.filter(F.col("Age").isNotNull() & F.col("Score").isNotNull())

 

count over x :

 

count_over_55 = df_clean.filter(

  (F.col("Age") > 55) & (F.col("Score") > X)

).count()

 

### Counting the duplicates in a list

 

 

This is a manual frequency counter for lists (counting duplicates).

 

Here, items just means some iterable of elements — usually a list, but it could be a string, tuple, etc.

 

Examples:

 

items = ["a", "b", "a", "c", "b", "a"]

\# or

items = [1, 2, 2, 3, 1, 1]

 

freq = {}

for x in items:

  freq[x] = freq.get(x, 0) + 1

 

 

🔹 How it works, step by step

 

  freq = {} → start with an empty dictionary.

 

  for x in items: → loop through each element.

 

  freq.get(x, 0) → lookup x in the dictionary:

 

​    If it exists, return its current count.

 

​    If not, return 0 (default).

 

  \+ 1 → add one to that count.

 

  freq[x] = ... → store the updated count back in the dictionary.

 

🔹 Example in action

 

items = ["a", "b", "a", "c", "b", "a"]

freq = {}

 

for x in items:

  freq[x] = freq.get(x, 0) + 1

 

print(freq)

 

{'a': 3, 'b': 2, 'c': 1}

 

So "a" appeared 3 times, "b" 2 times, "c" once.

 

🔹 Why .get() is handy

 

Without .get(), you’d have to check manually:

 

if x in freq:

  freq[x] += 1

else:

  freq[x] = 1

 

.get(x, 0) lets you do this in one line.

 

 

 

### De-duplicating a List (remove duplicates)

 

1) Fast, order preserving

 

seen = set()

unique = []

for x in items:

  if x not in seen:

​    seen.add(x)

​    unique.append(x)

 

  Use when: you must keep first occurrence order.

 

  Works for: hashable elements (str, int, tuple, etc.).

 

  Time: O(n)

 

One-liner (readable)

 

def dedupe_keep_order(items):

  seen = set()

  return [x for x in items if not (x in seen or seen.add(x))]

 

2) Fast, order not preserved

 

unique = list(set(items))

 

 