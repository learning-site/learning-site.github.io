# Reglas de Learning Site (Quizzes & Readers)

0. **Cuentas de Usuario (`R`, `student` y `admin`) y Nombre de la App (`Learning Site`):**
   - La aplicación se llama **`Learning Site`**. En la pantalla de `Acceso Privado`, el campo de usuario dice únicamente `Usuario...` (sin mencionar `R` ni `student`).
   - **Instancia `R`**: Usuario **`R`**, Contraseña **`Baerga`**. Contiene los quizzes de lectura de R (50% Multiple Choice, 25% Matching Columns, 25% True/False + Spelling Cipher).
   - **Instancia `student` (internamente `'D'`)**: Usuario **`student`**, Contraseña **`study`**. Contiene dos botones de quizzes de **5 preguntas** separados: **`☑️ Multiple Choice Quiz (5 Qs)`** y **`✍️ Fill in the Blank Quiz (5 Qs)`** (`20.0 pts` por pregunta = `100 pts` cada quiz, sin quiz combinado). Al terminar `Multiple Choice`, salta automáticamente a `Fill in the Blank`.
   - **Cuenta `admin`**: Usuario **`admin`**, Contraseña **`Santana`**. Es la **única cuenta** donde está visible y habilitada la función de creación de quizzes con IA (`✨ Crear / Quizzes (IA)`), permitiendo crear y gestionar quizzes tanto para `R` como para `student`.
   - El botón `👤 User: ... (Switch)` en el encabezado permite cambiar de usuario en cualquier momento.

1. **Distribución Obligatoria de Preguntas en los Quizzes de `R` (Todas Mezcladas en un Solo Quiz):**
   - Todas las preguntas están mezcladas en un solo quiz (sin separar por botones de tipo de pregunta):
     - **50% Multiple Choice (`mc`)** (ej. 10 de 20 preguntas)
     - **25% Matching Columns (`match`)** (ej. 5 de 20 preguntas)
     - **25% True or False (`tf`)** (ej. 5 de 20 preguntas)
   - Cada pregunta vale **5.0 puntos** (`4.0 pts` tarea de comprensión + `1.0 pt` deletreo `Spelling`). Con 20 preguntas el total es exactamente **100 puntos**.

2. **Reglas de Matching Columns (`match`):**
   - La columna izquierda tiene **4 opciones** y la columna derecha tiene **5 opciones** (las 4 respuestas verdaderas + **1 opción extra distractora** `distractorVariants` que no coincide con ninguna opción de la columna izquierda).
   - Cada opción de la columna izquierda tiene un valor de **`1.25 pts`** y permite un **máximo de 2 intentos**: el **1.er intento fallido** descuenta la mitad (**`-0.625 pt`**) y el **2.º intento fallido** descuenta la otra mitad (**`-0.625 pt`**, total `-1.25 pts`) y deshabilita esa opción marcándola como incorrecta (**`✗ Wrong` / `failedPairs`**), haciendo que la pregunta pase a `Review Missed` al enviarse.
   - Cuando el usuario comete un error al emparejar las columnas, las parejas ya respondidas correctamente (`solvedPairs`) y las marcadas incorrectas en la columna izquierda (`failedPairs`) **NO se reinician, NO se reorganizan de posición y NO cambian su redacción**; únicamente se reorganizan y cambian de redacción (`rightVariants` / `distractorVariants`) las opciones que aún siguen activas.

3. **Reglas de Deletreo (`Spelling` Bilingüe EN + ES) y `Submit Answer`:**
   - En el área de `Spelling` para el usuario `R`, cada palabra incluye **ambas versiones**: **Inglés (`0.5 pt`)** y **Español (`0.5 pt`)**, dividiendo el punto de deletreo entre las dos (`0.5 + 0.5 = 1.0 pt`), cada una con su propio botón de bocina (`🔊`) para escucharla en su idioma y 3 intentos independientes.
   - **Prohibido poner la palabra de `Spelling` en la pregunta o respuestas**: La palabra de deletreo (`word`, ni su raíz directa) **NUNCA** debe aparecer escrita en el enunciado (`q`), opciones/respuestas (`options` / `answer`), columnas (`pairs` / `rightVariants` / `distractorVariants`), pista (`hint`) ni pista de deletreo (`clue`) de esa misma pregunta.
   - El botón `Submit Answer` permanece deshabilitado hasta que se haya respondido la pregunta principal **y** ambas versiones de `Spelling` (inglés y español) se hayan resuelto o agotado sus 3 intentos.
   - Si el usuario agota sus 3 oportunidades en `Spelling` y aún no acierta, las casillas **conservan el último intento incorrecto** (en rojo, `wrong-word`) y **debajo** se muestra la **ortografía correcta (`Right Spelling`)** en verde (`buildRightSpellingReveal`), además del mensaje de estado. Aplica a inglés y español.
   - Tanto la parte de comprensión como el `Spelling` cuentan para marcar la pregunta como correcta; si falla cualquiera de las dos, la pregunta pasa a `Review Missed`.

4. **Reglas de `Review Missed`, `Hint`, `Reread chapter` y Sin Referencias de Ubicación:**
   - `Review Missed` está bloqueado hasta que el usuario termine el quiz completo (`isQuizCompleted === true`).
   - Los botones `Hint` y `Reread chapter` permanecen disponibles mientras se responde la pregunta, pero **se deshabilitan inmediatamente después de enviar la respuesta (`Submit Answer` / `isQuizAnswered === true`)** o al resolver todas las opciones izquierdas de una pregunta `Matching Columns` (`solvedPairs + failedPairs === 4`).
   - **Prohibido dar referencias de ubicación**: Ni las preguntas (`q`), ni los `Hints` (`hint`), ni las pistas de deletreo (`clue`) deben mencionar en qué parte o capítulo del libro está la respuesta (ej. **nunca** decir `"At the start of Chapter 7"`, `"in Chapter 8"`, `"at the end of the chapter"`, `"read page #"`, `"check page #"` ni `"look on page #"`). Deben enfocarse únicamente en los eventos y personajes de la historia.
   - Debajo de `Submit Answer` (con espacio de separación) siempre existen los botones `Hint (-1.0 pt)` y `📖 Reread chapter to find answer (-1.0 pt)`.

5. **Audio Feedback, Animación por Libro y Canciones Finales:**
   - Cada libro en el selector (`#headerBookSelect`) cuenta con su propia animación temática en la esquina superior izquierda (`BOOK_ANIMATION_THEMES` + `applyBookAnimationTheme`), con personajes, escenario, medidor de duelo y ataques especiales acordes a la historia de ese libro. Solo el libro `Trapped in a Video Game: The Invisible Invasion` usa el acabado 3D de `enhanceDuelSvg3D` (relieve con luz `#duel3dBevel`, suelo en perspectiva, sombras, fondo con parallax, partículas, haces de luz, parpadeo y zoom en golpes especiales); no animar `#bartSvg` con transformaciones continuas porque vuelve borrosos a los personajes.
   - Al presionar `Submit Answer`, suena uno de los 3 audios TTS para respuesta correcta o 3 para incorrecta.
   - **Batalla Final (`startFinalBattle`)**: solo para el usuario `R` y solo cuando termina el quiz con **todas las preguntas correctas** (`mistakesPool` vacío). La pantalla final con el grado se muestra **5 segundos** con cuenta regresiva y luego abre una pelea a pantalla completa con los dos personajes del libro activo: el usuario ataca dando taps sobre el enemigo (combos, golpes críticos, escudo que bloquea) hasta dejar su vida en 0. No se abre al restaurar un quiz ya terminado ni si el usuario sale de la pantalla final durante la cuenta.
   - Al terminar el quiz, suena automáticamente la canción de 30 segundos correspondiente al grado obtenido (**A**, **B**, **C** o **D y menor**) sin barra de progreso ni botones de Play/Replay.

6. **Contraseña de Reinicio (`Reset`), `AI Quiz Studio` y Acceso Privado:**
   - El botón `Reset` y el botón `✨ Crear / Quizzes (IA)` (`AI Quiz Studio`) solicitan la contraseña `admin123` en un modal con soporte para la tecla `Enter`.
   - Dentro de `AI Quiz Studio`, se pueden subir **imágenes/fotos de páginas**, **documentos (`PDF`, `.docx`, `.txt`, `.md`)** o **pegar texto directo** para que Gemini IA genere automáticamente nuevos quizzes cumpliendo todas las reglas (50% MC, 25% Match con 4 `rightVariants`, 25% T/F, `Spelling`, `Hint`, `Reread chapter` y lector HTML) y los sincronice en `quizzes-library.json`.
   - Mantener siempre la pantalla de `Acceso Privado` con soporte para tecla `Enter` y la etiqueta en inglés `Updated: [hora] - [mes/día/año]` (hora de Nueva York; la app está en inglés) visible en una esquina tanto en `Acceso Privado` como en la pantalla principal.
