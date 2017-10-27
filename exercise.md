##

```
scala> val r1 = sc.textFile("file:///home/ec2-user/patients.csv")
r1: org.apache.spark.rdd.RDD[String] = file:///home/ec2-user/patients.csv MapPartitionsRDD[31] at textFile at <console>:28

scala> r1.count

Caused by: java.io.FileNotFoundException: File file:/home/ec2-user/patients.csv does not exist
```

```
scala> val r1 = sc.textFile("/loudacre/patients.csv")
r1: org.apache.spark.rdd.RDD[String] = /loudacre/patients.csv MapPartitionsRDD[35] at textFile at <console>:28

scala> r1.take(4).foreach(println)
patientID,name,dateOfBirth,lastVisitDate
1001,Ah Teck,1991-12-31,2012-01-20
1002,Kumar,2011-01-30,2012-09-20
1003,Ali,2011-01-30,2012-10-21

scala> case class Patient(patientID: Int,name: String,dateOfBirth: String,lastVisitDate: String)
defined class Patient

scala> val pat = r1.filter(s => !s.startsWith("p")).map(_.split(",")).map(p => Patient(p(0).toInt, p(1), p(2), p(3)))
pat: org.apache.spark.rdd.RDD[Patient] = MapPartitionsRDD[40] at map at <console>:32


scala> patdf.printSchema
root
 |-- patientID: integer (nullable = false)
 |-- name: string (nullable = true)
 |-- dateOfBirth: string (nullable = true)
 |-- lastVisitDate: string (nullable = true)


scala>

scala> patdf.registerTempTable("ps")

scala> val res = sqlContext.sql("select * from ps")
res: org.apache.spark.sql.DataFrame = [patientID: int, name: string, dateOfBirth: string, lastVisitDate: string]

scala> res.show
+---------+-------+-----------+-------------+
|patientID|   name|dateOfBirth|lastVisitDate|
+---------+-------+-----------+-------------+
|     1001|Ah Teck| 1991-12-31|   2012-01-20|
|     1002|  Kumar| 2011-01-30|   2012-09-20|
|     1003|    Ali| 2011-01-30|   2012-10-21|
+---------+-------+-----------+-------------+

scala> sqlContext.sql("select * from ps where TO_DATE(CAST(UNIX_TIMESTAMP(lastVisitDate, 'yyyy-MM-dd') AS TIMESTAMP)) between '2012-09-15' and current_timestamp() order by lastVisitDate")
res29: org.apache.spark.sql.DataFrame = [patientID: int, name: string, dateOfBirth: string, lastVisitDate: string]

scala> res29.show
+---------+-----+-----------+-------------+
|patientID| name|dateOfBirth|lastVisitDate|
+---------+-----+-----------+-------------+
|     1002|Kumar| 2011-01-30|   2012-09-20|
|     1003|  Ali| 2011-01-30|   2012-10-21|
+---------+-----+-----------+-------------+

scala> sqlContext.sql("select name, dateOfBirth, datediff(current_date(), TO_DATE(CAST(UNIX_TIMESTAMP(dateOfBirth, 'yyyy-MM-dd') AS TIMESTAMP)))/365 as age from ps ").show
+-------+-----------+------------------+
|   name|dateOfBirth|               age|
+-------+-----------+------------------+
|Ah Teck| 1991-12-31|25.838356164383562|
|  Kumar| 2011-01-30| 6.742465753424658|
|    Ali| 2011-01-30| 6.742465753424658|
+-------+-----------+------------------+


scala>

scala> sqlContext.sql("select * from ps where TO_DATE(CAST(UNIX_TIMESTAMP(dateOfBirth, 'yyyy-MM-dd') AS TIMESTAMP)) > date_sub(current_date(), 18*365) ").show
+---------+-----+-----------+-------------+
|patientID| name|dateOfBirth|lastVisitDate|
+---------+-----+-----------+-------------+
|     1002|Kumar| 2011-01-30|   2012-09-20|
|     1003|  Ali| 2011-01-30|   2012-10-21|
+---------+-----+-----------+-------------+


scala> sqlContext.sql("select * from ps where TO_DATE(CAST(UNIX_TIMESTAMP(dateOfBirth, 'yyyy-MM-dd') AS TIMESTAMP)) < date_sub(current_date(), 18*365) ").show
+---------+-------+-----------+-------------+
|patientID|   name|dateOfBirth|lastVisitDate|
+---------+-------+-----------+-------------+
|     1001|Ah Teck| 1991-12-31|   2012-01-20|
+---------+-------+-----------+-------------+
```


##

```
scala> val keysWithValuesList = Array("foo=A", "foo=A", "foo=A", "foo=A", "foo=B", "bar=C", "bar=D", "bar=D")
keysWithValuesList: Array[String] = Array(foo=A, foo=A, foo=A, foo=A, foo=B, bar=C, bar=D, bar=D)

scala> val data = sc.parallelize(keysWithValuesList)
data: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[0] at parallelize at <console>:29

scala> data.first
res0: String = foo=A

scala> val kv = data.map(_.split("=")).map(v => (v(0), v(1)))
kv: org.apache.spark.rdd.RDD[(String, String)] = MapPartitionsRDD[3] at map at <console>:31

scala> kv.first
res2: (String, String) = (foo,A)

scala> val initCount = 0
initCount: Int = 0

scala> val addToCounts = (n: Int, v: String) => n + 1
addToCounts: (Int, String) => Int = <function2>

scala> val sumPartitionCounts = (p1: Int, p2: Int) => p1 + p2
sumPartitionCounts: (Int, Int) => Int = <function2>

scala> val countByKey = kv.aggregateByKey(initCount)(addToCounts, sumPartitionCounts)
countByKey: org.apache.spark.rdd.RDD[(String, Int)] = ShuffledRDD[4] at aggregateByKey at <console>:39

scala> countByKey.collect
res3: Array[(String, Int)] = Array((foo,5), (bar,3))

scala> import scala.collection._
import scala.collection._

scala> val initSet = scala.collection.mutable.HashSet.empty[String]
initSet: scala.collection.mutable.HashSet[String] = Set()

scala> val addToSet = (s: mutable.HashSet[String], v: String) => s += v
addToSet: (scala.collection.mutable.HashSet[String], String) => scala.collection.mutable.HashSet[String] = <function2>

scala> val mergePartitionSets = (p1: mutable.HashSet[String], p2: mutable.HashSet[String]) => p1 ++= p2
mergePartitionSets: (scala.collection.mutable.HashSet[String], scala.collection.mutable.HashSet[String]) => scala.collection.mutable.HashSet[String] = <function2>

scala> val uniqueByKey = kv.aggregateByKey(initSet)(addToSet, mergePartitionSets)
uniqueByKey: org.apache.spark.rdd.RDD[(String, scala.collection.mutable.HashSet[String])] = ShuffledRDD[5] at aggregateByKey at <console>:42

scala> uniqueByKey.collect
res4: Array[(String, scala.collection.mutable.HashSet[String])] = Array((foo,Set(B, A)), (bar,Set(C, D)))
```
