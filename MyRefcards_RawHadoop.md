# Raw Hadoop

### Purpose
To install a raw hadoop from apache on windows desktop.

## Downlaod

## Install

## Config

## Usage 

### Start

### Stop

## Projects

## Tips & Notes

### Help

sqoop help

### Version

sqoop version

### List Databases

    sqoop list-databases \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --verbose

### List Tables

    sqoop list-tables \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --verbose

### Provide Password from Command Prompt

    sqoop list-tables \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        -P

### Import Sqoop Password File into HDFS

    echo -n "cloudera" > sqoop.password
    hdfs dfs -put sqoop.password /user/cloudera/sqoop.password
    hdfs dfs -chown 400 /user/cloudera/sqoop.password
    rm sqoop.password

### Provide Password from a Password File

    sqoop list-tables \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password-file sqoop.password

### Import a Table

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --warehouse-dir=/user/hive/warehouse
    
### Import a Table in SequenceFile format

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --target-dir=/user/hive/warehouse/departments_ff_sq \
        --as-sequencefile
    
### Import a Table in Avro File format

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --target-dir=/user/hive/warehouse/departments_ff_avro \
        --as-avrodatafile
    
### Import a Table in Parquet File format

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --target-dir=/user/hive/warehouse/departments_ff_parquet \
        --as-parquetfile
    
### Import a Table in GZip compressed format

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --target-dir=/user/hive/warehouse/departments_compressed_GZip \
        --compress
    
### Import a Table in BZip2 compressed format

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --target-dir=/user/hive/warehouse/departments_compressed_BZip2 \
        --compress \
        --compression-codec org.apache.hadoop.io.compress.BZip2Codec
    
### Import only a sub-set of Table data

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --where "department_id >= 5" \
        --target-dir=/user/hive/warehouse/departments_gt_5
    
### Override Column Type Mapping

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --target-dir=/user/hive/warehouse/departments_col_mapping \
        --map-column-java department_id=Long,department_name=String
    
### Increasing the Number Of Mappers For Larger Dataset

    sqoop import \
        -m 10 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --target-dir=/user/hive/warehouse/departments_10_mappers
    
### Handling NULL Values From Database

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table departments  \
        --target-dir=/user/hive/warehouse/departments_NULL_handling \
        --null-string '\\N' \
        --null-non-string '\\N'
    
### Bulk Import All Tables

    sqoop import-all-tables \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --warehouse-dir=/user/hive/warehouse/import_all
    
### Exclude Tables During Bulk Import

    sqoop import-all-tables \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --warehouse-dir=/user/hive/warehouse/import_all_with_excludes \
        --exclude-tables orders,customers
    
### Importing Only New Data Based On ID Column Value

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table orders \
        --where "order_id <= 4000" \
        --target-dir=/user/hive/warehouse/orders_incremental_append

    sqoop import \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table orders \
        --target-dir=/user/hive/warehouse/orders_incremental_append \
        --incremental=append \
        --check-column=order_id \
        --last-value=4000
    
### Importing Only New Data Based On LastModified Timestamp

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table orders \
        --where "order_date < '2013-08-17 00:00:00'" \
        --target-dir=/user/hive/warehouse/orders_incremental_lastmodified
    
    sqoop import \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table orders \
        --target-dir=/user/hive/warehouse/orders_incremental_lastmodified \
        --incremental=lastmodified \
        --check-column=order_date \
        --last-value='2013-08-17 00:00:00' \
        --merge-key=order_id
    
### Create Sqoop Job For Incremental Import

    sqoop import \
        -m 1 \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table orders \
        --where "order_date < '2013-08-17 00:00:00'" \
        --target-dir=/user/hive/warehouse/orders_incremental_lastmodified
    
    sqoop job \
        --create sqoop_job_incremental_import_Orders \
        -- \
        import \
        --connect jdbc:mysql://quickstart:3306/retail_db \
        --username=retail_dba \
        --password=cloudera \
        --table orders \
        --target-dir=/user/hive/warehouse/orders_incremental_lastmodified \
        --incremental=lastmodified \
        --check-column=order_date \
        --last-value='2013-08-17 00:00:00' \
        --merge-key=order_id
    
    sqoop job --list
    
    sqoop job --show sqoop_job_incremental_import_Orders
    
    sqoop job --exec sqoop_job_incremental_import_Orders
    