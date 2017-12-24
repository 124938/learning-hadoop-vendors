## Cloudera QuickStart VM - Hive Integration With Spark

### Configuration Changes

* **Step-0: Login to Quick Start VM or gateway node of hadoop cluster using ssh**
~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Sun Oct 29 18:49:10 2017 from 192.168.211.1
[cloudera@quickstart ~]$
~~~

* **Step-1: Start `spark-shell` in YARN mode (before configuration changes):**
~~~
[cloudera@quickstart ~]$ spark-shell --master yarn --num-executors 1
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel).
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 1.6.0
      /_/

Using Scala version 2.10.5 (Java HotSpot(TM) 64-Bit Server VM, Java 1.7.0_67)
Type in expressions to have them evaluated.
Type :help for more information.
Spark context available as sc (master = yarn-client, app id = application_1513682465599_0030).
SQL context available as sqlContext.

scala> sqlContext
res6: org.apache.spark.sql.SQLContext = org.apache.spark.sql.hive.HiveContext@2dd944c6

scala> sqlContext.sql("show databases")
17/12/24 02:47:13 WARN metastore.ObjectStore: Version information not found in metastore. hive.metastore.schema.verification is not enabled so recording the schema version 1.1.0-cdh5.12.0
17/12/24 02:47:13 WARN metastore.ObjectStore: Failed to get database default, returning NoSuchObjectException
17/12/24 02:47:19 WARN metastore.ObjectStore: Version information not found in metastore. hive.metastore.schema.verification is not enabled so recording the schema version 1.1.0-cdh5.12.0
17/12/24 02:47:20 WARN metastore.ObjectStore: Failed to get database default, returning NoSuchObjectException
res7: org.apache.spark.sql.DataFrame = [result: string]

scala> sqlContext.sql("show databases").show(10)
+-------+
| result|
+-------+
|default|
+-------+

scala> :quit
Stopping spark context.
~~~

* **Step-2 : Create soft link of hive-site.xml under spark configuration folder:**
~~~
[cloudera@quickstart conf]$ cd /etc/hive/conf
[cloudera@quickstart conf]$ ls -ltr
total 24
-rw-r--r-- 1 root root 2060 Jun 29 04:04 ivysettings.xml
-rw-r--r-- 1 root root 3505 Jun 29 04:04 hive-log4j.properties
-rw-r--r-- 1 root root 2662 Jun 29 04:04 hive-exec-log4j.properties
-rw-r--r-- 1 root root 2378 Jun 29 04:04 hive-env.sh.template
-rw-r--r-- 1 root root 1196 Jun 29 04:04 beeline-log4j.properties.template
-rw-rw-r-- 1 root root 1937 Jul 19 12:16 hive-site.xml
[cloudera@quickstart ~]$ cd /etc/spark/conf
[cloudera@quickstart conf]$ ls -ltr
total 52
-rwxr-xr-x 1 root root 4209 Jun 29 05:02 spark-env.sh.template
-rw-r--r-- 1 root root 1292 Jun 29 05:02 spark-defaults.conf.template
-rw-r--r-- 1 root root  865 Jun 29 05:02 slaves.template
-rw-r--r-- 1 root root 6671 Jun 29 05:02 metrics.properties.template
-rw-r--r-- 1 root root 2025 Jun 29 05:02 log4j.properties.template
-rw-r--r-- 1 root root 1105 Jun 29 05:02 fairscheduler.xml.template
-rw-r--r-- 1 root root  987 Jun 29 05:02 docker.properties.template
-rwxr-xr-x 1 root root 6026 Jun 29 05:02 spark-env.sh
-rw-rw-r-- 1 root root   92 Jul 19 12:16 slaves
-rw-r--r-- 1 root root 1468 Dec 19 04:11 spark-defaults.conf
[cloudera@quickstart conf]$ sudo ln -s /etc/hive/conf/hive-site.xml hive-site.xml
[cloudera@quickstart conf]$ ls -ltr
total 52
-rwxr-xr-x 1 root root 4209 Jun 29 05:02 spark-env.sh.template
-rw-r--r-- 1 root root 1292 Jun 29 05:02 spark-defaults.conf.template
-rw-r--r-- 1 root root  865 Jun 29 05:02 slaves.template
-rw-r--r-- 1 root root 6671 Jun 29 05:02 metrics.properties.template
-rw-r--r-- 1 root root 2025 Jun 29 05:02 log4j.properties.template
-rw-r--r-- 1 root root 1105 Jun 29 05:02 fairscheduler.xml.template
-rw-r--r-- 1 root root  987 Jun 29 05:02 docker.properties.template
-rwxr-xr-x 1 root root 6026 Jun 29 05:02 spark-env.sh
-rw-rw-r-- 1 root root   92 Jul 19 12:16 slaves
-rw-r--r-- 1 root root 1468 Dec 19 04:11 spark-defaults.conf
lrwxrwxrwx 1 root root   28 Dec 24 03:05 hive-site.xml -> /etc/hive/conf/hive-site.xml
~~~

* **Step-3: Start `spark-shell` in YARN mode (after configuration changes):**
~~~
[cloudera@quickstart ~]$ spark-shell --master yarn --num-executors 1
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel).
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 1.6.0
      /_/

Using Scala version 2.10.5 (Java HotSpot(TM) 64-Bit Server VM, Java 1.7.0_67)
Type in expressions to have them evaluated.
Type :help for more information.
17/12/24 03:05:57 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
17/12/24 03:05:57 WARN util.Utils: Your hostname, quickstart.cloudera resolves to a loopback address: 127.0.0.1; using 192.168.211.142 instead (on interface eth1)
17/12/24 03:05:57 WARN util.Utils: Set SPARK_LOCAL_IP if you need to bind to another address
17/12/24 03:05:59 WARN shortcircuit.DomainSocketFactory: The short-circuit local reads feature cannot be used because libhadoop cannot be loaded.
Spark context available as sc (master = yarn-client, app id = application_1513682465599_0031).
SQL context available as sqlContext.

scala> sqlContext.
     sql("show databases").
     show(10)
17/12/24 03:06:27 WARN metastore.ObjectStore: Version information not found in metastore. hive.metastore.schema.verification is not enabled so recording the schema version 1.1.0-cdh5.12.0
17/12/24 03:06:27 WARN metastore.ObjectStore: Failed to get database default, returning NoSuchObjectException
+---------+
|   result|
+---------+
|  default|
|retail_db|
+---------+


scala> sqlContext.
     sql("use retail_db")
res1: org.apache.spark.sql.DataFrame = [result: string]

scala> sqlContext.
     sql("show tables").
     show
+-----------+-----------+
|  tableName|isTemporary|
+-----------+-----------+
| categories|      false|
|  customers|      false|
|departments|      false|
|order_items|      false|
|     orders|      false|
|   products|      false|
+-----------+-----------+

scala> 
~~~

