## 1.
     ##unlock hr user       
     ##connect as HR user
     ##create table t1 with two columns , column 1 number , column2 varchar2
     ##insert 3 rows in this table
     ##find information related to this table, and in which tablespace it saved.
     
     
 ``` Oracle
$ vi  ~/.bash_profile
$ export ORACLE_HOME=/u01/oracle
$ export ORACLE_SID=ITI
$ export PATH=$ORACLE_HOME/bin:$PATH
$ source ~/.bash_profile
$ sqlplus /  as  sysdba
$ sqlplus iti  as sysdba
$ ps -ef | grep PMON

       
$ alter user hr identified by hr account unlock;
$ connect hr/hr;

 $ CREATE TABLE t1 (column1 NUMBER,column2 VARCHAR2(50));
   $ insert into t1 values (1,'anubis1');
	$ insert into t1 values (2,'anubis2');
	$ insert into t1 values (3,'anubis3');
  
  select OWNER, STATUS,table_name, tablespace_name from all_tables where table_name='T1';
 ```
![1](https://user-images.githubusercontent.com/128603198/231267744-238fb400-6cb2-4b77-9e92-4f574069ff96.png)

![2](https://user-images.githubusercontent.com/128603198/231268312-c4147b9e-62c8-4908-a2b5-f5089f59b970.png)

 ![3](https://user-images.githubusercontent.com/128603198/231271653-67ae4c9a-e64e-4ea0-906b-ac46b6dd45ee.png)



#### 2.
      ##find the indexes for table hr.departments
      ##find all objects related to HR schema
      ##rebuild one of this indexes  "DEPT_ID_PK" 
     
 ``` Oracle

   $ select index_name, index_type from all_indexes where owner = 'HR' and table_name='DEPARTMENTS'
   $ select * from all_objects where owner='HR';
   $ alter index hr.DEPT_ID_PK REBUILD;
   
 ```
 ![4](https://user-images.githubusercontent.com/128603198/231271696-d24e5160-f5b6-4f83-871c-f52f9ddbdb07.png)

  ![5](https://user-images.githubusercontent.com/128603198/231271751-63ba1e8e-09ae-4507-9beb-c76a7275076c.png)
  
 ![6](https://user-images.githubusercontent.com/128603198/231271786-05e9e149-2f20-494e-8a80-fbc2b6a8aaf3.png)


 
 #### 3.
     ##change parameter audit_trail from none to DB.
     ##restart the database to find the changes
     ##create pfile from the spfile and find the change you made on it.
     ##edit the pfile and make the audit_sys_operations=true 
     ##start the database using this pfile.
     ##create spfile from the pfile.
 
 ``` oracle
      $ alter system set audit_trail=DB scope=spfile;
      $ alter system set audit_trail= DB, EXTENDED scope=spfile;
      $ startup force;
      $ SELECT name, value FROM v$parameter WHERE name = 'audit_trail';
      $ create pfile from spfile;
      
      
      $ startup pfile='/u01/oracle/dbs/initITI.ora'
      $ create spfile from pfile;
 ```  
![7](https://user-images.githubusercontent.com/128603198/231274909-19de14eb-dc60-4433-9af7-823c0c6ab6c1.png)
![8](https://user-images.githubusercontent.com/128603198/231274918-d64bd905-2ee6-4378-a5fa-645584f337b0.png)
![8](https://user-images.githubusercontent.com/128603198/231274937-867856ea-24df-461f-a3ad-e136118a8570.png)
![7](https://user-images.githubusercontent.com/128603198/231274945-5a7bb64c-0a5a-4f52-9db9-fb64fe0b082a.png)

![9](https://user-images.githubusercontent.com/128603198/231274951-65be4fc3-929f-4ceb-9c99-ffca824c4db4.png)
![10](https://user-images.githubusercontent.com/128603198/231274969-4ccbf006-dfd3-4872-9152-91089fc763b9.png)

  ![11](https://user-images.githubusercontent.com/128603198/231274983-876c0eb2-22b8-4590-b537-5451801e40e6.png)
![12](https://user-images.githubusercontent.com/128603198/231274999-4ec55738-7102-4fb9-bfac-c8d263e6048c.png)

  

    


 #### 4.
     ##how the version of your database using the view "v$version"
     ##show hostname, instance name, status, open mode and startup_time using the views "v$instance" and "v$database"
     ##show the name and type of parameter "db_create_file_dest" using view "v$parameter"
     ##show your archiving status then enable the archiving mode if not enabled and show your archiving status again.
     ##Identify the location of the alert log file.
     ##shutdown your database with the most safe and fast method and watch your alert log while doing this (use "tail -f" command).
     ##Start up your database step by step and watch your alert log while doing this (use "tail -f" command).
     
     
 ``` oracle
       $ select * from v$version;
       $ Select INSTANCE_NAME, HOST_NAME, STATUS, STARTUP_TIME from v$instance;
	     $ Select OPEN_MODE from v$database;
       $ desc v$parameter;
       $ select type from v$parameter where NAME='db_create_file_dest'; 
       
       $ shutdown
       $ startup mount
       $ select LOG_MODE from V$DATABASE;
       $ ALTER DATABASE ARCHIVELOG;
       $ SHOW PARAMETER background_dump_dest;   >> in /u01/oracle/rdbms/log
	    
      
       $ tail -f /u01/oracle_base/diag/rdbms/iti/ITI/trace/alert_ITI.log
	     $ shutdown immediate
     
       $ startup nomount
```
![13](https://user-images.githubusercontent.com/128603198/231275133-4b7afa4d-11e8-42a1-b29b-1d94744ebdd6.png)
![14](https://user-images.githubusercontent.com/128603198/231275143-43bc7228-c347-4afb-8298-3818fd08e775.png)
![15](https://user-images.githubusercontent.com/128603198/231275151-a5d42276-549d-44ed-95b3-2b7d876db059.png)
![16](https://user-images.githubusercontent.com/128603198/231275165-6e17c80d-008c-4367-8da9-c1972d493f38.png)
![17](https://user-images.githubusercontent.com/128603198/231275178-e44493d4-a6bd-4271-8144-2dad05b37b85.png)

  
 
    

 
