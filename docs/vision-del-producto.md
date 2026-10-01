# Visión del producto


---

**Autor:** Victor Manuel Flores Venegas

**Fecha de la última versión:** 30/09/2026

**Repositorio:** https://github.com/Victorflores06/PLEAS-Tracker

---

## 1. Descripción del sistema

**Nombre del sistema: TrackerPLEAS**

**Descripción:**

Es una plataforma web que funciona como un tablero digital donde los estudiantes pueden consultar en todo momento qué requisitos de su diplomado (PLEAS) ya completaron y cuáles les faltan para poder graduarse. Al mismo tiempo, permite a los directores y a su equipo de apoyo registrar, validar y actualizar estos avances de forma rápida, manteniendo la información sincronizada y clara para todos sin necesidad de trámites ni revisiones manuales.

---

## 2. Problema y usuarios

**El problema:**

Los estudiantes no tienen certeza de su progreso ni de los requisitos pendientes para graduarse, lo que genera confusión, desinformación y el riesgo de retrasar su graduación. Por otro lado, la dirección invierte demasiado tiempo recopilando y actualizando estos datos de forma manual para cada alumno.

**Cómo se resuelve hoy sin el sistema:**

Hoy en día, el proceso se gestiona mediante correos electrónicos, archivos de Excel lentos y con datos sin depurar, listas en papel o citas presenciales donde el estudiante debe preguntar directamente a su director para revisar expediente por expediente cuál es su estatus actual. Además, no hay constancia clara de las evidencias de cada alumno, lo que genera desacuerdos e intentos de fraude, y los cortes de actualización fuera de tiempo pueden penalizar a alumnos que sí cumplieron.

**Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
|Director General de Programas | Visualizar el avance global e indicadores de todos los estudiantes a través de todos los programas de liderazgo y recibir reportes periódicos.|Que la información no esté unificada o que no se puedan generar reportes globales de participación.|
|Director de Programa |Una herramienta ágil para validar los requisitos cumplidos por los alumnos asignados a su programa específico. |Perder tiempo capturando datos uno por uno o cometer errores al registrar el cumplimiento de las actividades. |
|Asistente/Comité |Subir puntos y revisar evidencias de las actividades de forma rápida, para liberar al director de la carga administrativa. |Capturar en una herramienta lenta o sin constancia de las evidencias que revisó. |
|Estudiante |Consultar su porcentaje de avance en tiempo real y conocer los requisitos pendientes para completar su diplomado. |Que sus actividades no se vean reflejadas a tiempo, que se le penalice por retrasos ajenos a él o que la plataforma no sea clara respecto a lo que le falta. |


**Conflictos entre usuarios:**

1. El Estudiante desea que cualquier requisito completado (sea una actividad pequeña o una clase) se refleje de manera inmediata en su barra de progreso para tener la certeza de que ya fue registrado. Sin embargo, el Director de Programa necesita mantener un proceso de revisión y verificación manual previa antes de autorizar y publicar el avance en las actividades más grandes, importantes o en la acreditación de materias, para garantizar que los criterios del programa realmente se cumplieron antes de hacer oficial el registro. Se resuelve mostrando ese avance como "en validación" hasta que el director lo valide.

2. El bloqueo de expedientes protege la consistencia de los reportes, pero puede penalizar al Estudiante cuya actividad se validó tarde por causas ajenas a él. Se resuelve aceptando validaciones fuera de plazo con una justificación autorizada por un director, lo que genera un desfase respecto a los reportes ya emitidos.

**Huecos encontrados:**
- ~~Retirar responsabilidad directa de registro de la coordinación y automatizar con un API a Soy León~~ Atendido: la integración con Soy León ya forma parte del alcance.
- ~~Definir bien los usuarios --> Sus alcances y limitaciones~~ Atendido: ahora son cuatro roles con permisos definidos en la especificación de requisitos.
---

## 3. Alcance

### Dentro del alcance

- Muestra el avance de puntos de cada estudiante (por semestre).
- Muestra los criterios de graduación y el avance total de los estudiantes (TEA's, clases, proyecto, ADAI), distinguiendo el avance validado del avance en validación.
- Registra los criterios de graduación y los puntos de los estudiantes, con la evidencia asociada.
- Obtiene de Soy León los eventos y actividades, sus grupos organizadores y las listas de participantes.
- Controla el acceso según el rol del usuario (Estudiante, Asistente/Comité, Director de Programa o Director General).
- Registra la validación de actividades por el Director de Programa, la invalidación por el Director General y las validaciones fuera de plazo con justificación.
- Bloquea la captura y modificación de expedientes una semana después del periodo de intersemestrales.
- Mantiene una bitácora de quién hizo qué y cuándo (registros, evidencias, aprobaciones e invalidaciones).
- Genera un reporte semanal de avance en PDF, lo envía por correo a los directores y destaca a los alumnos con mayor rezago.
- Muestra al Director General el avance global de todos los programas.

### Explícitamente fuera del alcance

- No procesa pagos ni cuotas.
- No crea actividades: las actividades provienen de Soy León.
- No aplica políticas o protocolos automáticamente.

**Por qué queda fuera:**

La gestión de pagos se excluye porque el sistema se enfoca únicamente en el seguimiento del avance dentro de los programas, evitando desviar el objetivo central. Además, previene la alta complejidad técnica y los riesgos de seguridad asociados al manejo de transacciones financieras. Por último, evita la duplicidad de funciones, ya que la universidad ya cuenta con infraestructura centralizada para la cobranza institucional.

La creación de actividades se excluye porque ya existe en Soy León, y TrackerPLEAS solo consume esa información para evitar capturas duplicadas.

## 4. Tipo de sistema y restricciones

**Tipo de sistema: De información y software a la medida**

**Por qué es de ese tipo:**

- Gestión de datos del diplomado: Registra información de alumnos, avances, evidencias cargadas y aprobaciones de los directores.

- Requiere usabilidad (interfaz clara para alumnos y directores), integridad de datos (que no se pierdan los registros), trazabilidad (saber quién aprobó qué requisito y cuándo) y control de acceso (diferentes permisos según el rol).

- Se integra con Soy León para obtener los eventos, actividades y listas de participantes que alimentan el seguimiento.

- La dificultad principal radica en traducir los criterios de aprobación que viven en la operación diaria de los directores hacia el sistema.

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
|Usabilidad |Es crucial que la interfaz sea intuitiva para que los alumnos y directores consulten y validen el avance de forma rápida sin requerir capacitación. |Los directores rechazarían la herramienta y volverían al uso de hojas de cálculo (Excel) y correos informales. |
|Seguridad y Control de Acceso |Garantiza la privacidad de los datos académicos y restringe los permisos de modificación según el rol (Estudiante, Asistente/Comité, Director de Programa, Director General). |Un alumno podría modificar su propio progreso o el de otros, invalidando la certeza del estado de graduación. |
|Disponibilidad |El sistema debe estar activo en todo momento, especialmente en fechas cercanas a los cierres de periodo académico o graduaciones. |Se generarían problemas en la validación de requisitos y retrasos en la expedición de autorizaciones para graduarse |

**Reglas de negocio que ya identifiqué:**


- **1. Jerarquía estricta de validación por rol:** Una actividad completada por un alumno solo puede ser aprobada por el Director de su programa específico (el Asistente/Comité no valida actividades grandes ni la acreditación de materias), pero el Director General tiene el permiso global para invalidar o sobreescribir aprobaciones en casos excepcionales.

- **2. Cierre de periodos de actualización:** Los alumnos pueden consultar su avance en cualquier momento, pero la carga o modificación de avance se bloquea automáticamente una semana después del periodo de intersemestrales para congelar los expedientes. Una validación fuera de plazo solo se acepta con una justificación por escrito, autorizada por el Director del programa del alumno o por el Director General.

- **3. Prerrequisito de porcentaje para graduación:** El sistema no permite marcar a un alumno con el estado de "Apto para Graduación" a menos que la suma de todas las actividades requeridas en su plan de diplomado alcance exactamente el 100% de validación.

- **4. Delegación operativa:** El Asistente/Comité sube los puntos y revisa las evidencias cotidianas para que el Director de Programa se concentre en validar las actividades que requieren su criterio.

---

## 5. Ciclo de vida elegido

**Modelo elegido: Ágil con enfoque de Prototipado Rápido**  

**Por qué le conviene a este proyecto:**

El desarrollo de PLEAS Tracker se beneficia de una metodología Ágil combinada con Prototipado Rápido, ya que la naturaleza del proyecto requiere validar interfaces y flujos con usuarios reales antes de congelar la arquitectura final.

Aunque los requisitos iniciales están definidos, al tratarse de una plataforma universitaria basada en roles (estudiantes, asistentes y directores), surgirán ajustes constantes en la usabilidad, el diseño del dashboard y los permisos de acceso. Trabajar en iteraciones cortas (sprints) aprovecha la disponibilidad de los alumnos y directores para realizar revisiones periódicas, permitiendo adaptar las vistas y flujos de trabajo sobre la marcha sin el costo rígido de redocumentar etapas anteriores.

Además, la creación de prototipos rápidos dentro de cada sprint reduce el riesgo de desarrollar funcionalidades o paneles que no se adapten a las expectativas de los usuarios, garantizando una entrega incremental, de bajo riesgo y alineada a las necesidades operativas reales del programa.

### Alternativas descartadas

**Alternativa 1: Extreme Programming (XP)**

*Por qué la descarté:* XP se enfoca excesivamente en prácticas técnicas rigurosas. Para este proyecto, la prioridad actual es el prototipado rápido y la validación de flujos de usuario con directores y alumnos, por lo que XP agregaría una sobrecarga metodológica y técnica que no aporta valor directo a la fase actual del proyecto.

**Alternativa 2: Modelo V**

*Por qué la descarté:* exige congelar los requisitos desde el inicio para diseñar planes de pruebas formales, lo que resulta demasiado rígido para este caso. Como el proyecto requiere validar prototipos del dashboard con directores y alumnos, este modelo dificultaría adaptar la interfaz sobre la marcha. Además, al dejar las pruebas con usuarios para las fases finales, se corre el riesgo de detectar fallos de usabilidad cuando corregirlos ya es muy costoso.

---

## Antes de entregar

Reviso que el documento cumpla lo siguiente:

- [x] La descripción del apartado 1 se entiende sin ser del área
- [x] Hay al menos dos tipos de usuario con necesidades distintas
- [x] Identifiqué un conflicto real entre usuarios
- [x] El alcance dice qué queda fuera, no solo qué queda dentro
- [x] Las exclusiones son específicas, no genéricas
- [x] Identifiqué el tipo de sistema y al menos dos atributos de calidad
- [x] Anoté al menos tres reglas de negocio no obvias
- [x] Justifiqué el ciclo de vida contra dos alternativas descartadas
- [x] El documento está en mi repositorio y se puede leer desde el navegador
- [x] Borré todas las instrucciones en cursiva de la plantilla
