---
name: oece-contrataciones
description: Consultar compras públicas peruanas del OECE y SEACE mediante API OCDS o descargas oficiales, analizar entidades como CENARES por año y generar listados verificables, gráficos y presentaciones ejecutivas. Usar para procesos, adjudicaciones, proveedores, montos, evolución e informes de contratación pública.
---

# Analista OECE
Responder en español y ejecutar el trabajo solicitado con herramientas disponibles. Leer references/fuentes.md antes de consultar.

## Alcance
- Resolver entidad por nombre oficial e identificadores en fuentes oficiales y datos. No confundir CENARES con todo MINSA ni entidad compradora con unidad convocante. Registrar alias incluidos; preguntar solo ante ambigüedad material.
- Para "procesos realizados en 2025", asumir convocados en 2025 y declarar este criterio. Usar fecha inicial de convocatoria en America/Lima e intervalo [2025-01-01, 2026-01-01). No sustituirla por fecha de publicación OCDS, actualización o año del código.
- Distinguir procesos convocados, adjudicaciones y contratos del año. Distinguir estado actual y estado al cierre; para cierre reconstruir únicamente publicaciones anteriores al corte mediante reglas OCDS.

## Consultar
1. Usar la API como primera vía para consultas acotadas; para extracciones masivas elegir API o descargas según cobertura y volumen. Leer la conexión verificada en references/fuentes.md y comprobar la documentación vigente. Distinguir la página /api de la base de datos /api/v1/. No inventar filtros por RUC o entidad.
2. Probar un GET pequeño al endpoint documentado desde un cliente HTTP local autorizado. Verificar estado HTTP, Content-Type y estructura JSON OCDS: un 200 con HTML no acredita acceso a datos. Si solo la herramienta web devuelve 403 al leer la documentación, registrar ese resultado como fallo de esa herramienta y comprobar la documentación con el navegador o el cliente local. No declarar caída ni bloqueo general sin probar el endpoint de datos. Usar solo lectura y no guardar credenciales.
3. Consultar records para consolidación y releases para historia según disponibilidad documentada. Usar compiledRelease cuando exista; si falta aplicar reglas oficiales de fusión, nunca asumir que el último release contiene todo el expediente.
4. Determinar qué universo cubre el período solicitado. Los filtros startDate/endDate documentados para recordsAfter corresponden al inicio del período de licitación, no a awards.date. Para adjudicaciones de un año, filtrar awards.date en America/Lima y cubrir también procesos convocados en años anteriores; no presentar como total anual una consulta limitada a convocatorias de ese año. Tampoco asumir que el año de un archivo OCP representa el año de adjudicación ni que doce archivos mensuales cubren todos los procesos del año.
5. Recorrer todas las páginas/cursores documentados hasta agotarlos; en paquetes que incluyan links.next, seguir ese enlace sin modificar filtros ni reconstruir el cursor y detectar enlaces repetidos. Verificar que siga siendo una URL HTTPS del host oficial bajo /api/v1/ antes de solicitarla. Validar que records/releases sea una lista; aceptar una lista vacía como resultado válido, pero no un JSON de error. Terminar cuando links.next sea null; si falta el campo, comprobar la respuesta antes de asumir agotamiento. No superar size=100. Para más de 10.000 resultados usar la variante After. Registrar parámetros, páginas y criterio de terminación. Marcar parcial si hay límite operativo, paginación ambigua o páginas faltantes.
6. Aplicar timeout y reintentos limitados con espera progresiva a 429/5xx, respetando Retry-After. Distinguir errores de red/permisos locales de respuestas HTTP del servidor. Ante 401/403 del endpoint de datos en el cliente local, detener esa vía y explicar el acceso requerido; no cambiar identidades, credenciales o rutas para eludir controles. Si la API sigue inaccesible, intentar descargas oficiales documentadas y después OCP con procedencia, verificando cobertura y esquema. Registrar qué cliente, URL y respuesta motivaron el cambio de fuente.
7. Si acceso alternativo falla, informar impedimento y pedir documentación/exportación adjunta. No inventar cifras. Conservar datos originales y manifiesto reproducible.

## Depurar y verificar
- Contar procesos únicos por ocid, publicaciones por (ocid, release.id). Nunca contar releases como procesos.
- Mantener tablas relacionadas de procesos, adjudicaciones y contratos; evitar duplicar montos al aplanar relaciones uno a muchos.
- Incluir cuando existan: ocid, código SEACE, entidad e ID, objeto, método, convocatoria, estado y fecha del estado, moneda, monto referencial, monto adjudicado, proveedores, contratos y URL oficial. Distinguir ausente, cero y no aplicable.
- Separar montos referenciales, adjudicados y contratados. Deduplicar adjudicaciones y contratos por ID dentro de ocid, declarar estados incluidos y sumar por moneda. Convertir solo con fuente y criterio explícitos. Distinguir participante de adjudicatario.
- Revisar repeticiones entre OCID: se observaron adjudicaciones idénticas bajo OCID diferentes. Excluir una repetición del total económico solo si coinciden sistema de origen, tender.id, awards.id y el contenido de la adjudicación. Conservar OCID originales, alias y ajuste de conteos/montos; no fusionar por título, proveedor o monto solamente. Si hay diferencias, investigarlas antes de sumar o descartar.
- Calcular montos con precisión decimal y declarar cualquier redondeo; conservar el valor original. No atribuir adjudicaciones sin proveedor a un adjudicatario ni tratar un estado ausente como vigente. Separar estos casos en el resumen.
- Comprobar identidad, intervalo, unicidad, fechas ausentes, extracción agotada y conciliación de conteos con fuente oficial equivalente si existe.
- Declarar "completa respecto del universo consultado" solo con cobertura temporal/de entidad y paginación verificadas, indicando restricciones del publicador. En otro caso declarar parcial o no verificada y causa.

## Entregar
- Producir resumen y listado detallado CSV/XLSX si se piden todos los procesos; no entregar únicamente principales procesos. Mantener adjudicaciones y contratos en tablas separadas cuando corresponda.
- Calcular procesos únicos por mes, método y estado; montos adjudicados por moneda y principales adjudicatarios si hay datos. Reconciliar indicadores, gráficos y tabla.
- Usar barras para rankings y métodos; columnas/líneas para evolución. Mostrar período, unidad, fuente y denominador. No usar datos simulados ni confundir nulos con cero.
- Para PPTX usar skill Presentations y verificar render. Preparar 6–8 diapositivas: alcance/cobertura, indicadores, evolución, métodos/estados, adjudicaciones/proveedores, hallazgos y limitaciones, metodología/fuentes. Añadir archivo con todos los procesos.
- Usar las skills Spreadsheets o PDF cuando corresponda. Guardar según reglas del entorno. Separar hechos de hipótesis y no inferir corrupción a partir de indicadores.
- Incluir fuentes enlazadas, fecha de extracción, identidad, criterio temporal y cobertura en entregables. Enviar a terceros solo por instrucción expresa y con destino identificado.

## Ejemplo
"Qué procesos realizó CENARES en 2025, con gráficos y presentación ejecutiva": resolver identidad, declarar convocatoria como criterio, verificar interfaz, extraer universo completo, filtrar, deduplicar y generar tabla, gráficos y PPTX. Si falla acceso, explicar el bloqueo sin producir resultados ficticios.
