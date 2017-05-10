Ques:Explain in breif with an example
● Bucketing
● Bucketing V/S Partitioning
● Sampling

Ans:
# BUCKETING in HIVE: 
- When we write data in bucketed table in hive, it places the data in distinct buckets as files.
- Hive uses some hashing algorithm to generate a number in range of 1 to N buckets [as mentioned in DDL] and based on the result of hashing, data is placed in a particular buckets as a file.
- Let's create a hive bucketed table T_USER_LOG_BUCKET with a partition column as DT and having 4 buckets. We specify bucketing column in CLUSTERED BY (column_name) clause in hive table DDL.

# Bucketing V/S Partitioning:
*Partitioning data is often used for distributing load horizontally, this has performance benefit, and helps in organizing data in a logical fashion.

-- Example:
if we are dealing with a large employee table and often run queries with WHERE clauses that restrict the results to a particular country or department . For a faster query response Hive table can be PARTITIONED BY (country STRING, DEPT STRING). Partitioning tables changes how Hive structures the data storage and Hive will now create subdirectories reflecting the partitioning structure like

.../employees/country=ABC/DEPT=XYZ.

*Bucketing is another technique for decomposing data sets into more manageable parts.

-- Example:
Suppose a table using date as the top-level partition and employee_id as the second-level partition leads to too many small partitions. Instead, if we bucket the employee table and use employee_id as the bucketing column, the value of this column will be hashed by a user-defined number into buckets. Records with the same employee_id will always be stored in the same bucket. Assuming the number of employee_id is much greater than the number of buckets, each bucket will have many employee_id. While creating table you can specify like CLUSTERED BY (employee_id) INTO XX  BUCKETS; where XX is the number of buckets . Bucketing has several advantages. The number of buckets is fixed so it does not fluctuate with data. If two tables are bucketed by employee_id, Hive can create a logically correct sampling. Bucketing also aids in doing efficient map-side joins etc.

# Sampling:
- Sampling is concerned with the selection of a subset of data from a large dataset to run queries and verify results. The dataset may be too large to run queries on the whole data. Therefore in development and testing phases it is a good idea to run queries on a sample of dataset.

*TABLESAMPLE Clause:
- We can run Hive queries on a sample of data using the TABLESAMPLE clause. Any column can be used for sampling the data. We need to provide the required sample size in the queries.

*Sampling by Bucketing:

- We can use TABLESAMPLE clause to bucket the table on the given column and get data from only some of the buckets.

    TABLESAMPLE (BUCKET x OUT OF y [ON colname])
colname indicates the column to be used to bucket the data into y buckets[1-y]. All the rows which are in the bucket x are returned.
