## Cloudera QuickStart VM - Retail DataSet Setup (In Sequence file format)

### Step-0.1: Copy sqoop serde JAR file to VM
~~~
asus@asus-GL553VD:~$ scp /home/asus/source_code/github/124938/learning-hadoop-vendors/cloudera/_1_quickstart_vm/_1_1_retail_dataset_setup/_1_1_4_sequence/hive-sqoop-serde.jar cloudera@192.168.211.142:/home/cloudera/hive-sqoop-serde.jar
cloudera@192.168.211.142's password: 
asus@asus-GL553VD:~$ 
~~~

### Step-0.2: Login To VM
~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Tue Dec 26 22:09:16 2017 from 192.168.211.1
[cloudera@quickstart ~]$
~~~

* **Verification of hive-sqoop-serde.jar file**

~~~
[cloudera@quickstart ~]$ ls -ltr hive-sqoop-serde.jar
-rw-rw-r-- 1 cloudera cloudera 4966 Dec 26 23:05 hive-sqoop-serde.jar
~~~

### Step-1: Importing all tables available under `retail_db` database of MySQL to HDFS in sequence file format

* **1.0 - Clean required files/folders**

~~~
[cloudera@quickstart ~]$ hadoop fs -rm -R /user/cloudera/sqoop/import-all-tables-sequence
Deleted /user/cloudera/sqoop/import-all-tables-sequence

[cloudera@quickstart ~]$ rm -fr retail_db_sequence_bin
~~~

* **1.1 - Execute below sqoop command**

~~~
[cloudera@quickstart ~]$ mkdir retail_db_sequence_bin

[cloudera@quickstart ~]$ sqoop import-all-tables \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--num-mappers 1 \
--warehouse-dir /user/cloudera/sqoop/import-all-tables-sequence \
--package-name com.learning.sqoop.sequencefile \
--bindir /home/cloudera/retail_db_sequence_bin \
--as-sequencefile
~~~

* **1.2 - Verification of retail_db_sequence_bin folder**

~~~
[cloudera@quickstart ~]$ ls -ltr /home/cloudera/retail_db_sequence_bin
total 64
drwxrwxr-x 3 cloudera cloudera  4096 Dec 26 23:10 com
-rw-rw-r-- 1 cloudera cloudera  6460 Dec 26 23:10 categories.jar
-rw-rw-r-- 1 cloudera cloudera 11617 Dec 26 23:11 customers.jar
-rw-rw-r-- 1 cloudera cloudera  5577 Dec 26 23:11 departments.jar
-rw-rw-r-- 1 cloudera cloudera  9143 Dec 26 23:11 order_items.jar
-rw-rw-r-- 1 cloudera cloudera  7571 Dec 26 23:11 orders.jar
-rw-rw-r-- 1 cloudera cloudera  9216 Dec 26 23:11 products.jar
~~~

* **1.3 - Verification of sequence file on HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop/import-all-tables-sequence/categories/part-m-00000 | tail

[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop/import-all-tables-sequence/customers/part-m-00000 | tail

[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop/import-all-tables-sequence/departments/part-m-00000 | tail

[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop/import-all-tables-sequence/order_items/part-m-00000 | tail

[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop/import-all-tables-sequence/orders/part-m-00000 | tail

[cloudera@quickstart ~]$ hadoop fs -cat /user/cloudera/sqoop/import-all-tables-sequence/products/part-m-00000 | tail
~~~

### Step-2: `retail_db_sequence` database setup in HIVE

* **2.1 - Create database called `retail_db_sequence` in Hive**

~~~
[cloudera@quickstart ~]$ hive

Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.

hive> show databases;
default
retail_db
retail_db_avro
retail_db_json
retail_db_parquet

hive> create database if not exists retail_db_sequence;

hive> use retail_db_sequence;

hive> set hive.cli.print.current.db=true;

hive (retail_db_sequence)> 
~~~

* **2.2 - Add required JAR files in classpath of Hive**

~~~
hive (retail_db_sequence)> ADD JAR /usr/lib/sqoop/sqoop-1.4.6-cdh5.12.0.jar;
Added [/usr/lib/sqoop/sqoop-1.4.6-cdh5.12.0.jar] to class path
Added resources: [/usr/lib/sqoop/sqoop-1.4.6-cdh5.12.0.jar]
~~~

~~~
hive (retail_db_sequence)> ADD JAR /home/cloudera/hive-sqoop-serde.jar;
Added [/home/cloudera/hive-sqoop-serde.jar] to class path
Added resources: [/home/cloudera/hive-sqoop-serde.jar]
~~~

~~~
hive (retail_db_sequence)> ADD JAR /home/cloudera/retail_db_sequence_bin/categories.jar;
Added [/home/cloudera/retail_db_sequence_bin/categories.jar] to class path
Added resources: [/home/cloudera/retail_db_sequence_bin/categories.jar]
~~~

~~~
hive (retail_db_sequence)> ADD JAR /home/cloudera/retail_db_sequence_bin/customers.jar;
Added [/home/cloudera/retail_db_sequence_bin/customers.jar] to class path
Added resources: [/home/cloudera/retail_db_sequence_bin/customers.jar]
~~~

~~~
hive (retail_db_sequence)> ADD JAR /home/cloudera/retail_db_sequence_bin/departments.jar;
Added [/home/cloudera/retail_db_sequence_bin/departments.jar] to class path
Added resources: [/home/cloudera/retail_db_sequence_bin/departments.jar]
~~~

~~~
hive (retail_db_sequence)> ADD JAR /home/cloudera/retail_db_sequence_bin/order_items.jar;
Added [/home/cloudera/retail_db_sequence_bin/order_items.jar] to class path
Added resources: [/home/cloudera/retail_db_sequence_bin/order_items.jar]
~~~

~~~
hive (retail_db_sequence)> ADD JAR /home/cloudera/retail_db_sequence_bin/orders.jar;
Added [/home/cloudera/retail_db_sequence_bin/orders.jar] to class path
Added resources: [/home/cloudera/retail_db_sequence_bin/orders.jar]
~~~

~~~
hive (retail_db_sequence)> ADD JAR /home/cloudera/retail_db_sequence_bin/products.jar;
Added [/home/cloudera/retail_db_sequence_bin/products.jar] to class path
Added resources: [/home/cloudera/retail_db_sequence_bin/products.jar]
~~~

* **2.3 - Create tables under `retail_db_sequence` database in Hive**

~~~
hive (retail_db_sequence)> create external table if not exists categories(
  category_id int,
  category_department_id int,
  category_name string)
row format serde 'com.cloudera.sqoop.contrib.FieldMappableSerDe'
with serdeproperties (
  "fieldmappable.classname" = "com.learning.sqoop.sequencefile.categories"
)
stored as sequencefile
location '/user/cloudera/sqoop/import-all-tables-sequence/categories';
~~~

~~~
hive (retail_db_sequence)> create external table if not exists customers(
  customer_id int,
  customer_fname string,
  customer_lname string,
  customer_email string,
  customer_password string,
  customer_street string,
  customer_city string,
  customer_state string,
  customer_zipcode string)
row format serde 'com.cloudera.sqoop.contrib.FieldMappableSerDe'
with serdeproperties (
  "fieldmappable.classname" = "com.learning.sqoop.sequencefile.customers"
)
stored as sequencefile
location '/user/cloudera/sqoop/import-all-tables-sequence/customers';
~~~

~~~
hive (retail_db_sequence)> create external table if not exists departments(
  department_id int,
  department_name string)
row format serde 'com.cloudera.sqoop.contrib.FieldMappableSerDe'
with serdeproperties (
  "fieldmappable.classname" = "com.learning.sqoop.sequencefile.departments"
)
stored as sequencefile
location '/user/cloudera/sqoop/import-all-tables-sequence/departments';
~~~

~~~
hive (retail_db_sequence)> create external table if not exists order_items(
  order_item_id int,
  order_item_order_id int,
  order_item_product_id int,
  order_item_quantity int,
  order_item_subtotal float,
  order_item_product_price float)
row format serde 'com.cloudera.sqoop.contrib.FieldMappableSerDe'
with serdeproperties (
  "fieldmappable.classname" = "com.learning.sqoop.sequencefile.order_items"
)
stored as sequencefile
location '/user/cloudera/sqoop/import-all-tables-sequence/order_items';
~~~

~~~
hive (retail_db_sequence)> create external table if not exists orders(
  order_id int,
  order_date string,
  order_customer_id int,
  order_status string)
row format serde 'com.cloudera.sqoop.contrib.FieldMappableSerDe'
with serdeproperties (
  "fieldmappable.classname" = "com.learning.sqoop.sequencefile.orders"
)
stored as sequencefile
location '/user/cloudera/sqoop/import-all-tables-sequence/orders';
~~~

~~~
hive (retail_db_sequence)> create external table if not exists products(
  product_id int,
  product_category_id int,
  product_name string,
  product_description string,
  product_price float,
  product_image string)
row format serde 'com.cloudera.sqoop.contrib.FieldMappableSerDe'
with serdeproperties (
  "fieldmappable.classname" = "com.learning.sqoop.sequencefile.products"
)
stored as sequencefile
location '/user/cloudera/sqoop/import-all-tables-sequence/products';
~~~

* **2.4 - Verification of tables under `retail_db_sequence` database in Hive**

~~~
hive (retail_db_sequence)> describe formatted categories;
OK
# col_name            	data_type           	comment             
	 	 
category_id         	int                 	from deserializer   
category_department_id	int                 	from deserializer   
category_name       	string              	from deserializer   
	 	 
# Detailed Table Information	 	 
Database:           	retail_db_sequence  	 
Owner:              	cloudera            	 
CreateTime:         	Tue Dec 26 23:22:35 PST 2017	 
LastAccessTime:     	UNKNOWN             	 
Protect Mode:       	None                	 
Retention:          	0                   	 
Location:           	hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-sequence/categories	 
Table Type:         	EXTERNAL_TABLE      	 
Table Parameters:	 	 
	COLUMN_STATS_ACCURATE	false               
	EXTERNAL            	TRUE                
	numFiles            	0                   
	numRows             	-1                  
	rawDataSize         	-1                  
	totalSize           	0                   
	transient_lastDdlTime	1514359355          
	 	 
# Storage Information	 	 
SerDe Library:      	com.cloudera.sqoop.contrib.FieldMappableSerDe	 
InputFormat:        	org.apache.hadoop.mapred.SequenceFileInputFormat	 
OutputFormat:       	org.apache.hadoop.hive.ql.io.HiveSequenceFileOutputFormat	 
Compressed:         	No                  	 
Num Buckets:        	-1                  	 
Bucket Columns:     	[]                  	 
Sort Columns:       	[]                  	 
Storage Desc Params:	 	 
	fieldmappable.classname	com.learning.sqoop.sequencefile.categories
	serialization.format	1                   
Time taken: 0.076 seconds, Fetched: 35 row(s)
~~~

~~~
hive (retail_db_sequence)> describe formatted customers;
OK
# col_name            	data_type           	comment             
	 	 
customer_id         	int                 	from deserializer   
customer_fname      	string              	from deserializer   
customer_lname      	string              	from deserializer   
customer_email      	string              	from deserializer   
customer_password   	string              	from deserializer   
customer_street     	string              	from deserializer   
customer_city       	string              	from deserializer   
customer_state      	string              	from deserializer   
customer_zipcode    	string              	from deserializer   
	 	 
# Detailed Table Information	 	 
Database:           	retail_db_sequence  	 
Owner:              	cloudera            	 
CreateTime:         	Tue Dec 26 23:23:38 PST 2017	 
LastAccessTime:     	UNKNOWN             	 
Protect Mode:       	None                	 
Retention:          	0                   	 
Location:           	hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-sequence/customers	 
Table Type:         	EXTERNAL_TABLE      	 
Table Parameters:	 	 
	COLUMN_STATS_ACCURATE	false               
	EXTERNAL            	TRUE                
	numFiles            	0                   
	numRows             	-1                  
	rawDataSize         	-1                  
	totalSize           	0                   
	transient_lastDdlTime	1514359418          
	 	 
# Storage Information	 	 
SerDe Library:      	com.cloudera.sqoop.contrib.FieldMappableSerDe	 
InputFormat:        	org.apache.hadoop.mapred.SequenceFileInputFormat	 
OutputFormat:       	org.apache.hadoop.hive.ql.io.HiveSequenceFileOutputFormat	 
Compressed:         	No                  	 
Num Buckets:        	-1                  	 
Bucket Columns:     	[]                  	 
Sort Columns:       	[]                  	 
Storage Desc Params:	 	 
	fieldmappable.classname	com.learning.sqoop.sequencefile.customers
	serialization.format	1                   
Time taken: 0.086 seconds, Fetched: 41 row(s)
~~~

~~~
hive (retail_db_sequence)> describe formatted departments;
OK
# col_name            	data_type           	comment             
	 	 
department_id       	int                 	from deserializer   
department_name     	string              	from deserializer   
	 	 
# Detailed Table Information	 	 
Database:           	retail_db_sequence  	 
Owner:              	cloudera            	 
CreateTime:         	Tue Dec 26 23:24:32 PST 2017	 
LastAccessTime:     	UNKNOWN             	 
Protect Mode:       	None                	 
Retention:          	0                   	 
Location:           	hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-sequence/departments	 
Table Type:         	EXTERNAL_TABLE      	 
Table Parameters:	 	 
	COLUMN_STATS_ACCURATE	false               
	EXTERNAL            	TRUE                
	numFiles            	0                   
	numRows             	-1                  
	rawDataSize         	-1                  
	totalSize           	0                   
	transient_lastDdlTime	1514359472          
	 	 
# Storage Information	 	 
SerDe Library:      	com.cloudera.sqoop.contrib.FieldMappableSerDe	 
InputFormat:        	org.apache.hadoop.mapred.SequenceFileInputFormat	 
OutputFormat:       	org.apache.hadoop.hive.ql.io.HiveSequenceFileOutputFormat	 
Compressed:         	No                  	 
Num Buckets:        	-1                  	 
Bucket Columns:     	[]                  	 
Sort Columns:       	[]                  	 
Storage Desc Params:	 	 
	fieldmappable.classname	com.learning.sqoop.sequencefile.departments
	serialization.format	1                   
Time taken: 0.057 seconds, Fetched: 34 row(s)
~~~

~~~
hive (retail_db_sequence)> describe formatted order_items;
OK
# col_name            	data_type           	comment             
	 	 
order_item_id       	int                 	from deserializer   
order_item_order_id 	int                 	from deserializer   
order_item_product_id	int                 	from deserializer   
order_item_quantity 	int                 	from deserializer   
order_item_subtotal 	float               	from deserializer   
order_item_product_price	float               	from deserializer   
	 	 
# Detailed Table Information	 	 
Database:           	retail_db_sequence  	 
Owner:              	cloudera            	 
CreateTime:         	Tue Dec 26 23:26:09 PST 2017	 
LastAccessTime:     	UNKNOWN             	 
Protect Mode:       	None                	 
Retention:          	0                   	 
Location:           	hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-sequence/order_items	 
Table Type:         	EXTERNAL_TABLE      	 
Table Parameters:	 	 
	COLUMN_STATS_ACCURATE	false               
	EXTERNAL            	TRUE                
	numFiles            	0                   
	numRows             	-1                  
	rawDataSize         	-1                  
	totalSize           	0                   
	transient_lastDdlTime	1514359569          
	 	 
# Storage Information	 	 
SerDe Library:      	com.cloudera.sqoop.contrib.FieldMappableSerDe	 
InputFormat:        	org.apache.hadoop.mapred.SequenceFileInputFormat	 
OutputFormat:       	org.apache.hadoop.hive.ql.io.HiveSequenceFileOutputFormat	 
Compressed:         	No                  	 
Num Buckets:        	-1                  	 
Bucket Columns:     	[]                  	 
Sort Columns:       	[]                  	 
Storage Desc Params:	 	 
	fieldmappable.classname	com.learning.sqoop.sequencefile.order_items
	serialization.format	1                   
Time taken: 0.063 seconds, Fetched: 38 row(s)
~~~

~~~
hive (retail_db_sequence)> describe formatted orders;
OK
# col_name            	data_type           	comment             
	 	 
order_id            	int                 	from deserializer   
order_date          	string              	from deserializer   
order_customer_id   	int                 	from deserializer   
order_status        	string              	from deserializer   
	 	 
# Detailed Table Information	 	 
Database:           	retail_db_sequence  	 
Owner:              	cloudera            	 
CreateTime:         	Tue Dec 26 23:26:55 PST 2017	 
LastAccessTime:     	UNKNOWN             	 
Protect Mode:       	None                	 
Retention:          	0                   	 
Location:           	hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-sequence/orders	 
Table Type:         	EXTERNAL_TABLE      	 
Table Parameters:	 	 
	COLUMN_STATS_ACCURATE	false               
	EXTERNAL            	TRUE                
	numFiles            	0                   
	numRows             	-1                  
	rawDataSize         	-1                  
	totalSize           	0                   
	transient_lastDdlTime	1514359615          
	 	 
# Storage Information	 	 
SerDe Library:      	com.cloudera.sqoop.contrib.FieldMappableSerDe	 
InputFormat:        	org.apache.hadoop.mapred.SequenceFileInputFormat	 
OutputFormat:       	org.apache.hadoop.hive.ql.io.HiveSequenceFileOutputFormat	 
Compressed:         	No                  	 
Num Buckets:        	-1                  	 
Bucket Columns:     	[]                  	 
Sort Columns:       	[]                  	 
Storage Desc Params:	 	 
	fieldmappable.classname	com.learning.sqoop.sequencefile.orders
	serialization.format	1                   
Time taken: 0.065 seconds, Fetched: 36 row(s)
~~~

~~~
hive (retail_db_sequence)> describe formatted products;
OK
# col_name            	data_type           	comment             
	 	 
product_id          	int                 	from deserializer   
product_category_id 	int                 	from deserializer   
product_name        	string              	from deserializer   
product_description 	string              	from deserializer   
product_price       	float               	from deserializer   
product_image       	string              	from deserializer   
	 	 
# Detailed Table Information	 	 
Database:           	retail_db_sequence  	 
Owner:              	cloudera            	 
CreateTime:         	Tue Dec 26 23:29:10 PST 2017	 
LastAccessTime:     	UNKNOWN             	 
Protect Mode:       	None                	 
Retention:          	0                   	 
Location:           	hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-sequence/products	 
Table Type:         	EXTERNAL_TABLE      	 
Table Parameters:	 	 
	COLUMN_STATS_ACCURATE	false               
	EXTERNAL            	TRUE                
	numFiles            	0                   
	numRows             	-1                  
	rawDataSize         	-1                  
	totalSize           	0                   
	transient_lastDdlTime	1514359750          
	 	 
# Storage Information	 	 
SerDe Library:      	com.cloudera.sqoop.contrib.FieldMappableSerDe	 
InputFormat:        	org.apache.hadoop.mapred.SequenceFileInputFormat	 
OutputFormat:       	org.apache.hadoop.hive.ql.io.HiveSequenceFileOutputFormat	 
Compressed:         	No                  	 
Num Buckets:        	-1                  	 
Bucket Columns:     	[]                  	 
Sort Columns:       	[]                  	 
Storage Desc Params:	 	 
	fieldmappable.classname	com.learning.sqoop.sequencefile.products
	serialization.format	1                   
Time taken: 0.075 seconds, Fetched: 38 row(s)
~~~

* **2.5 - Execute join query in Hive**

~~~
hive (retail_db)> SET hive.auto.convert.join=false;

hive (retail_db)> select o.order_date, sum(oi.order_item_subtotal)
from orders o join order_items oi on (o.order_id = oi.order_item_order_id)
group by o.order_date 
limit 10;

Query ID = cloudera_20171227012929_10922e7e-ebd4-47fb-b1b5-86026e1fc1db
Total jobs = 2
Launching Job 1 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514349472177_0043, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514349472177_0043/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514349472177_0043
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2017-12-27 01:29:52,672 Stage-1 map = 0%,  reduce = 0%
2017-12-27 01:29:59,986 Stage-1 map = 50%,  reduce = 0%, Cumulative CPU 4.78 sec
2017-12-27 01:30:01,045 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 8.68 sec
2017-12-27 01:30:07,289 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 11.28 sec
MapReduce Total cumulative CPU time: 11 seconds 280 msec
Ended Job = job_1514349472177_0043
Launching Job 2 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514349472177_0044, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514349472177_0044/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514349472177_0044
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2017-12-27 01:30:14,255 Stage-2 map = 0%,  reduce = 0%
2017-12-27 01:30:18,523 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 1.04 sec
2017-12-27 01:30:23,715 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 2.21 sec
MapReduce Total cumulative CPU time: 2 seconds 210 msec
Ended Job = job_1514349472177_0044
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 11.28 sec   HDFS Read: 11533461 HDFS Write: 17364 SUCCESS
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 2.21 sec   HDFS Read: 22571 HDFS Write: 407 SUCCESS
Total MapReduce CPU Time Spent: 13 seconds 490 msec
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
Time taken: 38.015 seconds, Fetched: 10 row(s)
~~~