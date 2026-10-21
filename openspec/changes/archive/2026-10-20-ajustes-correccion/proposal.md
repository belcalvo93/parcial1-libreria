# Proposal — ajustes-correccion

## Why

El proyecto ya está aprobado y el profesor dejó dos recomendaciones sobre la entrega. Aplicarlas mejora el comportamiento de las tablas en mobile sin alterar su semántica y corrige el texto del pie de página, que todavía menciona la materia en lugar de la librería.

## What Changes

- **Tablas con contenedor propio.** Cada tabla se envuelve en un `<div class="tabla-contenedor">…</div>`:
  - `index.html`: `#tabla-catalogo`, `#tabla-libros`, `#tabla-clientes`, `#tabla-historial` y la tabla estática dentro del `<details>` de V-001. No cambian ids, clases ni contenido de las tablas.
  - `js/ventas.js`: en `crearDetalle`, el `<div class="tabla-contenedor">` se crea con `createElement`, la tabla generada se agrega dentro del div y el div se agrega al `<details>` en lugar de la tabla. Nada más de `ventas.js` cambia.
  - `css/styles.css` (sección 10): `.tabla-contenedor` concentra `width:100%`, `max-width:100%`, `overflow-x:auto` y `-webkit-overflow-scrolling:touch`. `.tabla` pierde `display:block` y `overflow-x`, conserva su `display` nativo de tabla y mantiene `width:100%` y `border-collapse:collapse`. Las celdas siguen con `white-space:nowrap`.
  - Los `.js` que ocultan una tabla con `tabla.hidden` no cambian: el atributo `hidden` sigue funcionando sobre la tabla.
- **Pie de página.** En `index.html` se reemplaza «Librería Página Viva — TPI Programación III» por «© 2026 Librería Página Viva».

## Capabilities

### New Capabilities

Ninguna.

### Modified Capabilities

- `presentacion`: el requisito de tablas desplazables pasa a exigir un contenedor `.tabla-contenedor` y que la `<table>` conserve su `display` nativo.
- `estructura-navegacion`: se fija el texto del pie de página («© 2026 Librería Página Viva»).

## Impact

- Archivos modificados: `index.html` (5 envoltorios de tabla + texto del footer), `js/ventas.js` (solo `crearDetalle`), `css/styles.css` (sección 10 y su comentario).
- Sin archivos nuevos. Sin `innerHTML`.
- No se toca la lógica restante de los `.js`, ni los changes archivados, ni la estructura de datos.
- Fuera de alcance: rediseñar las tablas, cambiar su contenido, agregar funcionalidad nueva o modificar el comportamiento de ocultamiento existente.
