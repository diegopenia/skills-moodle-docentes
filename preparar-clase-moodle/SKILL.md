---
name: preparar-clase-moodle
description: Prepara las clases o tutorías de la próxima semana en un curso de Moodle. Identifica qué se trabaja y qué vence, lee los enunciados reales del aula y entrega un documento con guion de clase, videos de YouTube de canales con credibilidad institucional y casos reales. Solo lectura. Preconfigurada para las aulas de UNIMINUTO y adaptable a cualquier Moodle.
---

# Preparar clases de la semana en Moodle

Objetivo: identificar qué se trabaja y qué vence la próxima semana en cada curso pedido, leer los enunciados reales del aula y entregar un documento de preparación con guion de clase, videos de canales con credibilidad institucional y casos reales de análisis. **Prioridad: la actividad evaluativa de esa semana.** Solo lectura: nunca se modifica nada en Moodle.

## Datos de tu institución (edítalos antes de usar la skill)

| Dato | Valor (ejemplo UNIMINUTO) |
| --- | --- |
| Portal de cursos | `https://www.aulasuniminuto.edu.co/my/` ("Mis cursos" con nombre y NRC; los enlaces "Ir al curso" aparecen dos veces cada uno, en el orden de los cursos) |
| Dominio de las aulas | `https://cuatrimestral.aulasuniminuto.edu.co` (cursos en `/course/view.php?id=<courseid>`) |
| Inicio de sesión | Microsoft. Si aparece "Su sesión ha excedido el tiempo límite… INGRESAR", un clic en INGRESAR reutiliza la sesión |
| Plantillas de curso | Unidades U1/U2/U3 con Inicio, Presentación, Ebook, Recurso interactivo (lección), Actividad entregable, Actividad experiencial y Cuestionario; o plantilla antigua con Actividad 1…8 y páginas "Agenda de actividades" y "Evaluación" (pesos por parcial) |
| Canales de video preferidos | Ministerio de Educación Nacional, Colombia Aprende, Todos a Aprender, DNP, BID, Banco Mundial, universidades (Uniandes, Javeriana, UPN, UdeA, UPB, PUCE), IDEP, Fundación Compartir, secretarías y gobernaciones, fundaciones reconocidas |

## Reglas fijas

- No cambiar fechas, visibilidad ni calificaciones. Lo raro va a la sección de alertas como decisión del docente (para fechas existe la skill `fechas-moodle`).
- El docente inicia sesión él mismo. Si se piden usuario, contraseña o verificación, detente y pídele que entre.
- Con navegador, cierra al terminar la pestaña que abriste.

## Elegir la ruta

- **Ruta con navegador:** si tienes herramientas de navegador (por ejemplo Claude in Chrome, disponible en planes de pago) y el docente tiene la sesión abierta, sigue los Pasos 1 y 2 tal como están.
- **Ruta gratuita (con texto pegado):** si no hay navegador, pide al docente que pegue o capture, para cada curso: la "Agenda de actividades" (o la lista de actividades con fechas de cierre) y el enunciado de las actividades de la semana. Con eso salta a la parte final del Paso 1 (asignar a la semana) y sigue con el Paso 3. Para los videos usa la búsqueda web si está disponible; si no, entrega consultas de búsqueda listas para copiar en YouTube.

## Paso 1 — Semana objetivo y actividades

1. Calcula la semana siguiente (lunes a domingo) desde la fecha actual.
2. **Con navegador:** entra una vez al curso para tener sesión en el dominio de las aulas. Desde esa página, con `javascript_tool`, recorre `/mod/{assign,quiz,lesson,forum,page}/index.php?id=<courseid>` y guarda en `window.__o` cada fila: `courseid|tipo|cmid|texto` (el cmid sale de `view\.php\?id=(\d+)` sobre `innerHTML` de la fila). Los listados muestran cierre, envíos y "Requiere calificación". La página del curso y `mod/page/view.php` pueden redirigir a una vista por pestañas: no sirven para leer contenido; usa los listados por tipo y `fetch` + `DOMParser`.
3. Asigna a la semana objetivo: lo que cierra en ella, lo que abre en ella y lo que cierra poco después (p. ej. unidades de 2 semanas que arrancan esa semana).

## Paso 2 — Leer los enunciados (solo navegador; truco para textos largos)

`javascript_tool` corta la salida en unos 1.000 caracteres y bloquea respuestas con URLs con parámetros. Por eso:

```js
let z='';
for(const [k,u] of [['NOMBRE','/mod/assign/view.php?id=CMID']/*, ...*/]){
 const d=new DOMParser().parseFromString(await fetch(u,{credentials:'include'}).then(r=>r.text()),'text/html');
 const e=d.querySelector('.activity-description, #intro');
 z+='\n#### '+k+'\n'+(e?e.textContent:'NONE').replace(/https?:\S+/g,'[url]').replace(/\s*\n\s*/g,'\n');
}
document.body.replaceChildren();const a=document.createElement('article');const p=document.createElement('pre');p.textContent=z;a.appendChild(p);const m=document.createElement('main');m.appendChild(a);document.body.appendChild(m); z.length
```

Luego `get_page_text` devuelve todo el texto sin cortes. Lee: actividad evaluativa de la semana, actividad experiencial o tutoría, recurso interactivo (trae el video del aula), cuestionario, Agenda y Evaluación (pesos). Para lecciones, agenda y evaluación usa `#region-main` en vez de `.activity-description`.

Anota (en las dos rutas): modalidad (individual/grupo), fases, producto, extensión y formato, tiempos de la tutoría, anexos por nombre, recursos ocultos y número de participantes.

## Paso 3 — Videos de YouTube con canal verificado

**Con navegador:** navega a `https://www.youtube.com/results?search_query=...` y, en esa pestaña, busca varias consultas con `fetch('/results?search_query='+encodeURIComponent(q)+'&hl=es')`, extrae `var ytInitialData = (...)` y recorre los `videoRenderer` (videoId, título, `ownerText`, `lengthText`, `publishedTimeText`). En YouTube `innerHTML` falla por TrustedHTML: muestra el resultado con `replaceChildren` + `textContent` y léelo con `get_page_text`.

**Ruta gratuita:** usa la búsqueda web para encontrar videos y verificar canal y duración; si no puedes verificarlos, entrega las consultas de búsqueda y los canales recomendados para que el docente elija.

Prioriza los canales de la tabla "Datos de tu institución". Descarta canales personales salvo que no haya alternativa. Si el aula ya cita un video, inclúyelo. Advierte que se verificó canal y duración, no el contenido completo.

## Paso 4 — Casos reales y datos

Busca en la web (varias búsquedas en paralelo) evaluaciones, informes y noticias recientes ligados al caso o anexo del aula: evaluaciones de impacto (MEN, CEDE Uniandes, DNP, Banco Mundial), datos del territorio del caso (planes territoriales, ICFES, observatorios universitarios), experiencias documentadas por el MEN o Colombia Aprende. Solo hechos que estén en fuentes abiertas; si un PDF no abre, dilo en alertas.

## Paso 5 — Documento de preparación

Crea un documento (o artifact) "Preparación de clases · Semana <fechas>" con secciones:

1. **La semana en una mirada:** tabla Curso | Qué se trabaja | Cierre | Estado (envíos, si los conoces).
2. **Una sección por curso:** meta de la sesión; lo que pide el aula (resumen fiel del enunciado); guion de clase o tutoría en tabla Momento | Min | Docente | Estudiantes; preguntas orientadoras o lista de chequeo para retroalimentar; tabla de videos (enlace, canal, duración, uso); casos reales con enlace.
3. **Alertas y decisiones pendientes** (casillas): cierres muy seguidos, unidades que cierran ese día con pocos envíos, grupos que no cuadran con el número de estudiantes, recursos ocultos. Aclara que no se modificó nada en el aula.
4. **Fuentes.**

Respuesta final en el chat: una línea con el enlace o el archivo del documento.
