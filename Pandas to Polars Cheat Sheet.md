# Pandas to polars for myself

| **Function** | **Pandas** | **Polars** |
|---|---|---|
| Reading csv | ```df = pd.read_csv("file", header = (0\1,2,etc) , index_col = "A")``` | ```df = pl.read_csv("file", (header=True\skip_rows=1,2,etc)) <br/> df = df.sort("A")``` |
| filtering, selecting | ```df1 = df.loc[df["column"] == value].index.values[0].replace("string", "variable")``` | ```df1 = df.filter(pl.col("column") == value).select(pl.col("column2")).item().replace("string", "variable")``` |
|Obtaining a list from filtering|```df.loc[df["column"] == "value1", "column2"].tolist()```|```[list(row) for row in (df.filter(pl.col("column").select(pl.col("column2").rows())]```|
