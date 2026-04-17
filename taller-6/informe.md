# Informe técnico - Taller 6

- **Curso:** Arquitectura Empresarial - Universidad de La Sabana
- **Taller:** Checklist de Cumplimiento Normativo
- **Fecha:** 16 de abril de 2026

## Integrantes
- Carlos David Cruz Pavas (`CarlosDaCruz`)
- Juan Felipe Cepeda Uribe
- Esteban Fernando Forero Montejo (`EstebanForero`)

## Contexto del cliente
El cliente corresponde a la **Dirección de Desarrollo Estratégico de la Universidad de La Sabana**. El proceso transforma lineamientos del CNA en un banco estructurado de preguntas, lo envía a un proveedor externo y luego usa la salida para aplicación y análisis comparativo entre periodos.

Desde cumplimiento, el caso tiene dos frentes:
- **cumplimiento funcional**, porque el proceso debe reflejar correctamente los lineamientos del CNA;
- **cumplimiento de tratamiento de datos**, porque puede involucrar archivos, públicos objetivo y resultados que contienen o pueden asociarse a personas naturales.

## Normativa y criterio de evaluación

### Ley 1581 de 2012
Se tomó como base porque la SIC indica que la ley aplica a datos personales registrados en cualquier base de datos susceptible de tratamiento en territorio colombiano.

### Decreto 1377 de 2013
Se usó como apoyo para autorización, tratamiento de datos sensibles, limitación temporal del tratamiento, políticas internas y relación con encargados.

### ISO/IEC 27001:2022
Se tomó como marco de buenas prácticas, no como obligación legal directa del caso. Se usa para sustentar gobierno de acceso, continuidad, gestión de activos y control de cambios.

### Marco normativo del CNA
Se usó como fuente funcional del proceso, porque la validez del sistema depende de traducir correctamente los aspectos por evaluar a preguntas y conservar trazabilidad entre periodos.

El checklist diligenciado para el cliente real se encuentra en [checklist-cliente.xlsx](./checklist-cliente.xlsx). Ese archivo conserva la estructura del trabajo en clase y adapta los criterios al proceso real del cliente.

## Checklist de cumplimiento

| Dominio | Criterio evaluado | Fuente de referencia | Estado | Evidencia observada | Brecha / impacto |
|---------|-------------------|----------------------|--------|---------------------|------------------|
| Base normativa | La fuente oficial del proceso está identificada | CNA | Cumple | El proceso parte de lineamientos oficiales del CNA | El reto no es identificar la fuente, sino mantener su traducción correcta |
| Finalidad | La finalidad del tratamiento y uso del archivo debe estar definida | Ley 1581 / Decreto 1377 | Parcial | La finalidad operativa es clara para el equipo | No se evidencia una formalización suficiente de finalidades, responsables y usos derivados |
| Autorización | Si se tratan datos personales no exceptuados, debe existir autorización o base jurídica aplicable | Ley 1581 / Decreto 1377 | Parcial | El caso no documenta aquí el detalle de autorizaciones o bases jurídicas de cada conjunto de datos | Riesgo de tratamiento insuficientemente documentado |
| Datos sensibles | Si existen datos sensibles, su tratamiento exige reglas reforzadas | Ley 1581 / Decreto 1377 art. 6 | Parcial | El taller no confirma manejo directo de datos sensibles en el archivo maestro, pero sí advierte posible circulación de resultados o datos asociados | Debe clasificarse mejor qué datos circulan realmente |
| Trazabilidad | Debe poder demostrarse qué cambió entre versiones | CNA / control interno | Parcial | Se usan marcas manuales y comparación histórica en Excel | Trazabilidad útil pero frágil y difícil de auditar |
| Conservación | Los datos deben conservarse solo por el tiempo razonable y necesario | Decreto 1377 art. 11 | Parcial | Se conservan históricos para comparación entre periodos | No se evidencia política formal de retención, archivo y supresión |
| Políticas internas | Deben existir procedimientos y políticas consistentes con el tratamiento realizado | Decreto 1377 arts. 13, 18, 26, 27 | Parcial | El flujo existe como práctica operativa | No se evidencia en este material una política formal suficiente para todo el ciclo |
| Seguridad | Deben existir medidas de seguridad apropiadas | Decreto 1377 art. 19 / ISO 27001 | Parcial | Hay uso de OneDrive y revisión interna | Persisten riesgos de edición incorrecta, versiones confusas y continuidad limitada |
| Relación con terceros | El encargado debe actuar conforme a la finalidad y obligaciones definidas por el responsable | Decreto 1377 art. 25 | Parcial | El proveedor externo transforma el archivo y genera salidas | Debe fortalecerse la definición de entregables, controles y evidencia de aceptación |
| Derechos del titular | Deben existir medios y procedimientos para atención de solicitudes cuando aplique | Decreto 1377 arts. 18 y 23 | Parcial | No se documenta aquí cómo se atenderían solicitudes ligadas a datos personales del proceso | Brecha de formalización procedimental |
| Continuidad | Deben existir mecanismos de recuperación y preservación de evidencia | ISO 27001 / NIST | Brecha | No se observa un plan formal de recuperación o contingencia del proceso | Riesgo operativo alto ante corrupción del archivo o indisponibilidad del tercero |
| Segregación de roles | Debe evitarse que edición, revisión y aprobación dependan del mismo actor | ISO 27001 | Parcial | Hay intervención de más de una persona en el proceso | No se evidencia una matriz formal de roles y privilegios |

## Hallazgos principales

### Hallazgo 1. El cumplimiento funcional depende de la trazabilidad
**Evidencia:** el proceso necesita distinguir preguntas nuevas, modificadas y eliminadas para mantener comparabilidad histórica.

**Brecha:** la trazabilidad parece apoyarse principalmente en mecanismos manuales dentro del Excel.

**Impacto:** si la evidencia del cambio no queda clara, se debilita la capacidad de justificar cómo un aspecto CNA terminó reflejado en el instrumento final.

### Hallazgo 2. El cumplimiento legal y el operativo no están igual de maduros
**Evidencia:** el flujo operativo está claro, pero en el material revisado no aparece una política formal suficiente sobre finalidades, retención, atención de derechos y responsabilidades frente a terceros.

**Brecha:** hay práctica operativa, pero no necesariamente suficiente formalización documental.

**Impacto:** ante auditoría o revisión, el proceso puede depender demasiado de conocimiento tácito.

### Hallazgo 3. La relación con el proveedor necesita más control contractual y operativo
**Evidencia:** el proveedor recibe el archivo aprobado, genera links y reportes, y por tanto actúa sobre una etapa crítica del ciclo.

**Brecha:** en el material revisado no se evidencian criterios formales de aceptación, versionado, corrección y tratamiento de información por parte del tercero.

**Impacto:** más riesgo de retrabajo, pérdida de trazabilidad y ambigüedad sobre responsabilidades.

## Recomendaciones

### Prioridad alta
- Crear una bitácora formal de cambios por pregunta, con responsable, fecha, motivo y aprobación.
- Definir una política simple de retención: borrador, versión aprobada, histórico y reportes.
- Formalizar el intercambio con el proveedor: versión entregada, fecha, responsable, criterios de aceptación y evidencia de corrección.
- Construir una matriz RACI mínima para edición, revisión, aprobación y consulta.

### Prioridad media
- Clasificar qué datos del proceso son públicos, personales o sensibles.
- Documentar finalidad y base de tratamiento para cada conjunto de datos relevante.
- Definir procedimiento de recuperación del archivo maestro y de continuidad si el proveedor se retrasa o falla.

### Prioridad estructural
- Migrar el banco de preguntas a una solución con auditoría por registro.
- Separar mejor el repositorio histórico, la edición operativa y la salida hacia el proveedor.

## Juicio general
El proceso muestra **cumplimiento operativo parcial**: existe una fuente normativa clara, un flujo de trabajo reconocible y controles humanos de revisión. Sin embargo, persisten brechas en formalización de políticas, continuidad, relación con terceros y trazabilidad auditable.

## Conclusión
En este caso, cumplir no significa solo “tener una encuesta publicada”. Significa poder demostrar que la encuesta fue construida con base en lineamientos vigentes del CNA, que los cambios entre periodos quedaron registrados de manera verificable y que, si el proceso involucra datos personales, el tratamiento está documentado y controlado.

La prioridad más importante no es agregar más documentos, sino convertir el flujo actual en un proceso con evidencia suficiente, roles claros y controles verificables.
