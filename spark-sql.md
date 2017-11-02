## created a df

```
scala> case class Token(name: String, productId: Int, score: Double)
defined class Token

scala> val data = Seq(
     |   Token("aaa", 100, 0.12),
     |   Token("aaa", 200, 0.29),
     |   Token("bbb", 200, 0.53),
     |   Token("bbb", 300, 0.42))
data: Seq[Token] = List(Token(aaa,100,0.12), Token(aaa,200,0.29), Token(bbb,200,0.53), Token(bbb,300,0.42))

scala> val tokens = data.toDS.cache
tokens: org.apache.spark.sql.Dataset[Token] = [name: string, productId: int, score: double]

scala> tokens.show
+----+---------+-----+
|name|productId|score|
+----+---------+-----+
| bbb|      200| 0.53|
| bbb|      300| 0.42|
| aaa|      100| 0.12|
| aaa|      200| 0.29|
+----+---------+-----+

```

## rank

```
scala> :paste
// Entering paste mode (ctrl-D to finish)

val customers = sc.parallelize(List(("Alice", "2016-05-01", 50.00),
                                    ("Alice", "2016-05-03", 45.00),
                                    ("Alice", "2016-05-04", 55.00),
                                    ("Bob", "2016-05-01", 25.00),
                                    ("Bob", "2016-05-04", 29.00),
                                    ("Bob", "2016-05-06", 27.00))).
                               toDF("name", "date", "amountSpent")

// Exiting paste mode, now interpreting.

customers: org.apache.spark.sql.DataFrame = [name: string, date: string, amountSpent: double]

scala> customers.show
+-----+----------+-----------+
| name|      date|amountSpent|
+-----+----------+-----------+
|Alice|2016-05-01|       50.0|
|Alice|2016-05-03|       45.0|
|Alice|2016-05-04|       55.0|
|  Bob|2016-05-01|       25.0|
|  Bob|2016-05-04|       29.0|
|  Bob|2016-05-06|       27.0|
+-----+----------+-----------+

scala> import org.apache.spark.sql.expressions.Window
import org.apache.spark.sql.expressions.Window

scala> val wSpec3 = Window.partitionBy("name").orderBy("date")
wSpec3: org.apache.spark.sql.expressions.WindowSpec = org.apache.spark.sql.expressions.WindowSpec@2ba4f4fe

scala> customers.withColumn("rank", rank().over(wSpec3)).show
+-----+----------+-----------+----+
| name|      date|amountSpent|rank|
+-----+----------+-----------+----+
|Alice|2016-05-01|       50.0|   1|
|Alice|2016-05-03|       45.0|   2|
|Alice|2016-05-04|       55.0|   3|
|  Bob|2016-05-01|       25.0|   1|
|  Bob|2016-05-04|       29.0|   2|
|  Bob|2016-05-06|       27.0|   3|
+-----+----------+-----------+----+


scala>

scala> val wSpec4 = Window.partitionBy("name").orderBy("amountSpent")
wSpec4: org.apache.spark.sql.expressions.WindowSpec = org.apache.spark.sql.expressions.WindowSpec@48771635

scala> customers.withColumn("rank", rank().over(wSpec4)).show
+-----+----------+-----------+----+
| name|      date|amountSpent|rank|
+-----+----------+-----------+----+
|Alice|2016-05-03|       45.0|   1|
|Alice|2016-05-01|       50.0|   2|
|Alice|2016-05-04|       55.0|   3|
|  Bob|2016-05-01|       25.0|   1|
|  Bob|2016-05-06|       27.0|   2|
|  Bob|2016-05-04|       29.0|   3|
+-----+----------+-----------+----+

scala> val rdd = sc.parallelize(List(
     |     (0, "Anna", "female", 172),
     |     (1, "Bert", "male", 182),
     |     (2, "Curt", "male", 170),
     |     (3, "Doris", "female", 164),
     |     (4, "Edna", "female", 171),
     |     (5, "Felix", "male", 160)
     | ))
rdd: org.apache.spark.rdd.RDD[(Int, String, String, Int)] = ParallelCollectionRDD[28] at parallelize at <console>:28

scala> rdd.take(6).foreach(println)
(0,Anna,female,172)
(1,Bert,male,182)
(2,Curt,male,170)
(3,Doris,female,164)
(4,Edna,female,171)
(5,Felix,male,160)

scala> val df = rdd.toDF("id", "name", "sex", "height")
df: org.apache.spark.sql.DataFrame = [id: int, name: string, sex: string, height: int]

scala> df.select('name, 'height).show
+-----+------+
| name|height|
+-----+------+
| Anna|   172|
| Bert|   182|
| Curt|   170|
|Doris|   164|
| Edna|   171|
|Felix|   160|
+-----+------+

scala> df.select('name, 'height, rank().over(Window.orderBy('height)).alias("rank1")).show
17/10/30 14:40:49 WARN execution.Window: No Partition Defined for Window operation! Moving all data to a single partition, this can cause serious performance degradation.
+-----+------+-----+
| name|height|rank1|
+-----+------+-----+
|Felix|   160|    1|
|Doris|   164|    2|
| Curt|   170|    3|
| Edna|   171|    4|
| Anna|   172|    5|
| Bert|   182|    6|
+-----+------+-----+


scala> df.select('name, 'height, rank().over(Window.orderBy('height.desc)).alias("rank1")).show
17/10/30 14:41:16 WARN execution.Window: No Partition Defined for Window operation! Moving all data to a single partition, this can cause serious performance degradation.
+-----+------+-----+
| name|height|rank1|
+-----+------+-----+
| Bert|   182|    1|
| Anna|   172|    2|
| Edna|   171|    3|
| Curt|   170|    4|
|Doris|   164|    5|
|Felix|   160|    6|
+-----+------+-----+


scala> df.select('name, 'sex, 'height, rank().over(Window.partitionBy('sex).orderBy('height.desc)).alias("rank2")).show
+-----+------+------+-----+
| name|   sex|height|rank2|
+-----+------+------+-----+
| Anna|female|   172|    1|
| Edna|female|   171|    2|
|Doris|female|   164|    3|
| Bert|  male|   182|    1|
| Curt|  male|   170|    2|
|Felix|  male|   160|    3|
+-----+------+------+-----+


scala> df.select('name, 'sex, 'height, rank().over(Window.partitionBy('sex).orderBy('height)).alias("rank3")).show
+-----+------+------+-----+
| name|   sex|height|rank3|
+-----+------+------+-----+
|Doris|female|   164|    1|
| Edna|female|   171|    2|
| Anna|female|   172|    3|
|Felix|  male|   160|    1|
| Curt|  male|   170|    2|
| Bert|  male|   182|    3|
+-----+------+------+-----+

scala> df.sort('height).show
+---+-----+------+------+
| id| name|   sex|height|
+---+-----+------+------+
|  5|Felix|  male|   160|
|  3|Doris|female|   164|
|  2| Curt|  male|   170|
|  4| Edna|female|   171|
|  0| Anna|female|   172|
|  1| Bert|  male|   182|
+---+-----+------+------+


scala> df.sort('height.desc).show
+---+-----+------+------+
| id| name|   sex|height|
+---+-----+------+------+
|  1| Bert|  male|   182|
|  0| Anna|female|   172|
|  4| Edna|female|   171|
|  2| Curt|  male|   170|
|  3|Doris|female|   164|
|  5|Felix|  male|   160|
+---+-----+------+------+


scala> df.sort('id.desc).show
+---+-----+------+------+
| id| name|   sex|height|
+---+-----+------+------+
|  5|Felix|  male|   160|
|  4| Edna|female|   171|
|  3|Doris|female|   164|
|  2| Curt|  male|   170|
|  1| Bert|  male|   182|
|  0| Anna|female|   172|
+---+-----+------+------+


scala> df.sort('name.desc).show
+---+-----+------+------+
| id| name|   sex|height|
+---+-----+------+------+
|  5|Felix|  male|   160|
|  4| Edna|female|   171|
|  3|Doris|female|   164|
|  2| Curt|  male|   170|
|  1| Bert|  male|   182|
|  0| Anna|female|   172|
+---+-----+------+------+


scala> df.sort('name).show
+---+-----+------+------+
| id| name|   sex|height|
+---+-----+------+------+
|  0| Anna|female|   172|
|  1| Bert|  male|   182|
|  2| Curt|  male|   170|
|  3|Doris|female|   164|
|  4| Edna|female|   171|
|  5|Felix|  male|   160|
+---+-----+------+------+


scala> df.orderBy('height).show
+---+-----+------+------+
| id| name|   sex|height|
+---+-----+------+------+
|  5|Felix|  male|   160|
|  3|Doris|female|   164|
|  2| Curt|  male|   170|
|  4| Edna|female|   171|
|  0| Anna|female|   172|
|  1| Bert|  male|   182|
+---+-----+------+------+


scala> df.orderBy('height.desc).show
+---+-----+------+------+
| id| name|   sex|height|
+---+-----+------+------+
|  1| Bert|  male|   182|
|  0| Anna|female|   172|
|  4| Edna|female|   171|
|  2| Curt|  male|   170|
|  3|Doris|female|   164|
|  5|Felix|  male|   160|
+---+-----+------+------+
```
## groupBy

```
scala> df.groupBy('sex).count.show
+------+-----+
|   sex|count|
+------+-----+
|female|    3|
|  male|    3|
+------+-----+

scala> df.groupBy('sex).agg(sum('height)).show
+------+-----------+
|   sex|sum(height)|
+------+-----------+
|female|        507|
|  male|        512|
+------+-----------+

scala> df.groupBy('sex).agg(max('height)).show
+------+-----------+
|   sex|max(height)|
+------+-----------+
|female|        172|
|  male|        182|
+------+-----------+

scala> df.groupBy('sex).agg(min('height)).show
+------+-----------+
|   sex|min(height)|
+------+-----------+
|female|        164|
|  male|        160|
+------+-----------+

scala> df.groupBy('sex).agg(mean('height)).show
+------+------------------+
|   sex|       avg(height)|
+------+------------------+
|female|             169.0|
|  male|170.66666666666666|
+------+------------------+

scala> df.groupBy('sex).agg(avg('height)).show
+------+------------------+
|   sex|       avg(height)|
+------+------------------+
|female|             169.0|
|  male|170.66666666666666|
+------+------------------+

scala> df.groupBy('sex).agg(count('height)).show
+------+-------------+
|   sex|count(height)|
+------+-------------+
|female|            3|
|  male|            3|
+------+-------------+

```
