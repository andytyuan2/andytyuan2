# Pandas to polars for myself

**Tip for polars:** 

- Transformations for dataframes can be chained together:

> ```df = df.filter(pl.col("column") == value).with_columns().select(),etc```
> Can have multiple pl.when.then before any .otherwise.alias, so long as they have the same otherwise and goal column
> polars does not have format inference like pandas, so you need to specify the format


| **Function** | **Pandas** | **Polars** |
|---|---|---|
| Reading csv | ```df = pd.read_csv("file", header = (0\1,2,etc) , index_col = "A")``` | ```df = pl.read_csv("file", (header=True\skip_rows=1,2,etc)) df = df.sort("A")``` |
| filtering, selecting | ```df1 = df.loc[df["column"] == value].index.values[0].replace("string", "variable")``` | ```df1 = df.filter(pl.col("column") == value).select(pl.col("column2")).item().replace("string", "variable")``` |
|Obtaining a list from filtering|```df.loc[df["column"] == "value1", "column2"].tolist()```|```[list(row) for row in (df.filter(pl.col("column").select(pl.col("column2").rows())]```|
|filtering|```df = df.loc[~df["column"].isin([list of values]), :,]```|```df = df.filter(~pl.col("column").is_in([list of values])```|
|changing column type|```df["column'] = pd.to_numeric(df["column"], errors = "coerce")```|```df.with_columns(pl.col("column").cast(pl.Float62, strict = False).alias("column")```|
|adding columns based on another column|```df.loc[:, "goal_column"] = df["source"].multiply(1000)```|```df.with_columns((pl.col("source") * 1000).alias("goal column")```|
|transforming only some rows in a column|```df.loc[condition, "source"] = "change value"```|```df.with_columns(pl.when(filtering/condition).then(change value).otherwise(pl.col("source")).alias("source")```|
|lazy execution|not available|for csv: ```pl.scan_csv().transformations.collect()```<br/> for excel: ```df = pl.read_excel()```, ```lazy = df.to_lazy()```, ```final = lazy.transformations.collect()```|
|date format change|```df[goal column] = pd.to_datetime(df[source], format = "%Y-%m-%d 00:00:00", errors = "coerce").dt.strftime("%#m/%#d/%Y")```|```df = df.with_columns(pl.col("source").str.strptime(pl.Datetime, "%Y-%m-%d %H:%M:%S", strict = False).dt.strftime("%-m/%-d/%Y").alias(goal column)```|
