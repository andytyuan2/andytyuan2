# Pandas to polars for myself

| **Function** | **Pandas** | **Polars** |
|---|---|---|
| Reading csv | ```python df = pd.read_csv("file", header = (0\1,2,etc) , index_col = "A")``` | ```python df = pl.read_csv("file", (header=True\skip_rows=1,2,etc)) <br/> df = df.sort("A")``` |
| Data A2 | Data B2 | Data C2 |
