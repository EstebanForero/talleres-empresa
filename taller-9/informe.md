# Analisis de Riesgos - Taller 9

## Integrantes

- Carlos David Cruz Pavas (`CarlosDaCruz`)
- Juan Felipe Cepeda Uribe
- Esteban Fernando Forero Montejo (`EstebanForero`)

## Contexto del sistema

El sistema evaluado es la aplicacion de Autoevaluacion CNA que se esta desarrollando para la Direccion de Desarrollo Estrategico de la Universidad de La Sabana. La aplicacion busca reemplazar un flujo anterior basado principalmente en archivos Excel consolidados, manejo manual de preguntas, control informal de versiones y revision externa con baja trazabilidad.

La solucion nueva centraliza la informacion en una base de datos local compatible con libSQL/Turso o SQLite, importa los consolidados de Excel, normaliza la jerarquia CNA, administra el banco de preguntas, conserva una linea base original, valida condiciones antes de exportar, genera instrumentos con comparacion de cambios y permite revisar entregas del proveedor por instrumento y audiencia.

En este alcance, el cliente no administra respuestas de encuestados ni datos personales. Por tanto, el analisis se concentra en riesgos de integridad, trazabilidad, continuidad, versionamiento, control operativo, arquitectura local y entrega al proveedor.

## Criterio de valoracion

| Nivel | Probabilidad | Impacto |
|------|--------------|---------|
| Bajo | Poco probable o controlado por practicas actuales | Afecta parcialmente el trabajo y se corrige con esfuerzo bajo |
| Medio | Puede ocurrir durante el ciclo normal del proceso | Genera reproceso, retrasos o perdida parcial de evidencia |
| Alto | Es plausible por la forma actual de trabajo | Afecta validez de la encuesta, trazabilidad, continuidad o confianza |
| Critico | Puede comprometer el estado completo del banco o la version oficial | Interrumpe el proceso o invalida entregas relevantes |

## Dominios de analisis de riesgos

### 1. Datos y modelo CNA

**Pregunta guia:** ¿La informacion del CNA y del banco de preguntas queda representada de forma consistente?

El riesgo principal es que preguntas, factores, caracteristicas o aspectos se dupliquen, se importen incompletos o queden mal normalizados desde Excel. La solucion nueva reduce este riesgo al usar base de datos, reglas de unicidad y normalizacion de jerarquia CNA, pero requiere validaciones de importacion y revision de los casos ambiguos.

### 2. Trazabilidad y baseline

**Pregunta guia:** ¿Se puede saber que cambio frente a la version original?

La trazabilidad era debil en el proceso anterior porque dependia de nombres de archivo, colores manuales y memoria del equipo. En el TO-BE, la linea base original, los hashes y los snapshots permiten comparar cambios y generar instrumentos con diferencias. El riesgo nuevo es fijar una baseline incorrecta o restaurar un snapshot equivocado.

### 3. Operacion e importacion

**Pregunta guia:** ¿El sistema evita errores al importar y preparar instrumentos?

La importacion desde Excel sigue siendo una frontera critica. Formatos distintos, celdas combinadas, columnas faltantes o jerarquias incompletas pueden contaminar la base desde el inicio. Por eso se necesitan validaciones bloqueantes, forward-fill controlado, reporte de inconsistencias y pruebas con archivos de muestra.

### 4. Persistencia y disponibilidad

**Pregunta guia:** ¿La base local puede recuperarse si falla el equipo, el archivo o la sincronizacion?

La base local mejora el control frente a multiples Excel, pero tambien concentra el estado editable. Si se corrompe o se pierde, afecta preguntas, lineamientos, baseline, snapshots y revisiones. La mitigacion debe combinar respaldos completos, snapshots por hito, sincronizacion controlada y pruebas de restauracion.

### 5. Seguridad y accesos locales

**Pregunta guia:** ¿Esta claro quien puede editar, revisar, aprobar o restaurar informacion?

La aplicacion debe autenticar usuarios y evitar que varias personas trabajen con perfiles compartidos o permisos excesivos sobre la carpeta local o sincronizada. La mitigacion incluye perfil de editor, carpeta controlada, permisos minimos, revision de accesos y confirmaciones reforzadas para operaciones destructivas.

### 6. Exportacion y revision del proveedor

**Pregunta guia:** ¿La salida entregada al proveedor coincide con la version aprobada?

El proceso anterior permitia entregar instrumentos desde archivos editados manualmente. La solucion nueva debe generar exportaciones desde la base y bloquear entregas incompletas. Tambien debe registrar la revision del proveedor por instrumento/audiencia, con estado, observacion y evidencia.

### 7. Sincronizacion cloud y continuidad

**Pregunta guia:** ¿OneDrive o Microsoft Graph reducen riesgo sin crear nuevos problemas?

La sincronizacion en la nube aporta respaldo, redundancia e historial, pero puede crear riesgos de conflicto, exposicion accidental o subida de temporales. La mitigacion es definir una carpeta de sincronizacion, permisos claros, exclusiones de temporales, backups completos y procedimiento de recuperacion.

## Riesgos de la aplicacion anterior

En la aplicacion o proceso anterior, el mayor problema no era solo tecnico. El riesgo principal era que la informacion dependia demasiado de archivos editados manualmente, criterios operativos poco controlados y conocimiento de personas especificas. Esto generaba errores dificiles de detectar, duplicados, perdida de trazabilidad y poca confianza al momento de entregar instrumentos o revisar cambios.

| Riesgo | Causa | Impacto | Probabilidad | Arquitectura afectada | Mitigacion propuesta |
|--------|-------|---------|--------------|------------------------|----------------------|
| Dependencia de Excel manuales | Los consolidados se editan directamente en archivos y no desde una fuente unica de verdad | Alto | Alta | Datos / Procesos | Centralizar el banco de preguntas en base de datos y generar exportaciones desde el sistema |
| Duplicidad de preguntas | No hay una regla tecnica fuerte que impida repetir codigos o preguntas similares | Alto | Alta | Datos | Usar unicidad por codigo de pregunta y validaciones antes de importar o exportar |
| Duplicidad de lineamientos | La jerarquia CNA puede repetirse por errores de copia, celdas combinadas o diligenciamiento incompleto | Alto | Alta | Datos / Modelo CNA | Normalizar Factor, Caracteristica y Aspecto; deduplicar por alcance y codigos CNA |
| Perdida de trazabilidad de cambios | Los archivos se modifican y circulan sin una linea base inmutable | Alto | Alta | Procesos / Gobierno TI | Definir una linea base original y comparar los cambios contra esa version |
| Versiones inconsistentes de instrumentos | Diferentes personas pueden trabajar sobre copias distintas del consolidado | Alto | Media | Procesos / Exportacion | Generar instrumentos desde la base de datos y no desde archivos editados manualmente |
| Errores por jerarquia CNA incompleta | Las celdas de Factor, Caracteristica o Aspecto pueden venir vacias por formato de Excel | Medio | Alta | Importacion / Datos | Aplicar forward-fill controlado durante la importacion |
| Falta de validacion antes de entrega | El proceso anterior puede permitir entregar instrumentos incompletos o inconsistentes | Alto | Media | Validacion / Proveedor | Crear reglas bloqueantes antes de exportar o entregar al proveedor |
| Baja auditoria de revision del proveedor | Las correcciones del proveedor se revisan sin estado formal por pregunta | Medio | Media | Provider Review / Gobierno | Registrar revision por instrumento, estado, observacion y evidencia |
| Perdida de evidencia de revision | Las evidencias pueden quedar en correos, carpetas sueltas o rutas no estandarizadas | Medio | Media | Procesos / Documentacion | Adjuntar rutas o imagenes locales en la revision y exportar reporte DOCX |
| Dependencia de personas clave | El criterio de que archivo es el correcto o que cambios se hicieron vive en la memoria del equipo | Alto | Media | Gobierno TI | Documentar reglas, snapshots, baseline y flujo de confirmacion reforzada |
| Falta de recuperacion ante errores | Si se dana un archivo o se elimina informacion, no hay snapshots consistentes del estado editable | Alto | Media | Persistencia / Continuidad | Crear historial automatico y snapshots manuales persistentes |
| Riesgo de datos no cifrados o mal protegidos | Los Excel pueden circular por carpetas compartidas sin controles suficientes | Alto | Media | Seguridad / Datos | Limitar la operacion a repositorio local controlado y definir respaldos seguros |

## Riesgos de la solucion nueva en desarrollo

La solucion nueva reduce varios riesgos del proceso anterior porque introduce base de datos, reglas de unicidad, linea base original, validaciones y exportaciones controladas. Aun asi, tambien aparecen nuevos riesgos propios de una aplicacion local con persistencia, importacion de archivos, generacion de documentos y sincronizacion con OneDrive/Microsoft Graph.

| Riesgo | Causa | Impacto | Probabilidad | Arquitectura afectada | Mitigacion propuesta |
|--------|-------|---------|--------------|------------------------|----------------------|
| Base de datos local como punto unico de falla | La informacion editable queda concentrada en una base local libSQL/SQLite | Critico | Media | Persistencia / Disponibilidad | Implementar respaldos frecuentes, snapshots manuales y sincronizacion controlada con carpeta de respaldo |
| Corrupcion o perdida de la base de datos | Fallos del equipo, cierre inesperado o sincronizacion incorrecta | Critico | Media | Persistencia | Mantener backups antes de fijar baseline y antes de restaurar snapshots |
| Baseline fijada por error | Un usuario podria marcar como original un contenido incorrecto | Alto | Baja | Original Baseline / Gobierno | Mantener confirmacion reforzada con texto `FIJAR ORIGINAL`, acknowledgement de reemplazo, backup y perfil guardado |
| Importacion incorrecta de Excel | Estructuras de archivo no esperadas, celdas vacias o formatos cambiantes | Alto | Media | Import / Datos | Validar columnas requeridas, forward-fill controlado, pruebas con workbook de muestra y reporte de inconsistencias |
| Deduplicacion agresiva | La normalizacion podria unir lineamientos que parecen iguales pero representan casos distintos | Medio | Media | CNA Model / Import | Revisar claves de unicidad por scope, factor, caracteristica y aspecto; dejar trazabilidad de registros omitidos |
| Codigos de aspecto generados incorrectamente | Aspectos manuales dependen de claves deterministicas generadas | Medio | Media | CNA Model | Usar generacion estable basada en factor, caracteristica y descripcion; permitir revision visual del resultado |
| Exportaciones con colores incorrectos | La comparacion por codigo y hash podria clasificar mal cambios si el baseline esta mal fijado | Alto | Media | Export / Original Baseline | Bloquear exportacion sin baseline y mostrar resumen de diferencias antes de generar archivos |
| Restauracion de snapshot equivocada | Restaurar reemplaza preguntas y lineamientos actuales | Alto | Baja | History / Persistencia | Usar confirmacion reforzada, mostrar fecha, autor y alcance del snapshot antes de restaurar |
| Historial incompleto como backup total | El snapshot no guarda todas las tablas auxiliares de la base | Medio | Media | History / Persistencia | Comunicar que snapshots cubren estado editable; mantener backup completo de base para recuperacion total |
| Falta de monitoreo operativo | Al ser aplicacion local, puede no haber observabilidad centralizada | Medio | Media | Disponibilidad / Soporte | Registrar errores de importacion, exportacion y repositorio; agregar logs locales consultables |
| Accesos locales no controlados | Si varias personas usan el mismo equipo o carpeta, pueden modificar informacion sin control | Alto | Media | Seguridad / Workspace | Guardar perfil de editor, controlar ubicacion de base, restringir permisos de carpeta y evitar usuarios compartidos |
| Exposicion por sincronizacion cloud | OneDrive o Microsoft Graph pueden sincronizar archivos sensibles o versiones intermedias | Alto | Media | Workspace / Seguridad | Definir carpeta de sincronizacion, evitar subir temporales sensibles y documentar permisos de Microsoft Graph |
| Evidencias no soportadas en DOCX | Algunos archivos adjuntos no se incrustan y solo quedan como ruta | Bajo | Media | Provider Review / Export | Mantener lista de formatos soportados y conservar rutas para formatos no embebibles |
| Dependencia de convenciones importadas | Codigos de audiencia, formatos o convenciones pueden variar entre consolidados | Medio | Media | Question Bank / Export | Generar hoja de Convencion y normalizar codigos durante importacion |
| Complejidad de modulos de persistencia | La separacion en schema, parsing, helpers, snapshots y reviews puede dificultar mantenimiento | Medio | Media | Persistencia / Gobierno TI | Mantener responsabilidades documentadas y pruebas por modulo de repositorio |

## Analisis capa por capa de la solucion TO-BE

| Capa | Funcion en la solucion | Riesgos principales | Controles y mitigacion |
|------|------------------------|---------------------|------------------------|
| Aplicacion local | Facilita edicion, importacion, revision, snapshots y exportacion | Fallos de validacion, uso incorrecto, errores silenciosos | Validaciones bloqueantes, logs locales, mensajes claros, pruebas por flujo |
| Autenticacion y perfil | Identifica editor o supervisor responsable de acciones | Usuarios compartidos o acciones sin responsable claro | Perfil guardado, roles, confirmaciones reforzadas y registro de autor |
| Importacion Excel | Convierte consolidados en estructura normalizada | Columnas faltantes, jerarquia incompleta, duplicados | Validar schema, forward-fill controlado, reporte de inconsistencias |
| Modelo CNA | Normaliza Factor, Caracteristica, Aspecto y preguntas | Deduplicacion incorrecta o codigos mal generados | Claves estables, revision visual, trazabilidad de registros omitidos |
| Baseline original | Define referencia para comparar cambios | Fijar una version equivocada | Confirmacion `FIJAR ORIGINAL`, backup previo y bloqueo de exportacion sin baseline |
| Base local libSQL/SQLite | Guarda estado editable, lineamientos y revisiones | Punto unico de falla, corrupcion o perdida | Backups completos, snapshots, integridad transaccional y pruebas de restauracion |
| Sincronizacion OneDrive | Aporta redundancia y recuperacion en la nube | Conflictos, exposicion accidental, temporales sincronizados | Carpeta definida, permisos minimos, reglas de sincronizacion y exclusion de temporales |
| Exportacion | Genera instrumentos y reportes desde la base | Exportar version no aprobada o clasificar mal cambios | Validaciones previas, resumen de diferencias, version identificable |
| Revision del proveedor | Registra estado, evidencia y observaciones por entrega | Evidencia dispersa o formatos no soportados | Checklist por instrumento/audiencia, rutas estandarizadas, reporte DOCX |

## Comparacion AS-IS vs TO-BE

| Dimension | Aplicacion anterior / AS-IS | Solucion nueva / TO-BE |
|-----------|-----------------------------|-------------------------|
| Fuente de verdad | Archivos Excel editados manualmente | Base de datos local con banco de preguntas editable |
| Control de cambios | Revision manual entre versiones | Baseline original con hashes y colores de diferencia |
| Duplicados | Alta probabilidad por copia y consolidacion manual | Indices unicos para preguntas y lineamientos |
| Trazabilidad | Baja, depende de nombres de archivo y memoria del equipo | Snapshots, baseline, estados de revision y reportes |
| Validacion | Manual y tardia | Reglas bloqueantes antes de exportar o entregar |
| Revision del proveedor | Informal o dispersa | Checklist por instrumento/audiencia con evidencia |
| Disponibilidad | Depende del archivo correcto y de quien lo tenga | Depende de la base local, respaldos y sincronizacion |
| Seguridad | Riesgo por archivos circulando en carpetas o correos | Riesgo controlable con permisos locales, perfil y carpeta definida |
| Gobierno TI | Decisiones poco documentadas | Reglas explicitas para baseline, importacion, exportacion e historial |

## Riesgos priorizados

| Prioridad | Riesgo | Razon |
|-----------|--------|-------|
| 1 | Perdida o corrupcion de la base de datos local | Afectaria todo el banco de preguntas, baseline, lineamientos y revisiones |
| 2 | Baseline fijada incorrectamente | Todas las exportaciones con colores dependerian de una referencia equivocada |
| 3 | Importacion incorrecta del consolidado | Puede crear preguntas o lineamientos mal normalizados desde el inicio del ciclo |
| 4 | Acceso local o sincronizacion cloud sin control | Puede exponer informacion academica, evidencias o instrumentos de evaluacion |
| 5 | Restauracion equivocada de snapshot | Puede reemplazar el estado actual y generar perdida operativa |

## Plan de mitigacion

| Fase | Accion | Riesgo que reduce | Responsable sugerido |
|------|--------|-------------------|----------------------|
| Preparacion | Documentar donde vive la base local, que carpeta se sincroniza y quien tiene acceso | Acceso local sin control, confusion de versiones | Supervisor del proceso |
| Importacion | Validar columnas, jerarquia CNA, duplicados y conteo de preguntas antes de aceptar el archivo | Importacion incorrecta, duplicidad, jerarquia incompleta | Analista responsable |
| Baseline | Mantener confirmacion reforzada, backup previo y perfil de quien fija la linea base | Baseline incorrecta, perdida de referencia | Supervisor del proceso |
| Persistencia | Crear respaldos completos antes de fijar baseline, restaurar snapshots o ejecutar cambios masivos | Corrupcion o perdida de base | Analista responsable |
| Exportacion | Bloquear exportacion sin baseline y mostrar resumen de diferencias antes de generar instrumentos | Exportaciones incorrectas | Aplicacion / analista |
| Revision proveedor | Registrar estado, observacion y evidencia por instrumento/audiencia | Baja auditoria de revision | Analista y supervisor |
| Operacion | Agregar logs locales para importacion, exportacion, restauracion y errores de repositorio | Falta de monitoreo operativo | Equipo tecnico |
| Seguridad | Revisar permisos de OneDrive y Microsoft Graph; evitar subir temporales sensibles | Exposicion por sincronizacion cloud | Responsable TI / equipo interno |
| Calidad | Mantener pruebas automaticas de importacion, deduplicacion, baseline y exportacion | Regresiones tecnicas | Equipo tecnico |

## Respuestas a las preguntas guia

| Pregunta guia | Respuesta aplicada al negocio |
|---------------|-------------------------------|
| ¿Que activos son mas criticos? | Base local, baseline original, banco de preguntas, jerarquia CNA, snapshots, exportaciones, evidencias de revision y reportes del proveedor |
| ¿Que puede salir mal? | Importacion incorrecta, baseline mal fijada, duplicados, perdida de base, snapshot equivocado, exportacion con diferencias mal clasificadas o sincronizacion insegura |
| ¿Que tan probable es? | Medio a alto para errores de importacion y operacion; medio para corrupcion o sincronizacion; bajo a medio para baseline incorrecta si existe confirmacion reforzada |
| ¿Cual seria el impacto? | Alto o critico si afecta la base local, la linea base, la trazabilidad o la version enviada al proveedor |
| ¿Que controles existen o estan propuestos? | Base local, reglas de unicidad, baseline, snapshots, validaciones antes de exportar, revision del proveedor y sincronizacion con OneDrive |
| ¿Que controles faltan o deben reforzarse? | Backups completos, logs locales, pruebas automaticas, permisos de carpeta, reglas de sincronizacion y documentacion operativa |
| ¿Quien debe gestionar el riesgo? | Analista responsable, supervisor del proceso y equipo tecnico que mantiene la aplicacion |

## Recomendaciones

- Mantener respaldos completos de la base antes de fijar o reemplazar la linea base original.
- Documentar claramente donde vive la base local, que carpeta se sincroniza y quien tiene acceso.
- Conservar la confirmacion reforzada para operaciones destructivas o de alto impacto.
- Agregar logs locales para importacion, exportacion, restauracion de snapshots y errores de repositorio.
- Probar cada importacion con validaciones de duplicados, jerarquia CNA y conteo de preguntas antes de permitir exportar.
- Revisar periodicamente los permisos de OneDrive y Microsoft Graph para evitar exposicion accidental.
- Mantener pruebas automaticas de importacion, deduplicacion, comparacion contra baseline y exportacion de instrumentos.

## Conclusion

El proceso anterior concentra riesgos en el trabajo manual con Excel, la falta de trazabilidad y la dependencia de personas. La solucion nueva corrige buena parte de esos problemas al mover la operacion hacia una base de datos, baseline inmutable, validaciones y exportaciones generadas desde el sistema.

Sin embargo, la nueva arquitectura tambien necesita controles propios: respaldos, gobierno de baseline, permisos locales, cuidado con sincronizacion cloud y pruebas de importacion/exportacion. El riesgo principal cambia de "no saber cual archivo es el correcto" a "proteger bien la base y las reglas que gobiernan el ciclo de autoevaluacion".
