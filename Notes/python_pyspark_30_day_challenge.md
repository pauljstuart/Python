# Python & PySpark — 30 Practical Coding Challenges

**How to use:** Do one Python and one PySpark challenge per day for 15 days, or alternate subjects over 30 days. Write code from memory, test your output, then explain the approach aloud. No model solutions included.

**Rules:** Use Python 3 and PySpark DataFrame API. Unless explicitly requested, avoid pandas for PySpark tasks and avoid collecting data to Python to perform transformations. For PySpark, create the example DataFrame from the supplied rows. SQL is fine as a secondary solution, but first solve with the DataFrame API. If the specification is ambiguous, document your assumption.

Difficulty: ★ foundation · ★★ intermediate · ★★★ stretch.

## Python

### P01 · Filter and transform records ★

Given `people = [{"name":"Alice","age":24},{"name":"Bob","age":32},{"name":"Cara","age":29},{"name":"Dan","age":41}]`, return the names of people aged **30 or more**, alphabetically sorted. Expected: `["Bob", "Dan"]`. Practise comprehension and `sorted()`.



```
#P01 · Filter and transform records ★

#Given 
# 
people = [{"name":"Alice","age":24},{"name":"Bob","age":32},{"name":"Cara","age":29},{"name":"Dan","age":41}]

# return the names of people aged **30 or more**, alphabetically sorted. Expected: `["Bob", "Dan"]`.

results = [ I["name"] for I in people if I["age"] >= 30 ]

sorted_results = sorted(results)

print( sorted_results )

```



### P02 · Character frequencies ★
Write `count_chars(text)` that ignores spaces and case and returns a dictionary of character counts. Input: `"Hello World"`. Expected: `{'h':1,'e':1,'l':3,'o':2,'w':1,'r':1,'d':1}` (dictionary key order irrelevant). Practise `.lower()` and `.get()`.



```
########### P02

# Write `count_chars(text)` that ignores spaces and case and returns a dictionary of character counts. 
# Input: `"Hello World"`. Expected: `{'h':1,'e':1,'l':3,'o':2,'w':1,'r':1,'d':1}` (dictionary key order irrelevant). 
# Practise `.lower()` and `.get(

def count_chars( text ):

    text = text.replace( ' ', '').lower()
    count_dict = {}
    for char in text :
        # count_dict[char] = count_dict.get(char, 0) + 1
        if char in count_dict:
            count_dict[char] +=1
        else:
            count_dict[char] = 1
    return count_dict


count_chars("Hello World")
```



### P03 · First non-repeating character ★★

Write `first_unique(text)` to return the first character appearing once, or `None` if none exists. Input: `"swiss"` → `"w"`; `"aabb"` → `None`. Case-sensitive. Aim for O(n).



```
#####  P03 · First non-repeating character ★★


# Write `first_unique(text)` to return the first character appearing once, or `None` if none exists. 
# Input: `"swiss"` → `"w"`; `"aabb"` → `None`. 
# Case-sensitive. Aim for O(n).

def first_unique( text ):

    count_dict = {}
    for char in text :
        count_dict[char] = count_dict.get(char, 0) + 1
        # if char in count_dict:
        #     count_dict[char] +=1
        # else:
        #     count_dict[char] = 1

    for char, count in count_dict.items():
        if count == 1:
            return char

    return None




result = first_unique( "aabb")

print( result )
```



### P04 · Group orders by customer ★★

Given `orders = [{"customer":"A","amount":20},{"customer":"B","amount":15},{"customer":"A","amount":30}]`, return customer totals as a dictionary. Expected: `{"A":50,"B":15}`. Practise dictionary accumulation.



```

###########  P04 · Group orders by customer ★★

# Given `orders = [{"customer":"A","amount":20},{"customer":"B","amount":15},{"customer":"A","amount":30}]`, return customer totals as a dictionary. 
# Expected: `{"A":50,"B":15}`. 
# Practise dictionary accumulation.



def get_customer_total( orders_list):

    customer_total = {}
    for amount_record in orders_list:
        customer_name = amount_record["customer"]
        customer_amount = amount_record["amount"]

        customer_total[customer_name] = customer_total.get(customer, 0) + customer_amount
        # if customer_name in customer_total:
        #     customer_total[ customer_name ] += customer_amount
        # else:
        #     customer_total[ customer_name ] = customer_amount
    return customer_total

orders = [{"customer":"A","amount":20},{"customer":"B","amount":15},{"customer":"A","amount":30} ]

totals = get_customer_total( orders)

print( totals)
```



### P05 · Sort dictionaries by multiple criteria ★★

Given `students = [{"name":"Amy","score":90},{"name":"Ben","score":80},{"name":"Cara","score":90}]`, sort by **score descending**, then **name ascending**. Expected names: `['Amy','Cara','Ben']`. Practise `sorted(..., key=...)`.

```
###########  P05 · Sort dictionaries by multiple criteria ★★

#Given `students = [{"name":"Amy","score":90},{"name":"Ben","score":80},{"name":"Cara","score":90}]`, 
# sort by **score descending**, then **name ascending**. Expected names: `['Amy','Cara','Ben']`. 
# Practise `sorted(..., key=...)`.


students = [{"name":"Amy","score":90},{"name":"Ben","score":80},{"name":"Cara","score":90}]

sorted_list = sorted(students, key=lambda x: x["name"])


sorted_list = sorted(sorted_list, key=lambda x: x["score"], reverse=True)

# Important interview rule: When using multiple stable sorts, sort by the secondary criterion first and the primary criterion last.


print( sorted_list)
```





### P06 · Merge two lists of records ★★

Given `users = [{"id":1,"name":"Alice"},{"id":2,"name":"Bob"}]` and `scores = [{"id":2,"score":95},{"id":1,"score":88}]`, return records with `id`, `name`, and `score` in `users` order. Expected: `[{'id':1,'name':'Alice','score':88},{'id':2,'name':'Bob','score':95}]`. Avoid nested O(n²) scans.



```
########### P06  Merge two lists of records ★★


# Given `users = [{"id":1,"name":"Alice"},{"id":2,"name":"Bob"}]` and `scores = [{"id":2,"score":95},{"id":1,"score":88}]`, return records with `id`, `name`, and `score` in `users` order. Expected: `[{'id':1,'name':'Alice','score':88},{'id':2,'name':'Bob','score':95}]`. 
#Avoid nested O(n²) scans.


users = [{"id":1,"name":"Alice"},{"id":2,"name":"Bob"}] 
scores = [{"id":2,"score":95},{"id":1,"score":88}]

score_by_id_dict = {  s["id"] : s["score"]   for s in scores}
print( score_by_id_dict )


merged_list = [ {"id" : X["id"], "name": X["name"], "score": score_by_id_dict[ X["id"] ] } for X in users ]

print( merged_list)
```



### P07 · Valid parentheses ★★

Write `is_balanced(s)` for strings containing only `()[]{}`. Input: `"{[()]}"` → `True`; `"([)]"` → `False`; `""` → `True`. Practise stack/list.



```
### P07 · Valid parentheses ★★

#Write `is_balanced(s)` for strings containing only `()[]{}`. Input: `"{[()]}"` → `True`; `"([)]"` → `False`; `""` → `True`. 
#Practise stack/list.


def is_balanced( s ):

    state_string = []
    open = "{[("
    closed = "])}"
    
    for char in s :


        if char in open :
            state_string.append(char)
            continue
        if char in closed:
            if state_string:
                open_char = state_string[ -1 ]
            else:
                return False
            if open_char == "(" and char != ")":
                return False
            if open_char == "[" and char != "]":
                return False
            if open_char == "{" and char != "}":
                return False
            state_string.pop()

    # in case of opens with no cloeses at all
    return len(state_string) == 0


print( is_balanced( "{[()]}") )

print( is_balanced( "([)]" ) )


print( is_balanced( "]" )   )
```



### P08 · Two Sum ★★

Write `two_sum(nums, target)` to return indices of two **distinct** elements that sum to the target; return `None` if absent. Input: `nums=[2,7,11,15], target=9` → `(0,1)`. Aim for O(n).

```
########### P08 · Two Sum ★★

#Write `two_sum(nums, target)` to return indices of two **distinct** elements that sum to the target; return `None` if absent. 
# Input: `nums=[2,7,11,15], target=9` → `(0,1)`. 
# Aim for O(n).

nums=[2,7,11,15]

# Two Sum = calculate the complement → look it up → store the current value


def two_sums(nums, target):

    seen = {}
    for index, value in enumerate( nums ):
        seeking = target - value
        if seeking in seen:
            return seen[seeking], index
        seen[value] = index

    return None
        

print(two_sums(nums, 18) )
```





### P09 · Find the missing integer ★
An array contains all integers from `1` to `n` except one, in any order. Write `missing_number(nums, n)`. Input: `[5,2,1,4], n=5` → `3`. Avoid sorting if possible.



```
########### P09 · Find the missing integer ★


# An array contains all integers from `1` to `n` except one, in any order. 
# Write `missing_number(nums, n)`. Input: `[5,2,1,4], n=5` → `3`. 
# Avoid sorting if possible.


# better to use sets rather than lists as sets are more efficient

def missing_number( nums, n+1):
    ideal_range = set(range(1, n + 1))

    for x in nums:
        ideal_range.discard(x)

    return ideal_range.pop()


# using set substraction

def missing_two(nums, n):
    expected = set(range(1, n + 1))
    actual = set(nums)

    return (expected - actual).pop()




# calculating the sequence sum


# sum = n * (n + 1) // 2

def missing_number(nums, n):
    expected_sum = n * (n + 1) // 2
    actual_sum = sum(nums)

    return expected_sum - actual_sum



print( missing_number( [5,2,3,4] , 5  )  )

```



### P10 · Merge overlapping intervals ★★★

Write `merge_intervals(intervals)` for closed intervals, returning sorted non-overlapping intervals. Input: `[[1,3],[2,6],[8,10],[10,12]]` → `[[1,6],[8,12]]`. Touching intervals count as overlapping.



```
### P10 · Merge overlapping intervals ★★★

#Write `merge_intervals(intervals)` for closed intervals, returning sorted non-overlapping intervals. 


# Input: [[1,3],[2,6],[8,10],[10,12]] → [[1,6],[8,12]] 


# Test cases worth remembering
# Input	Expected output
# [[1,3],[2,6],[8,10],[10,12]]	[[1,6],[8,12]]
# [[1,10],[2,5]]	[[1,10]]
# [[8,10],[1,3],[2,6]]	[[1,6],[8,10]]
# []	[]
# [[1,2]]

# Touching intervals count as overlapping.


def merge_intervals(intervals):

    if not intervals:
        return []

    intervals = sorted(intervals, key=lambda x: x[0])

    merged_interval_list = []
    index = 0
    merged_interval = intervals[0].copy()

    while index < len(intervals) - 1:

        this_list = intervals[index + 1]

        if this_list[0] <= merged_interval[1]:
            merged_interval[1] = max(merged_interval[1], this_list[1])
        else:
            merged_interval_list.append(merged_interval)
            merged_interval = this_list.copy()

        index += 1

    merged_interval_list.append(merged_interval)

    return merged_interval_list


output_list = merge_intervals([[1,3],[2,6],[8,10],[10,12]])

print( output_list)
```



### P11 · Parse nested JSON-like dictionaries ★★
Given `payload = {'response':{'items':[{'id':'a','status':'ok'},{'id':'b','status':'error'},{'id':'c','status':'ok'}]}}`, return all IDs with status `"ok"`. Expected: `['a','c']`. Bonus: safely handle missing `items` with an empty list.



```

### Next: P11 — Parse Nested JSON-like Dictionaries ★★

# Given:


payload = {    "response": {        "items": [            {"id": "a", "status": "ok"},            {"id": "b", "status": "error"},            {"id": "c", "status": "ok"}        ]    }}


# Return the IDs of items whose status is `"ok"`.

# Expected:

# ```
# ["a", "c"]
# ```

# Bonus: Make your solution safely handle a missing `"items"` key by returning an empty list.

# Try solving it from memory using dictionary access and list comprehension.
def get_ok( payload : dict):

    try :
        item_list = payload["response"]["items"]
    except KeyError:
        print("no items found")
        return []

    target_list = []
    for this_item in item_list:
        #print( f"examining : {this_item}")
        if this_item["status"] == "ok":
            target_list.append(this_item["id"])
    
    return target_list




# the .get() appraoach :
# def get_ok(payload):
#     items = payload.get("response", {}).get("items", [])

#     return [
#         item["id"]
#         for item in items
#         if item["status"] == "ok"
#     ]


payload2 = {    "response": {   } }
                            
result = get_ok( payload)

print( result)

```



### P12 · Batch an iterable ★★
Write `batches(items, size)` that yields consecutive lists of at most `size` elements. Input: `list(batches([1,2,3,4,5],2))` → `[[1,2],[3,4],[5]]`. Raise `ValueError` if `size <= 0`. Practise generators.



```
### P12 · Batch an iterable ★★

#Write `batches(items, size)` that yields consecutive lists of at most `size` elements. 


#Input: `list(batches([1,2,3,4,5],2))` → `[[1,2],[3,4],[5]]`. 

#Raise `ValueError` if `size <= 0`. 

#Practise generators.


def batches( items, size ):

    if size <= 0:
        raise ValueError
    
    for I in range(0,len(items), size):
        this_item = items[I: I+size  ]  # python automatically stops at the end of the list with slicing
        yield( this_item)


list(batches([1,2,3,4,5],2))


# Indexing accesses one element and can raise IndexError. Slicing extracts a range and tolerates out-of-bounds endpoints.

# items[start:stop:step]

# - start — where slicing begins (inclusive).
# - stop — where slicing ends (exclusive).
# - step — how many positions to move each time (defaults to 1).

items = [10, 20, 30, 40, 50]

print(items[2:2])  # []
print(items[3:1])  # []
```





### P13 · Retry a transient failure ★★★

Write `retry(fn, attempts)` which calls a zero-argument function until it succeeds, returning its result. Catch **only** `TimeoutError`; after all allowed attempts, re-raise the final exception. Demonstrate using a test function that fails twice then returns `"ok"` on its third invocation. No sleep/backoff required.



```

### P13 · Retry a transient failure ★★★

 #Write `retry(fn, attempts)` which calls a zero-argument function until it succeeds, returning its result. 
 # 
 # 
 # Catch **only** `TimeoutError`; after all allowed attempts, re-raise the final exception. 
 # Demonstrate using a test function that fails twice then returns "ok" on its third invocation. No sleep/backoff required.


def test_generator() :
    yield( "fail")
    yield(  "fail")
    yield( "ok")


gen = test_generator()


def test_function():
    result = next(gen)

    if result == "fail":
        raise TimeoutError("Temporary timeout")

    return result


def retry(fn, attempts):

    for i in range(attempts):
        print(f"attempt {i + 1}")

        try:
            return fn()

        except TimeoutError:
            if i == attempts - 1:
                raise
    
retry( test_function, 3)


```



### P14 · Log processing ★★

Given `logs = ['INFO startup','ERROR connection','WARN latency','ERROR timeout','INFO ready']`, build a frequency dictionary of log levels and return the most common level. Expected counts: `{'INFO':2,'ERROR':2,'WARN':1}`. For ties, return the alphabetically earliest level (`'ERROR'`).



```
# Next: P14 — Log Processing ★★
# Given:
logs = [    'INFO startup',    'ERROR connection',    'WARN latency',    'ERROR timeout',    'INFO ready']



# Build a frequency dictionary of log levels and return the most common level.

# Expected counts:
# {'INFO': 2, 'ERROR': 2, 'WARN': 1}



# Important: If two levels have the same count, return the alphabetically earliest level.

# Expected result: "ERROR".

# Try implementing this from memory. You'll be able to reuse dictionary accumulation from P02 and P04, along with sorting concepts from P05.

freq_dict = {}

for log_entry in logs:
    if 'INFO' in log_entry :
        freq_dict['INFO'] = freq_dict.get('INFO',0 ) + 1
    if 'ERROR' in log_entry :
        freq_dict['ERROR'] = freq_dict.get('ERROR',0 ) + 1
    if 'WARN' in log_entry :
        freq_dict['WARN'] = freq_dict.get('WARN',0 ) + 1


sorted_list =  sorted( freq_dict.items() , key = lambda X : (-X[1],X[0])   )

print( sorted_list )

most_common = sorted_list[0][0]

print( most_common)


```



### P15 · Sliding-window maximum sum ★★★
Write `max_window_sum(nums,k)` to return the largest sum of **exactly k adjacent elements**. Input: `[2,1,5,1,3,2], k=3` → `9` (`[5,1,3]`). Work in O(n); raise `ValueError` for `k < 1` or `k > len(nums)`.



```
# Next: P15 — Sliding Window Maximum Sum ★★★
# This is the final Python challenge in your 15-question set.

# Write max_window_sum(nums, k) that returns the largest sum of exactly k consecutive elements.

# Given:
nums = [2, 1, 5, 1, 3, 2]



# Expected result:
# 9  # From [5, 1, 3]



# Requirements:
# - Aim for O(n) time complexity.
# - Raise ValueError if k < 1 or k > len(nums).
# - Avoid recalculating the entire sum for every window.
# Hint: Think about what happens to the sum when a window moves one position to the right. One number leaves, and another enters.

# Give it a try from memory.

def max_window_sum( nums, k) :
    length_num = len(nums)
    if k < 1 or k > length_num:
        raise ValueError

    highest = 0
    # get first window
    window_list = nums[k]
    window_sum = sum(window_list)
    highest = window_sum
    #print( window_sum)
    for I in range(0,  length_num - k ):
        window_sum += nums[I+k]
        window_sum -= nums[I]
        #print( f"I : {I} sum : {window_sum} ")
        highest = max(highest, window_sum)
    return highest


output = max_window_sum(nums, 3)

print( output )
```





## PySpark Challenges



### S01 · Create, filter and select ★
Rows: `[('Alice',24,'London'),('Bob',32,'Paris'),('Cara',29,'London')]`; columns `name, age, city`. Create a DataFrame and return just `name` and `age` for London residents aged 25 or more. Expected: `(Cara,29)`.



```
### S01 · Create, filter and select ★


#Create a DataFrame and return just `name` and `age` for London residents aged 25 or more. 

rows = [('Alice',24,'London'),('Bob',32,'Paris'),('Cara',29,'London')]

# Expected: `(Cara,29)`.


from pyspark.sql.functions import col


columns = [ "name", "age", "city" ] 


df = spark.createDataFrame( rows, schema = columns )

df2 = (
    df.filter(
        (col("city") == "London") &
        (col("age") >= 25)
    )
    .select("name", "age")
)

df2.show()
df2.display()
```





### S02 · Add a conditional column ★
Rows: `[('A',40000),('B',65000),('C',90000)]`; columns `employee,salary`. Add `band`: `"high"` for salary >= 70000, `"medium"` for >= 50000, otherwise `"low"`. Expected bands: low, medium, high.



```
### S02 · Add a conditional column ★
z
rows = [('A',40000),('B',65000),('C',90000)]

columns =  ('employee','salary')

#Add `band`: `"high"` for salary >= 70000, `"medium"` for >= 50000, otherwise `"low"`. 
# Expected bands: low, medium, high.

from pyspark.sql.functions import col, when

df = spark.createDataFrame( rows, columns)

df2 = ( df.withColumn("band", 
                    when(col("salary") >= 70000, "high")
                    .when(col("salary") >= 50000, "medium")
                    .otherwise("low"))
)
df2.show()
```





### S03 · Clean names and null values ★★

Rows: `[(' alice ',None),('BOB',45),(' Cara ',30)]`; columns `name,age`. Create `clean_name` with trimmed, title-cased names; replace null ages with `0`. Expected clean names: Alice, Bob, Cara.



```
### S03 · Clean names and null values ★★

rows = [(' alice ',None),('BOB',45),(' Cara ',30)]

columns = ("name","age") 

#Create `clean_name` with trimmed, title-cased names; 
# replace null ages with `0`. Expected clean names: Alice, Bob, Cara.

from pyspark.sql.functions import col, when, isnull, trim, initcap

df = spark.createDataFrame( rows, columns)

#clean_name = df.withColumn("age", when( isnull(col("age")) , 0).otherwise( col("age")) )

clean_name = ( df.fillna(  { "age":  0} )
            .withColumn("clean_name", trim(col("name")))
            .withColumn("clean_name", initcap( col("clean_name" )   ))
            )
clean_name.display()

clean_name.show()
```





### S04 · Count events by type ★
Rows: `[('login',),('click',),('login',),('purchase',),('click',),('login',)]`; column `event`. Return count by event, sorted by count descending and event ascending. Expected login=3, click=2, purchase=1.



```

## S04 - count events by type


rows = [
    ("login",),
    ("click",),
    ("login",),
    ("purchase",),
    ("click",),
    ("login",)
]
import pyspark.sql.functions as F

columns = ["event"]

#Return the number of occurrences of each event, 
# sorted by count descending and then event name ascending.

df = spark.createDataFrame( rows, columns)

df_agg = df.groupBy("event").agg( F.count('*').alias("occurences") )

df_agg = df_agg.orderBy( F.col("occurences").desc(), F.col("event").asc() )
df_agg.display()
df_agg.show()
```



### S05 · Group with multiple aggregates ★★
Rows: `[('IT',60000),('IT',80000),('HR',45000),('HR',55000)]`; columns `dept,salary`. Return `dept`, employee count, average salary and max salary; order by `dept`. Expected: HR → 2, 50000, 55000; IT → 2, 70000, 80000.



```
### S05 · Group with multiple aggregates ★★


rows = [    ("IT", 60000),    ("IT", 80000),    ("HR", 45000),    ("HR", 55000)]
columns = ["dept", "salary"]


# Create a DataFrame and return the following information for each department:

# - `dept` — department name
# - `employee_count` — number of employees
# - `average_salary` — average salary
# - `max_salary` — highest salary

# Sort by department name ascending.

# Expected output:

# ```
# +----+--------------+--------------+----------+
# |dept|employee_count|average_salary|max_salary|
# +----+--------------+--------------+----------+
# |  HR|             2|       50000.0|     55000|
# |  IT|             2|       70000.0|     80000|
# +----+--------------+--------------+----------+

#Hint: You can call `agg()` once with multiple aggregation expressions.

#Try solving it from memory using your `F` import convention.

# This is a particularly useful pattern for data engineering interviews because you're often asked to produce summary statistics from large datasets.


import pyspark.sql.functions as F

df = spark.createDataFrame( rows, columns)

df_agg = (df.groupBy("dept")
          .agg( F.count('*').alias("employee_count"), 
               F.avg("salary").alias("average_salara"), 
               F.max("salary").alias("max_salary") ) 
          .orderBy("dept")
)


df_agg.collect()
df_agg.show()

```





### S06 · Deduplicate using a business key ★★
Rows: `[(1,'a@example.com'),(2,'b@example.com'),(3,'a@example.com')]`; columns `id,email`. Return unique email addresses only, alphabetically. Expected `a@example.com`, `b@example.com`. Bonus: discuss why `dropDuplicates(['email'])` does *not* guarantee which `id` is retained.

```

# S06 · Deduplicate using a business key ★★

rows = [
    (1, "a@example.com"),
    (2, "b@example.com"),
    (3, "a@example.com")
]

columns = ["id", "email"]

# Create a DataFrame and return unique email addresses only, sorted alphabetically.

# Expected output:
# +-------------+
# |        email|
# +-------------+
# |a@example.com|
# |b@example.com|
# +-------------+


# Hint: Consider distinct() or dropDuplicates().



df = spark.createDataFrame( rows, columns)

df2 = df.select("email").distinct().orderBy("email")

#df2 = df.dropDuplicates( ["email"])
df2.show()


# Bonus interview question: If you use dropDuplicates(["email"]) while retaining the id column, which ID would Spark keep for a@example.com — 1 or 3?

# Try answering that as well as writing the code.

# answer : there is no guarantee

# Interview takeaway: distinct() and dropDuplicates() are ideal for removing duplicates, but don't rely on dropDuplicates() to choose a particular surviving record.
```





### S07 · Inner and left joins ★★
Customers: `[(1,'Alice'),(2,'Bob'),(3,'Cara')]`; columns `customer_id,name`. Orders: `[(101,1,20),(102,1,30),(103,2,15)]`; columns `order_id,customer_id,amount`. Return each customer and their order amounts via a left join, including Cara with null amount. Explain why Alice appears twice.

```
#07 · Inner and left joins ★★

customers = [
    (1, "Alice"),
    (2, "Bob"),
    (3, "Cara")
]

orders = [
    (101, 1, 20),
    (102, 1, 30),
    (103, 2, 15)
]

df_customers = spark.createDataFrame(
    customers, ["customer_id", "name"]
)

df_orders = spark.createDataFrame(
    orders, ["order_id", "customer_id", "amount"]
)

# Task: Write PySpark code using a left join to return each customer's name and their order amounts, including customers who have no orders.

# Expected output (order of rows may vary):
# +-----+------+
# | name|amount|
# +-----+------+
# |Alice|    20|
# |Alice|    30|
# |  Bob|    15|
# | Cara|  NULL|
# +-----+------+

import pyspark.sql.functions as F

# the left table is the one before the .join
df = df_customers.join(df_orders, on = "customer_id", how="left").select("name", "amount")

df.show()

# Alice appears twice as there are two entries for Alice in the orders table
```





### S08 · Customers with no orders ★★

Use S07 inputs. Return customers who have placed no orders. Expected: Cara. Use `left_anti` join.



```
# S08 · Customers with no orders ★★
# Use the same two DataFrames from S07:
# df_customersdf_orders


# Task: Return only customers who have never placed an order.

# Expected result:
# +-----------+----+
# |customer_id|name|
# +-----------+----+
# |          3|Cara|
# +-----------+----+


# Hint: PySpark has a join type called left_anti, which is specifically designed for this.

# Bonus interview question: What's the difference between a left join and a left_anti join?

# Try writing the solution and explaining that distinction.

df = df_customers.join(df_orders, on = "customer_id", how="left_anti")

# essentially a left anti join returns the rows that had no match from the standard left join, these are the rows
# that would have resulted in nulls.

df.show()
```





### S09 · Revenue by customer (including zero) ★★
Use S07 inputs. Return each customer's total order amount, including customers without orders as 0. Expected Alice=50, Bob=15, Cara=0. Beware aggregation after a left join.



```

# S09 · Revenue by customer (including zero) ★★


# Using the same DataFrames:
# df_customers,df_orders



# Task: Return each customer's total order amount, including customers without orders, whose total should be 0.

# Expected output:
# +-----+------------+
# | name|total_amount|
# +-----+------------+
# |Alice|          50|
# |  Bob|          15|
# | Cara|           0|
# +-----+------------+


# Sort by name ascending.

# Hints: You'll need join(), groupBy(), sum() and a way of handling nulls.

# Bonus interview question: Why does Cara's total initially become NULL rather than 0 when you use sum("amount") after a left join?

# Try solving it from memory, using  import pyspark.sql.functions as F


import pyspark.sql.functions as F

df = df_customers.join( df_orders, on = "customer_id", how="left").fillna({"amount":0}  )
# we use a left join on df_customers so we have all customers, so Cara's amount column will be null


df2 = df.groupBy("name").agg( F.sum( F.col("amount")  ).alias("total_amount")  ).orderBy( F.col("name").asc() )

df2.show()
```



### S10 · Latest event per device ★★★
Rows: `[('A','2026-01-01 09:00','ok'),('A','2026-01-02 08:00','fail'),('B','2026-01-01 10:00','ok')]`; columns `device, timestamp, status`. Cast timestamp to a timestamp type; keep only the latest record per device with `row_number()` window. Expected A→fail, B→ok. Assume distinct timestamps per device.



```
# S10 · Latest event per device ★★★
# We're moving on to window functions, one of the most important intermediate-to-advanced PySpark interview topics.

# Given:

rows = [    ("A", "2026-01-01 09:00", "ok"),    ("A", "2026-01-02 08:00", "fail"),    ("B", "2026-01-01 10:00", "ok")]
columns = ["device", "timestamp", "status"]



# Task: Return only the latest event for each device.

# Requirements:
# 1. Convert the string timestamp column to a proper Spark timestamp type.
# 2. Use row_number() with a window partitioned by device.
# 3. Order timestamps descending within each partition.
# 4. Keep only the latest record for each device.
# Expected result:
# +------+-------------------+------+
# |device|timestamp          |status|
# +------+-------------------+------+
# |A     |2026-01-02 08:00:00|fail  |
# |B     |2026-01-01 10:00:00|ok    |
# +------+-------------------+------+


# Hints: Window.partitionBy(), orderBy(), row_number() and to_timestamp().

# Bonus interview question: What's the difference between row_number(), rank() and dense_rank() when two records have the same timestamp?

# Give it a try from memory, and we'll work through any unfamiliar window syntax.


import pyspark.sql.functions as F
from pyspark.sql import Window

initial = spark.createDataFrame( rows, columns)

latest_event = (initial
                .withColumn("timestamp", F.to_timestamp("timestamp"))
                .withColumn("rank", F.row_number().over( Window.partitionBy("device").orderBy( F.col("timestamp").desc()     )))
                .filter( F.col("rank") == 1)
                .drop("rank")


)

latest_event.show()

# the difference between row_number(), rank() and dense_rank() is how they deal with duplicates.  row_number() gives 
# each row a unique id, rank() gives duplicates the same id, but preserves the overall numbering, and dense_rank()
# also labels duplicates with the same id, but it does not preserve the overall numbering
```



### S11 · Rank salaries within departments ★★★
Rows: `[('IT','Alice',70000),('IT','Bob',80000),('IT','Cara',80000),('HR','Dan',50000)]`; columns `dept,name,salary`. Return all rows with `dense_rank` descending by salary within department. Expected IT Bob/Cara rank 1, Alice rank 2; HR Dan rank 1.

```
### S11 · Rank salaries within departments ★★★

# Given:


rows = [    ("IT", "Alice", 70000),    ("IT", "Bob", 80000),    ("IT", "Cara", 80000),    ("HR", "Dan", 50000) ]

columns = ["dept", "name", "salary"]


# Create a DataFrame and rank employees by salary within each department, highest salary first.

# This time, use `dense_rank()` so employees with equal salaries receive the same rank.

# Expected output:

# 
# +----+-----+------+----+
# |dept| name|salary|rank|
# +----+-----+------+----+
# |  HR|  Dan| 50000|   1|
# |  IT|  Bob| 80000|   1|
# |  IT| Cara| 80000|   1|
# |  IT|Alice| 70000|   2|
# +----+-----+------+----+
# 

# Sort by `dept` ascending, `rank` ascending, and `name` ascending.

# Bonus interview question: Why would using `row_number()` instead of `dense_rank()` produce an incorrect result for Bob and Cara?

# You already have nearly all the syntax from S10, so try this one without looking anything up.

import pyspark.sql.functions as f
from pyspark.sql import Window


df = spark.createDataFrame( rows, columns)

df2 = df.withColumn( "rank", 
                    f.dense_rank().over( Window.partitionBy(f.col("dept") ).orderBy( f.col("salary").desc() ) ) 
        ).orderBy( f.col("dept").asc(),f.col("rank").asc(),f.col("name").asc()  )
                                        

df2.show()                              

# row_number() will allocate them unique ranks, when in fact they are the same.
```





### S12 · Null-safe filtering ★★

Rows: `[('A',60),('B',None),('C',85),('D',50)]`; columns `name,score`. Return count of records with score >= 60 and separately count records with null score. Expected 2 and 1. Explain why a normal comparison does not count null rows.

```
### S12 · Null-safe filtering ★★

import pyspark.sql.functions as f

Rows = [('A',60),('B',None), ('C',85),('D',50) ] 


#
# Return count of records with score >= 60 and separately count records with null score. 
# Expected 2 and 1. 
# 
# Explain why a normal comparison does not count null rows.

df = spark.createDataFrame( Rows, ["name","score" ])

count = df.filter( f.col("score") >= 60 ).count()

print( f"60 and over : {count} " )



count = df.filter( f.col("score").isNull() ).count()


print( f"nulls : {count} " )

# null isn't a valid valid so it will never be included in any normal filter.
```





### S13 · Parse a JSON string column ★★★

Rows: `[(1,'{"type":"click","value":3}'),(2,'{"type":"view","value":1}')]`; columns `id,payload`. Parse payload into a struct with `from_json` and explicit schema; select `id`, `type`, and `value`. Expected `(1,click,3)`, `(2,view,1)`.



```
# S13 · Parse a JSON string column ★★★
# This is particularly relevant for data engineering and AI engineering pipelines, where API responses and event messages frequently contain JSON.

# Given:


rows = [    (1, '{"type":"click","value":3}'),    (2, '{"type":"view","value":1}')]
columns = ["id", "payload"]


# Create a DataFrame and:
# 1. Parse the JSON string in payload into a structured column using from_json().
# 2. Define an explicit schema using StructType, StructField, StringType and IntegerType.
# 3. Extract the fields type and value.
# 4. Return only id, type and value.
# Expected output:
# +---+-----+-----+
# | id| type|value|
# +---+-----+-----+
# |  1|click|    3|
# |  2| view|    1|
# +---+-----+-----+


# Hints: You'll need these imports:
# import pyspark.sql.functions as f
# from pyspark.sql.types import (    StructType, StructField, StringType, IntegerType)


# The unfamiliar part may be defining the schema. Here's the basic pattern:
# schema = StructType([    StructField("field_name", StringType(), True)])

# For extracting a nested field, remember that PySpark supports dot notation:
# f.col("parsed.type")

import pyspark.sql.functions as f
from pyspark.sql.types import (    StructType, StructField, StringType, IntegerType)

json_schema = StructType( [ StructField("type", StringType(), True), StructField("value", IntegerType(), True)] )

df = spark.createDataFrame( rows, columns )

df_json = df.withColumn( "parsed", f.from_json( f.col("payload"), json_schema ) )

df_json.printSchema()

#df_json.display()

df_final = df_json.select( f.col("id"), f.col("parsed.type"), f.col("parsed.value"))
df_final.show()

# root
#  |-- id: long (nullable = true)
#  |-- payload: string (nullable = true)
#  |-- parsed: struct (nullable = true)
#  |    |-- type: string (nullable = true)
#  |    |-- value: integer (nullable = true)

# +---+-----+-----+
# | id| type|value|
# +---+-----+-----+
# |  1|click|    3|
# |  2| view|    1|
# +---+-----+-----+



# Bonus interview question: Why is it generally preferable to parse JSON into a structured column rather than repeatedly extracting individual fields from the original JSON string?

# just for performance reasons I imagine

```



### S14 · Explode an array column ★★

Rows: `[(1,['ai','python']),(2,['spark'])]`; columns `doc_id,tags`. Produce one row per tag using `explode`; expected `(1,ai)`, `(1,python)`, `(2,spark)`. Explain what happens to rows with empty arrays.



```
# S14 · Explode an array column ★★
# Now let's practise working with arrays.

# Given:
rows = [    (1, ["ai", "python"]),    (2, ["spark"])]

columns = ["doc_id", "tags"]



# Create a DataFrame that produces one row per tag.

# Expected output:
# +------+------+
# |doc_id|   tag|
# +------+------+
# |     1|    ai|
# |     1|python|
# |     2| spark|
# +------+------+


# Use PySpark's explode() function.

# Hints: explode() transforms an array into multiple rows. You can use .withColumn() or .select() to apply it.

# Bonus interview question: What happens if a record contains an empty array, such as:
# (3, [])
# Will it produce a row with NULL, or will the row disappear entirely?

import pyspark.sql.functions as f

df = spark.createDataFrame( rows, columns)

df2 = df.withColumn( "explode", f.explode("tags") )


# +------+------------+-------+
# |doc_id|        tags|explode|
# +------+------------+-------+
# |     1|[ai, python]|     ai|
# |     1|[ai, python]| python|
# |     2|     [spark]|  spark|
# +------+------------+-------+



df2.show()

df2 = df2.select(f.col("doc_id"), f.col("explode").alias("tag")  ) 

df2.show()

# +------+------+
# |doc_id|   tag|
# +------+------+
# |     1|    ai|
# |     1|python|
# |     2| spark|
# +------+------+

# Bonus interview question: What happens if a record contains an empty array, such as:
# (3, [])
# Will it produce a row with NULL, or will the row disappear entirely?

# I think if a row has an array with no elements, then no exploded rows will be created.



```



### S15 · Rolling average of daily measurements ★★★
Rows: `[('A','2026-01-01',10),('A','2026-01-02',20),('A','2026-01-03',30),('B','2026-01-01',5)]`; columns `sensor,date,value`. Produce a rolling average over the **current and previous two rows** within each sensor, sorted by date. Expected sensor A averages 10, 15, 20; B average 5. Use a window with `rowsBetween(-2,0)`; for this exercise, dates are unique per sensor.

```

# S15 · Rolling average of daily measurements ★★★
# Final challenge
# This is the last exercise in your original 15 PySpark challenges, and it combines window functions with aggregation.


rows = [    ("A", "2026-01-01", 10),    ("A", "2026-01-02", 20),    ("A", "2026-01-03", 30),    ("B", "2026-01-01", 5)]

columns = ["sensor", "date", "value"]



# Task: Calculate a rolling average for each sensor using the current row and the previous two rows, ordered by date.

# Expected output:
# +------+----------+-----+-----------+
# |sensor|      date|value|rolling_avg|
# +------+----------+-----+-----------+
# |     A|2026-01-01|   10|       10.0|
# |     A|2026-01-02|   20|       15.0|
# |     A|2026-01-03|   30|       20.0|
# |     B|2026-01-01|    5|        5.0|
# +------+----------+-----+-----------+


# Requirements:
# 1. Partition the window by sensor.
# 2. Order by date ascending.
# 3. Include the current row and the previous two rows.
# 4. Use f.avg() to calculate the rolling average.
# 5. Name the new column rolling_avg.
# Hint: The new syntax is:
# Window.partitionBy("sensor") \      .orderBy("date") \      .rowsBetween(-2, 0)

from pyspark.sql.window import Window
import pyspark.sql.functions as f

# Here, -2 means two rows before the current row, and 0 means the current row.

# Give S15 a go from memory. After this, you'll have completed the entire first set of 15 PySpark challenges!

w = Window.partitionBy( "sensor").orderBy("date").rowsBetween(-2,0)  

df = spark.createDataFrame( rows, columns)

df2 = df.withColumn( "rolling_avg", f.avg( f.col("value")).over( w))

df2.show()

# Bonus interview question: What is the difference between rowsBetween() and rangeBetween() in PySpark window functions?

# The difference seems to be that with rangeBetween() you can specify the upper and lower values that you want to encompass the window
```





###  S16 -  Average salary by department

"Using PySpark, find the average salary for each department, and return the departments ordered by average salary, highest first."

employees

| name    | department | salary |
| ------- | ---------- | ------ |
| Alice   | IT         | 60,000 |
| Bob     | HR         | 45,000 |
| Charlie | IT         | 75,000 |
| David   | HR         | 55,000 |
| Eve     | IT         | 80,000 |

Expected result:

| department | avg_salary |
| ---------- | ---------- |
| IT         | 71,666.67  |
| HR         | 50,000     |

This one exercise uses three critical operations:

`groupBy()` → `agg()` → `orderBy()`

Here's the solution:

```
data = [
    ("Alice", "IT", 60000),
    ("Bob", "HR", 45000),
    ("Charlie", "IT", 75000),
    ("David", "HR", 55000),
    ("Eve", "IT", 80000)
]

df = spark.createDataFrame(
    data, schema=["name", "dept", "salary"]
)

df.display()


from pyspark.sql.functions import avg, col


result = (
    df.groupBy("dept").agg(
        avg("salary").alias("avg_salary")
    )
)

# .orderBy(col("avg_salary").desc())

result = result.orderBy(col("avg_salary").desc())
result.show()
```



### S17 -  Return the number of people over X



return the number of people over 55 considering that score over x




```python
import numpy as np
import pandas as pd

data = {
    'Name': ['Alice', 'Bob', np.nan, 'David', 'Alice'],
    'Age': [24, np.nan, 21, 34, 24],
    'Score': [90, 85, np.nan, 88, 90]
}


from pyspark.sql import functions as f
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()

pandas_df = pd.DataFrame( data)

#print( pandas_df )


df = spark.createDataFrame(pandas_df)


#df.show()

x = 20

df = df.filter( (f.col("age").isNotNull()) & ( f.col("score").isNotNull() )) 

df.show()

df2 = df.filter( f.col("score") > x )
df2.show()

Y = df2.count()


print( f"The number of people over score {x} is {Y}   ")
```



## Self-review rubric

After each problem, check:

- **Correctness:** Is the output right for the sample and an edge case?
- **Clarity:** Could I explain the approach before writing code?
- **Memorisation:** Which syntax did I need to look up?
- **Efficiency:** Is the algorithm reasonable? For Spark, did I keep computation in Spark rather than `collect()`?
- **Improvement:** Could I express it more clearly using a standard library or DataFrame API pattern?

## Progress tracker

| Day | Python | PySpark | From memory? | Notes |
|---|---|---|---|---|
| 1 | P01 | S01 | ☐ | |
| 2 | P02 | S02 | ☐ | |
| 3 | P03 | S03 | ☐ | |
| 4 | P04 | S04 | ☐ | |
| 5 | P05 | S05 | ☐ | |
| 6 | P06 | S06 | ☐ | |
| 7 | P07 | S07 | ☐ | |
| 8 | P08 | S08 | ☐ | |
| 9 | P09 | S09 | ☐ | |
| 10 | P10 | S10 | ☐ | |
| 11 | P11 | S11 | ☐ | |
| 12 | P12 | S12 | ☐ | |
| 13 | P13 | S13 | ☐ | |
| 14 | P14 | S14 | ☐ | |
| 15 | P15 | S15 | ☐ | |
