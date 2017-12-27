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

### Step-1: Importing all tables available under retail_db database of MySQL to HDFS in sequence file format

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

* **1.2 - Verification of retail_db_sequence_bin directory**

~~~
/home/cloudera/retail_db_sequence_bin/com/learning/sqoop/sequencefile:
total 232
-rw-rw-r-- 1 cloudera cloudera 14147 Dec 26 23:11 products.class
-rw-rw-r-- 1 cloudera cloudera   781 Dec 26 23:11 products$5.class
-rw-rw-r-- 1 cloudera cloudera   784 Dec 26 23:11 products$6.class
-rw-rw-r-- 1 cloudera cloudera   787 Dec 26 23:11 products$1.class
-rw-rw-r-- 1 cloudera cloudera   787 Dec 26 23:11 products$2.class
-rw-rw-r-- 1 cloudera cloudera   784 Dec 26 23:11 products$3.class
-rw-rw-r-- 1 cloudera cloudera   784 Dec 26 23:11 products$4.class
-rw-rw-r-- 1 cloudera cloudera   289 Dec 26 23:11 products$FieldSetterCommand.class
-rw-rw-r-- 1 cloudera cloudera 12475 Dec 26 23:11 orders.class
-rw-rw-r-- 1 cloudera cloudera   776 Dec 26 23:11 orders$2.class
-rw-rw-r-- 1 cloudera cloudera   773 Dec 26 23:11 orders$3.class
-rw-rw-r-- 1 cloudera cloudera   770 Dec 26 23:11 orders$4.class
-rw-rw-r-- 1 cloudera cloudera   773 Dec 26 23:11 orders$1.class
-rw-rw-r-- 1 cloudera cloudera   283 Dec 26 23:11 orders$FieldSetterCommand.class
-rw-rw-r-- 1 cloudera cloudera 14068 Dec 26 23:11 order_items.class
-rw-rw-r-- 1 cloudera cloudera   808 Dec 26 23:11 order_items$4.class
-rw-rw-r-- 1 cloudera cloudera   802 Dec 26 23:11 order_items$5.class
-rw-rw-r-- 1 cloudera cloudera   802 Dec 26 23:11 order_items$6.class
-rw-rw-r-- 1 cloudera cloudera   808 Dec 26 23:11 order_items$1.class
-rw-rw-r-- 1 cloudera cloudera   808 Dec 26 23:11 order_items$2.class
-rw-rw-r-- 1 cloudera cloudera   808 Dec 26 23:11 order_items$3.class
-rw-rw-r-- 1 cloudera cloudera   298 Dec 26 23:11 order_items$FieldSetterCommand.class
-rw-rw-r-- 1 cloudera cloudera  9719 Dec 26 23:11 departments.class
-rw-rw-r-- 1 cloudera cloudera   808 Dec 26 23:11 departments$1.class
-rw-rw-r-- 1 cloudera cloudera   805 Dec 26 23:11 departments$2.class
-rw-rw-r-- 1 cloudera cloudera   298 Dec 26 23:11 departments$FieldSetterCommand.class
-rw-rw-r-- 1 cloudera cloudera 16365 Dec 26 23:11 customers.class
-rw-rw-r-- 1 cloudera cloudera   791 Dec 26 23:11 customers$7.class
-rw-rw-r-- 1 cloudera cloudera   791 Dec 26 23:11 customers$8.class
-rw-rw-r-- 1 cloudera cloudera   791 Dec 26 23:11 customers$9.class
-rw-rw-r-- 1 cloudera cloudera   791 Dec 26 23:11 customers$3.class
-rw-rw-r-- 1 cloudera cloudera   791 Dec 26 23:11 customers$4.class
-rw-rw-r-- 1 cloudera cloudera   791 Dec 26 23:11 customers$5.class
-rw-rw-r-- 1 cloudera cloudera   791 Dec 26 23:11 customers$6.class
-rw-rw-r-- 1 cloudera cloudera   794 Dec 26 23:11 customers$1.class
-rw-rw-r-- 1 cloudera cloudera   791 Dec 26 23:11 customers$2.class
-rw-rw-r-- 1 cloudera cloudera   292 Dec 26 23:11 customers$FieldSetterCommand.class
-rw-rw-r-- 1 cloudera cloudera 10743 Dec 26 23:10 categories.class
-rw-rw-r-- 1 cloudera cloudera   798 Dec 26 23:10 categories$3.class
-rw-rw-r-- 1 cloudera cloudera   801 Dec 26 23:10 categories$1.class
-rw-rw-r-- 1 cloudera cloudera   801 Dec 26 23:10 categories$2.class
-rw-rw-r-- 1 cloudera cloudera   295 Dec 26 23:10 categories$FieldSetterCommand.class
~~~

* **1.4 - Verification of generated sequence file on HDFS**

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

* **2.4 - Verification of tables under `retail_db_parquet` database in Hive**

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