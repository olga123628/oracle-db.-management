Microsoft Windows [Version 10.0.22631.4169]
(c) Microsoft Corporation. All rights reserved.

C:\Users\user>sqlplus sys as sysdba

SQL*Plus: Release 21.0.0.0.0 - Production on Thu Oct 3 15:18:51 2024
Version 21.3.0.0.0

Copyright (c) 1982, 2021, Oracle.  All rights reserved.

Enter password:

Connected to:
Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

SQL> show user;
USER is "SYS"
SQL> select instance_name from v$instance;

INSTANCE_NAME
----------------
orcl

SQL> show pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 ORCLPDB                        MOUNTED
SQL>  SELECT CON_ID,TABLESPACE_NAME,FILE_NAME
  2  FROM CDB_DATA_FILES
  3  WHERE CON_ID = 3;

no rows selected

SQL> SELECT TABLESPACE_NAME FROM DBA_TABLESPACES;

TABLESPACE_NAME
------------------------------
SYSTEM
SYSAUX
UNDOTBS1
TEMP
USERS

SQL> SELECT TABLESPACE_NAME, FILE_NAME
  2  FROM DBA_DATA_FILES
  3  WHERE TABLESPACE_NAME = 'USERS';

TABLESPACE_NAME
------------------------------
FILE_NAME
--------------------------------------------------------------------------------
USERS
C:\USERS\USER\DOWNLOADS\ORADATA\ORCL\USERS01.DBF


SQL> SELECT TABLESPACE_NAME, FILE_NAME
  2  FROM DBA_DATA_FILES;

TABLESPACE_NAME
------------------------------
FILE_NAME
--------------------------------------------------------------------------------
USERS
C:\USERS\USER\DOWNLOADS\ORADATA\ORCL\USERS01.DBF

UNDOTBS1
C:\USERS\USER\DOWNLOADS\ORADATA\ORCL\UNDOTBS01.DBF

SYSTEM
C:\USERS\USER\DOWNLOADS\ORADATA\ORCL\SYSTEM01.DBF


TABLESPACE_NAME
------------------------------
FILE_NAME
--------------------------------------------------------------------------------
SYSAUX
C:\USERS\USER\DOWNLOADS\ORADATA\ORCL\SYSAUX01.DBF


SQL> CREATE PLUGGABLE DATABASE XEPDB1
  2
SQL> CREATE PLUGGABLE DATABASE PLSQL_CLASS2024DB
  2  admin user pdbadmin identified by admin
  3  FILE_NAME_CONVERT=('C:\Users\user\Downloads\oradata\ORCL\pdbseed','C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024');

Pluggable database created.

SQL> alter session set container = PLSQL_CLASS2024DB
  2  ;

Session altered.

SQL> alter pluggable database PLSQL_CLASS2024DB open;

Pluggable database altered.

SQL>
SQL>  create user ol_plsqlauca identified by olga12;

User created.

SQL> grant all privileges to ol_plsqlauca;

Grant succeeded.

SQL> show pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         4 PLSQL_CLASS2024DB              READ WRITE NO
SQL> alter session set container = cdb$root;

Session altered.

SQL> CREATE PLUGGABLE DATABASE ol_to_delete_pdb
  2  from PLSQL_CLASS2024DB
  3  file_name_convert =('C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024DB','C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\ol_to_delete');
CREATE PLUGGABLE DATABASE ol_to_delete_pdb
*
ERROR at line 1:
ORA-65005: missing or invalid file name pattern for file -
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024\SYSTEM01.DBF


SQL> CREATE PLUGGABLE DATABASE ol_to_delete_pdb
  2  from PLSQL_CLASS2024DB
  3  file_name-convert=('C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024DB','C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\ol_to _delete_pdb');
file_name-convert=('C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024DB','C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\ol_to _delete_pdb')
*
ERROR at line 3:
ORA-00922: missing or invalid option


SQL> CREATE PLUGGABLE DATABASE ol_to_delete_pdb
  2  from PLSQL_CLASS2024DB
  3  file_name_convert=('C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024DB','C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\ol_to_delete
_pdb');
CREATE PLUGGABLE DATABASE ol_to_delete_pdb
*
ERROR at line 1:
ORA-65005: missing or invalid file name pattern for file -
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024\SYSTEM01.DBF


SQL> CREATE PLUGGABLE DATABASE ol_to_delete_pdb
  2  file_name-convert=(C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024','C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\ol_to_delete_pd
b');
file_name-convert=(C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024','C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\ol_to_delete_pdb')
*
ERROR at line 2:
ORA-00922: missing or invalid option


SQL> CREATE PLUGGABLE DATABASE ol_to_delete_pdb
  2  from PLSQL_CLASS2024DB
  3  file_name_convert =('C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\PLSQL_CLASS2024','C:\APP\USER\PRODUCT\21C\ORADATA\XE\XE\ol_to_delete'
);

Pluggable database created.

SQL>
SQL> alter pluggable database PLSQL_CLASS2024DB save state;

Pluggable database altered.

SQL> show pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 ORCLPDB                        MOUNTED
         4 PLSQL_CLASS2024DB              READ WRITE NO
         6 OL_TO_DELETE_PDB               MOUNTED
SQL>  alter session set container = ol_to_delete_pdb;

Session altered.

SQL> alter pluggable database ol_to_delete_pdb open;

Pluggable database altered.

SQL>
SQL> alter pluggable database ol_to_delete_pdb save state;

Pluggable database altered.

SQL> show pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         6 OL_TO_DELETE_PDB               READ WRITE NO
SQL> alter pluggable database OL_TO_DELETE_PDB close immediate;

Pluggable database altered.

SQL> alter session set container = cdb$root;

Session altered.

SQL> alter pluggable database OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml';
alter pluggable database OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml'
*
ERROR at line 1:
ORA-65026: XML metadata file error : LPX-00202: could not open
"C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml" (error 0)


SQL> alter pluggable database OL_TO_DELETE_PDB unplug into C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml';
alter pluggable database OL_TO_DELETE_PDB unplug into C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml'
                                                      *
ERROR at line 1:
ORA-00922: missing or invalid option


SQL> alter pluggable database OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml';
alter pluggable database OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml'
*
ERROR at line 1:
ORA-65026: XML metadata file error : LPX-00202: could not open
"C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml" (error 0)


SQL> alter pluggable database OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.XML';
alter pluggable database OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.XML'
*
ERROR at line 1:
ORA-65026: XML metadata file error : LPX-00202: could not open
"C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.XML" (error 0)


SQL>  drop pluggable database ol_to_delete_pdb;
 drop pluggable database ol_to_delete_pdb
*
ERROR at line 1:
ORA-65179: cannot keep datafiles for a pluggable database that is not unplugged


SQL> alter pluggable database OL_TO_DELETE_PDB unplug into C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml'; alter pluggable database OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.XML');
alter pluggable database OL_TO_DELETE_PDB unplug into C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.xml'; alter pluggable database OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.XML')
                                                      *
ERROR at line 1:
ORA-00922: missing or invalid option


SQL> alter session set container = cdb$root;

Session altered.

SQL> alter pluggable databse OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.XML',
  2  ;
alter pluggable databse OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.XML',
                *
ERROR at line 1:
ORA-02000: missing DATABASE keyword


SQL> alter pluggable databse OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.XML';
alter pluggable databse OL_TO_DELETE_PDB unplug into 'C:\APP\USER\PRODUCT\21C\ADMIN\XE\dpdumpin\ol_to_delete_pdb.XML'
                *
ERROR at line 1:
ORA-02000: missing DATABASE keyword

SQL>



C:\Users\user>sqlplus

SQL*Plus: Release 21.0.0.0.0 - Production on Thu Oct 3 18:11:17 2024
Version 21.3.0.0.0

Copyright (c) 1982, 2021, Oracle.  All rights reserved.

Enter user-name: system
Enter password:
Last Successful login time: Thu Oct 03 2024 13:42:27 +02:00

Connected to:
Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

SQL> show con_name;

CON_NAME
------------------------------
CDB$ROOT
SQL>  alter session set container = CDB$ROOT;

Session altered.

SQL> SELECT SYS_CONTEXT('USERENV','CON_NAME') AS current_container FROM dual;

CURRENT_CONTAINER
--------------------------------------------------------------------------------
CDB$ROOT

SQL> SELECT DBMS_XDB_CONFIG.GETHTTPPORT() AS HTTP_PORT,
  2  DBMS_XDB_CONFIG.GETHTTPSPORT() AS HTTPS_PORT
  3  FROM dual;

 HTTP_PORT HTTPS_PORT
---------- ----------
         0       5500

SQL> BEGIN
  2  DBMS_XDB_CONFIG.SETHTTPPORT(8080);
  3  DBMS_XDB_CONFIG.SETHTTPSPORT(8443);-- optional for HTTPS
  4  END;
  5  /

PL/SQL procedure successfully completed.

SQL> SELECT DBMS_XDB_CONFIG.GETHTTPPORT() AS HTTP_PORT,
  2   DBMS_XDB_CONFIG.GETHTTPSPORT() AS HTTPS_PORT
  3  FROM dual;

 HTTP_PORT HTTPS_PORT
---------- ----------
      8080       8443

SQL>  select dbms_xdb_config.gethttpsport() from dual;

DBMS_XDB_CONFIG.GETHTTPSPORT()
------------------------------
                          8443

SQL> SELECT *
  2  FROM dba_hosts
  3  WHERE host_name = '<HOST_NAME>';
FROM dba_hosts
     *
ERROR at line 2:
ORA-00942: table or view does not exist


SQL> show user;
USER is "SYSTEM"
SQL> show con_name;

CON_NAME
------------------------------
CDB$ROOT
SQL>
SQL> SQL> SELECT DBMS_XDB_CONFIG.GETHTTPPORT() AS HTTP_PORT,
SP2-0734: unknown command beginning "SQL> SELEC..." - rest of line ignored.
SQL>   2  DBMS_XDB_CONFIG.GETHTTPSPORT() AS HTTPS_PORT
SQL>   3  FROM dual;
SQL>
SQL> SELECT DBMS_XDB_CONFIG.GETHTTPPORT() AS HTTP_PORT,
  2  DBMS_XDB_CONFIG.GETHTTPSPORT() AS HTTPS_PORT FROM dual;

 HTTP_PORT HTTPS_PORT
---------- ----------
      8080       8443

SQL>
