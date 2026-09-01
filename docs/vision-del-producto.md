# Visión del producto


---

**Autor:** Victor Manuel Flores Venegas

**Fecha de la última versión:** 01/09/2026

**Repositorio:** https://github.com/Victorflores06/PLEAS-Tracker

---

## 1. Descripción del sistema

**Nombre del sistema: TrackerPLEAS**

**Descripción:**

Es una plataforma web que funciona como un tablero digital donde los estudiantes pueden consultar en todo momento qué requisitos de se diplomado (PLEAS) ya completaron y cuáles les faltan para poder graduarse. Al mismo tiempo, permite a los directores registrar y actualizar estos avances de forma rápida, manteniendo la información sincronizada y clara para todos sin necesidad de trámites ni revisiones manuales.

---

## 2. Problema y usuarios

**El problema:**

Los estudiantes no tienen certeza de su progreso ni de los requisitos pendientes para graduarse, lo que genera confusión, desinformación y el riesgo de retrasar su graduación. Por otro lado, la dirección invierte demasiado tiempo recopilando y actualizando estos datos de forma manual para cada alumno.

**Cómo se resuelve hoy sin el sistema:**

Hoy en día, el proceso se gestiona mediante correos electrónicos, archivos de Excel desactualizados, listas en papel o citas presenciales donde el estudiante debe preguntar directamente a su director para revisar expediente por expediente cuál es su estatus actual.

**Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
|Director General de Programas | Visualizar el avance global e indicadores de todos los estudiantes a través de todos los programas de liderazgo.|Que la información no esté unificada o que no se puedan generar reportes globales de participación.|
|Director de Programa |Una herramienta ágil para actualizar y validar los requisitos cumplidos por los alumnos asignados a su programa específico. |Perder tiempo capturando datos uno por uno o cometer errores al registrar el cumplimiento de las actividades. |
|Estudiante |Consultar su porcentaje de avance en tiempo real y conocer los requisitos pendientes para completar su diplomado. |Que sus actividades no se vean reflejadas a tiempo o que la plataforma no sea clara respecto a lo que le falta. |


**Un conflicto entre usuarios:**

El Estudiante desea que cualquier requisito completado (sea una actividad pequeña o una clase) se refleje de manera inmediata en su barra de progreso para tener la certeza de que ya fue registrado. Sin embargo, el Director de Programa necesita mantener un proceso de revisión y verificación manual previa antes de autorizar y publicar el avance en las actividades más grandes, importantes o en la acreditación de materias, para garantizar que los criterios del programa realmente se cumplieron antes de hacer oficial el registro.

**Huecos encontrados:**
- Retirar responsabilidad directa de registro de la coordinación y automatizar con un API a Soy León
- Definir bien los usuarios --> Sus alcances y limitaciones
---

## 3. Alcance


### Dentro del alcance

- Muestra el avance de puntos de cada estudiante (Por semetre)
- Muestra criterios de graduación y avance total de los estudiantes (TEA's, clases, proyecto, ADAI)
- Registro de criterios de graduación de estudiantes
- Registro de puntos de estudiantes 
- Control de acceso segun el usario (Director o estudiante)
- Generación de reportes de avance (Directores)

### Explícitamente fuera del alcance

- No procesa pagos ni cuotas
- No crea ni registra actividades
- No valida avance automáticamente

**Por qué queda fuera:**

La gestión de pagos se excluye porque el sistema se enfoca únicamente en el seguimiento del avance dentro de los programas, evitando desviar el objetivo central. Además, previene la alta complejidad técnica y los riesgos de seguridad asociados al manejo de transacciones financieras. Por último, evita la duplicidad de funciones, ya que la universidad ya cuenta con infraestructura centralizada para la cobranza institucional.

## 4. Tipo de sistema y restricciones

**Tipo de sistema:**

Prototipado Rápido

**Por qué es de ese tipo:**

- Al ser un dashboard para alumnos y directores, necesitas retroalimentación constante sobre la interfaz y la usabilidad antes de codificar la lógica final.
- Permite ajustar vistas y roles sobre la marcha sin el costo rígido de volver a documentar fases previas como exigen otros modelos.
- Garantiza que los accesos y vistas diferenciados para estudiantes y directores se ajusten exactamente a las expectativas de cada perfil
  
**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
|Usabilidad |Es crucial que la interfaz sea intuitiva para que los alumnos y directores consulten y validen el avance de forma rápida sin requerir capacitación. |Los directores rechazarían la herramienta y volverían al uso de hojas de cálculo (Excel) y correos informales. |
|Seguridad y Control de Acceso |Garantiza la privacidad de los datos académicos y restringe los permisos de modificación según el rol (Estudiante, Director de Programa, Director General). |Un alumno podría modificar su propio progreso o el de otros, invalidando la certeza del estado de graduación. |
|Disponibilidad |El sistema debe estar activo en todo momento, especialmente en fechas cercanas a los cierres de periodo académico o graduaciones. |Se generarían problemas en la validación de requisitos y retrasos en la expedición de autorizaciones para graduarse |

**Reglas de negocio que ya identifiqué:**



- **1. Jerarquía estricta de validación por rol:** Una actividad completada por un alumno solo puede ser aprobada por el Director de su programa específico, pero el Director General tiene el permiso global para invalidar o sobreescribir aprobaciones en casos excepcionales.

- **2. Cierre de periodos de actualización:** Los alumnos pueden consultar su avance en cualquier momento, pero la carga o modificación de avance se bloquea automáticamente tantos días hábiles antes de la ceremonia oficial de graduación para congelar los expedientes.
  
- **3. Prerrequisito de porcentaje para graduación:** El sistema no permite marcar a un alumno con el estado de "Apto para Graduación" a menos que la suma de todas las actividades requeridas en su plan de diplomado alcance exactamente el 100% de validación.

---

## 5. Ciclo de vida elegido

**Modelo elegido: Agil/Scrum**

**Por qué le conviene a este proyecto:**

- Los requisitos iniciales del sistema están definidos, pero al tratarse de una plataforma universitaria, surgirán ajustes según la retroalimentación de directores y alumnos.
- Los directores del programa y estudiantes si tienen la disponibilidad para revisiones, permitiendo validar avances rápidamente.
- El riesgo es moderado-bajo. Trabajar en iteraciones cortas reduce el riesgo de construir funcionalidades que no se adapten a lo que se busca.

### Alternativas descartadas

**Alternativa 1: Extreme Programming (XP)**

*Por qué la descarté:* XP se enfoca excesivamente en prácticas técnicas rigurosas. Para este proyecto, la prioridad actual es el prototipado rápido y la validación de flujos de usuario con directores y alumnos, por lo que XP agregaría una sobrecarga metodológica y técnica que no aporta valor directo a la fase actual del proyecto.

**Alternativa 2: Kanban**

*Por qué la descarté:* Kanban opera bajo un flujo de trabajo continuo y sin periodos fijos de tiempo. Este proyecto requiere de la estructura de entregables cortos (Sprints) para establecer objetivos claros, organizar la entrega incremental de prototipos y agendar sesiones de revisión periódicas con los usuarios finales.

---

## Antes de entregar

Reviso que el documento cumpla lo siguiente:

- [ ] La descripción del apartado 1 se entiende sin ser del área
- [ ] Hay al menos dos tipos de usuario con necesidades distintas
- [ ] Identifiqué un conflicto real entre usuarios
- [ ] El alcance dice qué queda fuera, no solo qué queda dentro
- [ ] Las exclusiones son específicas, no genéricas
- [ ] Identifiqué el tipo de sistema y al menos dos atributos de calidad
- [ ] Anoté al menos tres reglas de negocio no obvias
- [ ] Justifiqué el ciclo de vida contra dos alternativas descartadas
- [ ] El documento está en mi repositorio y se puede leer desde el navegador
- [ ] Borré todas las instrucciones en cursiva de la plantilla
