# Verificación del precio unitario

Leer al buscar, comparar o explicar precios unitarios de compras públicas. La meta es que el usuario pueda reproducir el resultado y detectar errores de cantidad, unidad, alcance o etapa. No es suficiente comprobar la división.

## Identificar el precio que se necesita

Determinar producto/servicio, presentación y unidad objetivo: ampolla, tableta, caja, sesión, paciente-mes, hora, etc. Inferirla del pedido cuando sea clara; pedir precisión solo si hay alternativas materiales y continuar reuniendo evidencia mientras tanto.

Fijar el alcance: precio ofertado, adjudicado, contratado o efectivamente pagado. No sustituir uno por otro. Conservar OCID, tender.id, award.id, item.id/posición y proveedor para enlazar las fuentes. Un proceso puede contener varios ítems, adjudicatarios, convocatorias o versiones de las bases.

## Revisar los datos estructurados

- Extraer descripción, clasificación y presentación; quantity, unit.name, unit.id/scheme, unit.value cuando exista, totalValue y moneda del mismo ítem. Identificar si provienen de tender, awards o contracts y guardar la ruta JSON. No combinar automáticamente cantidad de convocatoria con monto adjudicado.
- Buscar primero un precio unitario explícito de la etapa solicitada. Su existencia no demuestra que mida la unidad que quiere el usuario; revisar a qué presentación, período o prestación se refiere.
- No repartir el monto de toda una adjudicación entre sus productos. Si solo existe ese monto y varios ítems, buscar el desglose documental. No atribuir todo el importe a cada ítem. Con un único ítem, justificar la correspondencia entre su monto y el total de adjudicación antes de utilizarlo.
- Tratar monto/cantidad como cociente provisional hasta comprobar el significado de ambos. Una unidad genérica como Servicio y una cantidad 30 pueden representar capacidad mensual de pacientes y no sesiones compradas.
- Revisar si el monto cubre varios meses/años, si la cantidad es capacidad mensual o demanda total, y si la presentación agrupa unidades. La coincidencia de CUBSO o nombre no prueba equivalencia comercial.

## Buscar activamente el denominador y el precio

Ante ambigüedad, discrepancia o solicitud de comprobación documental:

1. Abrir los enlaces oficiales de documents del registro, conservando título, tipo, fecha y URL. Revisar la buena pro y la oferta económica aceptada para obtener el precio explícito y la cantidad económica; las bases integradas, requerimiento/TDR, ficha de homologación y anexos para definir alcance, unidad, presentación y fórmula. Revisar contrato/adendas si se pide precio contratado, hay modificaciones o aún falta información. Buscar en el portal oficial del proceso/SEACE y la entidad cuando el registro no enlace los documentos necesarios.
2. Descargar y abrir los documentos pertinentes, incluidos PDF dentro de ZIP/7z y sus anexos. No deducir el contenido por título, URL o nombre del archivo: cotejar nomenclatura, número de convocatoria, ítem, proveedor y fecha en su interior. Priorizar la versión aplicable; una plantilla de oferta vacía explica cómo calcular pero no prueba el precio aceptado.
3. Localizar tablas económicas y buscar términos pertinentes: precio unitario, cantidad, pacientes mensuales, sesiones, plazo, presentación, contenido, unidad de medida, impuestos y forma de pago. Leer completas las páginas relevantes, encabezados y notas al pie; estas pueden definir el denominador. Registrar página PDF y numeración impresa cuando difieran. Si un PDF no tiene texto, utilizar OCR o lectura visual; revisar visualmente la tabla decisiva y los números antes de concluir.
4. Reconstruir una conversión solo con factores respaldados en esos documentos: por ejemplo, pacientes × sesiones/paciente/mes × meses, o cajas × unidades/caja. Registrar el significado, valor, unidad y fuente de cada factor. No asumir 13 sesiones, 30 días, 12 meses ni contenido de caja como constantes universales. Tampoco convertir días a meses sin la equivalencia contractual pertinente.
5. Conciliar el precio explícito con el total y la cantidad económica de la misma etapa. Si no hay precio explícito, calcularlo con monto y cantidad económica documentados y marcarlo como calculado, no publicado. Resolver diferencias de impuestos, moneda, descuentos, versión o alcance antes de considerar equivalentes las cifras.

La investigación documental forma parte del trabajo solicitado. No terminar con “puedo revisar las bases” si todavía es posible consultarlas dentro del alcance autorizado.

Si una vía falla, conservar URL, cliente, fecha y error; usar las alternativas oficiales y OCP permitidas en fuentes.md, sin eludir controles. No repetir un acceso denegado ni inventar rutas. Tras agotar las fuentes pertinentes accesibles, indicar qué documento o dato falta, las búsquedas realizadas y por qué impide determinar el precio. No adivinar los factores ni solicitar archivos al usuario antes de intentar obtenerlos por las vías disponibles. No es necesario recorrer documentos ajenos al ítem ni repetir comprobaciones que ya resuelven la pregunta.

## Cálculo y conciliación

Usar Decimal u otra aritmética decimal exacta. Conservar los importes y precios originales, la precisión, la fórmula sin redondear y la regla de presentación. No reducir por defecto a dos decimales un precio farmacéutico que necesita más precisión.

Mostrar las dos identidades cuando correspondan:

- Cantidad económica = producto de factores documentados, con sus unidades y cancelación dimensional.
- Precio de la unidad objetivo = monto del mismo alcance ÷ cantidad económica.
- Total reconstruido = precio × cantidad económica; diferencia = total reconstruido − monto de referencia.

Si el documento expresa el precio en una unidad y el usuario pide otra, conservar ambos precios y explicar la conversión documentada. No convertir monedas ni concentraciones/presentaciones por parecido textual. No llamar precio de un medicamento al cociente de un paquete que incluye otros productos o servicios sin un desglose respaldado.

Cantidad cero/negativa, monto ausente y unidad desconocida no producen un precio cero: requieren investigación. No mezclar moneda del monto con la de otro valor. Comprobar si cada monto incluye impuestos y otros costos; si no consta, decirlo en vez de presumir una tasa o base tributaria.

La tolerancia de conciliación debe responder a los decimales publicados y a la cantidad, no a un porcentaje arbitrario. Informar la diferencia exacta, la precisión de cada dato y cualquier redondeo documentado. No ocultar una diferencia material bajo una etiqueta de redondeo. Si las fuentes discrepan sin explicación, mostrar ambas y marcar conflicto.

## Ficha para que el usuario lo compruebe

Por cada ítem analizado, presentar en tabla o archivo enlazado, con el detalle proporcional a la consulta:

| Campo | Información necesaria |
|---|---|
| Identidad y etapa | Entidad, proceso/convocatoria, OCID, adjudicación/contrato e ítem, proveedor; precio adjudicado/contratado/etc. |
| Objeto y unidad objetivo | Producto/servicio, concentración/presentación si aplica; qué se considera una unidad económica |
| Monto utilizado | Valor original, moneda, etapa, si corresponde a todo el paquete o solo al ítem; impuestos y costos incluidos conocidos o no informados |
| Cantidad publicada | Valor y etiqueta originales, ruta JSON o tabla documental; qué significa realmente |
| Cantidad económica | Valor utilizado para fijar el precio; si coincide o no con la publicada |
| Factores y plazo | Cada multiplicador con unidad y fuente; período de la prestación, si afecta la cantidad |
| Alcance incluido | Prestaciones, productos, frecuencias y exclusiones que afectan la interpretación/comparación |
| Precio publicado y calculado | Ambos cuando existan, unidad inequívoca, fórmula numérica, resultado sin redondear y mostrado |
| Conciliación | Total reconstruido, monto de referencia, diferencia y explicación |
| Evidencia | URL oficial, documento, fecha/versión, página PDF/impresa y tabla/nota por cada dato; archivo conservado y fecha de extracción |
| Estado y límites | Verificado, calculado con base documentada, provisional, no determinable o en conflicto; razón y evidencia pendiente |

Si un campo no aplica, indicarlo; si es relevante y no está publicado, señalarlo como no informado. Para listados grandes, incluir en el resumen cuántos precios están verificados/provisionales y la evidencia por fila en el detalle. No llamar verificado al conjunto si solo se revisó una muestra.

Usar estos estados con significado explícito:

- **Verificado documentalmente:** la documentación aplicable confirma el precio explícito y su unidad económica, y el total concilia. Identificar el documento y el cotejo realizado.
- **Calculado con base documentada:** los factores y el monto están respaldados y concilian, pero no se encontró un precio unitario publicado para cotejarlo.
- **Provisional:** existe precio publicado sin corroboración suficiente o un cociente cuya unidad/alcance está pendiente. Mostrar el motivo y excluirlo de comparaciones concluyentes.
- **No determinable:** falta un dato necesario tras revisar las vías pertinentes accesibles. Detallar el dato y la gestión pendiente.
- **En conflicto:** las fuentes del mismo ítem/etapa arrojan precios o cantidades incompatibles que no se han conciliado. No escoger silenciosamente uno.

Al corregir un resultado anterior, mostrar precio anterior, denominador incorrecto, evidencia nueva y precio/unidad corregidos. Rectificar los informes o archivos derivados dentro del alcance de la tarea; preservar los datos originales. La aritmética correcta no basta para validar la interpretación económica.

## Caso documentado: FISSAL, hemodiálisis, 2026

Caso de aprendizaje, no tarifa ni fórmula universal. No usarlo como dato actualizado de otra consulta.

- Proceso: CP Homologación-ABR-1-2026-SIS-FISSAL-3; OCID ocds-dgv273-seacev3-1240417; ítem 2, item.id 21345975; adjudicación 1240417-20551385168.
- JSON: quantity=30, unit.name=Servicio, monto adjudicado PEN 3.903.120. Dividir entre 30 da PEN 130.104, pero no el precio por sesión.
- Bases integradas, página PDF 26 (impresa 24): 30 **pacientes mensuales** por ítem y plazo de 1.095 días. Página PDF 69 (impresa 67), Anexo 6, nota 63: 13 sesiones mensuales referenciales por paciente y equivalencia contractual de 36 meses.
- Cantidad económica: 30 pacientes × 13 sesiones/paciente/mes × 36 meses = **14.040 sesiones**.
- Acta N.º 05-2026, página 9: ítem 2, Centro de Diálisis Virgen de Lourdes, 14.040 sesiones, **PEN 278** unitarios y PEN 3.903.120 totales; ítem 3, La Merced, misma cantidad económica, **PEN 315**, total PEN 4.422.600. El acta pertenece a los ítems 2 y 3 aunque sus archivos tengan nombres que mencionan otros ítems: verificar el contenido.
- Conciliación ítem 2: 14.040 × PEN 278 = PEN 3.903.120, diferencia PEN 0. El valor PEN 130.104 equivale a 468 sesiones por cupo de paciente durante el período, no a una sesión.
- La ficha de homologación incluye hemodiálisis, consulta nefrológica, nutrición, psicología, trabajo social, medicamentos y laboratorio, con sus propias frecuencias. El precio por sesión es la unidad económica de la atención integral; no permite aislar el precio de cada medicamento sin desglose adicional.

Fuentes del ejemplo, revisadas el 25/10/2026:

- [Ficha oficial del proceso](https://contratacionesabiertas.oece.gob.pe/proceso/ocds-dgv273-seacev3-1240417).
- [Bases integradas, archivo 7z](https://prod1.seace.gob.pe/SeaceWeb-PRO/SdescargarArchivoAlfresco?fileCode=585f1db4-792e-4f59-947a-21ede76049ce), incluye bases, TDR y ficha de homologación.
- [Buena pro, archivo ZIP](https://prod1.seace.gob.pe/SeaceWeb-PRO/SdescargarArchivoAlfresco?fileCode=81355ba8-a221-421b-834c-81e13ec58bf8), contiene reporte y actas.
