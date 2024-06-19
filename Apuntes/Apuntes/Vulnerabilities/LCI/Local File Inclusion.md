- - -
Consiste que el la url pueda incluir un archi que enrealidad sea mío
![[Pasted image 20240606141959.png]]
Suponiendo que la página este presurizada para que no le metan un [[Undirectory Path Trasversal]] de la siguiente manera 
```php
<?php
    $filename = $_GET['filename']:
    $filename = str_replace("../", "", $filename);

    include("/var/www/html/" . $filename . ".php");
?>
```
De esta manera está impidiendo que busque por las extenciones php.
En la versión 5. y algo todavía no habían corregido en null bait injection, que consiste en meterle un porcentaje cero al final y de esta manera cuela
![[Pasted image 20240606154055.png]]
![[Pasted image 20240606154616.png]]

