# **🐼 Pandas Cheat Sheet (Core Functions)**

### **1. Creating / Loading Data**

pd.Series(), pd.DataFrame() → create data structures
pd.read_csv(), pd.read_excel(), pd.read_sql(), pd.read_json() → load data
df.to_csv(), df.to_excel() → save/export data


### **2. Inspecting Data**

df.head(), df.tail() → preview rows
df.info() → data types + non-nulls
df.describe() → summary stats
df.shape, df.columns, df.dtypes → structure info


### **3. Selection & Indexing**

df['col'], df[['col1','col2']] → select columns
df.loc[row_label, col_label] → label-based
df.iloc[row_idx, col_idx] → position-based
df[df['col'] > 10] → boolean filtering


### **4. Data Cleaning**

df.isna(), df.notna() → check missing
df.dropna(), df.fillna() → handle missing
df.drop() → drop rows/columns
df.rename() → rename cols/index
df.astype() → type conversion
df.duplicated(), df.drop_duplicates()


### **5. Transformations**

df.apply(func) → apply function row/col
df.map(func) → element-wise (Series)
df.replace() → substitute values
df.sort_values(), df.sort_index()


### **6. Aggregation & Grouping**

df.groupby('col') → group by categories
.agg({'col1':'mean','col2':'sum'}) → multiple aggregations
.mean(), .sum(), .count(), .min(), .max(), .median()
df.value_counts() → frequencies


### **7. Merging & Reshaping**

pd.concat([df1, df2]) → stack/append
pd.merge(df1, df2, on='col') → SQL-style join
df.pivot(), df.pivot_table() → reshape wide
df.melt() → reshape long
df.stack(), df.unstack()


### **8. Time Series**

pd.to_datetime() → convert to datetime
df.resample('M').mean() → resample to monthly
df.rolling(window=3).mean() → moving window
df.shift(), df.diff()