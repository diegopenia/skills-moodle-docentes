---
name: fechas-moodle
description: Ajusta las fechas de apertura y cierre de los entregables de un curso en Moodle según el calendario académico oficial que el docente adjunta en PDF. Calcula las semanas, propone una tabla de cambios para aprobación y, si hay navegador disponible, la aplica en el aula. Preconfigurada para UNIMINUTO (cuatrimestral, distancia tradicional) y adaptable a cualquier Moodle.
---

# Ajuste de fechas de entregables en Moodle

Objetivo: leer el calendario académico oficial que adjunta el docente, calcular las fechas de cada entregable según la semana del periodo y mostrar una tabla de cambios para aprobación. Solo tras el visto bueno se aplican los cambios: el docente a mano (ruta gratuita) o Claude con la sesión del docente en el navegador (ruta con navegador). También sirve para **revisar un curso que el docente ajustó a mano** y señalar lo que quedó mal.

## Datos de tu institución (edítalos antes de usar la skill)

Esta sección es lo único que cambia entre instituciones. Viene llena con el ejemplo de UNIMINUTO; si usas otro Moodle, reemplaza los valores.

| Dato | Valor (ejemplo UNIMINUTO) |
| --- | --- |
| Portal de cursos | `https://www.aulasuniminuto.edu.co/my/` |
| Dominio de las aulas | `https://cuatrimestral.aulasuniminuto.edu.co` (cursos en `/course/view.php?id=<courseid>`) |
| Inicio de sesión | Microsoft. Al entrar por primera vez al subdominio aparece "Identifíquese usando su cuenta en: INGRESAR"; el clic reutiliza la sesión de Microsoft ya abierta |
| Calendario oficial | PDF cuatrimestral con tres periodos: 16 semanas, Momento 1 (8 semanas) y Momento 2 (8 semanas), cada uno con tres registros de notas |
| Sistema de registro de notas | Génesis |
| Estructura típica del curso | Unidades 1, 2 y 3; páginas "Agenda de actividades" y "Evaluación" (esta dice qué actividades forman cada parcial y su peso) |
| Zona horaria | America/Bogota |

## Preferencias por defecto (ajústalas a tu forma de trabajar)

- Apertura: lunes 00:00 de la semana, o lo que indique el diseño del curso. Cierre: domingo 23:59.
- Entregable final antes del fin del periodo, para tener tiempo de calificar. Si el domingo de la última semana cae después del fin del periodo, proponer cerrar antes (miércoles de la última semana o domingo de la penúltima), dejando al menos 5 días antes del último registro de notas. Mostrar las alternativas como Opción A / B.
- Si un entregable cierra 3 días hábiles o menos antes del registro de notas de su parcial, marcarlo con ⚠️ y ofrecer adelantarlo a mitad de semana.
- Tareas: fecha límite (cut-off) igual al cierre si el resto del curso la usa; señalar las tareas que no la tienen y preguntar.
- No activar "Recordarme calificar" salvo que el docente lo pida.

## Reglas fijas

- **Fuente del calendario:** siempre el PDF oficial que adjunta el docente. Si no lo adjuntó, pídelo antes de seguir. No uses calendarios de periodos anteriores ni de otras modalidades.
- **Nada se guarda sin aprobación:** primero la tabla de cambios, luego se aplica solo lo aprobado. La aprobación cubre exactamente esas filas; cualquier cambio extra requiere preguntar de nuevo.
- **No tocar:** calificaciones, restricciones de acceso, configuración de intentos, actividades ocultas duplicadas ni actividades fuera de la tabla aprobada. Si una fecha parece rara pero pudo ser intencional (p. ej. un examen diagnóstico que cierra después de la Unidad 1), pregúntala; no la cambies por tu cuenta.
- **Credenciales:** el docente inicia sesión él mismo. Nunca escribas usuario, contraseña ni códigos de verificación. Si aparece una pantalla de inicio de sesión o un CAPTCHA, pide al docente que entre y avise.
- **Datos de estudiantes:** esta skill no los necesita. Si llegan en algún archivo, no los copies en la respuesta.

## Elegir la ruta

Revisa qué herramientas tienes:

- **Ruta con navegador:** si tienes herramientas de navegador (por ejemplo Claude in Chrome, disponible en planes de pago) y el docente tiene la sesión abierta en el aula, sigue los Pasos 1 a 8 completos.
- **Ruta gratuita (con archivos):** si no hay navegador, pide al docente lo que necesitas como archivos o texto pegado y salta los pasos marcados "solo navegador". Para las fechas actuales del curso, pídele que abra en el aula los listados por tipo de actividad (`/mod/assign/index.php?id=<courseid>`, y lo mismo con `quiz`, `lesson`, `forum`, `feedback`) y pegue o capture la tabla, o que pegue la página "Agenda de actividades". Al final, entrega la tabla aprobada como lista de pasos para aplicarla a mano: Moodle no trae un informe nativo para editar fechas en lote (el informe "Fechas" es un plugin adicional que no todas las instituciones instalan), así que se edita actividad por actividad en su configuración.

## Paso 1 — Datos de entrada

Pregunta solo lo que falte, en una sola tanda:
1. PDF del calendario académico.
2. Periodo (en el ejemplo de UNIMINUTO: 16 semanas, Momento 1 o Momento 2).
3. Curso(s) por nombre o NRC.
4. Si tiene `plan_semanas_<nrc>.json` de un periodo anterior: úsalo y omite la inferencia del Paso 4.
5. Ruta gratuita: la lista de actividades con sus fechas actuales (ver "Elegir la ruta").

## Paso 2 — Extraer el calendario

Lee el PDF completo. Para el periodo elegido: inicio, fin, recesos (si los hay), los registros de notas y la consulta de notas definitivas. Si el PDF trae varios periodos, lístalos con sus fechas y su estado según la fecha de hoy (terminado / en curso / por iniciar) y pide al docente que elija; "el curso vigente" es el periodo en curso. Busca los festivos de Colombia (o del país del docente) del año si el PDF no los trae. Resume en pocas líneas, numera las semanas (lunes a domingo) y señala si el domingo de la última semana queda después del fin del periodo.

Calcula las semanas en código, no a mano:

```python
from datetime import date, timedelta, datetime

DIAS = {"lunes":0,"martes":1,"miercoles":2,"miércoles":2,"jueves":3,"viernes":4,"sabado":5,"sábado":5,"domingo":6}

def lunes_de(d):
    return d - timedelta(days=d.weekday())

def semanas_lectivas(inicio, fin, recesos):
    """recesos: lista de (desde, hasta). Devuelve lista de lunes de semanas lectivas."""
    out, l = [], lunes_de(inicio)
    while l <= fin:
        if not any(a <= l + timedelta(days=3) <= b for a, b in recesos):
            out.append(l)
        l += timedelta(days=7)
    return out

def fecha(semanas, n, dia, hora="23:59"):
    l = semanas[n-1]
    h, m = map(int, hora.split(":"))
    return datetime.combine(l + timedelta(days=DIAS[dia]), datetime.min.time()).replace(hour=h, minute=m)
```

## Paso 3 — Leer las fechas actuales del curso (solo navegador)

1. Abre el curso una vez en una pestaña del navegador para tener sesión en el dominio de las aulas (resuelve el botón de ingreso si aparece).
2. Obtén los cmid de las actividades con fecha desde los listados por tipo: `/mod/{assign,quiz,lesson,feedback,forum,workshop,choice}/index.php?id=<courseid>`. Extrae los ids con una expresión regular sobre el HTML (`mod/<tipo>/view\.php\?id=(\d+)`), porque algunos enlaces no están dentro de `table a`. La página principal del curso puede cargar las unidades por JavaScript y no sirve para listar actividades.
3. Lee todos los campos de fecha con fetch al formulario de edición (solo lectura) y DOMParser:

```js
const ids=[/* cmids */]; const out=[];
for(const id of ids){
 const d=new DOMParser().parseFromString(await fetch('/course/modedit.php?update='+id,{credentials:'include'}).then(r=>r.text()),'text/html');
 const g=n=>d.querySelector(`[name="${n}"]`);
 const fields=[...d.querySelectorAll('[name$="[enabled]"]')].map(e=>{const p=e.name.replace('[enabled]','');if(!g(p+'[year]'))return null;const v=n=>String(g(p+`[${n}]`).value).padStart(2,'0');return `${p}=${e.checked?`${v('year')}-${v('month')}-${v('day')} ${v('hour')}:${v('minute')}`:'off'}`}).filter(x=>x&&!x.startsWith('completionexpected'));
 out.push(`${id}|${g('name')?.value}|vis:${g('visible')?.value}|${fields.join(' ; ')}`);
}
out
```

Campos por tipo: tarea `allowsubmissionsfromdate`, `duedate`, `cutoffdate`, `gradingduedate`; cuestionario `timeopen`, `timeclose`; lección `available`, `deadline`; feedback `timeopen`, `timeclose`; foro `duedate`, `cutoffdate`.

Puede haber actividades duplicadas ocultas (visible = 0) sin fecha: repórtalas, no las toques.

## Paso 4 — Plan de semanas

Semana 1 = la semana que contiene el inicio del periodo; las semanas de receso no cuentan. Asigna la semana de cierre de cada actividad en este orden:
1. `plan_semanas_<nrc>.json` adjunto.
2. **Corrimiento del diseño anterior:** si las fechas actuales son de otro periodo, o el docente ya ajustó una parte, calcula el desplazamiento en semanas completas entre lo que el docente ya puso y el patrón anterior (p. ej. la Unidad 1 pasó del 30 de agosto al 25 de octubre = +8 semanas) y aplica el mismo corrimiento al resto. Así se conserva el diseño del curso (unidades de 2 semanas, examen final un viernes, etc.).
3. Página "Evaluación" del curso (o su equivalente): qué actividades forman cada parcial. Cada parcial debe cerrar antes del registro de notas que le corresponde.
4. Si nada de lo anterior aplica: una actividad evaluativa por semana, en el orden de la agenda, y pregunta lo dudoso.

Reglas al calcular:
- Si un cierre cae en festivo, márcalo y pregunta; no lo muevas por tu cuenta.
- Ningún cierre después del fin del periodo ni del último registro de notas; márcalo si pasa.
- Si una tarea ya tiene entregas y su nueva apertura queda en el futuro, avísalo (los demás estudiantes no podrán entregar hasta esa fecha).

## Paso 5 — Revisión de cursos ajustados a mano

Cuando el docente pide revisar un curso que ya ajustó, clasifica cada actividad:
- ✅ Bien: coincide con el periodo y el diseño.
- ⚠️ Raro pero quizá intencional: pregúntalo, no lo cambies.
- ❌ Error: fechas de un periodo ya terminado o de años anteriores, actividades de una misma unidad con cierres distintos, fecha límite vencida que bloquea entregas futuras, tareas sin fecha límite cuando el resto la tiene.

Advierte primero lo que bloquea a los estudiantes. Revisa también lecciones, acuerdo pedagógico, examen diagnóstico, examen final y autoevaluación, no solo tareas.

## Paso 6 — Tabla de cambios para aprobación

Tabla compacta por curso. Agrupa en una fila las actividades de la misma unidad que comparten fechas: Actividades | Abre | Cierra (y fecha límite) | Registro de notas correspondiente. Si hay alternativas (p. ej. cierre del final), muéstralas en columnas Opción A / B. Termina con las preguntas pendientes, numeradas, y espera el "sí". Si son muchos cursos, entrégala como hoja de cálculo o documento y deja el resumen en el chat.

## Paso 7 — Aplicar

**Ruta gratuita:** entrega, para cada actividad aprobada, una línea con el nombre, dónde editarla (Configuración de la actividad > Disponibilidad, o el enlace `/course/modedit.php?update=<cmid>` si lo conoces) y los valores exactos a poner. Ordénala por unidad para que el docente la siga de arriba abajo.

**Ruta con navegador:** para cada actividad aprobada, navega a `/course/modedit.php?update=<cmid>`, fija los campos con JavaScript sobre el formulario real y envía con "Guardar cambios y regresar al curso" (`submitbutton2`):

```js
const S=(p,y,m,d,h,mi)=>{const q=n=>document.querySelector(`[name="${p}[${n}]"]`);const en=q('enabled');if(en&&!en.checked)en.click();for(const [n,v] of [['year',y],['month',m],['day',d],['hour',h],['minute',mi]]){const e=q(n);e.disabled=false;e.value=String(v);e.dispatchEvent(new Event('change',{bubbles:true}));}};
S('allowsubmissionsfromdate',2026,10,19,0,0); S('duedate',2026,10,25,23,59); S('cutoffdate',2026,10,25,23,59);
document.querySelector('[name="submitbutton2"]').click(); 'ok'
```

- Trabaja en lotes de 4 a 6 actividades (navegar → JS → esperar 3-4 s). Lotes más grandes pueden pasar el tiempo límite de la herramienta.
- Si un lote se corta o alguna página no vuelve al curso, **no repitas a ciegas**: verifica primero (Paso 8) y reaplica solo lo pendiente.

## Paso 8 — Verificación y reporte

1. **Solo navegador:** vuelve a leer las fechas con el script del Paso 3 y compáralas contra lo aprobado; reporta OK o PENDIENTE por actividad y corrige lo pendiente. Revisa los listados `/mod/<tipo>/index.php?id=<courseid>` para confirmar día y hora tal como los ven los estudiantes.
2. **Ruta gratuita:** pide al docente que pegue o capture de nuevo los listados por tipo y compáralos contra la tabla aprobada.
3. Reporte breve: tabla final por unidad o actividad, lo que no se tocó y por qué, y avisos.
4. Guarda el plan como `plan_semanas_<nrc>.json` y entrégalo como archivo para reutilizarlo el próximo periodo:

```json
{
  "curso": "Nombre del curso",
  "nrc": "<NRC>",
  "moodle": "<dominio de las aulas>/course/view.php?id=<courseid>",
  "periodo_referencia": "<nombre del periodo>",
  "notas": "Decisiones del docente que deben conservarse",
  "actividades": [
    {"nombre": "Actividad 2 - Evaluativa", "cmid": "<cmid>", "tipo": "assign", "parcial": 1, "semana": 2, "dia_cierre": "domingo", "hora_cierre": "23:59", "apertura_semana": 2, "dias_gracia_corte": 0}
  ]
}
```

Los cmid sirven mientras el curso sea el mismo; si la institución crea un aula nueva para el siguiente periodo, empareja por nombre de actividad y vuelve a leer los cmid.
