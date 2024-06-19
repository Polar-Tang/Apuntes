 Es un token simple que se utiliza para identificar el origen de las solicitudes. Se usa principalmente para controlar el acceso a la API, rastrear el uso y prevenir abusos. A diferencia de los Bearer Tokens, las API Keys no llevan información sobre el usuario ni los permisos.
Puede enviarse en el encabezado, en la URL, o en el cuerpo de la solicitud.Ejemplo:
```
GET /protected/resource?api_key=abcdef12345 HTTP/1.1
Host: api.example.com
```
