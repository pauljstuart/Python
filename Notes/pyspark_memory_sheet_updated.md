# PySpark Interview Memory Sheet

**Purpose:** Core PySpark DataFrame syntax and reusable patterns to reproduce confidently in a live data/AI engineering interview.  
**Priority:** ⭐ Memorise = practise without notes; ◇ Recognise = know when to use it.

**How to use:** Run the setup once, then practise sections independently. Examples use the DataFrame API rather than collecting data into Python.

## 1. Imports, creating a DataFrame ⭐

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.window import Window
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

spark = SparkSession.builder.getOrCreate()

# Example 1 – from a list of tuples

rows = [
    ("Alice", 25, "London", 50000),
    ("Bob", 30, "Paris", 60000),
    ("Charlie", 35, "London", 70000),
]
df = spark.createDataFrame(rows, ["name", "age", "city", "salary"])
df.show()

# Example 2 – from a list of Dictionaries
# The easiest and most intuitive, and you don’t need to specify the schema as it’s inside the dictionary.

from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()

data = [
    {
        "tenant": "GFZN",
        "role_name": "CURDS_OT_999",
    },
    {
        "tenant": "GFZN",
        "role_name": "CURDS_US_054",
    }  ]

df = spark.createDataFrame(data)
df.show()

# Example 3 - a column oriented dictionary (pandas only)
import numpy as np
import pandas as pd

data = {
    'Name': ['Alice', 'Bob', np.nan, 'David', 'Alice'],
    'Age': [24, np.nan, 21, 34, 24],
    'Score': [90, 85, np.nan, 88, 90]
}

pdf = pd.DataFrame(data)
df = spark.createDataFrame(pdf)
```

## 2. Reading a DataFrame from a file

```python
# reading a csv file :
csv_path = "/mnt/training/pageviews.csv"
df = spark.read.option("sep", "\t").option("inferSchema", csv(csv_path)

spark.read.json()    # read json file
spark.read.load()    # read a parquet file
```

```python
# delta catalog
spark.table()    # read a catalog delta table
spark.sql()      # returns a dataframe, sql on a catalog delta tab
```

**Remember:** `F.col("age")` refers to a Spark column, not a Python value. The `F.` convention makes it easy to see where a function comes from and avoids shadowing Python built-ins (`sum`, `max`). Alternatively: `from pyspark.sql.functions import col, lit, when`.

## 2. Selecting and filtering ⭐

```python
# selecting columns
df.select("name", "salary")                                           # choose columns
selected_columns = [‘columna’,’columnb’]
df = df.select( *selected_columns) 
df.select(F.col("salary").alias("pay"))                               # rename in result
# filtering
df.filter(F.col("age") > 30)                                          # filter rows
df.where(F.col("city") == "London")                                   # same idea as filter
df.filter((F.col("age") >= 25) & (F.col("city") == "London"))         # multiple filters & condition
df.filter((F.col("city") == "London") | (F.col("city") == "Paris"))   # multiple filters | condition
df.filter(~(F.col("age") < 30))                                       # NOT
df.filter(F.col("city").isin("London", "Paris"))
df.filter(F.col("name").like("A%"))                                   # SQL LIKE
df.filter( col("article").startswith("Special:") == False)
```

**Remember:** Use `&`, `|`, `~` for Spark column conditions, **not** Python `and`, `or`, `not`. Put each comparison in parentheses when combining conditions. `==` means equality.

## 3. Adding, replacing, renaming and dropping columns ⭐

```python
df.withColumn("bonus", F.col("salary") * 0.10)    # new column
df.withColumn("country", F.lit("UK"))           # constant column
df.withColumn("salary", F.col("salary") + 1000) # REPLACES salary
df.withColumnRenamed("salary", "annual_salary")
df.drop("city")
df.select("name", "age")
```

**Important:** `withColumn("name", ...)` replaces `name` if it already exists; `withColumn("clean_name", ...)` adds a new column. Transformations produce new DataFrames; assign the result if you want to keep it.

## 4. Conditional columns: when / otherwise ⭐

```python
pay_bands = df.withColumn(
    "band",
    F.when(F.col("salary") >= 70000, "high")
     .when(F.col("salary") >= 50000, "medium")
     .otherwise("low")
)
```

**Remember:** Conditions are checked from top to bottom; the **first match wins**. Without `.otherwise(...)`, unmatched rows become `null`. Use `F.lit(...)` for an explicit literal when useful.

## 5. String cleaning and text functions ⭐

```python
names = spark.createDataFrame(
    [(" alice ",), ("BOB",), (" Cara ",)], ["name"]
)

names.withColumn("clean_name", F.initcap(F.trim(F.col("name"))))
names.select(
    F.trim("name").alias("trimmed"),          # remove outer whitespace
    F.lower("name").alias("lowercase"),
    F.upper("name").alias("uppercase"),
    F.length("name").alias("characters"),
)
```

◇ Recognise:

```python
F.regexp_replace(F.col("name"), r"\s+", " ")   # collapse whitespace runs
F.substring(F.col("name"), 1, 3)             # first 3 chars; Spark starts at 1
F.concat_ws(" ", F.col("first"), F.col("last"))
F.split(F.col("tags"), ",")                   # string -> array
```

**Remember:** `trim()` removes outer spaces; it does not fix all internal whitespace. Use `initcap(trim(...))` to title-case cleaned names.

## 6. Missing values (NULL) ⭐

```python
# Suppose df has age, salary, name and city columns.
df.filter(F.col("salary").isNull())
df.filter(F.col("salary").isNotNull())

df.fillna(0)                                # compatible numeric columns
df.fillna(0, subset=["age", "salary"])     # selected columns
df.fillna({"age": 0, "name": "Unknown"})   # per-column replacements
df.na.fill({"salary": 0})                  # equivalent API
df.dropna()                                 # drop rows with any null
df.dropna(subset=["salary"])               # only check salary

# Use a fallback column/value when one is null:
df.withColumn("salary_safe", F.coalesce(F.col("salary"), F.lit(0)))
```

**Remember:** `fillna()` replaces **null**, not `""` or whitespace. `F.coalesce()` here is the *expression* that returns the first non-null value; `df.coalesce(n)` (section 14) is a different method for partitions. `col == None` is not the right way to test for null: use `.isNull()`.

## 7. Grouping and aggregations ⭐

```python
df.groupBy("city").count()                 # number of rows per city

df.groupBy("city").agg(
    F.count("*").alias("employees"),
    F.sum("salary").alias("total_salary"),
    F.avg("salary").alias("average_salary"),
    F.max("salary").alias("highest_salary"),
    F.min("salary").alias("lowest_salary"),
)

# Global aggregation (no groupBy):
df.agg(F.count("*").alias("total_rows"))
```

**Important:** `F.count("*")` counts rows including those with null fields; `F.count("salary")` counts non-null salaries. Most aggregations ignore nulls. `df.count()` is an **action returning a Python integer**, while `F.count()` is a Spark aggregation expression.

## 8. Sorting and deduplication ⭐

```python
df.orderBy("city")                                      # ascending
df.orderBy(F.col("salary").desc())
df.orderBy(F.col("salary").desc(), F.col("name").asc())
df.sort("name")                                       # orderBy alias

df.distinct()                                         # unique whole rows
df.dropDuplicates(["name"])                          # unique by key
df.select("city").distinct()                          # unique cities
```

**Remember:** `dropDuplicates(["name"])` does **not** guarantee which other column values are kept for duplicate names. For a deterministic choice (such as latest per key), use a window and a tie-breaker.

## 9. Joining DataFrames ⭐

```python
customers = spark.createDataFrame(
    [(1, "Alice"), (2, "Bob"), (3, "Cara")], ["customer_id", "name"]
)
orders = spark.createDataFrame(
    [(101, 1, 20), (102, 1, 30), (103, 2, 15)],
    ["order_id", "customer_id", "amount"]
)

customers.join(orders, on="customer_id", how="inner")
customers.join(orders, on="customer_id", how="left")
customers.join(orders, on="customer_id", how="left_anti") # no orders
customers.join(orders, on="customer_id", how="left_semi") # matched customers only

# Different join key names:
# left.join(right, left["id"] == right["customer_id"], "inner")


```

### The rule to memorise ⭐

| Join        | What is preserved?                                |
| ----------- | ------------------------------------------------- |
| `left`      | All rows from the DataFrame before `.join()`      |
| `right`     | All rows from the DataFrame passed into `.join()` |
| `inner`     | Only matching rows                                |
| `full`      | All rows from both DataFrames                     |
| `left_anti` | Left rows with no match on the right              |

**Remember:** A one-to-many join produces multiple rows per left-side key. `left_anti` returns left rows without a right match; `left_semi` returns left rows that have a match without right-side columns. Null join keys do not match under normal equality.

## 10. Pattern: totals after a left join ⭐

```python
result = (
    customers.join(orders, on="customer_id", how="left")
    .groupBy("customer_id", "name")
    .agg(F.sum("amount").alias("total_amount"))
    .fillna({"total_amount": 0})
)
```

**Remember:** If a customer has no orders, `sum(amount)` is null; fill it *after* aggregation. If you count orders, use `count("order_id")`, not `count("*")`, to avoid counting the unmatched left-join row as an order.

## 11. Window functions: latest, rank, lag ⭐

```python
people = spark.createDataFrame(
    [("IT", "Alice", 70000), ("IT", "Bob", 80000),
     ("IT", "Cara", 80000), ("HR", "Dan", 50000)],
    ["dept", "name", "salary"]
)

from pyspark.sql import Window  # note capital W, it's an object!

w = Window.partitionBy("dept").orderBy(F.col("salary").desc())
ranked = people.withColumn("salary_rank", F.dense_rank().over(w))
numbered = people.withColumn("rn", F.row_number().over(w))

# Deterministically pick one row per dept, including a tie-breaker:
latest = (
    people.withColumn(
        "rn",
        F.row_number().over(
            Window.partitionBy("dept")
                  .orderBy(F.col("salary").desc(), F.col("name").asc())
        ),
    )
    .filter(F.col("rn") == 1)
    .drop("rn")
)

with_previous = people.withColumn("previous_salary", F.lag("salary").over(w))
with_next = people.withColumn("next_salary", F.lead("salary").over(w))
```

row_number(), rank() and dense_rank() 

| Salary | row_number | rank | dense_rank |
| ------ | ---------- | ---- | ---------- |
| 90000  | 1          | 1    | 1          |
| 80000  | 2          | 2    | 2          |
| 80000  | 3          | 2    | 2          |
| 70000  | 4          | 4    | 3          |

- `row_number()` — unique sequential numbers, even when values tie.
- `rank()` — same rank for ties, but skips subsequent rank numbers.
- `dense_rank()` — same rank for ties, without gaps.
- `row_number()` needs a deterministic tie-breaker for a reproducible winner.

### Rolling average over current and previous two rows ◇

```python
measurements = spark.createDataFrame(
    [("A", "2026-01-01", 10), ("A", "2026-01-02", 20),
     ("A", "2026-01-03", 30), ("B", "2026-01-01", 5)],
    ["sensor", "date", "value"]
)
rolling_window = (
    Window.partitionBy("sensor").orderBy("date").rowsBetween(-2, 0)
)
measurements.withColumn("rolling_avg", F.avg("value").over(rolling_window))
```

**Remember:** `rowsBetween(-2, 0)` is the current row plus the previous two **rows**, not necessarily the previous two calendar days.

## 12. Dates and timestamps ◇

```python
# Assuming columns date_text and timestamp_text exist:
dates_df = df_with_dates.withColumn("day", F.to_date("date_text", "yyyy-MM-dd"))
dates_df = dates_df.withColumn(
    "event_ts", F.to_timestamp("timestamp_text", "yyyy-MM-dd HH:mm:ss")
)
dates_df.select(
    F.year("day"),
    F.month("day"),
    F.date_add("day", 7),
    F.datediff(F.current_date(), F.col("day")),
)
```

**Remember:** These are patterns using a hypothetical `df_with_dates` DataFrame; create it with the named fields to run them. For latest-event windows, parse timestamps before ordering; raw date strings only sort chronologically if consistently formatted.

## 13. Arrays, structs and JSON ◇

```python
# exploding an array :
docs = spark.createDataFrame(
    [(1, ["ai", "python"]), (2, ["spark"])], ["doc_id", "tags"]
)
docs.select("doc_id", F.explode("tags").alias("tag"))

# using StructType and from_json() to parse a json string
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

payloads = spark.createDataFrame(
    [(1, '{"type":"click","value":3}'), (2, '{"type":"view","value":1}')],
    ["id", "payload"]
)
payload_schema = StructType([
    StructField("type", StringType(), True),
    StructField("value", IntegerType(), True),
])
parsed = payloads.withColumn("parsed", F.from_json("payload", payload_schema))
parsed.select("id", F.col("parsed.type").alias("type"),
              F.col("parsed.value").alias("value"))
```

**Remember:** `explode()` produces one row per array element; empty or null arrays produce zero rows. `explode_outer()` can preserve a row for null/empty arrays. `from_json()` requires a schema and produces a struct/array/map depending on that schema.

## 14. Execution and performance fundamentals ◇

```python
filtered = df.filter(F.col("age") >= 30) # transformation: lazy
filtered.show()                         # action: runs the query
filtered.count()                         # action
filtered.explain()                       # inspect execution plan

cached = filtered.cache()                # mark for caching
cached.count()                           # materialise cache
cached.unpersist()                       # release cache

# When one join side is small enough to broadcast:
# big_df.join(F.broadcast(small_df), on="id", how="inner")

# Partition operations (not the same as coalesce expression):
# df.repartition(8, "city")               # redistributes with shuffle
# df.coalesce(2)                          # usually reduces partitions without full shuffle
```

**Remember:** DataFrames are distributed and unordered unless you explicitly sort. Avoid `collect()` on large data, Python loops over Spark rows, and unnecessary UDFs; prefer built-in column functions. `groupBy`, joins and global sorts can trigger shuffles. Cache only reused, expensive computations.

## 15. Inspecting and debugging ⭐

```python
df.show(20, truncate=False)
df.printSchema()
df.columns                                # Python list of column names
df.count()                                # row count (action)
df.limit(5).show()                        # sample first 5; no guaranteed order
df.explain()                              # query plan
# display(df)                             # Databricks-specific convenience
```

**Remember:** In most PySpark environments, use `df.show()`; Databricks also supports `display(df)` and, in some contexts, `df.display()`.



## 16. Writing out dataframes



```python
df.write.format("delta")
           .mode("overwrite")
           .saveAsTable("meta_data.role_dataset_map")                                # writing delta table
df.createOrReplaceTempView("temp_table")                                             # write to temp view
```





## 17. Code Snippets ⭐

**1 — Filter, select:**

```python
df.filter((F.col("city") == "London") & (F.col("age") >= 25)) \
  .select("name", "age")
```

**2 — Clean and fill nulls:**

```python
df.withColumn("clean_name", F.initcap(F.trim("name"))) \
  .fillna({"age": 0})
```

**3 — Conditional category:**

```python
df.withColumn("band", F.when(F.col("salary") >= 70000, "high")
                       .when(F.col("salary") >= 50000, "medium")
                       .otherwise("low"))
```

**4 — Count and sort:**

```python
df.groupBy("city").count() \
  .orderBy(F.col("count").desc(), F.col("city").asc())
```

**5 — Find unmatched keys:**

```python
customers.join(orders, "customer_id", "left_anti")
```

**6 - Iterating over a Dataframe:**

``` python
sql_text = "select task_name from table"
tasks_df = spark.sql(sql_text).collect()

output_text = "".join( 
             "\ntask edit {row.task_name}\nplugin reset\npersist"
             for row in tasks_df
print( output_text)
```

**6 - Writing to nested dictionary:**

``` python
sql_text = """select tenant,environment, aad_group_name
from meta_data.table
"""

df = spark.sql(sql_text)
df.show()

security_dict = {}

for row in df.collect():
    tenant = row["tenant"]
    environment = row["environment"]
    group_name = row["aad_group_name"]

    if tenant not in security_dict
        security_dict[tenant] = {}
    security_dict[tenant][environment] = group_name
    #security_dict.setdefault(tenant, {})[environment] = group_name
        

print(security_dict)
{
    'ASDT': {
        'DEVELOPMENT': 'MIM_GS4-GWG_ASDT_MEGDP_UC_SCHEMA_RO',
        'PRODUCTION': 'MIM_GS4-GWS_ASDT_MEGDP_UC_SCHEMA_RO',
        'PRODUCTION ACCEPTANCE TEST': 'MIM_GS4-GWW_ASDT_MEGDP_UC_SCHEMA_RO',
        'USER ACCEPTANCE TEST': 'MIM_GS4-GWL_ASDT_MEGDP_UC_SCHEMA_RO'
    }
}
```



---

## How to revise this sheet

- **First pass:** Memorise sections **1–9 and 15**; these are the most common everyday DataFrame operations.
- **Second pass:** Reproduce the five patterns in **section 16** without notes.
- **Third pass:** Practise **joins and windows** (sections 9–11) and explain duplicates, nulls and ties aloud.
- **Later:** Review the ◇ topics (dates, JSON, arrays, performance) without letting them crowd out fundamentals.

**Interview habit:** Explain the input schema → state the transformation → write the DataFrame chain → check a null/duplicate edge case → explain where a shuffle or action might occur.

**Provenance:** Sections 1–4 and 6–9, 15 expand and reorganise your original `pyspark_memorise.md`; the ⭐/◇ layout mirrors `python_memory_sheet.md`. Additional material in sections 5 and 10–14 plus practice patterns is supplied to support S01–S15 of your coding challenge track.
