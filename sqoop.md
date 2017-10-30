[ec2-user@ip-172-29-1-12 ~]$ sqoop import-all-tables \
--connect jdbc:mysql://172.29.1.12/test \
--username root \
--password #### \
--hive-import \
--create-hive-table \
--hive-database accounts \
--compression-codec org.apache.hadoop.io.compress.SnappyCodec \
--target-dir /loudacre/accounts_hive

```
Warning: /opt/cloudera/parcels/CDH-5.11.2-1.cdh5.11.2.p0.4/bin/../lib/sqoop/../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
17/10/26 16:29:34 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.11.2
17/10/26 16:29:34 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
17/10/26 16:29:34 ERROR tool.BaseSqoopTool: Error parsing arguments for import-all-tables:
17/10/26 16:29:34 ERROR tool.BaseSqoopTool: Unrecognized argument: --target-dir
17/10/26 16:29:34 ERROR tool.BaseSqoopTool: Unrecognized argument: /loudacre/accounts_hive
```

[ec2-user@ip-172-29-1-12 ~]$ sqoop import-all-tables \
--connect jdbc:mysql://172.29.1.12/test \
--username root \
--password #### \
--hive-import \
--create-hive-table \
--hive-database testhive \
--compression-codec org.apache.hadoop.io.compress.SnappyCodec \
--warehouse-dir /loudacre/accounts_hive

## It was not necessary to set 'warehouse-dir' because the final data was stored into /user/hive/warehouse/testhive.db automatically.

## Sqoop first imports the data to a temporary location in HDFS.  It then generates a query for creating a table and also anohter query for loading the data from the temporary HDFS location, using the Hive `load data inpath` statement to move the data into the Hive warehouse directory in HDFS.  You can specify the temporary location with either the `--target-dir` parameter or the `--warehouse-dir` parameter.

##

- By default, sqoop stores the data in the user's home directory with the same name as the imported table.
- You can specify an alternative directory with the `--target-dir` parameter.
- Make sure the directory you specify with the `--target-dir` parameter doesn't already exist in HDFS.  Otherwise sqoop will throw an error, just as Hadoop does when a MapReduce output directory is already present.
- If you don't want to use your HDFS home directory, you can specify that all sqoop imports go into a special directory with the parameter `--warehouse-dir`.  Sqoop will then load the imported data under the directory you specify for the `--warehouse-dir` parameter, in a subdirectory named the same as the imported table.  Make sure you don't use the Hive warehouse directory, which is `/user/hive/warehouse` by default, for your temporary location.


```
sqoop import
--connect jdbc:mysql://172.29.1.12/test
--username root
--password ####
--table accounts
--hive-table accounts
--create-hive-table
--hive-import
--hive-home /path/to/hive_home
```
