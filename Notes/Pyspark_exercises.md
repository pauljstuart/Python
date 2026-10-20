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

