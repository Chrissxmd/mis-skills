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
1. Revisar documentación oficial o especificación OpenAPI enlazada. Verificar base URL, método, filtros, formato de fechas, autenticación, límites, paginación y esquema antes de implementar. No inventar filtros por RUC o entidad.
2. Probar consulta pequeña y validar JSON OCDS frente a HTML de error. Usar solo lectura y no guardar credenciales.
3. Consultar records para consolidación y releases para historia según disponibilidad documentada. Usar compiledRelease cuando exista; si falta aplicar reglas oficiales de fusión, nunca asumir que el último release contiene todo el expediente.
4. Determinar qué universo cubre las convocatorias del período. Una consulta por fecha de publicación o modificación no equivale a convocatoria; no asumir que doce archivos mensuales cubren todos los procesos del año.
5. Recorrer todas las páginas/cursores documentados hasta agotarlos; detectar cursores repetidos. Registrar parámetros, páginas y criterio de terminación. Marcar parcial si hay límite operativo o páginas faltantes.
6. Aplicar timeout y reintentos limitados con espera progresiva a 429/5xx, respetando Retry-After. Ante 401/403 detener esa vía; no evadir controles. Intentar descargas oficiales documentadas verificando cobertura y esquema.
7. Si acceso alternativo falla, informar impedimento y pedir documentación/exportación adjunta. No inventar cifras. Conservar datos originales y manifiesto reproducible.

## Depurar y verificar
- Contar procesos únicos por ocid, publicaciones por (ocid, release.id). Nunca contar releases como procesos.
- Mantener tablas relacionadas de procesos, adjudicaciones y contratos; evitar duplicar montos al aplanar relaciones uno a muchos.
- Incluir cuando existan: ocid, código SEACE, entidad e ID, objeto, método, convocatoria, estado y fecha del estado, moneda, monto referencial, monto adjudicado, proveedores, contratos y URL oficial. Distinguir ausente, cero y no aplicable.
- Separar montos referenciales, adjudicados y contratados. Deduplicar adjudicaciones y contratos por ID dentro de ocid, declarar estados incluidos y sumar por moneda. Convertir solo con fuente y criterio explícitos. Distinguir participante de adjudicatario.
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
