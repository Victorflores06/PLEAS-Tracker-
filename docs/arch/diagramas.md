# Diagramas de Mermaid

## Diagrama de contexto
```mermaid
flowchart TD
rec([Recepcionista])
vet((Veterinario))
sis[Sistema de la clínica veterinaria]
lab[Laboratorio externo]
pago[Pasarela de pagos]
rec -->|agenda y cobra| sis
vet -->|consulta expedientes| sis
sis -->|envia estudios| lab
sis -.->|cobra consultas| pago
style sis fill:#1e88e5,color:#fff
```

## Diagrama de contenedores
```mermaid
flowchart TD
rec([Recepcionista])
subgraph sistema[Sistema de la clinica]
web[App web<br/>React]
api[API<br/>Python]
db[(Base de datos<br/>PostgreSQL)]
end
lab{{Laboratorio externo}}
rec -->|usa| web
web -->|HTTPS/JSON| api
api -->|lee y escribe| db
api ==>|SFTP| lab
style api fill:#2e7d32,color:#fff
```

## Diagrama de componentes
```mermaid
flowchart TD
web[App web]
subgraph api[API del sistema]
agenda[Agenda<br/>citas, horarios y turnos]
cobros[Cobros<br/>cuentas y pagos]
exp[Expedientes<br/>historial clinico]
notif(["Notificador<br/>interfaz de envio"])
end
db[(Base de datos)]
correo((Servicio de correo))
web --> agenda
web --> cobros
web --> exp
cobros -->|pregunta que se atendio| agenda
cobros -.->|enviarRecibo| notif
notif --> correo
agenda --> db
cobros --> db
exp --> db
style cobros fill:#8e24aa,color:#fff
```

## Diagrama de flujo con decisiones
```mermaid
flowchart TD
ini([Inicio])
val{¿Monto mayor a cero?}
exc{¿Excede el saldo?}
apl[Aplicar el abono]
err1[Monto invalido]
err2[Excede el saldo]
fin(((Fin)))
ini --> val
val -->|No| err1
val -->|Si| exc
exc -->|Si| err2
exc -->|No| apl
err1 -.-> fin
err2 -.-> fin
apl --> fin
style apl fill:#2e7d32,color:#fff
```

## Diagrama de secuencia
```mermaid
sequenceDiagram
actor Rec as Cajera
participant Web as App web
participant Cob as Cobros
participant Age as Agenda
participant BD as BD PostgreSQL
Rec->>Web: Registra un abono de 800
Web->>Cob: abonar(cuenta, 800)
Cob->>Age: Que servicio se atendio?
Age-->>Cob: Vacunacion
Cob->>BD: Guarda el pago
BD-->>Cob: Listo
Cob-->>Web: Pago registrado
Web-->>Rec: Muestra el nuevo saldo
```

## Diagrama de clases
```mermaid
classDiagram
class CuentaDePaciente {
-saldo: float
-pagos: List~Pago~
+abonar(monto) string
+consultarSaldo() float
+consultarMovimientos() List~Pago~
}
class Pago {
-monto: float
-fecha: Date
}
class Paciente {
-nombre: string
-especie: string
-peso: float
}
Paciente "1" --> "1" CuentaDePaciente : posee
CuentaDePaciente "1" *-- "*" Pago : registra
```

## Diagrama de estados
```mermaid
stateDiagram-v2
[*] --> Agendada
Agendada --> Confirmada: el dueño confirma
Agendada --> Cancelada: el dueño cancela a tiempo
Confirmada --> Atendida: llega a consulta
Confirmada --> NoAsistio: no se presenta
Confirmada --> Cancelada: cancela el mismo dia
Atendida --> [*]
Cancelada --> [*]
NoAsistio --> [*]
```

## Diagrama entidad-relacion
```mermaid
erDiagram
PACIENTE ||--o{ CITA : tiene
PACIENTE ||..|| CUENTA : posee
CITA ||--o| EXPEDIENTE : produce
CUENTA ||--|{ PAGO : registra
PACIENTE {
int id
string nombre
string especie
string dueno
}
CITA {
int id
date fecha
string motivo
}
PAGO {
int id
decimal monto
date fecha
}
```
