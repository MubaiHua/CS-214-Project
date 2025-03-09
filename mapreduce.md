Start the docker container
`docker-compose -p hdfs-mapreduce -f hdfs-mapreduce.yml up -d`

Upload employees.csv to HDFS
`docker cp employees.csv primarynode:/tmp/employees.csv`

Create a directory in HDFS and then upload the file:
`docker exec -it primarynode hdfs dfs -mkdir -p /data`
`docker exec -it primarynode hdfs dfs -put /tmp/employees.csv /data/`

Make sure the data has been loaded successfully
`docker exec -it primarynode hdfs dfs -ls /data`

Running a Spark Shell
`docker exec -it hive-server /bin/bash`
`beeline -u jdbc:hive2://localhost:10000`

Run the following command to execute a simple query
```
docker exec -it primarynode hadoop jar /opt/hadoop-2.7.4/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.4.jar grep /data/employees.csv /data/employees_grep_output "Engineering"

```

`docker exec -it primarynode hdfs dfs -cat /data/employees_grep_output/part-r-00000`

`docker exec -it primarynode hdfs dfs -cat /data/employees_grep_output/part-r-00000`

use chatGPT to generate more