apt-file search ifconfig | grep bin
Ahi lo que te cuenta es el paquete y en que ruta te va a meter ese ejecutable que buscas, siempre será si es ejecutable algun directorio que contenga bin, sbin o similar, por lo que con grep lo puedes filtrar
ver todos los ejecutables que trae net-tools:
```
dpkg -L net-tools | grep bin
```
Para remover un paquete y todas sus dependencias
```
apt remove --purge npm
```