```
spark-submit \
--class org.apache.spark.examples.SparkPi \
--driver-java-options=-XX:+PrintGCTimeStamps -XX:+PrintGCDetails \
/opt/cloudera/parcels/CDH/lib/spark/examples/lib/spark-examples-1.6.0-cdh5.11.2-hadoop2.6.0-cdh5.11.2.jar \
10

spark-submit \
--class org.apache.spark.examples.SparkPi \
--conf "spark.driver.extraJavaOptions=-XX:+PrintGCTimeStamps -XX:+PrintGCDetails" \
--conf "spark.executor.extraJavaOptions=-XX:+PrintGCTimeStamps -XX:+PrintGCDetails" \
/opt/cloudera/parcels/CDH/lib/spark/examples/lib/spark-examples-1.6.0-cdh5.11.2-hadoop2.6.0-cdh5.11.2.jar \
10
```
