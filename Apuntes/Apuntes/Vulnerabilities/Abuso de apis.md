

Voy a enviarte unos apuntes. Nesecito que hagas un examen con 10 preguntas y/o consignas. Quiero que hagas un examén díficil y desafiante, de todos los apuntes. No olvides ninguna parte y recuerda que estan copiados desde obsidian por lo que no te asustes si ves algunas palabras entre corrchetes, hasshtags e incluso ![[Pasted image]] 
 los apuntes son estos:

Vamos a jugar con un laboratorio de [apis]([https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI))
![[Pasted image 20240616204017.png]]
```
cd /home/kali/crAPI/deploy
curl -o docker-compose.yml https://raw.githubusercontent.com/OWASP/crAPI/develop/deploy/docker/docker-compose.yml**
VERSION=develop docker-compose pull
VERSION=develop docker-compose -f docker-compose.yml --compatibility up -d
```
La api genera un [[json web token]], que es algo así como un [[cookie de sesión]] que vincula mi cuenta a los datos de la misma.
usuario (como su ID) y lo firma con una clave secreta. Este token es enviado al cliente (como un navegador o una aplicación Postman) y se almacena, generalmente, en las cookies o en el almacenamiento local del navegador.

En cada solicitud subsiguiente, el cliente envía este token al servidor en el encabezado de autorización (`Authorization: Bearer <token>`). El servidor verifica el token para asegurarse de que es válido y que el usuario está autenticado.
lo podemos ver apenas loguear en la parte de network del inspector:
![[Pasted image 20240616230106.png]]El json web token
![[Pasted image 20240616232540.png]]
Primero me descargo el postman, dentro de este le doy a create collection > new > HTTP y creo una petición de http a la url `http://localhost:8888/identity/api/auth/login` que extraje cuando le abri el network y me loguee a mi cuenta y entonces le hice click al network del login y ahí estaba la petición
![[Pasted image 20240616233945.png]]
(en la imagen dice get pero es POST).
Dentro de la página, en el inspector voy a request, y lo pongo en formato raw
![[Pasted image 20240616234300.png]]
Y ese json lo pongo en el postman > Body, aclarando que esta en raw y que el lenguaje es json (entonces le cambia el content-type)
![[Pasted image 20240616234417.png]]
Le damos al save que está en la esquina superior derecha y lo guardamos en el new collection
Si hacemos el mismo proceso con dashboard (que está dentro del network) y le damos a send nos va a dar un error, porque la petición solo funcionaría si ya estaríamos logueados, y como lo estamos ejecutando desde una página aislada no funciona meeeen (En el Header podemos ver el token de sesión)
![[Pasted image 20240616235018.png]]
- - -
## Scope del token
Si ponemos la authorization beader, osea esa especie de cokie de sesión que extraímos del header
clickeamos new Collection > variables y creamos una variable en esa tablita
y pegamos en token dentro del current value para que tenga mayor [[scope]]
![[Pasted image 20240616235300.png]]
Aclaramos el lenguaje en la pestaña Authorization y ponemos lo correspondiente, el tipo de token y luego, en token, el nombre de la variable
![[Pasted image 20240616235631.png]]
- - -
## Enumeración dentro de Postman
Ahora entramos shop, abrimos el inspector y vamos a network, recargamos para interceptar la petición

![[Pasted image 20240617000019.png]]
Y procedemos a aprehender, dentro del postman, las peticiones http.
![[Pasted image 20240617003403.png]]
- - -
## Vulnerabilidad de OTP
![[Pasted image 20240617003733.png]]
Se envia un OTP al localhost:8025
![[Pasted image 20240617003853.png]]
Al ser 4 digitos es bastante débil y podemos usar una combinatoria de un diccionario de sec list tranquilamente
Interceptamos el cambio de contraseña con el network
![[Pasted image 20240617004602.png]]
Copiamos el request del body
![[Pasted image 20240617004848.png]]
Para el ataque de fuerza bruta utilizamos en [[ffuf]]
```
ffuf -u http://localhost:8888/identity/api/auth/v3/check-otp -w /usr/share/SecLists/Fuzzing/4-digits-0000-9999.txt -X POST -d '{"email":"roberto@gmail.com","otp":"FUZZ","password":"p@Wn3ddddd"}' -p 1
```

*-u* Representa la url, que es la misma que en la petición que tuvimos que poner en el body 
*-w* es la wordlist, o el diccionario
*-X* Representa el método
*-d* representa la data que queremos manipular, donde dice FUZZ es donde va aprobar ataque de fuerza bruta
*-p* es para que vaya más lento y demore un segundo por solicitud

Rápidamente nos va a bloquear el sistema para detenerel ataque, sin embargo,
La documentación oficial de la API a menudo lista todas las versiones disponibles y sus endpoints. Revisar la documentación puede proporcionar una visión clara de las versiones que se pueden utilizar. si vemos en la url tenemos v3 por la versión y si esta mal securizado podemos ir probando las versiones:
![[Pasted image 20240617010831.png]]
Así que ejecutamos el mismo comando pero modificando la ruta por el de la versión anterior:

```
ffuf -u http://localhost:8888/identity/api/auth/v2/check-otp -w /usr/share/SecLists/Fuzzing/4-digits-0000-9999.txt -X POST -d '{"email":"roberto@gmail.com","otp":"FUZZ","password":"p@Wn3ddddd"}'  -H "Content-Type: application/json" -p 1 -mc 200
```
*-H* aclara el tipo de lenguaje
![[Pasted image 20240617011435.png]]
PAWNED

otra forma de ver las versiones viejas es con [[nmap]]
```
nmap -p 8888 --script=http-enum localhost
```
Tambien podemos ver las versiones viejas soportadas utilizando curl
```
curl -X OPTIONS http://localhost:8888/identity/api/

```
- - -
Ahora interceptamos la pestana de products
![[Pasted image 20240617012701.png]]
Tambien podemos ir probando métodos
![[Pasted image 20240617012857.png]]
Ahora el ffuf sería masomenos así:
```
ffuf -u "http://localhost:8888/workshop/api/shop/products?limit=30&offset=0" -w /usr/share/SecLists/Fuzzing/http-request-methods.txt -X FUZZ -p 1
```
Vemos los que acepta, pero buscamos un [[código de estado]] de 200
![[Pasted image 20240617014350.png]]
El método options para obtener una descripción completa de los métodos de API disponibles en el servicio. El método `OPTIONS` es útil para descubrir los métodos HTTP que un endpoint soporta y qué tipo de peticiones y parámetros acepta. Nos pide la solicitud de un precio requerido, nombre, imagen, como si creamos un nuevo producto, que crear un nuevo producto sería un [[mass asignment atack]], vamos a avaible product o shop como le pusimos nosotros, lo sacamos de options, porque era para rastrear métodos y lo ponemos en POST para que lo publique,
enviamos en el body y agregamos un nuevo producto
![[Pasted image 20240617014943.png]]
Vamos al http de comprar productos y compramos el producto que creamos:
![[Pasted image 20240617015049.png]]
- - -
## Cupones
Ahora vamos a la opción de cupones
![[Pasted image 20240617015336.png]]
ponemos el request
![[Pasted image 20240617015440.png]]
Dentro del body
![[Pasted image 20240617015454.png]]
No devuelve nada, son unas llaves vacías porque es inválido, está corriendo un mongodb así que le mandamos una NoSQLI
![[Pasted image 20240617015726.png]] (esta en pretty), ponomes un código de cupon not equal 1 lo cual es válido
![[Pasted image 20240617015853.png]]
y ponemos el código del cupón
- - -
Dashboard > add a Vehicle, y nos llega el pin code en el localhost:8025
![[Pasted image 20240617020556.png]]
interceptamos el location
![[Pasted image 20240617020714.png]]
En el response vemos los datos, no veo el de los usarios porque sino sería un [[Bolap (broken objetc level authorization)]] o capaz sí, tendríamos que proporcionar el carid de otro usuario
lo enumeramos y directamente save![[Pasted image 20240617020835.png]]
En comunity vemos a los usuarios, si interceptamos a uno de esos usuarios:
![[Pasted image 20240617020918.png]]
y luego vamos a response me tira más informació de la que debería
![[Pasted image 20240617020941.png]]
Ponemos el ID del auto en el postman![[Pasted image 20240617021032.png]]
	
eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJyb2JlcnRvQGdtYWlsLmNvbSIsImlhdCI6MTcxODY0MjU4MywiZXhwIjoxNzE5MjQ3MzgzLCJyb2xlIjoidXNlciJ9.WwR5Ny1czGWCp2vHHQZL88iRmRZVvZvfqqVk_QNwnAqf9as3d8HhqGEPu3xpLbJYtWqKdFcbBqvGiL1wPhrfnS2b9eTzB-mP6ba36ahhMsYCxbS4I4lfUmkJpTLrlv7VPOdRn4zNG7kdMaujaEXVUxvnIGPCB2lNDNxQuXkiyMZ5NUI7DSdSlZOyDleXdpJb3ycmfZzt2w8RKELG9Tg4H5eKNPEo95t6WhNnXph6DVSMZfw5VTS553j001XCbedHeGFvRFVjP5slMnVfjRKxFD1O3LVErbalyF1D6Dn4R94t2WnX0o_uTLtCT_dvCmmciYarjuwoWCYeS5dTcyJZvQ
contenthub.netacad.com/devnet/4.4.5