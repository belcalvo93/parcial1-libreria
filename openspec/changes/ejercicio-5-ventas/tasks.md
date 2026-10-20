# Tasks — ejercicio-5-ventas

> Alcance: JS de ventas e historial. No tocar `css/`. Solo `textContent`/`createElement` y `hidden`, nunca `innerHTML` ni `style.display`.

## 1. Datos

- [ ] 1.1 Agregar `ventas` a `DATOS` en `js/datos.js` con la V-001 del Ej. 1 (2 × 1500 + 1 × 2000 = 5000)

## 2. Formulario de venta

- [ ] 2.1 Al cargar, poblar `venta-cliente` desde `DATOS.clientes` con «Elegí un cliente» primero y reemplazar el renglón estático por uno generado desde `DATOS.libros`
- [ ] 2.2 «Agregar renglón» con el primer libro libre y «Quitar renglón N»
- [ ] 2.3 Rechazar libro repetido al cambiar el selector (mensaje y vuelve al libro anterior)
- [ ] 2.4 Precio unitario, subtotal y total con dos decimales, actualizados al cambiar libro o cantidad
- [ ] 2.5 Exponer `window.actualizarSelectsVenta()` y usarla desde `js/altas.js` después de cada alta

## 3. Confirmación

- [ ] 3.1 `submit` con `preventDefault`; validar cliente, renglones, cantidades enteras > 0 y repetidos
- [ ] 3.2 Armar la venta (`V-00N`, fecha, cliente, renglones con título y `precioUnitario` copiados, total), `DATOS.ventas.push` y `window.guardarDatos()` si existe
- [ ] 3.3 Limpiar el formulario y mostrar `mensaje-ok-venta` con el identificador

## 4. Historial

- [ ] 4.1 Envolver el `<details>` estático en `<div id="historial-detalles">` y agregar `<script src="js/ventas.js"></script>` en `index.html`
- [ ] 4.2 `renderHistorial()` desde `DATOS.ventas` con fila y `<details>` por venta; mensaje de historial vacío

## 5. Verificación

- [ ] 5.1 Probar en el navegador: venta sin cliente, cantidad 0, negativa y decimal, libro repetido, venta válida 2 × 1500 + 1 × 2000
- [ ] 5.2 Cambiar el precio de un libro después de confirmar y verificar que el historial no cambia
- [ ] 5.3 `grep -r innerHTML js/` sin resultados y `openspec validate ejercicio-5-ventas`
