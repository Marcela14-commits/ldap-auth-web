1.Explicación de autenticación LDAP

LDAP (Lightweight Directory Access Protocol) es un protocolo utilizado para acceder a servicios de directorio, permitiendo la gestión centralizada de usuarios.

La autenticación LDAP permite validar credenciales (usuario y contraseña) contra un servidor centralizado.

Funcionamiento:
El usuario ingresa sus credenciales
La aplicación se conecta al servidor LDAP
Se busca el usuario en el directorio
Se intenta autenticar (bind)
Si es correcto → acceso permitido


2. Flujo de autenticación
El usuario ingresa usuario y contraseña en el formulario web
PHP recibe los datos mediante método POST
Se establece conexión con el servidor LDAP
Se busca el usuario en el directorio
Se obtiene el DN (Distinguished Name)
Se intenta autenticación con ldap_bind
Resultado:
✅ Acceso permitido
❌ Acceso denegado

 3. Código PHP
📄 index.php
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Login LDAP - FitPower</title>
</head>
<body>

    <h2>Login LDAP - FitPower</h2>

    <form action="./login.php" method="POST">
        
        <label>Usuario:</label><br>
        <input type="text" name="user" required><br><br>

        <label>Contraseña:</label><br>
        <input type="password" name="pass" required><br><br>

        <button type="submit">Ingresar</button>

    </form>

</body>
</html>
📄 login.php
<?php
$ldap_host = "ldap://localhost";
$base_dn = "dc=fitpower,dc=local";

$user = $_POST['user'];
$pass = $_POST['pass'];

$conn = ldap_connect($ldap_host);
ldap_set_option($conn, LDAP_OPT_PROTOCOL_VERSION, 3);

// Buscar usuario en LDAP
$search = ldap_search($conn, $base_dn, "(uid=$user)");
$entries = ldap_get_entries($conn, $search);

if ($entries["count"] > 0) {
    $dn = $entries[0]["dn"];

    // Intentar autenticación
    if (@ldap_bind($conn, $dn, $pass)) {
        echo "✅ Login exitoso<br>";
        echo "Usuario autenticado: " . $dn;
    } else {
        echo "❌ Contraseña incorrecta";
    }
} else {
    echo "❌ Usuario no existe";
}
?>


4. EVIDENCIAS

<img width="488" height="230" alt="Login Exitoso" src="https://github.com/user-attachments/assets/a7330def-adc2-445a-8854-eaca46dcaef1" />
<img width="355" height="251" alt="Longin con ituser" src="https://github.com/user-attachments/assets/e7ed98d4-7885-4a64-805b-69b647e8fb7f" />
<img width="441" height="164" alt="Longin exitoso con ituser" src="https://github.com/user-attachments/assets/4ea06a7c-a0d4-40c5-8b22-a80182ab99f4" />
<img width="549" height="314" alt="Login Formulario" src="https://github.com/user-attachments/assets/f912fd0e-2fde-49a5-8dbe-38bef9b2a225" />
<img width="487" height="167" alt="Ejemplo de login fallido" src="https://github.com/user-attachments/assets/11d85ebf-51fe-4fb7-adab-a84c257527fb" />














