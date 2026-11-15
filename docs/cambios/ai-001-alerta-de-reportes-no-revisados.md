# Análisis de impacto de SC-001: Alertas de reportes no revisados vía SMS y WhatsApp

**Sistema:** TrackerPLEAS (monolito modular, software instalado)
**Autor:** Victor Manuel Flores Venegas
**Fecha del análisis:** 14-11-26

---

## Solicitud que se analiza

- **Solicitud:** `sc-001-alerta-de-reportes-no-revisados.md`
- **Quién la pidió:** la dupla de revisión, en el rol de coordinador de PLEAS.
- **Fecha de la solicitud:** 12-11-26 (estado: Propuesta).
- **Tipo de mantenimiento:** Perfectivo. Estoy de acuerdo con la clasificación: el sistema funciona hoy y el cambio lo mejora, no corrige un error ni responde a un cambio del entorno.
- **Prioridad sugerida por el solicitante:** Media.

## Traducción a términos del sistema

Lo pedido: que si un director no revisa un reporte semanal en 5 días, el sistema le mande un SMS; si pasan 3 días más sin revisión, un WhatsApp; y que avise cuando el reporte ya fue revisado.

Traducido a TrackerPLEAS:

1. **Reporte semanal** (módulo Reportes y avisos): hoy el reporte se genera y se consulta, pero no tiene ciclo de vida. Necesita un **estado de revisión** (pendiente o revisado), con la fecha de generación, la fecha de revisión y quién lo revisó.
2. **Tarea programada** dentro de Reportes y avisos que evalúa periódicamente los reportes pendientes: con 5 días sin revisión dispara el SMS; con 72 horas desde el SMS y sin revisión dispara el WhatsApp; al pasar a revisado dispara la confirmación.
3. **Interfaz de envío (nueva)**: hoy el sistema no tiene ninguna pieza que envíe mensajes fuera del correo del reporte, así que hay que crear un contrato de envío con dos implementaciones (SMS y WhatsApp). Cada una es un **Adapter** hacia un proveedor externo de mensajería que el sistema no controla, con el mismo razonamiento que ya usé para Soy León: si el proveedor cambia su formato, solo cambia el Adapter.
4. **Director de Programa** (módulo Acceso y roles): necesita un atributo de **teléfono de contacto para alertas**, además de un lugar para registrarlo y actualizarlo.
5. **Registro de alertas**: cada alerta enviada se guarda (reporte, canal, fecha, resultado), por la trazabilidad que el sistema promete en la Visión del producto.

### Puntos que no puedo traducir sin que el solicitante los aclare

1. **¿Qué significa "revisado"?** ¿Abrir el reporte, descargarlo o una acción explícita de "marcar como revisado"? Hoy el sistema no tiene ese concepto.
2. **¿Los 5 días son naturales o hábiles?** La regla de cierre de periodos del sistema se mide en días naturales (una semana después del periodo de intersemestrales, RF-011), pero la solicitud no dice cuál aplica aquí. Además, la solicitud dice "3 días" en un lugar y "72 horas" en otro.
3. **¿La confirmación es parte del cambio?** Aparece en "Qué se pide" pero no en el criterio de aceptación.
4. **¿Qué pasa en vacaciones o periodos de cierre?** Si el reporte sigue generándose, las alertas podrían dispararse cuando nadie está trabajando.

**Supuestos con los que estimo mientras no haya respuesta:** "revisado" es una acción explícita del Director de Programa; el mensaje va al teléfono del Director de Programa; los plazos son días naturales (5 días y 72 horas); la confirmación sale por el mismo canal del último aviso; y las alertas siguen corriendo en periodos de cierre.

## Impacto directo

| Artefacto | Unidad | Qué hay que cambiar |
|---|---|---|
| Requisito de generación del reporte semanal (RF-014) | 2 | Modificar: el reporte nace en estado "pendiente de revisión". |
| Requisitos nuevos (RF-028 a RF-032) | 2 | Agregar cinco: (1) El sistema registra el estado de revisión, la fecha y el director que revisó cada reporte semanal. (2) El sistema envía un SMS al teléfono registrado del Director de Programa cuando un reporte lleva 5 días sin revisarse. (3) El sistema envía un WhatsApp al mismo contacto cuando pasan 72 horas desde el SMS y el reporte sigue sin revisarse. (4) El sistema envía una confirmación al contacto cuando el reporte pendiente se revisa. (5) El sistema permite al Director de Programa registrar y actualizar su teléfono de contacto para alertas. Origen: SC-001. |
| Requisitos no funcionales nuevos (RNF-SEG-002, RNF-CON-003) | 2 | Seguridad: los mensajes de alerta no incluyen nombres ni avance de estudiantes, solo el número de reportes pendientes. Confiabilidad: si el proveedor no responde, el sistema reintenta hasta 3 veces en 1 hora, registra el fallo y no afecta la generación ni la consulta de reportes. Confiabilidad: se envía como máximo un SMS y un WhatsApp por reporte pendiente. |
| Caso de uso CU-07 · Recibir reporte semanal de avance | 2 | Modificar: el director puede marcar el reporte como revisado, y eso dispara la confirmación. |
| Casos de uso nuevos | 2 | "Recibir alerta de reporte pendiente" (actor: Director de Programa; actor secundario: proveedor de mensajería) y "Registrar contacto de alertas" (actor: Director de Programa). |
| Diagrama de clases | 4 | Reporte semanal gana estado de revisión, fecha de generación, fecha de revisión y revisado por. Clase nueva Alerta (canal, fecha de envío, resultado). Director de Programa gana teléfono de contacto. Interfaz de envío nueva con dos realizaciones: EnviadorSMS y EnviadorWhatsApp. Los plazos (5 días, 72 horas) se guardan como configuración y no escritos en el código. |
| Diagrama de secuencia | 4 | Nuevo: "Alertar reporte no revisado" (tarea programada, Reportes y avisos, interfaz de envío, proveedor). La única secuencia que existe hoy es la de CU-01 y no cambia; harían falta además las de generar el reporte (nace pendiente) y marcarlo como revisado (envía la confirmación). |
| Base de datos y prototipo | 2 y 4 | Columnas o tablas nuevas para estado de revisión, alertas y teléfono. En el prototipo, un botón o indicador de "revisado" en el reporte y un campo de teléfono en el perfil del director. |

## Impacto indirecto

| Artefacto | Unidad | Por qué se ve afectado |
|---|---|---|
| ADR-001 (monolito modular) | 3 | Sigue vigente y no se reemplaza: el cambio cabe en el monolito modular. Pero una de sus razones de ser deja de ser exacta: el ADR justifica sacar primero Reportes y avisos porque "solo lee de Expediente y solo escribe sus propios datos (los reportes ya generados)". Con el cambio, Reportes escribe además el estado de revisión y su registro de alertas (no escribe en Expediente, así que la frontera se mantiene) y además habla con un servicio externo. Hay que revisar si el ADR necesita una nota o si basta con el nuevo ADR de abajo. |
| ADR nuevo (ADR-002) | 3 | Registrar la decisión de enviar alertas mediante un proveedor externo de mensajería detrás de una interfaz de envío, con el contexto (usuario único, sin infraestructura propia de mensajería) y las consecuencias (costo por mensaje, dependencia externa). |
| Diagrama de contexto C4 (nivel 1) | 3 | Aparece un sistema externo nuevo (proveedor de mensajería) y una relación nueva: el sistema envía alertas al Director de Programa por SMS y WhatsApp. |
| Diagrama de contenedores C4 (nivel 2) | 3 | La aplicación del servidor tiene una flecha de salida nueva hacia el proveedor (HTTPS). La base de datos guarda datos nuevos. No aparece ningún contenedor nuevo si la tarea programada corre dentro de la misma aplicación. |
| Diagrama de componentes C4 (nivel 3) | 3 | Reportes y avisos gana la tarea programada y la lógica de alertas. Dependencias nuevas: Reportes y avisos pregunta a Acceso y roles por el teléfono del director, y envía mediante la nueva interfaz de envío. No se crean ciclos con Expediente ni con Validación. |
| Diagrama de paquetes | 4 | Todavía no existe en el repositorio. Cuando se haga, debe incluir la dependencia de Reportes y avisos hacia Acceso y roles y verificar que no cree un ciclo. |
| Máquina de estados | 4 | La que existe hoy es la de RegistroAvance y no cambia. Reporte semanal no tenía ciclo de vida y nace uno nuevo: Pendiente, SMS enviado, WhatsApp enviado, Revisado. El contenido del reporte sigue sin cambiar después de generarse (RF-014); lo que cambia es su estado de revisión. |
| Reglas de negocio (Visión y especificación) | 1 y 2 | Regla nueva sobre plazos de alerta. Además, el bloqueo de periodos y la validación extemporánea pueden generar reportes desfasados que se reabran: hay que definir si eso reinicia el plazo de revisión. |
| Alcance de la Visión del producto | 1 | El alcance incluye generación de reportes de avance, pero no alertas externas. Hay que agregarlas dentro del alcance, aclarando que son avisos y no aplican políticas ni sanciones (eso sigue fuera del alcance). |
| Tabla de trazabilidad y registro de cambios | 2 | Filas nuevas para cada RF nuevo (origen: SC-001) y una entrada en el registro de cambios. |
| Pruebas | 2 a 4 | Las pruebas del reporte semanal deben contemplar el estado inicial. Las pruebas por tiempo (5 días, 72 horas) exigen poder simular el reloj, y el criterio de aceptación habla de un ambiente de pruebas con un proveedor de pruebas. |
| Integración con Soy León | 3 | Sin impacto. |

## Lo que queda sin tocar

- **Integración con Soy León** y su Adapter: ni los datos ni la traducción cambian.
- **Validación de actividades** y su jerarquía por rol: el cambio no altera quién valida ni cómo.
- **Cálculo de avance y de "Apto para Graduación"** en Expediente, incluido el Strategy de criterios de graduación (TEA, clases, proyecto, ADAI).
- **Observer de la validación**: Expediente sigue enterándose igual de cada cambio de estado. No se agrega nada al evento de validación.
- **Contenido del reporte semanal**: qué datos muestra y cómo se ordena (por rezago) no cambia, solo su estado.
- **Permisos por rol**: no cambian, salvo el nuevo campo de teléfono. El Director General no recibe alertas porque la solicitud no lo pide.
- **Modelo de entrega**: sigue siendo software instalado para un solo cliente. No se introduce multi-tenancy.

## Riesgos

| Riesgo | Probabilidad | Efecto | Qué haría |
|---|---|---|---|
| "Revisado" no queda definido y se implementa algo distinto a lo que el coordinador espera | Alta | Alto: se retrabaja el cambio completo | Resolver los puntos abiertos con el solicitante antes de implementar. |
| WhatsApp exige condiciones propias (consentimiento previo del destinatario y mensajes con plantillas aprobadas, según entiendo de su política de uso empresarial; por verificar) | Media | Retraso del segundo canal | Dejar WhatsApp para una segunda fase y confirmar requisitos con el proveedor. |
| Costo recurrente por mensaje que la universidad no había considerado | Media | Alto: sin presupuesto el cambio no se puede operar | Pedir al solicitante que confirme quién paga. |
| Datos personales del director (teléfono) en un sistema nuevo y compartidos con un tercero | Media | Medio a alto | Guardar solo lo necesario, pedir consentimiento y revisar el aviso de privacidad de la universidad. |
| El proveedor falla o tarda, y el envío arrastra al módulo de reportes | Media | Alto: justo cuando más se usa el sistema | Enviar de forma aislada, con tiempo de espera y reintentos, y registrar el fallo sin detener el reporte. |
| Mensajes duplicados o excesivos (por reintentos o por varios reportes pendientes a la vez) | Media | Medio: el director empieza a ignorarlos | Un mensaje por reporte y canal, y agrupar cuando haya varios pendientes. |
| Director sin teléfono registrado o con un número desactualizado | Alta | Medio: la alerta nunca llega y nadie se entera | Definir qué pasa en ese caso (por ejemplo, avisar en el propio sistema) y registrar el intento fallido. |
| El alcance crece: piden alertas para el Director General o para estudiantes | Media | Medio | Mantener el cambio limitado a lo pedido y abrir otra solicitud si se necesita más. |
| **Riesgo de no hacerlo:** los reportes se acumulan sin revisar y dejan de servir para el seguimiento | Media | Medio | Es el problema que describe la solicitud; sin alertas se mantiene el seguimiento manual que la Visión quería retirar. |

## Alternativas consideradas

1. **Opción A: lo pedido, con un proveedor externo de mensajería detrás de una interfaz de envío nueva, en dos fases.** Primero estado de revisión y SMS; después WhatsApp y confirmación. Cuesta unas 70 horas y trae dependencia externa, costo por mensaje y trato de datos personales. Deja pendiente el contrato con el proveedor. **Me inclino por esta**, porque el problema que describe la solicitud es justamente que los canales pasivos se pierden: la Visión ya dice que hoy el seguimiento vive en correos y archivos que nadie revisa a tiempo, y otro correo se sumaría al mismo ruido.
2. **Opción B: alertas por correo institucional y aviso dentro del sistema, con los mismos plazos.** Reutiliza el canal de correo que el sistema ya usa para el reporte semanal, sin proveedor nuevo ni costo por mensaje, y cuesta unas 35 horas. Deja pendiente que no cumple con lo pedido (SMS y WhatsApp) y que el correo es el canal donde hoy ya se pierden los pendientes.
3. **Opción C: solo un indicador de "reportes pendientes" en el tablero del director, sin mensajes salientes.** Cuesta unas 15 horas y casi no agrega riesgos. Deja pendiente que es seguimiento pasivo solo si el director entra al sistema, que es el comportamiento que se quiere corregir.

La Opción A deja el envío detrás de la interfaz de envío, así que si el costo o la política del proveedor lo hacen inviable, se puede cambiar de canal (por ejemplo, a la Opción B) sin tocar Reportes.

## Estimación de esfuerzo

| Concepto | Esfuerzo |
|---|---|
| Cambios de diseño y documentación | 12 h |
| Implementación | 41 h |
| Pruebas | 17 h |
| **Total** | **70 h (rango de 55 a 90 h)** |

**De dónde sale este número.** Es una estimación por tareas, sumada de abajo hacia arriba, para una sola persona (yo construyo el sistema solo). No hay una referencia histórica contra la cual comparar, porque el sistema todavía no tiene código: por eso doy un rango y no un solo número.

- **Diseño y documentación (12 h):** requisitos y criterios de aceptación (2 h), casos de uso (1.5 h), diagrama de clases (1.5 h), dos diagramas de secuencia (2 h), tres diagramas C4 y de paquetes (1.5 h), máquina de estados (1 h), ADR nuevo y revisión del ADR-001 (1.5 h), Visión, trazabilidad y registro de cambios (1 h).
- **Implementación (41 h):** estado de revisión y base de datos (4 h), marcar revisado en el flujo del director (4 h), tarea programada con las reglas de plazo (6 h), Adapter de SMS (6 h), Adapter de WhatsApp (8 h), confirmación (3 h), reintentos y control de duplicados (5 h), pantalla del teléfono de contacto (3 h), configuración y secretos del proveedor (2 h).
- **Pruebas (17 h):** pruebas unitarias de los plazos con reloj simulado (6 h), pruebas de integración con el ambiente de pruebas del proveedor (5 h), pruebas manuales de punta a punta (4 h), revisión de privacidad y seguridad de los mensajes (2 h).

**Supuestos:** el módulo de reportes ya tiene una tarea programada (la del reporte semanal). La interfaz de envío no existe y su creación está incluida en las horas de los dos Adapters. El proveedor ofrece un ambiente de pruebas. Se aplican los supuestos de la sección de traducción.

**No incluye:** contratar al proveedor, aprobar plantillas de WhatsApp, actualizar el aviso de privacidad, el costo económico de los mensajes ni el tiempo de espera de las respuestas del solicitante. Tampoco incluye alertas para otros roles.

## Recomendación

**Aceptar, en dos fases y con condiciones previas.**

- **Condiciones antes de escribir código:** que el solicitante defina "revisado", a quién va el mensaje y los plazos (naturales u hábiles), y que confirme quién paga la mensajería y cuál es el proveedor.
- **Fase 1 (alrededor de 40 h):** estado de revisión, teléfono de contacto y SMS.
- **Fase 2 (alrededor de 30 h):** WhatsApp y confirmación.
- **Por qué aceptar:** el cambio cabe en el monolito modular sin romper ninguna frontera ni decisión registrada, ataca un problema real de seguimiento que la Visión ya nombraba, y el costo está acotado.
- **Por qué en fases:** la parte de más riesgo (WhatsApp, costo y datos personales) depende de decisiones fuera del sistema, y la Fase 1 ya da valor por sí sola.
- **Cuándo aplazar:** si no se resuelven las condiciones previas, recomiendo aplazar y, mientras tanto, entregar la Opción C, que no depende de nadie más. Se retomaría cuando haya proveedor, presupuesto y definición de "revisado".
