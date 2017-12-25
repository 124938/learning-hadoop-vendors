## Cloudera QuickStart VM - Retail DataSet Setup

### Step-0: Login To VM
~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Sun Nov 26 17:58:32 2017 from 192.168.211.1
[cloudera@quickstart ~]$
~~~

### Step-1: Importing all tables available under retail_db database of MySQL to HDFS (in .txt format)

* **1.1 - Execute below sqoop command**

~~~
[cloudera@quickstart ~]$ sqoop import-all-tables \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--num-mappers 1 \
--warehouse-dir /user/cloudera/sqoop/import-all-tables-text \
--as-textfile
~~~

* **1.2 - Verify retail_db dataset on HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop/import-all-tables-text
drwxr-xr-x   - cloudera cloudera          0 2017-12-19 03:24 /user/cloudera/sqoop/import-all-tables-text/categories
-rw-r--r--   1 cloudera cloudera          0 2017-12-19 03:24 /user/cloudera/sqoop/import-all-tables-text/categories/_SUCCESS
-rw-r--r--   1 cloudera cloudera       1029 2017-12-19 03:24 /user/cloudera/sqoop/import-all-tables-text/categories/part-m-00000
drwxr-xr-x   - cloudera cloudera          0 2017-12-19 03:24 /user/cloudera/sqoop/import-all-tables-text/customers
-rw-r--r--   1 cloudera cloudera          0 2017-12-19 03:24 /user/cloudera/sqoop/import-all-tables-text/customers/_SUCCESS
-rw-r--r--   1 cloudera cloudera     953525 2017-12-19 03:24 /user/cloudera/sqoop/import-all-tables-text/customers/part-m-00000
drwxr-xr-x   - cloudera cloudera          0 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/departments
-rw-r--r--   1 cloudera cloudera          0 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/departments/_SUCCESS
-rw-r--r--   1 cloudera cloudera         60 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/departments/part-m-00000
drwxr-xr-x   - cloudera cloudera          0 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/order_items
-rw-r--r--   1 cloudera cloudera          0 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    5408880 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/order_items/part-m-00000
drwxr-xr-x   - cloudera cloudera          0 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/orders
-rw-r--r--   1 cloudera cloudera          0 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera    2999944 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/orders/part-m-00000
drwxr-xr-x   - cloudera cloudera          0 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/products
-rw-r--r--   1 cloudera cloudera          0 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/products/_SUCCESS
-rw-r--r--   1 cloudera cloudera     173993 2017-12-19 03:25 /user/cloudera/sqoop/import-all-tables-text/products/part-m-00000
~~~

* **1.3 - Creation of database called retail_db and required tables in Hive**

~~~
[cloudera@quickstart ~]$ hive
Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.
hive> show databases;
OK
default
Time taken: 0.651 seconds, Fetched: 1 row(s)
  
hive> create database retail_db;

hive> use retail_db;

hive> set hive.cli.print.current.db=true;

hive (retail_db)>  create external table categories(
  category_id int,
  category_department_id int,
  category_name string)
row format delimited 
  fields terminated by ','
stored as textfile
location '/user/cloudera/sqoop/import-all-tables-text/categories';

hive (retail_db)>  create external table customers(
  customer_id int,
  customer_fname string,
  customer_lname string,
  customer_email string,
  customer_password string,
  customer_street string,
  customer_city string,
  customer_state string,
  customer_zipcode string)
row format delimited 
  fields terminated by ','
stored as textfile
location '/user/cloudera/sqoop/import-all-tables-text/customers';

hive (retail_db)>  create external table departments(
  department_id int,
  department_name string)
row format delimited
  fields terminated by ','
stored as textfile
location '/user/cloudera/sqoop/import-all-tables-text/departments';

hive (retail_db)>  create external table order_items(
  order_item_id int,
  order_item_order_id int,
  order_item_product_id int,
  order_item_quantity int,
  order_item_subtotal float,
  order_item_product_price float)
row format delimited
  fields terminated by ','
stored as textfile
location '/user/cloudera/sqoop/import-all-tables-text/order_items';

hive (retail_db)>  create external table orders(
  order_id int,
  order_date string,
  order_customer_id int,
  order_status string)
row format delimited
  fields terminated by ','
stored as textfile
location '/user/cloudera/sqoop/import-all-tables-text/orders';

hive (retail_db)>  create external table products(
  product_id int,
  product_category_id int,
  product_name string,
  product_description string,
  product_price float,
  product_image string)
row format delimited
  fields terminated by ','
stored as textfile
location '/user/cloudera/sqoop/import-all-tables-text/products';
~~~

* **1.4 - Execute join query in Hive**

~~~
hive (retail_db)> SET hive.auto.convert.join=false;

hive (retail_db)> select o.order_date, sum(oi.order_item_subtotal)
from orders o join order_items oi on (o.order_id = oi.order_item_order_id)
group by o.order_date 
limit 10;

Query ID = cloudera_20171126213232_23711b96-18fa-4cd6-9e88-0836f97b0833
Total jobs = 2
Launching Job 1 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1509278183296_0029, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1509278183296_0029/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1509278183296_0029
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2017-11-26 21:32:43,839 Stage-1 map = 0%,  reduce = 0%
2017-11-26 21:32:50,231 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 5.56 sec
2017-11-26 21:32:55,445 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 8.23 sec
MapReduce Total cumulative CPU time: 8 seconds 230 msec
Ended Job = job_1509278183296_0029
Launching Job 2 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1509278183296_0030, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1509278183296_0030/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1509278183296_0030
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2017-11-26 21:33:02,308 Stage-2 map = 0%,  reduce = 0%
2017-11-26 21:33:06,478 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 0.83 sec
2017-11-26 21:33:12,787 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 2.0 sec
MapReduce Total cumulative CPU time: 2 seconds 0 msec
Ended Job = job_1509278183296_0030
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 8.23 sec   HDFS Read: 8422614 HDFS Write: 17364 SUCCESS
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 2.0 sec   HDFS Read: 22571 HDFS Write: 407 SUCCESS
Total MapReduce CPU Time Spent: 10 seconds 230 msec
OK

2013-07-25 00:00:00.0	68153.83132743835
2013-07-26 00:00:00.0	136520.17266082764
2013-07-27 00:00:00.0	101074.34193611145
2013-07-28 00:00:00.0	87123.08192253113
2013-07-29 00:00:00.0	137287.09244918823
2013-07-30 00:00:00.0	102745.62186431885
2013-07-31 00:00:00.0	131878.06256484985
2013-08-01 00:00:00.0	129001.62241744995
2013-08-02 00:00:00.0	109347.00200462341
2013-08-03 00:00:00.0	95266.89186286926
Time taken: 36.754 seconds, Fetched: 10 row(s)
~~~


### Step-2: Importing all tables available under retail_db database of MySQL to HDFS (in .avro format)

* **2.1 - Execute below sqoop command**

~~~
[cloudera@quickstart ~]$ sqoop import-all-tables \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--num-mappers 1 \
--warehouse-dir /user/cloudera/sqoop/import-all-tables-avro \
--as-avrodatafile
~~~

* **2.2 - Verify retail_db dataset on HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop/import-all-tables-avro
drwxr-xr-x   - cloudera cloudera          0 2017-12-24 20:41 /user/cloudera/sqoop/import-all-tables-avro/categories
-rw-r--r--   1 cloudera cloudera          0 2017-12-24 20:41 /user/cloudera/sqoop/import-all-tables-avro/categories/_SUCCESS
-rw-r--r--   1 cloudera cloudera       1534 2017-12-24 20:41 /user/cloudera/sqoop/import-all-tables-avro/categories/part-m-00000.avro
drwxr-xr-x   - cloudera cloudera          0 2017-12-24 20:41 /user/cloudera/sqoop/import-all-tables-avro/customers
-rw-r--r--   1 cloudera cloudera          0 2017-12-24 20:41 /user/cloudera/sqoop/import-all-tables-avro/customers/_SUCCESS
-rw-r--r--   1 cloudera cloudera    1032483 2017-12-24 20:41 /user/cloudera/sqoop/import-all-tables-avro/customers/part-m-00000.avro
drwxr-xr-x   - cloudera cloudera          0 2017-12-24 20:41 /user/cloudera/sqoop/import-all-tables-avro/departments
-rw-r--r--   1 cloudera cloudera          0 2017-12-24 20:41 /user/cloudera/sqoop/import-all-tables-avro/departments/_SUCCESS
-rw-r--r--   1 cloudera cloudera        450 2017-12-24 20:41 /user/cloudera/sqoop/import-all-tables-avro/departments/part-m-00000.avro
drwxr-xr-x   - cloudera cloudera          0 2017-12-24 20:42 /user/cloudera/sqoop/import-all-tables-avro/order_items
-rw-r--r--   1 cloudera cloudera          0 2017-12-24 20:42 /user/cloudera/sqoop/import-all-tables-avro/order_items/_SUCCESS
-rw-r--r--   1 cloudera cloudera    3933008 2017-12-24 20:42 /user/cloudera/sqoop/import-all-tables-avro/order_items/part-m-00000.avro
drwxr-xr-x   - cloudera cloudera          0 2017-12-24 20:42 /user/cloudera/sqoop/import-all-tables-avro/orders
-rw-r--r--   1 cloudera cloudera          0 2017-12-24 20:42 /user/cloudera/sqoop/import-all-tables-avro/orders/_SUCCESS
-rw-r--r--   1 cloudera cloudera    1779793 2017-12-24 20:42 /user/cloudera/sqoop/import-all-tables-avro/orders/part-m-00000.avro
drwxr-xr-x   - cloudera cloudera          0 2017-12-24 20:42 /user/cloudera/sqoop/import-all-tables-avro/products
-rw-r--r--   1 cloudera cloudera          0 2017-12-24 20:42 /user/cloudera/sqoop/import-all-tables-avro/products/_SUCCESS
-rw-r--r--   1 cloudera cloudera     175677 2017-12-24 20:42 /user/cloudera/sqoop/import-all-tables-avro/products/part-m-00000.avro
~~~

* **2.3 - Print schema details of avro file**

~~~
[cloudera@quickstart ~]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/categories/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "type" : "record",
  "name" : "categories",
  "doc" : "Sqoop import of categories",
  "fields" : [ {
    "name" : "category_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "category_id",
    "sqlType" : "4"
  }, {
    "name" : "category_department_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "category_department_id",
    "sqlType" : "4"
  }, {
    "name" : "category_name",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "category_name",
    "sqlType" : "12"
  } ],
  "tableName" : "categories"
}
[cloudera@quickstart ~]$ clear

[cloudera@quickstart ~]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/categories/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "type" : "record",
  "name" : "categories",
  "doc" : "Sqoop import of categories",
  "fields" : [ {
    "name" : "category_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "category_id",
    "sqlType" : "4"
  }, {
    "name" : "category_department_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "category_department_id",
    "sqlType" : "4"
  }, {
    "name" : "category_name",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "category_name",
    "sqlType" : "12"
  } ],
  "tableName" : "categories"
}
[cloudera@quickstart ~]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/customers/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "type" : "record",
  "name" : "customers",
  "doc" : "Sqoop import of customers",
  "fields" : [ {
    "name" : "customer_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "customer_id",
    "sqlType" : "4"
  }, {
    "name" : "customer_fname",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "customer_fname",
    "sqlType" : "12"
  }, {
    "name" : "customer_lname",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "customer_lname",
    "sqlType" : "12"
  }, {
    "name" : "customer_email",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "customer_email",
    "sqlType" : "12"
  }, {
    "name" : "customer_password",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "customer_password",
    "sqlType" : "12"
  }, {
    "name" : "customer_street",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "customer_street",
    "sqlType" : "12"
  }, {
    "name" : "customer_city",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "customer_city",
    "sqlType" : "12"
  }, {
    "name" : "customer_state",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "customer_state",
    "sqlType" : "12"
  }, {
    "name" : "customer_zipcode",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "customer_zipcode",
    "sqlType" : "12"
  } ],
  "tableName" : "customers"
}
[cloudera@quickstart ~]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/departments/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "type" : "record",
  "name" : "departments",
  "doc" : "Sqoop import of departments",
  "fields" : [ {
    "name" : "department_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "department_id",
    "sqlType" : "4"
  }, {
    "name" : "department_name",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "department_name",
    "sqlType" : "12"
  } ],
  "tableName" : "departments"
}
[cloudera@quickstart ~]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/order_items/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "type" : "record",
  "name" : "order_items",
  "doc" : "Sqoop import of order_items",
  "fields" : [ {
    "name" : "order_item_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_item_id",
    "sqlType" : "4"
  }, {
    "name" : "order_item_order_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_item_order_id",
    "sqlType" : "4"
  }, {
    "name" : "order_item_product_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_item_product_id",
    "sqlType" : "4"
  }, {
    "name" : "order_item_quantity",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_item_quantity",
    "sqlType" : "-6"
  }, {
    "name" : "order_item_subtotal",
    "type" : [ "null", "float" ],
    "default" : null,
    "columnName" : "order_item_subtotal",
    "sqlType" : "7"
  }, {
    "name" : "order_item_product_price",
    "type" : [ "null", "float" ],
    "default" : null,
    "columnName" : "order_item_product_price",
    "sqlType" : "7"
  } ],
  "tableName" : "order_items"
}
[cloudera@quickstart ~]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/orders/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "type" : "record",
  "name" : "orders",
  "doc" : "Sqoop import of orders",
  "fields" : [ {
    "name" : "order_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_id",
    "sqlType" : "4"
  }, {
    "name" : "order_date",
    "type" : [ "null", "long" ],
    "default" : null,
    "columnName" : "order_date",
    "sqlType" : "93"
  }, {
    "name" : "order_customer_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "order_customer_id",
    "sqlType" : "4"
  }, {
    "name" : "order_status",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "order_status",
    "sqlType" : "12"
  } ],
  "tableName" : "orders"
}
[cloudera@quickstart ~]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/products/part-m-00000.avro
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
{
  "type" : "record",
  "name" : "products",
  "doc" : "Sqoop import of products",
  "fields" : [ {
    "name" : "product_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "product_id",
    "sqlType" : "4"
  }, {
    "name" : "product_category_id",
    "type" : [ "null", "int" ],
    "default" : null,
    "columnName" : "product_category_id",
    "sqlType" : "4"
  }, {
    "name" : "product_name",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "product_name",
    "sqlType" : "12"
  }, {
    "name" : "product_description",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "product_description",
    "sqlType" : "12"
  }, {
    "name" : "product_price",
    "type" : [ "null", "float" ],
    "default" : null,
    "columnName" : "product_price",
    "sqlType" : "7"
  }, {
    "name" : "product_image",
    "type" : [ "null", "string" ],
    "default" : null,
    "columnName" : "product_image",
    "sqlType" : "12"
  } ],
  "tableName" : "products"
}

~~~

* **2.3 - Generate avro schema from avro file**

~~~
[cloudera@quickstart ~]$ mkdir /home/cloudera/retail_db_avro_schema
[cloudera@quickstart ~]$ cd /home/cloudera/retail_db_avro_schema
[cloudera@quickstart retail_db_avro_schema]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/categories/part-m-00000.avro > /home/cloudera/retail_db_avro_schema/categories.avsc
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
[cloudera@quickstart retail_db_avro_schema]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/customers/part-m-00000.avro > /home/cloudera/retail_db_avro_schema/customers.avsc
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
[cloudera@quickstart retail_db_avro_schema]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/departments/part-m-00000.avro > /home/cloudera/retail_db_avro_schema/departments.avsc
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
[cloudera@quickstart retail_db_avro_schema]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/order_items/part-m-00000.avro > /home/cloudera/retail_db_avro_schema/order_items.avsc
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
[cloudera@quickstart retail_db_avro_schema]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/orders/part-m-00000.avro > /home/cloudera/retail_db_avro_schema/orders.avsc
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
[cloudera@quickstart retail_db_avro_schema]$ avro-tools getschema hdfs://quickstart.cloudera/user/cloudera/sqoop/import-all-tables-avro/products/part-m-00000.avro > /home/cloudera/retail_db_avro_schema/products.avsc
log4j:WARN No appenders could be found for logger (org.apache.hadoop.metrics2.lib.MutableMetricsFactory).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
[cloudera@quickstart retail_db_avro_schema]$ ls -ltr /home/cloudera/retail_db_avro_schema/
total 24
-rw-rw-r-- 1 cloudera cloudera  595 Dec 24 20:53 categories.avsc
-rw-rw-r-- 1 cloudera cloudera 1510 Dec 24 20:54 customers.avsc
-rw-rw-r-- 1 cloudera cloudera  441 Dec 24 20:56 departments.avsc
-rw-rw-r-- 1 cloudera cloudera 1100 Dec 24 20:57 order_items.avsc
-rw-rw-r-- 1 cloudera cloudera  708 Dec 24 20:57 orders.avsc
-rw-rw-r-- 1 cloudera cloudera 1042 Dec 24 20:57 products.avsc
~~~