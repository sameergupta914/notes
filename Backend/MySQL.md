# Dbms notes

- database-> it is a shared collection of data

- dbms-> helps to define, manipulate, sharing of data, control data redundancy, helps us to put constraints, restriction of authorization, east backup and recovery

- RDBMS -> relational dbms maintains data in the form of tables, eg- mysql, oracle, sql, etc.
- sql-> structured query language, it is a declarative language.

- list all databases

```sql
SHOW DATABASES;
```

- create a new db

```sql
CREATE DATABASE movies;
```

- start working on a db

```sql
USE movies;
```

- delete a whole db

```sql
DROP DATABASE movies;
```

- create a table

```sql
CREATE TABLE details(name varchar(20) NOT NULL,
 age INT,
 caste VARCHAR(20), 
 college VARCHAR(20), 
 Id INT AUTO_INCREMENT, 
 PRIMARY KEY(Id)
 );
```

- create table if not exists

```sql
CREATE TABLE IF NOT EXISTS details(name varchar(20) NOT NULL, age INT, caste VARCHAR(20), college VARCHAR(20), Id INT AUTO_INCREMENT, PRIMARY KEY(Id));
```

- show all tables-> `SHOW TABLES;`

- delete a table -> `DROP TABLE details;`

- get details about a table and its attributes-> `DESCRIBE details;` or `DESC details;`

NUMERIC - INT, DECIMAL, BIGINT,
STRING- CHAR, VARCHAR, ENUM
DATETIME- DATE, TIME, DATETIME
JSON

- insert data into table
`INSERT INTO details(name, age, caste) VALUES ("sameer", 20, "baniya");` 

`INSERT INTO details(name, age, caste) VALUES ("sameer", 20, "baniya"),("sahil", 20, "baniya");`

- retrieving from table
`SELECT * FROM details;` `SELECT name, age FROM details;`

- WHERE clause
`select * from details where id<3;`

- operators- >, <, >=, <=, { !=, <>} (not equals), =, IN, LIKE, etc

- LIKE
`select col1, col2,.. from table where col1 like %string%;` 
- prefix matching ->`select * from actors where name like "chris%"`
- suffix-> `%chris` 
- in between->  `%chris%`

- combing query filters

`select * from actors where charges >= 3500000 and id<4;`

`select * from actors where not(charges >= 3500000 and id<4);`

- ORDER BY
`select * from actors order by charges desc;`

- first acc to charges in desc then name in ascending-> `select * from actors order by charges desc, name;`
- first acc to charges then name in desc->
`select * from actors order by charges desc, name desc;`

- limit and offset
- get limited no. of data use limit
- define the starting point from which we have to fetch the data use offset
`select * from actors order by charges desc LIMIT 2 OFFSET 1;

- update table
`update actors set name="jennifer laurence" where name="tara sutaria";`

- alter table
`alter table actors add DOB datetime;` alter table actors drop dob;

- join
`select * from movies join actors on [movies.actor=actors.id](http://movies.actor=actors.id/);`

`select * from movies as m join actors as a on [m.actor=a.id](http://m.actor=a.id/);`

- ORM (object relational mapper)-> Libraries that help you to actually do database queries but instead writing sql queries syntax you write object oriented syntax

- to find difference betwwen consecutive row
`datediff(w1.recordDate,w2.recordDate)=1`

- to compare consecutive rows
`SELECT [w1.id](http://w1.id/) FROM Weather w1, Weather w2 WHERE DATEDIFF(w1.recordDate, w2.recordDate) = 1 AND w1.temperature > w2.temperature;`

- left join
`SELECT column_name(s) FROM table1 LEFT JOIN table2  ON table1.column_name = table2.column_name;`