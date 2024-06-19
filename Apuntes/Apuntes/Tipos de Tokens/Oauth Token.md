**OAuth Token:**

- **Descripción:** Utilizado en el marco de autenticación OAuth, estos tokens se obtienen mediante un flujo de autenticación y se utilizan para acceder a recursos en nombre de un usuario. Hay dos tipos principales: tokens de acceso (access tokens) y tokens de actualización (refresh tokens).
- **Uso:** Los tokens de acceso se envían en el encabezado de la solicitud como `Authorization: Bearer <token>`. Los tokens de actualización se usan para obtener nuevos tokens de acceso.
- **Ejemplo**
```HTP
GET /protected/resource HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```