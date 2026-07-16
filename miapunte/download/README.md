# El APK de Mi Apunte ahora se publica por GitHub Releases

Los botones "Descargar APK" de `/miapunte/` y `/miapunte/en/` apuntan a la URL estable:

```
https://github.com/sgermosen/sgermosen.github.io/releases/latest/download/MiApunte.apk
```

**Ya no se sube ningún APK a esta carpeta.** Cada release nuevo actualiza la descarga sin
tocar el HTML.

## Cómo publicar una versión (automático)

El pipeline vive en el repo privado `sgermosen/TextToImage`
(`.github/workflows/release-apk.yml`):

1. En `MiApunte.csproj`, sube `<ApplicationVersion>` (versionCode) y
   `<ApplicationDisplayVersion>`. **Si no subes el versionCode, Android rechaza la
   actualización.**
2. Commit + push.
3. Taggea: `git tag v1.0.1 && git push origin v1.0.1`

El workflow compila el APK firmado y crea el release `miapunte-v1.0.1` **en este repo** con
el asset `MiApunte.apk`. El `.aab` (para Play Console) queda como artifact privado del run.

## Reglas que no cambian

- **Firma siempre con el mismo keystore** (`C:\dev\keys\miapunte.keystore`, respaldado en 2+
  lugares). Si la firma cambia, Android rechaza la instalación y desinstalar borra los datos.
- Los releases de este repo son **solo de Mi Apunte**: la URL `releases/latest` es del repo
  entero, y un release de otro producto rompería el botón de descarga.

## Publicación manual (fallback sin pipeline)

```powershell
$env:MIAPUNTE_KEYSTORE_PASS = "tu-password"
.\build-release.ps1   # en C:\dev\TextToImage
Copy-Item src\MiApunte\bin\Release\net10.0-android\publish\com.miapunte.app-Signed.apk MiApunte.apk
gh release create miapunte-v1.0.1 MiApunte.apk --repo sgermosen/sgermosen.github.io --title "Mi Apunte 1.0.1" --latest
```
