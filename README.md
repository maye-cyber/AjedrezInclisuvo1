# S'kaqe

Aplicación móvil de **ajedrez inclusivo** para personas con discapacidad visual.

El producto es una app Flutter (Android / iOS) que habla con una API Express. El tablero, Stockfish y el TTS viven en el dispositivo. El backend guarda cuentas, partidas, aprendizaje y estadísticas.

**Backend de producción (Render):** `https://skaqe-api.onrender.com`

El plan Free puede dormir el servicio; el primer request tarda más. El API local `http://localhost:3001` sigue siendo solo para desarrollo.

## Tecnologías

- Flutter / Dart
- Node.js / Express / TypeScript
- PostgreSQL (Prisma 6.16.3)
- Stockfish (en el dispositivo)
- Text-to-Speech (`flutter_tts`)
- Autenticación JWT propia (no Supabase Auth)

## Características

- Partidas de ajedrez con reglas reales
- Oponente Stockfish en el dispositivo
- Aprendizaje, ejercicios y progreso
- Estadísticas para usuarios autenticados
- Narración TTS (bajo demanda)
- Accesibilidad (contraste, tamaño de texto, TalkBack)
- Modo invitado (sin cuenta)
- Registro e inicio de sesión

## Arquitectura

```text
Flutter  -->  Express  -->  Prisma  -->  PostgreSQL
   |
   +-- Stockfish (local)
   +-- TTS (local)
```

Flutter **no** se conecta a la base de datos. No conoce `DATABASE_URL`, `JWT_SECRET` ni `GEMINI_API_KEY`. Esas variables viven solo en `backend/.env` (nunca en Git).

## Requisitos

- Flutter 3.24+ (recomendado 3.44+)
- Node.js 20+
- Android Studio o Xcode según la plataforma
- PostgreSQL remoto (p. ej. Supabase) si quieres autenticación y datos persistidos

Guía paso a paso: [`docs/SETUP.md`](docs/SETUP.md).

## Desarrollo local

### Backend (solo desarrollo)

```bash
cp backend/.env.example backend/.env
cd backend
npm install
npm run dev
```

El API queda en `http://localhost:3001`. Eso **no** es una URL de producción.

Si `GEMINI_API_KEY`, `DATABASE_URL` o `JWT_SECRET` están vacíos, el servidor arranca igual. Se puede jugar como invitado. El entrenador y el registro responden que no están configurados.

### App Flutter

```bash
cd mobile
flutter pub get
flutter run
```

La URL del API se pasa en tiempo de ejecución. **No** se guarda una URL de producción en el código.

Emulador Android (solo desarrollo):

```bash
flutter run --dart-define=API_URL=http://10.0.2.2:3001
```

Teléfono en la misma red (solo desarrollo; sustituye por tu IP local):

```bash
flutter run --dart-define=API_URL=http://192.168.x.x:3001
```

Más adelante, cuando exista un host público, el build de release usará el mismo mecanismo:

```text
flutter build apk --release --dart-define=API_URL=https://URL-REAL
```

Esa URL real **aún no existe** en este proyecto.

## Tests

```bash
cd backend && npm test
cd mobile && flutter test
```

## Estructura

```text
ajedrez-inclusivo/
├── mobile/       # App Flutter
├── backend/      # API Express + Prisma
├── docs/         # Documentación
├── package.json
└── README.md
```

## Seguridad

- Copia `backend/.env.example` → `backend/.env` y rellena valores locales.
- No subas `.env`, keystores, APK ni claves.
- Gemini, JWT y PostgreSQL solo en el backend.
