Los navegadores no interpretan SQL directamente. En su lugar, las solicitudes del usuario se envían al servidor, donde el backend maneja las consultas SQL y la lógica de la aplicación.
Es un oftware de base de datos basada en el CRUD(Create, read, uPdate, delete)
Create (create user 'PolarTang'@'localhost' identified by 'Marco123';)
Read (describe tabla_name)
Update(insert into(columna1, columna2, columna3) values(1, 'nombre_columna2', 'Nombre_columna3' )

## Create
```
create user 's4vitar'@'localhost' identified by 's4vitar';
```



## Read
```
describe table_name
```
te muestra los campos de la tabla
```
select * from table_name;
```
te selecciona todo lo de la tabla
```
select username from users where id = '3' order by 2;-- -';
```
Lo primero que tenemos que hacer como atacante es ver cuantas columnas hay dentro de la database
## Update
```
grant all privileges on Hack4u.* to 'PolarTang'@'localhost';
```
le asigna todos los privilegios
https://github.com/Datalux/Osintgram
```
update users set id=2 where username='name'
```
el comando update es para cambia algo de un campo