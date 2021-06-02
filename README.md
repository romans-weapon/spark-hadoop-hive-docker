# spark-hadoop-hive-docker

[![Code Quality Grade](https://www.code-inspector.com/project/23311/status/svg)](https://www.code-inspector.com/project/23311/status/svg)
[![Code Quality Score](https://www.code-inspector.com/project/23311/score/svg)](https://www.code-inspector.com/project/23311/score/svg)
[![GitHub tag](https://img.shields.io/github/v/release/AnudeepKonaboina/spark-hadoop-hive-docker)](https://github.com/AnudeepKonaboina/spark-hadoop-hive-docker/tags)

This repo consists of the all docker/configuration/shell-script files related to spinning up spark with hadoop and hive ecosystem within a docker container.

# How to Run
1. Navigate to the main directory 
```commandline
cd spark-hadoop-hive-docker/
```
2. Run the script file
```commandline
sh setup.sh
```

3. After the setup is completed you will have two containers started as shown below
```commandline
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS          PORTS                                                                                                                                                           NAMES
feca5a88cca9   spark-with-hadoop-hive:latest   "/usr/sbin/init"         12 minutes ago   Up 12 minutes   22/tcp, 0.0.0.0:4040-4041->4040-4041/tcp, :::4040-4041->4040-4041/tcp, 0.0.0.0:8089->8088/tcp, :::8089->8088/tcp, 0.0.0.0:8090->18080/tcp, :::8090->18080/tcp   spark
bd8e86d70920   hive-metastore:latest           "docker-entrypoint.s…"   12 minutes ago   Up 12 minutes   5432/tcp                                                                                                                                                        hive_metastore
```
4. Go into the spark container using the command
```commandline
docker exec -it spark bash 
```
5. Once you get into the container,you will have spark hdfs and hive ready for you to use

## To run hive inside container
```commandline
[root@hadoop /]# hive
which: no hbase in (/usr/bin/apache-hive-2.1.1-bin/bin:/usr/bin/spark-2.4.7-bin-without-hadoop/bin:/usr/bin/spark-2.4.7-bin-without-hadoop/sbin:/usr/bin/hadoop-2.10.1/bin:/usr/bin/hadoop-2.10.1/sbin:/usr/lib/jvm/java-1.8.0-openjdk/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin)
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/bin/apache-hive-2.1.1-bin/lib/log4j-slf4j-impl-2.4.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/bin/hadoop-2.10.1/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]

Logging initialized using configuration in jar:file:/usr/bin/apache-hive-2.1.1-bin/lib/hive-common-2.1.1.jar!/hive-log4j2.properties Async: true
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
hive>
```

## To run hdfs commands within container
```commandline
[root@hadoop /]# hdfs dfs -ls /
21/06/02 12:49:26 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Found 2 items
drwxr-xr-x   - root supergroup          0 2021-06-02 12:48 /tmp
drwxr-xr-x   - root supergroup          0 2021-06-02 12:22 /user
[root@hadoop /]#
```

## To run spark shell within container
```commandline
[root@hadoop /]# spark-shell
21/06/02 12:50:55 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Spark context Web UI available at http://hadoop.spark:4040
Spark context available as 'sc' (master = local[*], app id = local-1622638263693).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.4.7
      /_/

Using Scala version 2.11.12 (OpenJDK 64-Bit Server VM, Java 1.8.0_292)
Type in expressions to have them evaluated.
Type :help for more information.

scala>
```

# Points to remember
1. All the services installed within the conatiner are standalone servies used for testing purposes.So should be used with less load only for testing.
2. The queries that you can run are only limited to select queries and you cant run count(*) or insert queries(i.e.., queries which include MR) because there is no yarn installed for a mapreduce job.
3. But still if you want to execute insert and count(*) queies run the below command before running your query.
```commandline
hive>set mapreduce.framework.name=local; //default value is yarn.Possible values are yarn/classic and local
hive>select count(*) from table;
```
which sets the runtime framework for executing MapReduce jobs to local because is is a standaalone service.

