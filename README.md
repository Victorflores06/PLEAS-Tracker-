# TrackerPLEAS

**Autor:** Victor Manuel Flores Venegas
**Nombre del sistema:** TrackerPLEAS — Sistema de seguimiento del avance de graduación en los programas de liderazgo PLEAS

## Descripción

TrackerPLEAS es una plataforma web que funciona como un tablero digital donde los estudiantes pueden consultar en todo momento qué requisitos de su diplomado (PLEAS) ya completaron y cuáles les faltan para graduarse. Al mismo tiempo, permite a los directores registrar, validar y dar seguimiento a estos avances de forma rápida, manteniendo la información sincronizada y clara para todos, sin depender de archivos de Excel, correos ni listas en papel.

El proyecto contempla cuatro tipos de usuario: Estudiante, Asistente/Comité, Director de Programa y Director General. Entre sus funciones principales están la consulta del avance por semestre y de los criterios de graduación (TEA's, clases, proyecto y ADAI), la validación de actividades por el director del programa, la bitácora de quién hizo qué y cuándo, la obtención de actividades y participantes desde Soy León, y los reportes semanales de avance.

## Documentación

La documentación del proyecto se encuentra en la carpeta `docs` del repositorio. Entre los documentos principales se encuentran:

* [`Visión del producto`](docs/vision-del-producto.md)
* [`Guion de entrevista`](docs/guion-entrevista.md)
* [`Especificación de requisitos`](docs/especificacion-requisitos.md)

El diagrama de casos de uso y sus archivos asociados se encuentran en:

* [`docs/diagramas`](docs/diagramas)

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

TrackerPLEAS se encuentra actualmente en fase de especificación de requisitos y prototipado para la materia Ingeniería de Software I (SIS3407).
