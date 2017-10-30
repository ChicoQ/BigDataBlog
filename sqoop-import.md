```
sqoop import \
--connect jdbc:mysql://172.29.1.12/test \
--username root -P \
-table=accountdevice \
-where "account_id between 1 and 10" \
-hive-import \
-m 1

0: jdbc:hive2://172.29.1.202:10000/default> SELECT * FROM accountdevice;
INFO  : Compiling command(queryId=hive_20171030153838_91da1b3f-c4f0-478f-b675-7621488a80e6): SELECT * FROM accountdevice
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:accountdevice.id, type:int, comment:null), FieldSchema(name:accountdevice.account_id, type:int, comment:null), FieldSchema(name:accountdevice.device_id, type:int, comment:null), FieldSchema(name:accountdevice.activation_date, type:string, comment:null), FieldSchema(name:accountdevice.account_device_id, type:string, comment:null)], properties:null)
INFO  : Completed compiling command(queryId=hive_20171030153838_91da1b3f-c4f0-478f-b675-7621488a80e6); Time taken: 0.062 seconds
INFO  : Executing command(queryId=hive_20171030153838_91da1b3f-c4f0-478f-b675-7621488a80e6): SELECT * FROM accountdevice
INFO  : Completed executing command(queryId=hive_20171030153838_91da1b3f-c4f0-478f-b675-7621488a80e6); Time taken: 0.001 seconds
INFO  : OK
+-------------------+---------------------------+--------------------------+--------------------------------+---------------------------------------+--+
| accountdevice.id  | accountdevice.account_id  | accountdevice.device_id  | accountdevice.activation_date  |    accountdevice.account_device_id    |
+-------------------+---------------------------+--------------------------+--------------------------------+---------------------------------------+--+
| 1                 | 1                         | 9                        | 2008-10-23 16:05:05.0          | 7eb61253-55cd-4309-8f17-129089fee461  |
| 2                 | 1                         | 29                       | 2011-12-05 19:37:42.0          | 20b671b4-2467-42d4-9b41-5d3784360b9f  |
| 3                 | 2                         | 5                        | 2008-11-12 03:00:01.0          | 658e3fac-8e85-40e0-9d08-ee3ee537b802  |
| 4                 | 3                         | 5                        | 2008-12-21 09:19:50.0          | fb674b1b-135e-408d-b5e3-39d10250e9cd  |
| 5                 | 4                         | 38                       | 2008-11-28 00:08:09.0          | c0b0c922-7fd4-46bb-a6d6-68d4064f22d5  |
| 6                 | 5                         | 5                        | 2008-11-15 23:06:06.0          | b72a7271-864e-46dc-b6bf-f5dd810f27ee  |
| 7                 | 5                         | 29                       | 2013-12-29 13:53:04.0          | e054dafa-88b8-41ef-8a7e-d530537eb0fd  |
| 8                 | 6                         | 9                        | 2008-11-20 12:39:33.0          | 7d11b8e3-d67d-45f9-b705-fbbbf16a0e02  |
| 9                 | 7                         | 38                       | 2008-12-09 10:32:12.0          | 98428e3a-bfb0-4870-af00-c53baa135a76  |
| 10                | 8                         | 10                       | 2008-12-15 08:49:38.0          | 58830f32-8a63-4afd-9851-0771c5ad4c77  |
| 11                | 8                         | 5                        | 2011-12-29 10:56:23.0          | c49c4f82-9e22-4b30-9476-6d1b12e72b18  |
| 12                | 9                         | 1                        | 2008-11-07 17:58:55.0          | 45fe71c6-c56f-4355-9e2b-94f3c215da50  |
| 13                | 9                         | 29                       | 2013-12-15 19:25:55.0          | 5266e928-9304-4ee0-b3f0-5cd1811649a6  |
| 14                | 10                        | 1                        | 2008-12-02 23:28:01.0          | ece98119-a036-400a-a4f1-b619458038ed  |
| 15                | 10                        | 1                        | 2013-11-08 19:12:49.0          | 27bdf2f3-82a2-48da-b249-faff113ca1f7  |
+-------------------+---------------------------+--------------------------+--------------------------------+---------------------------------------+--+
15 rows selected (0.154 seconds)
```
