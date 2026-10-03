# Life Calendar 📅

App Android que **convierte tu vida en una rejilla de puntos y la pone de fondo
de pantalla**: cada punto es una semana (Life Calendar) o un día del año (Year
Tracker). Kotlin con layouts XML, `minSdk 26` / `targetSdk 34`, interfaz en
inglés y español.

<p align="center">
  <a href="https://github.com/disruptorh/Life-Calendar-Wallpaper-Android/releases/latest/download/Life-calendar-android-wallpaper.apk">
    <img alt="Descargar" src="https://img.shields.io/badge/%E2%AC%87%20Download-latest%20release-2f6feb?style=for-the-badge&logo=github&logoColor=white">
  </a>
  <a href="https://github.com/disruptorh/Life-Calendar-Wallpaper-Android/releases/latest">
    <img alt="Versiones" src="https://img.shields.io/github/v/release/disruptorh/Life-Calendar-Wallpaper-Android?label=release&style=flat&logo=github&logoColor=white">
  </a>
  <a href="./LICENSE">
    <img alt="Licencia" src="https://img.shields.io/badge/licencia-Apache--2.0-blue?style=flat">
  </a>
</p>

## 📥 Descarga rápida

El botón de arriba descarga el APK **ya firmado** de la última release
(`Life-calendar-android-wallpaper.apk`). Necesitas Android 8.0 (API 26) o
superior.

Para instalarlo a mano: abre el APK descargado y, si Android lo pide, activa
**Ajustes → Apps → Acceso especial → Instalar apps desconocidas** para la app
que lo abre (navegador o gestor de archivos).

Desde un ordenador con el móvil por cable o ADB Wi-Fi:

```bash
# 1. Descargar la última release publicada
curl -L -o Life-calendar-android-wallpaper.apk https://github.com/disruptorh/Life-Calendar-Wallpaper-Android/releases/latest/download/Life-calendar-android-wallpaper.apk

# 2. Instalar (o reinstalar) en el dispositivo conectado
adb install -r Life-calendar-android-wallpaper.apk
```

## 🚀 Uso rápido

1. Abre **Life Calendar**. En el primer arranque aparece el selector de idioma
   (English / Español); después ya no se vuelve a mostrar.
2. Elige el tipo de wallpaper en el conmutador superior: **Life Calendar** o
   **Year Tracker**.
3. En **Life Calendar**, pulsa **Select Birthdate** y elige tu fecha de
   nacimiento (el selector no deja elegir fechas futuras), y ajusta la
   expectativa de vida con el deslizador, de 50 a 120 años (por defecto 80).
4. Activa **Daily Auto-Update** si quieres que el fondo se regenere solo cada
   noche.
5. Pulsa **Set Wallpaper Now**. La app dibuja el fondo y lo aplica
   inmediatamente; el sistema no abre ningún selector.

### Los dos modos

**Life Calendar.** Rejilla donde **cada fila es un año (52 semanas)** y cada
punto es una semana vivida. Los puntos ya vividos son blancos y los que te
quedan, grises. Lleva el título *LIFE CALENDAR*, las etiquetas *WEEK OF THE
YEAR* y *YEAR OF YOUR LIFE* (esta última rotada) y marcadores de año cada 10
años. Deja libre el 38% superior de la pantalla para el reloj. En la app te
muestra también las semanas vividas, las que te quedan y el porcentaje.

**Year Tracker.** Rejilla de **15 columnas** donde **cada punto es un día** del
año en curso (365, o 366 si es bisiesto): los días ya pasados en blanco, el día
actual en rojo y los que quedan en gris. Debajo, el porcentaje del año, una
barra de progreso roja y el contador de días restantes.

En la vista de **Year Tracker** aparece el interruptor **Days Counter
Position**: guarda tu preferencia (arriba o abajo) en las preferencias de la app,
pero el generador todavía la ignora al dibujar, así que el contador sale
siempre abajo. Es un ajuste reservado.

El diseño es fondo negro puro con puntos grises y blancos, pensado para
pantallas AMOLED.

## 📦 Compilar desde código

La raíz del repositorio **es** el proyecto Gradle: no hay subdirectorio
`android/`. Todos los comandos de esta sección se ejecutan desde ahí.

### Requisitos

- **JDK 17** (obligatorio: `sourceCompatibility` y `jvmTarget` son 17).
- **Android SDK** con la plataforma `android-35` instalada (o Android Studio,
  que la gestiona por ti).
- Nada más: el **Gradle Wrapper 8.9** va incluido, así que no hace falta
  instalar Gradle. Usa siempre `./gradlew`, no un Gradle del sistema.

### Clonar

```bash
# 1. Clonar el repositorio
git clone https://github.com/disruptorh/Life-Calendar-Wallpaper-Android.git
cd Life-Calendar-Wallpaper-Android
```

### Dependencias

Gradle necesita saber dónde está tu SDK de Android. Eso se guarda en
`local.properties`, en la raíz del repo, con una única línea `sdk.dir=`.

> **Aviso:** `local.properties` está en `.gitignore`, pero si copiaste el repo
> desde otra máquina puede venir con la ruta del SDK de esa otra máquina, que en
> tu equipo no existe. Si te aparece un error tipo "SDK location not found",
> sobrescribe el fichero con el bloque de abajo.

```bash
# 2. Crear local.properties apuntando a tu SDK de Android
printf 'sdk.dir=%s\n' "$HOME/Android/Sdk" > local.properties
```

Si tu SDK está en otro sitio, cambia la ruta: en macOS suele ser
`$HOME/Library/Android/sdk` y en Windows
`C:\Users\<tu-usuario>\AppData\Local\Android\Sdk`. Comprueba cuál es con
`ls $HOME/Android/Sdk` o, si usas Android Studio, con **Settings → Languages &
frameworks → Android SDK → SDK location**.

El build descarga las dependencias de `google()` y `mavenCentral()` la primera
vez, así que hace falta conexión a internet en el primer `./gradlew`.

### Compilar

```bash
# 3. Compilar el APK de depuración (va firmado con el keystore de debug que crea el SDK)
./gradlew :app:assembleDebug
```

Sale en `app/build/outputs/apk/debug/app-debug.apk`. Se instala en un
dispositivo conectado, sin configuración adicional:

```bash
# 4. Instalar el APK de depuración en el dispositivo conectado
./gradlew :app:installDebug
```

O a mano, con la ruta exacta del fichero:

```bash
adb install -r app/build/outputs/apk/debug/app-debug.apk
```

El APK de **release** (sin minificar) necesita un keystore propio: mira
[Firma del APK](#firma-del-apk).

```bash
# 5. Compilar el APK de release (sin firmar si no le pasas un keystore)
./gradlew :app:assembleRelease
```

### Tests

Este proyecto **no tiene tests unitarios**: no existe `app/src/test`. Solo hay
un módulo instrumentado por defecto, que no se usa:

```bash
./gradlew :app:testDebugUnitTest
```

La tarea existe y no falla, pero no ejecuta nada. El análisis estático sí es
útil:

```bash
./gradlew :app:lintDebug
```

### Ejecutar la aplicación

Con Android Studio: abre la carpeta del repo y pulsa **Run** sobre la
configuración `app`. Sin Android Studio, `installDebug` (o el `adb install` de
arriba) la deja instalada y lista para lanzar con un toque en el icono.

## 🧰 Comandos útiles

| Tarea | Comando | Qué hace |
|---|---|---|
| Compilar debug | `./gradlew :app:assembleDebug` | APK de depuración en `app/build/outputs/apk/debug/app-debug.apk` |
| Instalar debug | `./gradlew :app:installDebug` | Instala en el dispositivo conectado |
| Compilar release | `./gradlew :app:assembleRelease` | APK de release, sin firmar |
| Release firmado | `./gradlew :app:assembleRelease -PkeystoreProperties=/ruta/abs/a/tu/keystore.properties` | Ver [Firma del APK](#firma-del-apk) |
| Lint | `./gradlew :app:lintDebug` | Análisis estático |
| Compilar todo | `./gradlew :app:assemble` | Debug y release de una vez |
| Limpiar | `./gradlew clean` | Borra los ficheros generados |
| Ver tareas | `./gradlew :app:tasks --all` | Lista todas las tareas disponibles |

## 🔐 Activar y mantener el wallpaper

Esto **no es un live wallpaper de Android**: el manifiesto no declara ningún
`WallpaperService`. Es un wallpaper estático que la propia app dibuja y aplica
con `WallpaperManager.setBitmap()` sobre la pantalla principal. Lo único que
"se mueve" es el contenido, porque la app lo vuelve a dibujar cada noche.

Cómo se activa:

- **Desde la app:** *Set Wallpaper Now*. Se aplica en el momento, sin
  selectores de por medio. Si está en modo Life Calendar y no has puesto fecha
  de nacimiento, avisa y no hace nada.
- **Después:** como cualquier wallpaper estático, se cambia desde **Ajustes →
  Fondo de pantalla y estilo**, y el de esta app aparece en la lista de
  wallpapers del sistema. No aparece en el selector de *wallpapers en vivo*,
  porque no lo es.

Cómo se mantiene al día:

- Activa **Daily Auto-Update**. La app programa una alarma exacta para las
  00:00:05 del día siguiente con `AlarmManager.setExactAndAllowWhileIdle()`; al
  dispararse, `MidnightWallpaperReceiver` vuelve a dibujar el wallpaper según el
  modo guardado y se reprograma para la siguiente medianoche.
- Si no lo activas, el fondo se queda congelado en el momento en que lo pusiste.
  Vuelve a pulsar *Set Wallpaper Now* cuando quieras actualizarlo.

Permisos que usa el manifiesto:

| Permiso | Por qué |
|---|---|
| `SET_WALLPAPER` | Escribir el fondo generado con `WallpaperManager` |
| `SCHEDULE_EXACT_ALARM` | Disparar la regeneración exacta a medianoche |
| `USE_EXACT_ALARM` | Declarado junto al anterior; Google Play solo lo concede a apps de tipo reloj o calendario. Para uso directo por sideload basta con `SCHEDULE_EXACT_ALARM` |

Si la actualización de medianoche no te salta, mira en **Ajustes → Apps → Life
Calendar → Alarmas y recordatorios** si el acceso a alarmas exactas está
concedido: sin él, Android puede aplicar la alarma.

## 🛠️ Arquitectura

UI clásica con **ViewBinding** (no Compose). Los dos generadores producen un
`Bitmap` del tamaño real de la pantalla y lo aplican vía `WallpaperManager`:

```text
app/src/main/java/com/lifecalendar/
├── LanguageSelectionActivity.kt  # selector de idioma, solo en el primer arranque
├── MainActivity.kt               # ajustes: tipo, fecha, expectativa, posición, auto-update
├── WallpaperGenerator.kt         # rejilla de semanas → Bitmap
├── YearTrackerGenerator.kt       # rejilla de días → Bitmap
├── MidnightWallpaperReceiver.kt  # BroadcastReceiver + AlarmManager a las 00:00
├── WallpaperWorker.kt            # Worker de WorkManager con la misma lógica
└── PreferencesManager.kt         # SharedPreferences (idioma, fecha, años, posición)
```

- `MainActivity` decide qué generador usar, guarda la preferencia del modo y es
  quien programa y cancela la alarma de medianoche.
- `MidnightWallpaperReceiver` es quien refresca de verdad a diario, y se
  reprograma a sí mismo tras cada disparo, así que basta con activarlo una vez.
- `WallpaperWorker` contiene esa misma lógica como `Worker` de WorkManager, pero
  **ningún código lo encola**: la actualización real va por `AlarmManager`, no
  por WorkManager. La dependencia `androidx.work:work-runtime-ktx` está
  declarada y el worker compila, pero está sin usar.
- El idioma se aplica en `attachBaseContext()` leyendo la preferencia antes de
  inflar la vista, y `MainActivity` redirige a `LanguageSelectionActivity` en el
  primer arranque.
- Toda la configuración vive en un `SharedPreferences` (`life_calendar_prefs`):
  fecha de nacimiento en ISO, expectativa, tipo de wallpaper, posición del
  contador, auto-update, idioma y marca de primer arranque. Las semanas vividas
  se calculan siempre como `días transcurridos / 7`, sin fecha fija de
  referencia.

Dependencias: AndroidX Core, AppCompat, Material, ConstraintLayout y
`androidx.work:work-runtime-ktx:2.9.0`.

## 🔏 Firma del APK

A diferencia de los otros proyectos, aquí **no hay ninguna configuración de
firma por defecto**. `app/build.gradle.kts` solo crea la configuración de firma
si le pasas la propiedad de Gradle `keystoreProperties` apuntando a un fichero.

**Qué pasa si no le pasas nada:** `./gradlew :app:assembleRelease` termina sin
errores, pero produce `app/build/outputs/apk/release/app-release-unsigned.apk`.
Android no lo puede instalar: un APK sin firma se rechaza. Para instalar y
probar, compila `assembleDebug`, que va firmado con el keystore de depuración
que genera el propio SDK de Android.

Para firmar el release tienes que aportar tu propio keystore. Estos bloques
usan valores **de prueba** (`clave-local-de-pruebas` / `mi-alias`) que
funcionan tal cual al pegar; cámbialos por los tuyos si prefieres.

```bash
# 1. Crear un keystore local de pruebas (el del release publicado no está en el repo)
rm -f mi-keystore.jks
keytool -genkeypair -v -keystore mi-keystore.jks -alias mi-alias -keyalg RSA -keysize 2048 -validity 10000 -storepass 'clave-local-de-pruebas' -keypass 'clave-local-de-pruebas' -dname "CN=Pruebas locales, C=ES"

# 2. Crear el fichero de credenciales al lado del keystore
printf 'storeFile=mi-keystore.jks\nstorePassword=clave-local-de-pruebas\nkeyAlias=mi-alias\nkeyPassword=clave-local-de-pruebas\n' > mi-keystore.properties

# 3. Compilar el release firmado pasando la ruta ABSOLUTA del .properties
./gradlew :app:assembleRelease -PkeystoreProperties="$PWD/mi-keystore.properties"
```

Qué significa cada propiedad:

- `storeFile` — ruta del keystore. Puede ser absoluta; si es relativa, se
  busca primero junto al propio fichero `.properties` y después en un
  subdirectorio `app/` de su carpeta.
- `storePassword` — contraseña del almacén del keystore.
- `keyAlias` — alias de la clave dentro del keystore (el que le diste a
  `keytool -alias`).
- `keyPassword` — contraseña de esa clave.

El fichero de credenciales y el keystore están en `.gitignore`: **no los subas
nunca a Git**.

Con tu keystore, el resultado es
`app/build/outputs/apk/release/app-release.apk` (firmado). Ojo con esto: tu
firma es distinta de la del APK publicado en releases, así que Android no te
deja instalarlo encima. Desinstala la versión anterior primero:

```bash
adb uninstall com.lifecalendar
adb install -r app/build/outputs/apk/release/app-release.apk
```

## 🗂️ Estructura del proyecto

```text
.
├── app/
│   ├── build.gradle.kts              # SDK, firma por -PkeystoreProperties, ViewBinding
│   ├── proguard-rules.pro            # reglas R8 (el release no minifica)
│   └── src/main/
│       ├── AndroidManifest.xml        # SET_WALLPAPER y alarmas exactas
│       ├── java/com/lifecalendar/    # las 7 clases Kotlin, ver arriba
│       └── res/
│           ├── drawable/              # icono y progress_bar
│           ├── layout/                # activity_main, activity_language_selection
│           ├── mipmap-anydpi-v26/     # iconos adaptativos
│           ├── values/                # colores, strings, tema (inglés)
│           └── values-es/strings.xml  # traducción al español
├── build.gradle.kts                  # versiones de AGP 8.7.3 y Kotlin 2.0.21
├── settings.gradle.kts               # repositorios google() + mavenCentral(); módulo :app
├── gradle.properties                 # AndroidX, 2 GB de heap para Gradle
├── gradle/wrapper/                   # Gradle Wrapper 8.9 (incluido)
├── gradlew                           # siempre ./gradlew
├── local.properties                  # NO se versiona: la ruta de tu SDK (sdk.dir=)
├── LICENSE                           # Apache-2.0
└── README.md
```

## 💡 Inspiración

El concepto viene de **"Your Life in Weeks"** de Tim Urban
([Wait But Why](https://waitbutwhy.com/2014/05/life-weeks.html)), que dibuja una
vida humana como una rejilla de 52 × 90 semanas.

## 📄 Licencia

Apache-2.0 — ver [`LICENSE`](./LICENSE).