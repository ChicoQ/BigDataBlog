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
