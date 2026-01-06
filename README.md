

<pre>
Axel AES Encrypt Text.bat
@echo off
powershell -ExecutionPolicy Bypass -File "%~dp0cifrador.ps1"

cifrador.ps1

# ========================================================
# Axel AES Encrypt Text — AES Text Encryption Tool
# Autor: Axel N. Inca
# Creado: Lunes 5 de enero, 2026
# Plataforma: Windows 7+
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
