# OracleDBA_Labs
## Day6_Lab4
1) USER management:
=======================
- create PERMANENT tablespace with name ITI with two data files with intial size 50 MB each, 250 MB maxsize and increment of 10 MB
- create new user <your_name> and make ITI tablespace is the default tablespace with unlimited quota on it.
- try to connect with the new created user.
- give the new user connect, resource and create view privilges.
- try to connect with the new created user.
- list all users exist in the system.
- show the usernames, their passwords, their tablespace, their roles and their acccount status using the views "dba_users" and "DBA_ROLE_PRIVS"
- create new role  <your_name>_role by sys user , and grant select any table to this role.
- assign this role to the new user
- create new profile <your_name>_prof and make max number of failed login attempts=2.
- assign the profile to the new user
- enter wrong password three times , check if user is locked. then unlock it


``` oracle
CREATE TABLESPACE ITI2 
DATAFILE 'iti_data01.dbf' SIZE 50M AUTOEXTEND ON NEXT 10M MAXSIZE 250M, 
         'iti_data02.dbf' SIZE 50M AUTOEXTEND ON NEXT 10M MAXSIZE 250M;
         
         
CREATE USER <anubis> 
IDENTIFIED BY <anubis> 
DEFAULT TABLESPACE ITI2 
QUOTA UNLIMITED ON ITI2;


GRANT CONNECT, RESOURCE, CREATE VIEW TO <anubis>;

SELECT * FROM dba_users;

SELECT username, password, default_tablespace, account_status 
FROM dba_users 
WHERE username = '<anubis>';

SELECT grantee, granted_role 
FROM dba_role_privs 
WHERE grantee = '<anubis>';


CREATE ROLE <anubis>_role;

GRANT SELECT ANY TABLE TO <anubis>_role;

GRANT <anubis>_role TO <anubis>;

CREATE PROFILE <anubis>_prof LIMIT 
FAILED_LOGIN_ATTEMPTS 2;


ALTER USER <anubis> 
PROFILE <anubis>_prof;


ALTER USER <anubis> ACCOUNT UNLOCK;





```

