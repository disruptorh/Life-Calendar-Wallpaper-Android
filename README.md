# Life Calendar 📅

> **Visualiza tu vida entera en semanas, directamente en el fondo de pantalla**

![Licencia](https://img.shields.io/badge/License-Apache--2.0-yellow.svg)

App Android minimalista de wallpaper con dos modos de visualización:

1. **Life Calendar** — convierte tu vida en una rejilla de puntos, donde cada
   punto es **una semana**.
2. **Year Tracker** — visualiza el año actual como una rejilla de días, y los
   días que ya han pasado se van apagando.

---

## ✨ Features

- **Dos modos:**
  - **Life Calendar:** semanas vividas (rellenas) vs. semanas restantes (huecas).
  - **Year Tracker:** puntos verde menta para los días que quedan, negros para
    los que ya pasaron.
- **Auto-actualización diaria:** el wallpaper se regenera a medianoche para que
  el contador nunca vaya atrasado.
- **Configurable:**
  - **Expectativa de vida:** de 50 a 120 años.
  - **Posición del contador de días:** arriba o abajo (Year Tracker).
- **Multilingüe:** interfaz en inglés y español (`values-es`), con pantalla de
  selección de idioma en el primer arranque.
- **Diseño minimalista:** fondo negro puro, pensado para pantallas AMOLED.
- **Ahorro de batería:** una sola actualización al día, programada con
  `AlarmManager.setExactAndAllowWhileIdle`.

---

## 📱 Modos

### 1. Life Calendar
- Rejilla donde **cada fila = 1 año** (52 semanas)
- **Puntos rellenos** = semanas que ya has vivido
- **Puntos huecos** = semanas que te quedan
- Marcadores de año cada 10 años

### 2. Year Tracker
- Rejilla donde **cada punto = 1 día** del año actual
- **Puntos verde menta** = días que quedan
- **Puntos negros** = días que ya pasaron ("perdidos para siempre")
- Contador de días restantes (posición conmutable arriba/abajo)

---

## 📐 Permisos

| Permiso | Por qué |
|---|---|
| `SET_WALLPAPER` | Escribir el fondo de pantalla generado |
| `SCHEDULE_EXACT_ALARM`, `USE_EXACT_ALARM` | Disparar la regeneración exacta a medianoche |

`USE_EXACT_ALARM` exige que la app sea de tipo reloj/calendario en Google Play;
para uso directo por sideload basta con conceder `SCHEDULE_EXACT_ALARM`.

---

## 🛠️ Arquitectura

```
app/src/main/java/com/lifecalendar/
  ├─ LanguageSelectionActivity.kt  # Selector de idioma (primer arranque)
  ├─ MainActivity.kt               # Ajustes: fecha de nacimiento, expectativa, modo
  ├─ WallpaperGenerator.kt         # Dibuja la rejilla de semanas → Bitmap
  ├─ YearTrackerGenerator.kt       # Dibuja la rejilla de días → Bitmap
  ├─ MidnightWallpaperReceiver.kt  # BroadcastReceiver + AlarmManager a las 00:00
  ├─ WallpaperWorker.kt            # WorkManager: regenera fuera del hilo principal
  └─ PreferencesManager.kt         # SharedPreferences (fecha, años, posición)
```

UI clásica con **ViewBinding** (no Compose). Los dos generadores producen un
`Bitmap` y lo aplican vía `WallpaperManager`.

---

## 🔨 Build

- Android SDK 26+ (`minSdk 26`), `targetSdk 34`, `compileSdk 35`
- **JDK 17** (obligatorio: `sourceCompatibility = VERSION_17`)
- Gradle wrapper, Kotlin + Material 3, `androidx.work:work-runtime-ktx:2.9.0`

```bash
git clone https://github.com/disruptorh/Life-Calendar-Wallpaper-Android.git
cd Life-Calendar-Wallpaper-Android

./gradlew assembleDebug      # app/build/outputs/apk/debug/app-debug.apk
./gradlew assembleRelease    # app/build/outputs/apk/release/
```

### Firma de la release

El build de release **no está minificado** (`isMinifyEnabled = false`) y solo se
firma si le pasas un fichero de keystore por propiedad de Gradle:

```bash
./gradlew assembleRelease -PkeystoreProperties=/ruta/a/keystore.properties
```

Sin esa propiedad sale un `app-release-unsigned.apk`. El fichero necesita
`storeFile`, `storePassword`, `keyAlias` y `keyPassword`; el `storeFile` puede
ser absoluto o relativo al directorio del propio `.properties`.

---

## 📥 Instalación

1. Descarga el APK (releases o compílalo tú).
2. Si lo instalas a mano: **Ajustes → Seguridad → Instalar apps desconocidas**,
   y permite tu navegador o gestor de archivos.
3. Abre el APK e instálalo.
4. Arranca Life Calendar, elige idioma y fecha de nacimiento.

---

## 💡 Inspiration

Inspired by the concept of "Your Life in Weeks" by Tim Urban ([Wait But Why](https://waitbutwhy.com/2014/05/life-weeks.html)), which visualizes a human life as a grid of 52 × 90 weeks — a powerful reminder of time's passage.

---

## Licencia

Apache-2.0 — ver [LICENSE](LICENSE).
