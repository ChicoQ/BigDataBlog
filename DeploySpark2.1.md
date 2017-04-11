## Deploy the Latest Spark 2.1 on CDH

You can use the [editor on GitHub](https://github.com/ChicoQ/BigDataBlog/edit/master/DeploySpark2.1.md) to maintain and preview the content for your website in Markdown files.

I had followed this article [Running Spark 2.x.x on Cloudera Hadoop Distro (CDH)](https://www.linkedin.com/pulse/running-spark-2xx-cloudera-hadoop-distro-cdh-deenar-toraskar-cfa) and made some changes.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

#### Check the version of CDH and Hadoop running on your cluster using

```makedown
# hadoop version
Hadoop 2.6.0-cdh5.7.0
```

#### Download Spark's source code from [Spark] (http://spark.apache.org/downloads.html)

#### Compile

```markdown
-Phive -Phive-thriftserver -Pyarn
```

#### Change SPARK_HOME

#### Change configuration

```markdown

```

#### Copying Spark floder to all nodes

```
# scp
```

#### Finally test your new Spark

```markdown
./bin/run-example SparkP 10 --master yarn
./bin/spark-shell --master yarn
./bin/pyspark

```

### Known Issue

#### Cannot Load Hive Table into Spark

Hive version is 1.1.0-cdh5.7.0

```markdown
# cd /usr/lib/spark-2.1.0/
[root@dataapp01 spark-2.1.0]# bin/spark-shell
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
java.lang.IllegalArgumentException: Error while instantiating 'org.apache.spark.sql.hive.HiveSessionState':
  at org.apache.spark.sql.SparkSession$.org$apache$spark$sql$SparkSession$$reflect(SparkSession.scala:981)
  at org.apache.spark.sql.SparkSession.sessionState$lzycompute(SparkSession.scala:110)
  at org.apache.spark.sql.SparkSession.sessionState(SparkSession.scala:109)
  at org.apache.spark.sql.SparkSession$Builder$$anonfun$getOrCreate$5.apply(SparkSession.scala:878)
  at org.apache.spark.sql.SparkSession$Builder$$anonfun$getOrCreate$5.apply(SparkSession.scala:878)
  at scala.collection.mutable.HashMap$$anonfun$foreach$1.apply(HashMap.scala:99)
  at scala.collection.mutable.HashMap$$anonfun$foreach$1.apply(HashMap.scala:99)
  at scala.collection.mutable.HashTable$class.foreachEntry(HashTable.scala:230)
  at scala.collection.mutable.HashMap.foreachEntry(HashMap.scala:40)
  at scala.collection.mutable.HashMap.foreach(HashMap.scala:99)
  at org.apache.spark.sql.SparkSession$Builder.getOrCreate(SparkSession.scala:878)
  at org.apache.spark.repl.Main$.createSparkSession(Main.scala:95)
  ... 47 elided
Caused by: java.lang.reflect.InvocationTargetException: java.lang.IllegalArgumentException: Error while instantiating 'org.apache.spark.sql.hive.HiveExternalCatalog':
  at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
  at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
  at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
  at java.lang.reflect.Constructor.newInstance(Constructor.java:422)
  at org.apache.spark.sql.SparkSession$.org$apache$spark$sql$SparkSession$$reflect(SparkSession.scala:978)
  ... 58 more
Caused by: java.lang.IllegalArgumentException: Error while instantiating 'org.apache.spark.sql.hive.HiveExternalCatalog':
  at org.apache.spark.sql.internal.SharedState$.org$apache$spark$sql$internal$SharedState$$reflect(SharedState.scala:169)
  at org.apache.spark.sql.internal.SharedState.<init>(SharedState.scala:86)
  at org.apache.spark.sql.SparkSession$$anonfun$sharedState$1.apply(SparkSession.scala:101)
  at org.apache.spark.sql.SparkSession$$anonfun$sharedState$1.apply(SparkSession.scala:101)
  at scala.Option.getOrElse(Option.scala:121)
  at org.apache.spark.sql.SparkSession.sharedState$lzycompute(SparkSession.scala:101)
  at org.apache.spark.sql.SparkSession.sharedState(SparkSession.scala:100)
  at org.apache.spark.sql.internal.SessionState.<init>(SessionState.scala:157)
  at org.apache.spark.sql.hive.HiveSessionState.<init>(HiveSessionState.scala:32)
  ... 63 more
Caused by: java.lang.reflect.InvocationTargetException: java.lang.NoSuchFieldError: METASTORE_CLIENT_SOCKET_LIFETIME
  at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
  at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
  at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
  at java.lang.reflect.Constructor.newInstance(Constructor.java:422)
  at org.apache.spark.sql.internal.SharedState$.org$apache$spark$sql$internal$SharedState$$reflect(SharedState.scala:166)
  ... 71 more
Caused by: java.lang.NoSuchFieldError: METASTORE_CLIENT_SOCKET_LIFETIME
  at org.apache.spark.sql.hive.HiveUtils$.hiveClientConfigurations(HiveUtils.scala:194)
  at org.apache.spark.sql.hive.HiveUtils$.newClientForMetadata(HiveUtils.scala:269)
  at org.apache.spark.sql.hive.HiveExternalCatalog.<init>(HiveExternalCatalog.scala:65)
  ... 76 more
<console>:14: error: not found: value spark
       import spark.implicits._
              ^
<console>:14: error: not found: value spark
       import spark.sql
              ^
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.1.0
      /_/

Using Scala version 2.11.8 (OpenJDK 64-Bit Server VM, Java 1.8.0_65)
Type in expressions to have them evaluated.
Type :help for more information.
```
##### Rebuild Spark and Add CDH Dependencies

```markdown
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal on project spark-launcher_2.11: Could not resolve dependencies for project org.apache.spark:spark-launcher_2.11:jar:2.1.0: Could not find artifact org.apache.hadoop:hadoop-client:jar:2.6.0-cdh5.7.0 in central (https://repo1.maven.org/maven2) -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/DependencyResolutionException
[ERROR]
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <goals> -rf :spark-launcher_2.11


# vim pom.xml

<repositories>
  <repository>
    <id>central</id>
    <!-- This should be at top, it makes maven try the central repo first and then others and hence faster dep resolution -->
    <name>Maven Repository</name>
    <url>https://repo1.maven.org/maven2</url>
    <releases>
      <enabled>true</enabled>
    </releases>
    <snapshots>
      <enabled>false</enabled>
    </snapshots>
  </repository>
  <repository>
    <id>cloudera-repo</id>
    <name>Cloudera Repository</name>
    <url>https://repository.cloudera.com/artifactory/cloudera-repos</url>
  </repository>
</repositories>

```

But there was the same issue.

```markdown
# bin/spark-shell --master yarn --queue root.default
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/lib/spark-2.1.0/jars/slf4j-log4j12-1.7.16.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/lib/zookeeper/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
17/04/03 14:48:16 WARN spark.SparkContext: Support for Java 7 is deprecated as of Spark 2.0.0
java.lang.IllegalArgumentException: Error while instantiating 'org.apache.spark.sql.hive.HiveSessionState':
  at org.apache.spark.sql.SparkSession$.org$apache$spark$sql$SparkSession$$reflect(SparkSession.scala:981)
  at org.apache.spark.sql.SparkSession.sessionState$lzycompute(SparkSession.scala:110)
  at org.apache.spark.sql.SparkSession.sessionState(SparkSession.scala:109)
  at org.apache.spark.sql.SparkSession$Builder$$anonfun$getOrCreate$5.apply(SparkSession.scala:878)
  at org.apache.spark.sql.SparkSession$Builder$$anonfun$getOrCreate$5.apply(SparkSession.scala:878)
  at scala.collection.mutable.HashMap$$anonfun$foreach$1.apply(HashMap.scala:99)
  at scala.collection.mutable.HashMap$$anonfun$foreach$1.apply(HashMap.scala:99)
  at scala.collection.mutable.HashTable$class.foreachEntry(HashTable.scala:230)
  at scala.collection.mutable.HashMap.foreachEntry(HashMap.scala:40)
  at scala.collection.mutable.HashMap.foreach(HashMap.scala:99)
  at org.apache.spark.sql.SparkSession$Builder.getOrCreate(SparkSession.scala:878)
  at org.apache.spark.repl.Main$.createSparkSession(Main.scala:95)
  ... 47 elided
Caused by: java.lang.reflect.InvocationTargetException: java.lang.IllegalArgumentException: Error while instantiating 'org.apache.spark.sql.hive.HiveExternalCatalog':
  at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
  at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
  at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
  at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
  at org.apache.spark.sql.SparkSession$.org$apache$spark$sql$SparkSession$$reflect(SparkSession.scala:978)
  ... 58 more
Caused by: java.lang.IllegalArgumentException: Error while instantiating 'org.apache.spark.sql.hive.HiveExternalCatalog':
  at org.apache.spark.sql.internal.SharedState$.org$apache$spark$sql$internal$SharedState$$reflect(SharedState.scala:169)
  at org.apache.spark.sql.internal.SharedState.<init>(SharedState.scala:86)
  at org.apache.spark.sql.SparkSession$$anonfun$sharedState$1.apply(SparkSession.scala:101)
  at org.apache.spark.sql.SparkSession$$anonfun$sharedState$1.apply(SparkSession.scala:101)
  at scala.Option.getOrElse(Option.scala:121)
  at org.apache.spark.sql.SparkSession.sharedState$lzycompute(SparkSession.scala:101)
  at org.apache.spark.sql.SparkSession.sharedState(SparkSession.scala:100)
  at org.apache.spark.sql.internal.SessionState.<init>(SessionState.scala:157)
  at org.apache.spark.sql.hive.HiveSessionState.<init>(HiveSessionState.scala:32)
  ... 63 more
Caused by: java.lang.reflect.InvocationTargetException: scala.MatchError: 1.1.0-cdh5.7.0 (of class java.lang.String)
  at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
  at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
  at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
  at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
  at org.apache.spark.sql.internal.SharedState$.org$apache$spark$sql$internal$SharedState$$reflect(SharedState.scala:166)
  ... 71 more
Caused by: scala.MatchError: 1.1.0-cdh5.7.0 (of class java.lang.String)
  at org.apache.spark.sql.hive.client.IsolatedClientLoader$.hiveVersion(IsolatedClientLoader.scala:93)
  at org.apache.spark.sql.hive.HiveUtils$.newClientForMetadata(HiveUtils.scala:283)
  at org.apache.spark.sql.hive.HiveUtils$.newClientForMetadata(HiveUtils.scala:270)
  at org.apache.spark.sql.hive.HiveExternalCatalog.<init>(HiveExternalCatalog.scala:65)
  ... 76 more
<console>:14: error: not found: value spark
       import spark.implicits._
              ^
<console>:14: error: not found: value spark
       import spark.sql
              ^
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.1.0
      /_/

Using Scala version 2.11.8 (OpenJDK 64-Bit Server VM, Java 1.7.0_111)
Type in expressions to have them evaluated.
Type :help for more information.

```

Even though I add this dedicated Hive version into Spark code.

```markdown
# cd sql/hive/src/main/scala/org/apache/spark/sql/hive/client/
# vim IsolatedClientLoader.scala

def hiveVersion(version: String): HiveVersion = version match {
  case "12" | "0.12" | "0.12.0" => hive.v12
  case "13" | "0.13" | "0.13.0" | "0.13.1" => hive.v13
  case "14" | "0.14" | "0.14.0" => hive.v14
  case "1.0" | "1.0.0" => hive.v1_0
  ## case "1.1" | "1.1.0" => hive.v1_1
  case "1.1" | "1.1.0" | "1.1.0-cdh5.7.0" => hive.v1_1
  case "1.2" | "1.2.0" | "1.2.1" => hive.v1_2
}

# ./build/mvn -Phadoop-2.6 -Pyarn -Phive -Phive-thriftserver -Dhadoop.version=2.6.0-cdh5.7.0 -DskipTests clean package
```

```markdown
scala> import org.apache.spark.sql.SparkSession
scala> val spark = SparkSession.builder().enableHiveSupport()
spark: org.apache.spark.sql.SparkSession.Builder = org.apache.spark.sql.SparkSession$Builder@3752722e

scala> spark.config("spark.sql.warehouse.dir", "/data/warehouse/")
scala> spark.enableHiveSupport
res2: org.apache.spark.sql.SparkSession.Builder = org.apache.spark.sql.SparkSession$Builder@3752722e
scala> spark.master("yarn")
res3: org.apache.spark.sql.SparkSession.Builder = org.apache.spark.sql.SparkSession$Builder@3752722e

scala> import org.apache.spark.sql.hive._
import org.apache.spark.sql.hive._
scala> val hc = new org.apache.spark.sql.hive.HiveContext(sc)
warning: there was one deprecation warning; re-run with -deprecation for details
hc: org.apache.spark.sql.hive.HiveContext = org.apache.spark.sql.hive.HiveContext@22ec2cd9


scala> hc.sql("SELECT COUNT(*) FROM backups.ibms_schedule_dp_20_mar").show(5)
[Stage 0:>                                                          (0 + 1) / 2]17/04/03 15:45:50 WARN scheduler.TaskSetManager: Lost task 0.0 in stage 0.0 (TID 0, datalake03, executor 1): java.lang.NoSuchMethodError: org.apache.hadoop.hive.ql.exec.Utilities.copyTableJobPropertiesToConf(Lorg/apache/hadoop/hive/ql/plan/TableDesc;Lorg/apache/hadoop/conf/Configuration;)V
	at org.apache.spark.sql.hive.HadoopTableReader$.initializeLocalJobConfFunc(TableReader.scala:341)
	at org.apache.spark.sql.hive.HadoopTableReader$$anonfun$12.apply(TableReader.scala:292)
	at org.apache.spark.sql.hive.HadoopTableReader$$anonfun$12.apply(TableReader.scala:292)
	at org.apache.spark.rdd.HadoopRDD$$anonfun$getJobConf$6.apply(HadoopRDD.scala:179)
	at org.apache.spark.rdd.HadoopRDD$$anonfun$getJobConf$6.apply(HadoopRDD.scala:179)
	at scala.Option.foreach(Option.scala:257)
	at org.apache.spark.rdd.HadoopRDD.getJobConf(HadoopRDD.scala:179)
	at org.apache.spark.rdd.HadoopRDD$$anon$1.<init>(HadoopRDD.scala:215)
	at org.apache.spark.rdd.HadoopRDD.compute(HadoopRDD.scala:211)
	at org.apache.spark.rdd.HadoopRDD.compute(HadoopRDD.scala:102)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:96)
	at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:53)
	at org.apache.spark.scheduler.Task.run(Task.scala:99)
	at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:282)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
	at java.lang.Thread.run(Thread.java:745)

[Stage 0:>                                                          (0 + 2) / 2]17/04/03 15:45:50 ERROR scheduler.TaskSetManager: Task 0 in stage 0.0 failed 4 times; aborting job
17/04/03 15:45:50 WARN spark.ExecutorAllocationManager: No stages are running, but numRunningTasks != 0
org.apache.spark.SparkException: Job aborted due to stage failure: Task 0 in stage 0.0 failed 4 times, most recent failure: Lost task 0.3 in stage 0.0 (TID 6, datalake03, executor 1): java.lang.NoSuchMethodError: org.apache.hadoop.hive.ql.exec.Utilities.copyTableJobPropertiesToConf(Lorg/apache/hadoop/hive/ql/plan/TableDesc;Lorg/apache/hadoop/conf/Configuration;)V
	at org.apache.spark.sql.hive.HadoopTableReader$.initializeLocalJobConfFunc(TableReader.scala:341)
	at org.apache.spark.sql.hive.HadoopTableReader$$anonfun$12.apply(TableReader.scala:292)
	at org.apache.spark.sql.hive.HadoopTableReader$$anonfun$12.apply(TableReader.scala:292)
	at org.apache.spark.rdd.HadoopRDD$$anonfun$getJobConf$6.apply(HadoopRDD.scala:179)
	at org.apache.spark.rdd.HadoopRDD$$anonfun$getJobConf$6.apply(HadoopRDD.scala:179)
	at scala.Option.foreach(Option.scala:257)
	at org.apache.spark.rdd.HadoopRDD.getJobConf(HadoopRDD.scala:179)
	at org.apache.spark.rdd.HadoopRDD$$anon$1.<init>(HadoopRDD.scala:215)
	at org.apache.spark.rdd.HadoopRDD.compute(HadoopRDD.scala:211)
	at org.apache.spark.rdd.HadoopRDD.compute(HadoopRDD.scala:102)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
	at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
	at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
	at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:96)
	at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:53)
	at org.apache.spark.scheduler.Task.run(Task.scala:99)
	at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:282)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
	at java.lang.Thread.run(Thread.java:745)

Driver stacktrace:
  at org.apache.spark.scheduler.DAGScheduler.org$apache$spark$scheduler$DAGScheduler$$failJobAndIndependentStages(DAGScheduler.scala:1435)
  at org.apache.spark.scheduler.DAGScheduler$$anonfun$abortStage$1.apply(DAGScheduler.scala:1423)
  at org.apache.spark.scheduler.DAGScheduler$$anonfun$abortStage$1.apply(DAGScheduler.scala:1422)
  at scala.collection.mutable.ResizableArray$class.foreach(ResizableArray.scala:59)
  at scala.collection.mutable.ArrayBuffer.foreach(ArrayBuffer.scala:48)
  at org.apache.spark.scheduler.DAGScheduler.abortStage(DAGScheduler.scala:1422)
  at org.apache.spark.scheduler.DAGScheduler$$anonfun$handleTaskSetFailed$1.apply(DAGScheduler.scala:802)
  at org.apache.spark.scheduler.DAGScheduler$$anonfun$handleTaskSetFailed$1.apply(DAGScheduler.scala:802)
  at scala.Option.foreach(Option.scala:257)
  at org.apache.spark.scheduler.DAGScheduler.handleTaskSetFailed(DAGScheduler.scala:802)
  at org.apache.spark.scheduler.DAGSchedulerEventProcessLoop.doOnReceive(DAGScheduler.scala:1650)
  at org.apache.spark.scheduler.DAGSchedulerEventProcessLoop.onReceive(DAGScheduler.scala:1605)
  at org.apache.spark.scheduler.DAGSchedulerEventProcessLoop.onReceive(DAGScheduler.scala:1594)
  at org.apache.spark.util.EventLoop$$anon$1.run(EventLoop.scala:48)
  at org.apache.spark.scheduler.DAGScheduler.runJob(DAGScheduler.scala:628)
  at org.apache.spark.SparkContext.runJob(SparkContext.scala:1918)
  at org.apache.spark.SparkContext.runJob(SparkContext.scala:1931)
  at org.apache.spark.SparkContext.runJob(SparkContext.scala:1944)
  at org.apache.spark.sql.execution.SparkPlan.executeTake(SparkPlan.scala:333)
  at org.apache.spark.sql.execution.CollectLimitExec.executeCollect(limit.scala:38)
  at org.apache.spark.sql.Dataset$$anonfun$org$apache$spark$sql$Dataset$$execute$1$1.apply(Dataset.scala:2371)
  at org.apache.spark.sql.execution.SQLExecution$.withNewExecutionId(SQLExecution.scala:57)
  at org.apache.spark.sql.Dataset.withNewExecutionId(Dataset.scala:2765)
  at org.apache.spark.sql.Dataset.org$apache$spark$sql$Dataset$$execute$1(Dataset.scala:2370)
  at org.apache.spark.sql.Dataset.org$apache$spark$sql$Dataset$$collect(Dataset.scala:2377)
  at org.apache.spark.sql.Dataset$$anonfun$head$1.apply(Dataset.scala:2113)
  at org.apache.spark.sql.Dataset$$anonfun$head$1.apply(Dataset.scala:2112)
  at org.apache.spark.sql.Dataset.withTypedCallback(Dataset.scala:2795)
  at org.apache.spark.sql.Dataset.head(Dataset.scala:2112)
  at org.apache.spark.sql.Dataset.take(Dataset.scala:2327)
  at org.apache.spark.sql.Dataset.showString(Dataset.scala:248)
  at org.apache.spark.sql.Dataset.show(Dataset.scala:636)
  at org.apache.spark.sql.Dataset.show(Dataset.scala:595)
  ... 50 elided
Caused by: java.lang.NoSuchMethodError: org.apache.hadoop.hive.ql.exec.Utilities.copyTableJobPropertiesToConf(Lorg/apache/hadoop/hive/ql/plan/TableDesc;Lorg/apache/hadoop/conf/Configuration;)V
  at org.apache.spark.sql.hive.HadoopTableReader$.initializeLocalJobConfFunc(TableReader.scala:341)
  at org.apache.spark.sql.hive.HadoopTableReader$$anonfun$12.apply(TableReader.scala:292)
  at org.apache.spark.sql.hive.HadoopTableReader$$anonfun$12.apply(TableReader.scala:292)
  at org.apache.spark.rdd.HadoopRDD$$anonfun$getJobConf$6.apply(HadoopRDD.scala:179)
  at org.apache.spark.rdd.HadoopRDD$$anonfun$getJobConf$6.apply(HadoopRDD.scala:179)
  at scala.Option.foreach(Option.scala:257)
  at org.apache.spark.rdd.HadoopRDD.getJobConf(HadoopRDD.scala:179)
  at org.apache.spark.rdd.HadoopRDD$$anon$1.<init>(HadoopRDD.scala:215)
  at org.apache.spark.rdd.HadoopRDD.compute(HadoopRDD.scala:211)
  at org.apache.spark.rdd.HadoopRDD.compute(HadoopRDD.scala:102)
  at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
  at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
  at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
  at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
  at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
  at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
  at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
  at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
  at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
  at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
  at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
  at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
  at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
  at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
  at org.apache.spark.rdd.MapPartitionsRDD.compute(MapPartitionsRDD.scala:38)
  at org.apache.spark.rdd.RDD.computeOrReadCheckpoint(RDD.scala:323)
  at org.apache.spark.rdd.RDD.iterator(RDD.scala:287)
  at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:96)
  at org.apache.spark.scheduler.ShuffleMapTask.runTask(ShuffleMapTask.scala:53)
  at org.apache.spark.scheduler.Task.run(Task.scala:99)
  at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:282)
  at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
  at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
  at java.lang.Thread.run(Thread.java:745)

```

### Conclusion

I suppose that the latest Spark 2.1.0 did not support the older Hive 1.1.0-cdh5.7.0.  So let me wait the official Spark 2.1.0 from Cloudera.

===
```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ChicoQ/BigDataBlog/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
