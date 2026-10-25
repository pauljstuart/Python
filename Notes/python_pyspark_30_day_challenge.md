# Python & PySpark — 30 Practical Coding Challenges

**How to use:** Do one Python and one PySpark challenge per day for 15 days, or alternate subjects over 30 days. Write code from memory, test your output, then explain the approach aloud. No model solutions included.

**Rules:** Use Python 3 and PySpark DataFrame API. Unless explicitly requested, avoid pandas for PySpark tasks and avoid collecting data to Python to perform transformations. For PySpark, create the example DataFrame from the supplied rows. SQL is fine as a secondary solution, but first solve with the DataFrame API. If the specification is ambiguous, document your assumption.

Difficulty: ★ foundation · ★★ intermediate · ★★★ stretch.

## Python

### P01 · Filter and transform records ★

Given `people = [{"name":"Alice","age":24},{"name":"Bob","age":32},{"name":"Cara","age":29},{"name":"Dan","age":41}]`, return the names of people aged **30 or more**, alphabetically sorted. Expected: `["Bob", "Dan"]`. Practise comprehension and `sorted()`.

### P02 · Character frequencies ★
Write `count_chars(text)` that ignores spaces and case and returns a dictionary of character counts. Input: `"Hello World"`. Expected: `{'h':1,'e':1,'l':3,'o':2,'w':1,'r':1,'d':1}` (dictionary key order irrelevant). Practise `.lower()` and `.get()`.

### P03 · First non-repeating character ★★

Write `first_unique(text)` to return the first character appearing once, or `None` if none exists. Input: `"swiss"` → `"w"`; `"aabb"` → `None`. Case-sensitive. Aim for O(n).

### P04 · Group orders by customer ★★

Given `orders = [{"customer":"A","amount":20},{"customer":"B","amount":15},{"customer":"A","amount":30}]`, return customer totals as a dictionary. Expected: `{"A":50,"B":15}`. Practise dictionary accumulation.

### P05 · Sort dictionaries by multiple criteria ★★

Given `students = [{"name":"Amy","score":90},{"name":"Ben","score":80},{"name":"Cara","score":90}]`, sort by **score descending**, then **name ascending**. Expected names: `['Amy','Cara','Ben']`. Practise `sorted(..., key=...)`.

### P06 · Merge two lists of records ★★

Given `users = [{"id":1,"name":"Alice"},{"id":2,"name":"Bob"}]` and `scores = [{"id":2,"score":95},{"id":1,"score":88}]`, return records with `id`, `name`, and `score` in `users` order. Expected: `[{'id':1,'name':'Alice','score':88},{'id':2,'name':'Bob','score':95}]`. Avoid nested O(n²) scans.

### P07 · Valid parentheses ★★

Write `is_balanced(s)` for strings containing only `()[]{}`. Input: `"{[()]}"` → `True`; `"([)]"` → `False`; `""` → `True`. Practise stack/list.

### P08 · Two Sum ★★

Write `two_sum(nums, target)` to return indices of two **distinct** elements that sum to the target; return `None` if absent. Input: `nums=[2,7,11,15], target=9` → `(0,1)`. Aim for O(n).

### P09 · Find the missing integer ★
An array contains all integers from `1` to `n` except one, in any order. Write `missing_number(nums, n)`. Input: `[5,2,1,4], n=5` → `3`. Avoid sorting if possible.

### P10 · Merge overlapping intervals ★★★

Write `merge_intervals(intervals)` for closed intervals, returning sorted non-overlapping intervals. Input: `[[1,3],[2,6],[8,10],[10,12]]` → `[[1,6],[8,12]]`. Touching intervals count as overlapping.

### P11 · Parse nested JSON-like dictionaries ★★
Given `payload = {'response':{'items':[{'id':'a','status':'ok'},{'id':'b','status':'error'},{'id':'c','status':'ok'}]}}`, return all IDs with status `"ok"`. Expected: `['a','c']`. Bonus: safely handle missing `items` with an empty list.

### P12 · Batch an iterable ★★
Write `batches(items, size)` that yields consecutive lists of at most `size` elements. Input: `list(batches([1,2,3,4,5],2))` → `[[1,2],[3,4],[5]]`. Raise `ValueError` if `size <= 0`. Practise generators.

### P13 · Retry a transient failure ★★★
Write `retry(fn, attempts)` which calls a zero-argument function until it succeeds, returning its result. Catch **only** `TimeoutError`; after all allowed attempts, re-raise the final exception. Demonstrate using a test function that fails twice then returns `"ok"` on its third invocation. No sleep/backoff required.

### P14 · Log processing ★★
Given `logs = ['INFO startup','ERROR connection','WARN latency','ERROR timeout','INFO ready']`, build a frequency dictionary of log levels and return the most common level. Expected counts: `{'INFO':2,'ERROR':2,'WARN':1}`. For ties, return the alphabetically earliest level (`'ERROR'`).

### P15 · Sliding-window maximum sum ★★★
Write `max_window_sum(nums,k)` to return the largest sum of **exactly k adjacent elements**. Input: `[2,1,5,1,3,2], k=3` → `9` (`[5,1,3]`). Work in O(n); raise `ValueError` for `k < 1` or `k > len(nums)`.

## PySpark

### S01 · Create, filter and select ★
Rows: `[('Alice',24,'London'),('Bob',32,'Paris'),('Cara',29,'London')]`; columns `name, age, city`. Create a DataFrame and return just `name` and `age` for London residents aged 25 or more. Expected: `(Cara,29)`.

### S02 · Add a conditional column ★
Rows: `[('A',40000),('B',65000),('C',90000)]`; columns `employee,salary`. Add `band`: `"high"` for salary >= 70000, `"medium"` for >= 50000, otherwise `"low"`. Expected bands: low, medium, high.

### S03 · Clean names and null values ★★
Rows: `[(' alice ',None),('BOB',45),(' Cara ',30)]`; columns `name,age`. Create `clean_name` with trimmed, title-cased names; replace null ages with `0`. Expected clean names: Alice, Bob, Cara.

### S04 · Count events by type ★
Rows: `[('login',),('click',),('login',),('purchase',),('click',),('login',)]`; column `event`. Return count by event, sorted by count descending and event ascending. Expected login=3, click=2, purchase=1.

### S05 · Group with multiple aggregates ★★
Rows: `[('IT',60000),('IT',80000),('HR',45000),('HR',55000)]`; columns `dept,salary`. Return `dept`, employee count, average salary and max salary; order by `dept`. Expected: HR → 2, 50000, 55000; IT → 2, 70000, 80000.

### S06 · Deduplicate using a business key ★★
Rows: `[(1,'a@example.com'),(2,'b@example.com'),(3,'a@example.com')]`; columns `id,email`. Return unique email addresses only, alphabetically. Expected `a@example.com`, `b@example.com`. Bonus: discuss why `dropDuplicates(['email'])` does *not* guarantee which `id` is retained.

### S07 · Inner and left joins ★★
Customers: `[(1,'Alice'),(2,'Bob'),(3,'Cara')]`; columns `customer_id,name`. Orders: `[(101,1,20),(102,1,30),(103,2,15)]`; columns `order_id,customer_id,amount`. Return each customer and their order amounts via a left join, including Cara with null amount. Explain why Alice appears twice.

### S08 · Customers with no orders ★★
Use S07 inputs. Return customers who have placed no orders. Expected: Cara. Use `left_anti` join.

### S09 · Revenue by customer (including zero) ★★
Use S07 inputs. Return each customer's total order amount, including customers without orders as 0. Expected Alice=50, Bob=15, Cara=0. Beware aggregation after a left join.

### S10 · Latest event per device ★★★
Rows: `[('A','2026-01-01 09:00','ok'),('A','2026-01-02 08:00','fail'),('B','2026-01-01 10:00','ok')]`; columns `device, timestamp, status`. Cast timestamp to a timestamp type; keep only the latest record per device with `row_number()` window. Expected A→fail, B→ok. Assume distinct timestamps per device.

### S11 · Rank salaries within departments ★★★
Rows: `[('IT','Alice',70000),('IT','Bob',80000),('IT','Cara',80000),('HR','Dan',50000)]`; columns `dept,name,salary`. Return all rows with `dense_rank` descending by salary within department. Expected IT Bob/Cara rank 1, Alice rank 2; HR Dan rank 1.

### S12 · Null-safe filtering ★★
Rows: `[('A',60),('B',None),('C',85),('D',50)]`; columns `name,score`. Return count of records with score >= 60 and separately count records with null score. Expected 2 and 1. Explain why a normal comparison does not count null rows.

### S13 · Parse a JSON string column ★★★
Rows: `[(1,'{"type":"click","value":3}'),(2,'{"type":"view","value":1}')]`; columns `id,payload`. Parse payload into a struct with `from_json` and explicit schema; select `id`, `type`, and `value`. Expected `(1,click,3)`, `(2,view,1)`.

### S14 · Explode an array column ★★
Rows: `[(1,['ai','python']),(2,['spark'])]`; columns `doc_id,tags`. Produce one row per tag using `explode`; expected `(1,ai)`, `(1,python)`, `(2,spark)`. Explain what happens to rows with empty arrays.

### S15 · Rolling average of daily measurements ★★★
Rows: `[('A','2026-01-01',10),('A','2026-01-02',20),('A','2026-01-03',30),('B','2026-01-01',5)]`; columns `sensor,date,value`. Produce a rolling average over the **current and previous two rows** within each sensor, sorted by date. Expected sensor A averages 10, 15, 20; B average 5. Use a window with `rowsBetween(-2,0)`; for this exercise, dates are unique per sensor.

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
