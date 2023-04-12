# OracleDBA_Labs
####Lab 3

1)

- check where is the location of archivelog
- switch online redo log file by command
- select group , archived, status from v$log ; --- ( I made mistake there's different between status and archived I will explain it )
- view the redo log files names, size, group number, status, archived or not and their sizes in MB using the views "v$log" and "v$logfile".
- Increase the size of all redo log files to be 100 MB, then repeat the last step. ( google it please )
- create new three redo log file groups 4,5 and 6  ( google it please ) then again view the redo log files names, size, group number, status, archived or not and their sizes in MB using the views "v$log" and "v$logfile"
- drop redo log group 6 ; 

``` oracle
SHOW PARAMETER log_archive_dest
ALTER SYSTEM SWITCH LOGFILE;
SELECT group#, archived, status FROM v$log;

SELECT
  l.group#,
  l.member,
  l.status,
  l.archived,
  l.bytes/1024/1024 AS size_mb,
  f.status
FROM v$log l, v$logfile fWHERE l.group# = f.group#;


ALTER DATABASE
  ADD LOGFILE GROUP 4 ('/path/to/redo04a.log', '/path/to/redo04b.log') SIZE 100M,
                   ('/path/to/redo04c.log', '/path/to/redo04d.log') SIZE 100M,
  ADD LOGFILE GROUP 5 ('/path/to/redo05a.log', '/path/to/redo05b.log') SIZE 100M,
                   ('/path/to/redo05c.log', '/path/to/redo05d.log') SIZE 100M,
  ADD LOGFILE GROUP 6 ('/path/to/redo06a.log', '/path/to/redo06b.log') SIZE 100M,
                   ('/path/to/redo06c.log', '/path/to/redo06d.log') SIZE 100M;




ALTER DATABASE
  ADD LOGFILE GROUP 4 ('/path/to/redo04a.log', '/path/to/redo04b.log') SIZE 50M,
                   ('/path/to/redo04c.log', '/path/to/redo04d.log') SIZE 50M;
ALTER DATABASE
  ADD LOGFILE GROUP 5 ('/path/to/redo05a.log', '/path/to/redo05b.log') SIZE 50M,
                   ('/path/to/redo05c.log', '/path/to/redo05d.log') SIZE 50M;
ALTER DATABASE
  ADD LOGFILE GROUP 6 ('/path/to/redo06a.log', '/path/to/redo06b.log') SIZE 50M,
                   ('/path/to/redo06c.log', '/path/to/redo06d.log') SIZE 50M;


ALTER DATABASE
  DROP LOGFILE GROUP 6;


```

2)

- create new undo tablespace , with one datafile 50MB
- Show the value of parameter undo_retention then change the value of it to be 3 minutes and gurantee this duration.
- make this new created tablespace the default tablespace
- create new tablespace ITI with one datafile with size 50MB and make it the default tablespace for HR user
- create temporary tablespace TEMP2 with size 50MB
- resize up the datafile of ITI tablespace from 50MB to 100MB
- show data files names, thier tablespaces, their size, their status using view "dba_data_files".

``` oracle
CREATE UNDO TABLESPACE undo_ts
DATAFILE 'undo_ts01.dbf' SIZE 50M;

-- Show the current value of undo_retention
SHOW PARAMETER undo_retention;

-- Change the value of undo_retention to 3 minutes
ALTER SYSTEM SET undo_retention = 180;


ALTER TABLESPACE undo_ts RETENTION GUARANTEE;

CREATE TABLESPACE ITI
DATAFILE 'ITI01.dbf' SIZE 50M;
ALTER USER HR DEFAULT TABLESPACE ITI;


CREATE TEMPORARY TABLESPACE TEMP2
TEMPFILE 'temp2.dbf' SIZE 50M;

ALTER DATABASE DATAFILE 'ITI01.dbf' RESIZE 100M;

SELECT file_name, tablespace_name, bytes/1024/1024 AS size_mb, status
FROM dba_data_files;



```


3)

- create an OS user with name "iti" and make him login to the database using "sqlplus /"
- make your personal Operating system user to connect locally without authentication as sysdba.

``` oracle
sqlplus / as sysdba

CREATE USER iti IDENTIFIED EXTERNALLY;
GRANT CONNECT, RESOURCE TO iti;

SQLNET.AUTHENTICATION_SERVICES=(NONE)  >> $ORACLE_HOME/network/admin

lsnrctl stop
lsnrctl start

sqlplus / as sysdba



```




