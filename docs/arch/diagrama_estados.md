## Diagrama de máquina de estados: RegistroAvance

```mermaid
stateDiagram-v2
    %% Ciclo de vida de un RegistroAvance (modulo Validacion de actividades)
    %% Etiquetas: evento [guarda] / accion
    %% Trazabilidad: RF-006, RF-007, RF-008, RF-009, RF-010, RF-011, RF-016, RF-026, RF-027

    state requiere <<choice>>

    [*] --> requiere: registrar
    requiere --> EnValidacion: [requiere validacion]
    requiere --> Validado: [no requiere validacion] / notificar

    EnValidacion --> Validado: validar [director del programa y expediente abierto] / notificar
    EnValidacion --> Validado: validar [expediente bloqueado y con justificacion] / notificar
    EnValidacion --> Rechazado: rechazar [director del programa] / notificar

    Validado --> Invalidado: invalidar [Director General] / notificar

    Rechazado --> EnValidacion: reenviar [con nueva evidencia]
    Invalidado --> EnValidacion: reenviar

    note right of Validado
        Unico estado que cuenta en el avance.
    end note

    note left of EnValidacion
        Se muestra aparte como avance en validacion.
    end note
```
## Diagrama de máquina de estados: RegistroAvance
 
```mermaid
stateDiagram-v2
    %% Ciclo de vida de un RegistroAvance (modulo Validacion de actividades)
    %% Etiquetas: evento [guarda] / accion
    %% Trazabilidad: RF-006, RF-007, RF-008, RF-009, RF-010, RF-011, RF-016, RF-026, RF-027
    direction LR
    state requiere <<choice>>
 
    [*] --> requiere: / determinarEstadoInicial()
    requiere --> EnValidacion: [requiere validacion]
    requiere --> Validado: [no requiere validacion] / notificar
 
    EnValidacion --> Validado: validar [director del programa, con justificacion si hay bloqueo] / notificar
    EnValidacion --> Rechazado: rechazar [director del programa] / notificar
 
    Validado --> Invalidado: invalidar [Director General] / notificar
 
    Rechazado --> EnValidacion: reenviar [con nueva evidencia]
    Invalidado --> EnValidacion: reenviar
```
 
No hay estado final: un registro de avance no termina, permanece en el expediente del estudiante. Solo el estado Validado cuenta en el avance; En validación se muestra aparte.
