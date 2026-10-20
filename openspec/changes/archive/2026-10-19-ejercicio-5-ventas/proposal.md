# Proposal — ejercicio-5-ventas

## Why

El Ejercicio 5 (20 pts) pide la venta simulada y el historial. Hoy `form-venta` y `tabla-historial` son maqueta estática del Ej. 1: el renglón `venta-libro-1` no calcula nada, no se puede confirmar una venta y el historial muestra siempre la V-001 fija. Además el Ej. 6 (Joaquín) necesita que las ventas vivan en `DATOS.ventas` para guardarlas en `localStorage`.

## What Changes

- Se crea `js/ventas.js` con la lógica de venta e historial.
- `venta-cliente` y los selectores de libro se pueblan desde `DATOS` al cargar (incluye lo recuperado por el Ej. 6). `venta-cliente` arranca en «Elegí un cliente» (valor vacío) para poder probar la venta sin cliente.
- Renglones dinámicos: «Agregar renglón» crea un renglón con el primer libro que no esté en la venta; «Quitar renglón N» lo saca. Si se elige un libro que ya está en otro renglón se informa el duplicado y el selector vuelve al libro anterior (no se crea otro renglón).
- Cálculos: subtotal = cantidad × precio, total = suma de subtotales, todo con dos decimales. Una cantidad inválida cuenta 0 hasta que se corrija.
- Confirmación (`preventDefault`): se rechaza sin cliente, sin renglones, con cantidades que no sean enteros mayores que cero o con libros repetidos. Si pasa, se arma la venta con `id` `V-00N` (desde `DATOS.ventas.length`), fecha, cliente (id y nombre), renglones con copia de título y `precioUnitario` (precio histórico) y total; `DATOS.ventas.push(venta)`, `window.guardarDatos()` si existe, re-render del historial, formulario limpio y mensaje de resultado.
- Historial dinámico desde `DATOS.ventas`: tabla con identificador, fecha, cliente y total, y un `<details>` por venta con sus renglones. Lee solo los datos guardados en la venta, nunca el precio actual del catálogo. No hay botones de editar ni eliminar.
- `js/datos.js` suma `ventas` con la V-001 de ejemplo del Ej. 1.
- `js/altas.js`: al dar de alta un libro o cliente llama a `window.actualizarSelectsVenta()` en lugar de agregar la opción a mano en `venta-libro-1` (que ahora puede no existir).
- `index.html`: solo se agrega `<script src="js/ventas.js"></script>` al final. No se toca `css/`.

## Capabilities

### Modified Capabilities

- `ventas`: el formulario pasa de maqueta a venta funcional con renglones dinámicos, validaciones y confirmación.
- `historial`: pasa de ejemplo estático a render desde `DATOS.ventas` con precio histórico.

## Impact

- Archivo nuevo: `js/ventas.js`.
- Archivos modificados: `js/datos.js` (+`ventas`), `js/altas.js` (opciones de venta), `index.html` (+1 línea `<script>`).
- Contrato con el Ej. 6: `DATOS.ventas`, `window.guardarDatos()` después de confirmar, ids desde `DATOS.ventas.length`, selects desde `DATOS` al cargar.
- Fuera de alcance: stock, cobros, facturación, editar o eliminar ventas, persistencia (Ej. 6).
