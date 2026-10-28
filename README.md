# BookVox

Android app that reads EPUB and PDF books aloud (phone voice or offline Piper voices) and saves the audio as MP3.
App para Android que lee libros EPUB y PDF en voz alta (voz del teléfono o voces Piper sin conexión) y guarda el audio como MP3.

- Author / Autor: José Aracil — bookvox54@gmail.com
- License / Licencia: GPL-3.0 (see [LICENSE](LICENSE)). BookVox includes sherpa-onnx, which bundles eSpeak NG (GPL-3.0).
- Privacy policy / Política de privacidad: https://asap3210.github.io/bookvox-privacy/

## Build
Requires JDK 17 and Gradle 8.11.1 (the project has no wrapper jar):

    gradle assembleDebug

The sherpa-onnx AAR (v1.12.40) is downloaded from its GitHub release at configuration time into `app/libs`.
Signed release builds are made by the GitHub Actions workflow (`.github/workflows/build.yml`) when the signing secrets exist.

---

# BookVox

Lee EPUB en voz alta (TTS nativo de Android) y guarda el audio como MP3. Gratis, sin claves de API, funciona sin internet.

## Compilar
1. Abre la carpeta en Android Studio (Ladybug o más reciente). Acepta la sincronización de Gradle y, si lo pide, la actualización de AGP/Kotlin.
2. Conecta el teléfono (depuración USB) y pulsa Run.

## Para que suene natural
Ajustes > Idioma > Salida de texto a voz > motor "Servicios de voz de Google" > instala la voz neuronal en español.

## Uso
- **Abrir EPUB**, elige capítulo, **Leer**. Sigue sonando con la pantalla apagada (controles en la notificación).
- **Guardar como MP3**: capítulo actual o libro completo. Se guarda en Música/BookVox.

## Estructura
- `EpubParser.kt`: lee el EPUB (ZIP + OPF) sin librerías externas.
- `ReaderService.kt`: lectura en segundo plano, cola de frases sin pausas.
- `Mp3Exporter.kt` / `ExportService.kt`: TTS a WAV y codificación a MP3 con LAME.
- `MainActivity.kt`: interfaz en Compose.

## Notas
- Voz mono a 64 kbps (suficiente para voz). Se cambia en `Mp3Exporter.kt` (`setOutBitrate`).
- Idioma: el del teléfono; si no está disponible usa español (España).

## Versión 2.4
- Foco de audio: se pausa con llamadas u otras apps de audio y reanuda al volver el foco (en pérdidas temporales).
- Temporizador de apagado: 15, 30, 45 o 60 minutos, o al terminar el capítulo.

## Versión 2.3
- Piper sigue generando el capítulo siguiente por adelantado: sin hueco al cambiar de capítulo.
- Botones de auriculares (incluido Bluetooth): pausa y reanuda; también pasan de capítulo (siguiente/anterior). Al desconectar los auriculares, se pausa.
- Modo oscuro por defecto.

## Versión 2.2
- Botón «No parar por batería»: abre la pantalla de restricción de batería (la de MIUI en Xiaomi) y muestra si está desactivada.

## Versión 2.1
- El diagnóstico registra también pausas, reinicios del servicio, cierre desde recientes y fin de capítulo.

## Versión 2.0
- Piper en segundo plano: genera hasta 40 frases por adelantado, buffer de audio de 2 s e hilos con prioridad de audio, para evitar paradas con la pantalla apagada.
- Diagnóstico de lectura (botón al final): registra cortes de audio y generación lenta.

## Versión 1.9
- Valores por defecto: velocidad 1,0, pausa tras frase 1,5 s, pausa tras coma 0,5 s.

## Versión 1.8
- Voces Piper en varios idiomas (español, inglés US/UK, francés, alemán, italiano, catalán, neerlandés, ruso). Se elige en el menú "Voz Piper"; las del idioma del sistema y el inglés salen primero. Cada voz se descarga una sola vez (~65 MB) y puedes tener varias instaladas.

## Versión 1.7
- Modo oscuro (sistema / claro / oscuro).
- Pausa configurable tras comas (al leer y en el MP3).
- Recuerda el último libro y la posición al abrir la app.
- Opción de leer el libro completo pasando de capítulo en capítulo.
- Lectura en segundo plano más robusta: wake lock, reinicio automático del servicio y reanudación donde iba.

## Versión 1.6: idiomas
- La app está en inglés (por defecto) y español, según el idioma del teléfono. Textos en `res/values/strings.xml` (en) y `res/values-es/strings.xml` (es).
- En Android 13 o superior también se puede cambiar el idioma solo para BookVox en Ajustes > Apps.

## Versión 1.3: lectura de PDF
- "Abrir EPUB o PDF" detecta el tipo por el contenido. Los capítulos del PDF salen de sus marcadores o, si no tiene, de bloques de 10 páginas.
- Los PDF escaneados (solo imagen) no tienen texto y no se pueden leer; protegidos con contraseña tampoco.
- Archivo: `PdfParser.kt` (con PdfBox-Android).

## Versión 1.1: voz Piper (medio)
- Elige "Piper (medio)" en la sección Voz. La primera vez descarga la voz (~65 MB, con internet); después funciona sin internet.
- Usa el motor sherpa-onnx con la voz es_ES davefx medium. Sirve para leer y para guardar MP3.
- Solo compila para móviles de 64 bits (arm64), que son casi todos los actuales.
- Archivos: `PiperEngine.kt` (descarga y síntesis) y cambios en `ReaderService.kt` y `Mp3Exporter.kt`.
- Integración con Claude (opcional, no incluida): limpiar el texto antes de leerlo.
