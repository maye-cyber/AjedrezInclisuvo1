# RELEASE NOTES — S'kaqe 1.0.0

- **Nombre:** S'kaqe
- **Versión:** 1.0.0
- **Version code:** 1
- **Fecha de build:** 2026-09-02
- **Tipo de build:** release
- **APK:** `mobile/build/app/outputs/flutter-apk/app-release.apk`
- **Tamaño:** 376.1 MB (394 405 414 bytes)
- **SHA-256:** `86BD94530F363233D54ABAB2E59C100C9A4181A8BE21734CF513A30ADD9208F6`
- **applicationId:** `com.ajedrezinclusivo.ajedrez_inclusivo`
- **Backend:** `https://skaqe-api.onrender.com`

## Funcionalidades incluidas

- Bienvenida en la primera instalación
- Registro, login, logout y modo invitado
- Partidas contra Stockfish en el dispositivo
- Aprendizaje, ejercicios y progreso local
- Sincronización de progreso con cuenta
- Estadísticas de partidas autenticadas
- TTS (escuchar, detener, repetir)
- Accesibilidad: contraste, tamaño de texto, Semantics

## Requisitos

- Android (arm64-v8a, armeabi-v7a o x86_64)
- Internet para login, partidas remotas, catálogo de lecciones, estadísticas y sincronización
- Permisos: Internet y micrófono (comandos de voz)

## Backend requerido

API Express en Render: `https://skaqe-api.onrender.com`

Comprobación: `GET /api/health` → `{"status":"ok","service":"ajedrez-inclusivo-api"}`

El plan Free de Render puede dormir el servicio. El primer acceso tras un rato de inactividad tarda más.

## Limitaciones conocidas

- La APK de esta entrega está firmada con la clave **debug** (instalable; no es la firma de Play Store).
- El catálogo de aprendizaje, el historial de partidas y las estadísticas necesitan el API.
- El progreso y los ejercicios locales funcionan sin red.
- Stockfish y el TTS van en el dispositivo.
- No se afirma que toda la app sea offline.
- Gemini solo está en el backend; la APK no incluye `GEMINI_API_KEY` ni `JWT_SECRET`.

## Tests (automatizados, 2026-09-02)

- Flutter: 323 passed
- Backend: 82 passed
- `dart analyze`: exit 0 (3 infos, sin errores)
- `npm run lint`: OK
- `npm run build`: OK

## Pruebas físicas

PENDIENTE — requieren instalación y ejecución manual en un dispositivo Android.

## Cómo se generó esta APK

```bash
cd mobile
flutter clean
flutter pub get
flutter build apk --release --dart-define=API_URL=https://skaqe-api.onrender.com
```
