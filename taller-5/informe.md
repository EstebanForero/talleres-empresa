# Informe técnico - Taller 5

- **Curso:** Arquitectura Empresarial - Universidad de La Sabana
- **Taller:** Evaluación de Seguridad con STRIDE
- **Fecha:** 16 de abril de 2026

## Integrantes
- Carlos David Cruz Pavas (`CarlosDaCruz`)
- Juan Felipe Cepeda Uribe
- Esteban Fernando Forero Montejo (`EstebanForero`)

## Contexto del cliente
El proceso analizado corresponde a la actualización de la **Encuesta de Autoevaluación Institucional por Programas**. El flujo parte de lineamientos del CNA, pasa por comparación y edición interna sobre Excel y termina con el envío a un proveedor externo que genera encuestas, enlaces y reportes.

En este caso, la seguridad se evalúa sobre un proceso documental y de transformación de información. Por eso, el valor principal del sistema no está en esconder el contenido del proceso, sino en asegurar **integridad, trazabilidad y disponibilidad suficiente** de la versión correcta del banco de preguntas.

## Flujo crítico seleccionado
1. Recepción de lineamientos CNA.
2. Comparación contra la versión histórica.
3. Edición del archivo consolidado.
4. Revisión y aprobación interna.
5. Envío al proveedor externo.
6. Generación de instrumentos y enlaces.
7. Validación final por parte del equipo interno.

La tabla STRIDE diligenciada para el cliente real se encuentra en [tabla-stride-cliente.xlsx](./tabla-stride-cliente.xlsx). Ese archivo conserva la estructura del taller en clase y adapta las amenazas al contexto real del proceso.

## Activos y clasificación básica

| Activo | Confidencialidad | Integridad | Disponibilidad | Justificación |
|--------|------------------|------------|----------------|---------------|
| Lineamientos CNA | Media | Alta | Media | Son fuente normativa del proceso; el mayor valor está en conservar su interpretación correcta |
| Excel histórico | Media | Alta | Alta | Soporta comparación entre periodos |
| Excel aprobado del nuevo ciclo | Media | Muy alta | Alta | Es la base que usa el proveedor para publicar la encuesta |
| Bitácora de cambios | Media | Muy alta | Media | Permite probar qué cambió y por qué |
| Enlaces y reportes del proveedor | Media | Alta | Alta | Son salida operativa necesaria para aplicar y revisar el instrumento |

## Fronteras de confianza

| Frontera | Qué cruza la frontera | Riesgo principal |
|----------|------------------------|------------------|
| Interna: equipo - OneDrive/Excel | edición, revisión y almacenamiento del archivo maestro | cambios incorrectos, borrados, permisos excesivos |
| Interna - externa: universidad - proveedor | archivo aprobado, instrucciones y correcciones | manipulación, interpretación errónea, pérdida de trazabilidad |
| Fuente normativa - operación interna | lineamientos del CNA traducidos a preguntas | error de interpretación o de transcripción |

## Criterio de valoración
Se usó una escala cualitativa simple:

- **Impacto alto:** afecta validez de la encuesta, comparabilidad histórica o continuidad del proceso.
- **Impacto medio:** genera retrabajo o exposición operativa limitada.
- **Probabilidad alta:** el escenario es plausible dentro del flujo actual y no existe un control fuerte que lo reduzca.
- **Riesgo crítico:** impacto alto + probabilidad alta.
- **Riesgo alto:** impacto alto con probabilidad media, o impacto medio con probabilidad alta.
- **Riesgo medio:** impacto y probabilidad moderados.

## Análisis STRIDE

| ID | Tipo STRIDE | Activo / punto | Evidencia del caso | Escenario | Impacto | Probabilidad | Riesgo | Controles / mitigación aterrizada |
|----|-------------|----------------|--------------------|-----------|---------|--------------|--------|-----------------------------------|
| T1 | Spoofing | Acceso al archivo maestro en OneDrive | El archivo se comparte entre usuarios internos y concentra valor operativo | Un tercero o usuario no autorizado usa credenciales ajenas y edita la versión vigente | Alto | Media | Alto | MFA, revisión trimestral de permisos, carpeta separada para versión aprobada y registro de quién puede editar |
| T2 | Tampering | Texto, clasificación o estado de preguntas | El proceso depende de edición manual y envío posterior al proveedor | Se cambia una pregunta, su factor o su público sin seguir revisión formal | Alto | Alta | Crítico | plantilla protegida, columnas bloqueadas, revisión por pares y bitácora obligatoria por cambio |
| T3 | Repudiation | Historial de cambios | El caso remarca necesidad de trazabilidad entre periodos | Se detecta una inconsistencia y no puede demostrarse quién hizo el cambio ni por qué | Alto | Alta | Crítico | hoja de control de cambios, evidencia de aprobación y conservación de versiones por hito |
| T4 | Information disclosure | Archivo y reportes | Circulan archivos y resultados entre actores internos y un tercero | Un archivo o reporte llega a personas no autorizadas | Medio | Media | Medio | permisos mínimos, carpeta restringida, clasificación documental y control de compartición externa |
| T5 | Denial of service | Archivo maestro o acceso al tercero | El flujo depende de un archivo central y de una salida operativa externa | El archivo queda inaccesible, corrupto o el tercero no puede procesar el envío a tiempo | Alto | Media | Alto | respaldo por hitos, procedimiento de recuperación, copia congelada de la última versión aprobada y canal alterno de contacto con proveedor |
| T6 | Elevation of privilege | Roles de edición y aprobación | El caso no evidencia una segregación formal de funciones | Un editor modifica y aprueba su propia versión o alguien con permiso limitado publica cambios | Alto | Media | Alto | matriz RACI simple, separación entre editor y aprobador y evidencia explícita de aprobación final |

## Hallazgos principales

### Hallazgo 1. La integridad domina el riesgo del caso
**Evidencia:** el flujo existe para traducir correctamente lineamientos del CNA a preguntas operativas.

**Riesgo:** una encuesta técnicamente publicada pero conceptualmente incorrecta es un fallo más grave que una indiscreción menor sobre el documento.

**Impacto:** resultados inválidos, comparaciones históricas defectuosas y reproceso institucional.

### Hallazgo 2. La trazabilidad es un control de seguridad
**Evidencia:** el negocio necesita distinguir preguntas nuevas, modificadas y eliminadas.

**Riesgo:** sin rastro confiable del cambio, no se puede probar origen, decisión ni aprobación.

**Impacto:** se debilita la auditoría del proceso y aumenta la posibilidad de repudiación.

### Hallazgo 3. La frontera con el proveedor amplía la superficie de ataque
**Evidencia:** el proveedor transforma el archivo aprobado en encuestas, links y reportes.

**Riesgo:** la organización pierde control directo sobre una etapa crítica y debe confiar en una traducción correcta.

**Impacto:** errores de configuración, links incorrectos o pérdida de alineación con la versión aprobada.

## Controles recomendados

### Preventivos
- MFA y revisión de permisos de edición en OneDrive.
- Estructura fija del Excel con columnas protegidas y validaciones.
- Separación de roles entre edición, revisión y aprobación.
- Entrega al proveedor con identificador único de versión y checksum simple o evidencia de envío.

### Detectivos
- Bitácora por pregunta modificada.
- Checklist previo al envío y checklist posterior a recepción de links.
- Comparación entre versión anterior y versión nueva antes de aprobar.
- Registro de incidentes y retrabajos con el proveedor.

### Correctivos
- Copia congelada de cada versión aprobada.
- Procedimiento de reversión a la versión anterior.
- Reenvío controlado al proveedor con número de versión y causa de corrección.

## Conclusión
Aplicar STRIDE al caso real muestra que los riesgos principales no están en ataques sofisticados sobre una plataforma compleja, sino en la combinación de **edición manual, archivo maestro compartido, trazabilidad insuficiente y dependencia de un tercero**.

Por eso, la mejora de seguridad más útil no es solo tecnológica. También requiere formalizar roles, evidencias de aprobación, respaldo por hitos y control del intercambio con el proveedor.
