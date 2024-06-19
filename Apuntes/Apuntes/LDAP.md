Ldap le usan las empresas que utilizan herramientas y distribuciones de linux, naturalmente corre por el puerto TCP 389.
Vemos lo que esta corriento detrás del contenedor que nos montamos de: [Docker Github](https://github.com/motikan2010/LDAP-Injection-Vuln-App)
```
ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin 'cn=admin'
```

*-x* : simple authentication, si tenes un usuario y contraseña lo representas de la siguiente forma
*-D* : Indica el usuario
*-w* : Indica la contraseña
*'cn=admin'* : Es un criterio de búsqueda
![[Pasted image 20240614120414.png]]
También con el lapsearch se puede complicar con los filtros de búsqueda
en vez de poner 'cn=admin' le mandamos lo siguiente para el final:
```
(&
	(cn=admin)
	(userPassword=admin)
)
```
*En este caso concatena dos parámetros de búsqueda, el de la cuenta admin y la contraseña admin*
```
(| (&
	(cn=admin)
	(edad=15)
)
(&
	(cn=admin)
	(rol=pentester)
)
)
```
En este caso, cuando vamos al burpsuite, vemos que se parece a las [[NoSQL]] injection porque en la petición interceptada, debajo de todo ponemos "user_id=b*" es decir un usuario que empiece por b y que sea cualquier cosa lo que siga
En la petición puede meter un null bait de cheto
```
(&(cn=admin))%00)((password=*))
```
De esta manera estamos haciendo que nos verifique el usuario sencillamente, porque comentamos el resto de la query, lo estás omitiendo. En el burpsuite se verría algo así
![[Pasted image 20240614125837.png]]
Ahí vemos 
```
user_id=admin))%00&password=testing&login=1&submit=Submit
```



![[Pasted image 20240614124730.png]]

Enviamos el archivo mientras escuchamos por el puerto 443
![[Pasted image 20240614123118.png]]


Para enumerar con los escripts que trae perce nmap, hacemos lo siguiente
```
nmap --script ldap\* -p389 localhost
```

![[Pasted image 20240614115122.png]]
Vemos que el DC, el dominio lo divide en dos

Las inyecciones **LDAP** (**Protocolo de Directorio Ligero**) son un tipo de ataque en el que se aprovechan las vulnerabilidades en las aplicaciones web que interactúan con un servidor LDAP. El servidor LDAP es un directorio que se utiliza para almacenar información de usuarios y recursos en una red.

La inyección LDAP funciona mediante la inserción de comandos LDAP maliciosos en los campos de entrada de una aplicación web, que luego son enviados al servidor LDAP para su procesamiento. Si la aplicación web no está diseñada adecuadamente para manejar la entrada del usuario, un atacante puede aprovechar esta debilidad para realizar operaciones no autorizadas en el servidor LDAP.

Al igual que las inyecciones SQL y NoSQL, las inyecciones LDAP pueden ser muy peligrosas. Algunos ejemplos de lo que un atacante podría lograr mediante una inyección LDAP incluyen:

- Acceder a información de usuarios o recursos que no debería tener acceso.
- Realizar cambios no autorizados en la base de datos del servidor LDAP, como agregar o eliminar usuarios o recursos.
- Realizar operaciones maliciosas en la red, como lanzar ataques de phishing o instalar software malicioso en los sistemas de la red.

Para evitar las inyecciones LDAP, las aplicaciones web que interactúan con un servidor LDAP deben validar y limpiar adecuadamente la entrada del usuario antes de enviarla al servidor LDAP. Esto incluye la validación de la sintaxis de los campos de entrada, la eliminación de caracteres especiales y la limitación de los comandos que pueden ser ejecutados en el servidor LDAP.

También es importante que las aplicaciones web se ejecuten con privilegios mínimos en la red y que se monitoreen regularmente las actividades del servidor LDAP para detectar posibles inyecciones.

A continuación, se proporciona el enlace directo al proyecto de Github que nos descargamos para desplegar un laboratorio práctico donde poder ejecutar esta vulnerabilidad: