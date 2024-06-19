Son capacidad que te permiten ejecutar ceirtas tareas con privilegio

Ahora vamos a ver una forma de [[Enumeración de sistemas]] con las capabilities
Se busca de la siguiente forma archivos con capabilities:
```
getcap -r / 2>/dev/null
```
Para añadir una capability a python3.9

```
setcap cap_setuid+ep /usr/bin/python3.9
```
Y si vemos  los permisos del archivo con un [[Which]]
```
which python3.9 | xargs ls -l
```
pero ahora que le añadi las capabilities puedo ejecutar el archivo

```
which python3.9 | xargs getcap
```
