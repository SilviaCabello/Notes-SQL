login: brew services start mysql
 mysql -u user -p

<img width="617" alt="Captura de pantalla 2021-06-06 a las 11 43 46" src="https://user-images.githubusercontent.com/78726341/120919910-755b4880-c6bc-11eb-8896-67a48b8ddc13.png">
<img width="614" alt="Captura de pantalla 2021-06-06 a las 11 44 07" src="https://user-images.githubusercontent.com/78726341/120919921-81470a80-c6bc-11eb-83a6-650602f45390.png">


# UNIQUE constraint

Syntax to define a UNIQUE constraint:

CREATE TABLE table_name(
 ...,
 column_name data_type UNIQUE,
 ...
 );
 
 
 Syntax to define a UNIQUE constraint for two or more columns:
 
 CREATE TABLE table_name(
  ...,
  column_name1 column_definition,
  column_name2 column_definition,
  ...,
  UNIQUE(column_name1,column_name2)
 );
 
 
