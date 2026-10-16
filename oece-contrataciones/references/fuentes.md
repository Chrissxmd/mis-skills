# Fuentes y verificación

- Documentación: https://contratacionesabiertas.oece.gob.pe/api
- Descargas: https://contratacionesabiertas.oece.gob.pe/descargas
- Servicio oficial: https://www.gob.pe/52005-acceder-al-portal-de-contrataciones-abiertas-de-la-compra-publica
- Estándar: https://standard.open-contracting.org/latest/es/
- Registros: https://standard.open-contracting.org/latest/es/schema/records_reference/
- Alternativa OCP con procedencia: https://data.open-contracting.org/es/publication/135
- Recolector OCP: https://kingfisher-collect.readthedocs.io/en/latest/_modules/kingfisher_scrapy/spiders/peru_oece_api_releases.html

Al crear esta skill el 2026-10-16 UTC, /api devolvió 403. La fuente oficial confirma Release, Record y Files. El recolector OCP menciona releasesAfter y recordsAfter. NO están verificados sus parámetros, rutas completas, autenticación, paginación, límites, filtros ni identificador de CENARES. Verificar antes de consultar; no tratar conjeturas como especificación.

Manifiesto de extracción: fuentes y versión de documentación, URLs y parámetros sin secretos, fecha UTC, criterio temporal y zona, identidad de entidad, páginas/archivos esperados y recibidos, filas originales, ocid únicos, descartes, errores, cobertura y justificación. Conciliar totales solo si corresponden al mismo universo y criterio temporal.
