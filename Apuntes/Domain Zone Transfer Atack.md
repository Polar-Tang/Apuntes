La transferencia de zona es una técnica la cual es empleada por los atacante para obtener info sobre los nombre de dominio y a sitios web que estén asociadas a un a un sitio web o a un dominio.  Trata de realizar una copia de todo lo que es toda la base de datos, nombre de dominio, direcciones web asocicadas al dominio. Esta basada en una debilidad del protocolo [[DNS]] manda una [[solicitud de transferencia de zona]] a los servidores de nombre de dominio que están configurado para responder este tipo de solicitudes. Si el servidor de nombre es vulnerable al responder otorga info privilegiada.
Existen dos tipos de solicitudes
## AXFR
Es toda la transferencia de zona completa como output para el atacante

hacemos un `git clone https://github.com/vulhub/vulhub`
`cd ~/vulhub/dns/dns-zone-transfer`
`nano named.conf.local`
y cambiamos donde dice zone ![[Pasted image 20240617124748.png]]
SI CATEAMOS LA BASE DE DATOS
![[Pasted image 20240617124831.png]]
Lo ns son la resolución de dominio por la zona, lo del final son subdominios a los cuales apunta para resolver, si enviara un solicitude de transferencia de zona para que el servidor otergue la infromación, lo cúal es crítico
En los CTF's suele ser así, dentro de la data base `nano  vulnhub.db`
![[Pasted image 20240617125214.png]]
Desplegamos el contenedor con un `docker-sompose up -d`
```
dig axfr @127.0.0.1 kali.local
```
dig comand
axfr el método, la vuln
@127.0.0.1 en este caso es la ip, que es el localhost
kali.local es el dominio que lo configuramos dentro del named.conf.local
![[Pasted image 20240617130013.png]]
## IXFR
Son las transferencias de zona incremental, la cual es más parcial porque se transfiere partiendo de la última transferencia que se hizo. La información esta incompleta

dns-zone-transfer