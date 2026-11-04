# Reglas de Learning Site (Quizzes & Readers)

0. **Cuentas de Usuario (`R`, `D` y `admin`) y Nombre de la App (`Learning Site`):**
   - La aplicación se llama **`Learning Site`**. En la pantalla de `Acceso Privado`, el campo de usuario dice únicamente `Usuario...` (sin mencionar `R` ni `D`).
   - **Instancia `R`**: Usuario **`R`**, Contraseña **`Baerga`**. Contiene los quizzes de lectura de R (50% Multiple Choice, 25% Matching Columns, 25% True/False + Spelling Cipher).
   - **Instancia `D`**: Usuario **`D`**, Contraseña **`Lesish`**. Contiene dos botones de quizzes de **5 preguntas** separados: **`☑️ Multiple Choice Quiz (5 Qs)`** y **`✍️ Fill in the Blank Quiz (5 Qs)`** (`20.0 pts` por pregunta = `100 pts` cada quiz, sin quiz combinado). Al terminar `Multiple Choice`, salta automáticamente a `Fill in the Blank`.
   - **Cuenta `admin`**: Usuario **`admin`**, Contraseña **`Santana`**. Es la **única cuenta** donde está visible y habilitada la función de creación de quizzes con IA (`✨ Crear / Quizzes (IA)`), permitiendo crear y gestionar quizzes tanto para `R` como para `D`.
   - El botón `👤 User: ... (Switch)` en el encabezado permite cambiar de usuario en cualquier momento.

1. **Distribución Obligatoria de Preguntas en los Quizzes de `R` (Todas Mezcladas en un Solo Quiz):**
   - Todas las preguntas están mezcladas en un solo quiz (sin separar por botones de tipo de pregunta):
     - **50% Multiple Choice (`mc`)** (ej. 10 de 20 preguntas)
     - **25% Matching Columns (`match`)** (ej. 5 de 20 preguntas)
     - **25% True or False (`tf`)** (ej. 5 de 20 preguntas)
   - Cada pregunta vale **5.0 puntos** (`4.0 pts` tarea de comprensión + `1.0 pt` deletreo `Spelling`). Con 20 preguntas el total es exactamente **100 puntos**.

2. **Reglas de Matching Columns (`match`):**
   - La columna izquierda tiene **4 opciones** y la columna derecha tiene **5 opciones** (las 4 respuestas verdaderas + **1 opción extra distractora** `distractorVariants` que no coincide con ninguna opción de la columna izquierda).
   - Cada línea de la columna izquierda da **1 sola oportunidad libre** para equivocarse; a partir del **segundo intento fallido (`2nd wrong try`) en adelante** en esa línea, se descuentan **`0.25` puntos** (`-0.25 pt`) por cada error adicional.
   - Cuando el usuario comete un error al emparejar las columnas, las parejas ya respondidas correctamente **NO se reinician, NO se reorganizan de posición y NO cambian su redacción** (`solvedPairs` se mantiene intacto); únicamente se reorganizan y cambian de redacción (`rightVariants` / `distractorVariants`) las opciones que aún no han sido respondidas correctamente.

3. **Reglas de Deletreo (`Spelling`) y `Submit Answer`:**
   - En el área de `Spelling` para el usuario `R`, existe un botón únicamente con el icono de bocina (`🔊`) que pronuncia la palabra en voz alta al hacer clic o tocarlo.
   - El botón `Submit Answer` permanece deshabilitado hasta que se haya respondido la pregunta principal **y** se haya resuelto correctamente el `Spelling` o agotado sus 3 intentos.
   - Si el usuario agota sus 3 oportunidades en `Spelling` y aún no acierta, se muestra automáticamente la **ortografía correcta (`Right Spelling`)** tanto en las casillas de letras como en el mensaje de estado.
   - Tanto la parte de comprensión como el `Spelling` cuentan para marcar la pregunta como correcta; si falla cualquiera de las dos, la pregunta pasa a `Review Missed`.

4. **Reglas de `Review Missed`, `Hint`, `Reread chapter` y Sin Referencias de Ubicación:**
   - `Review Missed` está bloqueado hasta que el usuario termine el quiz completo (`isQuizCompleted === true`).
   - Los botones `Hint` y `Reread chapter` permanecen disponibles mientras se responde la pregunta, pero **se deshabilitan inmediatamente después de enviar la respuesta (`Submit Answer` / `isQuizAnswered === true`)** o al completar todas las parejas de una pregunta `Matching Columns`.
   - **Prohibido dar referencias de ubicación**: Ni las preguntas (`q`), ni los `Hints` (`hint`), ni las pistas de deletreo (`clue`) deben mencionar en qué parte o capítulo del libro está la respuesta (ej. **nunca** decir `"At the start of Chapter 7"`, `"in Chapter 8"`, `"at the end of the chapter"`, `"read page #"`, `"check page #"` ni `"look on page #"`). Deben enfocarse únicamente en los eventos y personajes de la historia.
   - Debajo de `Submit Answer` (con espacio de separación) siempre existen los botones `Hint (-1.0 pt)` y `📖 Reread chapter to find answer (-1.0 pt)`.

5. **Audio Feedback y Canciones Finales:**
   - Al presionar `Submit Answer`, suena uno de los 3 audios TTS para respuesta correcta o 3 para incorrecta.
   - Al terminar el quiz, suena automáticamente la canción de 30 segundos correspondiente al grado obtenido (**A**, **B**, **C** o **D y menor**) sin barra de progreso ni botones de Play/Replay.

6. **Contraseña de Reinicio (`Reset`), `AI Quiz Studio` y Acceso Privado:**
   - El botón `Reset` y el botón `✨ Crear / Quizzes (IA)` (`AI Quiz Studio`) solicitan la contraseña `admin123` en un modal con soporte para la tecla `Enter`.
   - Dentro de `AI Quiz Studio`, se pueden subir **imágenes/fotos de páginas**, **documentos (`PDF`, `.docx`, `.txt`, `.md`)** o **pegar texto directo** para que Gemini IA genere automáticamente nuevos quizzes cumpliendo todas las reglas (50% MC, 25% Match con 4 `rightVariants`, 25% T/F, `Spelling`, `Hint`, `Reread chapter` y lector HTML) y los sincronice en `quizzes-library.json`.
   - Mantener siempre la pantalla de `Acceso Privado` con soporte para tecla `Enter` y la etiqueta `Actualizado: [hora] - [mes/día/año]` visible en una esquina tanto en `Acceso Privado` como en la pantalla principal.
