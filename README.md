    Historia

Desde muy joven sentí curiosidad por el cifrado de textos. Alrededor del año 2012, cuando tenía entre 11 y 12 años, desarrollé por mi cuenta una forma simple de cifrar el abecedario utilizando otros símbolos. Lo hacía para escribir en mis carpetas de la primaria y evitar que mis compañeros, que solían burlarse de mí, pudieran leer lo que escribía. Sin saberlo del todo, ahí nació una idea que nunca me abandonó.

Más adelante, entre 2016 y 2017, cuando tenía 16 y 17 años, solía escribirme con 713114116651091413. Compartíamos mensajes cifrados utilizando un sistema que me enseñó y que, según me contó, usaba en su infancia para comunicarse con su madre mientras hacían bromas para su padre. Esa experiencia reforzó algo que ya venía creciendo en mí: el deseo de tener mi propia herramienta para cifrar textos, algo personal, confiable y hecho a mi manera.

Durante mi adolescencia me atrajo mucho la programación en CMD. Sentía que ese entorno básico me desafiaba a aprender y a crear. Con los conocimientos que tenía en ese momento, lo máximo que logré fue mejorar sistemas muy simples creados por otras personas: programas que tomaban texto desde un archivo .txt y desplazaban las letras, haciendo que la “a” pasara a valer “b”, y así sucesivamente. Eran métodos rudimentarios, sin verdadera seguridad, y nunca alcanzaban el nivel de protección que yo buscaba.

Hoy, con más libertad para investigar y aprender, con documentación extensa y herramientas pensadas específicamente para este tipo de tareas, el panorama es distinto. Aun así, durante mucho tiempo no encontré algo que realmente se ajustara a lo que quería: una herramienta simple, local, que permitiera copiar un texto, introducir una clave personal y obtener un texto cifrado de verdad, sin depender de programas .exe desconocidos ni de sitios web que no inspiran una confianza plena.

Este proyecto fue posible gracias a las herramientas de inteligencia artificial ChatGPT, que me permitió acceder a una documentación prácticamente infinita sobre PowerShell. Mi idea inicial era hacerlo completamente en Batch clásico (CMD), pero llegué a un límite técnico imposible de superar. Aunque intenté combinarlo con llamadas a PowerShell, no lograba lo que buscaba. Finalmente tuve que ceder y trabajar directamente sobre PowerShell, algo que no me gusta demasiado. Por esa razón, el programa incluye un lanzador en .bat, que permite ejecutar el script y visualizarlo de una forma más cercana al CMD clásico, que es el entorno que realmente me apasiona.

Mi historia está muy marcada por la programación en sistemas básicos. Muchas veces llevé esos límites al extremo y, como último recurso, tuve que aprender más sobre PowerShell para poder avanzar. La interfaz gráfica del programa no está excesivamente trabajada a propósito: decidí dejar la consola en su estado base, ya que limitar el tamaño de la ventana impediría visualizar y copiar correctamente textos extensos. Preferí priorizar la funcionalidad antes que la estética.

Este sistema de cifrado comenzó a tomar forma la noche del viernes 2 de enero de 2026. Lo desarrollé durante el sábado 3 de enero y perfeccioné su organización esa misma tarde. Esa noche empecé a idear una nueva herramienta, Axel AES Cipher.bat, que programé en la madrugada del domingo 4 de enero y pulí gráficamente durante el día. Sin embargo, sentí que aún no había terminado. Con lo aprendido, en la madrugada del lunes 5 de enero desarrollé Axel AES Encrypt Text y finalicé su diseño gráfico por la mañana. Por la tarde y la noche retomé mis responsabilidades, tanto en mi grupo universitario como en mi taller mecánico.

Agradezco profundamente a todas las personas que alguna vez me acompañaron e impulsaron estas ideas en mí. Lamento no haber podido cumplir sus expectativas cuando todavía creían en mí. Hoy, cuando esas personas ya no están, siento que por fin tengo la fuerza y el conocimiento necesarios, aunque también me siento solo. Como una forma de saldar esa deuda, decido hacer públicos mis programas. Espero que sean útiles para quienes los utilicen y que, si tienen la amabilidad de hacerlo, recuerden que este programa tiene una historia. Y esta es la mía.

    Funcionamiento y explicación detallada
El archivo comienza configurando la apariencia de la consola. Se establece el fondo negro, el texto blanco y se asigna un título a la ventana. Esto no afecta al funcionamiento del cifrado, pero fija un entorno visual consistente antes de que ocurra cualquier interacción. Inmediatamente después se limpia la pantalla para que el usuario nunca vea mensajes previos del sistema o residuos de ejecuciones anteriores.

Luego se define la función DerivarClave. Esta función recibe la contraseña que el usuario escribe y no la usa directamente como clave de cifrado. En su lugar, convierte ese texto en bytes UTF-8 y lo procesa con SHA-256. El resultado es un bloque de 32 bytes que se utiliza como clave criptográfica real. Esto asegura que, independientemente de la longitud o del contenido de la contraseña, la clave final tenga siempre el tamaño correcto para AES-256.

La función NuevoAES crea un objeto AES nuevo cada vez que se necesita cifrar o descifrar. Se configura explícitamente el modo CBC y el relleno PKCS7. Esto define cómo se procesan los bloques de datos y cómo se completa el último bloque si el texto no encaja exactamente en el tamaño requerido por el algoritmo. La función devuelve el objeto ya listo para recibir una clave y un vector de inicialización.

La función EsperarClipboard actúa como un punto de control. El programa se detiene silenciosamente hasta que el usuario presiona ENTER, y recién después lee el contenido completo del portapapeles en modo “raw”. Esto permite pegar textos largos, con saltos de línea, tildes, ñ y símbolos, sin que PowerShell interprete nada ni corte el contenido. Todo lo que el usuario haya copiado entra al programa exactamente como estaba.

Cuando el usuario elige la opción de cifrar, se limpia la pantalla y se muestra una instrucción clara indicando que debe copiar el texto original. Al presionar ENTER, el texto pasa desde el portapapeles a la variable $texto. A partir de ese momento, el contenido ya no depende del portapapeles: queda en memoria dentro del programa.

Luego se limpia nuevamente la pantalla y se solicita la clave. La contraseña que el usuario escribe se guarda en $pass y se envía a la función de derivación, que la transforma en una clave binaria segura. En paralelo, se crea un objeto AES nuevo y se genera un vector de inicialización aleatorio. Ese vector es distinto cada vez, incluso si el texto y la contraseña se repiten.

El texto original se convierte a bytes UTF-8 y se cifra usando la clave derivada y el vector de inicialización. El resultado cifrado se concatena con el vector de inicialización al comienzo, y todo ese conjunto se convierte a una cadena Base64. Esto permite que el texto cifrado sea copiable, pegable y publicable en Blogger sin perder información ni romper caracteres.

Después de cifrar, la pantalla se limpia una vez más y se muestra únicamente el resultado final: el texto cifrado completo y, debajo, la clave que se utilizó. No hay mensajes técnicos ni advertencias. El programa se detiene en silencio hasta que el usuario presiona ENTER, sin mostrar instrucciones adicionales.

Cuando el usuario elige descifrar, el recorrido es similar pero inverso. Primero se limpia la pantalla y se pide copiar el texto cifrado. Al presionar ENTER, ese texto Base64 se lee desde el portapapeles y se almacena en memoria. Luego se solicita la clave, que se deriva de la misma forma que en el cifrado.

El texto cifrado se convierte nuevamente de Base64 a bytes. Los primeros 16 bytes se separan y se usan como vector de inicialización, y el resto se toma como el contenido cifrado real. Con la clave derivada y ese vector, el objeto AES intenta descifrar los datos. Si la clave es correcta, los bytes resultantes se convierten a texto UTF-8, recuperando exactamente el contenido original, con saltos de línea, tildes y ñ intactos.

Finalmente, la pantalla se limpia y se muestra únicamente el texto descifrado, sin encabezados adicionales ni mensajes. El programa vuelve a detenerse en silencio y, al presionar ENTER, retorna al menú principal.

El bucle final mantiene el programa activo indefinidamente. Cada vez que vuelve al menú, la pantalla se limpia y se muestra el título general junto a las dos opciones disponibles. La selección del usuario determina si el flujo de datos entra por la ruta de cifrado o por la de descifrado, pero en ambos casos el proceso es lineal, predecible y aislado: el texto entra desde el portapapeles, se transforma en memoria y el resultado sale nuevamente como texto visible y copiable.

En conjunto, el archivo funciona como una herramienta cerrada y autosuficiente. No escribe archivos, no guarda información en disco y no depende de estados anteriores. Cada ejecución es independiente, y toda la seguridad descansa en la contraseña y en el uso correcto del cifrado AES con clave derivada y vector de inicialización aleatorio.

    Código original de Axel AES Encrypt Text.bat (lanzador) y cifrador.ps1 (código)
Axel AES Encrypt Text.bat
<pre>
@echo off
powershell -ExecutionPolicy Bypass -File "%~dp0cifrador.ps1"
</pre>
cifrador.ps1
<pre>
# ========================================================
# Axel AES Encrypt Text — AES Text Encryption Tool
# Autor: Axel N. Inca
# Creado: Lunes 5 de enero, 2026
# Plataforma: Windows 10
# Desarrollo asistido por ChatGPT (OpenAI)
# Información:
#    -	https://axelincapublico.blogspot.com/2026/01/axel-aes-encrypt-text.html
#    -	https://github.com/userdistortion/Axel-AES-Encrypt-Text
#    -	https://youtu.be/RBlVN9DvbmU
# Mensaje: Por favor comparte tu experiencia con el programa y si te sirvió mucho para tus tareas. Puedes hacerlo en mi blog personal o en el video de youtube que figura en la información.
# ========================================================

$Host.UI.RawUI.BackgroundColor = "Black"
$Host.UI.RawUI.ForegroundColor = "White"
$Host.UI.RawUI.WindowTitle = "Axel AES Encrypt Text"

Clear-Host

function DerivarClave($pass) {
    $sha = [Security.Cryptography.SHA256]::Create()
    return $sha.ComputeHash([Text.Encoding]::UTF8.GetBytes($pass))
}

function NuevoAES {
    $aes = [Security.Cryptography.Aes]::Create()
    $aes.Mode = [Security.Cryptography.CipherMode]::CBC
    $aes.Padding = [Security.Cryptography.PaddingMode]::PKCS7
    return $aes
}

function EsperarClipboard {
    Read-Host | Out-Null
    return Get-Clipboard -Raw
}

function Cifrar {
    Clear-Host
    Write-Host "========================================================================================================================"
    Write-Host "= SELECCIONE TEXTO PARA CIFRAR                                                                                         ="
    Write-Host "========================================================================================================================"
    Write-Host ""
    Write-Host "Copie el texto COMPLETO."
    Write-Host "Luego presione ENTER."
    $texto = EsperarClipboard

    Clear-Host
    Write-Host "========================================================================================================================"
    Write-Host "= REQUERIMIENTO PARA CIFRAR EL TEXTO - INSERTE LA CLAVE                                                                ="
    Write-Host "========================================================================================================================"
    Write-Host ""
    $pass = Read-Host "Escriba la clave de seguridad"

    $aes = NuevoAES
    $aes.Key = DerivarClave $pass
    $aes.GenerateIV()

    $bytes = [Text.Encoding]::UTF8.GetBytes($texto)
    $cifrado = $aes.CreateEncryptor().TransformFinalBlock($bytes,0,$bytes.Length)
    $resultado = [Convert]::ToBase64String($aes.IV + $cifrado)

    Clear-Host
    Write-Host "========================================================================================================================"
    Write-Host "= TEXTO CIFRADO CORRECTAMENTE                                                                                          ="
    Write-Host "========================================================================================================================"
    Write-Host ""
    Write-Host $resultado
    Write-Host ""
    Write-Host "Clave: $pass"

    Read-Host | Out-Null
}

function Descifrar {
    Clear-Host
    Write-Host "========================================================================================================================"
    Write-Host "= SELECCIONE TEXTO PARA DESCIFRAR                                                                                      ="
    Write-Host "========================================================================================================================"
    Write-Host ""
    Write-Host "Copie el texto CIFRADO."
    Write-Host "Luego presione ENTER."
    $texto = EsperarClipboard

    Clear-Host
    Write-Host "========================================================================================================================"
    Write-Host "= REQUERIMIENTO PARA DESCIFRAR EL TEXTO - INSERTE LA CLAVE                                                             ="
    Write-Host "========================================================================================================================"
    Write-Host ""
    $pass = Read-Host "Escriba la clave de seguridad"

    $datos = [Convert]::FromBase64String($texto)
    $aes = NuevoAES
    $aes.Key = DerivarClave $pass
    $aes.IV = $datos[0..15]

    $contenido = $datos[16..($datos.Length-1)]
    $descifrado = $aes.CreateDecryptor().TransformFinalBlock($contenido,0,$contenido.Length)
    $resultado = [Text.Encoding]::UTF8.GetString($descifrado)

    Clear-Host
    Write-Host $resultado

    Read-Host | Out-Null
}

do {
    Clear-Host
    Write-Host "========================================================================================================================"
    Write-Host "= CIFRADOR DE TEXTOS - AES v5                                                                                          ="
    Write-Host "========================================================================================================================"

    Write-Host ""
    Write-Host "1. Cifrar"
    Write-Host "2. Descifrar"
    Write-Host ""

    $op = Read-Host ">"

    switch ($op) {
        "1" { Cifrar }
        "2" { Descifrar }
    }
} while ($true)
</pre>
