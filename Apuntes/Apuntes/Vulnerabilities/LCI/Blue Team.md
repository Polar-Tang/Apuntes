- - -
Una forma de sencilla de securizar
```php
<?php
        $filename = $_GET['filename']:
        include("/var/www/html/" . $filename);
?>
```
Pero hay que tener en cuenta que pueden hacer un [[Undirectory Path Trasversal]] se puede inyectar simplemente añadiendo varias salidas de archivo "../" ![[Pasted image 20240606151429.png]]
Si los últimos 6 caracteres son iguales a passwd la página no muestra nada, pero esto se burla sencillo también 
![[Pasted image 20240606162224.png]]