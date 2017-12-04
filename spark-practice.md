[cloudera@quickstart Downloads]$ sqoop import --connect jdbc:mysql://quickstart/loudacre --username root --password cloudera --table accounts --target-dir /user/cloudera/sqoop14




scala> r2.show(5)
+---+--------------------+----+------+--------+--------------------+-------------+---+-----+----------+--------------------+--------------------+
| nu|                 cdt| cls|    fn|      ln|                 add|           ct|ste|  zip|        ph|                 crd|                  md|
+---+--------------------+----+------+--------+--------------------+-------------+---+-----+----------+--------------------+--------------------+
|  1|2008-10-23 16:05:...|null|Donald|  Becton|2275 Washburn Street|      Oakland| CA|94660|5100032418|2014-03-18 13:29:...|2014-03-18 13:29:...|
|  2|2008-11-12 03:00:...|null| Donna|   Jones| 3885 Elliott Street|San Francisco| CA|94171|4150835799|2014-03-18 13:29:...|2014-03-18 13:29:...|
|  3|2008-12-21 09:19:...|null|Dorthy|Chalmers|    4073 Whaley Lane|    San Mateo| CA|94479|6506877757|2014-03-18 13:29:...|2014-03-18 13:29:...|
|  4|2008-11-28 00:08:...|null| Leila| Spencer|    1447 Ross Street|    San Mateo| CA|94444|6503198619|2014-03-18 13:29:...|2014-03-18 13:29:...|
|  5|2008-11-15 23:06:...|null| Anita|Laughlin|    2767 Hill Street|     Richmond| CA|94872|5107754354|2014-03-18 13:29:...|2014-03-18 13:29:...|
+---+--------------------+----+------+--------+--------------------+-------------+---+-----+----------+--------------------+--------------------+
only showing top 5 rows


scala> val nm = r2.select('fn, 'ln)
nm: org.apache.spark.sql.DataFrame = [fn: string, ln: string]

scala> nm.show(5)
+------+--------+
|    fn|      ln|
+------+--------+
|Donald|  Becton|
| Donna|   Jones|
|Dorthy|Chalmers|
| Leila| Spencer|
| Anita|Laughlin|
+------+--------+
only showing top 5 rows


scala> nm.count
res4: Long = 129761                                                             

scala> nm.distinct.count
res5: Long = 47120                                                              

scala> val nm1 = nm.distinct
nm1: org.apache.spark.sql.DataFrame = [fn: string, ln: string]

scala> nm1.sort
sort                   sortWithinPartitions   

scala> nm1.sort('ln).show(50)

scala> nm1.sort('ln).select('ln, 'fn).show(50)
+--------+---------+                                                            
|      ln|       fn|
+--------+---------+
|   Aaron|   Violet|
|   Aaron|    Willa|


scala> nm1.sort('ln).select(concat('ln, lit(" "), 'fn)).show(50)
+----------------+                                                              
| concat(ln, ,fn)|
+----------------+
|    Aaron Violet|
|     Aaron Willa|
|     Aaron Robin|



scala> val nm2 = nm1.sort('ln).select(concat('ln, lit(" "), 'fn))
nm2: org.apache.spark.sql.DataFrame = [concat(ln, ,fn): string]

scala> nm2.rdd.first
res12: org.apache.spark.sql.Row = [Aaron Robin]                                 

scala> nm2.rdd.map(row => row.getAs[String](0))
res13: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[64] at map at <console>:42

scala> nm2.rdd.map(row => row.getAs[String](0)).first
res14: String = Aaron Robin

scala> val nm21 = nm1.sort('ln).map(row => row.getAs[String](1) + " " + row.getAs[String](0))
nm21: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[80] at map at <console>:39

scala> nm21.first
res15: String = Aaron Robin                                                     

scala> 

scala> 

scala> r2.registerTempTable("acctbl")

scala> val df2 = sqlContext.sql("select * from acctbl")
df2: org.apache.spark.sql.DataFrame = [nu: int, cdt: string, cls: string, fn: string, ln: string, add: string, ct: string, ste: string, zip: int, ph: bigint, crd: string, md: string]

scala> df2.sort('cdt.desc).show(10)
+------+--------------------+----+--------+---------+--------------------+---------------+---+-----+----------+--------------------+--------------------+
|    nu|                 cdt| cls|      fn|       ln|                 add|             ct|ste|  zip|        ph|                 crd|                  md|
+------+--------------------+----+--------+---------+--------------------+---------------+---+-----+----------+--------------------+--------------------+
|129553|2014-03-14 23:48:...|null|   Jerry|   Rivera|   1088 Smith Street|       Berkeley| CA|94777|5107873261|2014-03-18 13:33:...|2014-03-18 13:33:...|
|127446|2014-03-14 23:43:...|null| Juanita| Mitchell|    474 Clover Drive|      San Diego| CA|92082|6194815153|2014-03-18 13:33:...|2014-03-18 13:33:...|
|124592|2014-03-14 23:12:...|null| Charles|   Manzer|  1364 Morgan Street|  Santa Barbara| CA|93163|8053104481|2014-03-18 13:33:...|2014-03-18 13:33:...|
|129502|2014-03-14 23:09:...|null|Virginia|  Wilburn|2243 Broaddus Avenue|         Mojave| CA|93573|6613774459|2014-03-18 13:33:...|2014-03-18 13:33:...|
|127493|2014-03-14 23:06:...|null| Kaitlyn|   Muller|1844 Tator Patch ...|       Van Nuys| CA|91466|8188257749|2014-03-18 13:33:...|2014-03-18 13:33:...|
|127583|2014-03-14 22:59:...|null|  Agatha|   Fuller|       3094 D Street|      Las Vegas| NV|89004|7022258169|2014-03-18 13:33:...|2014-03-18 13:33:...|
|122207|2014-03-14 22:30:...|null|  Sheryl|    Smith|       253 Selah Way|       Alhambra| CA|91895|6266400732|2014-03-18 13:33:...|2014-03-18 13:33:...|
|128005|2014-03-14 21:52:...|null| Theresa| Procopio|    4871 Five Points|    Los Angeles| CA|90143|2134060049|2014-03-18 13:33:...|2014-03-18 13:33:...|
|127575|2014-03-14 21:19:...|null|  Dennis|Vanhouten|    747 Grand Avenue|North Hollywood| CA|91657|8185105677|2014-03-18 13:33:...|2014-03-18 13:33:...|
|123724|2014-03-14 21:08:...|null|  Thomas|  Daniels| 1875 Dola Mine Road|           Elko| NV|89896|7752825009|2014-03-18 13:33:...|2014-03-18 13:33:...|
+------+--------------------+----+--------+---------+--------------------+---------------+---+-----+----------+--------------------+--------------------+
only showing top 10 rows


scala> df2.sort('cdt.desc).select('nu, 'cdt, 'fn, 'ln).show(10)
+------+--------------------+--------+---------+
|    nu|                 cdt|      fn|       ln|
+------+--------------------+--------+---------+
|129553|2014-03-14 23:48:...|   Jerry|   Rivera|
|127446|2014-03-14 23:43:...| Juanita| Mitchell|
|124592|2014-03-14 23:12:...| Charles|   Manzer|
|129502|2014-03-14 23:09:...|Virginia|  Wilburn|
|127493|2014-03-14 23:06:...| Kaitlyn|   Muller|
|127583|2014-03-14 22:59:...|  Agatha|   Fuller|
|122207|2014-03-14 22:30:...|  Sheryl|    Smith|
|128005|2014-03-14 21:52:...| Theresa| Procopio|
|127575|2014-03-14 21:19:...|  Dennis|Vanhouten|
|123724|2014-03-14 21:08:...|  Thomas|  Daniels|
+------+--------------------+--------+---------+
only showing top 10 rows


scala> df2.sort('cdt).select('nu, 'cdt, 'fn, 'ln).show(10)
+---+--------------------+--------+--------+
| nu|                 cdt|      fn|      ln|
+---+--------------------+--------+--------+
|405|2008-10-21 03:35:...|    Jose|    Kent|
|155|2008-10-21 15:38:...|   Jason|   Stone|
|269|2008-10-21 19:07:...|   Colin|Francois|
|194|2008-10-21 22:13:...|   Chris| Clayton|
| 89|2008-10-21 22:21:...|Kathleen| Trotter|
|374|2008-10-22 08:16:...|    Jean|Benjamin|
|146|2008-10-22 18:34:...|  Steven|  Rivers|
|474|2008-10-22 19:36:...| Kathryn|    Hurt|
|131|2008-10-22 21:42:...|   Renee|  Torres|
|204|2008-10-23 00:36:...|   Garry|    Ford|
+---+--------------------+--------+--------+
only showing top 10 rows


scala> 

scala> 

scala> 

scala> 

scala> 

scala> sqlContext.getConf("spark.sql.parquet.compression.codec")
res21: String = gzip


scala> r2.write.parquet("/user/cloudera/loudacre1")

scala> import com.databricks.spark.avro._
import com.databricks.spark.avro._

scala> sqlContext.setConf("spark.sql.avro.compression.codec", "snappy")

scala> 

scala> df2.write.avro("/user/cloudera/loudacre2")
                                                                                
scala> 

scala> sqlContext.setConf("spark.sql.avro.compression.codec", "uncompressed")

scala> 

scala> df2.write.avro("/user/cloudera/loudacre3")






[cloudera@quickstart ~]$ hdfs dfs -ls loudacre1
Found 7 items
-rw-r--r--   1 cloudera cloudera          0 2017-12-04 01:07 loudacre1/_SUCCESS
-rw-r--r--   1 cloudera cloudera       1032 2017-12-04 01:07 loudacre1/_common_metadata
-rw-r--r--   1 cloudera cloudera       7222 2017-12-04 01:07 loudacre1/_metadata
-rw-r--r--   1 cloudera cloudera     963201 2017-12-04 01:07 loudacre1/part-r-00000-09ad8099-068d-449b-8da3-7cd402723e71.gz.parquet
-rw-r--r--   1 cloudera cloudera     965107 2017-12-04 01:07 loudacre1/part-r-00001-09ad8099-068d-449b-8da3-7cd402723e71.gz.parquet
-rw-r--r--   1 cloudera cloudera     956036 2017-12-04 01:07 loudacre1/part-r-00002-09ad8099-068d-449b-8da3-7cd402723e71.gz.parquet
-rw-r--r--   1 cloudera cloudera     925881 2017-12-04 01:07 loudacre1/part-r-00003-09ad8099-068d-449b-8da3-7cd402723e71.gz.parquet
[cloudera@quickstart ~]$ 
[cloudera@quickstart ~]$ hdfs dfs -ls loudacre2
Found 5 items
-rw-r--r--   1 cloudera cloudera          0 2017-12-04 01:08 loudacre2/_SUCCESS
-rw-r--r--   1 cloudera cloudera    2233168 2017-12-04 01:08 loudacre2/part-r-00000-252ca4ba-ad23-491c-8bfd-39a396b687d2.avro
-rw-r--r--   1 cloudera cloudera    2240330 2017-12-04 01:08 loudacre2/part-r-00001-252ca4ba-ad23-491c-8bfd-39a396b687d2.avro
-rw-r--r--   1 cloudera cloudera    2219813 2017-12-04 01:08 loudacre2/part-r-00002-252ca4ba-ad23-491c-8bfd-39a396b687d2.avro
-rw-r--r--   1 cloudera cloudera    2164626 2017-12-04 01:08 loudacre2/part-r-00003-252ca4ba-ad23-491c-8bfd-39a396b687d2.avro
[cloudera@quickstart ~]$ 
[cloudera@quickstart ~]$ 
[cloudera@quickstart ~]$ 
[cloudera@quickstart ~]$ hdfs dfs -ls loudacre3
Found 5 items
-rw-r--r--   1 cloudera cloudera          0 2017-12-04 01:12 loudacre3/_SUCCESS
-rw-r--r--   1 cloudera cloudera    4751908 2017-12-04 01:12 loudacre3/part-r-00000-0a636b84-30cc-4851-bf23-1d687c94cf22.avro
-rw-r--r--   1 cloudera cloudera    4737366 2017-12-04 01:12 loudacre3/part-r-00001-0a636b84-30cc-4851-bf23-1d687c94cf22.avro
-rw-r--r--   1 cloudera cloudera    4720383 2017-12-04 01:12 loudacre3/part-r-00002-0a636b84-30cc-4851-bf23-1d687c94cf22.avro
-rw-r--r--   1 cloudera cloudera    4683184 2017-12-04 01:12 loudacre3/part-r-00003-0a636b84-30cc-4851-bf23-1d687c94cf22.avro
