sqlContext.sql("create table if not exists mytab (key int, value string) row format delimited fields terminated by \",\" ")
sqlContext.sql("create table if not exists mytab (key int, value string) row format delimited fields terminated by ',' ")

sqlContext.sql("load data local inpath '/home/cloudera/test.txt' overwrite into table mytab ")

sqlContext.sql("drop table mytab")


val vr = sc.parallelize(List(1, 2, 4, 8, 16))

val kvr = sc.parallelize(List((1950, 0), (1950, 22), (1950, -11), (1949, 111), (1949, 78)))


val v2 = vr.map{case v => math.log10(v.toDouble)/math.log10(2.0)}

```
scala> :paste
// Entering paste mode (ctrl-D to finish)

val raw = sc.parallelize(List(
("Thin", "Cell Phone", 6000),
  ("Normal", "Tablet", 1500),
  ("Mini", "Tablet", 5500),
  ("Ultra thin", "Cell Phone", 5500),
  ("Very thin", "Cell Phone", 6000),
  ("Big", "Tablet", 2500),
  ("Bendable", "Cell Phone", 3000),
  ("Foldable", "Cell Phone", 3000),
  ("Pro", "Tablet", 4500),
  ("Pro2", "Tablet", 6500)
))

// Exiting paste mode, now interpreting.

raw: org.apache.spark.rdd.RDD[(String, String, Int)] = ParallelCollectionRDD[0] at parallelize at <console>:27


scala> case class DFS(product: String, category: String, revenue: Int)
defined class DFS

scala> val df = raw.map{case (p, c, r) => DFS(p, c, r)}.toDF
df: org.apache.spark.sql.DataFrame = [product: string, category: string, revenue: int]

scala> df.show
+----------+----------+-------+
|   product|  category|revenue|
+----------+----------+-------+
|      Thin|Cell Phone|   6000|
|    Normal|    Tablet|   1500|
|      Mini|    Tablet|   5500|
|Ultra thin|Cell Phone|   5500|
| Very thin|Cell Phone|   6000|
|       Big|    Tablet|   2500|
|  Bendable|Cell Phone|   3000|
|  Foldable|Cell Phone|   3000|
|       Pro|    Tablet|   4500|
|      Pro2|    Tablet|   6500|
+----------+----------+-------+


scala> import org.apache.spark.sql.expressions.Window
import org.apache.spark.sql.expressions.Window


scala> val w = Window.partitionBy('category).orderBy('revenue.desc)
w: org.apache.spark.sql.expressions.WindowSpec = org.apache.spark.sql.expressions.WindowSpec@26c1ad

scala> df.select($"product", $"category", $"revenue", dense_rank.over(w).alias("rank")).show
+----------+----------+-------+----+                                            
|   product|  category|revenue|rank|
+----------+----------+-------+----+
|      Pro2|    Tablet|   6500|   1|
|      Mini|    Tablet|   5500|   2|
|       Pro|    Tablet|   4500|   3|
|       Big|    Tablet|   2500|   4|
|    Normal|    Tablet|   1500|   5|
|      Thin|Cell Phone|   6000|   1|
| Very thin|Cell Phone|   6000|   1|
|Ultra thin|Cell Phone|   5500|   2|
|  Bendable|Cell Phone|   3000|   3|
|  Foldable|Cell Phone|   3000|   3|
+----------+----------+-------+----+

scala> df.select($"product", $"category", $"revenue", rank.over(w).alias("rank")).show
+----------+----------+-------+----+                                            
|   product|  category|revenue|rank|
+----------+----------+-------+----+
|      Pro2|    Tablet|   6500|   1|
|      Mini|    Tablet|   5500|   2|
|       Pro|    Tablet|   4500|   3|
|       Big|    Tablet|   2500|   4|
|    Normal|    Tablet|   1500|   5|
|      Thin|Cell Phone|   6000|   1|
| Very thin|Cell Phone|   6000|   1|
|Ultra thin|Cell Phone|   5500|   3|
|  Bendable|Cell Phone|   3000|   4|
|  Foldable|Cell Phone|   3000|   4|
+----------+----------+-------+----+


val myrdd = sc.parallelize(List(
  (0,"Alex",30,123),
  (1,"Bert",32,234),
  (2,"Curt",28,312),
  (3,"Don",32,89)
  ))


```

```

scala> :paste
// Entering paste mode (ctrl-D to finish)

val myrdd = sc.parallelize(List(
  (0,"Alex",30,123),
  (1,"Bert",32,234),
  (2,"Curt",28,312),
  (3,"Don",32,89)
  ))

// Exiting paste mode, now interpreting.

myrdd: org.apache.spark.rdd.RDD[(Int, String, Int, Int)] = ParallelCollectionRDD[72] at parallelize at <console>:28

scala> myrdd.collect
res18: Array[(Int, String, Int, Int)] = Array((0,Alex,30,123), (1,Bert,32,234), (2,Curt,28,312), (3,Don,32,89))

scala> case class CC(no: Int, nm: String, age: Int, fd: Int)
defined class CC

scala> val mydf = myrdd.map{case (n, nm, ag, fd) => CC(n, nm, ag, fd)}.toDF
mydf: org.apache.spark.sql.DataFrame = [no: int, nm: string, age: int, fd: int]

scala> mydf.show
+---+----+---+---+
| no|  nm|age| fd|
+---+----+---+---+
|  0|Alex| 30|123|
|  1|Bert| 32|234|
|  2|Curt| 28|312|
|  3| Don| 32| 89|
+---+----+---+---+


scala> case class HB(nu: Int, hb: String)
defined class HB

scala> val hobbies = sc.parallelize(List((0, "writing"), (0, "gym"), (1, "swimming"))).map{case (n, h) => HB(n, h)}.toDF
hobbies: org.apache.spark.sql.DataFrame = [nu: int, hb: string]

scala> hobbies.show
+---+--------+
| nu|      hb|
+---+--------+
|  0| writing|
|  0|     gym|
|  1|swimming|
+---+--------+



```


```

scala> hobbies.join(mydf, $"nu" === $"no").show
+---+--------+---+----+---+---+                                                 
| nu|      hb| no|  nm|age| fd|
+---+--------+---+----+---+---+
|  0| writing|  0|Alex| 30|123|
|  0|     gym|  0|Alex| 30|123|
|  1|swimming|  1|Bert| 32|234|
+---+--------+---+----+---+---+


scala>

scala> case class CC1(nu: Int, nm: String, age: Int, fd: Int)
defined class CC1

scala> val mydf1 = myrdd.map{case (n, nm, ag, fd) => CC1(n, nm, ag, fd)}.toDF
mydf1: org.apache.spark.sql.DataFrame = [nu: int, nm: string, age: int, fd: int]

scala> mydf1.show
+---+----+---+---+
| nu|  nm|age| fd|
+---+----+---+---+
|  0|Alex| 30|123|
|  1|Bert| 32|234|
|  2|Curt| 28|312|
|  3| Don| 32| 89|
+---+----+---+---+


scala> mydf1.join(hobbies, "nu").show
+---+----+---+---+--------+                                                     
| nu|  nm|age| fd|      hb|
+---+----+---+---+--------+
|  0|Alex| 30|123| writing|
|  0|Alex| 30|123|     gym|
|  1|Bert| 32|234|swimming|
+---+----+---+---+--------+


scala> mydf1.join(hobbies, "nu").explain
== Physical Plan ==
Project [nu#19,nm#20,age#21,fd#22,hb#18]
+- SortMergeJoin [nu#19], [nu#17]
   :- Sort [nu#19 ASC], false, 0
   :  +- TungstenExchange hashpartitioning(nu#19,200), None
   :     +- ConvertToUnsafe
   :        +- Scan ExistingRDD[nu#19,nm#20,age#21,fd#22]
   +- Sort [nu#17 ASC], false, 0
      +- TungstenExchange hashpartitioning(nu#17,200), None
         +- ConvertToUnsafe
            +- Scan ExistingRDD[nu#17,hb#18]


scala> mydf1.join(hobbies, Seq("nu"), "fullouter").show
+---+----+---+---+--------+                                                     
| nu|  nm|age| fd|      hb|
+---+----+---+---+--------+
|  0|Alex| 30|123| writing|
|  0|Alex| 30|123|     gym|
|  1|Bert| 32|234|swimming|
|  2|Curt| 28|312|    null|
|  3| Don| 32| 89|    null|
+---+----+---+---+--------+


scala> mydf1.join(hobbies, Seq("nu"), "left").show
+---+----+---+---+--------+                                                     
| nu|  nm|age| fd|      hb|
+---+----+---+---+--------+
|  0|Alex| 30|123| writing|
|  0|Alex| 30|123|     gym|
|  1|Bert| 32|234|swimming|
|  2|Curt| 28|312|    null|
|  3| Don| 32| 89|    null|
+---+----+---+---+--------+


scala>

scala> mydf1.join(hobbies, Seq("nu"), "right").show
+---+----+---+---+--------+                                                     
| nu|  nm|age| fd|      hb|
+---+----+---+---+--------+
|  0|Alex| 30|123| writing|
|  0|Alex| 30|123|     gym|
|  1|Bert| 32|234|swimming|
+---+----+---+---+--------+


```

```
val rdd = sc.parallelize(List(
    (0, "Anna", "female", 172),
    (1, "Bert", "male", 182),
    (2, "Curt", "male", 170),
    (3, "Doris", "female", 164),
    (4, "Edna", "female", 171),
    (5, "Felix", "male", 160)
))

scala> :paste
// Entering paste mode (ctrl-D to finish)

val rdd = sc.parallelize(List(
    (0, "Anna", "female", 172),
    (1, "Bert", "male", 182),
    (2, "Curt", "male", 170),
    (3, "Doris", "female", 164),
    (4, "Edna", "female", 171),
    (5, "Felix", "male", 160)
))

// Exiting paste mode, now interpreting.

rdd: org.apache.spark.rdd.RDD[(Int, String, String, Int)] = ParallelCollectionRDD[143] at parallelize at <console>:28

scala>

scala> rdd.collect
res40: Array[(Int, String, String, Int)] = Array((0,Anna,female,172), (1,Bert,male,182), (2,Curt,male,170), (3,Doris,female,164), (4,Edna,female,171), (5,Felix,male,160))

scala> case class Record(ID: Int, name: String, sex: String, height: Int)
defined class Record

scala> val df = rdd.map{case (id, nm, sx, ht) => Record(id, nm, sx, ht)}.toDF
df: org.apache.spark.sql.DataFrame = [ID: int, name: string, sex: string, height: int]

scala> df.show
+---+-----+------+------+
| ID| name|   sex|height|
+---+-----+------+------+
|  0| Anna|female|   172|
|  1| Bert|  male|   182|
|  2| Curt|  male|   170|
|  3|Doris|female|   164|
|  4| Edna|female|   171|
|  5|Felix|  male|   160|
+---+-----+------+------+


scala> df.select('name, 'height, rank().over(Window.orderBy('height)).alias("rank1")).show
17/11/01 18:13:21 WARN execution.Window: No Partition Defined for Window operation! Moving all data to a single partition, this can cause serious performance degradation.
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


scala>

scala> df.select('name, 'height, rank().over(Window.orderBy('height.desc)).alias("rank1")).show
17/11/01 18:13:59 WARN execution.Window: No Partition Defined for Window operation! Moving all data to a single partition, this can cause serious performance degradation.
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


scala>

scala> df.select('name, 'height, rank().over(Window.partitionBy('sex).orderBy('height)).alias("rank2")).show
+-----+------+-----+                                                            
| name|height|rank2|
+-----+------+-----+
|Doris|   164|    1|
| Edna|   171|    2|
| Anna|   172|    3|
|Felix|   160|    1|
| Curt|   170|    2|
| Bert|   182|    3|
+-----+------+-----+


scala> df.select('name, 'sex, 'height, rank().over(Window.partitionBy('sex).orderBy('height)).alias("rank2")).show
+-----+------+------+-----+                                                     
| name|   sex|height|rank2|
+-----+------+------+-----+
|Doris|female|   164|    1|
| Edna|female|   171|    2|
| Anna|female|   172|    3|
|Felix|  male|   160|    1|
| Curt|  male|   170|    2|
| Bert|  male|   182|    3|
+-----+------+------+-----+


scala>

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



```


```

scala> df.show
+---+-----+------+------+
| ID| name|   sex|height|
+---+-----+------+------+
|  0| Anna|female|   172|
|  1| Bert|  male|   182|
|  2| Curt|  male|   170|
|  3|Doris|female|   164|
|  4| Edna|female|   171|
|  5|Felix|  male|   160|
+---+-----+------+------+


scala> import com.databricks.spark.avro._
import com.databricks.spark.avro._

scala> sqlContext.setConf("spark.sql.avro.compression.codec", "snappy")

scala> sqlContext.getConf("spark.sql.avro.compression.codec")
res56: String = snappy

scala> df.write.avro("avrotest")

scala> val df1 = sqlContext.read.avro("avrotest").show
+---+-----+------+------+
| ID| name|   sex|height|
+---+-----+------+------+
|  0| Anna|female|   172|
|  1| Bert|  male|   182|
|  2| Curt|  male|   170|
|  3|Doris|female|   164|
|  4| Edna|female|   171|
|  5|Felix|  male|   160|
+---+-----+------+------+

df1: Unit = ()

```

http://arun-teaches-u-tech.blogspot.co.nz/p/file-formats.html



## Quick reference table for reading and writing into several file formats in hdfs.

|File Format 	|Action 	|Procedure and points to remember|
| --------|---------|-------|
|TEXT FILE 	|READ 	|sparkContext.textFile(`<path to file>`);|
||WRITE 	|sparkContext.saveAsTextFile(<path to file>,classOf[compressionCodecClass]);
|||//use any codec here org.apache.hadoop.io.compress.(BZip2Codec or GZipCodec or SnappyCodec)|
|SEQUENCE FILE 	|READ 	|sparkContext.sequenceFile(<path location>,classOf[<class name>],classOf[<compressionCodecClass >]);|
|||//read the head of sequence file to understand what two class names need to be used here|
||WRITE 	|rdd.saveAsSequenceFile(<path location>, Some(classOf[compressionCodecClass]))|
|||//use any codec here (BZip2Codec,GZipCodec,SnappyCodec)
|||//here rdd is MapPartitionRDD and not the regular pair RDD.
|PARQUET FILE 	|READ 	|//use data frame to load the file.
|||sqlContext.read.parquet(<path to location>); //this results in a data frame object.
||WRITE 	|sqlContext.setConf("spark.sql.parquet.compression.codec","gzip") //use gzip, snappy, lzo or uncompressed here
|||dataFrame.write.parquet(<path to location>);
|ORC FILE 	|READ 	|sqlContext.read.orc(<path to location>); //this results in a dataframe
||WRITE 	|df.write.mode(SaveMode.Overwrite).format("orc") .save(<path to location>)
|AVRO FILE 	|READ 	|import com.databricks.spark.avro._;
|||sqlContext.read.avro(<path to location>); // this results in a data frame object
||WRITE 	|sqlContext.setConf("spark.sql.avro.compression.codec","snappy") //use snappy, deflate, uncompressed;
|||dataFrame.write.avro(<path to location>);
|JSON FILE 	|READ 	|sqlContext.read.json();
||WRITE 	|dataFrame.toJSON().saveAsTextFile(<path to location>,classOf[Compression Codec])|



##

[cloudera@quickstart ~]$ sqoop import --connect jdbc:mysql://quickstart/cm --username root --password cloudera --table USERS --target-dir /user/cloudera/sqoop5 --as-avrodatafile --compression-codec org.apache.hadoop.io.compress.GzipCodec
Error: org.apache.avro.AvroRuntimeException: Unrecognized codec: gzip

##

sqoop import --connect jdbc:mysql://quickstart/cm --username root --password cloudera --table USERS --target-dir /user/cloudera/sqoop

hdfs dfs -ls sqoop
hdfs dfs -cat sqoop/part-m-00000

sqoop import --connect jdbc:mysql://quickstart/cm --username root --password cloudera --table USERS --target-dir /user/cloudera/sqoop1 --fields-terminated-by "\t"
hdfs dfs -ls sqoop1
hdfs dfs -cat sqoop1/part-m-00000

sqoop import --connect jdbc:mysql://quickstart/cm --username root --password cloudera --table USERS --target-dir /user/cloudera/sqoop2 --as-parquetfile
hdfs dfs -ls sqoop2

parquet-tools head hdfs://quickstart/user/cloudera/sqoop2/0d762369-3646-4484-9ec1-b702fff1d764.parquet
parquet-tools schema hdfs://quickstart/user/cloudera/sqoop2/0d762369-3646-4484-9ec1-b702fff1d764.parquet
parquet-tools meta hdfs://quickstart/user/cloudera/sqoop2/0d762369-3646-4484-9ec1-b702fff1d764.parquet

sqoop import --connect jdbc:mysql://quickstart/cm --username root --password cloudera --table USERS --target-dir /user/cloudera/sqoop3 --as-parquetfile --compression-codec org.apache.hadoop.io.compress.SnappyCodec

parqeut-tools schema hdfs://quickstart/user/cloudera/sqoop3/277a1c62-97fc-48e9-8703-5e3d75e8baff.parquet

parquet-tools meta hdfs://quickstart/user/cloudera/sqoop3/277a1c62-97fc-48e9-8703-5e3d75e8baff.parquet
parquet-tools meta hdfs://quickstart/user/cloudera/sqoop2/0d762369-3646-4484-9ec1-b702fff1d764.parquet

sqoop import --connect jdbc:mysql://quickstart/cm --username root --password cloudera --table USERS --target-dir /user/cloudera/sqoop4 --as-parquetfile --compression-codec org.apache.hadoop.io.compress.GzipCodec

cloudera@quickstart ~]$ hdfs dfs -ls sqoop4
Found 6 items
drwxr-xr-x   - cloudera cloudera          0 2017-11-02 11:53 sqoop4/.metadata
drwxr-xr-x   - cloudera cloudera          0 2017-11-02 11:53 sqoop4/.signals
-rw-r--r--   1 cloudera cloudera       2077 2017-11-02 11:53 sqoop4/8ae1c639-fe0b-45ef-b390-25ea89e8ec0c.parquet
-rw-r--r--   1 cloudera cloudera       2307 2017-11-02 11:53 sqoop4/b2389ee7-ac0b-49e4-9903-912971ee5227.parquet
-rw-r--r--   1 cloudera cloudera       2273 2017-11-02 11:53 sqoop4/ec8c2dc7-9ab7-430a-8900-662c34bf8397.parquet
-rw-r--r--   1 cloudera cloudera       2277 2017-11-02 11:53 sqoop4/eccc51ce-2c2d-4baa-b92b-32b87b617a3e.parquet
[cloudera@quickstart ~]$
[cloudera@quickstart ~]$
[cloudera@quickstart ~]$ parquet-tools meta hdfs://quickstart/user/cloudera/sqoop4/8ae1c639-fe0b-45ef-b390-25ea89e8ec0c.parquet
creator:                 parquet-mr version 1.5.0-cdh5.12.0 (build ${buildNumber})
extra:                   parquet.avro.schema = {"type":"record","name":"USERS","doc":"Sqoop import of USERS","fields":[{"name":"USER_ID","type":["null" [more]...

file schema:             USERS
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
USER_ID:                 OPTIONAL INT64 R:0 D:1
USER_NAME:               OPTIONAL BINARY O:UTF8 R:0 D:1
PASSWORD_HASH:           OPTIONAL BINARY O:UTF8 R:0 D:1
PASSWORD_SALT:           OPTIONAL INT64 R:0 D:1
PASSWORD_LOGIN:          OPTIONAL BOOLEAN R:0 D:1
OPTIMISTIC_LOCK_VERSION: OPTIONAL INT64 R:0 D:1

row group 1:             RC:2 TS:585
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
USER_ID:                  INT64 SNAPPY DO:0 FPO:4 SZ:65/63/0.97 VC:2 ENC:PLAIN,BIT_PACKED,RLE
USER_NAME:                BINARY SNAPPY DO:0 FPO:69 SZ:67/65/0.97 VC:2 ENC:PLAIN,BIT_PACKED,RLE
PASSWORD_HASH:            BINARY SNAPPY DO:0 FPO:136 SZ:301/297/0.99 VC:2 ENC:PLAIN,BIT_PACKED,RLE
PASSWORD_SALT:            INT64 SNAPPY DO:0 FPO:437 SZ:65/63/0.97 VC:2 ENC:PLAIN,BIT_PACKED,RLE
PASSWORD_LOGIN:           BOOLEAN SNAPPY DO:0 FPO:502 SZ:36/34/0.94 VC:2 ENC:PLAIN,BIT_PACKED,RLE
OPTIMISTIC_LOCK_VERSION:  INT64 SNAPPY DO:0 FPO:538 SZ:64/63/0.98 VC:2 ENC:PLAIN,BIT_PACKED,RLE
[cloudera@quickstart ~]$
[cloudera@quickstart ~]$ parquet-tools head hdfs://quickstart/user/cloudera/sqoop4/8ae1c639-fe0b-45ef-b390-25ea89e8ec0c.parquet
USER_ID = 1
USER_NAME = admin
PASSWORD_HASH = df559cbcb008277c166f0df9baa551e834d55ce08faf1dacdeda46f53e9367b8
PASSWORD_SALT = 2940308216918769267
PASSWORD_LOGIN = true
OPTIMISTIC_LOCK_VERSION = 2

USER_ID = 2
USER_NAME = cloudera
PASSWORD_HASH = d552e45ab348cc0600c3721f4bc72dc265081bcc9e8e6b0c71fdd6d91e591680
PASSWORD_SALT = -5340757984488052789
PASSWORD_LOGIN = true
OPTIMISTIC_LOCK_VERSION = 1



##

[cloudera@quickstart ~]$ sqoop import --connect jdbc:mysql://quickstart/cm --username root --password cloudera --table USERS --target-dir /user/cloudera/sqoop5 --as-avrodatafile --compression-codec org.apache.hadoop.io.compress.SnappyCodec

```
[cloudera@quickstart ~]$ hdfs dfs -ls sqoop5
Found 5 items
-rw-r--r--   1 cloudera cloudera          0 2017-11-02 11:56 sqoop5/_SUCCESS
-rw-r--r--   1 cloudera cloudera       1009 2017-11-02 11:56 sqoop5/part-m-00000.avro
-rw-r--r--   1 cloudera cloudera       1052 2017-11-02 11:56 sqoop5/part-m-00001.avro
-rw-r--r--   1 cloudera cloudera       1064 2017-11-02 11:56 sqoop5/part-m-00002.avro
-rw-r--r--   1 cloudera cloudera       1054 2017-11-02 11:56 sqoop5/part-m-00003.avro
```

[cloudera@quickstart ~]$ avro-tools --help
Version 1.7.6-cdh5.12.0 of Apache Avro
Copyright 2010 The Apache Software Foundation

This product includes software developed at
The Apache Software Foundation (http://www.apache.org/).

C JSON parsing provided by Jansson and
written by Petri Lehtinen. The original software is
available from http://www.digip.org/jansson/.
----------------
Available tools:
          cat  extracts samples from files
      compile  Generates Java code for the given schema.
       concat  Concatenates avro files without re-compressing.
   fragtojson  Renders a binary-encoded Avro datum as JSON.
     fromjson  Reads JSON records and writes an Avro data file.
     fromtext  Imports a text file into an avro data file.
      getmeta  Prints out the metadata of an Avro data file.
    getschema  Prints out schema of an Avro data file.
          idl  Generates a JSON schema from an Avro IDL file
 idl2schemata  Extract JSON schemata of the types from an Avro IDL file
       induce  Induce schema/protocol from Java class/interface via reflection.
   jsontofrag  Renders a JSON-encoded Avro datum as binary.
       random  Creates a file with randomly generated instances of a schema.
      recodec  Alters the codec of a data file.
       repair  Recovers data from a corrupt Avro Data file
  rpcprotocol  Output the protocol of a RPC service
   rpcreceive  Opens an RPC Server and listens for one message.
      rpcsend  Sends a single RPC message.
       tether  Run a tethered mapreduce job.
       tojson  Dumps an Avro data file as JSON, record per line or pretty.
       totext  Converts an Avro data file to a text file.
     totrevni  Converts an Avro data file to a Trevni file.
  trevni_meta  Dumps a Trevni file's metadata as JSON.
trevni_random  Create a Trevni file filled with random instances of a schema.
trevni_tojson  Dumps a Trevni file as JSON.


[cloudera@quickstart ~]$ avro-tools totext hdfs://quickstart/user/cloudera/sqoop5/part-m-00000.avro aout.txt
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
Avro file is not generic text schema
No options specified


[cloudera@quickstart ~]$ avro-tools getmeta hdfs://quickstart/user/cloudera/sqoop5/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
avro.codec	snappy
avro.schema	{"type":"record","name":"USERS","doc":"Sqoop import of USERS","fields":[{"name":"USER_ID","type":["null","long"],"default":null,"columnName":"USER_ID","sqlType":"-5"},{"name":"USER_NAME","type":["null","string"],"default":null,"columnName":"USER_NAME","sqlType":"12"},{"name":"PASSWORD_HASH","type":["null","string"],"default":null,"columnName":"PASSWORD_HASH","sqlType":"12"},{"name":"PASSWORD_SALT","type":["null","long"],"default":null,"columnName":"PASSWORD_SALT","sqlType":"-5"},{"name":"PASSWORD_LOGIN","type":["null","boolean"],"default":null,"columnName":"PASSWORD_LOGIN","sqlType":"-7"},{"name":"OPTIMISTIC_LOCK_VERSION","type":["null","long"],"default":null,"columnName":"OPTIMISTIC_LOCK_VERSION","sqlType":"-5"}],"tableName":"USERS"}


[cloudera@quickstart ~]$ avro-tools getschema hdfs://quickstart/user/cloudera/sqoop5/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "type" : "record",
  "name" : "USERS",
  "doc" : "Sqoop import of USERS",
  "fields" : [ {
    "name" : "USER_ID",
    "type" : [ "null", "long" ],
    "default" : null,
    "columnName" : "USER_ID",
    "sqlType" : "-5"
  }, {
    "name" : "USER_NAME",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "USER_NAME",
    "sqlType" : "12"
  }, {
    "name" : "PASSWORD_HASH",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "PASSWORD_HASH",
    "sqlType" : "12"
  }, {
    "name" : "PASSWORD_SALT",
    "type" : [ "null", "long" ],
    "default" : null,
    "columnName" : "PASSWORD_SALT",
    "sqlType" : "-5"
  }, {
    "name" : "PASSWORD_LOGIN",
    "type" : [ "null", "boolean" ],
    "default" : null,
    "columnName" : "PASSWORD_LOGIN",
    "sqlType" : "-7"
  }, {
    "name" : "OPTIMISTIC_LOCK_VERSION",
    "type" : [ "null", "long" ],
    "default" : null,
    "columnName" : "OPTIMISTIC_LOCK_VERSION",
    "sqlType" : "-5"
  } ],
  "tableName" : "USERS"
}

[cloudera@quickstart ~]$ avro-tools tojson hdfs://quickstart/user/cloudera/sqoop5/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{"USER_ID":{"long":1},"USER_NAME":{"string":"admin"},"PASSWORD_HASH":{"string":"df559cbcb008277c166f0df9baa551e834d55ce08faf1dacdeda46f53e9367b8"},"PASSWORD_SALT":{"long":2940308216918769267},"PASSWORD_LOGIN":{"boolean":true},"OPTIMISTIC_LOCK_VERSION":{"long":2}}
{"USER_ID":{"long":2},"USER_NAME":{"string":"cloudera"},"PASSWORD_HASH":{"string":"d552e45ab348cc0600c3721f4bc72dc265081bcc9e8e6b0c71fdd6d91e591680"},"PASSWORD_SALT":{"long":-5340757984488052789},"PASSWORD_LOGIN":{"boolean":true},"OPTIMISTIC_LOCK_VERSION":{"long":1}}


[cloudera@quickstart ~]$ avro-tools tojson hdfs://quickstart/user/cloudera/sqoop5/part-m-00000.avro --pretty
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "USER_ID" : {
    "long" : 1
  },
  "USER_NAME" : {
    "string" : "admin"
  },
  "PASSWORD_HASH" : {
    "string" : "df559cbcb008277c166f0df9baa551e834d55ce08faf1dacdeda46f53e9367b8"
  },
  "PASSWORD_SALT" : {
    "long" : 2940308216918769267
  },
  "PASSWORD_LOGIN" : {
    "boolean" : true
  },
  "OPTIMISTIC_LOCK_VERSION" : {
    "long" : 2
  }
}
{
  "USER_ID" : {
    "long" : 2
  },
  "USER_NAME" : {
    "string" : "cloudera"
  },
  "PASSWORD_HASH" : {
    "string" : "d552e45ab348cc0600c3721f4bc72dc265081bcc9e8e6b0c71fdd6d91e591680"
  },
  "PASSWORD_SALT" : {
    "long" : -5340757984488052789
  },
  "PASSWORD_LOGIN" : {
    "boolean" : true
  },
  "OPTIMISTIC_LOCK_VERSION" : {
    "long" : 1
  }
}
[cloudera@quickstart ~]$
