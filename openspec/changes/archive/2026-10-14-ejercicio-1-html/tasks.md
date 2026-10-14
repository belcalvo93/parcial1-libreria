# Tasks — ejercicio-1-html

> Alcance estricto: solo estructura HTML. Nada de CSS, JavaScript, validaciones, cálculos ni `localStorage` (Ejercicios 2–6).

## 1. Cáscara global y navegación

- [x] 1.1 Crear `index.html` con `header` («Librería Página Viva»), `nav` con 5 enlaces de ancla, `main` con 5 `<section id>`, `footer`, `<link>` a `css/styles.css` y `css/styles.css` vacío; verificar abriendo `index.html` con doble clic y recorriendo las 5 vistas solo con Tab y Enter
- [x] 1.2 Aplicar las 9 clases del contrato con el Ej. 2 (`card`, `form`, `form__grupo`, `btn`, `btn--peligro`, `tabla`, `mensaje`, `mensaje--error`, `mensaje--ok`) e `id` según el inventario congelado de `design.md` §2 (sin `tabla-ventas`); verificar que no haya estilos propios, `<script>`, ni referencias externas/CDN

## 2. Catálogo

- [x] 2.1 Agregar buscador (`catalogo-busqueda` `type="search"` con `label`, botón `catalogo-buscar`) y tabla `tabla-catalogo` con `thead`/`th` (título, ISBN, editorial, género, autores, precio) y los 6 libros congelados de `design.md` §5 (2 «Sin clasificar», uno con 2 autores); verificar que cada fila muestra los 6 datos
- [x] 2.2 Agregar mensajes `mensaje-catalogo-vacia` y `mensaje-catalogo-sin-resultados` con `hidden`; verificar que existen, están ocultos y tienen `id` claro

## 3. Libros y clientes

- [x] 3.1 Agregar `form-libro` con `novalidate`, los 6 campos con `label` (`for`/`id`), `type`/`min` correctos, `select` de editorial/género (con «Sin clasificar»)/autores múltiple con las opciones congeladas de `design.md` §5, botón descriptivo, contenedores `mensaje-error-libro` y `mensaje-ok-libro` con `hidden`, tabla `tabla-libros` con `thead`/`th` (6 libros congelados) y mensaje `mensaje-libros-vacia` con `hidden`; verificar cada `for`/`id`, el `novalidate` y que editoriales/géneros/autores no tienen ABM
- [x] 3.2 Agregar `form-cliente` con `novalidate` (nombre `type="text"`, email `type="email"`, ambos con `label`), botón descriptivo, contenedores `mensaje-error-cliente` y `mensaje-ok-cliente` con `hidden`, tabla `tabla-clientes` con `thead`/`th` (3 clientes congelados de `design.md` §5) y mensaje `mensaje-clientes-vacia` con `hidden`; verificar cada `for`/`id` y el `novalidate`

## 4. Ventas e historial

- [x] 4.1 Agregar `form-venta` con `novalidate`, `select` `venta-cliente` (3 clientes congelados), renglón de ejemplo según `design.md` §5 (`venta-libro-1`, `venta-cantidad-1` `type="number"` con `min`, `venta-precio-1`/`venta-subtotal-1` como `<output>` no editable, `venta-total` = 5000.00), botones `venta-agregar` («Agregar renglón») / `venta-quitar-1` («Quitar») / `venta-confirmar` («Confirmar venta») sin botón de limpiar, resumen con total y contenedores `mensaje-error-venta` y `mensaje-ok-venta` con `hidden`; verificar que no existe botón «Limpiar venta» y que el precio unitario no es editable
- [x] 4.2 Agregar `tabla-historial` con `thead`/`th` (identificador, fecha, cliente, total), la venta V-001 congelada de `design.md` §5 (cliente 1, 2026-10-14, 2×1500.00 + 1×2000.00 = 5000.00), detalle en `<details>`/`<summary>` inline y mensaje `mensaje-historial-vacia` con `hidden`; verificar que el detalle se abre sin salir de la página

## 5. Verificación integral

- [x] 5.1 Repasar los 14 criterios de aceptación de `prompts/prompt-ejercicio-1.md` sobre `index.html` (5 vistas navegables, semántica, `label`/`id`, botones, `thead`/`th`, «Sin clasificar», mensajes con `hidden`/`id`, 6/2/2/3/3/≥1 datos, sin CSS/JS, sin CDN, `openspec/` versionada) y validar con `openspec validate --change "ejercicio-1-html"`; verificar que la validación pasa y cada criterio se cumple a mano en el navegador

## Workflow follow-up

- Revisar este propose con el profe antes del apply (frena acá por decisión del usuario).
- Tras aprobar: implementar con apply siguiendo este `tasks.md`.
- Tras confirmar que todo funciona: sincronizar specs y archivar con `openspec archive "ejercicio-1-html"`.
