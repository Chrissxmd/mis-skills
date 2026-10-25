# Fuentes y verificación

- Documentación: https://contratacionesabiertas.oece.gob.pe/api
- Descargas: https://contratacionesabiertas.oece.gob.pe/descargas
- Servicio oficial: https://www.gob.pe/52005-acceder-al-portal-de-contrataciones-abiertas-de-la-compra-publica
- Estándar: https://standard.open-contracting.org/latest/es/
- Registros: https://standard.open-contracting.org/latest/es/schema/records_reference/
- Alternativa OCP con procedencia: https://data.open-contracting.org/es/publication/135
- Recolector OCP: https://kingfisher-collect.readthedocs.io/en/latest/_modules/kingfisher_scrapy/spiders/peru_oece_api_releases.html

## Conexión verificada el 24 de octubre de 2026 (America/Lima)

La herramienta de búsqueda web respondió 403 para la página de documentación, pero el navegador y un GET local a esa misma página respondieron correctamente. /api sirve HTML de documentación; la base para datos es https://contratacionesabiertas.oece.gob.pe/api/v1/.

El siguiente GET, ejecutado desde PowerShell sin enviar Authorization, cookies ni clave API, devolvió HTTP 200, application/json y un paquete con un record y compiledRelease:

```powershell
$uri = 'https://contratacionesabiertas.oece.gob.pe/api/v1/recordsAfter?size=1&ocid=ocds-dgv273-seacev3-1135298'
$respuesta = Invoke-RestMethod -Uri $uri -Method Get -Headers @{ Accept = 'application/json' } -TimeoutSec 40
$respuesta.records[0].compiledRelease
```

Usar este expediente público como prueba de conectividad, no como dato fijo para responder consultas actuales. La respuesta comprobada identifica a CENARES, buyer.id PE-CONSUCODE-200363, con una adjudicación del 2025-08-08 por PEN 95460000. El RUC 20538298485 se comprobó previamente en parties.additionalIdentifiers de los datos descargados. Verificar siempre la identidad en la extracción que se utilice.

## Endpoints y filtros documentados

- GET /records y /releases: paginación tradicional para menos de 10.000 resultados.
- GET /recordsAfter y /releasesAfter: recorrido secuencial para más de 10.000 resultados.
- GET /record/{ocid} y /record/{sourceId}/{tenderId}: consulta puntual de registros.
- GET /release/{id} y /release/{sourceId}/{tenderId}: consulta puntual de entregas.
- GET /files y /file/{source}/{type}/{year}/{month}: descargas masivas; comprobar sus parámetros en la documentación antes de usarlos.

En /recordsAfter se observaron estos parámetros en la documentación oficial:

| Parámetro | Significado |
|---|---|
| size | Tamaño de página; predeterminado 10, máximo 100 |
| sourceId | seace_v3 o seace_v2 (este último también incluye SEACE V1) |
| startDate, endDate | Inicio del período de licitación; formato YYYY-MM-DD |
| dataSegmentationID | Segmentación con formato YYYY-MM |
| tenderId | Identificador del expediente/convocatoria según sistema de origen |
| ocid | Identificador OCDS del proceso |
| tenderTitle | Nomenclatura del proceso |
| mainProcurementCategory | goods, works o services |

No se observó filtro por RUC ni entidad en esta operación. No extrapolar estos parámetros a otras operaciones sin revisar su documentación. La descripción del valor predeterminado de startDate contiene una referencia ambigua a sí mismo: para búsquedas por período, proporcionar ambas fechas y comprobar sus límites con los datos; no suponer inclusividad ni fechas predeterminadas.

## Paginación comprobada

Se probó GET /recordsAfter con sourceId=seace_v3, startDate=2025-01-01 y endDate=2025-01-02:

- Con size=100 se recibieron 31 records y links.next=null.
- Con size=10 y siguiendo cada links.next se recibieron cuatro páginas, 31 records y 31 OCID distintos. El conjunto coincidió exactamente con el resultado de size=100.
- Con size=1 se comprobó además que la segunda página avanzaba a otro OCID.
- El enlace de continuación incluye searchAfter y first. Copiar la URL suministrada por el servidor sin reconstruir esos valores, cambiar los filtros ni decodificar/re-escapar el cursor manualmente.

Recorrido: solicitar la URL inicial, validar el paquete, conservar la página, extraer records y seguir links.next hasta que sea null. Registrar las URLs ya visitadas; detener y marcar extracción parcial ante enlace repetido, página inválida o error sin resolver. Una página con menos resultados que size no demuestra por sí sola terminación. Si falta links o next, no asumir agotamiento sin comprobar el contrato de respuesta.

El límite de prueba fue diez páginas y no se alcanzó. Es un límite operativo de la prueba, no un límite del servicio; no imponerlo a una extracción completa del usuario. Esta comprobación verifica la paginación del intervalo consultado, no la cobertura anual de CENARES ni una carga superior a 10.000 resultados.

La muestra incluyó tender.tenderPeriod.startDate=2025-01-02 aunque endDate=2025-01-02. Aplicar el intervalo final solicitado localmente; no asumir que endDate es exclusivo. Para consultas anuales por adjudicación, estas fechas de licitación no sustituyen awards.date.

## Diagnóstico y alternativa

- Un 403 en la herramienta web no demuestra que el endpoint falle desde el equipo del usuario. Comprobar una petición local normal, con los permisos de red del entorno y sin evadir controles del servicio.
- Una respuesta 200 con HTML en /api es la página de documentación, no un paquete OCDS. Consultar la URL de datos y validar records/releases y compiledRelease según corresponda.
- La prueba no requirió autenticación explícita; esto no garantiza acceso futuro ni a todos los endpoints. Ante 401/403 real del endpoint, detener esa vía y documentarlo.
- OCP es una alternativa con procedencia OECE, no una consulta directa a la API oficial. Antes de filtrar por año, verificar el criterio de partición del archivo y su cobertura. No equiparar año de publicación/actualización con fecha de adjudicación.

Manifiesto de extracción: fuentes y versión de documentación, URLs y parámetros sin secretos, fecha UTC, criterio temporal y zona, identidad de entidad, páginas/archivos esperados y recibidos, filas originales, ocid únicos, descartes, errores, cobertura y justificación. Conciliar totales solo si corresponden al mismo universo y criterio temporal.
