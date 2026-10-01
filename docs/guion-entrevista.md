# Guion de entrevista

**Sistema:** TrackerPLEAS
**Autor:** Victor Manuel Flores Venegas
**Fecha de la entrevista:** 22 de septiembre de 2026

---

## 1. Guion de preguntas

### Preguntas de contexto

1. ¿Cómo es un día a día en la gestión y seguimiento académico de los alumnos dentro de los programas de liderazgo?
2. ¿Cuáles son las actividades o procesos que más tiempo le consumen al equipo directivo durante el semestre?

### Preguntas sobre cómo se resuelve hoy el problema

1. ¿Me podría describir paso a paso la última vez que un alumno le preguntó por su estatus de graduación y cómo consultó esa información?
2. ¿Qué herramientas, archivos o documentos utilizó exactamente en la última revisión de expedientes del cierre de periodo?
3. ¿Podría darme un ejemplo de un caso reciente en el que un dato no estuviera actualizado en un archivo de Excel o correo, y cómo afectó ese seguimiento?

### Preguntas sobre excepciones

1. ¿Qué ocurre y cuál es el procedimiento cuando un alumno argumenta haber completado un requisito, pero no aparece registrado en los archivos correspondientes?
2. ¿Qué sucede si por alguna contingencia un alumno debe validar un requisito fuera de la fecha límite establecida previa a la graduación?

### Preguntas de verificación (derivadas de los supuestos)

1. ¿Con cuántos días hábiles de anticipación a la ceremonia de graduación debe bloquearse en el sistema la captura o modificación de avances de los alumnos?
2. ¿Cómo funciona o qué datos debería intercambiar el sistema con plataformas institucionales como Soy León?
3. ¿En qué formato necesitan visualizar y descargar los reportes de seguimiento de los programas?


---

## 2. Resultados de la entrevista

### Supuestos que se confirmaron

- **Ineficiencia de las herramientas actuales:** se confirmó que el seguimiento por Excel es lento, propenso a trabarse por el volumen de datos y contiene información no depurada.
- **Integración con Soy León:** se validó la necesidad de conectar con Soy León, confirmando que la información clave a intercambiar son los eventos/actividades, los grupos organizadores y la lista de participantes.
- **Necesidad de congelar expedientes:** se reconfirmó la regla de bloqueo de edición antes del cierre, estableciendo que debe ocurrir una semana después del periodo de intersemestrales.
- **Vulnerabilidades en la validación manual:** se confirmó la existencia de desacuerdos o intentos de fraude por parte de algunos alumnos al no existir trazabilidad clara de sus evidencias.

### Supuestos que resultaron falsos o requieren ajuste

- **Formato de reportes de salida:** se suponía una exportación estándar o consulta bajo demanda; sin embargo, se prefiere un reporte semanal automatizado en PDF enviado directamente por correo electrónico, enfocado en destacar a los alumnos con mayor rezago.
- **Proceso rígido de cierre extemporáneo:** se asumía un bloqueo inflexible, pero en la práctica sí se aceptan validaciones fuera de plazo en casos justificados, lo cual genera un problema técnico actual al desfasar la información de los reportes ya emitidos.

### Lo que apareció que no esperábamos (nuevos hallazgos)

- **Creación de un nuevo rol operativo ("Asistente" o "Comité"):** el director expresó la necesidad de delegar la carga administrativa de subir puntos y revisar evidencias cotidianas a un rol de menor jerarquía, para poder enfocarse en actividades estratégicas y entrevistas.
- **Identificadores visuales/atributos de alumno:** se identificó la dificultad para reconocer a la totalidad de la matrícula por rostro o contexto. Requieren identificadores dentro del sistema que muestren características clave de cada estudiante de un vistazo.
- **Mecanismos alternativos de validación de evidencias:** ante faltas de registro, la dirección actualmente valida asistencias pidiendo un resumen de la actividad al estudiante o revisando inscripciones en Soy León.
- **Penalizaciones injustificadas por desfase de datos:** ocurren errores donde alumnos son penalizados debido a cortes de actualización extemporáneos en los sistemas actuales, a pesar de haber cumplido en tiempo y forma.

---

## 3. Ficha de dominio · Programas de Liderazgo (PLEAS)

*Para quien hace de director de programa.*

### Quién eres

Eres el director de uno de los programas de liderazgo universitarios. Te encargas de gestionar las actividades y validar que los alumnos inscritos cumplan con los requisitos para graduarse de su diplomado.


### Cómo es tu día

Llegas a la oficina, revisas correos de alumnos preguntando por sus puntos y atendiendo solicitudes de validación de actividades. Tienes a tu cargo a decenas de estudiantes y debes revisar constantemente quién ya cumplió con sus criterios y a quién le faltan requisitos para graduarse.

### Reglas que conoces y no vas a decir si no te preguntan

- Cada alumno debe acumular puntos semestrales y cumplir con requisitos como TEA's, clases, proyecto y ADAI.
- Un alumno solo se considera apto para graduación cuando alcanza exactamente el 100% de la validación de sus actividades requeridas.
- Tienes la autoridad para validar las actividades diarias de tus alumnos, pero el Director General puede sobreescribir aprobaciones en casos excepcionales.
- Se debe congelar la carga y modificación de expedientes semanas antes de la ceremonia de graduación.

### Una excepción que ocurre a veces

Cuando un alumno argumenta que ya entregó una evidencia o asistió a un evento, pero no está registrado en los archivos, tienes que buscar en correos antiguos o consultar con los coordinadores para verificar si realmente asistió. Ese proceso frena todo tu trabajo del día.

### Lo que te molesta de cómo trabajas hoy

Perder tiempo valioso buscando datos en archivos de Excel desactualizados, listas en papel y cadenas interminables de correos. Te molesta tener que revisar expediente por expediente de forma manual cada vez que un alumno te pregunta su porcentaje de avance o cuando se acerca el cierre del periodo.

### Cómo responder

Contesta solo lo que te pregunten. Si te preguntan algo que no está en la ficha, invéntalo, pero mantenlo coherente con lo demás. Si te preguntan de forma vaga, responde de forma vaga.
