DIAGRAMA DE CLASES POR PARTES (7 laminas)
 
##### PARTE 1 de 7 · MAPA GENERAL (solo nombres) #####
```mermaid
classDiagram
direction LR
 
class Usuario
class Estudiante
class Programa
class Expediente
class CriterioGraduacion
class RegistroAvance
class Actividad
class Evidencia
class ReporteSemanal
 
Estudiante "1" --> "1" Usuario : tiene cuenta
Estudiante "*" --> "1" Programa : cursa
Estudiante "1" -- "1" Expediente : tiene
Programa "*" --> "1" Usuario : dirigido por
Programa "1" --> "*" CriterioGraduacion : exige
Expediente "1" --> "*" RegistroAvance : reune
RegistroAvance "*" --> "1" CriterioGraduacion : cuenta hacia
RegistroAvance "*" --> "0..1" Actividad : proviene de
Evidencia "*" --> "1" RegistroAvance : respalda
ReporteSemanal "*" --> "1" Programa : de
 
```
 
##### PARTE 2 de 7 · ACCESO Y BITACORA #####
 
```mermaid
classDiagram
direction LR
 
class Rol {
    <<enumeration>>
    ESTUDIANTE
    ASISTENTE_COMITE
    DIRECTOR_PROGRAMA
    DIRECTOR_GENERAL
}
 
class Usuario {
    -nombreUsuario: string
    -contrasena: string
    -rol: Rol
    +verificarCredenciales(contrasena: string) bool
    +puedeRealizar(accion: string) bool
}
 
class Bitacora {
    +registrar(usuario: Usuario, accion: string, requisito: string) void
}
 
class EntradaBitacora {
    -accion: string
    -requisitoAfectado: string
    -fechaHora: Date
}
 
class Estudiante
class Programa
 
style Estudiante fill:#eeeeee,stroke:#999999,color:#555555
style Programa fill:#eeeeee,stroke:#999999,color:#555555
 
Estudiante "1" --> "1" Usuario : tiene cuenta
Programa "*" --> "1" Usuario : dirigido por
Bitacora "1" *-- "*" EntradaBitacora : guarda
EntradaBitacora "*" --> "1" Usuario : realizada por
 
note for Bitacora "Unica puerta para registrar quien hizo que y cuando (RF-013)"
note for EntradaBitacora "Inmutable: se crea y no cambia (RF-025)"
```
 
##### PARTE 3 de 7 · EXPEDIENTE Y AVANCE #####
 
```mermaid
classDiagram
direction LR
 
class Estudiante {
    -nombre: string
    -foto: string
    -semestre: int
    +obtenerPrograma() Programa
    +obtenerFicha() string
}
 
class Programa {
    -nombre: string
    -avanceEsperado: List~double~
    +perteneceADirector(usuario: Usuario) bool
    +calcularIndicador() double
}
 
class Expediente {
    -esApto: bool
    +calcularPorcentaje() double
    +calcularAvanceEnValidacion() double
    +listarFaltantes() List~CriterioGraduacion~
    +marcarApto() bool
    +estaBloqueado() bool
    +aceptarCambio(registro: RegistroAvance, justificacion: string) bool
    +alCambiarEstado(registro: RegistroAvance) void
    +calcularPuntosPorSemestre() List~int~
}
 
class PeriodoAcademico {
    -tipo: string
    -fechaFinIntersemestrales: Date
    +calcularFechaBloqueo() Date
    +estaAbierto(fecha: Date) bool
}
 
class CriterioGraduacion
class RegistroAvance
 
style CriterioGraduacion fill:#eeeeee,stroke:#999999,color:#555555
style RegistroAvance fill:#eeeeee,stroke:#999999,color:#555555
 
Estudiante "*" --> "1" Programa : cursa
Estudiante "1" -- "1" Expediente : tiene
Programa "1" --> "*" CriterioGraduacion : exige
Expediente "1" --> "*" RegistroAvance : reune
Expediente ..> CriterioGraduacion : pregunta si esta cumplido
Expediente ..> PeriodoAcademico : consulta si esta abierto
 
note for Expediente "Apto solo con 100% (RF-012)"
note for Expediente "Con bloqueo, solo acepta cambios con justificacion (RF-016)"
note for PeriodoAcademico "El bloqueo se calcula aqui, no se guarda"
```
 
##### PARTE 4 de 7 · CRITERIOS DE GRADUACION (patron Strategy) #####
 
```mermaid
classDiagram
direction TB
 
class CriterioGraduacion {
    <<abstract>>
    -nombre: string
    -cantidadRequerida: int
    -esImportante: bool
    +estaCumplido(registros: List~RegistroAvance~)* bool
    +calcularAporte(registros: List~RegistroAvance~)* double
    +requiereValidacion() bool
}
 
class CriterioTEA {
    +estaCumplido(registros: List~RegistroAvance~) bool
    +calcularAporte(registros: List~RegistroAvance~) double
}
 
class CriterioClase {
    -opcionesDelPrograma: List~string~
    +estaCumplido(registros: List~RegistroAvance~) bool
    +calcularAporte(registros: List~RegistroAvance~) double
}
 
class CriterioProyecto {
    +estaCumplido(registros: List~RegistroAvance~) bool
    +calcularAporte(registros: List~RegistroAvance~) double
}
 
class CriterioADAI {
    +estaCumplido(registros: List~RegistroAvance~) bool
    +calcularAporte(registros: List~RegistroAvance~) double
}
 
class Programa
class Expediente
class RegistroAvance
 
style Programa fill:#eeeeee,stroke:#999999,color:#555555
style Expediente fill:#eeeeee,stroke:#999999,color:#555555
style RegistroAvance fill:#eeeeee,stroke:#999999,color:#555555
 
CriterioGraduacion <|-- CriterioTEA
CriterioGraduacion <|-- CriterioClase
CriterioGraduacion <|-- CriterioProyecto
CriterioGraduacion <|-- CriterioADAI
 
Programa "1" --> "*" CriterioGraduacion : exige
Expediente ..> CriterioGraduacion : pregunta si esta cumplido
RegistroAvance "*" --> "1" CriterioGraduacion : cuenta hacia
 
note for CriterioGraduacion "Strategy: Expediente pregunta sin saber el tipo"
note "Se requieren: TEA 8, Clase 4, Proyecto 1, ADAI 1"
```
 
##### PARTE 5 de 7 · VALIDACION DE AVANCE (patron Observer) #####
 
```mermaid
classDiagram
direction LR
 
class EstadoRegistro {
    <<enumeration>>
    EN_VALIDACION
    VALIDADO
    INVALIDADO
    RECHAZADO
}
 
class ObservadorValidacion {
    <<interface>>
    +alCambiarEstado(registro: RegistroAvance) void
}
 
class RegistroAvance {
    -puntos: int
    -estado: EstadoRegistro
    -semestre: int
    -justificacion: string
    -medioAsistencia: string
    -observadores: List~ObservadorValidacion~
    +validarPuntos() bool
    +determinarEstadoInicial() EstadoRegistro
    +validar(director: Usuario, justificacion: string) void
    +invalidar(usuario: Usuario, justificacion: string) void
    +rechazar(director: Usuario, justificacion: string) void
    +reenviar(usuario: Usuario) void
    +cuentaEnAvance() bool
    +suscribir(observador: ObservadorValidacion) void
    -notificar() void
}
 
class Evidencia {
    -archivo: string
    -fechaCarga: Date
    -fechaRevision: Date
    -resultado: string
    +registrarRevision(revisor: Usuario, resultado: string) void
}
 
class Expediente
class Usuario
 
style Expediente fill:#eeeeee,stroke:#999999,color:#555555
style Usuario fill:#eeeeee,stroke:#999999,color:#555555
 
Expediente "1" --> "*" RegistroAvance : reune
RegistroAvance "*" --> "0..1" Usuario : validado por
Evidencia "*" --> "1" RegistroAvance : respalda
Evidencia "*" --> "1" Usuario : cargada por
Evidencia "*" --> "0..1" Usuario : revisada por
RegistroAvance "1" --> "*" ObservadorValidacion : notifica
Expediente ..|> ObservadorValidacion
 
note for RegistroAvance "Observer: avisa a sus observadores sin saber cuantos son"
note for RegistroAvance "medioAsistencia: RESUMEN_ACTIVIDAD o INSCRIPCION"
```
 
##### PARTE 6 de 7 · INTEGRACION CON SOY LEON (patron Adapter) #####
 
```mermaid
classDiagram
direction LR
 
class FuenteDeActividades {
    <<interface>>
    +obtenerActividades() List~Actividad~
    +obtenerParticipantes(actividad: Actividad) List~string~
}
 
class AdaptadorSoyLeon {
    +obtenerActividades() List~Actividad~
    +obtenerParticipantes(actividad: Actividad) List~string~
    -traducirFormato(datosSoyLeon: string) Actividad
}
 
class Actividad {
    -nombre: string
    -grande: bool
    -grupoOrganizador: string
    -participantes: List~string~
    +incluyeParticipante(estudiante: Estudiante) bool
    +esGrande() bool
}
 
class RegistroAvance
 
style RegistroAvance fill:#eeeeee,stroke:#999999,color:#555555
 
AdaptadorSoyLeon ..|> FuenteDeActividades
FuenteDeActividades ..> Actividad : entrega
RegistroAvance "*" --> "0..1" Actividad : proviene de
 
note for AdaptadorSoyLeon "Adapter: traduce el formato de Soy Leon a Actividad"
note for AdaptadorSoyLeon "Si Soy Leon cambia, solo cambia esta clase"
```
 
##### PARTE 7 de 7 · REPORTES SEMANALES #####
 
```mermaid
classDiagram
direction LR
 
class ReporteSemanal {
    -fechaGeneracion: Date
    -nombre: string
    -archivoPDF: string
    +ordenarPorRezago() void
    +generarPDF() string
}
 
class LineaReporte {
    -nombreEstudiante: string
    -porcentajeAvance: double
    -rezago: double
}
 
class Programa
 
style Programa fill:#eeeeee,stroke:#999999,color:#555555
 
ReporteSemanal "*" --> "1" Programa : de
ReporteSemanal "1" *-- "*" LineaReporte : contiene
 
note for ReporteSemanal "Copia fija al generarse (RF-014, RF-016)"
note for ReporteSemanal "Por eso guarda LineaReporte y no apunta a Expediente"
```
