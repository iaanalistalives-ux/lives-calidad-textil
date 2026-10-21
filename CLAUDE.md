# CLAUDE.md — Seguimiento de Aprobación de Tono (lives-calidad-textil)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
`aprobacion-tono/index.html` es un **tablero de solo lectura**, sin formulario. Cada fila es una **partida** (un lote de tinturado) de una tela enviada al cliente para aprobar el tono. Tintorería es el taller que tiñe y broker es el intermediario.
- **No usa la base de datos.** El usuario arrastra el Excel "SEGUIMIENTO APROB TONO CLIENTES" y se lee en el navegador (SheetJS). No se guarda nada en ningún lado.
- Hojas: APROBACIÓN CLIENTE y APROBACIÓN INTERNA (se detectan por las columnas `STATUS CLIENTE` o `STATUS FINAL CLIENTE` y `STATUS INTERNO`). También acepta el formato antiguo de una sola hoja.
- Columnas esperadas: MES, AÑO, GRUPO, ITEM (= cliente), TINTORERIA, PARTIDA, COLOR, ARTICULO, BROKER, STATUS CLIENTE, STATUS INTERNO, FECHA STATUS CLIENTE, FECHA STATUS INTERNO y COMENTARIOS.
- Estados: APROBADO, RECHAZADO, PENDIENTE y vacío (sin respuesta del cliente o sin registro interno). El cruce entre lo interno y lo del cliente usa la llave `grupo|partida|color`, uno a uno.
- Status global de la partida: RECHAZADO si el cliente o el interno rechazaron; APROBADO si el cliente aprobó; PENDIENTE en cualquier otro caso. Esa regla está pendiente de confirmar con el área.
- Filtros: año (uno solo), mes, cliente, tintorería y broker; "Limpiar filtros".
- Pestañas: Resumen Global (3 donas: cliente, interno y global), Status Cliente, Status Interno y Tarjetas por Cliente (equivalen a la pestaña DASHBOARD del Excel).
- KPIs: % aprobado global (principal), % aprobado cliente (aprobadas ÷ respondidas), % aprobado interno, rechazadas con sus "fugas" (aprobadas internamente pero rechazadas por el cliente) y pendientes (incluye las sin respuesta). Evolución mensual y rankings por cliente y por tintorería con el detalle de las no aprobadas. No hay meta: el veredicto compara el último mes con el anterior.
- "Exportar para observadores" descarga un HTML autónomo con los datos adentro (`tono_<año>_snapshot_<fecha>.html`), que se abre sin servidor ni usuario. Es la forma actual de compartirlo. Imprimir y "Cargar otro archivo".

## Base de datos
Ninguna. No hay tablas, funciones ni Storage de esta herramienta.

## Quién la usa
No aparece en el registro de entradas del portal de los últimos 60 días (al 21/10/2026).

## Quién puede registrar
Nadie registra: no hay guardado. Cualquiera que abra la página puede cargar un Excel y verlo; los datos no quedan en ningún lado.

## Pendientes vistos al escribir esto (21/10/2026)
- Es una herramienta que aún no pasó a la base: el Excel manual es la única fuente. Para el destino SQL Server hace falta una tabla de partidas (cliente, grupo, partida, color, artículo, tintorería, broker, estados cliente e interno con sus fechas y comentarios) y una función de lectura, y decidir si la carga de Excel se mantiene como importación. El "Exportar para observadores" se reemplazaría por una vista compartida.
- Choca con la regla "todo sale de la base": hoy los datos dependen de quién carga el archivo.
- Los textos de ayuda dicen que el comparativo entre años aún no está incluido.
- No se verificó si el botón "Descargar Excel" del bloque común aparece en este tablero; no tiene un exportador propio.
