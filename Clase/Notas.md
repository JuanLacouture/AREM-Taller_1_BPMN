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
