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
