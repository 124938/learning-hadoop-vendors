## Cloudera QuickStart VM - Retail DataSet Setup (In PARQUET file format)

### Step-0: Login To VM
~~~
asus@asus-GL553VD:~$ ssh cloudera@192.168.211.142
cloudera@192.168.211.142's password: 
Last login: Sun Dec 24 02:37:11 2017 from 192.168.211.1
[cloudera@quickstart ~]$
~~~

### Step-1: Importing all tables available under retail_db database of MySQL to HDFS in .parquet format

* **1.1 - Execute below sqoop command**

~~~
[cloudera@quickstart ~]$ sqoop import-all-tables \
--connect jdbc:mysql://quickstart.cloudera:3306/retail_db \
--username retail_dba \
--password cloudera \
--num-mappers 1 \
--warehouse-dir /user/cloudera/sqoop/import_all_tables_parquet \
--as-parquetfile
~~~

* **1.2 - Verify .parquet files on HDFS**

~~~
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/cloudera/sqoop/import_all_tables_parquet
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/categories
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:13 /user/cloudera/sqoop/import_all_tables_parquet/categories/.metadata
-rw-r--r--   1 cloudera cloudera        213 2017-12-26 03:13 /user/cloudera/sqoop/import_all_tables_parquet/categories/.metadata/descriptor.properties
-rw-r--r--   1 cloudera cloudera        594 2017-12-26 03:13 /user/cloudera/sqoop/import_all_tables_parquet/categories/.metadata/schema.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:13 /user/cloudera/sqoop/import_all_tables_parquet/categories/.metadata/schemas
-rw-r--r--   1 cloudera cloudera        594 2017-12-26 03:13 /user/cloudera/sqoop/import_all_tables_parquet/categories/.metadata/schemas/1.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/categories/.signals
-rw-r--r--   1 cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/categories/.signals/unbounded
-rw-r--r--   1 cloudera cloudera       1957 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/categories/9fee9acf-6c12-472c-8efe-c1983966604b.parquet
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/customers
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/customers/.metadata
-rw-r--r--   1 cloudera cloudera        211 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/customers/.metadata/descriptor.properties
-rw-r--r--   1 cloudera cloudera       1509 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/customers/.metadata/schema.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/customers/.metadata/schemas
-rw-r--r--   1 cloudera cloudera       1509 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/customers/.metadata/schemas/1.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/customers/.signals
-rw-r--r--   1 cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/customers/.signals/unbounded
-rw-r--r--   1 cloudera cloudera     254648 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/customers/61663bea-0e07-4932-b6e3-d78760ecd400.parquet
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/departments
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/departments/.metadata
-rw-r--r--   1 cloudera cloudera        215 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/departments/.metadata/descriptor.properties
-rw-r--r--   1 cloudera cloudera        440 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/departments/.metadata/schema.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/departments/.metadata/schemas
-rw-r--r--   1 cloudera cloudera        440 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/departments/.metadata/schemas/1.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/departments/.signals
-rw-r--r--   1 cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/departments/.signals/unbounded
-rw-r--r--   1 cloudera cloudera        801 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/departments/22fe6dd0-ceaa-4022-8e97-a1b51f12f3b3.parquet
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/order_items
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/order_items/.metadata
-rw-r--r--   1 cloudera cloudera        215 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/order_items/.metadata/descriptor.properties
-rw-r--r--   1 cloudera cloudera       1099 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/order_items/.metadata/schema.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/order_items/.metadata/schemas
-rw-r--r--   1 cloudera cloudera       1099 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/order_items/.metadata/schemas/1.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/order_items/.signals
-rw-r--r--   1 cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/order_items/.signals/unbounded
-rw-r--r--   1 cloudera cloudera    1647133 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/order_items/67196bf6-d5ea-423b-b069-1e885a03aee7.parquet
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/orders
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/orders/.metadata
-rw-r--r--   1 cloudera cloudera        205 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/orders/.metadata/descriptor.properties
-rw-r--r--   1 cloudera cloudera        707 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/orders/.metadata/schema.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/orders/.metadata/schemas
-rw-r--r--   1 cloudera cloudera        707 2017-12-26 03:14 /user/cloudera/sqoop/import_all_tables_parquet/orders/.metadata/schemas/1.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/orders/.signals
-rw-r--r--   1 cloudera cloudera          0 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/orders/.signals/unbounded
-rw-r--r--   1 cloudera cloudera     488257 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/orders/00aafa65-0fbf-4b77-a43d-08ba88db9779.parquet
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/products
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/products/.metadata
-rw-r--r--   1 cloudera cloudera        209 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/products/.metadata/descriptor.properties
-rw-r--r--   1 cloudera cloudera       1041 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/products/.metadata/schema.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/products/.metadata/schemas
-rw-r--r--   1 cloudera cloudera       1041 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/products/.metadata/schemas/1.avsc
drwxr-xr-x   - cloudera cloudera          0 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/products/.signals
-rw-r--r--   1 cloudera cloudera          0 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/products/.signals/unbounded
-rw-r--r--   1 cloudera cloudera      44856 2017-12-26 03:15 /user/cloudera/sqoop/import_all_tables_parquet/products/e027bf51-7558-4834-9cdb-5a4b6a42f652.parquet
~~~

* **1.3 - Dump schema of parquet file on terminal**

~~~
[cloudera@quickstart ~]$ parquet-tools schema --detailed hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/categories/9fee9acf-6c12-472c-8efe-c1983966604b.parquet
message categories {
  optional int32 category_id;
  optional int32 category_department_id;
  optional binary category_name (UTF8);
}

creator: parquet-mr version 1.5.0-cdh5.12.0 (build ${buildNumber})
extra: parquet.avro.schema = {"type":"record","name":"categories","doc":"Sqoop import of categories","fields":[{"name":"category_id","type":["null","int"],"default":null,"columnName":"category_id","sqlType":"4"},{"name":"category_department_id","type":["null","int"],"default":null,"columnName":"category_department_id","sqlType":"4"},{"name":"category_name","type":["null","string"],"default":null,"columnName":"category_name","sqlType":"12"}],"tableName":"categories"}

file schema: categories
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
category_id: OPTIONAL INT32 R:0 D:1
category_department_id: OPTIONAL INT32 R:0 D:1
category_name: OPTIONAL BINARY O:UTF8 R:0 D:1

row group 1: RC:58 TS:1353
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
category_id:  INT32 SNAPPY DO:0 FPO:4 SZ:276/273/0.99 VC:58 ENC:PLAIN,RLE,BIT_PACKED
category_department_id:  INT32 SNAPPY DO:0 FPO:280 SZ:104/100/0.96 VC:58 ENC:RLE,PLAIN_DICTIONARY,BIT_PACKED
category_name:  BINARY SNAPPY DO:0 FPO:384 SZ:741/980/1.32 VC:58 ENC:PLAIN,RLE,BIT_PACKED
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools schema --detailed hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/customers/61663bea-0e07-4932-b6e3-d78760ecd400.parquet
message customers {
  optional int32 customer_id;
  optional binary customer_fname (UTF8);
  optional binary customer_lname (UTF8);
  optional binary customer_email (UTF8);
  optional binary customer_password (UTF8);
  optional binary customer_street (UTF8);
  optional binary customer_city (UTF8);
  optional binary customer_state (UTF8);
  optional binary customer_zipcode (UTF8);
}

creator: parquet-mr version 1.5.0-cdh5.12.0 (build ${buildNumber})
extra: parquet.avro.schema = {"type":"record","name":"customers","doc":"Sqoop import of customers","fields":[{"name":"customer_id","type":["null","int"],"default":null,"columnName":"customer_id","sqlType":"4"},{"name":"customer_fname","type":["null","string"],"default":null,"columnName":"customer_fname","sqlType":"12"},{"name":"customer_lname","type":["null","string"],"default":null,"columnName":"customer_lname","sqlType":"12"},{"name":"customer_email","type":["null","string"],"default":null,"columnName":"customer_email","sqlType":"12"},{"name":"customer_password","type":["null","string"],"default":null,"columnName":"customer_password","sqlType":"12"},{"name":"customer_street","type":["null","string"],"default":null,"columnName":"customer_street","sqlType":"12"},{"name":"customer_city","type":["null","string"],"default":null,"columnName":"customer_city","sqlType":"12"},{"name":"customer_state","type":["null","string"],"default":null,"columnName":"customer_state","sqlType":"12"},{"name":"customer_zipcode","type":["null","string"],"default":null,"columnName":"customer_zipcode","sqlType":"12"}],"tableName":"customers"}

file schema: customers
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
customer_id: OPTIONAL INT32 R:0 D:1
customer_fname: OPTIONAL BINARY O:UTF8 R:0 D:1
customer_lname: OPTIONAL BINARY O:UTF8 R:0 D:1
customer_email: OPTIONAL BINARY O:UTF8 R:0 D:1
customer_password: OPTIONAL BINARY O:UTF8 R:0 D:1
customer_street: OPTIONAL BINARY O:UTF8 R:0 D:1
customer_city: OPTIONAL BINARY O:UTF8 R:0 D:1
customer_state: OPTIONAL BINARY O:UTF8 R:0 D:1
customer_zipcode: OPTIONAL BINARY O:UTF8 R:0 D:1

row group 1: RC:12435 TS:333855
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
customer_id:  INT32 SNAPPY DO:0 FPO:4 SZ:49783/49787/1.00 VC:12435 ENC:RLE,PLAIN,BIT_PACKED
customer_fname:  BINARY SNAPPY DO:0 FPO:49787 SZ:14043/14512/1.03 VC:12435 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
customer_lname:  BINARY SNAPPY DO:0 FPO:63830 SZ:22763/25572/1.12 VC:12435 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
customer_email:  BINARY SNAPPY DO:0 FPO:86593 SZ:87/83/0.95 VC:12435 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
customer_password:  BINARY SNAPPY DO:0 FPO:86680 SZ:87/83/0.95 VC:12435 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
customer_street:  BINARY SNAPPY DO:0 FPO:86767 SZ:114293/186626/1.63 VC:12435 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
customer_city:  BINARY SNAPPY DO:0 FPO:201060 SZ:20984/22872/1.09 VC:12435 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
customer_state:  BINARY SNAPPY DO:0 FPO:222044 SZ:9593/9678/1.01 VC:12435 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
customer_zipcode:  BINARY SNAPPY DO:0 FPO:231637 SZ:20926/24642/1.18 VC:12435 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools schema --detailed hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/departments/22fe6dd0-ceaa-4022-8e97-a1b51f12f3b3.parquet
message departments {
  optional int32 department_id;
  optional binary department_name (UTF8);
}

creator: parquet-mr version 1.5.0-cdh5.12.0 (build ${buildNumber})
extra: parquet.avro.schema = {"type":"record","name":"departments","doc":"Sqoop import of departments","fields":[{"name":"department_id","type":["null","int"],"default":null,"columnName":"department_id","sqlType":"4"},{"name":"department_name","type":["null","string"],"default":null,"columnName":"department_name","sqlType":"12"}],"tableName":"departments"}

file schema: departments
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
department_id: OPTIONAL INT32 R:0 D:1
department_name: OPTIONAL BINARY O:UTF8 R:0 D:1

row group 1: RC:6 TS:177
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
department_id:  INT32 SNAPPY DO:0 FPO:4 SZ:63/63/1.00 VC:6 ENC:RLE,BIT_PACKED,PLAIN
department_name:  BINARY SNAPPY DO:0 FPO:67 SZ:113/114/1.01 VC:6 ENC:RLE,BIT_PACKED,PLAIN
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools schema --detailed hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/order_items/67196bf6-d5ea-423b-b069-1e885a03aee7.parquet
message order_items {
  optional int32 order_item_id;
  optional int32 order_item_order_id;
  optional int32 order_item_product_id;
  optional int32 order_item_quantity;
  optional float order_item_subtotal;
  optional float order_item_product_price;
}

creator: parquet-mr version 1.5.0-cdh5.12.0 (build ${buildNumber})
extra: parquet.avro.schema = {"type":"record","name":"order_items","doc":"Sqoop import of order_items","fields":[{"name":"order_item_id","type":["null","int"],"default":null,"columnName":"order_item_id","sqlType":"4"},{"name":"order_item_order_id","type":["null","int"],"default":null,"columnName":"order_item_order_id","sqlType":"4"},{"name":"order_item_product_id","type":["null","int"],"default":null,"columnName":"order_item_product_id","sqlType":"4"},{"name":"order_item_quantity","type":["null","int"],"default":null,"columnName":"order_item_quantity","sqlType":"-6"},{"name":"order_item_subtotal","type":["null","float"],"default":null,"columnName":"order_item_subtotal","sqlType":"7"},{"name":"order_item_product_price","type":["null","float"],"default":null,"columnName":"order_item_product_price","sqlType":"7"}],"tableName":"order_items"}

file schema: order_items
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
order_item_id: OPTIONAL INT32 R:0 D:1
order_item_order_id: OPTIONAL INT32 R:0 D:1
order_item_product_id: OPTIONAL INT32 R:0 D:1
order_item_quantity: OPTIONAL INT32 R:0 D:1
order_item_subtotal: OPTIONAL FLOAT R:0 D:1
order_item_product_price: OPTIONAL FLOAT R:0 D:1

row group 1: RC:172198 TS:1782812
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
order_item_id:  INT32 SNAPPY DO:0 FPO:4 SZ:688891/688839/1.00 VC:172198 ENC:PLAIN,RLE,BIT_PACKED
order_item_order_id:  INT32 SNAPPY DO:0 FPO:688895 SZ:437431/574533/1.31 VC:172198 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
order_item_product_id:  INT32 SNAPPY DO:0 FPO:1126326 SZ:151619/151481/1.00 VC:172198 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
order_item_quantity:  INT32 SNAPPY DO:0 FPO:1277945 SZ:64876/64870/1.00 VC:172198 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
order_item_subtotal:  FLOAT SNAPPY DO:0 FPO:1342821 SZ:172955/173306/1.00 VC:172198 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
order_item_product_price:  FLOAT SNAPPY DO:0 FPO:1515776 SZ:129822/129783/1.00 VC:172198 ENC:RLE,BIT_PACKED,PLAIN_DICTIONARY
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools schema --detailed hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/orders/00aafa65-0fbf-4b77-a43d-08ba88db9779.parquet
message orders {
  optional int32 order_id;
  optional int64 order_date;
  optional int32 order_customer_id;
  optional binary order_status (UTF8);
}

creator: parquet-mr version 1.5.0-cdh5.12.0 (build ${buildNumber})
extra: parquet.avro.schema = {"type":"record","name":"orders","doc":"Sqoop import of orders","fields":[{"name":"order_id","type":["null","int"],"default":null,"columnName":"order_id","sqlType":"4"},{"name":"order_date","type":["null","long"],"default":null,"columnName":"order_date","sqlType":"93"},{"name":"order_customer_id","type":["null","int"],"default":null,"columnName":"order_customer_id","sqlType":"4"},{"name":"order_status","type":["null","string"],"default":null,"columnName":"order_status","sqlType":"12"}],"tableName":"orders"}

file schema: orders
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
order_id: OPTIONAL INT32 R:0 D:1
order_date: OPTIONAL INT64 R:0 D:1
order_customer_id: OPTIONAL INT32 R:0 D:1
order_status: OPTIONAL BINARY O:UTF8 R:0 D:1

row group 1: RC:68883 TS:487926
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
order_id:  INT32 SNAPPY DO:0 FPO:4 SZ:275594/275579/1.00 VC:68883 ENC:PLAIN,BIT_PACKED,RLE
order_date:  INT64 SNAPPY DO:0 FPO:275598 SZ:6483/7186/1.11 VC:68883 ENC:BIT_PACKED,PLAIN_DICTIONARY,RLE
order_customer_id:  INT32 SNAPPY DO:0 FPO:282081 SZ:170413/170378/1.00 VC:68883 ENC:BIT_PACKED,PLAIN_DICTIONARY,RLE
order_status:  BINARY SNAPPY DO:0 FPO:452494 SZ:34779/34783/1.00 VC:68883 ENC:BIT_PACKED,PLAIN_DICTIONARY,RLE
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools schema --detailed hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/products/e027bf51-7558-4834-9cdb-5a4b6a42f652.parquet
message products {
  optional int32 product_id;
  optional int32 product_category_id;
  optional binary product_name (UTF8);
  optional binary product_description (UTF8);
  optional float product_price;
  optional binary product_image (UTF8);
}

creator: parquet-mr version 1.5.0-cdh5.12.0 (build ${buildNumber})
extra: parquet.avro.schema = {"type":"record","name":"products","doc":"Sqoop import of products","fields":[{"name":"product_id","type":["null","int"],"default":null,"columnName":"product_id","sqlType":"4"},{"name":"product_category_id","type":["null","int"],"default":null,"columnName":"product_category_id","sqlType":"4"},{"name":"product_name","type":["null","string"],"default":null,"columnName":"product_name","sqlType":"12"},{"name":"product_description","type":["null","string"],"default":null,"columnName":"product_description","sqlType":"12"},{"name":"product_price","type":["null","float"],"default":null,"columnName":"product_price","sqlType":"7"},{"name":"product_image","type":["null","string"],"default":null,"columnName":"product_image","sqlType":"12"}],"tableName":"products"}

file schema: products
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
product_id: OPTIONAL INT32 R:0 D:1
product_category_id: OPTIONAL INT32 R:0 D:1
product_name: OPTIONAL BINARY O:UTF8 R:0 D:1
product_description: OPTIONAL BINARY O:UTF8 R:0 D:1
product_price: OPTIONAL FLOAT R:0 D:1
product_image: OPTIONAL BINARY O:UTF8 R:0 D:1

row group 1: RC:1345 TS:103792
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
product_id:  INT32 SNAPPY DO:0 FPO:4 SZ:5424/5423/1.00 VC:1345 ENC:RLE,PLAIN,BIT_PACKED
product_category_id:  INT32 SNAPPY DO:0 FPO:5428 SZ:405/398/0.98 VC:1345 ENC:RLE,PLAIN_DICTIONARY,BIT_PACKED
product_name:  BINARY SNAPPY DO:0 FPO:5833 SZ:16270/33473/2.06 VC:1345 ENC:RLE,PLAIN_DICTIONARY,BIT_PACKED
product_description:  BINARY SNAPPY DO:0 FPO:22103 SZ:57/53/0.93 VC:1345 ENC:RLE,PLAIN_DICTIONARY,BIT_PACKED
product_price:  FLOAT SNAPPY DO:0 FPO:22160 SZ:1952/1994/1.02 VC:1345 ENC:RLE,PLAIN_DICTIONARY,BIT_PACKED
product_image:  BINARY SNAPPY DO:0 FPO:24112 SZ:19215/62451/3.25 VC:1345 ENC:RLE,PLAIN_DICTIONARY,BIT_PACKED
~~~

* **1.4 - Dump parquet file in JSON format on terminal**

~~~
[cloudera@quickstart ~]$ parquet-tools cat --json hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/categories/9fee9acf-6c12-472c-8efe-c1983966604b.parquet | tail
{"category_id":49,"category_department_id":8,"category_name":"MLB"}
{"category_id":50,"category_department_id":8,"category_name":"NFL"}
{"category_id":51,"category_department_id":8,"category_name":"NHL"}
{"category_id":52,"category_department_id":8,"category_name":"NBA"}
{"category_id":53,"category_department_id":8,"category_name":"NCAA"}
{"category_id":54,"category_department_id":8,"category_name":"MLS"}
{"category_id":55,"category_department_id":8,"category_name":"International Soccer"}
{"category_id":56,"category_department_id":8,"category_name":"World Cup Shop"}
{"category_id":57,"category_department_id":8,"category_name":"MLB Players"}
{"category_id":58,"category_department_id":8,"category_name":"NFL Players"}
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools cat --json hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/customers/61663bea-0e07-4932-b6e3-d78760ecd400.parquet | tail
{"customer_id":12426,"customer_fname":"Jordan","customer_lname":"Valdez","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"5561 Quiet Loop","customer_city":"Brooklyn","customer_state":"NY","customer_zipcode":"11210"}
{"customer_id":12427,"customer_fname":"Mary","customer_lname":"Smith","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"3662 Round Barn Gate","customer_city":"Plano","customer_state":"TX","customer_zipcode":"75093"}
{"customer_id":12428,"customer_fname":"Jeffrey","customer_lname":"Travis","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"1552 Burning Dale Highlands","customer_city":"Caguas","customer_state":"PR","customer_zipcode":"00725"}
{"customer_id":12429,"customer_fname":"Mary","customer_lname":"Smith","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"92 Sunny Bear Villas","customer_city":"Gardena","customer_state":"CA","customer_zipcode":"90247"}
{"customer_id":12430,"customer_fname":"Hannah","customer_lname":"Brown","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"8316 Pleasant Bend","customer_city":"Caguas","customer_state":"PR","customer_zipcode":"00725"}
{"customer_id":12431,"customer_fname":"Mary","customer_lname":"Rios","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"1221 Cinder Pines","customer_city":"Kaneohe","customer_state":"HI","customer_zipcode":"96744"}
{"customer_id":12432,"customer_fname":"Angela","customer_lname":"Smith","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"1525 Jagged Barn Highlands","customer_city":"Caguas","customer_state":"PR","customer_zipcode":"00725"}
{"customer_id":12433,"customer_fname":"Benjamin","customer_lname":"Garcia","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"5459 Noble Brook Landing","customer_city":"Levittown","customer_state":"NY","customer_zipcode":"11756"}
{"customer_id":12434,"customer_fname":"Mary","customer_lname":"Mills","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"9720 Colonial Parade","customer_city":"Caguas","customer_state":"PR","customer_zipcode":"00725"}
{"customer_id":12435,"customer_fname":"Laura","customer_lname":"Horton","customer_email":"XXXXXXXXX","customer_password":"XXXXXXXXX","customer_street":"5736 Honey Downs","customer_city":"Summerville","customer_state":"SC","customer_zipcode":"29483"}
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools cat --json hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/departments/22fe6dd0-ceaa-4022-8e97-a1b51f12f3b3.parquet | tail
{"department_id":2,"department_name":"Fitness"}
{"department_id":3,"department_name":"Footwear"}
{"department_id":4,"department_name":"Apparel"}
{"department_id":5,"department_name":"Golf"}
{"department_id":6,"department_name":"Outdoors"}
{"department_id":7,"department_name":"Fan Shop"}
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools cat --json hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/order_items/67196bf6-d5ea-423b-b069-1e885a03aee7.parquet | tail
{"order_item_id":172189,"order_item_order_id":68880,"order_item_product_id":1014,"order_item_quantity":3,"order_item_subtotal":149.94,"order_item_product_price":49.98}
{"order_item_id":172190,"order_item_order_id":68880,"order_item_product_id":502,"order_item_quantity":5,"order_item_subtotal":250.0,"order_item_product_price":50.0}
{"order_item_id":172191,"order_item_order_id":68880,"order_item_product_id":1073,"order_item_quantity":1,"order_item_subtotal":199.99,"order_item_product_price":199.99}
{"order_item_id":172192,"order_item_order_id":68880,"order_item_product_id":1014,"order_item_quantity":5,"order_item_subtotal":249.9,"order_item_product_price":49.98}
{"order_item_id":172193,"order_item_order_id":68880,"order_item_product_id":1014,"order_item_quantity":3,"order_item_subtotal":149.94,"order_item_product_price":49.98}
{"order_item_id":172194,"order_item_order_id":68881,"order_item_product_id":403,"order_item_quantity":1,"order_item_subtotal":129.99,"order_item_product_price":129.99}
{"order_item_id":172195,"order_item_order_id":68882,"order_item_product_id":365,"order_item_quantity":1,"order_item_subtotal":59.99,"order_item_product_price":59.99}
{"order_item_id":172196,"order_item_order_id":68882,"order_item_product_id":502,"order_item_quantity":1,"order_item_subtotal":50.0,"order_item_product_price":50.0}
{"order_item_id":172197,"order_item_order_id":68883,"order_item_product_id":208,"order_item_quantity":1,"order_item_subtotal":1999.99,"order_item_product_price":1999.99}
{"order_item_id":172198,"order_item_order_id":68883,"order_item_product_id":502,"order_item_quantity":3,"order_item_subtotal":150.0,"order_item_product_price":50.0}
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools cat --json hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/orders/00aafa65-0fbf-4b77-a43d-08ba88db9779.parquet | tail
{"order_id":68874,"order_date":1404370800000,"order_customer_id":1601,"order_status":"COMPLETE"}
{"order_id":68875,"order_date":1404457200000,"order_customer_id":10637,"order_status":"ON_HOLD"}
{"order_id":68876,"order_date":1404630000000,"order_customer_id":4124,"order_status":"COMPLETE"}
{"order_id":68877,"order_date":1404716400000,"order_customer_id":9692,"order_status":"ON_HOLD"}
{"order_id":68878,"order_date":1404802800000,"order_customer_id":6753,"order_status":"COMPLETE"}
{"order_id":68879,"order_date":1404889200000,"order_customer_id":778,"order_status":"COMPLETE"}
{"order_id":68880,"order_date":1405234800000,"order_customer_id":1117,"order_status":"COMPLETE"}
{"order_id":68881,"order_date":1405753200000,"order_customer_id":2518,"order_status":"PENDING_PAYMENT"}
{"order_id":68882,"order_date":1406012400000,"order_customer_id":10000,"order_status":"ON_HOLD"}
{"order_id":68883,"order_date":1406098800000,"order_customer_id":5533,"order_status":"COMPLETE"}
~~~

~~~
[cloudera@quickstart ~]$ parquet-tools cat --json hdfs://quickstart.cloudera/user/cloudera/sqoop/import_all_tables_parquet/products/e027bf51-7558-4834-9cdb-5a4b6a42f652.parquet | tail
{"product_id":1336,"product_category_id":59,"product_name":"Nike Men's Home Game Jersey New York Giants O","product_description":"","product_price":100.0,"product_image":"http://images.acmesports.sports/Nike+Men%27s+Home+Game+Jersey+New+York+Giants+Odell+Beckham..."}
{"product_id":1337,"product_category_id":59,"product_name":"Nike Men's Home Game Jersey Carolina Panthers","product_description":"","product_price":100.0,"product_image":"http://images.acmesports.sports/Nike+Men%27s+Home+Game+Jersey+Carolina+Panthers+Kelvin..."}
{"product_id":1338,"product_category_id":59,"product_name":"Nike Men's Home Game Jersey Chicago Bears Jar","product_description":"","product_price":100.0,"product_image":"http://images.acmesports.sports/Nike+Men%27s+Home+Game+Jersey+Chicago+Bears+Jared+Allen+%2369"}
{"product_id":1339,"product_category_id":59,"product_name":"Nike Men's Home Game Jersey New York Giants E","product_description":"","product_price":100.0,"product_image":"http://images.acmesports.sports/Nike+Men%27s+Home+Game+Jersey+New+York+Giants+Eli+Manning+%2310"}
{"product_id":1340,"product_category_id":59,"product_name":"Majestic Men's Replica Texas Rangers Russell ","product_description":"","product_price":69.97,"product_image":"http://images.acmesports.sports/Majestic+Men%27s+Replica+Texas+Rangers+Russell+Wilson+%233+Home..."}
{"product_id":1341,"product_category_id":59,"product_name":"Nike Women's Cleveland Browns Johnny Football","product_description":"","product_price":34.0,"product_image":"http://images.acmesports.sports/Nike+Women%27s+Cleveland+Browns+Johnny+Football+Orange+T-Shirt"}
{"product_id":1342,"product_category_id":59,"product_name":"Nike Men's St. Louis Rams Michael Sam #96 Nam","product_description":"","product_price":32.0,"product_image":"http://images.acmesports.sports/Nike+Men%27s+St.+Louis+Rams+Michael+Sam+%2396+Name+and+Number..."}
{"product_id":1343,"product_category_id":59,"product_name":"Nike Men's Home Game Jersey St. Louis Rams Mi","product_description":"","product_price":100.0,"product_image":"http://images.acmesports.sports/Nike+Men%27s+Home+Game+Jersey+St.+Louis+Rams+Michael+Sam+%2396"}
{"product_id":1344,"product_category_id":59,"product_name":"Nike Men's Home Game Jersey St. Louis Rams Aa","product_description":"","product_price":100.0,"product_image":"http://images.acmesports.sports/Nike+Men%27s+Home+Game+Jersey+St.+Louis+Rams+Aaron+Donald+%2399"}
{"product_id":1345,"product_category_id":59,"product_name":"Nike Men's Home Game Jersey St. Louis Rams Gr","product_description":"","product_price":100.0,"product_image":"http://images.acmesports.sports/Nike+Men%27s+Home+Game+Jersey+St.+Louis+Rams+Greg+Robinson..."}
~~~

### Step-2: `retail_db_parquet` database setup in HIVE

* **2.1 - Create database called `retail_db_parquet` in Hive**

~~~
[cloudera@quickstart ~]$ hive

Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j.properties
WARNING: Hive CLI is deprecated and migration to Beeline is recommended.
hive> show databases;
default
retail_db
retail_db_avro

hive> create database if not exists retail_db_parquet;

hive> use retail_db_parquet;

hive> set hive.cli.print.current.db=true;

hive (retail_db_parquet)> 
~~~

* **2.2 - Create tables under `retail_db_parquet` database in Hive**

~~~
hive (retail_db_parquet)> create external table if not exists categories(
category_id int,
category_department_id int,
category_name string)
stored as parquet
location '/user/cloudera/sqoop/import_all_tables_parquet/categories';
~~~

~~~
hive (retail_db_parquet)> create external table if not exists customers(
customer_id int,
customer_fname string,
customer_lname string,
customer_email string,
customer_password string,
customer_street string,
customer_city string,
customer_state string,
customer_zipcode string)
stored as parquet
location '/user/cloudera/sqoop/import_all_tables_parquet/customers';
~~~

~~~
hive (retail_db_parquet)> create external table if not exists departments(
department_id int,
department_name string)
stored as parquet
location '/user/cloudera/sqoop/import_all_tables_parquet/departments';
~~~

~~~
hive (retail_db_parquet)> create external table if not exists order_items(
order_item_id int,
order_item_order_id int,
order_item_product_id int,
order_item_quantity int,
order_item_subtotal float,
order_item_product_price float)
stored as parquet
location '/user/cloudera/sqoop/import_all_tables_parquet/order_items';
~~~

~~~
hive (retail_db_parquet)> create external table if not exists orders(
order_id int,
order_date bigint,
order_customer_id int,
order_status string)
stored as parquet
location '/user/cloudera/sqoop/import_all_tables_parquet/orders';
~~~

~~~
hive (retail_db_parquet)> create external table if not exists products(
product_id int,
product_category_id int,
product_name string,
product_description string,
product_price float,
product_image string)
stored as parquet
location '/user/cloudera/sqoop/import_all_tables_parquet/products';
~~~

* **2.3 - Verification of tables under `retail_db_parquet` database in Hive**

~~~
hive (retail_db_parquet)> show tables;
categories
customers
departments
order_items
orders
products
~~~

~~~
hive (retail_db_parquet)> describe formatted categories;
OK
# col_name              data_type             comment             
     
category_id           int                                       
category_department_id  int                                       
category_name         string                                    
     
# Detailed Table Information     
Database:             retail_db_parquet      
Owner:                cloudera               
CreateTime:           Tue Dec 26 04:37:36 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import_all_tables_parquet/categories   
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514291856          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe  
InputFormat:          org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat  
OutputFormat:         org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat   
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.079 seconds, Fetched: 34 row(s)
~~~

~~~
hive (retail_db_parquet)> describe formatted customers;
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
Database:             retail_db_parquet      
Owner:                cloudera               
CreateTime:           Tue Dec 26 04:39:57 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import_all_tables_parquet/customers  
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514291997          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe  
InputFormat:          org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat  
OutputFormat:         org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat   
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.098 seconds, Fetched: 40 row(s)
~~~

~~~
hive (retail_db_parquet)> describe formatted departments;
OK
# col_name              data_type             comment             
     
department_id         int                                       
department_name       string                                    
     
# Detailed Table Information     
Database:             retail_db_parquet      
Owner:                cloudera               
CreateTime:           Tue Dec 26 04:41:03 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import_all_tables_parquet/departments  
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514292063          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe  
InputFormat:          org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat  
OutputFormat:         org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat   
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.09 seconds, Fetched: 33 row(s)
~~~

~~~
hive (retail_db_parquet)> describe formatted order_items;
OK
# col_name              data_type             comment             
     
order_item_id         int                                       
order_item_order_id   int                                       
order_item_product_id int                                       
order_item_quantity   int                                       
order_item_subtotal   float                                     
order_item_product_price  float                                     
     
# Detailed Table Information     
Database:             retail_db_parquet      
Owner:                cloudera               
CreateTime:           Tue Dec 26 04:41:58 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import_all_tables_parquet/order_items  
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514292118          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe  
InputFormat:          org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat  
OutputFormat:         org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat   
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.066 seconds, Fetched: 37 row(s)
~~~

~~~
hive (retail_db_parquet)> describe formatted orders;
OK
# col_name              data_type             comment             
     
order_id              int                                       
order_date            bigint                                    
order_customer_id     int                                       
order_status          string                                    
     
# Detailed Table Information     
Database:             retail_db_parquet      
Owner:                cloudera               
CreateTime:           Tue Dec 26 05:09:13 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import_all_tables_parquet/orders   
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514293753          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe  
InputFormat:          org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat  
OutputFormat:         org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat   
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.065 seconds, Fetched: 35 row(s)
~~~

~~~
hive (retail_db_parquet)> describe formatted products;
OK
# col_name              data_type             comment             
     
product_id            int                                       
product_category_id   int                                       
product_name          string                                    
product_description   string                                    
product_price         float                                     
product_image         string                                    
     
# Detailed Table Information     
Database:             retail_db_parquet      
Owner:                cloudera               
CreateTime:           Tue Dec 26 04:47:34 PST 2017   
LastAccessTime:       UNKNOWN                
Protect Mode:         None                   
Retention:            0                      
Location:             hdfs://quickstart.cloudera:8020/user/cloudera/sqoop/import_all_tables_parquet/products   
Table Type:           EXTERNAL_TABLE         
Table Parameters:    
  COLUMN_STATS_ACCURATE false               
  EXTERNAL              TRUE                
  numFiles              0                   
  numRows               -1                  
  rawDataSize           -1                  
  totalSize             0                   
  transient_lastDdlTime 1514292454          
     
# Storage Information    
SerDe Library:        org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe  
InputFormat:          org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat  
OutputFormat:         org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat   
Compressed:           No                     
Num Buckets:          -1                     
Bucket Columns:       []                     
Sort Columns:         []                     
Storage Desc Params:     
  serialization.format  1                   
Time taken: 0.096 seconds, Fetched: 37 row(s)
~~~

* **2.5 - Execute join query in Hive**

~~~
hive (retail_db_parquet)> SET hive.auto.convert.join=false;

hive (retail_db_parquet)> select from_unixtime(CAST(o.order_date/1000 as BIGINT), 'yyyy-MM-dd'), sum(oi.order_item_subtotal)
from orders o join order_items oi on (o.order_id = oi.order_item_order_id)
group by o.order_date 
limit 10;

Query ID = cloudera_20171226052121_08bce0d9-6c35-464a-b822-f88202b0ef3f
Total jobs = 2
Launching Job 1 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514175865139_0017, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514175865139_0017/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514175865139_0017
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2017-12-26 05:21:05,690 Stage-1 map = 0%,  reduce = 0%
2017-12-26 05:21:14,051 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 7.08 sec
2017-12-26 05:21:19,246 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 9.67 sec
MapReduce Total cumulative CPU time: 9 seconds 670 msec
Ended Job = job_1514175865139_0017
Launching Job 2 out of 2
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1514175865139_0018, Tracking URL = http://quickstart.cloudera:8088/proxy/application_1514175865139_0018/
Kill Command = /usr/lib/hadoop/bin/hadoop job  -kill job_1514175865139_0018
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2017-12-26 05:21:24,907 Stage-2 map = 0%,  reduce = 0%
2017-12-26 05:21:29,091 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 0.81 sec
2017-12-26 05:21:35,370 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 2.01 sec
MapReduce Total cumulative CPU time: 2 seconds 10 msec
Ended Job = job_1514175865139_0018
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 9.67 sec   HDFS Read: 911421 HDFS Write: 11844 SUCCESS
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 2.01 sec   HDFS Read: 17051 HDFS Write: 327 SUCCESS
Total MapReduce CPU Time Spent: 11 seconds 680 msec
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
Time taken: 36.314 seconds, Fetched: 10 row(s)
~~~