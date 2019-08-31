# Notas de SQL

Alan Badillo Salas (badillo.soft@hotmail.com)

SQL es un lenguaje de consulta estructurado, el cuál nos permite realizar peticiones a la base de datos mediante sentencias.

Una sentencia es una declación que contiene verbos o acciones, clausuras y parámetros.

Los verbos más importantes para construir sentencias en SQL son:

* USE - define la base de datos en la que se va a estar trabajando.

``` sql
mysql> use employees;
Database changed
```

* SHOW - muestra las base de datos o tablas en la base de datos de trabajo (la que se cargó con `use`).

``` sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| employees          |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.00 sec)
```

``` sql
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
```

* CREATE - permite crear base de datos y tablas principalmente.

``` sql
mysql> create database if not exists test;
Query OK, 1 row affected, 1 warning (0.01 sec)
```

Observa que en `create database` la restricción (o clausura) `if not exists` la cuál limita la creación de la base de datos sólo en caso de que no exista.

``` sql
mysql> create table nombres ( id_test int auto_increment primary key, nombre varchar(100) not null );
Query OK, 0 rows affected (0.07 sec)
```

Observa que `create table` se define el parámetro `nombres` que indica el nombre de la tabla a crear, seguido de una estructura de parámetros listados (es decir, parámetros separados por coma y puestos entre paréntesis), los parámetros de construcción de la tabla son la definición de cada campo, es decir, nombre del campo, tipo de dato y restricciones del campo, por ejemplo, `id_test int auto_increment primary key` significa que se desea crear un campo llamado `id_test` de tipo `int` con las restricciones que sea auto incrementable y llave primaria (no nulo y sin repeticiones o unico).

* DESCRIBE - nos permite describir una tabla para ver la estructura de sus campos, es decir, los nombres de los campos y sus restricciones.

``` sql
mysql> describe salaries;
+-----------+---------+------+-----+---------+-------+
| Field     | Type    | Null | Key | Default | Extra |
+-----------+---------+------+-----+---------+-------+
| emp_no    | int(11) | NO   | PRI | NULL    |       |
| salary    | int(11) | NO   |     | NULL    |       |
| from_date | date    | NO   | PRI | NULL    |       |
| to_date   | date    | NO   |     | NULL    |       |
+-----------+---------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

* SELECT - selecciona registros o computa operaciones para generar una tabla virtual con los resultados, siempre se devuelven registros.

``` sql
> select 1 + 1;
+-------+
| 1 + 1 |
+-------+
|     2 |
+-------+
1 row in set (0.00 sec)
```

``` sql
mysql> select database();
+------------+
| database() |
+------------+
| employees  |
+------------+
1 row in set (0.00 sec)
```

``` sql
mysql> select * from salaries limit 10;
+--------+--------+------------+------------+
| emp_no | salary | from_date  | to_date    |
+--------+--------+------------+------------+
|  10001 |  60117 | 1986-06-26 | 1987-06-26 |
|  10001 |  62102 | 1987-06-26 | 1988-06-25 |
|  10001 |  66074 | 1988-06-25 | 1989-06-25 |
|  10001 |  66596 | 1989-06-25 | 1990-06-25 |
|  10001 |  66961 | 1990-06-25 | 1991-06-25 |
|  10001 |  71046 | 1991-06-25 | 1992-06-24 |
|  10001 |  74333 | 1992-06-24 | 1993-06-24 |
|  10001 |  75286 | 1993-06-24 | 1994-06-24 |
|  10001 |  75994 | 1994-06-24 | 1995-06-24 |
|  10001 |  76884 | 1995-06-24 | 1996-06-23 |
+--------+--------+------------+------------+
10 rows in set (0.03 sec)
```

Observa que podemos asociar a `select` una sintaxis más amplia agregando el parámetro `*` que significa los campos a devolver si se asocia el adverbio `from` con el parámetro `salaries` que es el nombre de la tabla y se utiliza un clausura llamada `limit` con el parámetro `10` que le indica que sólo se desean consultar los primeros 10 registros.

* INSERT - permite insertar registros en la tabla especificada.

``` sql
insert into nombres (nombre) values ('Test 1');
```

* UPDATE - permite actualizar todos los registros. **Advertencia**: Este comando `update` debe contener una clausura `where` para evitar que todos los registros sean actualizados y sólo se permita actualizar los registros que cumplan la clausura `where`.

``` sql
mysql>  update nombres set nombre='Test 6' where id_test=6;
Query OK, 1 row affected (0.02 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

> **Si se olvida poner la clausura `where`, entonces todos los registros serán alterados, sin forma de deshacerlo.**

``` sql
mysql>  update nombres set nombre='Test X';
Query OK, 4 rows affected (0.07 sec)
Rows matched: 4  Changed: 4  Warnings: 0
```

* DELETE - permite eliminar registros en la tabla especificada. **Advertencia**: Este comando `delete` debe contener una clausura `where` para evitar que todos los registros sean borrados y sólo se permita borrar los registros que cumplan la clausura `where`.

``` sql
mysql> delete from nombres where id_test=5;
Query OK, 1 row affected (0.04 sec)
```

> **Si se olvida poner la clausura `where`, entonces todos los registros serán borrados, sin forma de deshacerlo.**

``` sql
mysql> delete from nombres;
Query OK, 3 rows affected (0.05 sec)
```

* DROP - permite borrar bases de datos o tablas, generalmente se usa con la clausura `if exists` para hacerlo seguro, es decir, que no provoque error si la base de datos o la tabla no existieran.

``` sql
mysql> drop table if exists nombres;
Query OK, 0 rows affected (0.14 sec)
```

``` sql
mysql> drop database if exists test;
Query OK, 0 rows affected (0.10 sec)
```

## Exportar e importar bases de datos

> https://www.linode.com/docs/databases/mysql/use-mysqldump-to-back-up-mysql-or-mariadb/

> Exportar la base de datos sakila (estar posicionado en la terminal en la carpeta dónde estará el archivo de backup `.sql`)

``` shell
> mysqldump -u root -p sakila > sakila-20190831.sql
```

> Importar la base de datos sakila (estar posicionado en la terminal en la carpeta dónde está el archivo de backup `.sql`)

``` shell
> mysql -u root -p sakila < sakila-20190831.sql
```

## Búsquedas

Una búsqueda en SQL significa basicamente seleccionar registros de tablas, para esto se deben dominar las clausuras `limit`, `where`, y `order by`.

### Buscar los registros mediante una página mediante la clausura `LIMIT`

Para acceder a los registros específicos de una página virtual (es decir, los `S` registros ya que pasaron `P` registros omitidos), debemos utilizar `LIMIT P, S` que omite los primeros `P` registros y selecciona los próximos `S` registros.

> Ejemplo: Seleccionar los registros del 101 al 120, es decir, ya pasaron `P=100` registros y queremos los siguientes `S=20` registros.

``` sql
mysql> select * from employees limit 100, 20;
+--------+------------+------------+------------+--------+------------+
| emp_no | birth_date | first_name | last_name  | gender | hire_date  |
+--------+------------+------------+------------+--------+------------+
|  10101 | 1952-04-15 | Perla      | Heyers     | F      | 1992-12-28 |
|  10102 | 1959-11-04 | Paraskevi  | Luby       | F      | 1994-01-26 |
|  10103 | 1953-11-26 | Akemi      | Birch      | M      | 1986-12-02 |
|  10104 | 1961-11-19 | Xinyu      | Warwick    | M      | 1987-04-16 |
|  10105 | 1962-02-05 | Hironoby   | Piveteau   | M      | 1999-03-23 |
|  10106 | 1952-08-29 | Eben       | Aingworth  | M      | 1990-12-19 |
|  10107 | 1956-06-13 | Dung       | Baca       | F      | 1994-03-22 |
|  10108 | 1952-04-07 | Lunjin     | Giveon     | M      | 1986-10-02 |
|  10109 | 1958-11-25 | Mariusz    | Prampolini | F      | 1993-06-16 |
|  10110 | 1957-03-07 | Xuejia     | Ullian     | F      | 1986-08-22 |
|  10111 | 1963-08-29 | Hugo       | Rosis      | F      | 1988-06-19 |
|  10112 | 1963-08-13 | Yuichiro   | Swick      | F      | 1985-10-08 |
|  10113 | 1963-11-13 | Jaewon     | Syrzycki   | M      | 1989-12-24 |
|  10114 | 1957-02-16 | Munir      | Demeyer    | F      | 1992-07-17 |
|  10115 | 1964-12-25 | Chikara    | Rissland   | M      | 1986-01-23 |
|  10116 | 1955-08-26 | Dayanand   | Czap       | F      | 1985-05-28 |
|  10117 | 1961-10-24 | Kiyotoshi  | Blokdijk   | F      | 1990-05-28 |
|  10118 | 1957-03-29 | Zhonghui   | Zyda       | F      | 1990-09-13 |
|  10119 | 1960-12-01 | Domenick   | Peltason   | M      | 1986-03-14 |
|  10120 | 1960-03-26 | Armond     | Fairtlough | F      | 1996-07-06 |
+--------+------------+------------+------------+--------+------------+
20 rows in set (0.01 sec)
```

### Ordenar la búsqueda por campos mediante la clausura `ORDER BY`

La clausura `ORDER BY X M` ordena los registros devueltos en la consulta ordenados según el campo `X` en el modo `M`, el modo de ordenamiento puede ser `ASC` (ascendendente o de menor a mayor) o `DESC` (descendente o de mayor a menor), en el caso que sean textos serán ordenados alfanumericamente y en caso de ser números o fechas, según su valor. Los valores `NULL` (nulos o sin valor) serán tomados como los menores de todos. **Nota:** La clausura `ORDER BY` debe ir antes de la clausura `LIMIT`.

> Ejemplo: Seleccionar los registros ordenados por el campo `birth_date` por defecto `ASC` de la página 5 (5 * 20 -> 20).

``` sql
mysql> select * from employees order by birth_date limit 100, 20;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
| 228314 | 1952-02-03 | Dinkar     | Pezzoli   | M      | 1994-12-10 |
| 211427 | 1952-02-03 | Yechiam    | Bala      | M      | 1985-02-27 |
| 294298 | 1952-02-03 | Vesna      | Coney     | M      | 1999-06-28 |
| 205074 | 1952-02-03 | Alselm     | Berendt   | M      | 1991-05-15 |
|  79440 | 1952-02-03 | Boalin     | Grandbois | M      | 1995-02-12 |
| 449170 | 1952-02-03 | Serenella  | Mellouli  | M      | 1985-07-19 |
|  43737 | 1952-02-03 | Debatosh   | Beerel    | F      | 1987-12-07 |
| 446088 | 1952-02-03 | Nevin      | Verspoor  | M      | 1988-05-11 |
| 249494 | 1952-02-03 | Boaz       | Ranum     | F      | 1992-02-14 |
|  16447 | 1952-02-03 | Zhiguo     | Savasere  | F      | 1987-04-28 |
| 262764 | 1952-02-03 | Mads       | Poupard   | M      | 1989-04-20 |
| 409650 | 1952-02-03 | Erzsebet   | Murtha    | F      | 1989-04-16 |
| 265242 | 1952-02-03 | George     | Sessa     | M      | 1989-04-02 |
| 406833 | 1952-02-03 | Aiichiro   | Kobuchi   | M      | 1990-11-23 |
|  84966 | 1952-02-03 | Jaroslava  | Aingworth | M      | 1996-03-25 |
|  16093 | 1952-02-03 | Luise      | Tramer    | M      | 1992-02-28 |
| 249312 | 1952-02-03 | Katsuyuki  | Pardalos  | F      | 1991-10-10 |
| 220060 | 1952-02-03 | Neelam     | Slaats    | F      | 1986-07-23 |
|  32094 | 1952-02-03 | Mohd       | Buchter   | F      | 1986-03-16 |
|  41374 | 1952-02-03 | JiYoung    | Schurmann | M      | 1988-01-25 |
+--------+------------+------------+-----------+--------+------------+
20 rows in set (0.28 sec)
```

> Ejemplo: Selecciona los registros ordenados por el campo `birth_date` descendente y la página 5.

``` sql
mysql> select * from employees order by birth_date desc limit 100, 20;
+--------+------------+------------+-------------+--------+------------+
| emp_no | birth_date | first_name | last_name   | gender | hire_date  |
+--------+------------+------------+-------------+--------+------------+
| 255779 | 1965-01-31 | Heejo      | Merle       | M      | 1991-10-11 |
|  34446 | 1965-01-31 | Chinhyun   | Bauknecht   | M      | 1985-10-15 |
| 457337 | 1965-01-31 | Xumin      | Beidas      | F      | 1995-04-27 |
| 230126 | 1965-01-31 | Bezalel    | Radivojevic | M      | 1988-07-02 |
| 106695 | 1965-01-31 | Lijie      | Garrabrants | F      | 1985-10-02 |
| 280420 | 1965-01-31 | Jahangir   | Hofting     | M      | 1986-09-28 |
|  62046 | 1965-01-31 | Karlis     | Nanard      | M      | 1992-09-19 |
| 296664 | 1965-01-31 | Dipayan    | Geffroy     | M      | 1987-09-17 |
|  71710 | 1965-01-31 | Hitomi     | Takanami    | F      | 1991-02-11 |
| 431657 | 1965-01-31 | Visit      | Thiran      | F      | 1986-11-14 |
| 476327 | 1965-01-31 | Reinhold   | Binkley     | M      | 1994-09-25 |
| 265558 | 1965-01-31 | Susuma     | Kleiser     | M      | 1987-08-02 |
|  17988 | 1965-01-31 | Mabhin     | Calkin      | M      | 1989-11-30 |
| 401333 | 1965-01-30 | Martins    | Shokrollahi | M      | 1986-11-04 |
|  15058 | 1965-01-30 | Bouchung   | Bugrara     | M      | 1986-05-31 |
| 291612 | 1965-01-30 | Siamak     | Gomatam     | M      | 1987-11-01 |
| 296718 | 1965-01-30 | Kamakshi   | Bahl        | F      | 1987-12-22 |
| 246146 | 1965-01-30 | Zhongwei   | Melski      | F      | 1986-04-12 |
|  29380 | 1965-01-30 | Satoru     | Vidal       | F      | 1998-03-13 |
| 219047 | 1965-01-30 | Aemilian   | Kusalik     | M      | 1987-05-27 |
+--------+------------+------------+-------------+--------+------------+
20 rows in set (0.20 sec)
```

Podemos ordenar varios campos separando los campos por coma, es decir, la sintaxis de la clausura `ORDER BY` quedaría como `ORDER BY X1 M1, X2 M2, X3 M3, ...`.

> Ejemplo: Seleccionar los registros ordenados primero por el campo `last_name ASC` y después por el campo `birth_date ASC`.

``` sql
mysql> select * from employees order by last_name asc, birth_date asc limit 20;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
| 218417 | 1952-02-03 | Moto       | Aamodt    | F      | 1986-05-29 |
| 455119 | 1952-02-07 | Mani       | Aamodt    | M      | 1991-10-16 |
| 470525 | 1952-04-22 | Geoffry    | Aamodt    | M      | 1995-06-14 |
|  42199 | 1952-05-02 | Mats       | Aamodt    | F      | 1989-10-03 |
|  88414 | 1952-05-29 | Ashish     | Aamodt    | F      | 1992-01-29 |
| 419084 | 1952-07-19 | Shen       | Aamodt    | M      | 1987-08-19 |
|  61219 | 1952-08-08 | Chuanyi    | Aamodt    | M      | 1991-03-01 |
| 474768 | 1952-09-04 | Valeri     | Aamodt    | F      | 1995-04-20 |
| 492771 | 1952-09-22 | Odoardo    | Aamodt    | M      | 1987-04-26 |
| 255282 | 1952-10-01 | Sumant     | Aamodt    | F      | 1988-08-09 |
|  76475 | 1952-10-09 | Theirry    | Aamodt    | F      | 1995-05-08 |
| 290159 | 1952-10-17 | Leandro    | Aamodt    | F      | 1993-03-25 |
| 404703 | 1952-10-18 | Florian    | Aamodt    | M      | 1991-08-04 |
| 206338 | 1952-11-14 | Arlette    | Aamodt    | M      | 1990-01-16 |
|  29182 | 1952-11-17 | Arumugam   | Aamodt    | F      | 1986-01-09 |
| 236802 | 1952-11-23 | Yoshimitsu | Aamodt    | M      | 1989-01-11 |
|  12982 | 1952-12-08 | Sachem     | Aamodt    | F      | 1992-01-11 |
| 403236 | 1952-12-11 | Rafael     | Aamodt    | F      | 1989-02-09 |
|  55985 | 1952-12-11 | Ung        | Aamodt    | F      | 1990-07-15 |
| 252763 | 1953-01-16 | Felicidad  | Aamodt    | F      | 1988-11-20 |
+--------+------------+------------+-----------+--------+------------+
20 rows in set (0.74 sec)
```

> Ejemplo: Seleccionar los registros ordenados primero por el campo `last_name ASC` y después por el campo `birth_date DESC`.

``` sql
mysql> select * from employees order by last_name asc, birth_date desc limit 20;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
| 234739 | 1965-01-31 | Foong      | Aamodt    | M      | 1986-06-28 |
| 100916 | 1965-01-18 | Heejo      | Aamodt    | M      | 1990-04-30 |
|  22105 | 1964-12-10 | Leah       | Aamodt    | M      | 1998-08-07 |
| 450068 | 1964-12-07 | Danco      | Aamodt    | F      | 1986-10-10 |
| 448061 | 1964-12-01 | Conal      | Aamodt    | F      | 1985-05-22 |
| 227725 | 1964-11-26 | Peternela  | Aamodt    | M      | 1985-06-03 |
| 463875 | 1964-10-24 | Tonny      | Aamodt    | M      | 1996-05-31 |
|  11761 | 1964-07-17 | Bartek     | Aamodt    | M      | 1991-06-12 |
| 295537 | 1964-06-30 | Shmuel     | Aamodt    | M      | 1986-05-27 |
| 100860 | 1964-06-20 | Amabile    | Aamodt    | F      | 1993-02-06 |
| 278581 | 1964-05-06 | Shakhar    | Aamodt    | M      | 1987-01-20 |
| 103736 | 1964-04-27 | Garnet     | Aamodt    | F      | 1985-08-06 |
| 290839 | 1964-04-14 | Guoxiang   | Aamodt    | M      | 1997-05-12 |
| 260324 | 1964-04-01 | Salvador   | Aamodt    | M      | 1986-03-16 |
| 408779 | 1964-03-14 | Tamiya     | Aamodt    | M      | 1985-08-03 |
| 489837 | 1964-03-12 | Tzvetan    | Aamodt    | F      | 1988-08-01 |
|  64157 | 1964-01-29 | Geraldo    | Aamodt    | M      | 1987-08-11 |
|  70441 | 1964-01-25 | Luise      | Aamodt    | F      | 1986-07-27 |
| 105020 | 1964-01-20 | Mostafa    | Aamodt    | F      | 1994-01-18 |
|  49426 | 1963-08-13 | Huican     | Aamodt    | M      | 1993-11-01 |
+--------+------------+------------+-----------+--------+------------+
20 rows in set (0.25 sec)
```

> Ejemplo: Consultar la tabla `employees` ordenada por `género ascendente`, `fecha de nacimiento descendente` y `fecha de contratación descendente`, es decir, queremos observar los empleados mujeres más jóvenes que han sido contratadas más recientemente.

``` sql
mysql> select * from employees order by gender asc, birth_date desc, hire_date desc limit 20;
+--------+------------+------------+-----------------+--------+------------+
| emp_no | birth_date | first_name | last_name       | gender | hire_date  |
+--------+------------+------------+-----------------+--------+------------+
|  74344 | 1965-02-01 | Hiroyasu   | Provine         | M      | 1994-11-25 |
| 223595 | 1965-02-01 | Chiranjit  | Dredge          | M      | 1994-11-10 |
| 284045 | 1965-02-01 | Fun        | Seiwald         | M      | 1994-06-24 |
| 424584 | 1965-02-01 | Kagan      | Dredge          | M      | 1994-02-26 |
|  80850 | 1965-02-01 | Koldo      | Luit            | M      | 1993-11-19 |
| 235729 | 1965-02-01 | Henk       | Anger           | M      | 1993-01-19 |
| 248758 | 1965-02-01 | Snehasis   | Muhlberg        | M      | 1991-07-16 |
| 433260 | 1965-02-01 | Make       | Olivero         | M      | 1991-06-01 |
| 270794 | 1965-02-01 | Dannz      | Zhang           | M      | 1990-10-24 |
| 409898 | 1965-02-01 | Divier     | Ishibashi       | M      | 1989-09-22 |
| 213447 | 1965-02-01 | Armond     | Perly           | M      | 1989-09-14 |
| 270792 | 1965-02-01 | Domenico   | Wendorf         | M      | 1988-09-08 |
| 109598 | 1965-02-01 | Stamatina  | Auyong          | M      | 1988-07-05 |
| 106242 | 1965-02-01 | Nevio      | Thisen          | M      | 1988-03-25 |
|  33293 | 1965-02-01 | Adamantios | Vanwelkenhuysen | M      | 1987-12-12 |
| 431759 | 1965-02-01 | Alassane   | Ramsay          | M      | 1987-11-05 |
| 287323 | 1965-02-01 | Steve      | Dengi           | M      | 1987-10-07 |
|  59869 | 1965-02-01 | Zsolt      | Riefers         | M      | 1987-09-25 |
| 241917 | 1965-02-01 | Wuxu       | Poupard         | M      | 1987-01-23 |
| 407837 | 1965-02-01 | Fei        | Erez            | M      | 1986-10-15 |
+--------+------------+------------+-----------------+--------+------------+
20 rows in set (0.30 sec)
```

Observa que el género menor es `M` y no `F`, es decir que los hombres son menores a las mujeres, esto se debe a que el campo fue definido mediante una enumeración `enum('M', 'F')` en la que `M` se definió antes que `F`, es decir, el campo `gender` no es de texto, sino una enumeración.

``` sql
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
```

Entonces tendrían que modificar la consulta para tomar los géneros en modo descendente.

``` sql
mysql> select * from employees order by gender desc, birth_date desc, hire_date desc limit 20;
+--------+------------+----------------+--------------+--------+------------+
| emp_no | birth_date | first_name     | last_name    | gender | hire_date  |
+--------+------------+----------------+--------------+--------+------------+
| 222616 | 1965-02-01 | Badri          | Schapire     | F      | 1995-12-08 |
| 298953 | 1965-02-01 | Zita           | Syang        | F      | 1995-06-16 |
| 437369 | 1965-02-01 | Kazuhide       | Biran        | F      | 1995-01-25 |
| 471397 | 1965-02-01 | Nigel          | Zirintsis    | F      | 1995-01-20 |
| 240132 | 1965-02-01 | Mizuhito       | Kobara       | F      | 1994-09-13 |
| 272860 | 1965-02-01 | Minghong       | Candan       | F      | 1994-02-10 |
| 418860 | 1965-02-01 | Ymte           | Dalton       | F      | 1992-11-22 |
| 294996 | 1965-02-01 | Tomoyuki       | Vigier       | F      | 1991-07-08 |
| 411906 | 1965-02-01 | Anneli         | Pappas       | F      | 1990-07-09 |
|  60091 | 1965-02-01 | Surveyors      | Bade         | F      | 1988-05-01 |
|  93278 | 1965-02-01 | Magdalena      | Penn         | F      | 1987-04-27 |
| 217800 | 1965-02-01 | Shay           | Servieres    | F      | 1987-04-11 |
| 406714 | 1965-02-01 | Malu           | Ossenbruggen | F      | 1986-11-21 |
| 211569 | 1965-02-01 | Zhonghua       | Snyers       | F      | 1986-11-06 |
|  93549 | 1965-02-01 | Arie           | Coullard     | F      | 1986-11-01 |
| 243077 | 1965-02-01 | Kazunori       | Perz         | F      | 1986-08-28 |
|  66702 | 1965-02-01 | Deniz          | Thibadeau    | F      | 1986-03-11 |
| 470472 | 1965-02-01 | Zhiguo         | Staudhammer  | F      | 1985-12-03 |
| 449984 | 1965-02-01 | Hinrich        | Perin        | F      | 1985-11-20 |
| 214866 | 1965-02-01 | Gopalakrishnan | Angel        | F      | 1985-09-19 |
+--------+------------+----------------+--------------+--------+------------+
20 rows in set (0.30 sec)
```

### Restringir las búsquedas mediante la clausura `WHERE`

La clausura `WHERE` es una de las más utilizadas en todas las consultas a la base de datos, básicamente se encarga de restringir los registros que serán operados, es decir, cuándo hacemos una consulta la base de datos tomará todos los registros para hacer la consulta, pero con la clausura `WHERE` sólo se aplicará el resto de la consulta a los registros que cumplan las condiciones impuestas por `WHERE`. La sintaxis es `WHERE c1 OP c2 OP c3 ...`. Las condices `WHERE` pueden ser comparaciones numéricas (`>, <, >=, <=, =, <>, ...`), pueden ser consultas en texto (`LIKE %angel%`) u operaciones más avanzadas.

> Ejemplo: Seleccionar todos los registros con género `F` (mujeres) que en su apellido contengan la palabra `angel` y hayan nacido después de `1965-01-01`.

``` sql
mysql> select * from employees where gender='F' and last_name like '%angel%' and birth_date > '1963-01-01' limit 20;
+--------+------------+----------------+--------------+--------+------------+
| emp_no | birth_date | first_name     | last_name    | gender | hire_date  |
+--------+------------+----------------+--------------+--------+------------+
|  16781 | 1963-04-12 | Emdad          | Cangellaris  | F      | 1985-10-15 |
|  17124 | 1964-05-21 | Amabile        | Angelopoulos | F      | 1987-07-15 |
|  21181 | 1965-01-21 | Jianhui        | Cangellaris  | F      | 1988-01-12 |
|  23534 | 1964-12-05 | Lidong         | Cangellaris  | F      | 1992-09-04 |
|  34206 | 1964-12-19 | Kiam           | Cangellaris  | F      | 1994-03-12 |
|  34722 | 1963-09-21 | Tze            | Angel        | F      | 1987-09-13 |
|  41473 | 1963-08-30 | Ohad           | Angelov      | F      | 1992-02-21 |
|  47304 | 1964-11-12 | Rosita         | Angel        | F      | 1991-09-14 |
|  70780 | 1965-01-27 | Behnaam        | Angelov      | F      | 1995-01-29 |
|  77782 | 1964-05-20 | Yoshimitsu     | Angelov      | F      | 1986-04-09 |
|  78431 | 1964-11-04 | Sandeepan      | Angel        | F      | 1988-06-13 |
|  89688 | 1963-08-14 | Charmane       | Angel        | F      | 1992-08-03 |
|  95410 | 1963-09-13 | Bodo           | Angelov      | F      | 1997-05-06 |
|  99556 | 1963-05-04 | Sangeeta       | Cangellaris  | F      | 1991-11-05 |
| 103838 | 1963-10-16 | Mart           | Cangellaris  | F      | 1991-11-05 |
| 108172 | 1963-01-24 | Alois          | Angelopoulos | F      | 1990-07-17 |
| 205269 | 1963-10-31 | Uriel          | Angel        | F      | 1995-10-18 |
| 213565 | 1963-10-28 | Przemyslawa    | Angel        | F      | 1991-10-11 |
| 214866 | 1965-02-01 | Gopalakrishnan | Angel        | F      | 1985-09-19 |
| 224510 | 1964-05-08 | Weiru          | Cangellaris  | F      | 1988-03-29 |
+--------+------------+----------------+--------------+--------+------------+
20 rows in set (0.10 sec)
```