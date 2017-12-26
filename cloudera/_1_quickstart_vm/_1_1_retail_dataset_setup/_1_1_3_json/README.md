## Cloudera QuickStart VM - Retail DataSet Setup (In JSON file format)

### Step-0: Login To VM
~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Sun Dec 24 02:37:11 2017 from 192.168.211.1
[cloudera@quickstart ~]$
~~~

### Step-1: Setup retail dataset from existing avro dataset in .json format

* **1.1 - Generate JSON file from avro file**

~~~
[cloudera@quickstart ~]$ mkdir retail_db_json
[cloudera@quickstart ~]$ cd retail_db_json/
~~~

~~~
[cloudera@quickstart retail_db_json]$ mkdir categories

[cloudera@quickstart retail_db_json]$ avro-tools tojson hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/categories/part-m-00000.avro > categories/part-m-00000.json
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
~~~

~~~
[cloudera@quickstart retail_db_json]$ mkdir customers

[cloudera@quickstart retail_db_json]$ avro-tools tojson hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/customers/part-m-00000.avro > customers/part-m-00000.json
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
~~~

~~~
[cloudera@quickstart retail_db_json]$ mkdir departments

[cloudera@quickstart retail_db_json]$ avro-tools tojson hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/departments/part-m-00000.avro > departments/part-m-00000.json
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
~~~

~~~
[cloudera@quickstart retail_db_json]$ mkdir order_items

[cloudera@quickstart retail_db_json]$ avro-tools tojson hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/order_items/part-m-00000.avro > order_items/part-m-00000.json
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
~~~

~~~
[cloudera@quickstart retail_db_json]$ mkdir orders

[cloudera@quickstart retail_db_json]$ avro-tools tojson hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/orders/part-m-00000.avro > orders/part-m-00000.json
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
~~~

~~~
[cloudera@quickstart retail_db_json]$ mkdir products

[cloudera@quickstart retail_db_json]$ avro-tools tojson hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/products/part-m-00000.avro > products/part-m-00000.json
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
~~~

~~~
[cloudera@quickstart retail_db_json]$ ls -ltr -R
.:
total 24
drwxrwxr-x 2 cloudera cloudera 4096 Dec 26 02:01 categories
drwxrwxr-x 2 cloudera cloudera 4096 Dec 26 02:02 customers
drwxrwxr-x 2 cloudera cloudera 4096 Dec 26 02:02 departments
drwxrwxr-x 2 cloudera cloudera 4096 Dec 26 02:02 order_items
drwxrwxr-x 2 cloudera cloudera 4096 Dec 26 02:03 orders
drwxrwxr-x 2 cloudera cloudera 4096 Dec 26 02:03 products

./categories:
total 8
-rw-rw-r-- 1 cloudera cloudera 6017 Dec 26 02:01 part-m-00000.json

./customers:
total 4200
-rw-rw-r-- 1 cloudera cloudera 4298540 Dec 26 02:02 part-m-00000.json

./departments:
total 4
-rw-rw-r-- 1 cloudera cloudera 402 Dec 26 02:02 part-m-00000.json

./order_items:
total 36732
-rw-rw-r-- 1 cloudera cloudera 37609906 Dec 26 02:02 part-m-00000.json

./orders:
total 9052
-rw-rw-r-- 1 cloudera cloudera 9268297 Dec 26 02:03 part-m-00000.json

./products:
total 396
-rw-rw-r-- 1 cloudera cloudera 404010 Dec 26 02:03 part-m-00000.json
~~~

* **1.2 - Copy JSON files from local file system to HDFS**

~~~
[cloudera@quickstart retail_db_json]$ hadoop fs -put /home/cloudera/retail_db_json /user/cloudera
~~~

~~~
[cloudera@quickstart retail_db_json]$ hadoop fs -ls -R /user/cloudera/retail_db_json
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 02:12 /user/cloudera/retail_db_json/categories
-rw-r--r--   1 cloudera cloudera       6017 2017-12-26 02:12 /user/cloudera/retail_db_json/categories/part-m-00000.json
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 02:12 /user/cloudera/retail_db_json/customers
-rw-r--r--   1 cloudera cloudera    4298540 2017-12-26 02:12 /user/cloudera/retail_db_json/customers/part-m-00000.json
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 02:12 /user/cloudera/retail_db_json/departments
-rw-r--r--   1 cloudera cloudera        402 2017-12-26 02:12 /user/cloudera/retail_db_json/departments/part-m-00000.json
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 02:12 /user/cloudera/retail_db_json/order_items
-rw-r--r--   1 cloudera cloudera   37609906 2017-12-26 02:12 /user/cloudera/retail_db_json/order_items/part-m-00000.json
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 02:12 /user/cloudera/retail_db_json/orders
-rw-r--r--   1 cloudera cloudera    9268297 2017-12-26 02:12 /user/cloudera/retail_db_json/orders/part-m-00000.json
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 02:12 /user/cloudera/retail_db_json/products
-rw-r--r--   1 cloudera cloudera     404010 2017-12-26 02:12 /user/cloudera/retail_db_json/products/part-m-00000.json
~~~

### Step-2: retail_db_json database setup in HIVE
