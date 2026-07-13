# Aquí va el APK de Mi Apunte

Los botones "Descargar APK" de `/miapunte/` y `/miapunte/en/` apuntan a:

```
/miapunte/download/MiApunte-1.0.0.apk
```

**Hasta que subas el archivo con ese nombre exacto, ese botón da 404.**

## Cómo publicar una versión

1. Compila el release firmado:
   ```powershell
   $env:MIAPUNTE_KEYSTORE_PASS = "tu-password"
   .\build-release.ps1
   ```
2. Copia `com.miapunte.app-Signed.apk` a esta carpeta y renómbralo a `MiApunte-1.0.0.apk`.
3. Commit y push. GitHub Pages lo sirve directo — no hace falta nada más.

## Al sacar una versión nueva

Sube el `versionCode` (`<ApplicationVersion>` en `MiApunte.csproj`) **y firma con el mismo
keystore de siempre**. Si la firma cambia, Android rechaza la instalación y tu usuaria
tendría que desinstalar — lo que le borraría todos sus datos.

Luego, o bien:

- **Reemplazas el archivo** manteniendo el nombre `MiApunte-1.0.0.apk` (los links siguen
  funcionando solos, pero el nombre queda mintiendo), **o**
- **Subes `MiApunte-1.1.0.apk`** y actualizas el `href` en los dos `index.html`
  (`/miapunte/index.html` y `/miapunte/en/index.html`) — es una línea en cada uno.

La segunda opción es la buena: deja el historial de versiones disponible y el nombre no miente.

## Límites

GitHub tiene un tope de **100 MB por archivo**. El APK ronda los 43 MB, así que entra sin
problema. El `.aab` (para Play Store) **no** va aquí: ese se sube a Play Console y no sirve
para instalación directa.
