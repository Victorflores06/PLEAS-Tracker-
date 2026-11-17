```mermaid
classDiagram
direction TB
 
%% ---------- Modulo: Acceso y roles ----------
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
 
%% ---------- Modulo: Expediente y avance ----------
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
 
class PeriodoAcademico {
    -tipo: string
    -fechaFinIntersemestrales: Date
    +calcularFechaBloqueo() Date
    +estaAbierto(fecha: Date) bool
}
 
%% ---------- Modulo: Validacion de actividades ----------
class Actividad {
    -nombre: string
    -grande: bool
    -grupoOrganizador: string
    -participantes: List~string~
    +incluyeParticipante(estudiante: Estudiante) bool
    +esGrande() bool
}
 
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
 
%% ---------- Modulo: Integracion con Soy Leon ----------
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
 
%% ---------- Modulo: Reportes y avisos ----------
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
 
%% ---------- Relaciones ----------
Estudiante "1" --> "1" Usuario : tiene cuenta
Estudiante "*" --> "1" Programa : cursa
Programa "*" --> "1" Usuario : dirigido por
Programa "1" --> "*" CriterioGraduacion : exige
Estudiante "1" -- "1" Expediente : tiene
Expediente "1" --> "*" RegistroAvance : reune
 
CriterioGraduacion <|-- CriterioTEA
CriterioGraduacion <|-- CriterioClase
CriterioGraduacion <|-- CriterioProyecto
CriterioGraduacion <|-- CriterioADAI
 
Expediente ..> CriterioGraduacion : pregunta si esta cumplido
Expediente ..> PeriodoAcademico : consulta si esta abierto
 
RegistroAvance "*" --> "1" CriterioGraduacion : cuenta hacia
RegistroAvance "*" --> "0..1" Actividad : proviene de
RegistroAvance "*" --> "0..1" Usuario : validado por
Evidencia "*" --> "1" RegistroAvance : respalda
Evidencia "*" --> "1" Usuario : cargada por
Evidencia "*" --> "0..1" Usuario : revisada por
 
%% Observer
RegistroAvance "1" --> "*" ObservadorValidacion : notifica
Expediente ..|> ObservadorValidacion
 
%% Adapter
AdaptadorSoyLeon ..|> FuenteDeActividades
FuenteDeActividades ..> Actividad : entrega
 
Bitacora "1" *-- "*" EntradaBitacora : guarda
EntradaBitacora "*" --> "1" Usuario : realizada por
ReporteSemanal "*" --> "1" Programa : de
ReporteSemanal "1" *-- "*" LineaReporte : contiene
 
%% ---------- Notas ----------
note for CriterioGraduacion "Strategy: Expediente solo pregunta, sin importar el tipo. TEA 8, Clase 4 de las opciones del programa, Proyecto 1, ADAI 1."
note for Expediente "Apto solo con 100% (RF-012). El bloqueo no se guarda: se calcula con PeriodoAcademico. Con bloqueo, acepta cambios solo con justificacion (RF-016)."
note for EntradaBitacora "Inmutable: se crea y no vuelve a cambiar (RF-025)."
note for Bitacora "Unica puerta para registrar quien hizo que y cuando (RF-013)."
note for ReporteSemanal "Copia fija al generarse (RF-014, RF-016): por eso guarda LineaReporte y no apunta a Expediente."
note for RegistroAvance "Observer: al validar, rechazar, invalidar o reenviar avisa a sus observadores sin saber cuantos son. medioAsistencia: RESUMEN_ACTIVIDAD o INSCRIPCION."
note for AdaptadorSoyLeon "Adapter: traduce el formato de Soy Leon, que no se puede modificar, a Actividad. Si Soy Leon cambia, solo cambia esta clase."
