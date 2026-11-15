# TrackerPLEAS

**Autor:** Victor Manuel Flores Venegas
**Nombre del sistema:** TrackerPLEAS — Sistema de seguimiento del avance de graduación en los programas de liderazgo PLEAS
**Materia:** Ingeniería de Software I (SIS3407)

## Descripción

TrackerPLEAS es una plataforma web que funciona como un tablero digital donde los estudiantes pueden consultar en todo momento qué requisitos de su diplomado (PLEAS) ya completaron y cuáles les faltan para graduarse. Al mismo tiempo, permite a los directores y a su equipo de apoyo registrar, validar y dar seguimiento a estos avances de forma rápida, manteniendo la información sincronizada y clara para todos, sin depender de archivos de Excel, correos ni listas en papel.

El proyecto contempla cuatro tipos de usuario: Estudiante, Asistente/Comité, Director de Programa y Director General. Entre sus funciones principales están la consulta del avance por semestre y de los criterios de graduación (TEA's, clases, proyecto y ADAI), la validación de actividades por el director del programa, la bitácora de quién hizo qué y cuándo, la obtención de actividades y participantes desde Soy León, y los reportes semanales de avance.

## Documentación

Toda la documentación está en la carpeta [`docs`](docs). El orden de lectura recomendado es el de las tablas: primero qué se necesita, después cómo se diseñó y al final qué cambios se han pedido.

### 1. Visión y requisitos

| Documento | Qué contiene |
|---|---|
| [Visión del producto](docs/vision-del-producto.md) | Problema, usuarios, alcance, reglas de negocio y ciclo de vida elegido. |
| [Guion de entrevista](docs/guion-entrevista.md) | Preguntas y resultados de la entrevista de elicitación con un director de programa. |
| [Especificación de requisitos](docs/especificacion-requisitos.md) | Requisitos funcionales y no funcionales, casos de uso, trazabilidad hasta el diseño y aspectos pendientes de validación. |
| [Diagrama de casos de uso](docs/diagramas/casos-de-uso.drawio.png) | Imagen del diagrama. El archivo editable es [`casos-de-uso.drawio`](docs/diagramas/casos-de-uso.drawio). |

### 2. Arquitectura y diseño

Los diagramas están escritos en Mermaid y se ven directamente en GitHub.

| Documento | Qué contiene |
|---|---|
| [ADR-001 · Monolito modular](docs/arch/adr-001-monolito-modular.md) | Decisión de arquitectura: un solo programa con cinco módulos separados. |
| [Diagramas de arquitectura](docs/arch/diagramas-sistema.md) | Contexto, contenedores y componentes del sistema (modelo C4, niveles 1 a 3). |
| [Diagrama de clases](docs/arch/diagrama_clases.md) | Clases por módulo, con los patrones Strategy, Observer y Adapter. |
| [Diagrama de secuencia](docs/arch/diagrama_secuencia.md) | El estudiante consulta su avance (CU-01). |
| [Diagrama de máquina de estados](docs/arch/diagrama_estados.md) | Ciclo de vida de un registro de avance: en validación, validado, rechazado e invalidado. |

El archivo [`docs/arch/diagramas.md`](docs/arch/diagramas.md) contiene ejemplos de práctica de Mermaid con un sistema de clínica veterinaria. No describe a TrackerPLEAS.

### 3. Solicitudes de cambio

| Documento | Qué contiene |
|---|---|
| [SC-001 · Alertas de reportes no revisados](docs/cambios/sc-001-alerta-de-reportes-no-revisados.md) | Solicitud de cambio escrita por la dupla de revisión. Estado: Propuesta. |
| [AI-001 · Análisis de impacto de SC-001](docs/cambios/ai-001-alerta-de-reportes-no-revisados.md) | Impacto directo e indirecto, riesgos, alternativas, estimación y recomendación. |

### 4. Actividades entregadas

En la carpeta [`docs`](docs) están también los PDF de las actividades de clase tal como se entregaron: el paradigma del sistema, patrones y arquitectura, y el diagrama de clases. Son el registro de cada entrega; la versión vigente del diseño es la de `docs/arch`.

## Prototipo

El prototipo de TrackerPLEAS fue desarrollado en Figma y representa el caso de uso CU-04 · Validar actividades de un alumno, que es el caso de uso principal del sistema. Incluye el flujo principal y los flujos alternos del caso de uso. Los datos que muestra son ficticios.

### Prototipo navegable

[Ejecutar prototipo navegable de TrackerPLEAS](https://www.figma.com/make/cUFKCMh9fzRMolQw5REEhe/TrackerPLEAS-validaci%C3%B3n-de-actividades?fullscreen=1&t=aUAmHRf5zow5vY4L-1&code-node-id=0-6)

### Video de explicación y recorrido

[Ver video en YouTube](https://youtu.be/EH2l_ni-t-Y)

### Flujo principal del prototipo

El prototipo permite recorrer el flujo principal de validación:

1. Inicio de sesión del Director de Programa.
2. Lista de actividades en validación de su programa.
3. Detalle de la actividad, con la ficha del alumno y sus evidencias.
4. Confirmación de la validación.
5. Validación registrada, con el avance actualizado del alumno.
6. Consulta de la bitácora.

También se contemplan flujos alternos para:

* Cuando no hay actividades en validación.
* Cuando la evidencia no es suficiente y la actividad se deja en validación.
* Cuando la actividad pertenece a un alumno de otro programa y se niega el acceso.
* Cuando el expediente está bloqueado y la validación se rechaza.
* Cuando el expediente está bloqueado y se registra la validación fuera de plazo con una justificación.

## Estado del proyecto

TrackerPLEAS se encuentra en fase de diseño para la materia Ingeniería de Software I (SIS3407). La visión, la especificación de requisitos, los casos de uso y el prototipo están completos, y el diseño cuenta con la decisión de arquitectura, los diagramas C4 y los diagramas de clases, de secuencia y de máquina de estados. Todavía no hay código. La primera solicitud de cambio (SC-001) está analizada y pendiente de decisión.
