# Especificación de requisitos

**Sistema:** TrackerPLEAS
**Autor:** Victor Manuel Flores Venegas
**Versión:** 0.2 (posterior a la entrevista de elicitación)
**Fecha de la última actualización:** 29/09/2026

---

## 1. Propósito y alcance

**Propósito del documento:**

Este documento define qué debe hacer TrackerPLEAS y con qué calidad. Está dirigido al equipo de desarrollo, a la dupla que revisa el documento, al profesor de la materia y a los directores de los programas de liderazgo, que son quienes validan que los requisitos reflejan su operación real. Esta versión incorpora lo confirmado y lo descubierto en la entrevista de elicitación con un director de programa.

**Alcance del sistema:**

- Muestra el avance de puntos de cada estudiante por semestre.
- Muestra los criterios de graduación y el avance total de cada estudiante (TEA's, clases, proyecto, ADAI), distinguiendo el avance validado del avance en validación.
- Registra los puntos y los criterios de graduación de los estudiantes, con la evidencia asociada.
- Obtiene de Soy León los eventos y actividades, sus grupos organizadores y las listas de participantes.
- Controla el acceso según el rol del usuario (Estudiante, Asistente/Comité, Director de Programa, Director General).
- Registra la validación de actividades por el Director de Programa, la invalidación por el Director General y las validaciones fuera de plazo con justificación.
- Bloquea la captura y modificación de expedientes después del periodo de intersemestrales.
- Mantiene una bitácora de quién hizo qué y cuándo, incluida la evidencia.
- Genera un reporte semanal de avance en PDF, lo envía por correo a los directores y destaca a los alumnos con mayor rezago.
- Muestra al Director General el avance global de todos los programas.

**Fuera del alcance:**

- No procesa pagos ni cuotas.
- No crea actividades: las actividades provienen de Soy León.
- No aplica políticas o protocolos automáticamente.

---

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema |
|---|---|---|
| Estudiante | Pregunta por correo o en cita presencial a su director cuál es su estatus de graduación. | Ver su avance y sus requisitos por cumplir en cualquier momento, y que no se le penalice por retrasos ajenos a él. |
| Asistente/Comité | Sube puntos y revisa evidencias en los archivos de Excel actuales (por confirmar). | Capturar puntos y revisar evidencias de forma rápida, sin depender de un archivo lento. |
| Director de Programa | Concilia archivos de Excel lentos y con datos sin depurar, correos y listas en papel; resuelve desacuerdos por falta de constancia de las evidencias. | Una herramienta ágil para validar los requisitos de los alumnos de su programa, con constancia de quién validó qué. |
| Director General | Recibe información no unificada entre programas. | Ver el avance global de todos los programas y recibir reportes periódicos. |

**Cambios respecto a la Visión del producto:** se agrega el rol Asistente/Comité (sube puntos y revisa evidencias, pero no valida actividades grandes ni la acreditación de materias). La regla de cierre de periodos se reconfirmó y su plazo, que la Visión dejaba sin valor, quedó definido: una semana después del periodo de intersemestrales. La integración con Soy León pasa de hueco identificado a parte del alcance.

**Conflictos identificados entre usuarios:**

1. El Estudiante quiere ver su avance reflejado de inmediato; el Director de Programa necesita verificar antes de oficializar las actividades grandes o la acreditación de materias. Se resuelve mostrando el avance como "en validación" hasta que el director lo valide (RF-008).
2. El bloqueo de expedientes protege la consistencia de los reportes, pero puede penalizar al Estudiante cuya actividad se validó tarde por causas ajenas a él. Se resuelve aceptando validaciones fuera de plazo con justificación (RF-016), lo que genera un desfase respecto a los reportes ya emitidos.

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID | Nombre | Prioridad | Origen |
|---|---|---|---|
| RF-001 | Identificación de usuarios | Imprescindible | Visión |
| RF-002 | Restricción de acceso según rol | Imprescindible | Visión |
| RF-003 | Consulta de puntos por semestre | Imprescindible | Visión |
| RF-004 | Consulta de criterios de graduación y avance total | Imprescindible | Visión |
| RF-005 | Muestra de requisitos por cumplir | Imprescindible | Visión |
| RF-006 | Registro de puntos de un estudiante | Imprescindible | Visión + entrevista |
| RF-007 | Registro de cumplimiento de criterios de graduación | Imprescindible | Visión + entrevista |
| RF-008 | Distinción entre avance en validación y validado | Importante | Supuesto propio + entrevista |
| RF-009 | Validación por el Director del programa del alumno | Imprescindible | Visión + entrevista |
| RF-010 | Invalidación de aprobaciones por el Director General | Importante | Visión (regla 1) |
| RF-011 | Bloqueo de captura y modificación de expedientes | Importante | Entrevista |
| RF-012 | Impedimento de "Apto para Graduación" sin 100% | Imprescindible | Visión (regla 3) |
| RF-013 | Bitácora de registros, evidencias y aprobaciones | Importante | Visión + entrevista |
| RF-014 | Generación del reporte semanal de avance | Importante | Entrevista |
| RF-015 | Vista global de indicadores | Importante | Visión |
| RF-016 | Validación fuera de plazo con justificación | Importante | Entrevista |
| RF-017 | Obtención de datos desde Soy León | Importante | Entrevista |
| RF-018 | Asociación de evidencia a un registro de puntos | Importante | Entrevista + supuesto propio |
| RF-019 | Revisión de evidencias | Deseable | Entrevista |
| RF-020 | Ficha del alumno | Deseable | Entrevista |
| RF-021 | Lista de actividades en validación | Imprescindible | Supuesto propio |
| RF-022 | Validación alterna de asistencia | Deseable | Entrevista + supuesto propio |
| RF-023 | Envío semanal automático del reporte por correo | Deseable | Entrevista + supuesto propio |
| RF-024 | Orden del reporte por rezago | Deseable | Entrevista |
| RF-025 | Inalterabilidad de la bitácora | Importante | Visión (trazabilidad) |

### 3.2 Fichas

#### RF-001 · Identificación de usuarios

| Campo | Contenido |
|---|---|
| Descripción | El sistema identifica a cada usuario antes de mostrarle información. |
| Origen | Visión del producto, alcance (control de acceso). |
| Prioridad | Imprescindible |
| Criterio de aceptación | Un usuario que no se ha identificado no ve ningún dato de avance. |
| Relacionado con | RF-002, RNF-SEG-001 |

#### RF-002 · Restricción de acceso según rol

| Campo | Contenido |
|---|---|
| Descripción | El sistema impide que un usuario ejecute funciones o consulte expedientes fuera de los permisos del rol asignado a su cuenta (Estudiante, Asistente/Comité, Director de Programa o Director General). |
| Origen | Visión del producto, atributo de seguridad y control de acceso. El rol Asistente/Comité surge de la entrevista. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Un Estudiante que intenta registrar puntos o consultar el expediente de otro alumno es rechazado. Un usuario identificado ve únicamente las opciones de su rol. |
| Relacionado con | RF-001, RF-009, RNF-SEG-001 |

#### RF-003 · Consulta de puntos por semestre

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Estudiante los puntos contabilizados en cada semestre, sin incluir los que están en validación. |
| Origen | Visión del producto, alcance. La exclusión de los puntos en validación aclara una ambigüedad de la v0.1. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Un estudiante con puntos en dos semestres ve el total de cada semestre, y cada total coincide con la suma de sus registros contabilizados. Un registro en validación no suma al total. |
| Relacionado con | RF-006, RF-008, RNF-USA-001 |

#### RF-004 · Consulta de criterios de graduación y avance total

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Estudiante los criterios de graduación (TEA's, clases, proyecto y ADAI) y su porcentaje de avance total validado. |
| Origen | Visión del producto, alcance. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al entrar, el estudiante ve cada criterio con su estado y un porcentaje total que coincide con los requisitos validados del plan. |
| Relacionado con | RF-007, RF-008, RF-012, RNF-USA-001 |

#### RF-005 · Muestra de requisitos por cumplir

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Estudiante la lista de requisitos que aún no ha cumplido. |
| Origen | Visión del producto, problema (los estudiantes no saben qué les falta). |
| Prioridad | Imprescindible |
| Criterio de aceptación | Un requisito no cumplido aparece en la lista. Al registrarse su cumplimiento deja de aparecer. |
| Relacionado con | RF-004, RF-007 |

#### RF-006 · Registro de puntos de un estudiante

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra los puntos de un estudiante con la actividad y la cantidad de puntos, capturados por el Asistente/Comité o por el Director de Programa. |
| Origen | Visión del producto, alcance. La entrevista confirmó que el Asistente/Comité sube puntos. Que el Director de Programa también pueda capturarlos es supuesto propio. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al guardar un registro con los dos datos, este aparece en el avance del estudiante. Si falta alguno, el sistema no guarda y señala cuál falta. |
| Relacionado con | RF-003, RF-008, RF-011, RF-017, RNF-USA-002 |

#### RF-007 · Registro de cumplimiento de criterios de graduación

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra el cumplimiento de cada criterio de graduación de un estudiante (TEA's, clases, proyecto y ADAI), capturado por el Asistente/Comité o por el Director de Programa. |
| Origen | Visión del producto, alcance. Quién captura es supuesto propio, por verificar. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al registrar un criterio que no requiere validación, el porcentaje de avance validado se actualiza y el criterio deja de aparecer como por cumplir. Si el criterio requiere validación, aparece como "en validación" (RF-008). |
| Relacionado con | RF-004, RF-005, RF-008, RF-012 |

#### RF-008 · Distinción entre avance en validación y validado

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra por separado el avance en validación y el avance validado cuando la actividad requiere validación del Director de Programa. |
| Origen | Supuesto propio: resolución del conflicto entre Estudiante y Director de la Visión. La entrevista confirmó que las actividades grandes y la acreditación de materias las valida el Director de Programa. Falta definir la lista concreta de actividades que requieren validación. |
| Prioridad | Importante |
| Criterio de aceptación | Una actividad grande recién registrada aparece de inmediato como "en validación". Cuando el director la valida, pasa a "validado" y se suma al avance validado. |
| Relacionado con | RF-003, RF-006, RF-009, RF-012 |

#### RF-009 · Validación por el Director del programa del alumno

| Campo | Contenido |
|---|---|
| Descripción | El sistema permite validar las actividades grandes y la acreditación de materias de un estudiante únicamente al Director del programa al que pertenece ese estudiante. |
| Origen | Visión del producto, regla de negocio 1. La entrevista confirmó que el Asistente/Comité no valida estos casos. |
| Prioridad | Imprescindible |
| Criterio de aceptación | El Director del programa A valida una actividad de un alumno del programa A. El mismo director no puede validar la de un alumno del programa B. El Asistente/Comité no puede validar una actividad grande ni una acreditación de materia. |
| Relacionado con | RF-002, RF-008, RF-010, RF-013, RF-021 |

#### RF-010 · Invalidación de aprobaciones por el Director General

| Campo | Contenido |
|---|---|
| Descripción | El sistema permite al Director General invalidar una aprobación registrada por un Director de Programa. |
| Origen | Visión del producto, regla de negocio 1. Se decidió conservar esta regla. |
| Prioridad | Importante |
| Criterio de aceptación | Cuando el Director General invalida una aprobación, el avance del alumno se recalcula sin esa aprobación y la acción queda en la bitácora. Ningún otro rol puede invalidar aprobaciones de otros. |
| Relacionado con | RF-009, RF-013 |

#### RF-011 · Bloqueo de captura y modificación de expedientes

| Campo | Contenido |
|---|---|
| Descripción | El sistema bloquea la captura y la modificación de avances a partir de una semana después de que termina el periodo de intersemestrales. |
| Origen | Visión del producto, regla de negocio 2, reconfirmada en la entrevista del 22 de septiembre de 2026, que definió el plazo (la Visión lo dejaba como N días hábiles). |
| Prioridad | Importante |
| Criterio de aceptación | Si el periodo de intersemestrales termina el día D, desde el día D+7 un intento de registrar o modificar avance es rechazado con un mensaje. La consulta de avance sigue disponible. Antes de D+7 el registro se acepta. |
| Relacionado con | RF-006, RF-007, RF-016 |

#### RF-012 · Impedimento de "Apto para Graduación" sin 100%

| Campo | Contenido |
|---|---|
| Descripción | El sistema impide asignar el estado "Apto para Graduación" a un alumno cuyo avance validado no alcanza el 100% de los requisitos de su plan. |
| Origen | Visión del producto, regla de negocio 3. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Con un alumno al 99% de avance validado, el intento de marcarlo "Apto para Graduación" es rechazado. Con 100%, el estado se asigna. |
| Relacionado con | RF-004, RF-007, RF-008 |

#### RF-013 · Bitácora de registros, evidencias y aprobaciones

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra el usuario, la acción, el requisito afectado, la fecha y la hora de cada registro, carga de evidencia, revisión, aprobación e invalidación. |
| Origen | Visión del producto, tipo de sistema (trazabilidad). La entrevista confirmó que hay fraude y desacuerdos por falta de constancia de las evidencias. |
| Prioridad | Importante |
| Criterio de aceptación | Después de aprobar un requisito o cargar una evidencia, la bitácora muestra quién lo hizo, sobre qué requisito y cuándo. |
| Relacionado con | RF-006, RF-009, RF-010, RF-016, RF-018, RF-019, RF-025, RNF-SEG-001 |

#### RF-014 · Generación del reporte semanal de avance

| Campo | Contenido |
|---|---|
| Descripción | El sistema genera cada semana un reporte de avance de los estudiantes en formato PDF. |
| Origen | Entrevista con el director, 22 de septiembre de 2026. Reemplaza la exportación bajo demanda de la v0.1. |
| Prioridad | Importante |
| Criterio de aceptación | Cada semana existe un PDF con el avance por alumno, con las validaciones registradas hasta el momento de generarlo. |
| Relacionado con | RF-015, RF-016, RF-023, RF-024 |

#### RF-015 · Vista global de indicadores

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Director General el avance global y los indicadores de los estudiantes de todos los programas. |
| Origen | Visión del producto, necesidad del Director General. |
| Prioridad | Importante |
| Criterio de aceptación | El Director General ve el avance por programa y el total general. Los valores coinciden con los expedientes individuales. |
| Relacionado con | RF-002, RF-014, RNF-REN-002 |

#### RF-016 · Validación fuera de plazo con justificación

| Campo | Contenido |
|---|---|
| Descripción | El sistema acepta una validación posterior al bloqueo de expedientes cuando la autoriza, con una justificación por escrito, el Director del programa del alumno o el Director General. |
| Origen | Entrevista con el director, 22 de septiembre de 2026: se aceptan validaciones extemporáneas justificadas y se busca evitar penalizaciones injustificadas. Que cualquiera de los dos directores pueda autorizarla fue confirmado. |
| Prioridad | Importante |
| Criterio de aceptación | Con el expediente bloqueado, una validación sin justificación es rechazada. Con justificación y autorización de uno de los dos directores, se acepta, la justificación queda en la bitácora y el ajuste aparece en el siguiente reporte semanal. Los reportes ya emitidos no cambian. |
| Relacionado con | RF-009, RF-011, RF-013, RF-014 |

#### RF-017 · Obtención de datos desde Soy León

| Campo | Contenido |
|---|---|
| Descripción | El sistema obtiene de Soy León los eventos y actividades, sus grupos organizadores y las listas de participantes. |
| Origen | Entrevista con el director, 22 de septiembre de 2026; cierra un hueco de la Visión. La frecuencia de actualización y qué se hace con la lista de participantes (registro automático o captura asistida) están por definir. |
| Prioridad | Importante |
| Criterio de aceptación | Una actividad dada de alta en Soy León aparece en el sistema con su grupo organizador y su lista de participantes. Al registrar puntos de esa actividad, el Asistente/Comité ve la lista. |
| Relacionado con | RF-006, RF-022 |

#### RF-018 · Asociación de evidencia a un registro de puntos

| Campo | Contenido |
|---|---|
| Descripción | El sistema asocia a cada registro de puntos la evidencia que cargó el Asistente/Comité o el Director de Programa. |
| Origen | Entrevista con el director, 22 de septiembre de 2026: hay desacuerdos por falta de constancia de las evidencias. Quién la carga y el tipo de archivo son supuesto propio. |
| Prioridad | Importante |
| Criterio de aceptación | Al cargar una evidencia en un registro de puntos, esta queda visible desde ese registro. La carga queda en la bitácora. |
| Relacionado con | RF-006, RF-013, RF-019 |

#### RF-019 · Revisión de evidencias

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra la revisión de una evidencia por el Asistente/Comité con su resultado, fecha y hora. |
| Origen | Entrevista con el director, 22 de septiembre de 2026: el Asistente/Comité revisa evidencias. Los posibles resultados de la revisión son supuesto propio. |
| Prioridad | Deseable |
| Criterio de aceptación | Después de revisar una evidencia, el registro muestra quién la revisó, el resultado y cuándo. La revisión queda en la bitácora. |
| Relacionado con | RF-013, RF-018 |

#### RF-020 · Ficha del alumno

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Asistente/Comité y a los directores la ficha de cada alumno con su programa, semestre, porcentaje de avance y foto, si existe. |
| Origen | Entrevista con el director, 22 de septiembre de 2026: se confirmaron los datos visibles del alumno. |
| Prioridad | Deseable |
| Criterio de aceptación | El Director de Programa localiza a un alumno de su programa y ve los cuatro datos. Si el alumno no tiene foto, la ficha se muestra sin ella y sin error. |
| Relacionado con | RF-002, RF-004 |

#### RF-021 · Lista de actividades en validación

| Campo | Contenido |
|---|---|
| Descripción | El sistema muestra al Director de Programa la lista de actividades de los alumnos de su programa que están en validación. |
| Origen | Supuesto propio: hueco detectado al revisar la v0.1, ya que sin esta lista el director no sabe qué validar. |
| Prioridad | Imprescindible |
| Criterio de aceptación | El Director del programa A ve solo las actividades en validación de alumnos del programa A. Al validar una, deja de aparecer en la lista. |
| Relacionado con | RF-008, RF-009 |

#### RF-022 · Validación alterna de asistencia

| Campo | Contenido |
|---|---|
| Descripción | El sistema valida la asistencia de un alumno a una actividad con el resumen de la actividad o con su inscripción en Soy León cuando no aparece en la lista de participantes. |
| Origen | Entrevista con el director, 22 de septiembre de 2026. Que lo haga el Asistente/Comité o el Director de Programa es supuesto propio. |
| Prioridad | Deseable |
| Criterio de aceptación | Al registrar la asistencia indicando uno de los dos medios, esta queda validada y el medio queda en la bitácora. Sin ninguno de los dos, el sistema no la valida. |
| Relacionado con | RF-013, RF-017 |

#### RF-023 · Envío semanal automático del reporte por correo

| Campo | Contenido |
|---|---|
| Descripción | El sistema envía cada semana el reporte por correo al Director de Programa (con sus alumnos) y al Director General (con todos los alumnos). |
| Origen | Entrevista con el director, 22 de septiembre de 2026: reporte semanal por correo. Los destinatarios exactos son supuesto propio. |
| Prioridad | Deseable |
| Criterio de aceptación | Al generarse el reporte semanal, cada destinatario recibe un correo con el PDF adjunto, sin que nadie lo solicite. |
| Relacionado con | RF-014 |

#### RF-024 · Orden del reporte por rezago

| Campo | Contenido |
|---|---|
| Descripción | El sistema ordena en el reporte a los alumnos de mayor a menor rezago, entendido como el avance menor al esperado para su semestre. |
| Origen | Entrevista con el director, 22 de septiembre de 2026: destacar alumnos con mayor rezago. La definición de rezago fue confirmada. El avance esperado por semestre está por definir. |
| Prioridad | Deseable |
| Criterio de aceptación | Con tres alumnos con distinta diferencia entre su avance y el esperado, el reporte los lista de mayor a menor diferencia. |
| Relacionado con | RF-014 |

#### RF-025 · Inalterabilidad de la bitácora

| Campo | Contenido |
|---|---|
| Descripción | El sistema impide editar o borrar las entradas de la bitácora. |
| Origen | Visión del producto, tipo de sistema (trazabilidad). Separado de RF-013 porque son dos ideas. |
| Prioridad | Importante |
| Criterio de aceptación | Ningún rol, incluido el Director General, puede editar ni borrar una entrada de la bitácora desde el sistema. |
| Relacionado con | RF-013, RNF-SEG-001 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
|---|---|---|---|---|
| RNF-USA-001 | Usabilidad | Consulta de avance sin capacitación | Imprescindible | Visión |
| RNF-USA-002 | Usabilidad | Rapidez del registro de puntos | Importante | Visión |
| RNF-SEG-001 | Seguridad | Aislamiento de expedientes y funciones por rol | Imprescindible | Visión |
| RNF-CON-001 | Confiabilidad | Disponibilidad en periodos de cierre | Imprescindible | Visión |
| RNF-CON-002 | Confiabilidad | Recuperación de datos tras una falla | Imprescindible | Visión |
| RNF-REN-001 | Rendimiento | Tiempo de carga del avance de un alumno | Importante | Entrevista + supuesto propio |
| RNF-REN-002 | Rendimiento | Tiempo de carga del tablero global | Importante | Entrevista + supuesto propio |

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
| Descripción | El Asistente/Comité o el Director de Programa registra los puntos de un alumno en máximo 30 segundos. |
| Métrica | Tiempo entre la selección del alumno y la confirmación del registro, medido con al menos tres usuarios de prueba. |
| Origen | Visión del producto (preocupación de los directores por perder tiempo capturando) y entrevista (el Asistente/Comité sube los puntos). Los valores son supuesto propio. |
| Prioridad | Importante |
| Por qué importa | Si capturar es lento, se regresa a Excel, que es lo que se busca reemplazar. |
| Afecta a | RF-006 |

#### RNF-SEG-001 · Aislamiento de expedientes y funciones por rol

| Campo | Contenido |
|---|---|
| Atributo de calidad | Seguridad |
| Descripción | Ningún usuario accede a expedientes ni funciones que no correspondan a su rol. |
| Métrica | Cero accesos indebidos en las pruebas que cubren cada combinación de rol (Estudiante, Asistente/Comité, Director de Programa, Director General) y función. |
| Origen | Derivado del tipo de sistema y de la Visión (seguridad y control de acceso). Se amplía a cuatro roles por la entrevista. |
| Prioridad | Imprescindible |
| Por qué importa | Un alumno podría modificar su progreso o el de otros, y el estado de graduación dejaría de ser confiable. |
| Afecta a | RF-001, RF-002, RF-009, RF-010, RF-013, RF-016, RF-025 |

#### RNF-CON-001 · Disponibilidad en periodos de cierre

| Campo | Contenido |
|---|---|
| Atributo de calidad | Confiabilidad |
| Descripción | El sistema está disponible al menos el 99% del tiempo durante los periodos de cierre académico y de graduación. |
| Métrica | Porcentaje de tiempo disponible, medido por periodo de cierre. |
| Origen | Derivado del tipo de sistema y de la Visión (disponibilidad). El 99% es supuesto propio. |
| Prioridad | Imprescindible |
| Por qué importa | Una caída en esas fechas retrasa validaciones y la expedición de autorizaciones para graduarse. |
| Afecta a | RF-006, RF-009, RF-011, RF-016 |

#### RNF-CON-002 · Recuperación de datos tras una falla

| Campo | Contenido |
|---|---|
| Atributo de calidad | Confiabilidad |
| Descripción | Ante una falla, el sistema recupera los datos con una antigüedad máxima de 24 horas. |
| Métrica | Antigüedad máxima de los datos recuperados tras una falla simulada; se cumple con 24 horas o menos. |
| Origen | Derivado del tipo de sistema (integridad de datos). Los valores son supuesto propio. |
| Prioridad | Imprescindible |
| Por qué importa | Perder aprobaciones obliga a revalidar expedientes y reintroduce las disputas que hoy existen. |
| Afecta a | RF-006, RF-009, RF-013 |

#### RNF-REN-001 · Tiempo de carga del avance de un alumno

| Campo | Contenido |
|---|---|
| Atributo de calidad | Rendimiento |
| Descripción | El avance de un alumno se despliega en menos de tres segundos con el total de alumnos registrados. |
| Métrica | Tiempo entre la solicitud y el despliegue completo, medido con hasta 200 alumnos registrados. |
| Origen | Entrevista con el director, 22 de septiembre de 2026: el Excel actual es lento y tiene datos sin depurar. Los valores de 3 segundos y 200 alumnos son supuesto propio. |
| Prioridad | Importante |
| Por qué importa | Si el sistema tarda, los usuarios vuelven a Excel o a preguntar directamente al director. |
| Afecta a | RF-003, RF-004, RF-020 |

#### RNF-REN-002 · Tiempo de carga del tablero global

| Campo | Contenido |
|---|---|
| Atributo de calidad | Rendimiento |
| Descripción | El tablero global se despliega en menos de tres segundos con el total de alumnos registrados. |
| Métrica | Tiempo entre la solicitud y el despliegue completo, medido con hasta 200 alumnos registrados. |
| Origen | Entrevista con el director, 22 de septiembre de 2026: el Excel actual es lento. Los valores de 3 segundos y 200 alumnos son supuesto propio. |
| Prioridad | Importante |
| Por qué importa | El Director General consulta el tablero para decidir; si tarda, vuelve a pedir los datos a cada programa. |
| Afecta a | RF-015 |

---

## 5. Casos de uso

El diagrama está en `docs/diagramas/casos-de-uso.png` (editable en `casos-de-uso.drawio`). Actores: Estudiante, Asistente/Comité, Director de Programa, Director General y Soy León (sistema externo).

Los requisitos RF-001, RF-002, RF-013 y RF-025 (identificación, permisos por rol, bitácora e inalterabilidad de la bitácora) no son un objetivo de un actor, sino condiciones de todos los casos de uso, por lo que se tratan como transversales.

### 5.1 Resumen

| ID | Nombre | Actores | Requisitos que realiza |
|---|---|---|---|
| CU-01 | Consultar avance de graduación | Estudiante | RF-003, RF-004, RF-005, RF-008 |
| CU-02 | Registrar avance de un estudiante | Asistente/Comité, Director de Programa, Soy León | RF-006, RF-007, RF-008, RF-011, RF-017, RF-018, RF-020, RF-022 |
| CU-03 | Revisar evidencias de una actividad | Asistente/Comité | RF-018, RF-019 |
| CU-04 | Validar actividades de un alumno | Director de Programa, Director General | RF-008, RF-009, RF-011, RF-016, RF-018, RF-020, RF-021 |
| CU-05 | Invalidar una aprobación | Director General | RF-010 |
| CU-06 | Declarar apto para graduación a un estudiante | Director de Programa | RF-012 |
| CU-07 | Recibir reporte semanal de avance | Director de Programa, Director General | RF-014, RF-023, RF-024 |
| CU-08 | Consultar indicadores globales | Director General | RF-015 |

CU-06 se apoya en un supuesto propio: que el Director de Programa asigna el estado "Apto para Graduación", pues los requisitos no dicen quién lo hace.

### 5.2 Caso de uso principal

#### CU-04 · Validar actividades de un alumno

| Campo | Contenido |
|---|---|
| Actor principal | Director de Programa |
| Actor secundario | Director General (solo en el flujo alterno 6b) |
| Objetivo | Oficializar en el avance de un alumno de su programa las actividades grandes o acreditaciones de materias que están en validación. |
| Precondición | El director está identificado con su rol y existe al menos una actividad de un alumno de su programa en validación. |
| Escenario principal | 1. El director abre la lista de actividades en validación de su programa.<br>2. El sistema muestra solo las de alumnos de su programa, con la evidencia asociada.<br>3. El director elige una actividad y abre la ficha del alumno.<br>4. El director revisa la evidencia y los datos de la actividad.<br>5. El director valida la actividad.<br>6. El sistema verifica que el expediente no esté bloqueado.<br>7. El sistema marca la actividad como validada, la suma al avance validado del alumno y la quita de la lista.<br>8. El sistema registra en la bitácora quién validó, qué y cuándo. |
| Flujos alternos | 1a. No hay actividades en validación: el sistema lo indica y el caso termina.<br>4a. La evidencia no basta: el director no valida y la actividad permanece en validación.<br>5a. La actividad es de un alumno de otro programa: el sistema rechaza la validación y no la registra.<br>6a. El expediente está bloqueado y no hay justificación: el sistema rechaza la validación.<br>6b. El expediente está bloqueado y el Director de Programa del alumno o el Director General autoriza con justificación por escrito: el sistema acepta, guarda la justificación en la bitácora y el ajuste aparece en el siguiente reporte semanal. |
| Postcondición | La actividad validada forma parte del avance validado del alumno, ya no aparece en la lista y la validación consta en la bitácora con usuario, fecha y hora. |
| Requisitos que realiza | RF-002, RF-004, RF-008, RF-009, RF-011, RF-013, RF-016, RF-018, RF-020, RF-021, RNF-SEG-001, RNF-CON-001 |

---

## 6. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo |
|---|---|---|---|
| RF-001 | Visión | Todos (transversal) | Pendiente |
| RF-002 | Visión | Todos (transversal) | Pendiente |
| RF-003 | Visión | CU-01 | Pendiente |
| RF-004 | Visión | CU-01 | Pendiente |
| RF-005 | Visión | CU-01 | Pendiente |
| RF-006 | Visión + entrevista | CU-02 | Pendiente |
| RF-007 | Visión + entrevista | CU-02 | Pendiente |
| RF-008 | Supuesto propio + entrevista | CU-01, CU-02, CU-04 | Pendiente |
| RF-009 | Visión + entrevista | CU-04 | Pendiente |
| RF-010 | Visión | CU-05 | Pendiente |
| RF-011 | Entrevista | CU-02, CU-04 | Pendiente |
| RF-012 | Visión | CU-06 | Pendiente |
| RF-013 | Visión + entrevista | Todos (transversal) | Pendiente |
| RF-014 | Entrevista | CU-07 | Pendiente |
| RF-015 | Visión | CU-08 | Pendiente |
| RF-016 | Entrevista | CU-04 | Pendiente |
| RF-017 | Entrevista | CU-02 | Pendiente |
| RF-018 | Entrevista + supuesto propio | CU-02, CU-03, CU-04 | Pendiente |
| RF-019 | Entrevista | CU-03 | Pendiente |
| RF-020 | Entrevista | CU-02, CU-04 | Pendiente |
| RF-021 | Supuesto propio | CU-04 | Pendiente |
| RF-022 | Entrevista + supuesto propio | CU-02 | Pendiente |
| RF-023 | Entrevista + supuesto propio | CU-07 | Pendiente |
| RF-024 | Entrevista | CU-07 | Pendiente |
| RF-025 | Visión | Todos (transversal) | Pendiente |

---

## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
|---|---|---|---|
| — | Todos | Primera versión (v0.1); sin cambios registrados. | — |
| 28/09/2026 | Secciones 1 y 2 | Se actualizó el alcance y se agregó el rol Asistente/Comité y un segundo conflicto entre usuarios. | Entrevista de elicitación. |
| 28/09/2026 | RF-001, RF-002 | RF-001 queda solo con la identificación; la asignación de rol se integra a RF-002, que ahora contempla cuatro roles. | Una idea por requisito; nuevo rol descubierto en la entrevista. |
| 28/09/2026 | RF-003, RF-004, RF-005 | Se aclara qué avance muestra cada uno (validado, sin incluir lo que está en validación) y se cambia "pendientes" por "por cumplir". | Ambigüedad detectada en la revisión de la v0.1: "pendiente" tenía dos significados. |
| 28/09/2026 | RF-006, RF-007 | Se indica quién captura: Asistente/Comité o Director de Programa. | Entrevista: el Asistente/Comité sube puntos. |
| 28/09/2026 | RF-008 | Se renombra "pendiente de validación" a "en validación". Sigue siendo supuesto propio, con la parte de quién valida confirmada. | Consistencia de términos y entrevista. |
| 28/09/2026 | RF-009 | Se aclara que el Asistente/Comité no valida actividades grandes ni acreditación de materias. | Supuesto confirmado en la entrevista. |
| 28/09/2026 | RF-011 | Se define el plazo del bloqueo: una semana después del periodo de intersemestrales (antes era N días hábiles sin valor). | Entrevista: se reconfirmó la regla y se obtuvo su plazo. |
| 28/09/2026 | RF-013, RF-025 | RF-013 incluye evidencias y revisiones; la inalterabilidad se separó en RF-025. | Una idea por requisito; fraude y desacuerdos por falta de constancia. |
| 28/09/2026 | RF-014, RF-023, RF-024 | El reporte bajo demanda se reemplaza por un reporte semanal en PDF (RF-014), enviado por correo (RF-023) y ordenado por rezago (RF-024). | Entrevista: el director prefiere reporte automático que destaque el rezago. |
| 28/09/2026 | RF-016 a RF-022 | Se agregan validación fuera de plazo, obtención de datos de Soy León, evidencias, ficha del alumno, lista de actividades en validación y validación alterna de asistencia. | Entrevista, y un hueco detectado en la revisión de la v0.1 (RF-021). |
| 28/09/2026 | RF-019, RF-020, RF-022, RF-023, RF-024 | Su prioridad se fijó en Deseable. RF-016, RF-017 y RF-018 se conservan como Importante. | Se reduce el alcance del prototipo: se mantiene lo que atiende los problemas centrales confirmados en la entrevista (extemporáneos, Soy León, evidencias) y el resto se pospone. |
| 28/09/2026 | RNF-USA-002, RNF-SEG-001 | Se incluye al Asistente/Comité y el cuarto rol. | Nuevo rol descubierto en la entrevista. |
| 28/09/2026 | RNF-CON-002 | Se eliminó "ningún registro se pierde" y se dejó solo la recuperación de 24 horas. | Una idea por requisito y una sola métrica comprobable. |
| 28/09/2026 | RNF-REN-001, RNF-REN-002 | Se separó en avance de un alumno y tablero global, y se agregó la entrevista al origen. | Una idea por requisito; el director confirmó que el Excel actual es lento. |
| 28/09/2026 | Secciones 5 y 6 | Se agregaron los 8 casos de uso, el caso de uso CU-04 completo y la columna de casos de uso de la trazabilidad. | Ejercicio de casos de uso de la semana 7. |

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
