## Cloudera QuickStart VM - Retail DataSet Setup (AVRO File)

### Step-0: Login To VM
~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Sun Nov 26 17:58:32 2017 from 192.168.211.1
[cloudera@quickstart ~]$
~~~

### Step-1: Importing all tables available under retail_db database of MySQL to HDFS (in .avro format)

* **1.1 - Execute below sqoop command**

~~~
[cloudera@quickstart ~]$ sqoop import-all-tables \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--num-mappers 1 \
--warehouse-dir /user/cloudera/sqoop/import-all-tables-avro \
--as-avrodatafile
~~~

* **1.2 - Verify retail_db dataset on HDFS**

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

* **1.3 - Print schema details of avro file**

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
~~~

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
~~~

~~~
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
~~~

~~~
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
~~~

~~~
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
~~~

~~~
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
~~~

~~~
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

* **1.4 - Generate avro schema from avro file**

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

* **1.5 - Copy avro schema from local file system to HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -put /home/cloudera/retail_db_avro_schema /user/cloudera
[cloudera@quickstart ~]$ hadoop fs -ls /user/cloudera/retail_db_avro_schema
-rw-r--r--   1 cloudera cloudera        595 2017-12-25 07:10 /user/cloudera/retail_db_avro_schema/categories.avsc
-rw-r--r--   1 cloudera cloudera       1510 2017-12-25 07:10 /user/cloudera/retail_db_avro_schema/customers.avsc
-rw-r--r--   1 cloudera cloudera        441 2017-12-25 07:10 /user/cloudera/retail_db_avro_schema/departments.avsc
-rw-r--r--   1 cloudera cloudera       1100 2017-12-25 07:10 /user/cloudera/retail_db_avro_schema/order_items.avsc
-rw-r--r--   1 cloudera cloudera        708 2017-12-25 07:10 /user/cloudera/retail_db_avro_schema/orders.avsc
-rw-r--r--   1 cloudera cloudera       1042 2017-12-25 07:10 /user/cloudera/retail_db_avro_schema/products.avsc
~~~

* **1.6 - Creation of database called retail_db_avro and required tables in Hive**

~~~
[cloudera@quickstart ~]$ hive

Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.
hive> show databases;
default
retail_db

hive> create database if not exists retail_db_avro;

hive> use retail_db_avro;

hive> set hive.cli.print.current.db=true;

hive (retail_db_avro)> create external table if not exists categories
stored as avro
location '/user/cloudera/sqoop/import-all-tables-avro/categories'
tblproperties ('avro.schema.url'='/user/cloudera/retail_db_avro_schema/categories.avsc');

hive (retail_db_avro)> create external table if not exists customers
stored as avro
location '/user/cloudera/sqoop/import-all-tables-avro/customers'
tblproperties ('avro.schema.url'='/user/cloudera/retail_db_avro_schema/customers.avsc');

hive (retail_db_avro)> create external table if not exists departments
stored as avro
location '/user/cloudera/sqoop/import-all-tables-avro/departments'
tblproperties ('avro.schema.url'='/user/cloudera/retail_db_avro_schema/departments.avsc');

hive (retail_db_avro)> create external table if not exists order_items
stored as avro
location '/user/cloudera/sqoop/import-all-tables-avro/order_items'
tblproperties ('avro.schema.url'='/user/cloudera/retail_db_avro_schema/order_items.avsc');

hive (retail_db_avro)> create external table if not exists orders
stored as avro
location '/user/cloudera/sqoop/import-all-tables-avro/orders'
tblproperties ('avro.schema.url'='/user/cloudera/retail_db_avro_schema/orders.avsc');

hive (retail_db_avro)> create external table if not exists products
stored as avro
location '/user/cloudera/sqoop/import-all-tables-avro/products'
tblproperties ('avro.schema.url'='/user/cloudera/retail_db_avro_schema/products.avsc');
~~~

* **1.7 - Verification of database retail_db_avro and created tables in Hive**

~~~
hive (retail_db_avro)> show tables;
categories
customers
departments
order_items
orders
products
~~~

~~~
hive (retail_db_avro)> describe formatted categories;
OK
# col_name              data_type             comment             
     
category_id           int                                       
category_department_id  int                                       
category_name         string                                    
     
# Detailed Table Information     
Database:             retail_db_avro         
Owner:                cloudera               
CreateTime:           Mon Dec 25 07:20:25 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-avro/categories  
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  avro.schema.url       /user/cloudera/retail_db_avro_schema/categories.avsc
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514215225          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.serde2.avro.AvroSerDe   
InputFormat:          org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat   
OutputFormat:         org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat  
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.237 seconds, Fetched: 35 row(s)
~~~

~~~
hive (retail_db_avro)> describe formatted customers;
OK
# col_name              data_type             comment             
     
customer_id           int                                       
customer_fname        string                                    
customer_lname        string                                    
customer_email        string                                    
customer_password     string                                    
customer_street       string                                    
customer_city         string                                    
customer_state        string                                    
customer_zipcode      string                                    
     
# Detailed Table Information     
Database:             retail_db_avro         
Owner:                cloudera               
CreateTime:           Mon Dec 25 07:36:56 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-avro/customers   
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  avro.schema.url       /user/cloudera/retail_db_avro_schema/customers.avsc
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514216216          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.serde2.avro.AvroSerDe   
InputFormat:          org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat   
OutputFormat:         org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat  
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.098 seconds, Fetched: 41 row(s)
~~~

~~~
hive (retail_db_avro)> describe formatted departments;
OK
# col_name              data_type             comment             
     
department_id         int                                       
department_name       string                                    
     
# Detailed Table Information     
Database:             retail_db_avro         
Owner:                cloudera               
CreateTime:           Mon Dec 25 07:38:03 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-avro/departments   
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  avro.schema.url       /user/cloudera/retail_db_avro_schema/departments.avsc
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514216283          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.serde2.avro.AvroSerDe   
InputFormat:          org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat   
OutputFormat:         org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat  
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.081 seconds, Fetched: 34 row(s)
~~~

~~~
hive (retail_db_avro)> describe formatted order_items;
OK
# col_name              data_type             comment             
     
order_item_id         int                                       
order_item_order_id   int                                       
order_item_product_id int                                       
order_item_quantity   int                                       
order_item_subtotal   float                                     
order_item_product_price  float                                     
     
# Detailed Table Information     
Database:             retail_db_avro         
Owner:                cloudera               
CreateTime:           Mon Dec 25 07:38:53 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-avro/order_items   
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  avro.schema.url       /user/cloudera/retail_db_avro_schema/order_items.avsc
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514216333          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.serde2.avro.AvroSerDe   
InputFormat:          org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat   
OutputFormat:         org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat  
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.09 seconds, Fetched: 38 row(s)
~~~

~~~
hive (retail_db_avro)> describe formatted orders;
OK
# col_name              data_type             comment             
     
order_id              int                                       
order_date            bigint                                    
order_customer_id     int                                       
order_status          string                                    
     
# Detailed Table Information     
Database:             retail_db_avro         
Owner:                cloudera               
CreateTime:           Mon Dec 25 07:39:42 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-avro/orders  
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  avro.schema.url       /user/cloudera/retail_db_avro_schema/orders.avsc
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514216382          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.serde2.avro.AvroSerDe   
InputFormat:          org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat   
OutputFormat:         org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat  
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.099 seconds, Fetched: 36 row(s)
~~~

~~~
hive (retail_db_avro)> describe formatted products;
OK
# col_name              data_type             comment             
     
product_id            int                                       
product_category_id   int                                       
product_name          string                                    
product_description   string                                    
product_price         float                                     
product_image         string                                    
     
# Detailed Table Information     
Database:             retail_db_avro         
Owner:                cloudera               
CreateTime:           Mon Dec 25 07:41:06 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import-all-tables-avro/products  
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  avro.schema.url       /user/cloudera/retail_db_avro_schema/products.avsc
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514216466          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.serde2.avro.AvroSerDe   
InputFormat:          org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat   
OutputFormat:         org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat  
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.08 seconds, Fetched: 38 row(s)
~~~

* **1.8 - Execute join query in Hive**

~~~
hive (retail_db_avro)> SET hive.auto.convert.join=false;

hive (retail_db_avro)> select o.order_date, sum(oi.order_item_subtotal)
from orders o join order_items oi on (o.order_id = oi.order_item_order_id)
group by o.order_date 
limit 10;

Query ID = cloudera_20171225074848_c6737bcf-b36d-4cf5-9631-5e852f4a10e6
Total jobs = 2
Launching Job 1 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514175865139_0008, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514175865139_0008/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514175865139_0008
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2017-12-25 07:48:13,230 Stage-1 map = 0%,  reduce = 0%
2017-12-25 07:48:21,576 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 7.94 sec
2017-12-25 07:48:27,839 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 10.5 sec
MapReduce Total cumulative CPU time: 10 seconds 500 msec
Ended Job = job_1514175865139_0008
Launching Job 2 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514175865139_0009, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514175865139_0009/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514175865139_0009
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2017-12-25 07:48:33,943 Stage-2 map = 0%,  reduce = 0%
2017-12-25 07:48:38,128 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 1.04 sec
2017-12-25 07:48:44,415 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 2.18 sec
MapReduce Total cumulative CPU time: 2 seconds 180 msec
Ended Job = job_1514175865139_0009
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 10.5 sec   HDFS Read: 5756040 HDFS Write: 11844 SUCCESS
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 2.18 sec   HDFS Read: 17051 HDFS Write: 327 SUCCESS
Total MapReduce CPU Time Spent: 12 seconds 680 msec
OK
1374735600000 68153.83132743835
1374822000000 136520.17266082764
1374908400000 101074.34193611145
1374994800000 87123.08192253113
1375081200000 137287.09244918823
1375167600000 102745.62186431885
1375254000000 131878.06256484985
1375340400000 129001.62241744995
1375426800000 109347.00200462341
1375513200000 95266.89186286926
Time taken: 37.569 seconds, Fetched: 10 row(s)
~~~