```python
if df1.count() > 0 and df2.count() > 0:
    # do something

if df1.first() and df2.first() > 0:
    # do something

if not df1.isEmpty() and not df2.isEmpty(): # Spark 3.3.0
    # do something
```

Spark 개발자는 위의 마법이 physical plane에서 실제로 어떻게 실행되는지 이해하려고 노력할 필요가 있다. 사실상 여기에서 개발자의 수준이 나뉘는 것 같다. DataFrame API로 ETL 작업만 했는지 성능 최적화의 단계까지 나아갔는지 말이다. 


```python

# https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html

df.columns

df.filter(df.age > 3)
df.filter("age = 2")

df.groupBy('name').agg({'age': 'mean'})
df.groupBy(df.name).avg()

df.join(df2, ['name', 'age'], 'outer')

df.coalease(1) # <- numPartitions

df.isEmpty() # -> bool

```