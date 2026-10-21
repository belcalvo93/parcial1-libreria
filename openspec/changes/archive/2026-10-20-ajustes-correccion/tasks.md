# Tasks — ajustes-correccion

> Alcance: `index.html` (5 envoltorios y footer), `css/styles.css` (sección 10) y `js/ventas.js` (solo `crearDetalle`). Sin `innerHTML`. No tocar el resto de los `.js` ni los changes archivados.

## 1. Contenedor de tablas en el HTML

- [x] 1.1 Envolver `#tabla-catalogo`, `#tabla-libros`, `#tabla-clientes` y `#tabla-historial` en `<div class="tabla-contenedor">…</div>`, sin cambiar ids, clases ni contenido de las tablas; verificar abriendo `index.html` que las cuatro tablas siguen mostrando las mismas filas
- [x] 1.2 Envolver la tabla estática del `<details>` de V-001 en `<div class="tabla-contenedor">…</div>`; verificar que el detalle abre y muestra las dos filas

## 2. Contenedor de tablas en el JS

- [x] 2.1 En `crearDetalle` de `js/ventas.js`, crear `var contenedor = document.createElement("div")`, asignarle `className = "tabla-contenedor"`, agregar la tabla dentro y el contenedor al `<details>` en lugar de la tabla; verificar confirmando una venta y abriendo su detalle

## 3. Estilos

- [x] 3.1 En la sección 10 de `css/styles.css`, agregar `.tabla-contenedor` con `width:100%`, `max-width:100%`, `overflow-x:auto` y `-webkit-overflow-scrolling:touch`
- [x] 3.2 En `.tabla`, quitar `display:block` y `overflow-x`, mantener `width:100%`, agregar `border-collapse:collapse`; conservar el `white-space:nowrap` de `th`/`td` y actualizar el comentario de la sección
- [x] 3.3 Verificar a 320 px que la tabla del catálogo tiene scroll solo dentro de `.tabla-contenedor` y la página no tiene scroll horizontal; verificar a 1280 px que no hay scroll interno

## 4. Pie de página

- [x] 4.1 En `index.html`, reemplazar «Librería Página Viva — TPI Programación III» por «© 2026 Librería Página Viva»; verificar el texto renderizado en el `footer`

## 5. Validación

- [x] 5.1 Ejecutar `openspec validate "ajustes-correccion" --type change` y confirmar que pasa sin errores

## Workflow follow-up

- Archive the change after the project's review requirements are satisfied.
