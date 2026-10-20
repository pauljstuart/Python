# Pyspark Memorise

 

 

## A.  Imports and creating a DataFrame



```
from pyspark.sql import SparkSession

from pyspark.sql.functions import col, lit, sum, avg, count, when

spark = SparkSession.builder.getOrCreate()

data = [
  ("Alice", 25, "London", 50000),
  ("Bob", 30, "Paris", 60000),
 ("Charlie", 35, "London", 70000)
]


df = spark.createDataFrame(  data, ["name", "age", "city", "salary"])

df.show()

```



 

## B.  Selecting, filtering and changing columns

 

```
# Select columns
df.select("name", "salary")

 
# Filter rows
df.filter(col("age") > 30)


# Add a calculated column
df = df.withColumn("bonus", col("salary") * 0.10)


# Add a constant column
df = df.withColumn("country", lit("UK"))


# Rename a column
df = df.withColumnRenamed("salary", "annual_salary")


# Remove a column
df = df.drop("city")


# Conditional logic
df = df.withColumn(
  "category",
  when(col("age") >= 30, "Senior").otherwise("Junior")
)
```





## C. Grouping, aggregating and sorting

 

 

 ```
 # Count records by city
 
 df.groupBy("city").count()
 
  
 
 # Average salary by city
 
 df.groupBy("city").agg(
 
   avg("salary").alias("average_salary")
 
 )
 
  
 
 # Multiple aggregations
 
 df.groupBy("city").agg(
 
   count("*").alias("employees"),
 
   sum("salary").alias("total_salary")
 
 )
 
  
 
 # Sort descending
 
 df.orderBy(col("salary").desc())
 
  
 
 # Remove duplicates
 
 df.dropDuplicates(["name"])
 
  
 
 # Get unique values
 
 df.select("city").distinct()
 ```



## D. Joining DataFrames

 

 ```
 # Join using a common column
 
 result = df1.join(df2, on="id", how="inner")
 
  
 
 # Left join
 
 result = df1.join(df2, on="id", how="left")
 
  
 
 # Find rows in df1 without a matching row in df2
 
 result = df1.join(df2, on="id", how="left_anti")
 ```





 

 

## E.  Nulls and inspecting results

 

 ```
 # Find missing values
 
 df.filter(col("salary").isNull())
 
  
 
 # Find non-null values
 
 df.filter(col("salary").isNotNull())
 
  
 
 # Replace missing values
 
 df.fillna({"salary": 0})
 
  
 
 # Remove rows containing nulls
 
 df.dropna()
 
  
 
 # Inspect data
 
 df.show()
 
 df.printSchema()
 
 df.count()
 
 df.columns
 ```



