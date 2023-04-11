# OracleDBA_Labs

## Rebuild of Index

1. reorganizing the index to remove fragmentation and/or repairing any corruption.
2. Process of Rebuilding : creating a new index structure, copying the data from the existing index to the new structure, and then dropping the old index
3. improve the performance of queries that use the index, but it should be done with caution and after careful analysis of the specific database performance issues. 


## Purpose of Rebuilding 
 >> Rebuilds a spatial index or a specified partition of a partitioned index.

``` oracle
ALTER INDEX my_index REBUILD;
ALTER INDEX [schema.]index REBUILD PARTITION partition  [PARAMETERS ('rebuild_params [physical_storage_params]' ) ];

```

## How To Convert Tabele to Partation ?

  >> partitioning is a technique used to divide large tables into smaller, more manageable pieces called partitions

``` oracle
-- Create a new partitioned table
CREATE TABLE my_table_part (
   my_column1    NUMBER,
   my_column2    VARCHAR2(50),
   my_date       DATE
)
PARTITION BY RANGE (my_date)
(
   PARTITION p1 VALUES LESS THAN (TO_DATE('01-JAN-2022','DD-MON-YYYY')) TABLESPACE ts1,
   PARTITION p2 VALUES LESS THAN (TO_DATE('01-JAN-2023','DD-MON-YYYY')) TABLESPACE ts2
);

-- Copy the data from the original table to the new partitioned table
INSERT INTO my_table_part
SELECT * FROM my_table;

-- Verify that the new partitioned table contains all of the data from the original table

-- Drop the original table
DROP TABLE my_table;
```

## Types of Temporary Tables ?
 >> 1. Global Temporary Tables (GTTs): 
     a- These tables are visible to all sessions and are stored in the temporary tablespace
     b- Data inserted into a GTT is visible only within the session that inserts it, and it is automatically deleted when the session ends
     c- useful when you need to store temporary data that is shared across multiple sessions
     
``` oracle
CREATE GLOBAL TEMPORARY TABLE gtt_emp (
   emp_id NUMBER,
   emp_name VARCHAR2(50),
   emp_salary NUMBER
)
ON COMMIT PRESERVE ROWS;
```

 >> 2.  Private Temporary Tables (PTTs):
        a- Data inserted into a PTT is visible only within the session that inserts it, and it is automatically deleted when the session ends.
        b- PTTs are useful when you need to store temporary data that is specific to a single session.
        
  ``` oracle
  CREATE PRIVATE TEMPORARY TABLE ptt_emp (
   emp_id NUMBER,
   emp_name VARCHAR2(50),
   emp_salary NUMBER
)
ON COMMIT PRESERVE ROWS;

  
  ```
  
