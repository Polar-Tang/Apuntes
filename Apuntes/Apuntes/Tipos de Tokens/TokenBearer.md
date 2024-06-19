Es un tipo de token de acceso que se utiliza comúnmente en autenticación y autorización de APIs. Un Bearer Token otorga acceso al portador a los recursos protegidos por el servidor. No requiere autenticación adicional; el servidor asume que cualquier solicitud con un token válido es legítima. Uso: Se envía en el encabezado de la solicitud HTTP como Authorization: Bearer token.  

**Ejemplo**
```HTP
GET /protected/resource HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```


**Bearer Token vs API Key:**
    
 **Seguridad:** Los Bearer Tokens son más seguros ya que pueden expirar y contener información sobre el usuario y sus permisos. Las API Keys son estáticas y no suelen expirar.
     **Contexto de uso:** Los Bearer Tokens se usan en autenticación de usuarios y control de acceso, mientras que las API Keys se usan principalmente para identificar y controlar el acceso a la API.
 **Bearer Token vs OAuth Token:**
    - **Flujo de obtención:** Los Bearer Tokens pueden ser parte de OAuth, obtenidos después de un flujo de autenticación. Los OAuth Tokens incluyen tanto el Bearer Token (access token) como el Refresh Token.
    - **Renovación:** Los OAuth Tokens incluyen mecanismos para renovar tokens de acceso (usando Refresh Tokens).