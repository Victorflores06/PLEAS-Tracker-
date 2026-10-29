# ADR-001: Monolito modular en lugar de monolito simple o microservicios

## Estado

Aceptada, 29 de octubre de 2026

## Contexto

TrackerPLEAS es un sistema a la medida para la universidad, que es su único cliente y opera su propia instalación para todos los programas. Lo construye una sola persona.

El sistema tiene cinco módulos con fronteras bastante claras: acceso y roles, expediente y avance, validación de actividades, reportes y avisos, e integración con Soy León. Soy León es un sistema de la universidad que no se puede modificar y que entrega datos en su propio formato.

Las reglas de negocio dependen del rol (estudiante, director de programa, director general) y no deben quedar regadas por todo el código. Los atributos de calidad que más importan son seguridad y control de acceso, disponibilidad y usabilidad. La carga se concentra en momentos concretos: el reporte semanal en PDF, los cierres de semestre y las graduaciones.

Se consideraron tres opciones: monolito simple, monolito modular y microservicios.

## Decisión

Construimos un monolito modular: un solo programa que se despliega completo, con los cinco módulos separados por carpetas e interfaces, y con la regla de que ningún módulo lee las tablas de otro. Lo que un módulo necesita de otro se pide por su interfaz.

## Consecuencias

Hay un solo despliegue y un solo registro de errores, que es lo que una persona sí puede operar hoy. No se pagan los costos de red, transacciones distribuidas y operación múltiple que traen los microservicios.

No se puede escalar un módulo por separado. Si la generación de reportes se llega a comer los recursos cerca de los cierres, se tendrá que sacar.

Si algún día hay que separar algo, el primer módulo en salir sería Reportes y avisos: solo lee de Expediente y no escribe en ningún lado, por lo que es el más fácil de cortar. La señal concreta sería que, mientras se generan los reportes cerca de los cortes o graduaciones, la consulta de avance de los estudiantes se vuelva lenta o se caiga.

El riesgo real es que, por atajo, un módulo lea las tablas de otro y la frontera se pierda con el tiempo. Se revisa en cada cambio antes de integrarlo.
