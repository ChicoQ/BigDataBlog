```
mysql> describe shapes;
+---------------------+-------------+------+-----+---------+-------+
| Field               | Type        | Null | Key | Default | Extra |
+---------------------+-------------+------+-----+---------+-------+
| shape_id            | varchar(45) | YES  |     | NULL    |       |
| shape_pt_lat        | varchar(20) | YES  |     | NULL    |       |
| shape_pt_lon        | varchar(20) | YES  |     | NULL    |       |
| shape_pt_sequence   | varchar(20) | YES  |     | NULL    |       |
| shape_dist_traveled | varchar(45) | YES  |     | NULL    |       |
+---------------------+-------------+------+-----+---------+-------+
5 rows in set (0.00 sec)


beeline> !connect jdbc:hive2://172.29.1.202:10000/default

0: jdbc:hive2://172.29.1.202:10000/default> describe shapes;
+----------------------+------------+----------+--+
|       col_name       | data_type  | comment  |
+----------------------+------------+----------+--+
| shape_id             | string     |          |
| shape_pt_lat         | double     |          |
| shape_pt_lon         | double     |          |
| shape_pt_sequence    | bigint     |          |
| shape_dist_traveled  | string     |          |
+----------------------+------------+----------+--+
5 rows selected (0.069 seconds)

0: jdbc:hive2://172.29.1.202:10000/default> select * from shapes limit 5;

+----------------------------+----------------------+----------------------+---------------------------+-----------------------------+--+
|      shapes.shape_id       | shapes.shape_pt_lat  | shapes.shape_pt_lon  | shapes.shape_pt_sequence  | shapes.shape_dist_traveled  |
+----------------------------+----------------------+----------------------+---------------------------+-----------------------------+--+
| 340-20170724124507_v56.18  | -36.89441            | 174.93224            | 1                         |                             |
| 340-20170724124507_v56.18  | -36.89444            | 174.93219            | 2                         |                             |
| 340-20170724124507_v56.18  | -36.8947             | 174.93256            | 3                         |                             |
| 340-20170724124507_v56.18  | -36.89498            | 174.93294            | 4                         |                             |
| 340-20170724124507_v56.18  | -36.89517            | 174.9332             | 5                         |                             |
+----------------------------+----------------------+----------------------+---------------------------+-----------------------------+--+


sqoop export \
--connect jdbc:mysql://172.29.1.12/test \
--username root -P \
--table shapes \
--export-dir /user/hive/warehouse/shapes/shapes.txt

```
