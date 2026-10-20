# Proposal — ejercicio-6-persistencia

## Why

El Ejercicio 6 (10 pts: persistencia 4, pruebas manuales registradas 3, documentación y entrega 3) cierra el TPI. Hoy todo vive en memoria: al recargar `index.html` se pierden los libros, clientes y ventas cargados. Además falta el `README.md` que pide el formato de entrega y el registro de los casos manuales.

## What Changes

- Se crea `js/persistencia.js`: guarda `DATOS.libros`, `DATOS.clientes` y `DATOS.ventas` en `localStorage` (una clave por colección, prefijo `paginaViva.`). En el primer inicio guarda los ejemplos de `js/datos.js`; en las siguientes aperturas **reemplaza** los arrays por lo guardado, así no se duplican ejemplos ni ventas. Expone `window.guardarDatos()`.
- `js/altas.js` llama a `guardarDatos()` después de cada alta válida (helper `persistir()` + 2 llamadas). No cambia ninguna validación.
- El JS de ventas del Ejercicio 5 llama a `guardarDatos()` al confirmar una venta (acordado con Bruno).
- `index.html`: +1 línea `<script src="js/persistencia.js"></script>` **entre** `datos.js` y `catalogo.js` (tiene que correr antes de que se renderice nada).
- Se crea `README.md` con todo lo que pide la consigna (integrantes, cómo abrir, navegación, funcionalidades, datos iniciales, localStorage y sus límites, casos manuales con resultado observado, entrega en `.zip`).

## Capabilities

### New Capabilities

- `persistencia`: carga y guardado de libros, clientes y ventas en `localStorage`, sin duplicar ejemplos, tolerante a almacenamiento no disponible o JSON roto.
- `documentacion`: `README.md` de entrega con los casos manuales registrados y el `.zip` sin dependencias.

## Impact

- Archivos nuevos: `js/persistencia.js`, `README.md`.
- Archivos modificados: `index.html` (+1 línea `<script>`), `js/altas.js` (helper `persistir()` + 2 llamadas).
- No se toca `css/` ni los changes archivados.
- Fuera de alcance: pantalla para editar precios (no se exige), sincronizar datos entre navegadores o usuarios, persistir editoriales/géneros/autores (no tienen alta).
