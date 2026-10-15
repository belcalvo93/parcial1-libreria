# Proposal — ejercicio-3-datos-filtro

## Why

El Ejercicio 3 necesita que el catálogo y los datos de ejemplo estén disponibles como arrays en JavaScript para que el render dinámico y el filtro por título funcionen. Actualmente los libros están hardcodeados en el HTML estático (Ejercicio 1) y el CSS ya está completo (Ejercicio 2). Sin esta capa de datos, el catálogo no puede filtrarse en tiempo real ni los formularios pueden leer los datos para las altas (Ejercicio 4).

## What Changes

- Se crea `js/datos.js` con un objeto `DATOS` que contiene los arrays: `editoriales` (2), `generos` (2), `autores` (3), `libros` (6 con `id`, `titulo`, `isbn`, `precio`, `editorialId`, `generoId|null`, `autorIds[]`), `clientes` (3). Valores exactamente los congelados del design archivado del Ej. 1 (§5). Sin `ventas` (queda para el Ej. 5 de Bruno).
- Se crea `js/catalogo.js` con la función `renderCatalogo(filtro = "")` que lee `DATOS.libros`, aplica el filtro por título (parcial, case-insensitive, con `trim`), y pinta las filas en `#tabla-catalogo tbody` creando `tr`/`td` con `textContent` (nunca `innerHTML`). Al inicializar (DOMContentLoaded) llama a `renderCatalogo()` sin filtro.
- Se conecta el buscador `#catalogo-busqueda` con el evento `input` (tiempo real) y el botón `#catalogo-buscar` con `click`, ambos hacia `renderCatalogo(valor)`.
- Se muestran/ocultan los mensajes `#mensaje-catalogo-vacia` y `#mensaje-catalogo-sin-resultados` según corresponda.
- Se enlazan los dos scripts al final de `index.html` (antes de `</body>`), sin modificar el HTML existente.

## Capabilities

### New Capabilities

- `datos`: capa de datos en `js/datos.js` con los arrays congelados del Ej. 1. El libro sin género tiene `generoId: null` (“Sin clasificar”); “Manual para perder trenes” tiene 2 `autorIds`.

### Modified Capabilities

- `catalogo`: el catálogo pasa de HTML estático a render dinámico desde `DATOS.libros` con filtro por título en tiempo real. Los mensajes de estado se controlan desde JS.

## Impact

- Archivos nuevos: `js/datos.js`, `js/catalogo.js`.
- Archivo modificado: `index.html` (se agrega `<script src="js/datos.js">` y `<script src="js/catalogo.js">` antes de `</body>`). No se toca `css/styles.css`.
- Los `id` del HTML no cambian: `#tabla-catalogo`, `#catalogo-busqueda`, `#mensaje-catalogo-vacia`, `#mensaje-catalogo-sin-resultados`.
- Fuera de alcance: validaciones, altas (Ejercicio 4), ventas/historial dinámicos (Ejercicio 5), localStorage, ABM de editoriales/géneros/autores.
