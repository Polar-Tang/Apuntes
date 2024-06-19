### Prevención

Para prevenir este tipo de vulnerabilidades, debes usar consultas preparadas y parametrizadas. Aquí tienes cómo podrías hacerlo en PHP con MySQLi:

```php
// Conexión a la base de datos
$servername = "localhost";
$username = "your_db_username";
$password = "your_db_password";
$dbname = "your_db_name";

$conn = new mysqli($servername, $username, $password, $dbname);

// Verificar la conexión
if ($conn->connect_error) {
    die("Conexión fallida: " . $conn->connect_error);
}

// Consulta preparada
$stmt = $conn->prepare("SELECT * FROM users WHERE name = ? AND password = ? LIMIT 0,1");

// Variables de entrada
$name = $_POST['name'];
$password = $_POST['password'];

// Asociar parámetros y ejecutar la consulta
$stmt->bind_param("ss", $name, $password);
$stmt->execute();

// Obtener resultados
$result = $stmt->get_result();
if ($result->num_rows > 0) {
    // Usuario autenticado
    echo "Usuario autenticado";
} else {
    // Autenticación fallida
    echo "Nombre de usuario o contraseña incorrectos";
}

// Cerrar la declaración y la conexión
$stmt->close();
$conn->close();

```
### Ventajas de la Consulta Preparada

1. **Protección contra Inyecciones SQL**: Los parámetros se manejan de manera segura, asegurando que el valor de los parámetros se trate como datos y no como parte de la consulta SQL.
2. **Separación de Datos y Código**: El código SQL está separado de los datos proporcionados por el usuario, lo que mejora la seguridad y la legibilidad del código.
3. **Reusabilidad y Eficiencia**: La misma consulta preparada puede ser ejecutada múltiples veces con diferentes parámetros, mejorando el rendimiento.