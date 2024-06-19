Hay ciertas funciones en sql que hacen zafar de una inyeccion, sanitización,
en este caso la funcion es mysqli_real_escape_string que hace que no tome las comillas de la inyeccion que se pretende aplicar, conn es de la conexión y GET es el método que se pretende 
```php
$id = mysqli_real_escape_string($conn, $_GET['id']);
```
más abajo en el código podría tener algo así, es muy importante que en la parte de id la encierre con comillas simp´les porque de lo contrario podria iniciar una query que no empiece necesariamente por comillas
```php
$data = myslqi_real($conn, "select username from users where id = $id");
```
Antes tecinamos una query que era algo como 
```php
select username from users where id = 1;
```
como tenia las comillas siemple era necesario meterle un comentario al final, pero si tenemosel id sin comilla no nesecitamos el com,entario ( representaco como #' o -- -')

```
curl -s -X GET "http://localhost/searchUsersa.php?id=2" -I
```
Otra forma es que registre que todo lo que continúe después de cierto parámetro que va a ir en el link, en este caso id, lo muestre como un error, para que no pueden manipular las queries desde el url, son las [[Boolean-based blind SQL Injection]]

```php
if(isset($response['id'])){
	echo $response['id'];
	http_response_code(404);
}
```
```php
if(isset($response['id'])){
	echo $response['id'];
	http_response_code(404);
}
```