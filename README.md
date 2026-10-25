# Skills de Claude para docentes que usan Moodle

Instrucciones reutilizables ("skills") para que Claude te ayude con el trabajo operativo del aula virtual: ajustar fechas al calendario académico, calificar con rúbrica y preparar las clases de la semana. **Tú apruebas todo antes de que se guarde.**

Creadas por Diego Fernando Peña ([ORCID 0000-0001-8762-2563](https://orcid.org/0000-0001-8762-2563)) a partir de su trabajo en las aulas de UNIMINUTO. Vienen preconfiguradas para UNIMINUTO y se adaptan a cualquier Moodle editando un bloque al inicio de cada skill.

## Qué hay aquí

| Skill | Para qué sirve | Plan gratuito | Con navegador (plan de pago) |
| --- | --- | --- | --- |
| [`fechas-moodle`](fechas-moodle/SKILL.md) | Ajustar fechas de entregables según el calendario en PDF | Calcula las semanas y te da la tabla de cambios para aplicarla a mano | Lee las fechas del aula y aplica lo que apruebes |
| [`calificar-entregas-moodle`](calificar-entregas-moodle/SKILL.md) | Calificar con rúbrica y retroalimentar | Con el ZIP de entregas y la rúbrica, propone nota y retroalimentación para que las copies | Guarda notas y comentarios aprobados en el calificador |
| [`preparar-clase-moodle`](preparar-clase-moodle/SKILL.md) | Preparar las clases de la próxima semana | Con la agenda y los enunciados pegados, arma guion, videos y casos | Lee la semana y los enunciados directo del aula |
| [`equipo-expertos`](equipo-expertos/SKILL.md) | Consultar a 12 especialistas (pedagogía, Moodle, IA educativa…) | Completa | Igual |

## Instalación (5 minutos, funciona con cuenta gratuita)

1. **Activa la ejecución de código.** En [claude.ai](https://claude.ai), ve a Configuración > Capacidades y activa "Ejecución de código y creación de archivos" (en el diseño nuevo se llama "Crear y editar archivos"). Las skills no funcionan sin esto.
2. **Descarga el ZIP de la skill** desde la sección [Releases](../../releases) de este repositorio: hay un ZIP por skill (`fechas-moodle.zip`, etc.). No uses el botón verde "Code > Download ZIP": ese descarga todo el repositorio y Claude espera una sola skill por ZIP.
3. **Súbela.** Ve a Configuración > Skills (o Personalización > Skills), pulsa "Agregar" y luego "Subir Skill", y elige el ZIP. Puedes subir varios a la vez. Claude hace un análisis de seguridad de cada archivo.
4. **Comprueba que esté activada:** aparece en "Creado por ti"; al abrirla, el interruptor de arriba a la derecha debe estar encendido.
5. **Pruébala.** En una conversación nueva escribe, por ejemplo: *"Usa la skill fechas-moodle. Te adjunto el calendario del periodo y la lista de actividades de mi curso. Muéstrame la tabla de cambios; no apliques nada."*

Si la carga falla, revisa que el ZIP contenga una sola carpeta con el mismo nombre de la skill y dentro el archivo `SKILL.md`.

## Adaptarla a tu institución

Cada skill empieza con dos bloques que puedes editar:

- **Datos de tu institución:** URL del Moodle, inicio de sesión, estructura de los cursos, calendario.
- **Preferencias por defecto:** día y hora de cierre, regla de no entrega, estilo de retroalimentación.

Para editarlas: descomprime el ZIP, cambia el `SKILL.md` con cualquier editor de texto, vuelve a comprimir la carpeta y súbela de nuevo. También puedes pedirle a Claude que la ajuste por ti.

## Dos rutas, la misma lógica

Todas siguen el patrón **proponer → aprobar → aplicar → verificar**:

- **Ruta gratuita:** tú descargas de Moodle lo necesario (calendario, entregas, rúbrica), Claude calcula y propone, y tú aplicas los cambios en el aula.
- **Ruta con navegador:** con [Claude in Chrome](https://support.claude.com/en/articles/12012173-get-started-with-claude-in-chrome) (planes Pro, Max, Team y Enterprise), Claude lee el aula con tu sesión abierta y aplica solo lo que apruebes. Nunca escribe tu usuario ni tu contraseña.

## Privacidad y uso responsable

- Los trabajos y notas de tus estudiantes son datos personales (en Colombia, Ley 1581 de 2012). Sube solo lo necesario y, si puedes, anonimiza los archivos antes.
- Revisa la política de tu institución sobre el uso de IA y la configuración de privacidad de tu cuenta de Claude.
- La nota es una decisión docente: las skills proponen; tú revisas y apruebas.
- Las alertas de "posible texto generado por IA" son una señal para conversar con el estudiante, no una prueba.
- Cuéntales a tus estudiantes que preparas la retroalimentación con apoyo de IA.

## Licencia

[MIT](LICENSE). Puedes usar, adaptar y compartir estas skills; agradezco que menciones la fuente. Si las mejoras, abre un *issue* o un *pull request*.
