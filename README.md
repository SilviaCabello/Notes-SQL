login: brew services start mysql
 mysql -u user -p

<img width="617" alt="Captura de pantalla 2021-06-06 a las 11 43 46" src="https://user-images.githubusercontent.com/78726341/120919910-755b4880-c6bc-11eb-8896-67a48b8ddc13.png">
<img width="614" alt="Captura de pantalla 2021-06-06 a las 11 44 07" src="https://user-images.githubusercontent.com/78726341/120919921-81470a80-c6bc-11eb-83a6-650602f45390.png">


# UNIQUE constraint

--> Syntax to define a UNIQUE constraint:

CREATE TABLE table_name(
 ...,
 column_name data_type UNIQUE,
 ...
 );
 
 
 --> Syntax to define a UNIQUE constraint for two or more columns:
 
 CREATE TABLE table_name(
  ...,
  column_name1 column_definition,
  column_name2 column_definition,
  ...,
  UNIQUE(column_name1,column_name2)
 );
 
 
 
 --> When you want to make UNIQUE the combination of two columns:
 
 CREATE TABLE suppliers (
    supplier_id INT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    phone VARCHAR(15) NOT NULL UNIQUE,
    address VARCHAR(255) NOT NULL,
    PRIMARY KEY (supplier_id),
    CONSTRAINT uc_name_address UNIQUE (name , address)
);


--> To drop a unique constraint

DROP INDEX index_name ON table_name;
DROP INDEX uc_name_adress ON suppliers;


--> To add a new unique constraint

ALTER TABLE suppliers
ADD CONSTRAINT uc_name_address 
UNIQUE (name,address);



# --------------- Advanced SQL Workshop -----------------

The firstname, lastname and age from the Person 
The firstname, lastname of the Person and their Kingdom, only for those connected to a Kingdom
The same as above, including all of the Person (LEFT JOIN)
The average age of the Person
The number of Person per Kingdom (include kingdoms with no Person) (RIGHT JOIN)
The average age by role.
Retrieve the average of all the Person which are not magicien
List all of the Person with their role and possible realm (Kingdom)
Show realms (Kingdom) that have at least 2 people (Person table)


mysql> SELECT firstname, lastname, age FROM person;
+-------------+---------------+-----+
| firstname   | lastname      | age |
+-------------+---------------+-----+
| Arthur      | Pendragon     |  35 |
| Guenièvre   | NULL          |  30 |
| Merlin      | NULL          | 850 |
| Perceval    | NULL          |  36 |
| Caradoc     | NULL          |  32 |
| Calogrenant | NULL          |  44 |
| Leodagan    | NULL          |  47 |
| Lancelot    | Du Lac        |  33 |
| Elias       | De Kelliwic'h |  52 |
| Mevanwi     |               |  28 |
| Yvain       |               |  23 |
+-------------+---------------+-----+
11 rows in set (0,00 sec)

mysql> SELECT person.firstname, person.lastname, kingdom.name AS Kingdom FROM Person JOIN Kingdom ON person.kingdom_id = Kingdom.id;
+-------------+-----------+-----------+
| firstname   | lastname  | Kingdom   |
+-------------+-----------+-----------+
| Arthur      | Pendragon | Logre     |
| Calogrenant | NULL      | Caledonie |
| Guenièvre   | NULL      | Carmelide |
| Leodagan    | NULL      | Carmelide |
| Yvain       |           | Carmelide |
| Caradoc     | NULL      | Vannes    |
| Mevanwi     |           | Vannes    |
| Perceval    | NULL      | Galles    |
+-------------+-----------+-----------+
8 rows in set (0,00 sec)

mysql> SELECT person.firstname,
    -> person.lastname, 
    -> kingdom.name AS Kingdom
    -> FROM Person 
    -> LEFT JOIN Kingdom
    -> ON person.kingdom_id = Kingdom.id;
+-------------+---------------+-----------+
| firstname   | lastname      | Kingdom   |
+-------------+---------------+-----------+
| Arthur      | Pendragon     | Logre     |
| Guenièvre   | NULL          | Carmelide |
| Merlin      | NULL          | NULL      |
| Perceval    | NULL          | Galles    |
| Caradoc     | NULL          | Vannes    |
| Calogrenant | NULL          | Caledonie |
| Leodagan    | NULL          | Carmelide |
| Lancelot    | Du Lac        | NULL      |
| Elias       | De Kelliwic'h | NULL      |
| Mevanwi     |               | Vannes    |
| Yvain       |               | Carmelide |
+-------------+---------------+-----------+
11 rows in set (0,00 sec)

mysql> SELECT AVG(age) AS Average Age FROM Person;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'Age FROM Person' at line 1
mysql> SELECTmysql> 
mysql> ;
;
^C
mysql> SELECT AVG(age) AS Average
    -> FROM Person;
+----------+
| Average  |
+----------+
| 110.0000 |
+----------+
1 row in set (0,00 sec)

mysql> SELECT AVG(age) AS Average_Age
    -> FROM Person;
+-------------+
| Average_Age |
+-------------+
|    110.0000 |
+-------------+
1 row in set (0,00 sec)

mysql> SELECT Kingdom.kingdom_name,
    ->        COUNT(Person.id)
    -> FROM Person
    ->     RIGHT JOIN Kingdom
    ->         ON person.kingdom_id = Kingdom.id
    -> GROUP BY Person.kingdom_name;
ERROR 1054 (42S22): Unknown column 'Kingdom.kingdom_name' in 'field list'
mysql> SELECT Kingdom.name,
    ->        COUNT(Person.id)
    -> FROM Person
    ->     RIGHT JOIN Kingdom
    ->         ON person.kingdom_id = Kingdom.id
    -> GROUP BY kingdom.name;
+-----------+------------------+
| name      | COUNT(Person.id) |
+-----------+------------------+
| Logre     |                1 |
| Caledonie |                1 |
| Carmelide |                3 |
| Vannes    |                2 |
| Galles    |                1 |
| Aquitaine |                0 |
+-----------+------------------+
6 rows in set (0,00 sec)

mysql> SELECT person.role_id,
    ->        AVG(age) AS Average_Age
    -> FROM person
    -> GROUP BY person.role_id;
+---------+-------------+
| role_id | Average_Age |
+---------+-------------+
|    NULL |     29.0000 |
|       1 |     42.0000 |
|       2 |     31.0000 |
|       3 |    451.0000 |
+---------+-------------+
4 rows in set (0,00 sec)

mysql> SELECT role.role,
    ->        AVG(age) AS Average_Age
    -> FROM person
    ->     JOIN Role
    ->         ON role.id = person.role_id
    -> GROUP BY role.role;
+-----------+-------------+
| role      | Average_Age |
+-----------+-------------+
| roi       |     42.0000 |
| chevalier |     31.0000 |
| magicien  |    451.0000 |
+-----------+-------------+
3 rows in set (0,00 sec)

mysql> 
