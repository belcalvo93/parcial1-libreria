# Tasks — ejercicio-1-html

> Alcance estricto: solo estructura HTML. Nada de CSS, JavaScript, validaciones, cálculos ni `localStorage` (Ejercicios 2–6).

## 1. Cáscara global y navegación

- [ ] 1.1 Crear `index.html` con `header` («Librería Página Viva»), `nav` con 5 enlaces de ancla, `main` con 5 `<section id>`, `footer`, `<link>` a `css/styles.css` y `css/styles.css` vacío; verificar abriendo `index.html` con doble clic y recorriendo las 5 vistas solo con Tab y Enter
- [ ] 1.2 Aplicar clases reutilizables (`card`, `form`, `btn`, `tabla`, `mensaje`, `mensaje--error`, `mensaje--ok`) e `id` según la convención de `design.md` §2; verificar que no haya estilos propios, `<script>`, ni referencias externas/CDN

## 2. Catálogo

- [ ] 2.1 Agregar buscador (`type="search"` con `label`) y tabla `tabla-catalogo` con `thead`/`th` (título, ISBN, editorial, género, autores, precio) y 6 libros de ejemplo con al menos uno «Sin clasificar»; verificar que cada fila muestra los 6 datos
- [ ] 2.2 Agregar mensajes `mensaje-catalogo-vacia` y `mensaje-catalogo-sin-resultados` con `hidden`; verificar que existen, están ocultos y tienen `id` claro

## 3. Libros y clientes

- [ ] 3.1 Agregar `form-libro` con `novalidate`, los 6 campos con `label` (`for`/`id`), `type`/`min` correctos, `select` de editorial/género (con «Sin clasificar»)/autores múltiple con las opciones fijas, botón descriptivo, contenedores `mensaje-error-libro` y `mensaje-ok-libro` con `hidden`, tabla `tabla-libros` con `thead`/`th` y mensaje `mensaje-libros-vacia` con `hidden`; verificar cada `for`/`id`, el `novalidate` y que editoriales/géneros/autores no tienen ABM
- [ ] 3.2 Agregar `form-cliente` con `novalidate` (nombre `type="text"`, email `type="email"`, ambos con `label`), botón descriptivo, contenedores `mensaje-error-cliente` y `mensaje-ok-cliente` con `hidden`, tabla `tabla-clientes` con `thead`/`th` (3 clientes) y mensaje `mensaje-clientes-vacia` con `hidden`; verificar cada `for`/`id` y el `novalidate`

## 4. Ventas e historial

- [ ] 4.1 Agregar `form-venta` con `novalidate`, `select` de cliente, un renglón de ejemplo (libro, cantidad `type="number"` con `min`, precio unitario como texto no editable igual al catálogo, subtotal), botones «Agregar renglón» / «Quitar» / «Confirmar venta» (sin botón de limpiar), resumen con total y contenedores `mensaje-error-venta` y `mensaje-ok-venta` con `hidden`; verificar que no existe botón «Limpiar venta» y que el precio unitario no es editable
- [ ] 4.2 Agregar `tabla-historial` con `thead`/`th` (identificador, fecha, cliente, total), al menos 1 venta de ejemplo coherente con el catálogo (total igual a la suma de subtotales), detalle por venta en `<details>`/`<summary>` inline y mensaje `mensaje-historial-vacia` con `hidden`; verificar que el detalle se abre sin salir de la página

## 5. Verificación integral

- [ ] 5.1 Repasar los 14 criterios de aceptación de `prompts/prompt-ejercicio-1.md` sobre `index.html` (5 vistas navegables, semántica, `label`/`id`, botones, `thead`/`th`, «Sin clasificar», mensajes con `hidden`/`id`, 6/2/2/3/3/≥1 datos, sin CSS/JS, sin CDN, `openspec/` versionada) y validar con `openspec validate --change "ejercicio-1-html"`; verificar que la validación pasa y cada criterio se cumple a mano en el navegador

## Workflow follow-up

- Revisar este propose con el profe antes del apply (frena acá por decisión del usuario).
- Tras aprobar: implementar con apply siguiendo este `tasks.md`.
- Tras confirmar que todo funciona: sincronizar specs y archivar con `openspec archive "ejercicio-1-html"`.
