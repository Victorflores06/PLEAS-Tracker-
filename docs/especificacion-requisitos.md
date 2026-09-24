# Especificación de requisitos

**Sistema:** TrackerPLEAS
**Autor:** Victor Manuel Flores Venegas
**Versión:** 0.1 (previa a la entrevista de elicitación)
**Fecha de la última actualización:** [FECHA DE LA PRIMERA VERSIÓN]

---

## 1. Propósito y alcance

**Propósito del documento:**

Este documento define qué debe hacer TrackerPLEAS y con qué calidad. Está dirigido al equipo de desarrollo, a la dupla que revisa el documento, al profesor de la materia y a los directores de los programas de liderazgo, que son quienes validan que los requisitos reflejan su operación real.

**Alcance del sistema:**

- Muestra el avance de puntos de cada estudiante por semestre.
- Muestra los criterios de graduación y el avance total de cada estudiante (TEA's, clases, proyecto, ADAI).
- Registra los puntos y los criterios de graduación de los estudiantes.
- Controla el acceso según el rol del usuario (Estudiante, Director de Programa, Director General).
- Genera reportes de avance para los directores.

**Fuera del alcance:**

- No procesa pagos ni cuotas.
- No crea ni registra actividades.
- No aplica políticas o protocolos automáticamente.

---

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema |
|---|---|---|
| Estudiante | Pregunta por correo o en cita presencial a su director cuál es su estatus de graduación. | Ver su avance y sus requisitos pendientes en cualquier momento. |
| Director de Programa | Concilia archivos de Excel desactualizados, correos y listas en papel para actualizar el estatus de cada alumno. | Una herramienta ágil para actualizar y validar los requisitos de los alumnos de su programa. |
| Director General | Recibe información no unificada entre programas. | Ver el avance global de todos los programas y generar reportes. |

**Conflictos identificados entre usuarios:**

El Estudiante quiere ver su avance reflejado de inmediato; el Director de Programa necesita verificar antes de oficializar las actividades grandes o la acreditación de materias. Se resuelve mostrando el avance como "pendiente de validación" hasta que el director lo valide (RF-008).

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID | Nombre | Prioridad | Origen |
|---|---|---|---|
| RF-001 | Identificación de usuarios y asignación de rol | Imprescindible | Visión |
| RF-002 | Restricción de acceso según rol | Imprescindible | Visión |
| RF-003 | Consulta de puntos por semestre | Imprescindible | Visión |
| RF-004 | Consulta de criterios de graduación y avance total | Imprescindible | Visión |
| RF-005 | Muestra de requisitos pendientes | Imprescindible | Visión |
| RF-006 | Registro de puntos de un estudiante | Imprescindible | Visión |
| RF-007 | Registro de cumplimiento de criterios de graduación | Imprescindible | Visión |
| RF-008 | Distinción entre avance pendiente y validado | Importante | Supuesto propio |
| RF-009 | Validación por el Director del programa del alumno | Imprescindible | Visión |
| RF-010 | Invalidación de aprobaciones por el Director General | Importante | Visión (regla 1) |
| RF-011 | Bloqueo de captura y modificación de expedientes | Importante | Visión (regla 2) |
| RF-012 | Impedimento de "Apto para Graduación" sin 100% | Imprescindible | Visión (regla 3) |
| RF-013 | Bitácora de aprobaciones y modificaciones | Importante | Visión |
| RF-014 | Generación de reportes de avance | Importante | Visión |
| RF-015 | Vista global de indicadores | Importante | Visión |

### 3.2 Fichas

#### RF-001 · Identificación de usuarios y asignación de rol

| Campo | Contenido |
|---|---|
| Descripción | El sistema identifica a cada usuario y le asigna su rol (Estudiante, Director de Programa o Director General) antes de mostrarle información. |
| Origen | Visión del producto, alcance (control de acceso). |
| Prioridad | Imprescindible |
| Criterio de aceptación | Un usuario que no se ha identificado no ve ningún dato de avance. Un usuario identificado ve únicamente las opciones de su rol. |
| Relacionado con | RF-002, RNF-SEG-001 |

#### RF-002 · Restricción de acceso según rol

| Campo | Contenido |
|---|---|
| Descripción | El sistema impide que un usuario ejecute funciones o consulte expedientes fuera de los permisos de su rol. |
| Origen | Visión del producto, atributo de seguridad y control de acceso. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Un Estudiante que intenta registrar puntos o consultar el expediente de otro alumno es rechazado. |
| Relacionado con | RF-001, RF-009, RNF-SEG-001 |

#### RF-003 · Consulta de puntos por semestre

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Estudiante los puntos acumulados en cada semestre. |
| Origen | Visión del producto, alcance. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Un estudiante con puntos registrados en dos semestres ve el total de cada semestre, y cada total coincide con la suma de sus registros. |
| Relacionado con | RF-006, RNF-USA-001 |

#### RF-004 · Consulta de criterios de graduación y avance total

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Estudiante los criterios de graduación (TEA's, clases, proyecto y ADAI) y su porcentaje de avance total. |
| Origen | Visión del producto, alcance. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al entrar, el estudiante ve cada criterio con su estado y un porcentaje total que coincide con los requisitos cumplidos del plan. |
| Relacionado con | RF-007, RF-012, RNF-USA-001 |

#### RF-005 · Muestra de requisitos pendientes

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Estudiante la lista de requisitos que aún no ha cumplido. |
| Origen | Visión del producto, problema (los estudiantes no saben qué les falta). |
| Prioridad | Imprescindible |
| Criterio de aceptación | Un requisito no cumplido aparece en la lista de pendientes. Al registrarse su cumplimiento deja de aparecer. |
| Relacionado con | RF-004, RF-007 |

#### RF-006 · Registro de puntos de un estudiante

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra los puntos de un estudiante con la actividad y la cantidad de puntos. |
| Origen | Visión del producto, alcance. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al guardar un registro con los dos datos, este aparece en el avance del estudiante. Si falta alguno, el sistema no guarda y señala cuál falta. |
| Relacionado con | RF-003, RF-008, RNF-USA-002 |

#### RF-007 · Registro de cumplimiento de criterios de graduación

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra el cumplimiento de cada criterio de graduación de un estudiante (TEA's, clases, proyecto y ADAI). |
| Origen | Visión del producto, alcance. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al marcar un criterio como cumplido, el porcentaje de avance total del estudiante se actualiza y el criterio deja de aparecer como pendiente. |
| Relacionado con | RF-004, RF-005, RF-012 |

#### RF-008 · Distinción entre avance pendiente y validado

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra por separado el avance pendiente de validación y el avance validado cuando la actividad requiere validación del Director de Programa. |
| Origen | Supuesto propio: resolución del conflicto entre Estudiante y Director de la Visión. |
| Prioridad | Importante |
| Criterio de aceptación | Una actividad grande recién registrada aparece de inmediato como "pendiente de validación". Cuando el director la valida, pasa a "validado" y se suma al avance validado. |
| Relacionado con | RF-006, RF-009 |

#### RF-009 · Validación por el Director del programa del alumno

| Campo | Contenido |
|---|---|
| Descripción | El sistema permite validar la actividad de un estudiante únicamente al Director del programa al que pertenece ese estudiante. |
| Origen | Visión del producto, regla de negocio 1. |
| Prioridad | Imprescindible |
| Criterio de aceptación | El Director del programa A valida una actividad de un alumno del programa A. El mismo director no puede validar la de un alumno del programa B. |
| Relacionado con | RF-002, RF-008, RF-010, RF-013 |

#### RF-010 · Invalidación de aprobaciones por el Director General

| Campo | Contenido |
|---|---|
| Descripción | El sistema permite al Director General invalidar una aprobación registrada por un Director de Programa. |
| Origen | Visión del producto, regla de negocio 1. |
| Prioridad | Importante |
| Criterio de aceptación | Cuando el Director General invalida una aprobación, el avance del alumno se recalcula sin esa aprobación y la acción queda en la bitácora. Ningún otro rol puede invalidar aprobaciones de otros. |
| Relacionado con | RF-009, RF-013 |

#### RF-011 · Bloqueo de captura y modificación de expedientes

| Campo | Contenido |
|---|---|
| Descripción | El sistema bloquea la captura y la modificación de avances durante los N días hábiles previos a la ceremonia oficial de graduación. |
| Origen | Visión del producto, regla de negocio 2. El valor de N no está definido y se verifica en la entrevista. |
| Prioridad | Importante |
| Criterio de aceptación | Dentro de los N días hábiles previos a la ceremonia, un intento de registrar o modificar avance es rechazado con un mensaje. La consulta de avance sigue disponible. |
| Relacionado con | RF-006, RF-007 |

#### RF-012 · Impedimento de "Apto para Graduación" sin 100%

| Campo | Contenido |
|---|---|
| Descripción | El sistema impide asignar el estado "Apto para Graduación" a un alumno cuyo avance validado no alcanza el 100% de los requisitos de su plan. |
| Origen | Visión del producto, regla de negocio 3. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Con un alumno al 99% de avance validado, el intento de marcarlo "Apto para Graduación" es rechazado. Con 100%, el estado se asigna. |
| Relacionado con | RF-004, RF-007, RF-008 |

#### RF-013 · Bitácora de aprobaciones y modificaciones

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra el usuario, la acción, el requisito afectado, la fecha y la hora de cada registro, aprobación e invalidación. |
| Origen | Visión del producto, tipo de sistema (trazabilidad: saber quién aprobó qué y cuándo). |
| Prioridad | Importante |
| Criterio de aceptación | Después de aprobar un requisito, la bitácora muestra quién lo aprobó, cuál requisito fue y cuándo. Las entradas de la bitácora no pueden editarse ni borrarse desde el sistema. |
| Relacionado con | RF-006, RF-009, RF-010, RNF-SEG-001 |

#### RF-014 · Generación de reportes de avance

| Campo | Contenido |
|---|---|
| Descripción | El sistema genera, a solicitud del director, un reporte del avance de los estudiantes y permite descargarlo. |
| Origen | Visión del producto, alcance. El formato y la forma de entrega son supuesto propio, por verificar en la entrevista. |
| Prioridad | Importante |
| Criterio de aceptación | Al solicitar el reporte, este muestra el avance por alumno y se puede descargar. |
| Relacionado con | RF-015, RNF-REN-001 |

#### RF-015 · Vista global de indicadores

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Director General el avance global y los indicadores de los estudiantes de todos los programas. |
| Origen | Visión del producto, necesidad del Director General. |
| Prioridad | Importante |
| Criterio de aceptación | El Director General ve el avance por programa y el total general. Los valores coinciden con los expedientes individuales. |
| Relacionado con | RF-002, RF-014, RNF-REN-001 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
|---|---|---|---|---|
| RNF-USA-001 | Usabilidad | Consulta de avance sin capacitación | Imprescindible | Visión |
| RNF-USA-002 | Usabilidad | Rapidez del registro de puntos | Importante | Visión |
| RNF-SEG-001 | Seguridad | Aislamiento de expedientes y funciones por rol | Imprescindible | Visión |
| RNF-CON-001 | Confiabilidad | Disponibilidad en periodos de cierre | Imprescindible | Visión |
| RNF-CON-002 | Confiabilidad | Conservación de registros | Imprescindible | Visión |
| RNF-REN-001 | Rendimiento | Tiempo de carga con el volumen total de alumnos | Importante | Supuesto propio |

### 4.2 Fichas

#### RNF-USA-001 · Consulta de avance sin capacitación

| Campo | Contenido |
|---|---|
| Atributo de calidad | Usabilidad |
| Descripción | Un estudiante que entra por primera vez encuentra su porcentaje de avance total en menos de un minuto y sin ayuda. |
| Métrica | Tiempo entre el inicio de sesión y la localización del porcentaje, en una prueba con al menos cinco estudiantes que no han usado el sistema; se cumple si al menos cuatro lo logran en menos de un minuto. |
| Origen | Derivado del tipo de sistema y de la Visión (atributo de usabilidad). Los valores son supuesto propio. |
| Prioridad | Imprescindible |
| Por qué importa | Si el estudiante no entiende la plataforma, sigue preguntando a su director y el sistema no resuelve el problema. |
| Afecta a | RF-003, RF-004, RF-005 |

#### RNF-USA-002 · Rapidez del registro de puntos

| Campo | Contenido |
|---|---|
| Atributo de calidad | Usabilidad |
| Descripción | Un director registra los puntos de un alumno en máximo 30 segundos. |
| Métrica | Tiempo entre la selección del alumno y la confirmación del registro, medido con al menos tres usuarios de prueba. |
| Origen | Visión del producto (preocupación de los directores por perder tiempo capturando). Los valores son supuesto propio. |
| Prioridad | Importante |
| Por qué importa | Si capturar es lento, los directores regresan a Excel, que es lo que se busca reemplazar. |
| Afecta a | RF-006 |

#### RNF-SEG-001 · Aislamiento de expedientes y funciones por rol

| Campo | Contenido |
|---|---|
| Atributo de calidad | Seguridad |
| Descripción | Ningún usuario accede a expedientes ni funciones que no correspondan a su rol. |
| Métrica | Cero accesos indebidos en las pruebas que cubren cada combinación de rol (Estudiante, Director de Programa, Director General) y función. |
| Origen | Derivado del tipo de sistema y de la Visión (seguridad y control de acceso). |
| Prioridad | Imprescindible |
| Por qué importa | Un alumno podría modificar su progreso o el de otros, y el estado de graduación dejaría de ser confiable. |
| Afecta a | RF-001, RF-002, RF-009, RF-010, RF-013 |

#### RNF-CON-001 · Disponibilidad en periodos de cierre

| Campo | Contenido |
|---|---|
| Atributo de calidad | Confiabilidad |
| Descripción | El sistema está disponible al menos el 99% del tiempo durante los periodos de cierre académico y de graduación. |
| Métrica | Porcentaje de tiempo disponible, medido por periodo de cierre. |
| Origen | Derivado del tipo de sistema y de la Visión (disponibilidad). El 99% es supuesto propio. |
| Prioridad | Imprescindible |
| Por qué importa | Una caída en esas fechas retrasa validaciones y la expedición de autorizaciones para graduarse. |
| Afecta a | RF-006, RF-009, RF-011 |

#### RNF-CON-002 · Conservación de registros

| Campo | Contenido |
|---|---|
| Atributo de calidad | Confiabilidad |
| Descripción | Ningún registro validado se pierde, y ante una falla se recuperan los datos con una antigüedad máxima de 24 horas. |
| Métrica | Antigüedad máxima de los datos recuperados tras una falla simulada; se cumple con 24 horas o menos. |
| Origen | Derivado del tipo de sistema (integridad de datos). Los valores son supuesto propio. |
| Prioridad | Imprescindible |
| Por qué importa | Perder aprobaciones obliga a revalidar expedientes y reintroduce las disputas que hoy existen. |
| Afecta a | RF-006, RF-009, RF-013 |

#### RNF-REN-001 · Tiempo de carga con el volumen total de alumnos

| Campo | Contenido |
|---|---|
| Atributo de calidad | Rendimiento |
| Descripción | El avance de un alumno y el tablero global se despliegan en menos de tres segundos con el total de alumnos registrados. |
| Métrica | Tiempo entre la solicitud y el despliegue completo, medido con hasta 500 alumnos registrados. El volumen de 500 es supuesto propio. |
| Origen | Derivado del tipo de sistema: de información, con consulta frecuente. Los valores son supuesto propio. |
| Prioridad | Importante |
| Por qué importa | Si el sistema tarda, los usuarios vuelven a Excel o a preguntar directamente al director. |
| Afecta a | RF-003, RF-004, RF-014, RF-015 |

---

## 5. Casos de uso

Pendiente: se trabajan en la semana 7, después de la entrevista.

---

## 6. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo |
|---|---|---|---|
| RF-001 | Visión | Pendiente | Pendiente |
| RF-002 | Visión | Pendiente | Pendiente |
| RF-003 | Visión | Pendiente | Pendiente |
| RF-004 | Visión | Pendiente | Pendiente |
| RF-005 | Visión | Pendiente | Pendiente |
| RF-006 | Visión | Pendiente | Pendiente |
| RF-007 | Visión | Pendiente | Pendiente |
| RF-008 | Supuesto propio | Pendiente | Pendiente |
| RF-009 | Visión | Pendiente | Pendiente |
| RF-010 | Visión | Pendiente | Pendiente |
| RF-011 | Visión | Pendiente | Pendiente |
| RF-012 | Visión | Pendiente | Pendiente |
| RF-013 | Visión | Pendiente | Pendiente |
| RF-014 | Visión | Pendiente | Pendiente |
| RF-015 | Visión | Pendiente | Pendiente |

---

## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
|---|---|---|---|
| — | — | Primera versión; sin cambios registrados. | — |

---

## Antes de entregar

- [x] Todos los requisitos tienen identificador único y ninguno está repetido
- [x] Cada requisito expresa una sola idea
- [x] Cada requisito funcional tiene criterio de aceptación comprobable
- [x] Cada requisito no funcional tiene una métrica, no solo un adjetivo
- [x] El campo Origen distingue lo confirmado por el cliente de lo que sigo suponiendo
- [x] Hay al menos un requisito no funcional por cada atributo de calidad que impone mi tipo de sistema
- [x] Ningún requisito impone una solución técnica
- [x] Todos los requisitos caben dentro del alcance declarado
- [ ] La tabla de trazabilidad está completa
- [ ] Mi dupla revisó el documento y su revisión está registrada
- [x] Borré los ejemplos y las instrucciones en cursiva
