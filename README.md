# pkmysql8cok
### Window functions
```
CUME_DIST
DENSE_RANK
FIRST_VALUE
LAG
LAST_VALUE
LEAD
NTH_VALUE
NTILE
PERCENT_RANK
RANK
ROW_NUMBER
```
### How to do it
```
alter table employees ADD hire_date_year YEAR AS (YEAR(hire_date)) VIRTUAL;
```

### Row Number
```
select concat(first_name, " ",last_name) as full_name, salary, ROW_NUMBER() OVER(ORDER BY salary DESC) AS 'Rank' From employees JOIN
salaries.emp_no=employees.emp_no LIMIT 10;
```

### Partition results
```
select hire_date_year, salary, ROW_NUMBER() OVER(PARTITION BY hire_date_year ORDER BY salary DESC) AS 'Rank' FROM
employees JOIN salaries ON salaries.emp_no=employees.emp_no ORDER BY salary DESC LIMIT 10;
```

### Named windows
```
select hire_date_year, salary, ROW_NUMBER() OVER w AS 'Rank' FROM
employees JOIN salaries ON salaries.emp_no=employees.emp_no WINDOW w (PARTITION BY hire_date_year ORDER BY salary DESC) ORDER BY salary DESC LIMIT 10;
```

### First,last,and nth values
If the row does not exist, return `NULL`
```
select hire_date_yaer, salary, RANK() OVER w AS 'RANK',
FIRST_VALUE(salary) OVER w AS 'first',
NTH_VALUE(salary,3) OVER w AS 'third',
LAST_VALUE(salary) OVER w AS 'last' FROM employees join salaries ON salaries.emp_no=employees.emp_no
WINDOW w AS (PARTITION BY hire_date_year ORDER BY salary DESC) ORDER BY salary DESC LIMIT 10;
```


## Configuring MySQL
### Introduction
MySQL has two types of parameters:
- static: takes effect after restarting MySQL server
- Dynamic, which can be changed on the fly without restarting MySQL server

Use SET command:
- @@persist.

### Using config file
location
- `/etc/my.cnf`  Redhat
- `/etc/mysql/my.cnf`  Debian


### How to do it

```
SET GLOBAL long_query_time=1
```
persist:
```
SET PERSIST long_query_time=1
```

OR
```
SET @@persist.long_query_time=1;
```
only for this session:
```
SET SESSION long_query_time=1;
```


### Using parameters with startup script
```
/usr/local/mysql/bin/mysqld --basedir=/usr/local/mysql --plugin-dir=/usr/local/mysql/lib/plugin --user=mysql
--log-error --pid-file --init-file
```

### Configuring the parameters
#### data directory
data directory has three sub dic
- `mysql`
- `performance_schema`
- `sys`

datadir file, default is ```/var/lib/mysql```
```
[mysqld]
datadir = /data/mysql
```

### innodb_buffer_pool_size
```
innodb_buffer_pool_size
```

### innodb_buffer_pool_instances

### innodb_log_file_size

### Changing the data directory
```
show variables like '%datadir%';
```
```
systemctl stop mysql
mkdir -pv /data
chown -R mysql:mysql /data/
rsync -av /var/lib/mysql /data
```
if error happens
```
mysql_install_db
```


## Transactions
### Performing transactions
```
SELECT balance into @a.bal from account where account_number="A";
UPDATE account SET balance=@a.bal+100 where account_number="A";
```
### Autocommit
disable autocommit:
```
set autocommit=0
```

### Isolation levels
```
SET @@transaction_isolation = 'READ-COMMITTED';
```


### Read uncommitted
The current transaction can read data written by another uncommitted transaction,which is called __dirty read__
