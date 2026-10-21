# Spec Delta — presentacion

## MODIFIED Requirements

### Requirement: Tablas desplazables dentro de su propio contenedor

El sistema SHALL envolver cada `.tabla` en un contenedor `.tabla-contenedor` que concentre el desplazamiento horizontal, de modo que la `<table>` conserve su `display` nativo de tabla y la página completa no genere desplazamiento horizontal.

#### Scenario: Envoltorio en el HTML y en el JS

- **WHEN** se inspeccionan `index.html` y el detalle generado por `js/ventas.js`
- **THEN** cada `<table class="tabla">` es hija directa de un `<div class="tabla-contenedor">`, y las tablas conservan sus ids, clases y contenido

#### Scenario: Tabla ancha a 320 px

- **WHEN** se muestra la tabla del catálogo (6 columnas) en un viewport de 320 px
- **THEN** el `.tabla-contenedor` presenta scroll horizontal propio y la página no muestra scroll horizontal

#### Scenario: Tabla cómoda a 1280 px

- **WHEN** se muestra la misma tabla en un viewport de 1280 px
- **THEN** todas las columnas caben en el contenedor y no hay desplazamiento interno

#### Scenario: Display nativo de tabla

- **WHEN** se inspecciona el `display` calculado de una `.tabla`
- **THEN** vale `table` (no `block`) y las reglas de tabla se aplican a la `<table>`, mientras que `.tabla-contenedor` aporta `width:100%`, `max-width:100%`, `overflow-x:auto` y `-webkit-overflow-scrolling:touch`

#### Scenario: Celdas sin cortar palabras

- **WHEN** una tabla excede el ancho disponible
- **THEN** las celdas conservan `white-space:nowrap`, de modo que el ancho mínimo de cada columna es el de su contenido y el texto no se parte letra por letra
