# 🗒️ Registro de Trabajo en Clase - Taller 1

## 📆 Fecha de la sesión
16/08/2025

## 👥 Integrantes presentes
- Andrey Esteban Conejo Gomez
- Juan Miguel Dimate
- Luis Santiago Galvis Pinilla
- Juan Andrés Lacouture Daza

## 🧠 Actividades realizadas en clase

**Descripción breve de la sesión**

- **¿Qué se discutió con el equipo?**  
  Se alineó el alcance del caso **Agendamiento de Citas Médicas** y se revisó la consistencia del flujo para evitar contradicciones entre pasos (selección, verificación de disponibilidad, confirmación y notificación). También se acordaron criterios de calidad del diagrama: claridad visual, etiquetado de compuertas (pregunta y salidas **Sí/No**), y uso correcto de **sequence flows** (dentro de un pool) y **message flows** (entre pools).

- **¿Qué decisiones de modelado se tomaron?**  
  1) **Participantes y límites**: dos *pools* — **Paciente** y **Prestador de Salud**; en este último, *lanes* para **Sistema de Citas** e **Integraciones/BD**.  
  2) **Interacciones**: intercambio Paciente–Sistema mediante **message flows** (solicitud de disponibilidad, envío de opciones, confirmación, notificación).  
  3) **Caminos del proceso**: *camino feliz* (confirmación) y *alterno* (reprogramación si no hay disponibilidad o si el paciente no acepta).  
  4) **Compuertas**: **XOR** para decisiones (p. ej., ¿hay disponibilidad?, ¿paciente confirma?) y **AND** para integraciones en paralelo (actualizar historia y preparar prefactura) con *split* y *join* del mismo tipo.  
  5) **Orden lógico**: **Confirmar → Registrar cita → Integraciones (paralelo) → Notificar al paciente**.  
  6) **Datos**: uso de **Data Stores** (Agenda/Turnos, Historia Clínica, Facturación) con **Data Associations** para lecturas/escrituras.  
  7) **Reutilización**: la reprogramación puede extraerse como **Call Activity** si se reutiliza en otros procesos (p. ej., laboratorio).

- **¿Qué herramientas se usaron (papel, pizarra, draw.io, Astah)?**  
  Se usó **draw.io (diagrams.net)** para el boceto digital del BPMN.

- **¿Qué parte del trabajo se alcanzó a desarrollar?**  
  Se definió el **mapa de proceso** y el **camino feliz**, se configuraron *pools/lanes*, compuertas principales y eventos de mensaje. Se dejó lista la estructura para detallar integraciones y asociaciones de datos.

## 🧩 Boceto inicial del modelo
> (Se adjuntará captura del diagrama preliminar en draw.io con pools/líneas de nado, compuertas y flujos de mensaje.)

## 🔁 Tareas definidas para complementar el taller

| Tarea asignada                                | Responsable                         | Fecha estimada |
|-----------------------------------------------|-------------------------------------|----------------|
| Modelado final en draw.io                      | Todos                                | 15/08          |
| Redacción del informe                          | Juan Miguel Dimate                  | 15/08          |
| Investigación y referencias (APA)              | Luis Santiago Galvis Pinilla        | 15/08          |
| Checklist BPMN 2.0 (flujos, eventos, datos)    | Juan Andrés Lacouture Daza          | 16/08          |
| Revisión por pares del diagrama                | Andrey / Juan Miguel                | 16/08          |
| Exportar a PDF/PNG y subir a GitHub            | Luis Santiago                       | 16/08          |
| Anexar “Buenas prácticas” y referencia única   | Todos                                | 16/08          |

---

## 📎 Contenido trabajado: Agendamiento de Citas Médicas (BPMN)

### Evento de inicio y fin
- **Inicio:** Evento de inicio en **Paciente** → *Solicitar Cita*.
- **Fin (éxito):** Evento de fin **“Proceso finalizado – éxito”** tras *Notificar Confirmación* al paciente.
- **Fin alterno:** Evento de fin **“Proceso de reprogramación”** cuando el paciente **no confirma** y se deriva a reprogramar.

### Actividades principales
#### Paciente
- *Solicitar Cita*
- *Escoger fecha / hora / lugar*
- *Confirmar Cita*

#### Sistema de Agendamiento
- *Ver disponibilidad*
- *Registrar cita*
- *Reprogramar* (si no hay disponibilidad)
- *Notificar fecha / hora / lugar de la cita* (al paciente)
- *Notificar confirmación* (al paciente)
- *Notificar falta de disponibilidad* (si aplica)

#### Atención al cliente
- *Atender llamada* (canal telefónico)
- *Proponer fecha / hora / lugar* (alternativa manual)

#### Historial
- *Añadir al historial* (actualización clínica)

#### Facturación
- *Generar factura* (prefactura o cargo asociado)

### Decisiones (gateways)
- **Canal:** *Digital* vs *Telefónico*.
- **Disponible:** ¿Hay disponibilidad? → **Sí** (registrar) / **No** (reprogramar o notificar falta).
- **Confirma:** ¿El paciente confirma la cita? → **Sí** (notificar confirmación y fin) / **No** (fin por reprogramación).

### Roles del proceso (lanes)
- **Paciente** (app/web)
- **Sistema de Agendamiento** (back-end)
- **Atención al cliente** (operador telefónico)
- **Historial** (sistema clínico)
- **Facturación** (sistema de cobro)

### Buenas prácticas BPMN aplicadas (una sola fuente, citas APA)
- **Separar participantes en *pools* y comunicar con *message flows*.**  
  Las **sequence flows** solo conectan elementos dentro del mismo *pool*; la interacción entre participantes se modela con **message flows** (Camunda, s. f.).

- **Decisiones con XOR y trabajo concurrente con AND (emparejar *split* y *join*).**  
  Usa **Exclusive Gateway** para rutas mutuamente excluyentes y **Parallel Gateway** para paralelismo; cuando abras caminos con **Parallel**, normalmente se sincronizan con otro **Parallel** (Camunda, s. f.).

- **Interacciones y notificaciones como eventos/tareas de mensaje.**  
  Entrega de opciones, confirmaciones y avisos deben representarse con **Message Events** (catch/throw) o **Send/Receive Task**, no como tareas genéricas (Camunda, s. f.).

- **Datos persistentes vs. transitorios.**  
  Repositorios que existen más allá del proceso (agenda, historia clínica, facturación) → **Data Store** con **Data Associations**; documentos/variables del flujo → **Data Objects** (Camunda, s. f.).

- **Reutilización con *Call Activity*.**  
  Subprocesos repetibles (p. ej., reprogramación) conviene extraerlos y reutilizarlos mediante **Call Activity** (Camunda, s. f.).

- **Subprocesos y eventos.**  
  Considera **Subprocess** (colapsado/expandido) y **Event Subprocess** para manejar excepciones/escenarios alternos sin romper el flujo principal (Camunda, s. f.).

#### Referencia (APA)
Camunda. (s. f.). *BPMN 2.0 Symbols — A complete guide with examples*. https://camunda.com/bpmn/reference/

---

_Este documento resume el trabajo colaborativo realizado durante la sesión del Taller 1 en el curso AREM - Universidad de La Sabana._
