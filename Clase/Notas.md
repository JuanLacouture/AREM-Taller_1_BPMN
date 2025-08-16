# Agendamiento de Citas Médicas (BPMN)

## Evento de inicio y fin
- **Inicio:** Evento de inicio en **Paciente** → *Solicitar Cita*.
- **Fin (éxito):** Evento de fin **“Proceso finalizado – éxito”** tras *Notificar Confirmación* al paciente.
- **Fin alterno:** Evento de fin **“Proceso de reprogramación”** cuando el paciente **no confirma** y se deriva a reprogramar.

## Actividades principales
### Paciente
- *Solicitar Cita*
- *Escoger fecha / hora / lugar*
- *Confirmar Cita*

### Sistema de Agendamiento
- *Ver disponibilidad*
- *Registrar cita*
- *Reprogramar* (si no hay disponibilidad)
- *Notificar fecha / hora / lugar de la cita* (al paciente)
- *Notificar confirmación* (al paciente)
- *Notificar falta de disponibilidad* (si aplica)

### Atención al cliente
- *Atender llamada* (canal telefónico)
- *Proponer fecha / hora / lugar* (alternativa manual)

### Historial
- *Añadir al historial* (actualización clínica)

### Facturación
- *Generar factura* (prefactura o cargo asociado)

## Decisiones (gateways)
- **Canal:** *Digital* vs *Telefónico*.
- **Disponible:** ¿Hay disponibilidad? → **Sí** (registrar) / **No** (reprogramar o notificar falta).
- **Confirma:** ¿El paciente confirma la cita? → **Sí** (notificar confirmación y fin) / **No** (fin por reprogramación).

## Roles del proceso (lanes)
- **Paciente** (app/web)
- **Sistema de Agendamiento** (back-end)
- **Atención al cliente** (operador telefónico)
- **Historial** (sistema clínico)
- **Facturación** (sistema de cobro)

## Buenas prácticas BPMN aplicadas (una sola fuente, citas APA)

- **Separar participantes en *pools* y comunicar con *message flows*.**  
  Las **sequence flows** solo conectan elementos dentro del mismo *pool*; la interacción entre participantes se modela con **message flows** (Camunda, s. f.). 

- **Decisiones con XOR y trabajo concurrente con AND (emparejar *split* y *join*).**  
  Usa **Exclusive Gateway** para rutas mutuamente excluyentes y **Parallel Gateway** para paralelismo; cuando abras caminos con **Parallel**, normalmente se sincronizan con otro **Parallel** (Camunda, s. f.).

- **Interacciones y notificaciones como eventos/tareas de mensaje.**  
  Entrega de opciones, confirmaciones y avisos deben representarse con **Message Events** (catch/throw) o **Send/Receive Task**, no como tareas genéricas (Camunda, s. f.). 

- **Datos persistentes vs. transitorios.**  
  Repositorios que existen más allá del proceso (agenda, historia clínica, facturación) → **Data Store** con **Data Associations**; documentos/variables del flujo → **Data Objects** (Camunda, s. f.). 

- **Reutilización con *Call Activity*.**  
  Subprocesos repetibles (p. ej., reprogramación) conviene extraerlos y reutilizarlos mediante **Call Activity** (Camunda, s. f.). :contentReference

- **Subprocesos y eventos.**  
  Considera **Subprocess** (colapsado/expandido) y **Event Subprocess** para manejar excepciones/escenarios alternos sin romper el flujo principal (Camunda, s. f.). :contentReference

---

## Referencia (APA)

Camunda. (s. f.). *BPMN 2.0 Symbols — A complete guide with examples*. https://camunda.com/bpmn/reference/



