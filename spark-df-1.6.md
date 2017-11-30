

```
scala> val file = "file:///usr/lib/spark/python/test_support/sql/people.json"
file: String = file:///usr/lib/spark/python/test_support/sql/people.json

scala> 

scala> val input = sqlContext.read.json(file)
input: org.apache.spark.sql.DataFrame = [age: bigint, name: string]             

scala> 

scala> 

scala> input.show(5)
+----+-------+
| age|   name|
+----+-------+
|null|Michael|
|  30|   Andy|
|  19| Justin|
+----+-------+


scala> 

scala> input.registerTempTable("cars")

scala> val result = sqlContext.sql("select * from cars")
result: org.apache.spark.sql.DataFrame = [age: bigint, name: string]

scala> result.show
+----+-------+
| age|   name|
+----+-------+
|null|Michael|
|  30|   Andy|
|  19| Justin|
+----+-------+


scala> result("name")
res3: org.apache.spark.sql.Column = name

scala> result("age")
res4: org.apache.spark.sql.Column = age

scala> 

scala> result.filter(result("age") > 18)
res5: org.apache.spark.sql.DataFrame = [age: bigint, name: string]

scala> 

scala> 

scala> 

scala> result.filter(result("age") > 18).show
+---+------+
|age|  name|
+---+------+
| 30|  Andy|
| 19|Justin|
+---+------+


scala> result.where(result("age") > 18).show
+---+------+
|age|  name|
+---+------+
| 30|  Andy|
| 19|Justin|
+---+------+


scala> result.where("age" > 18).show
<console>:28: error: type mismatch;
 found   : Int(18)
 required: String
              result.where("age" > 18).show
                                   ^

scala> result.where($"age" > 18).show
+---+------+
|age|  name|
+---+------+
| 30|  Andy|
| 19|Justin|
+---+------+


scala> 

scala> result.where('name === "Andy").show
+---+----+
|age|name|
+---+----+
| 30|Andy|
+---+----+


scala> result.where($"name" === "Andy").show
+---+----+
|age|name|
+---+----+
| 30|Andy|
+---+----+


scala> 

scala> result.where("name" === "Andy").show
<console>:28: error: value === is not a member of String
              result.where("name" === "Andy").show
                                  ^

scala> 

scala> 

scala> result.where(result("name") == "Andy").show
<console>:28: error: overloaded method value where with alternatives:
  (conditionExpr: String)org.apache.spark.sql.DataFrame <and>
  (condition: org.apache.spark.sql.Column)org.apache.spark.sql.DataFrame
 cannot be applied to (Boolean)
              result.where(result("name") == "Andy").show
                     ^

scala> result.filter(result("name") == "Andy").show
<console>:28: error: overloaded method value filter with alternatives:
  (conditionExpr: String)org.apache.spark.sql.DataFrame <and>
  (condition: org.apache.spark.sql.Column)org.apache.spark.sql.DataFrame
 cannot be applied to (Boolean)
              result.filter(result("name") == "Andy").show
                     ^

scala> 

scala> result.filter(result("name") = "Andy").show
<console>:28: error: value update is not a member of org.apache.spark.sql.DataFrame
              result.filter(result("name") = "Andy").show
                            ^

scala> 

scala> result.filter(result("age") > 20).show
+---+----+
|age|name|
+---+----+
| 30|Andy|
+---+----+




```
