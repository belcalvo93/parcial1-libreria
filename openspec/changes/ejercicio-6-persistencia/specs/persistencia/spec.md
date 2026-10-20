# Spec Delta — persistencia

## Purpose

Conserva libros, clientes y ventas en `localStorage` para que sobrevivan a recargas y reaperturas del navegador, sin duplicar los datos iniciales.

## ADDED Requirements

### Requirement: Primer inicio con datos de ejemplo

El sistema SHALL, cuando no existen las claves `paginaViva.libros`, `paginaViva.clientes` y `paginaViva.ventas`, usar los ejemplos de `js/datos.js` y guardarlos en esas claves como JSON.

#### Scenario: Primera apertura

- **WHEN** se abre `index.html` en un navegador sin datos guardados
- **THEN** el catálogo muestra los 6 libros de ejemplo y `localStorage` queda con 6 libros, 3 clientes y las ventas iniciales

### Requirement: Recuperación sin duplicados

El sistema SHALL, cuando existen datos guardados, reemplazar los arrays de `DATOS` por los guardados antes de cualquier render, sin sumarles los ejemplos.

#### Scenario: Recarga después de altas

- **WHEN** se agrega un libro y un cliente válidos y se recarga la página varias veces
- **THEN** el catálogo, `tabla-libros` y `tabla-clientes` muestran los registros nuevos una sola vez y los ejemplos una sola vez

#### Scenario: Recarga después de una venta

- **WHEN** se confirma una venta y se recarga la página
- **THEN** el historial muestra esa venta una sola vez, con sus renglones y total

### Requirement: Guardado tras cada operación

El sistema SHALL guardar libros, clientes y ventas mediante `window.guardarDatos()` después de cada alta válida de libro o cliente y después de cada venta confirmada. Las operaciones rechazadas no guardan nada nuevo.

#### Scenario: Alta válida persistida

- **WHEN** se agrega un libro con ISBN nuevo
- **THEN** `paginaViva.libros` contiene el libro nuevo inmediatamente, sin esperar a cerrar la página

### Requirement: Precio histórico independiente del catálogo guardado

El sistema SHALL mostrar en el historial el precio unitario, los subtotales y el total copiados al confirmar la venta, aunque luego cambie el precio del libro en `paginaViva.libros`.

#### Scenario: Cambio manual del precio en localStorage

- **WHEN** se confirma una venta, se modifica solo el precio de ese libro en `paginaViva.libros` con las herramientas del navegador y se recarga
- **THEN** el catálogo muestra el precio nuevo y el detalle de la venta conserva los importes confirmados

### Requirement: Tolerancia a almacenamiento no disponible o dañado

El sistema SHALL seguir funcionando en memoria si `localStorage` no está disponible, y SHALL volver a los ejemplos de una colección si su JSON guardado está dañado o no es un array.

#### Scenario: JSON dañado

- **WHEN** `paginaViva.clientes` contiene texto que no es JSON válido y se recarga
- **THEN** la página carga sin errores y `tabla-clientes` muestra los 3 clientes de ejemplo
