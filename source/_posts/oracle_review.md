---
title: Oracle数据库复习提纲
date: 2020-06-20 00:00:00
categories: 
    - programming
tag:
  - Oracle
  - database
---

[toc]

# Introduction

![image-20200620153013639](oracle_review/image-20200620153013639.png)

## Instance

p10

An Oracle instance: 

*   Is a means to access an Oracle database
*   Always opens one and only one database
*   Consists of memory and process structures

## Database

p12

An Oracle database: 

*   Is a collection of data that is treated as a unit
*   Consists of three file types
    *   Data files
    *   Control files
    *   Redo log files

## Physical Structure

p13

The physical structure of an Oracle database is determined by the operating system files that provide the actual physical storage for database information. 

*   Control files
*   Data files(includes data dictionary)
*   Redo log files

## Memory Structure

P14

Oracle’s memory structure consists of two memory areas known as:

### SGA

System Global Area (SGA): Allocated at instance startup, and is a fundamental component of an Oracle Instance

*    The SGA consists of several memory structures:
    *   Shared pool
    *   Database buffer cache
    *   Redo log buffer
    *   Other structures (e.g. lock and latch management, statistical data) 
*   There are two optional memory structures that can be configured within the SGA:
    *   Large pool
    *   Java pool

*   SGA is dynamic and sized using SGA_MAX_SIZE. 
*   SGA memory allocated and tracked in granules by SGA components
    *   Contiguous virtual memory allocation
    *   Size based on SGA_MAX_SIZE

#### Shared Pool

The shared pool is used to store the most recently executed SQL statements and the most recently used data definitions.

*   It consists of two key performance-related memory structures: 
    *   Library cache
    *   Data dictionary cache
*   Sized by the parameter SHARED_POOL_SIZE.

#### Library Cache

The library cache stores information about the most recently used SQL and PL/SQL statements. The library cache:

*   Enables the sharing of commonly used statements
*   Is managed by a least recently used (LRU) algorithm
*   Consists of two structures:
    *   Shared SQL area
    *   Shared PL/SQL area
*   Has its size determined by the shared pool sizing

#### Data Dictionary Cache

*   The data dictionary cache is a collection of the most recently used definitions in the database.
    *   It includes information about database files, tables, indexes, columns, users, privileges, and other database objects.
    *   During the parse phase, the server process looks at the data dictionary for information to resolve object names and validate access.
    *   Caching the data dictionary information into memory improves response time on queries.
    *   Size is determined by the shared pool sizing.

#### Database Buffer Cache 

The database buffer cache stores copies of data blocks that have been retrieved from the data files. • It enables great performance gains when you obtain and update data.

*   It is managed through a least recently used (LRU) algorithm.
*   DB_BLOCK_SIZE determines the primary block size.

#### Redo Log Buffer Cache

The redo log buffer cache records all changes made to the database data blocks.

*   Its primary purpose is recovery.
*   Changes recorded within are called redo entries.
*   Redo entries contain information to reconstruct or redo changes.
*   Size is defined by LOG_BUFFER.

#### Large Pool

The large pool is an optional area of memory in the SGA configured only in a shared server environment. 

*   It relieves the burden placed on the shared pool. 

*   This configured memory area is used for session memory (UGA), I/O slaves, and backup and restore operations.

*   Unlike the shared pool, the large pool does not use an LRU list.

*   Sized by LARGE_POOL_SIZE. 

    `ALTER SYSTEM SET LARGE_POOL_SIZE = 64M;`

#### Java Pool

The Java pool services the parsing requirements for Java commands.

*   Required if installing and using Java.
*   It is stored much the same way as PL/SQL in database tables.
*   It is sized by the JAVA_POOL_SIZE parameter.

### PGA

p25

Program Global Area (PGA): Allocated when the server process is started

The PGA is memory reserved for each user process that connects to an Oracle database

#### Process Structure

An Oracle process is a program that depending on its type can request information, execute a series of steps, or perform a specific task.

Oracle takes advantage of various types of processes: 

*   User process: Started at the time a database user requests connection to the Oracle server

    *   A user process is a program that requests interaction with the Oracle server. 
        *   It must first establish a connection.
        *   It does not interact directly with the Oracle server.

*   Server process: Connects to the Oracle Instance and is started when a user establishes a session.

    A server process is a program that directly interacts with the Oracle server.

    *   It fulfills calls generated and returns results.
    *   Can be dedicated or shared server.

*   Background process: Available when an Oracle instance is started

    The relationship between the physical and memory structures is maintained and enforced by Oracle’s background processes

    *   Mandatory background processes
        *   DBWn PMON CKPT LGWR SMON RECO P29
    *   Optional background processes
        *   ARCn LMON Snnn QMNn LMDn CJQ0 Pnnn LCKn Dnnn P35

## Logical Structure

p36

The logical structure of the Oracle architecture dictates how the physical space of a database is to be used.

A hierarchy exists in this structure that consists of tablespaces, segments, extents, and blocks.

![image-20200620220533075](oracle_review/image-20200620220533075.png)

## Processing a SQL Statement

*   Connect to an instance using:
    *   The user process
    *   The server process
*   The Oracle server components that are used depend on the type of SQL statement: 
    *   Queries return rows.
    *   DML statements log changes.
    *   Commit ensures transaction recovery. 
*   Some Oracle server components do not participate in SQL statement processing.

# Oracle Server

## Database Administration Tools

P42

# Managing Oracle Instances

p60

## Initialization Parameter Files

![image-20200620223657340](oracle_review/image-20200620223657340.png)

*   Entries are specific to the instance being accessed
*   There are two kinds of parameters:
    *   Explicit: Having an entry in the file
    *   Implicit: No entry within the file, but assuming the Oracle default values
*   Multiple files can be used for a single database to optimize performance in different situations.
*   Changes to entries in the file take effect based on the type of initialization parameter file used;
    *   Static parameter file, PFILE
    *   Persistent parameter file, SPFILE

### PFILE initSID.ora

*   The PFILE is a text file that can be modified with an operating system editor. 

*   Modifications to the file are made manually.

*   Changes to the file take effect on the next startup. • Its default location is $ORACLE_HOME/dbs.

*   example

    ```
    # Initialization Parameter File: initdb01.ora
    db_name = db01
    instance_name = db01
    control_files = ( /u03/oradata/db01/control01db01.ctl,
    /u03/oradata/db01/control02db01.ctl)
    db_block_size = 4096
    db_block_buffers = 500
    shared_pool_size = 31457280 # 30M Shared Pool
    db_files = 1024
    max_dump_file_size = 10240
    background_dump_dest = /u05/oracle9i/admin/db01/bdump
    user_dump_dest = /u05/oracle9i/admin/db01/udump
    core_dump_dest = /u05/oracle9i/admin/db01/cdump
    undo_management = auto
    undo_tablespace = undtbs
    
    ```

### SPFILE spfileSID.ora

p66

*   Binary file with the ability to make changes persistent across shutdown and startup 

*   Maintained by the Oracle server

*   Records parameter value changes made with the ALTER SYSTEM command

*   Can specify whether the change being made is temporary or persistent

*   Values can be deleted or reset to allow an instance to revert to the default value 

    `ALTER SYSTEM SET undo_tablespace = 'UNDO2';`

*   SPFILE can be created from an initSID.ora file using the CREATE SPFILE command, which can be executed before or after instance startup:

    ``` CREATE SPFILE FROM PFILE;```

*   example

    ```
    *.background_dump_dest='$ORACLE_HOME/admin/db01/bdump'
    *.compatible='9.0.0'
    *.control_files='/u03/oradata/db01/ctrl01db01.ctl','/u03/orad
    ata/db01/ctrl02db01.ctl'
    *.core_dump_dest='$ORACLE_HOME/admin/db01/cdump'
    *.db_block_buffers=500
    *.db_block_size=4096
    *.db_files=40
    *.db_name='db01'
    *.instance_name='db01'
    *.remote_login_passwordfile='exclusive'
    *.shared_pool_size=31457280 # 30M Shared Pool
    *.undo_management='AUTO'
    db01.undo_tablespace='UNDOTBS01'
    db02.undo_tablespace='UNDOTBS02'
    ```

### Oracle Managed Files

p69

Oracle Managed Files (OMF) simplify file administration

*   OMF are created and deleted by the Oracle server as directed by SQL commands
*   OMF are established by setting two parameters:
    *   DB_CREATE_FILE_DEST: Set to give the default location for data files 
    *   DB_CREATE_ONLINE_LOG_DEST_N: Set to give the default locations for online redo logs and control files, up to a maximum of 5 locations

## Starting Up a Database

p73

![image-20200621165731327](oracle_review/image-20200621165731327.png)

### Start up the instance and open the database:

```
STARTUP
STARTUP PFILE=$ORACLE_HOME/dbs/initdb01.ora
```

### The ALTER DATABASE Command

*   Change the state of the database from NOMOUNT to MOUNT:

```ALTER DATABASE db01 MOUNT;```

*   Open the database as a read--only database:

```ALTER DATABASE db01 OPEN READ ONLY;```

### Opening a Database in Restricted Mode

*   Use the STARTUP command to restrict access to a database:

``` STARTUP RESTRICT```

*   Use the ALTER SYSTEM command to place an instance in restricted mode:

``` ALTER SYSTEM ENABLE RESTRICTED SESSION;```

Opening a Database in Read-Only Mode

*   A databases can be   opened as a read-only database.
*   A read-only database can be used to:
    *   Execute queries
    *   Execute disk sorts using locally managed tablespaces
    *   Take data files offline and online, not tablespaces
    *   Perform recovery of offline data files and tablespaces

### Shutting Down the Database

p78

![image-20200621194559674](oracle_review/image-20200621194559674.png)

### Managing an Instance by Monitoring Diagnostic Files

p81

### Alert Log File

The alertSID.log file records the commands and Results of major events while the database is operational.

### Background Trace Files

Background trace files support information errors detected by any background process.

### User Trace File

A user trace file is produced by the user process connected to the Oracle server through the server process.

# Create Database

p88

## Creation Prerequisites

p91

To create a new database, you must have the following:

*   A privileged account authenticated in one of the following ways:
    *   By the operating system
    *   Using a password file
*   Sufficient memory to start the instance
*   Sufficient disk space for the planned database

## Planning Database File Locations

## Creating a Database

p93

base can be created using:

*   Oracle Database Configuration Assistant
*   The CREATE DATABASE command An Oracle datab

## Operating System Environment

### PATH

```
ORACLE_BASE
ORACLE_HOME
ORACLE_SID
ORA_NLS33
PATH
sLD_LIBRARY_PATH
```

##  Database Configuration Assistant

p95

## Database Information

p97

Specify: 

*   Global database name and SID
*   The features you want to use for your database, such as:
    *   Oracle Spatial
    *   Oracle OLAP Services
    *   Example Schemas
*   Mode in which you want the database to operate
    *   Dedicated server mode
    *   Shared server mode

## Other Parameters

*   Archive Parameters
    *   Use for database recovery
    *   May also be used for a standby database
*   Data Block Sizing
    *   Sets the default database block size
    *   Helps to determine the SORT_AREA_SIZE
*   File Locations
    *   Specify paths for trace files
    *   Specify paths for parameter files
    *   Database storage – Specify storage parameters

## Creating a Database Manually

p101

*   Decide on a unique instance and database name 
*   Choose a database character set 
*   Set the operating system variables
*   Edit / Create the initialization parameter file
*   Start the instance (nomount) 
*   Execute the CREATE DATABASE command
*   Run scripts to generate the data dictionary and accomplish post creation steps

### Preparing the Parameter File

### Creating SPFILE

### Starting the Instance

### Creating the Database

### Creating a Database Using OMF

### After Database Creation

The database contains:

*   Datafiles, control files, and redo log files
*   User SYS with the password change_on_install
*   User SYSTEM with the password manager
*   Internal tables (but no data dictionary views)

# Data Dictionary Contents and Usage

p110

## Data Dictionary

During database creation, the Oracle server creates additional object structures within the data files. 

*   Data dictionary tables
*   Dynamic performance tables

The data dictionary is a set of read-only tables and views that record, verify, and provide information about its associated database.

*   Describes the database and its objects
*   Includes two types of objects:
    *   Base tables
        *   Store description of database
        *   Created with CREATE DATABASE
    *   Data Dictionary views
        *   Summarize base table information
        *   Created using catalog.sql script

##  Contents

p114

## Usage

## Data Dictionary View Categories

## Dynamic Performance Tables

# Maintaining the Control File

p122

The control file is a binary file that defines the current state of the physical database.

*   Loss of the control file requires recovery
*   Is read at MOUNT stage
*   Is required to operate
*   Is linked to a single database
*   Should be multiplexed
*   Maintains integrity of database
*   Sized initially by CREATE DATABASE

## Contents

P125

A control file contains the following entries:

*   Database name and identifier
*   Time stamp of database creation
*   Tablespace names
*   Names and locations of data files and redo log files
*   Current redo log file sequence number
*   Checkpoint information
*   Begin and end of undo segments
*   Redo log archive information • Backup information

# Maintaining Redo Log Files

p132

## Managing Online Redo Logs with OMF

p144

## Archived Redo Log Files

# Managing Tablespaces and Data files

p150

## Database Storage Hierarchy

p153

![image-20200628012952676](oracle_review/image-20200628012952676.png)

## SYSTEM and Non-SYSTEM Tablespaces

p154

# Storage Structure and Relationships

p179

## Extent Allocation and Deallocation

p185

## Database Block

## Database Block Contents

## Data Dictionary Views

p196

![image-20200628013426264](oracle_review/image-20200628013426264.png)

## Obtaining Storage Information

# Managing Undo Data







---



# Oracle数据库结构

它由至少一个表空间和数据库模式对象组成。这里，模式是对象的集合，而模式对象是直接引用数据库数据的逻辑结构。模式对象包括这样一些结构:表、视图、序列、存储过程、同义词、索引、簇和数据库链等。逻辑存储结构包括表空间、段和范围，用于描述怎样使用数据库的物理空间。

总之,逻辑结构由逻辑存储结构(表空间,段,范围,块)和逻辑数据结构(表、视图、序列、存储过程、同义词、索引、簇和数据库链等)组成,而其中的模式对象(逻辑数据结构)和关系形成了数据库的关系设计。

段(Segment):

是表空间中一个指定类型的逻辑存储结构，它由一个或多个范围组成，段将占用并增长存储空间。

数据库的物理存储结构是由一些多种物理文件组成，主要有数据文件、控制文件、重做日志文件、归档日志文件、参数文件、口令文件、警告文件等。

控制文件:存储实例、数据文件及日志文件等信息的二进制文件。alter system set control_files='路径'。V$CONTROLFILE。

数据文件:存储数据，以.dbf做后缀。一句话:一个表空间对多个数据文件，一个数据文件只对一个表空间。dba_data_files/v$datafile。

# Oracle 的 oracle sql*plus常用命令

一、sys用户和system用户
Oracle安装会自动的生成sys用户和system用户
(1)、sys用户是超级用户，具有最高权限，具有sysdba角色，有create database的权限，该用户默认的密码是change_on_install
(2)、system用户是管理操作员，权限也很大。具有sysoper角色，没有create database的权限，默认的密码是manager
(3)、一般讲，对数据库维护，使用system用户登录就可以拉
注意：也就是说sys和system这两个用户最大的区别是在于有没有create database的权限。
          
二、sql\*plus工具简介
sql\*plus是oracle自带的工具软件，主要用于执行sql语句，pl\sql块。
操作如下：
(1)、在D:\dev\oracle\product\10.2.0\db_1\bin\目录下的sqlplusw.exe。(D:\dev\oracle\为oracle安装目录)
(2)、在运行栏中输入“sqlplusw”即可

三、sqlplus dos工具简介
(1)、概述：sqlplus是在dos下操作oracle的工具，其功能和sql*plus相似。
(2)、操作如下：在运行栏中输入“sqlplus”即可

四、PLSQL Developer工具，需要自己安装，推荐大家使用

五、sql\*plus常用命令
1)、连接命令
1.conn[ect]
用法：conn 用户名/密码@网络服务名 [as sysdba/sysoper]
说明：当用特权用户身份连接时，必须带上as sysdba或是as sysoper
eg、
SQL> show user
USER 为 "SCOTT"
SQL> conn system/oracle@orcl
已连接。
SQL> show user
USER 为 "SYSTEM"
SQL>
以上命令实现类似切换用户的效果
2.disc/disconn/disconnect
说明: 该命令用来断开与当前数据库的连接
3.pssw[ord]
说明: 该命令用于修改用户的密码，如果要想修改其它用户的密码，需要用sys/system登录。
eg、
SQL> conn scott/oracle
已连接。
SQL> passw
更改 SCOTT 的口令
旧口令:
新口令:
重新键入新口令:
口令已更改
SQL>
4.show user
说明: 显示当前用户名
5.exit
说明: 该命令会断开与数据库的连接，同时会退出sql*plus
5.clear screen
清空屏幕

2)、文件操作命令
1.start和@
说明: 运行sql脚本
案例: sql>@ d:\a.sql或是sql>start d:\a.sql
2.edit
说明: 该命令可以编辑指定的sql脚本
案例: sql>edit d:\a.sql,这样会把d:\a.sql 这个文件打开
3.spool
说明: 该命令可以将sql*plus屏幕上的内容输出到指定文件中去。
案例: sql>spool d:\b.sql并输入sql>spool off
eg、
sql>spool d:\b.sql;
sql>select * from emp;
sql>spool off;

3)、交互式命令
1.&
说明：可以替代变量，而该变量在执行时，需要用户输入。
select * from emp where job='&job'；

4)、显示和设置环境变量
概述：可以用来控制输出的各种格式，set show 如果希望永久的保存相关的设
置，可以去修改glogin.sql 脚本
1.linesize
说明：设置显示行的宽度，默认是80个字符
show linesize
set linesize 90
2.pagesize说明：设置每页显示的行数目，默认是14
用法和linesize 一样
至于其它环境参数的使用也是大同小异