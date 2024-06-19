- Tags: #web #wordpress #reconocimiento #herramientas
----
La herramienta **Wpscan** Es utilizada  para enumerar enumerar páginas web que dispongan de un gestor de contenidos [[Wordpress]]

Podríamos realizar un escaneo a modo de prueba ejecutando el siguiente comando de Bash
```bash
Wpscan --url https://tinder.com
```
si quisiéramos enumerar usuarios válidos potenciales extistentes en un dominio haríamos:
```bash
wpscan --url https://tinder.com --enumerate u
```

Otra de las posibilidades de esta herramienta es que es capaz de aplicar gfuerza bruta par a descubrir credenciales válidas en los paneles de autnticación

---
##Referencias
[^1]:Enlace al proyecto [Enlace]
[^1]:Procedimiento manual abusando del XMLRPC [[XMLRPC]]