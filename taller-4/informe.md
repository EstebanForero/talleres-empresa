# Informe técnico - Taller 4

- **Curso:** Arquitectura Empresarial - Universidad de La Sabana
- **Taller:** Mapa de Infraestructura y Diagnóstico Técnico
- **Fecha:** 16 de abril de 2026

## Integrantes
- Carlos David Cruz Pavas (`CarlosDaCruz`)
- Juan Felipe Cepeda Uribe
- Esteban Fernando Forero Montejo (`EstebanForero`)

## Contexto del cliente
El cliente real es la **Dirección de Desarrollo Estratégico de la Universidad de La Sabana**. El proceso estudiado corresponde a la actualización de la **Encuesta de Autoevaluación Institucional por Programas**, donde los lineamientos del CNA se revisan contra un histórico en Excel, se consolidan cambios y luego se envían a un proveedor externo que construye los instrumentos y entrega enlaces para aplicación.

En este caso, la infraestructura no debe entenderse como servidores y microservicios, sino como la **arquitectura operativa y tecnológica** que soporta el flujo: documentos normativos, archivos de trabajo, almacenamiento compartido, revisión humana y plataforma del proveedor.

## Objetivo del análisis
Modelar la infraestructura lógica actual del proceso e identificar riesgos potenciales de continuidad, trazabilidad, pérdida de información y dependencia operativa.

## Diagrama de infraestructura

<img width="1387" height="497" alt="image" src="https://github.com/user-attachments/assets/cdc25122-bf84-4607-83db-8b6bdd00d4df" />


### Componentes observados

| Capa | Componente | Función |
|------|------------|---------|
| Fuente normativa | Lineamientos del CNA | Definen aspectos y criterios que deben traducirse a preguntas |
| Base histórica | Excel consolidado histórico | Sirve como referencia comparativa entre periodos |
| Herramienta central | Excel de trabajo | Contiene la versión en edición del banco de preguntas |
| Colaboración y almacenamiento | OneDrive | Aloja y sincroniza el archivo entre usuarios internos |
| Control interno | Analista responsable y jefe | Revisan cambios, validan consistencia y autorizan envío |
| Tercero | Plataforma del proveedor | Carga el archivo aprobado, genera encuestas, enlaces y reportes |
| Salidas | Links y reportes comparativos | Resultado operativo del proceso |

### Flujo principal
1. El CNA publica o actualiza lineamientos.
2. El equipo compara manualmente el PDF con la versión histórica.
3. Se agregan, modifican o eliminan preguntas en Excel.
4. El archivo se comparte y sincroniza por OneDrive.
5. Se revisa internamente la versión consolidada.
6. Se envía el archivo final al proveedor externo.
7. El proveedor genera encuestas y enlaces.
8. El equipo valida manualmente los enlaces y la estructura final.

## Diagnóstico técnico

### Hallazgo 1. Concentración operativa en un archivo maestro
**Evidencia observada:** el flujo depende de un Excel consolidado como medio principal de edición, comparación y entrega.

**Riesgo potencial:** si la versión vigente se sobrescribe, se edita incorrectamente o se pierde contexto sobre qué archivo fue aprobado, el proceso completo se ve afectado.

**Impacto:** reproceso, pérdida de tiempo y dificultad para demostrar trazabilidad entre versiones.

**Matiz técnico:** OneDrive sí ofrece mecanismos útiles de recuperación, como historial de versiones, restauración de versiones previas y recuperación desde la papelera. Sin embargo, esos mecanismos no sustituyen un esquema formal de aprobación semántica, control de cambios por pregunta ni evidencia de aceptación interna.

### Hallazgo 2. Dependencia alta del trabajo manual
**Evidencia observada:** la comparación PDF vs Excel y la validación de enlaces se hacen manualmente.

**Riesgo potencial:** a mayor número de aspectos CNA, públicos y preguntas, aumenta la probabilidad de omisión o error humano.

**Impacto:** inconsistencias entre lineamiento, banco de preguntas e instrumento final.

### Hallazgo 3. Trazabilidad frágil entre periodos
**Evidencia observada:** el negocio necesita distinguir preguntas nuevas, modificadas y eliminadas para mantener comparabilidad histórica.

**Riesgo potencial:** si el registro del cambio depende solo de marcas manuales en Excel, el seguimiento de decisiones puede quedar incompleto.

**Impacto:** menor capacidad para justificar diferencias entre periodos y para auditar cómo una exigencia del CNA terminó reflejada en una pregunta concreta.

### Hallazgo 4. Dependencia del proveedor en una etapa crítica
**Evidencia observada:** la universidad no publica directamente la encuesta final; depende del proveedor para convertir el archivo en instrumentos, links y reportes.

**Riesgo potencial:** errores de configuración o interpretación pueden aparecer fuera del entorno interno y detectarse tarde.

**Impacto:** enlaces incorrectos, ajustes repetidos y retrasos en la salida final.

### Hallazgo 5. Continuidad limitada por ausencia de un esquema formal de recuperación del proceso
**Evidencia observada:** el caso describe sincronización y uso compartido, pero no evidencia un procedimiento formal de recuperación, reversión o continuidad.

**Riesgo potencial:** ante corrupción del archivo, eliminación accidental o indisponibilidad temporal del tercero, el equipo puede quedar sin una ruta operativa clara.

**Impacto:** interrupción del ciclo de actualización y dependencia de respuestas ad hoc.

## Cuellos de botella

| Punto | Causa principal | Efecto |
|------|------------------|--------|
| Comparación inicial | Revisión manual de PDF contra histórico | Tiempo alto y posibilidad de omisiones |
| Consolidación de cambios | Actualización fila por fila en Excel | Baja velocidad y control frágil |
| Revisión interna | Concentración en pocas personas | Dependencia de conocimiento tácito |
| Entrega al proveedor | Intercambio por archivo aprobado | Riesgo de interpretación y retrabajo |
| Verificación final | Prueba manual de links e instrumentos | Aseguramiento costoso y repetitivo |

## Sustento técnico de las recomendaciones
- El CNA mantiene un marco normativo y actualizaciones oficiales para programas académicos; por tanto, el proceso necesita trazabilidad suficiente para demostrar cómo esos cambios se incorporan.
- Microsoft documenta que OneDrive permite recuperar versiones previas y restaurar archivos, lo que reduce parte del riesgo de pérdida, pero no resuelve por sí solo la aprobación formal del contenido.
- NIST plantea que la continuidad requiere controles preventivos, estrategias de recuperación y mantenimiento del plan, no solo almacenamiento compartido.
- Microsoft Learn define redundancia, replicación y backup como mecanismos distintos; esto refuerza que sincronización no es equivalente a respaldo suficiente para todos los escenarios.

## Recomendaciones priorizadas

### Prioridad alta
- Definir una convención obligatoria de nombres y estados para cada versión: borrador, en revisión, aprobado, enviado al proveedor.
- Crear una hoja o bitácora de control de cambios con responsable, fecha, motivo y evidencia de aprobación.
- Formalizar un checklist de envío al proveedor y otro checklist de validación de links.
- Guardar hitos de recuperación por versión aprobada, no solo sincronización continua.

### Prioridad media
- Separar en el Excel la edición operativa, el histórico y la versión aprobada para publicación.
- Establecer una matriz simple de roles: editor, revisor, aprobador, lector.
- Registrar incidencias del proveedor y correcciones sobre la misma versión entregada.

### Prioridad estructural
- Migrar progresivamente el banco de preguntas a una base de datos o repositorio estructurado con auditoría por registro.
- Generar desde esa fuente estructurada el archivo que consume el proveedor.
- Reemplazar parte de la validación manual por verificaciones semiautomáticas de estructura, públicos y convenciones.

## Conclusión
La infraestructura actual funciona, pero lo hace con una dependencia fuerte de un archivo maestro, revisión humana y coordinación con un tercero. El principal problema no es de capacidad de cómputo sino de **continuidad, trazabilidad y control de cambios**.

La mejora más valiosa no sería “migrar todo a la nube” ni rediseñar el proceso desde cero, sino introducir controles concretos sobre versiones, aprobación, recuperación y evidencia de envío. Eso reduce riesgo sin exigir una transformación tecnológica inmediata.
