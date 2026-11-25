# TunEC

Plugin de corrección de pitch en tiempo real (VST3 / AU / Standalone) hecho con JUCE 8.
Motor de detección y corrección **original** (YIN + resíntesis por granos sincronizados al pitch), sin código de terceros.

## Controles

| Sección | Controles |
|---|---|
| Tuning | Input Type, Key, Scale (14 escalas + Custom), Retune Speed (0–400 ms), Flex Tune, Natural Vibrato, Humanize, Ignore Vibrato, Master Bypass |
| Formant & Pitch | Formant Correction, Throat Length, Transpose (±24 st), Detune (±100 cents) |
| MIDI | Target Notes via MIDI, Learn Scale from MIDI, MIDI Octaves (All / Match) |
| Vibrato | Shape, Rate, Onset Delay, Onset Rate, Variation, Pitch, Amplitude, Formant |

Monitor de pitch con traza de entrada/salida, nota detectada, cents y teclado de escala activa.

## Compilar en GitHub

1. Crea un repositorio y sube **todo el contenido de esta carpeta** (incluida `.github/`).
2. En la pestaña **Actions** se ejecuta *Build TunEC* en Windows, macOS (universal arm64+x86_64) y Linux.
3. Descarga los binarios en **Artifacts** al terminar. Para publicar un release: `git tag v1.0.0 && git push --tags`.

CMake descarga JUCE automáticamente (FetchContent), no hay que añadirlo al repo.

## Compilar en local

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release --parallel
ctest --test-dir build -C Release      # tests del motor DSP
```

Los plugins quedan en `build/TunEC_artefacts/Release/{VST3,AU,Standalone}`.
Solo el motor + tests (sin JUCE): `cmake -S . -B build -DTUNEC_BUILD_PLUGIN=OFF`.

## Notas

- **AAX (Pro Tools)**: requiere el SDK de Avid y firma PACE/iLok. Si defines `AAX_SDK_PATH` (variable de entorno o `-DAAX_SDK_PATH=...`) CMake añade el formato AAX. No se puede compilar en CI público.
- **macOS**: los binarios del CI no están firmados/notarizados; para distribuirlos necesitas tu Apple Developer ID.
- **Licencia**: JUCE se usa bajo AGPLv3/GPLv3 o licencia comercial de JUCE. Si distribuyes TunEC debes publicar el código bajo AGPLv3 o comprar la licencia de JUCE.
- Latencia reportada al host según *Input Type* (≈ 1/fmin del rango): Soprano ~10 ms, Alto/Tenor ~17 ms, Bass ~35 ms.
- Nombres, ID de plugin y bundle propios (`Tnca/Tnc1`, `com.tunecaudio.TunEC`); no usa identificadores ni marcas de otros fabricantes.
