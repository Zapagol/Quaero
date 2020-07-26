# QUAERO Assignment
You have a Kafka source which contains a JSON message having three parameter values inside it viz. customer_id, attribute_name, attribute_value.You have a Hive table named "email_mappings" which stores customer_id and email as 1:1 relationships.
-  Write Spark code which will read both the sources in a recurring scheduled job(daily/weekly etc) and write out.csv files in below format.
```
  email,<attribute_name1>=<attribute_value1>,<attribute_name2>=<attribute_value2>.......
```


## Assumptions

Provided input JSON is not in a valid format and hence I am assuming Kafka Source JSON in below format.I am ignoring other fields as we are only interested in custome details.

```json
    {"root":
        {"cust_info":
            [
                {
                    "customer_id": 123,
                    "attribute_name": "attribute_name1",
                    "attribute_value": "attribute_value1"
                },
                {
                    "customer_id": 111,
                    "attribute_name": "attribute_name2",
                    "attribute_value": "attribute_value2"
                 }
            ]
        }
    }
```

## Sample Input and Output

#### Input - Kafka Source

```json
    {"root":
        {"cust_info":
            [
                {
                    "customer_id": 123,
                    "attribute_name": "attribute_name1",
                    "attribute_value": "attribute_value1"
                },
                {
                    "customer_id": 123,
                    "attribute_name": "attribute_name2",
                    "attribute_value": "attribute_value2"
                 },
                 {
                    "customer_id": 111,
                    "attribute_name": "attribute_name1",
                    "attribute_value": "attribute_value1"
                 }
            ]
        }
    }
```

#### Input - Hive Table

```
+-----------+-------------+
|customer_id|email        |
+-----------+-------------+
|123        |abc@gmail.com|
|111        |xyz@yahoo.com|
+-----------+-------------+
```

#### Output

```
    email_id1,"attribute_name1 = attribute_value1,attribute_name2 = attribute_value2"
    email_id2,attribute_name1 = attribute_value1
```
### Generate Fat jar using sbt assembly

```
> sbt assembly
```
## Submitting the application

```
$ ./bin/spark-submit --class com.assignment.quaero.jobs.QuaeroAssignmentJob \
    --master yarn \
    --deploy-mode cluster \
    --driver-memory 4g \
    --executor-memory 2g \
    --executor-cores 1 \
    /target/scala-2.12/quaero-assembly_2.12-1.0.jar
```

