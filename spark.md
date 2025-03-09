Start the docker container
`
docker-compose -p hdfs-spark -f hdfs-spark.yml up -d
`

Upload employees.csv to HDFS
`
docker cp employees.csv namenode:/tmp/employees.csv
`

Create a directory in HDFS and then upload the file:
`docker exec -it namenode hdfs dfs -mkdir -p /data`
`docker exec -it namenode hdfs dfs -put /tmp/employees.csv /data/`

Make sure the data has been loaded successfully
`docker exec -it namenode hdfs dfs -ls /data`

Running a Spark Shell
`docker exec -it spark-master /bin/bash`

Run the following command to execute a simple query
```
// Read employees.csv from HDFS
val employeesDF = spark.read
  .option("header", "true")
  .option("inferSchema", "true")
  .csv("hdfs://namenode:8020/data/employees.csv")

// Create a temporary Spark SQL view
employeesDF.createOrReplaceTempView("employees")

// Run a more complex query
spark.sql("""
  SELECT department,
         AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department
""").show()

// Another example: Count employees by country, department, with filter
spark.sql("""
  SELECT country,
         department,
         COUNT(*) AS employee_count
  FROM employees
  WHERE salary > 6000
  GROUP BY country, department
  ORDER BY employee_count DESC
""").show()
```

To exist spark shell, use ":q" or ":quit"