# Windows 7 Explorer Mobile

Explorador de archivos Android nativo (Kotlin + Views XML) con la estructura del Explorador de Windows 7:
panel lateral (Favoritos / Bibliotecas / Equipo / Red) y panel principal **siempre visibles a la vez**.
minSdk 29 (Android 10). Dependencias: AppCompat, RecyclerView, lifecycle, corrutinas. Sin Material ni Compose.

## Compilar el APK
- **Android Studio**: abrir la carpeta, esperar el Sync y `Build > Build APK(s)`.
- **GitHub Actions**: subir el proyecto a un repositorio; `.github/workflows/build.yml` compila y publica el
  APK como artefacto (`Win7ExplorerMobile-apk`).
- **Línea de comandos** (Gradle 8.7 + JDK 17 + Android SDK 34): `gradle :app:assembleRelease`
  → `app/build/outputs/apk/release/`. (Firmado con la clave de depuración, apto para instalar a mano.)
Si falta `gradlew`: `gradle wrapper --gradle-version 8.7`.

## Estructura
- `core/`: modelos, historial, ordenamiento, formatos y operaciones de archivos (copiar, mover, borrar...).
- `data/`: repositorio de archivos real (java.io + MediaStore), volúmenes, permisos, caché de miniaturas.
- `ui/`: `MainActivity` (solo enlaza vistas), `ExplorerViewModel`, árbol lateral, lista, barra de direcciones.
- `aero/`: utilidades de medidas; los drawables Aero están en `res/drawable/aero_*.xml`.
- `settings/`: preferencias (orden, ancho de paneles, ocultos, carpeta Escritorio).

## Estado por fases
| Fase | Estado |
|------|--------|
| 1 Análisis y arquitectura | Hecha |
| 2 Interfaz con dos paneles | Escrita (sin probar en dispositivo) |
| 3 Componentes Aero e iconografía | Aero con gradientes XML; **iconos provisionales** |
| 4 Acceso real al almacenamiento | Escrita |
| 5 Operaciones de archivos | Escritas |
| 6 Optimización | Aplicada en diseño (E/S en IO, DiffUtil, miniaturas con LRU); sin medir |
| 7 Pruebas en Android 10+ | **Pendiente** (requiere dispositivo) |
| 8 APK | **Pendiente** de compilar (ver arriba) |

## Limitaciones conocidas
- Android/data y Android/obb no son accesibles en Android 11+.
- No hay Papelera de reciclaje (Android no la tiene): eliminar es permanente, con confirmación.
- "Red": nodo presente, sin SMB (fase posterior, requiere jcifs-ng).
- Los menús emergentes y diálogos usan el estilo AppCompat por defecto, no el de Windows 7.
- Tipografía: `sans-serif` del sistema (Segoe UI no es redistribuible).
- El panel lateral se ajusta arrastrando el divisor (25–50 %), pero nunca desaparece.
