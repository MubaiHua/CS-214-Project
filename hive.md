Start the docker container
`docker-compose -p hdfs-hive -f hdfs-hive.yml up -d`

Upload employees.csv to HDFS
`docker cp employees.csv hive-namenode:/tmp/employees.csv`

Create a directory in HDFS and then upload the file:
`docker exec -it hive-namenode hdfs dfs -mkdir -p /data`
`docker exec -it hive-namenode hdfs dfs -put /tmp/employees.csv /data/`

Make sure the data has been loaded successfully
`docker exec -it hive-namenode hdfs dfs -ls /data`

Running a Spark Shell
`docker exec -it hive-server /bin/bash`
`beeline -u jdbc:hive2://localhost:10000`

Run the following command to execute a simple query
```
CREATE DATABASE IF NOT EXISTS company_db;
USE company_db;

-- Create an external table pointing to the CSV in HDFS
CREATE EXTERNAL TABLE IF NOT EXISTS employees (
    id INT,
    name STRING,
    city STRING,
    country STRING,
    age INT,
    salary DOUBLE,
    department STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/data';

-- Simple check
SELECT * FROM employees LIMIT 5;

```

To exist spark shell, use ":q" or ":quit"