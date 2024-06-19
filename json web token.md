JSON Web Token (RFC 7519) es una **forma compacta y segura para URL de representar reclamaciones que se transfieren entre dos partes, como un servidor de recursos de Liberty y un proxy de autenticación**.
Un JWT está compuesto por tres partes: el encabezado (header), la carga útil (payload) y la firma (signature).
- **Header**: normalmente incluye el tipo de token y el algoritmo de firma.
- **Payload**: contiene las afirmaciones (claims). Las afirmaciones son declaraciones sobre una entidad (generalmente, el usuario) y datos adicionales.
- **Signature**: se usa para verificar que el mensaje no ha sido cambiado en el camino y, en el caso de los tokens firmados con una clave secreta, también asegura que el remitente del JWT es quien dice ser.
Páginas web como https://jwt.io/ Decodifican el código pasandolo a formato json y viceversa
- - -
**Proceso de autenticación y visualización del token en el navegador:**

1. **Login**: El usuario se autentica a través de una solicitud POST al servidor.
2. **Recepción del JWT**: El servidor responde con un JWT.
3. **Almacenamiento del JWT**: El token puede ser almacenado en las cookies o en el almacenamiento local del navegador.
4. **Envío del JWT**: En cada solicitud subsiguiente, el cliente envía el JWT en el encabezado de autorización.

Para visualizar el JWT en el navegador:

- Abre las herramientas de desarrollador.
- Ve a la pestaña "Network" y encuentra la solicitud de autenticación.
- Busca el encabezado de respuesta que contiene el JWT.
- - -
#### Cookies y JWT

A veces, los JWT se almacenan en cookies. Si este es el caso, puedes encontrar el JWT en la pestaña "Cookies". Los nombres de las cookies pueden variar, pero comúnmente tienen nombres como `token`, `jwt`, `session`, etc.
