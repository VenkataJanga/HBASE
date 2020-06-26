# HBASE
HBASE - integrate with mysql into Hbase and Hbase into Hive 


Assignment 1;- 
	1. Create a table in MYSQL and Import the data from HDFS into MYSQL table:
==============================================================================
		mysql> show databases;
		
		Step1: Create the table in mysql
		Create table hdfs_mysql_web_table(id int, username varchar(20),port_number varchar(20),host_address varchar(20),date_with_time varchar(20),hit_value1 varchar(20),hit_value2 varchar(20),hit_value3 varchar(20),timezone varchar(20),method varchar(20),product_type varchar(20),value varchar(20),sub_product varchar(20),web_info varchar(200),status_code varchar(20));
		
		Step2:
			Load the data from HDFS into MYSQL.
			load data infile '/home/cloudera/inputdata/1' into table hdfs_mysql_web_table fields terminated by ','
			load data infile '/user/cloudera/inputdata/1' into table hdfs_mysql_web_table fields terminated by ','
			
		Step3:
			Verify the data in mysql 
			select count(*) from hdfs_mysql_web_table;
		
		Step4:
			Load the data from HDFS into MYSQL.
			load data infile '/home/cloudera/inputdata/2' into table hdfs_mysql_web_table fields terminated by ','
		Step5:
			Verify the data in mysql 
			select count(*) from hdfs_mysql_web_table;
		Step6:
			Load the data from HDFS into MYSQL.
			load data infile '/home/cloudera/inputdata/3' into table hdfs_mysql_web_table fields terminated by ','
		Step7:
			Verify the data in mysql 
			select count(*) from hdfs_mysql_web_table;
		Step8:
			Load the data from HDFS into MYSQL.
			load data infile '/home/cloudera/inputdata/4' into table hdfs_mysql_web_table fields terminated by ','
		Step7:
			Verify the data in mysql 
			select count(*) from hdfs_mysql_web_table;
		Step8:
			Load the data from HDFS into MYSQL.
			load data infile '/home/cloudera/inputdata/5' into table hdfs_mysql_web_table fields terminated by ','
		Step7:
			Verify the data in mysql 
			select count(*) from hdfs_mysql_web_table;
			
		
	2.DATA INGESTION:
	================
		
		MYSQL hdfs_mysql_web_table DATA  into HBASE
		
		sqoop import --connect jdbc:mysql://localhost/zeyo_hbase_integration -username root --password cloudera --table hdfs_mysql_web_table --hbase-table hdfs_mysql_hbase_web_table --column-family cf --hbase-row-key id -m 1

	3. HBASE:-
	==========

	sudo service hbase-master restart
	sudo service hbase-regionserver restart
	hbase shell
	create 'hdfs_mysql_hbase_web_table','cf'
	scan 'hdfs_mysql_hbase_web_table'
	=============================================================
Assignment 2 ---
====================================
Create hbase table
Have RDBMS table 
Try to import by controlling it with Query
--query ""	

bigbird

	
	1. create table in HBASE 
	
		hbase shell
		
		create 'hdfs_mysql_query_hbase_web_table','cf'
		
	2. create a table in mysql
		mysql -uroot -pcloudera
	 create table hdfs_mysql_query_web_table(cust_id int, user_name varchar(20),count varchar(20),ip_address varchar(20),begin_time varchar(20),value1 varchar(20),value2 varchar(20),value3 varchar(20),ms varchar(20),http_type varchar(20),category varchar(20),total_value varchar(20), sub_category varchar(20),http_info varchar(20), status varchar(10));
	 
	3. Load the data from HDFS into MYSQL.
			load data infile '/home/cloudera/inputdata/1' into table hdfs_mysql_query_web_table fields terminated by ',';
			load data infile '/home/cloudera/inputdata/2' into table hdfs_mysql_query_web_table fields terminated by ',';
			load data infile '/home/cloudera/inputdata/3' into table hdfs_mysql_query_web_table fields terminated by ',';
			load data infile '/home/cloudera/inputdata/4' into table hdfs_mysql_query_web_table fields terminated by ',';
			load data infile '/home/cloudera/inputdata/5' into table hdfs_mysql_query_web_table fields terminated by ',';
		Step5:
			Verify the data in mysql 
			select count(*) from hdfs_mysql_query_web_table;

	3. HBASE:- scan 'hdfs_mysql_query_hbase_web_table'
	
	4. INGESTION data from mysql into HBASE:



	sqoop import --connect jdbc:mysql://localhost/zeyo_hbase_integration --username root --password cloudera --split-by cust_id --query 'select * from  hdfs_mysql_query_web_table where $CONDITIONS' --hbase-table hdfs_mysql_query_hbase_web_table --column-family cf1 --hbase-row-key cust_id -m 1
	
	sqoop import --connect jdbc:mysql://localhost/zeyo_hbase_integration --username root --password cloudera --split-by cust_id --query 'select * from  hdfs_mysql_query_web_table where user_name="bigbird" AND $CONDITIONS' --hbase-table task2 --column-family cf2 --hbase-row-key cust_id -m 1

	
	5. HBASE:-scan 'hdfs_mysql_query_hbase_web_table'
	6. HBASE:-count 'hdfs_mysql_query_hbase_web_table'
	
	
	
Assignment 3 ---
======================
Create hbase table
Have RDBMS table 
Try with Multiple mappers and show the observations
	
	1. Create Hbase table;
		hbase(main):003:0> create 'usdata','cf'
	2. creare table in mysql
		create table usdata(first_name varchar(20),last_name varchar(20),company_name varchar(20),address varchar(20),city varchar(20),county varchar(20),state varchar(20),zip varchar(20),age int,phone1 int,phone2 int,email varchar(20),web varchar(20));
	
	2. mysql> load data local infile "/home/cloudera/inputdata/usdata.csv" into table usdata fields terminated by "," lines terminated by "\n";
	   
	   
	4. mysql>
		IMPORT MAPPAER WITH 1
		sqoop import --connect jdbc:mysql://localhost/zeyo_hbase_integration --username root --password cloudera --table usdata --hbase-table usdata --column-family cf2 --hbase-row-key state -m 1
		
		IMPORT MAPPAER WITH 3
		sqoop import --connect jdbc:mysql://localhost/zeyo_hbase_integration --username root --password cloudera --table usdata --hbase-table usdata --column-family cf2 --hbase-row-key state -m 3 --split-by state

Assignment -4
=========================
Once data got imported to Hbase from RDBMS
Create hive table on top of that hbase table (Explore)
If i query that table, the data should come from Hbase table
	
	1. Done step1. We imported data from RDBMS to HABSE
	2. create the table in Hive
		
				create external table hdfs_mysql_query_web_table(cust_id int, user_name string,count string,ip_address string,begin_time string,value1 string,value2 string,value3 string,ms string,http_type string,category string,total_value string, sub_category string,http_info string, status string)  STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' WITH  SERDEPROPERTIES ("hbase.columns.mapping"=":key,cf1:user_name,cf1:count,cf1:ip_address,cf1:begin_time,cf1:value1,cf1:value2,cf1:value3,cf1:ms,cf1:http_type,cf1:category,cf1:total_value,cf1:sub_category,cf1:http_info,cf1:status") tblproperties("hbase.table.name"="hdfs_mysql_query_hbase_web_table");
		
	3. Verify the data in Hive Table (hdfs_mysql_query_web_table)
		select * from hdfs_mysql_query_web_table limit 10;
		
Assignment-5: 	Load the data into Spark Data Frame from HIVE table 
==========================================================================
			scala> val dataFromHiveDF = spark.sql("select * from test.hdfs_mysql_query_web_table")
			dataFromHiveDF: org.apache.spark.sql.DataFrame = [custid: int, username: string ... 13 more fields]
		
