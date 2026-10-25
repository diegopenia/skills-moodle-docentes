---
name: calificar-entregas-moodle
description: Califica con la rúbrica y retroalimenta las entregas de tareas de Moodle. Lee cada entrega, propone nota por criterio y retroalimentación para aprobación del docente, aplica la regla de no entrega y, si hay navegador disponible, guarda lo aprobado en el aula. Preconfigurada para las aulas de UNIMINUTO y adaptable a cualquier Moodle.
---

# Calificar y retroalimentar entregas en Moodle

Objetivo: para una o varias tareas que indique el docente, leer cada entrega, puntuarla con la rúbrica (o solo retroalimentarla si la tarea no lleva nota), redactar la retroalimentación, aplicar la regla de no entrega y mostrar todo en un documento para aprobación. Solo con el "sí" del docente se guarda: él mismo en el aula (ruta gratuita) o Claude con su sesión en el navegador (ruta con navegador).

## Datos de tu institución (edítalos antes de usar la skill)

| Dato | Valor (ejemplo UNIMINUTO) |
| --- | --- |
| Portal de cursos | `https://www.aulasuniminuto.edu.co/my/` |
| Dominio de las aulas | `https://cuatrimestral.aulasuniminuto.edu.co` (cursos en `/course/view.php?id=<courseid>`) |
| Inicio de sesión | Microsoft; en el subdominio el botón INGRESAR reutiliza la sesión ya abierta |
| Tareas evaluativas | Puntuación sobre 5, método Rúbrica, envío por archivo, retroalimentación por comentarios, sin flujo de trabajo (lo guardado queda visible) |
| Tareas formativas | Tipo de calificación "Ninguna": solo comentario, aunque el listado diga "Requiere calificación" |
| Modo de grupo habitual | Grupos separados, con "evitar envío sin grupo" activo |

## Preferencias por defecto (ajústalas a tu forma de trabajar)

- **Retroalimentación:** cálida, en segunda persona, en español, con tres partes: lo que logró, qué mejorar (ligado a los criterios donde perdió puntos o a lo que pide el enunciado) y un siguiente paso concreto. Cita o señala algo específico del trabajo (una diapositiva, una frase, un dato). De 80 a 160 palabras. Sin juicios sobre la persona.
- **Modalidad:** si las tutorías son virtuales y las entregas digitales, nunca pidas imprimir ni llevar algo en físico aunque la guía lo diga. La legibilidad en pantalla (letra muy pequeña, contraste, decoración que compite con el texto) sí se señala.
- **Solo lo entregado:** no bases la retroalimentación en fases de la guía que el estudiante no entrega; retroalimenta lo que está en el archivo.
- **Formato:** los incumplimientos de formato (extensión, tipo de documento, plantilla) no descuentan si la rúbrica no evalúa formato; se mencionan en la retroalimentación con una frase que pida ajustarse en las próximas entregas.
- **Notificación:** "Notificar a los estudiantes" en Sí al guardar.

## Regla de no entrega (ajústala a tu institución)

Para cada estudiante (o grupo) sin envío, determina si su plazo sigue abierto:

- **Plazo abierto** — la fecha actual es anterior al cierre que le aplica: **no se registra nada**, ni nota ni retroalimentación. Solo se lista como "sin entrega, plazo abierto".
- **Plazo vencido, tarea con nota** — retroalimentación **"No se evidencia entrega de la actividad"** y nota **1.0** (o el valor que indique el docente), **nunca 0.0**.
- **Plazo vencido, tarea sin nota numérica** (tipo de calificación "Ninguna") — solo el comentario, sin nota.

Cómo saber qué cierre le aplica a cada uno:
1. Si hay fecha límite (cut-off) habilitada, el plazo vence en la fecha límite; si no, en la fecha de entrega.
2. Si el estudiante tiene **ampliación de plazo** ("Ampliación concedida hasta…"), manda la fecha de la ampliación.
3. Un estado "Borrador (no enviado)" con archivo cuenta como **sin envío**; márcalo como alerta para que el docente decida.
4. **Estudiantes sin grupo:** en tareas en grupo con "evitar envío sin grupo", la columna Grupo dice que no pertenece a ningún grupo y no puede entregar. **No les apliques la regla**: repórtalos como alerta y pregunta al docente a qué grupo asignarlos o si les da ampliación.
5. Si el docente menciona excusas o casos especiales, se excluyen de la regla.

## Reglas fijas

- **Nada se guarda sin aprobación.** Primero el documento de propuestas; se guarda exactamente lo aprobado. Cualquier cambio posterior se vuelve a preguntar.
- **Integridad académica:** indicios de copia, definiciones casi idénticas entre entregas, texto ajeno al caso o contenido genérico (posible IA) se señalan como alerta en el documento, solo a criterio del docente. Los detectores de IA no son prueba: nunca penalices ni menciones al estudiante por cuenta propia.
- **No tocar** fechas, rúbricas ni configuración. Si una entrega ya tiene nota o comentario, muéstralo y pregunta antes de sobrescribirlo.
- **Credenciales:** el docente inicia sesión. Si se piden credenciales o verificación, detente y pídele que entre.
- **Privacidad:** los trabajos y notas son datos personales de los estudiantes. Úsalos solo para esta tarea, no los repitas fuera del documento de aprobación y recuerda al docente que puede anonimizar los archivos antes de subirlos (ruta gratuita).

## Elegir la ruta

- **Ruta con navegador:** si tienes herramientas de navegador (por ejemplo Claude in Chrome, disponible en planes de pago) y el docente tiene la sesión abierta, sigue todos los pasos.
- **Ruta gratuita (con archivos):** si no hay navegador, pide al docente:
  1. El enunciado de la tarea (texto pegado o captura).
  2. La rúbrica completa con criterios, niveles y puntos (texto pegado o captura de "Calificación avanzada").
  3. El ZIP de **"Descargar todas las entregas"** de la página de calificación de la tarea. Con "Descargar entregas en carpetas" (opción por defecto) trae una carpeta por estudiante con el formato `<Nombre completo>_<id>_assignsubmission_file`; en entregas por grupo usa el nombre del grupo.
  4. La tabla de entregas (captura o texto) con estado, grupo, fechas y ampliaciones, para aplicar la regla de no entrega.
  5. La fecha de entrega y la fecha límite de la tarea.

  Lee los archivos del ZIP con código (descomprime, extrae texto de PDF y Word, renderiza las páginas sin texto para verlas). Al final entrega el documento de aprobación y, para cada estudiante, el bloque listo para copiar en el calificador: nivel por criterio, comentario por criterio y retroalimentación general.

  **Hoja de calificaciones fuera de línea:** Moodle permite descargar y subir un CSV con notas y comentarios ("Hoja de calificaciones fuera de línea" en Tipos de retroalimentación), pero **solo funciona con calificación simple directa, no con rúbrica**. Si la tarea no usa rúbrica y el docente activó esa opción, pídele el CSV descargado, llena las columnas de calificación y comentarios de retroalimentación con lo aprobado y devuélvelo en UTF-8 para que lo suba con "Subir hoja de calificaciones". Con rúbrica, el docente copia los bloques en el calificador.

## Datos técnicos del aula (verificados en uso real en Moodle 4.x; solo navegador)

- La columna "Comentarios de retroalimentación" de la tabla de entregas no muestra el texto: la retroalimentación se verifica en el calificador.
- En tareas **en grupo**, en el calificador `applytoall` es un **select** (valor 1 = aplicar a todo el grupo), no una casilla. En no entregas grupales, aplica la regla al grupo completo.
- `javascript_tool` corta la salida en unos 1.000 caracteres y bloquea respuestas con URLs con parámetros (no devuelvas `location.search` ni enlaces). Para textos largos: escribe el resultado en la página (`document.body.replaceChildren(pre)` con `textContent`) y léelo con `get_page_text`.
- Siempre lee la rúbrica real de la tarea; no supongas criterios.

## Paso 1 — Alcance

Pregunta solo lo que falte, en una tanda: curso(s) y tarea(s), si incluye entregas tardías, casos especiales y énfasis adicionales. Con navegador, mapea nombres a cmid desde `/mod/assign/index.php?id=<courseid>`. Si el docente pide enfocarse en una sola tarea, no te desvíes a otras (repórtalas solo como alerta).

## Paso 2 — Configuración, enunciado, rúbrica y plazos

**Ruta gratuita:** toma estos datos de lo que adjuntó el docente (ver "Elegir la ruta") y confirma con él el tipo de calificación (Puntuación / Escala / Ninguna) si no es evidente.

**Con navegador**, desde la página `action=grading` de la tarea, en un solo script:
- Configuración desde `modedit.php`: `grade[modgrade_type]` (Puntuación / Escala / **Ninguna**), `grade[modgrade_point]`, `advancedgradingmethod_submissions` (Rúbrica / Calificación simple directa), `teamsubmission`, y los plazos `allowsubmissionsfromdate[...]`, `duedate[...]`, `cutoffdate[...]` comparados con la fecha y hora local.
- Filas de la tabla (Paso 3.1).
- Rúbrica, si la hay: `M.cfg.contextid` + `fetch('/grade/grading/manage.php?contextid=<ctx>&component=mod_assign&area=submissions')`; por cada `tr.criterion`, `.description` y sus `.level` con `.score` y `.definition`.

Vuelca todo en la página y léelo con `get_page_text`. En otro script: enunciado (`.activity-description` de `/mod/assign/view.php?id=<cmid>`) y texto de los archivos. Si el tipo es "Ninguna", díselo al docente: solo habrá retroalimentación.

## Paso 3 — Listar y leer entregas

1. Clasifica a cada estudiante: con envío / sin envío y plazo abierto / sin envío y plazo vencido / sin grupo. Con navegador, recorre `table tbody tr` en `action=grading`: nombre, estado (incluida ampliación), grupo, nota actual, fecha de entrega, archivos y `userid` del enlace `action=grader`.
2. Lee cada archivo:
   - **PDF:** extrae el texto por página. Con navegador: enlaces con `html.match(/https:[^"']*submission_files[^"']*\.pdf[^"']*/g)` sobre el HTML de `action=grading` (decodifica `&amp;`; usa `Set` porque se repiten), `import('https://cdnjs.cloudflare.com/ajax/libs/pdf.js/4.0.379/pdf.min.mjs')`, `workerSrc` = `.../pdf.worker.min.mjs`, `getDocument({data: arrayBuffer})` y `getTextContent()`.
   - **Páginas sin texto** (PDF escaneados, infografías, mapas, diapositivas como imagen): renderízalas y míralas. No califiques sin haber visto cada página.
   - **Word (.docx):** texto de `word/document.xml`.
   - **Imágenes sueltas:** míralas.
   - **Enlaces de YouTube:** verifica que el video es público y su duración si puedes. No es posible ver el video: califica lo legible y deja los criterios de ejecución como "pendiente: requiere ver el video".
3. Comprueba requisitos formales (extensión, tipo de documento, plantilla, datos institucionales, APA, cita de la lectura base, trabajo individual en actividad colaborativa) y legibilidad en pantalla.
4. Compara las entregas entre sí (definiciones o frases casi iguales) para la alerta de integridad.

## Paso 4 — Proponer calificación y retroalimentación

Con nota: nivel por criterio con justificación de una línea, total sobre la escala de la tarea, comentario corto por criterio y retroalimentación general. Sin nota: tabla comparativa (subtemas cubiertos, lo más fuerte, lo que falta) y retroalimentación. Lee todas las entregas antes de fijar niveles y calibra entre entregas parecidas.

Documento de aprobación (documento o artifact; hoja de cálculo si son muchas):
1. Empieza con el resumen: cuántas entregas, notas o "sin nota", quiénes no reciben nada y por qué, y la frase "nada se ha guardado".
2. **Entregas:** con nota, tabla Criterio | Nivel | Por qué | Comentario en la rúbrica y total; sin nota, la tabla comparativa. La retroalimentación de cada uno en cita.
3. **No entrega, plazo vencido:** Estudiante | Cierre | Nota (1.0 o "sin nota") | comentario fijo.
4. **Sin entrega, plazo abierto (no se registra nada):** Estudiante | Grupo | Cierre (o solo el conteo si son muchos).
5. **Alertas y preguntas** numeradas (sin grupo, formato, trabajo individual, integridad, videos pendientes).

Espera el "sí" (puede aprobar todo, algunas filas o pedir cambios). Los ajustes se hacen en el documento antes de guardar.

## Paso 5 — Guardar (solo lo aprobado)

**Ruta gratuita:** entrega, por estudiante y en el orden de la tabla de entregas, el bloque listo para copiar en el calificador (niveles, comentarios por criterio y retroalimentación), o el CSV de la hoja fuera de línea si aplica. Para la no entrega con rúbrica, explica el procedimiento de abajo para que el docente lo haga.

**Con navegador:** abre `/mod/assign/view.php?id=<cmid>&action=grader&userid=<userid>`, espera 3-4 s y, sobre el formulario real (en tareas sin nota no hay `tr.criterion` y el bucle no hace nada):

```js
// niveles: {criterio: puntos}; remarks: {criterio: texto}; fb: html de la retroalimentación
for(const tr of document.querySelectorAll('tr.criterion')){
 const desc=tr.querySelector('.description').textContent.trim(); const pts=niveles[desc]; if(pts==null) continue;
 const lv=[...tr.querySelectorAll('.level')].find(l=>parseFloat(l.querySelector('.score').textContent.replace(',','.'))===pts);
 const rd=lv.querySelector('input[type=radio]');
 if(!rd.checked) lv.click();            // OJO: clic en la celda .level, NO en el radio (el radio se desmarca)
 if(!rd.checked){rd.checked=true; rd.dispatchEvent(new Event('change',{bubbles:true}));}
 const rm=tr.querySelector('textarea[name*="remark"]'); if(rm&&remarks[desc]){rm.value=remarks[desc]; rm.dispatchEvent(new Event('change',{bubbles:true}));}
}
const ta=document.querySelector('[name="assignfeedbackcomments_editor[text]"]');
const ed=window.tinymce&&tinymce.get(ta.id);
const previo=ed?ed.getContent({format:'text'}).length:ta.value.length;   // si >0, ya había comentario: pregunta antes
if(ed){ed.setContent(fb); ed.setDirty(true); ed.fire('change'); ed.fire('input'); ed.save();}  // ed.save() es obligatorio: sin él el comentario se guarda vacío
ta.value=fb; ta.dispatchEvent(new Event('input',{bubbles:true})); ta.dispatchEvent(new Event('change',{bubbles:true}));
const all=document.querySelector('select[name="applytoall"]'); if(all) all.value='1';
const nt=document.querySelector('input[type=checkbox][name="sendstudentnotifications"]'); if(nt&&!nt.checked) nt.click();
JSON.stringify({previo, niveles:document.querySelectorAll('tr.criterion .level[aria-checked=true]').length, ta:ta.value.length, notify:nt?.checked})
```

Antes de guardar comprueba: que el nombre del estudiante en el panel es el correcto, comentario previo vacío, niveles marcados = número de criterios, `ta.value` con la longitud del texto y notificación en true. Pulsa "Guardar cambios" (barra inferior). Si el editor es Atto, escribe en su `div[contenteditable]` y dispara `input`. Si algo no coincide, no guardes: reporta y pregunta.

**Verificación obligatoria tras cada guardado:** vuelve a abrir el mismo calificador, espera 5 s y lee la retroalimentación guardada, el nombre del estudiante, los niveles marcados y los comentarios por criterio. Si la retroalimentación quedó vacía, repítela y guarda otra vez; avisa al docente si el estudiante recibió una notificación incompleta.

**No entrega con rúbrica** (el nivel más bajo vale 0, así que llenar la rúbrica daría 0.0):
1. En el calificador escribe solo el comentario, sin seleccionar niveles, y guarda.
2. Pon la nota en el **libro de calificaciones** (`/grade/report/grader/index.php?id=<courseid>`, modo edición) sobrescribiendo la casilla de esa tarea con 1.0 (queda "Sobrescrito").
3. **Primera vez en la sesión:** hazlo con un solo estudiante, verifica en el calificador y en el libro, y confirma con el docente antes de seguir. Si el aula no deja guardar el comentario sin rúbrica, detente y pregunta.
4. Si un estudiante con nota sobrescrita entrega después (por ampliación), avisa al docente: la sobrescritura bloquea la nota hasta que se quite.

En tareas sin nota basta el paso 1 (con prueba en un solo estudiante primero). Trabaja en lotes de 3 a 5. Si un guardado falla o no confirma, verifica antes de repetir.

**Cuando el plazo cierra después de la sesión:** si tu entorno permite programar tareas, ofrece programar la aplicación de la regla para la mañana siguiente al cierre, dejando en el mensaje curso, tarea, tipo de tarea (con o sin nota), lo ya aprobado y los casos a preguntar. Lo no aprobado se prepara para aprobación, no se guarda.

## Paso 5b — Asignar estudiantes a grupos (solo navegador y solo si el docente lo pide)

1. En `/group/index.php?id=<courseid>` lee los grupos y su número de miembros (`#groups option`; el texto termina en "(n)"). Usa el grupo vacío de numeración más baja salvo que el docente indique otro.
2. En `/group/members.php?group=<groupid>`, marca por JS las opciones de `#addselect` cuyo `value` sea el userid de cada estudiante y luego ejecuta `document.querySelector('#add').click()` **por JavaScript** (el clic por referencia no envía la selección).
3. Espera 2-3 s y verifica en `#removeselect` que aparecen; después confirma en la tabla de entregas que la columna Grupo ya muestra el grupo.

## Paso 6 — Verificación y reporte

1. **Con navegador:** recarga `action=grading` y el libro de calificaciones; compara la nota de cada estudiante con lo aprobado (OK / PENDIENTE) y la retroalimentación en el calificador. Comprueba que ninguna no entrega quedó en 0,00 y que nadie con plazo abierto recibió nota o comentario.
2. **Ruta gratuita:** pide al docente una captura de la tabla de entregas después de guardar y haz la misma comparación.
3. Reporte breve: entregas calificadas o retroalimentadas, promedio y distribución (si hay nota), criterios más débiles, no entregas registradas, estudiantes con plazo aún abierto, cambios de grupos y pendientes (videos, borradores, alertas).
