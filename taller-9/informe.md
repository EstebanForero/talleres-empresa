# Taller 9 - Parte 3: Dominios de analisis de riesgos

- **Curso:** Arquitectura Empresarial - Universidad de La Sabana
- **Cliente:** Direccion de Desarrollo Estrategico - Universidad de La Sabana
- **Proceso:** Actualizacion de la Encuesta de Autoevaluacion Institucional por Programas
- **Alcance:** analisis de riesgos del proceso interno de actualizacion del banco de preguntas, desde lineamientos CNA hasta entrega controlada al proveedor.

## 1. Contexto del negocio

El proceso actual transforma lineamientos del CNA en un banco de preguntas actualizado para la encuesta institucional por programas. El equipo interno compara el PDF del CNA contra un historico en Excel, identifica preguntas nuevas, modificadas o eliminadas, revisa la version consolidada y envia el resultado al proveedor externo, quien genera los enlaces de encuesta y reportes comparativos.

En este alcance, la Direccion de Desarrollo Estrategico no administra respuestas de encuestados ni datos personales. Por eso, el analisis se centra en riesgos de **cumplimiento funcional CNA, trazabilidad, continuidad, versionamiento, control interno y dependencia del proveedor**.

## 2. Criterio de valoracion

Se usa una escala cualitativa simple para priorizar riesgos:

| Nivel | Probabilidad | Impacto |
|------|--------------|---------|
| Bajo | Poco probable o controlado por practicas actuales | Afecta parcialmente el trabajo, con correccion simple |
| Medio | Puede ocurrir durante el ciclo normal del proceso | Genera reproceso, retrasos o perdida parcial de evidencia |
| Alto | Es plausible por la forma actual de trabajo | Afecta validez de la encuesta, trazabilidad o continuidad |

El nivel de riesgo se estima combinando probabilidad e impacto:

| Probabilidad / Impacto | Bajo | Medio | Alto |
|------------------------|------|-------|------|
| Baja | Bajo | Bajo | Medio |
| Media | Bajo | Medio | Alto |
| Alta | Medio | Alto | Critico |

## 3. Dominios de analisis de riesgos

### Dominio 1. Cumplimiento funcional CNA

**Pregunta guia:** ¿El proceso garantiza que los lineamientos CNA se traduzcan correctamente en preguntas?

Respuesta: parcialmente. La fuente normativa esta clara, pero la comparacion entre PDF y Excel es manual. Esto permite revisar con criterio humano, pero tambien abre espacio a omisiones, interpretaciones inconsistentes o cambios no documentados.

**Riesgo principal:** que una exigencia del CNA no quede reflejada correctamente en el banco de preguntas.

**Impacto:** encuesta incompleta, perdida de alineacion con criterios de acreditacion y dificultad para justificar el instrumento final.

**Controles esperados:** matriz de trazabilidad CNA-pregunta, revision por pares y evidencia de aprobacion de cambios.

### Dominio 2. Informacion y trazabilidad

**Pregunta guia:** ¿Se puede reconstruir que cambio, quien lo hizo, cuando y por que?

Respuesta: hoy la trazabilidad depende de marcas manuales, convenciones de color y versiones de archivo. Esto ayuda durante el trabajo operativo, pero no constituye una bitacora suficientemente auditable por pregunta.

**Riesgo principal:** perdida de evidencia sobre cambios entre periodos.

**Impacto:** dificultad para comparar versiones, explicar decisiones y responder ante revisiones internas.

**Controles esperados:** bitacora formal de cambios, identificador de version, responsable, fecha, motivo y estado de aprobacion.

### Dominio 3. Operacion y dependencia manual

**Pregunta guia:** ¿El proceso depende excesivamente de trabajo manual o conocimiento tacito?

Respuesta: si. La comparacion PDF contra historico, la consolidacion y la verificacion de enlaces se realizan manualmente. El proceso depende de pocas personas que conocen el detalle de la encuesta y sus convenciones.

**Riesgo principal:** errores por omision, cansancio, interpretacion o ausencia de responsables clave.

**Impacto:** retrasos, reproceso y menor confiabilidad del resultado.

**Controles esperados:** checklist operativo, roles definidos, revision cruzada y apoyo progresivo de una aplicacion estructurada.

### Dominio 4. Tecnologia e infraestructura

**Pregunta guia:** ¿La infraestructura actual soporta continuidad, recuperacion y control de versiones?

Respuesta: parcialmente. OneDrive ofrece sincronizacion e historial de versiones, pero el proceso sigue concentrado en un archivo Excel maestro. La arquitectura TO-BE propone una aplicacion local con base de datos ligera, como SQLite o Turso/libSQL local, sincronizada por OneDrive para evitar costos de hosting.

**Riesgo principal:** corrupcion, sobrescritura o confusion de versiones del archivo principal.

**Impacto:** interrupcion del proceso, perdida de trabajo y dificultad para identificar la version oficial.

**Controles esperados:** snapshots de versiones aprobadas, carpeta controlada, convencion de estados y procedimiento de recuperacion.

### Dominio 5. Accesos y segregacion de roles

**Pregunta guia:** ¿Esta claro quien puede editar, revisar, aprobar y consultar?

Respuesta: parcialmente. Hay participacion de analista y supervisor, pero no se evidencia una matriz formal de roles ni permisos. En el TO-BE, la aplicacion deberia autenticar usuarios y separar capacidades de edicion y aprobacion.

**Riesgo principal:** modificaciones no autorizadas o aprobaciones sin independencia suficiente.

**Impacto:** perdida de control interno y baja confianza en la version aprobada.

**Controles esperados:** matriz RACI, permisos minimos, autenticacion en la aplicacion y revision periodica de accesos.

### Dominio 6. Proveedor externo

**Pregunta guia:** ¿La entrega al proveedor y la salida recibida estan controladas?

Respuesta: parcialmente. El proveedor recibe el archivo aprobado y genera links y reportes, pero el control de version, criterios de aceptacion y evidencia de correcciones no estan plenamente formalizados.

**Riesgo principal:** que el proveedor configure una encuesta distinta a la version aprobada o que los ajustes no queden trazados.

**Impacto:** enlaces incorrectos, retrabajo y retraso en la aplicacion de la encuesta.

**Controles esperados:** entrega versionada, acta o evidencia de envio, checklist de recepcion, registro de correcciones y validacion contra la version aprobada.

### Dominio 7. Continuidad del proceso

**Pregunta guia:** ¿Existe una ruta clara si el archivo, OneDrive o el proveedor fallan?

Respuesta: no de forma suficiente. OneDrive reduce parte del riesgo tecnico, pero no reemplaza un procedimiento formal de continuidad. Tampoco se evidencia una ruta alterna si el proveedor se retrasa o falla.

**Riesgo principal:** interrupcion del ciclo de actualizacion por perdida de archivo, conflicto de sincronizacion o indisponibilidad del proveedor.

**Impacto:** retrasos en el proceso institucional y dependencia de soluciones improvisadas.

**Controles esperados:** procedimiento de recuperacion, backups por hito aprobado, responsable de contingencia y canal alterno con el proveedor.

## 4. Matriz de riesgos

| ID | Dominio | Riesgo | Causa principal | Probabilidad | Impacto | Nivel | Controles recomendados |
|----|---------|--------|-----------------|--------------|---------|-------|------------------------|
| R1 | Cumplimiento funcional CNA | Omision o mala interpretacion de un aspecto CNA | Comparacion manual PDF vs historico | Media | Alto | Alto | Matriz CNA-pregunta, revision por pares, evidencia de aprobacion |
| R2 | Informacion y trazabilidad | No poder demostrar que cambio entre periodos | Bitacora informal y marcas manuales en Excel | Alta | Alto | Critico | Bitacora por pregunta, responsable, fecha, motivo y version |
| R3 | Operacion manual | Error humano durante consolidacion o validacion | Trabajo repetitivo y dependencia de pocas personas | Alta | Medio | Alto | Checklist, revision cruzada, roles claros, validaciones semiautomaticas |
| R4 | Tecnologia e infraestructura | Perdida, corrupcion o confusion de version oficial | Archivo maestro compartido y sincronizacion continua | Media | Alto | Alto | Snapshots aprobados, versionado formal, procedimiento de recuperacion |
| R5 | Accesos y roles | Cambios no autorizados o aprobacion sin segregacion | Permisos y responsabilidades no formalizados | Media | Medio | Medio | Matriz RACI, permisos minimos, autenticacion y revision de accesos |
| R6 | Proveedor externo | Encuesta publicada no coincide con version aprobada | Handoff poco formal y validacion manual | Media | Alto | Alto | Entrega versionada, checklist de aceptacion, registro de correcciones |
| R7 | Continuidad | Retraso por falla de OneDrive, archivo o proveedor | Ausencia de plan de contingencia formal | Media | Alto | Alto | Backups por hito, ruta de recuperacion, canal alterno con proveedor |
| R8 | Arquitectura TO-BE | Conflictos de sincronizacion del archivo de base local | Uso simultaneo de archivo SQLite/Turso sincronizado por OneDrive | Media | Medio | Medio | Reglas de edicion, bloqueo logico, snapshots y validacion de integridad |

## 5. Analisis de riesgos de la arquitectura TO-BE

La arquitectura propuesta busca reducir el riesgo operativo del estado actual mediante una aplicacion local que facilite la edicion del banco de preguntas, registre cambios de forma mas trazable y sincronice el archivo de base de datos por OneDrive. Esto mejora el control frente al Excel maestro, pero introduce riesgos propios que deben gestionarse desde el diseno.

### Analisis por capas

| Capa | Funcion en la solucion | Riesgos principales | Mitigacion propuesta |
|------|------------------------|---------------------|----------------------|
| Usuario y acceso | Editores y supervisores ingresan a la app para consultar, editar o aprobar informacion | Uso de credenciales compartidas, permisos excesivos, aprobaciones sin separacion real | Autenticacion por usuario, roles editor/revisor/aprobador, MFA institucional si aplica, matriz RACI |
| Aplicacion local | Interfaz para editar preguntas, registrar cambios y generar salidas | Errores de validacion, fallas de la app, cambios incompletos o sin guardar | Validaciones obligatorias, mensajes de error claros, autoguardado controlado, pruebas antes de cada ciclo |
| Base de datos local | Repositorio estructurado del banco de preguntas, posiblemente SQLite o Turso/libSQL local | Corrupcion del archivo, bloqueo por uso concurrente, perdida de integridad referencial | Transacciones, validacion de integridad, backups antes de cambios masivos, reglas de edicion simultanea |
| Sincronizacion OneDrive | Replica el archivo de base de datos y conserva historial en la nube institucional | Conflictos de sincronizacion, version antigua sobrescribiendo version nueva, indisponibilidad temporal | Carpeta unica controlada, sincronizacion verificada antes y despues de editar, snapshots aprobados, procedimiento de restauracion |
| Trazabilidad y versionado | Permite saber que cambio, quien lo hizo, cuando y por que | Bitacora incompleta, cambios directos al archivo sin pasar por la app | Bitacora obligatoria en la app, bloqueo de edicion manual del archivo, estados de version: borrador, revision, aprobado, enviado |
| Exportacion al proveedor | Genera el archivo o paquete que consume el proveedor | Exportar una version no aprobada o con estructura incorrecta | Exportacion solo desde versiones aprobadas, identificador de version, checklist de envio, validacion contra plantilla esperada |
| Recuperacion y continuidad | Permite volver a una version aprobada si hay perdida o corrupcion | Recuperar una version equivocada o no saber cual era la ultima aprobada | Snapshots con nombre estandar, responsable de restauracion, prueba periodica de recuperacion |

### Riesgos especificos del TO-BE

| ID | Riesgo TO-BE | Impacto | Mitigacion |
|----|--------------|---------|------------|
| TB1 | Conflicto de sincronizacion del archivo de base local | Puede duplicar versiones o dejar inconsistencias entre usuarios | Definir reglas de uso, evitar edicion simultanea no controlada, revisar estado de OneDrive antes de abrir la app |
| TB2 | Corrupcion del archivo SQLite/Turso local | Puede bloquear el acceso al banco de preguntas | Backups automaticos por hito, snapshots aprobados, verificacion de integridad al iniciar la app |
| TB3 | Usuarios editan por fuera de la aplicacion | Se pierde trazabilidad y control de validaciones | Guardar el archivo en carpeta restringida, permisos minimos, uso obligatorio de la app |
| TB4 | Exportacion de una version no aprobada | El proveedor podria construir una encuesta incorrecta | Bloquear exportacion si la version no esta aprobada, incluir ID de version en el archivo enviado |
| TB5 | Fallo de autenticacion o permisos mal configurados | Usuarios pueden ver o modificar mas de lo necesario | Roles claros, revision periodica de accesos, separacion editor/aprobador |
| TB6 | Dependencia de OneDrive para sincronizacion | Trabajo retrasado si hay conflictos o indisponibilidad | Modo local temporal, procedimiento de sincronizacion posterior, canal de recuperacion desde historial |

### Plan de mitigacion

| Fase | Accion | Riesgo que reduce | Responsable sugerido |
|------|--------|-------------------|----------------------|
| Diseno | Definir roles, estados de version y flujo minimo de aprobacion | TB3, TB4, TB5 | Supervisor del proceso |
| Implementacion | Agregar bitacora obligatoria y validaciones de estructura en la app | R2, R3, TB3 | Equipo tecnico / analista responsable |
| Sincronizacion | Ubicar la base en carpeta OneDrive controlada y documentar reglas de uso | R4, R7, TB1, TB6 | Analista responsable |
| Respaldo | Crear snapshots automaticos o manuales por version aprobada | R4, R7, TB2 | Analista responsable |
| Entrega | Generar exportes solo desde versiones aprobadas con identificador unico | R6, TB4 | Analista y supervisor |
| Verificacion | Probar restauracion, exportacion y validacion antes de cada ciclo CNA | R1, R4, R6, TB2 | Supervisor del proceso |

## 6. Riesgos prioritarios

Los riesgos mas importantes para el negocio son:

1. **R2 - Perdida de trazabilidad entre periodos.** Es critico porque afecta la capacidad de justificar cambios y mantener comparabilidad historica.
2. **R1 - Omision o mala interpretacion de aspectos CNA.** Afecta directamente la validez funcional de la encuesta.
3. **R6 - Diferencia entre version aprobada y salida del proveedor.** Puede generar links incorrectos, retrabajo y retrasos.
4. **R4 - Confusion o perdida de la version oficial.** Compromete continuidad y evidencia del proceso.
5. **TB1 - Conflictos de sincronizacion en la arquitectura TO-BE.** Es un riesgo nuevo de la solucion local-first y debe controlarse con reglas de uso y snapshots.

## 7. Respuestas a las preguntas guia

| Pregunta guia | Respuesta aplicada al negocio |
|---------------|-------------------------------|
| ¿Que activos son mas criticos? | Lineamientos CNA, banco historico de preguntas, version aprobada, evidencia de cambios, links y reportes del proveedor |
| ¿Que puede salir mal? | Omisiones CNA, cambios no trazados, version equivocada, perdida de archivo, fallas de sincronizacion o salida incorrecta del proveedor |
| ¿Que tan probable es? | Medio a alto, porque el proceso actual depende de comparacion manual, Excel compartido y coordinacion con un tercero |
| ¿Cual seria el impacto? | Alto si afecta validez de la encuesta, comparabilidad historica, continuidad o confianza en la version aprobada |
| ¿Que controles existen hoy? | Revision humana, uso de OneDrive, Excel historico, validacion manual de links y comunicacion con proveedor |
| ¿Que controles faltan? | Bitacora formal, versionado por estados, matriz RACI, snapshots aprobados, checklist de proveedor y plan de recuperacion |
| ¿Quien debe gestionar el riesgo? | El analista responsable y supervisor del proceso, con apoyo del proveedor para riesgos de configuracion y entrega |

## 8. Conclusiones

El riesgo principal del proceso no esta en el tratamiento de datos personales, porque ese no hace parte del alcance interno confirmado. El riesgo central esta en construir una encuesta que no pueda demostrarse como fiel a los lineamientos CNA o que pierda trazabilidad entre periodos.

La arquitectura objetivo con aplicacion local y base sincronizada por OneDrive reduce riesgos actuales porque facilita la edicion, estructura mejor el banco de preguntas y mejora la trazabilidad. Sin embargo, no elimina la necesidad de controles: debe incluir autenticacion, roles, bitacora obligatoria, estados de version, snapshots y reglas claras para evitar conflictos de sincronizacion.

El plan de mitigacion debe tratar la solucion TO-BE como una mejora controlada: no basta con cambiar Excel por una base local. La aplicacion debe asegurar que los cambios queden registrados, que solo se exporten versiones aprobadas y que exista una forma clara de recuperar una version valida si OneDrive o el archivo local presentan problemas.
