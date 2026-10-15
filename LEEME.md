# Aprende Idiomas — Grupo de Estudio

App de práctica de vocabulario estilo Duolingo, hecha en HTML + CSS + JS puro (sin backend, sin frameworks).

## Cómo abrirla

Opción simple: doble clic en `index.html`. Se abre en el navegador y ya funciona.

Nota sobre el progreso guardado: algunos navegadores (sobre todo Firefox) restringen `localStorage` cuando el archivo se abre con doble clic (protocolo `file://`), así que la racha/XP podría no guardarse entre sesiones en ese caso. La app sigue funcionando igual, solo no recuerda el progreso anterior. Si quieren que la racha y el XP persistan siempre, sirvan la carpeta con cualquier servidor local estático:

```bash
# Python (ya viene instalado en Mac/Linux)
python3 -m http.server 8000
# luego abrir http://localhost:8000
```

o suban la carpeta tal cual a GitHub Pages / Netlify (gratis, sin configuración extra) cuando quieran compartirla con el grupo por un link.

## Qué incluye esta primera entrega (Fase 1 del plan)

- Selector de 4 idiomas (inglés activo, chino/japonés/árabe marcados "Próximamente").
- Las 8 palabras de la lección 1: tool, constraint, meadow, respite, ajar, bloat, sparge, moon.
- 3 dinámicas:
  1. **Comparación de palabras** — la que pediste explícitamente: tabla inglés | español, corrige cada fila mostrando la respuesta correcta si falla.
  2. **Opción múltiple** — palabra en inglés, 3 opciones en español.
  3. **Traducción con banco de palabras** — arma la oración en español tocando tiles, con distractores (como en tus capturas).
- Corazones (5, −1 por error), XP (+10 por acierto), racha por día.
- Corrección inmediata y visible en cada error (banner rojo con la respuesta correcta).
- Badge "PALABRA NUEVA" la primera vez que aparece cada palabra en la sesión.
- Voz (Web Speech API) en los botones 🔊 — sin archivos de audio grabados.
- Avatar libre generado con DiceBear (sin depender del arte de Duolingo).

## Qué falta (siguientes fases, ver el plan completo)

- Fase 2: emparejar pares, completar espacio en blanco, construcción de oraciones en contexto, escucha.
- Fase 3: producción libre con corrección por IA, historia corta.
- Fase 4: contenido real en chino, japonés y árabe (el selector y el CSS ya están listos para RTL).
- Fase 5 (opcional): racha compartida entre todo el grupo vía Firebase.

## Cómo agregar una palabra nueva (sin tocar el motor)

Editar `data/words.en.js` y agregar un objeto con esta forma al final del arreglo:

```js
{
  id: 'ejemplo_001',
  word: 'ejemplo',
  translation: 'traducción principal',
  acceptableAnswers: ['traducción principal', 'variante aceptada'],
  partOfSpeech: 'noun',
  sentence: {
    en: 'Example sentence with the word.',
    es: 'Oración de ejemplo con la palabra.'
  },
  distractors: ['palabra falsa 1', 'palabra falsa 2', 'palabra falsa 3']
}
```

Nada más se toca: el motor (`js/engine.js`) arma la lección automáticamente con las palabras que encuentre en ese archivo.

## Estructura del proyecto

```
index.html
css/styles.css
js/
  utils.js         → mezclar arreglos, normalizar texto
  storage.js        → localStorage
  audio.js          → texto a voz
  gamification.js   → corazones, XP, racha
  engine.js         → arma y controla la lección
  app.js            → arranque, selector de idioma, pantallas
  dynamics/
    comparacion.js
    opcionMultiple.js
    traduccionBanco.js
data/
  words.en.js        (8 palabras cargadas)
  words.zh.js / words.ja.js / words.ar.js  (vacíos, listos para la Fase 4)
```
