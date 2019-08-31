# Base de datos Employees

Alan Badillo Salas (badillo.soft@hotmail.com)

Descar de: https://github.com/datacharmer/test_db

> Importar la base de datos `employees`

``` shell
> mysql -u root -p employees < employees.sql
```

``` sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| almacen            |
| employees          |
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| test               |
+--------------------+
21 rows in set (0.00 sec)

mysql> use employees;
Database changed

mysql> show tables;
+----------------------+
| Tables_in_employees  |
+----------------------+
| current_dept_emp     |
| departments          |
| dept_emp             |
| dept_emp_latest_date |
| dept_manager         |
| employees            |
| salaries             |
| titles               |
+----------------------+
8 rows in set (0.00 sec)

mysql> describe employees;
+------------+---------------+------+-----+---------+-------+
| Field      | Type          | Null | Key | Default | Extra |
+------------+---------------+------+-----+---------+-------+
| emp_no     | int(11)       | NO   | PRI | NULL    |       |
| birth_date | date          | NO   |     | NULL    |       |
| first_name | varchar(14)   | NO   |     | NULL    |       |
| last_name  | varchar(16)   | NO   |     | NULL    |       |
| gender     | enum('M','F') | NO   |     | NULL    |       |
| hire_date  | date          | NO   |     | NULL    |       |
+------------+---------------+------+-----+---------+-------+
6 rows in set (0.01 sec)

mysql> describe departments;
+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| dept_no   | char(4)     | NO   | PRI | NULL    |       |
| dept_name | varchar(40) | NO   | UNI | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> select emp_no, first_name, last_name from employees limit 10;
+--------+------------+-----------+
| emp_no | first_name | last_name |
+--------+------------+-----------+
|  10001 | Georgi     | Facello   |
|  10002 | Bezalel    | Simmel    |
|  10003 | Parto      | Bamford   |
|  10004 | Chirstian  | Koblick   |
|  10005 | Kyoichi    | Maliniak  |
|  10006 | Anneke     | Preusig   |
|  10007 | Tzvetan    | Zielinski |
|  10008 | Saniya     | Kalloufi  |
|  10009 | Sumant     | Peac      |
|  10010 | Duangkaew  | Piveteau  |
+--------+------------+-----------+
10 rows in set (0.00 sec)

mysql> select emp_no, first_name, last_name from employees limit 3, 10;
+--------+------------+-----------+
| emp_no | first_name | last_name |
+--------+------------+-----------+
|  10004 | Chirstian  | Koblick   |
|  10005 | Kyoichi    | Maliniak  |
|  10006 | Anneke     | Preusig   |
|  10007 | Tzvetan    | Zielinski |
|  10008 | Saniya     | Kalloufi  |
|  10009 | Sumant     | Peac      |
|  10010 | Duangkaew  | Piveteau  |
|  10011 | Mary       | Sluis     |
|  10012 | Patricio   | Bridgland |
|  10013 | Eberhardt  | Terkki    |
+--------+------------+-----------+
10 rows in set (0.00 sec)
```

## TAREA 1

Para la tabla `salaries` de `employees` encontrar los siguientes datos para el empleado 10068:

* Máximo salario alcanzado
* Mínimo salario percibido
* Salario promedio

Para la tabla `salaries` de `employees` encontrar los siguientes datos para el empleado 10119:

* Máximo salario alcanzado
* Mínimo salario percibido
* Salario promedio

Para la tabla `salaries` de `employees` encontrar los siguientes datos para el empleado 10548:

* Máximo salario alcanzado
* Mínimo salario percibido
* Salario promedio

## TAREA 2

Para la tabla `employees` de `employees` mostrar los registros del número 70 al 92 que sean hombres (género `M`);























