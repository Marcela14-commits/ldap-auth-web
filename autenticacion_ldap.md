¿Qué es LDAP?
LDAP (Lightweight Directory Access Protocol) es un protocolo que permite consultar y autenticar usuarios en un directorio centralizado.

¿Cómo funciona la autenticación?

El usuario ingresa usuario y contraseña
La aplicación se conecta al servidor LDAP
Se construye el DN (Distinguished Name)
Se intenta hacer bind (login)
Si el bind es exitoso → acceso permitido

2. Flujo de autenticación


Usuario ingresa credenciales en el formulario
PHP recibe los datos (user y pass)
Se conecta al servidor LDAP (ldap_connect)
Se intenta autenticación (ldap_bind)
Si es correcto:
Login exitoso
Si falla:
Acceso denegado

3. Código PHP (LOGIN LDAP)



<?php
$ldap_host = "ldap://localhost";
$ldap_dn1 = "ou=IT,dc=ejemplo,dc=com";
$ldap_dn2 = "ou=Soporte,dc=ejemplo,dc=com";

$user = $_POST['user'];
$pass = $_POST['pass'];

$ldap_conn = ldap_connect($ldap_host);

if (!$ldap_conn) {
    die("No se pudo conectar a LDAP");
}

ldap_set_option($ldap_conn, LDAP_OPT_PROTOCOL_VERSION, 3);

// Intentar en OU 1
$dn1 = "uid=$user,$ldap_dn1";
$bind1 = @ldap_bind($ldap_conn, $dn1, $pass);

// Intentar en OU 2
$dn2 = "uid=$user,$ldap_dn2";
$bind2 = @ldap_bind($ldap_conn, $dn2, $pass);

if ($bind1 || $bind2) {
    echo "✅ Login exitoso";
} else {
    echo "❌ Usuario o contraseña incorrectos";
}
?>
