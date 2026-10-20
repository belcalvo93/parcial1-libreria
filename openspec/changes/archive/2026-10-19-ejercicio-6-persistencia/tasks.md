# Tasks — ejercicio-6-persistencia

> Alcance: `js/persistencia.js` nuevo, `README.md` nuevo, +1 línea `<script>` en `index.html`, helper `persistir()` en `js/altas.js` y un `ocultarError()` en `js/ventas.js`. No tocar `css/` ni changes archivados. Sin `innerHTML`.

## 1. Persistencia

- [x] 1.1 Crear `js/persistencia.js` con `cargarDatos()`, `guardarDatos()`, `leerColeccion()` y detección de `localStorage` disponible
- [x] 1.2 Claves `paginaViva.libros`, `paginaViva.clientes`, `paginaViva.ventas`; reemplazar arrays (nunca concatenar); inicializar `DATOS.ventas = []` si no existe
- [x] 1.3 Agregar `<script src="js/persistencia.js"></script>` entre `datos.js` y `catalogo.js`
- [x] 1.4 En `js/altas.js`, llamar a `persistir()` después de cada `push` válido (libro y cliente)
- [x] 1.5 Integrar con el Ej. 5: `guardarDatos()` al confirmar venta, ids `V-00N` desde `DATOS.ventas`, selectores de venta poblados desde `DATOS`

## 2. Pruebas manuales

- [x] 2.1 Caso 1 (filtro), caso 2 (alta de libro), caso 3 (alta de cliente)
- [x] 2.2 Recarga y reapertura después de altas: sin duplicados
- [x] 2.3 JSON dañado en una clave: vuelve a ejemplos sin error
- [x] 2.4 320 px y 1280 px sin scroll horizontal de página; foco visible con Tab
- [x] 2.5 Casos 4, 5 y 6 (ventas, cálculos y precio histórico) cuando esté el Ej. 5
- [x] 2.6 Caso 7 completo (recarga después de altas **y venta**)

## 3. Documentación y entrega

- [x] 3.1 `README.md` con integrantes, apertura sin npm, navegación, funcionalidades, datos iniciales, localStorage, casos manuales y entrega
- [x] 3.2 Completar en el README los resultados observados de los casos 4 a 7 y las funcionalidades del Ej. 5
- [x] 3.3 Generar el `.zip` final y validar con `openspec validate ejercicio-6-persistencia`

## 4. Ajuste de ventas

- [x] 4.1 En `js/ventas.js`, ocultar el error de venta al corregir la cantidad de un renglón: llamar a `ocultarError()` antes de `actualizarImportes()` en el listener `input` de `crearRenglon`
