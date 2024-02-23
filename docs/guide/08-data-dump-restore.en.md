# Data import and export

## pg_dump data export tool 

### pg_dump introduced

pg\_dump is a tool for backing up a single OpenTenBase database. Even if the database is being used concurrently, it creates consistent backups. pg\_dump does not block other users from accessing the database (read or write).

The data backed up can be in script or archive file format. The script content contains plain text data for SQL commands, which can be used to reconstruct the database to the state it was in when it was backed up. To recover one such script, you need to use the psql client tool. Script files can also be used to refactor the database on other machines. With some modifications, you can also refactor the database on other SQL Database products. Another optional archive file format must be used with pg\_restore to rebuild the database. pg\_dump provides a flexible archiving and transfer mechanism. pg\_dump can be used to back up the entire database, and then pg\_restore can be used to check the archive and/or select which parts of the database to be restored. pg\_restore supports parallel recovery and is compressed by default.  

### Parameters for connecting to a database
-d dbname  
--dbname=dbname  
SPECIFY THE NAME OF THE DATABASE TO BE CONNECTED TO, AND IF YOU DO NOT SPECIFY THIS PARAMETER, IT WILL BE OBTAINED FROM THE PGDATABASE ENVIRONMENT VARIABLE BY DEFAULT (IF SET).   

-h host  
--host=host  
Specify the host name or IP address of the machine to connect to the server. IF THIS PARAMETER IS NOT SPECIFIED, IT IS TAKEN FROM THE PGHOST ENVIRONMENT VARIABLE BY DEFAULT (IF SET).   

-p port  
--port=port  
Specify the TCP port to connect to the server to listen on, if you do not specify this parameter, it will be from the PGPORT environment variable (if set), otherwise the default value compiled in the program will be used.   

-U username  
--username=username  
Specify the username to connect to the service, if this parameter is not specified, it will be from the PGUSER environment variable (if set), otherwise the current operating system username will be used as the default.   

-w  
--no-password  
Never give a password prompt. If the server requires password authentication and there is no other way to provide the password (e.g. a .pgpass file), the connection attempt will fail. This option is useful for batch tasks and scripts, where none of the users enter the password.   

-W  
--password  
Force pg\_dump to prompt for a password before connecting to a database.  

### Usage Introduction  
#### Entire database backup
-	Backed up into text format

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 >/data/opentenbase/opentenbase.sql
```

-	Back up in an archived format

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -Fc>/data/opentenbase/opentenbase.sql  
```

#### Only objects in the specified mode are backed up  
Back up objects in opentenbase mode  

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -n opentenbase>/data/opentenbase/opentenbase.sql
```

Back up objects in opentenbase and pgxz mode, multiple sets of -n are used in multiple modes  

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -n opentenbase -n pgxz>/data/opentenbase/opentenbase.sql
```

#### Objects in the specified mode are not backed up  

Objects in opentenbase mode are not backed up

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -N opentenbase>/data/opentenbase/opentenbase.sql
```

Objects in opentenbase and pgxz modes are not backed up, and multiple sets of -N are used in multiple modes  

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -N opentenbase -N pgxz>/data/opentenbase/opentenbase.sql
```

#### Only certain data tables are backed up

Backup the data table opentenbase.t

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -t 'opentenbase.t' >/data/opentenbase/opentenbase.sql
```

Backup the data table opentenbase.t, pgxz.t  

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -t 'opentenbase.t' -t 'pgxz.t1'>/data/opentenbase/opentenbase.sql
```

All data tables starting with T in OpenTenbase backup mode

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -t 'opentenbase.t*' >/data/opentenbase/opentenbase.sql
```

#### Some data tables are not backed up

The opentenbase.t data table is not backed up  

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -T 'opentenbase.t' >/data/opentenbase/opentenbase.sql
```

The data table opentenbase.t and pgxz.t1 are not backed up

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -T 'opentenbase.t' -T 'pgxz.t1'>/data/opentenbase/opentenbase.sql
```

Do not back up all data tables starting with t in OpenTenbase mode

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -T 'opentenbase.t*' >/data/opentenbase/opentenbase.sql
```

#### Only the object definition is backed out

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -s >/data/opentenbase/opentenbase.sql
```

#### Only the data is backed out

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 -a >/data/opentenbase/opentenbase.sql
```

#### The data backed up is in the format of insert values(xxx)

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 --inserts >/data/opentenbase/opentenbase.sql
```

#### The data backed up is in the following format: insert(xxx) values(xxx)  

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 --column-inserts >/data/opentenbase/opentenbase.sql
```

#### The backup contains group information  

```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 --include-nodes >/data/opentenbase/opentenbase.sql
```

#### Data with non-logged tables is not backed up  
```
pg_dump -h 127.0.0.1 -U opentenbase -d postgres -p 15432 --no-unlogged-table-data >/data/opentenbase/opentenbase.sql
```

## pg_dumpall data export tool  
### pg_dumpall introduced  

pg\_dumpall is a tool used to back up all OpenTenBase databases, so you can't specify a database when using pg\_dumpall. Even if the database is being used concurrently, it creates consistent backups. pg\_dumpall does not block other users from accessing the database (read or write). Unlike pg\_dump pg\_dumpall can be used to back up OpenTenBase global objects, such as users and tablespace definitions. The format of the backup data cannot be specified when pg\_dumpall is backed up, that is, parameters such as -Fc cannot be used.

### Parameters for connecting to a database 

-h host  
--host=host  
Specify the host name or IP address of the machine to connect to the server. IF THIS PARAMETER IS NOT SPECIFIED, IT IS TAKEN FROM THE PGHOST ENVIRONMENT VARIABLE BY DEFAULT (IF SET).   

-p port  
--port=port  
Specify the TCP port to connect to the server to listen on, if you do not specify this parameter, it will be from the PGPORT environment variable (if set), otherwise the default value compiled in the program will be used.  

-U username  
--username=username  
Specify the username to connect to the service, if this parameter is not specified, it will be from the PGUSER environment variable (if set), otherwise the current operating system username will be used as the default.  

-w  
--no-password  
Never give a password prompt. If the server requires password authentication and there is no other way to provide the password (e.g. a .pgpass file), the connection attempt will fail. This option is useful for batch tasks and scripts, where none of the users enter the password.   

-W  
--password  
Force pg\_dump to prompt for a password before connecting to a database.   

-l dbname  
--database=dbname  
Specify which database to connect to backing up global objects and which other databases to back up. If not specified, the postgres database will be used, and if postgres does not exist, template1 will be used。  

### Usage Introduction  
#### Only global objects are backed up  

```
pg_dumpall -h 127.0.0.1 -U opentenbase -p 15432 -g >/data/opentenbase/opentenbase.sql
```

#### Only user roles are backed up  

```
pg_dumpall -h 127.0.0.1 -U opentenbase -p 15432 -r >/data/opentenbase/opentenbase.sql
```

#### Only tablespaces are backed up  

```
pg_dumpall -h 127.0.0.1 -U opentenbase -p 15432 -t>/data/opentenbase/opentenbase.sql
```

## pg_restore data import tool  

### pg_restore introduced  

pg\_restore is a tool used to recover PostgreSQL databases from non-text archives created by pg\_dump. These archives also allow pg\_restore to choose what to recover. PG\_restore can be operated in two modes. If a database name is specified, pg\_restore connects to that database and restores the archive directly to the database. Otherwise, a script is created that contains the SQL commands necessary to rebuild the database, which is written to a file or standard output. This script outputs the plain text output equivalent to pg\_dump. As a result, some of the options for controlling the output are similar to those for pg\_dump.  

### Connection parameters  

-d dbname  
--dbname=dbname  
Specify the name of the database you want to connect to.   

-h host  
--host=host  
Specify the host name or IP address of the machine to connect to the server. IF THIS PARAMETER IS NOT SPECIFIED, IT IS TAKEN FROM THE PGHOST ENVIRONMENT VARIABLE BY DEFAULT (IF SET).   

-p port  
--port=port  
Specify the TCP port to connect to the server to listen on, if you do not specify this parameter, it will be from the PGPORT environment variable (if set), otherwise the default value compiled in the program will be used.   

-U username  
--username=username  
Specify the username to connect to the service, if this parameter is not specified, it will be from the PGUSER environment variable (if set), otherwise the current operating system username will be used as the default.   

-w  
--no-password  
Never give a password prompt. If the server requires password authentication and there is no other way to provide the password (e.g. a .pgpass file), the connection attempt will fail. This option is useful for batch tasks and scripts, where none of the users enter the password.   

-W  
--password  
Force pg\_restore prompt for a password before connecting to a database.  

### Usage Introduction  

#### The output is in plain text format equivalent to the pg_dump  

```
pg_restore opentenbase.dump
```

Do not use the -d dbname parameter in the command, pg\_restore output a plain text output format equivalent to pg\_dump.

#### The output text is redirected to a file  

```
pg_restore opentenbase.dump -f  opentenbase.sql 
```
or  

```
pg_restore opentenbase.dump > opentenbase.sql  
```

Redirects the output text to a opentenbase.sql file.  

#### When restoring, the database is created first

```
pg_dump -h 127.0.0.1 -p 15432 -U opentenbase -d opentenbase -Fc > opentenbase.dump
dropdb -h 127.0.0.1 -p 15432 -U opentenbase opentenbase   
pg_restore -h 127.0.0.1 -U opentenbase -p 15432 -C -d postgres opentenbase.dump 
```

When using this option, the database mentioned in -d is only used to issue the initial DROP DATABASE and CREATE DATABASE commands. All data to be restored to that database name appears in the archive  

#### Only DDL is restored  

```
pg_restore -h 127.0.0.1 -U opentenbase -p 15432 -d opentenbase -s opentenbase.dump
```

#### Only the data is recovered  

```
pg_restore -h 127.0.0.1 -U opentenbase -p 15432 -d opentenbase -a opentenbase.dump
```

#### Clear before recreating the database object  
```
pg_restore -h 127.0.0.1 -U opentenbase -p 15432 -d opentenbase -c opentenbase.dump
```

#### Only certain modes are restored  

```
pg_restore -h 127.0.0.1 -U opentenbase -p 15432 -n 'public' -d opentenbase opentenbase.dump
```

#### Only certain tables are restored

Restore t_txt tables in all modes  

```
pg_restore -h 127.0.0.1 -U opentenbase -p 15432 -t 't_txt' -d opentenbase opentenbase.dump
```

Restore t_txt and T tables in all modes

```
pg_restore -h 127.0.0.1 -U opentenbase -p 15432 -t 't_txt' -t 't' opentenbase.dump
```

Restore the t_txt table in public mode  

```
pg_restore -h 127.0.0.1 -U opentenbase -p 15432 -n 'public' -t 't_txt' -d opentenbase opentenbase.dump
```

## PSQL Data Import Tool  
### Introduction to psql 
 
psql is a terminal-based OpenTenBase client tool, similar to the command-line tool sqlplus in Oracle, but much more powerful than sqlplus. It allows you to interactively type queries, send them to OpenTenBase, and view the results. Alternatively, the input can come from a file or a command-line argument. In addition, psql provides some meta commands and a variety of shell-like features to facilitate scripting and automating a variety of tasks. psql also supports command completion and historical command retrieval.  

### Parameters for connecting to a database  

-d dbname  
--dbname=dbname  
SPECIFY THE NAME OF THE DATABASE TO BE CONNECTED TO, AND IF YOU DO NOT SPECIFY THIS PARAMETER, IT WILL BE OBTAINED FROM THE PGDATABASE ENVIRONMENT VARIABLE BY DEFAULT (IF SET).   

-h host  
--host=host  
Specify the host name or IP address of the machine to connect to the server. IF THIS PARAMETER IS NOT SPECIFIED, IT IS TAKEN FROM THE PGHOST ENVIRONMENT VARIABLE BY DEFAULT (IF SET).   

-p port  
--port=port  
Specify the TCP port to connect to the server to listen on, if you do not specify this parameter, it will be from the PGPORT environment variable (if set), otherwise the default value compiled in the program will be used.   

-U username  
--username=username  
Specify the username to connect to the service, if this parameter is not specified, it will be from the PGUSER environment variable (if set), otherwise the current operating system username will be used as the default.   

-w  
--no-password  
Never give a password prompt. If the server requires password authentication and there is no other way to provide the password (e.g. a .pgpass file), the connection attempt will fail. This option is useful for batch tasks and scripts, where none of the users enter the password.   

-W  
--password  
Force pg\_dump to prompt for a password before connecting to a database.  

### Usage Introduction  
#### psql executes all commands in one SQL file  

-	Execute externally

```
[pgxz@VM_0_3_centos ~]$ cat /data/opentenbase/opentenbase.sql 
set search_path = public;
insert into opentenbase values(1,2);
select count(1) from opentenbase;
```

```
[pgxz@VM_0_3_centos ~]$ psql -h 172.16.0.29 -p 15432 -U opentenbase -d postgres -f /data/opentenbase/opentenbase.sql 
SET
INSERT 0 1
 count 
-------
 10001
(1 row)
```

- Performed internally

```
[pgxz@VM_0_3_centos ~]$ psql -h 172.16.0.29 -p 15432 -U opentenbase -d postgres 
psql (PostgreSQL 10 (opentenbase 2.01))
Type "help" for help.

postgres=# \i  /data/opentenbase/opentenbase.sql 
SET
INSERT 0 1
 count 
-------
 10002
(1 row)

```

## epilogue
OpenTenBase is an enterprise-grade distributed HTAP database management system. Through a single database cluster, it provides customers with high-consistency distributed database services and high-performance data warehouse services at the same time, forming a set of integrated and complete enterprise-level solutions. When you encounter related problems in the field of databases, please feel free to leave us a message.


