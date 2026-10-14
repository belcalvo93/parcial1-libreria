# Proposal — ejercicio-1-html

## Why

El TPI de la librería necesita una base HTML completa antes de estilizar (Ejercicio 2) y programar el comportamiento (Ejercicios 3–6). Sin esta estructura, cada integrante posterior trabajaría sobre una maqueta distinta y los criterios de aceptación del Ejercicio 1 (20 puntos: 10 de vistas y navegación + 10 de semántica, formularios y estados) quedarían sin verificar.

## What Changes

- Se crea `index.html` como único punto de entrada, con 5 `<section id>` (catálogo, libros, clientes, ventas, historial) y un `nav` de anclas que permite ir de cualquier vista a cualquier vista.
- Cada vista incluye su formulario y/o listado con etiquetas semánticas (`header`, `nav`, `main`, `section`, `footer`), `label` asociado a cada campo (`for`/`id`), botones con texto descriptivo y tablas con `thead`/`th`.
- Se prevén todos los mensajes de estado que el JS de los Ejercicios 3–5 va a mostrar u ocultar: listas vacías, búsqueda sin resultados, éxito y error por formulario, cada uno con `hidden` e `id` claro.
- Se cargan datos estáticos de ejemplo coherentes con el Ejercicio 3: 6 libros, 2 editoriales, 2 géneros, 3 autores, 3 clientes, al menos 1 venta y al menos un libro «Sin clasificar». Nombre de la librería: «Librería Página Viva».
- Se enlaza `css/styles.css` vacío desde el HTML. No se escribe CSS ni JavaScript. No se usa ninguna librería, framework ni CDN.

## Capabilities

### New Capabilities

- `estructura-navegacion`: cáscara global de una sola página (header, nav de anclas, main con 5 secciones, footer), convención de clases reutilizables para el Ejercicio 2, contenedores de mensajes con `hidden` e `id`, enlace a `css/styles.css` y regla de cero dependencias externas.
- `catalogo`: vista de catálogo con título de la librería, buscador por título y tabla de libros (título, ISBN, editorial, género, autores, precio; «Sin clasificar» si no hay género), más mensajes de lista vacía y sin resultados.
- `libros`: formulario de alta de libro (título, ISBN, precio, editorial, género con «Sin clasificar», autores múltiple) con `novalidate` y mensajes de éxito/error, más listado de libros registrados.
- `clientes`: formulario de alta de cliente (nombre completo, email) con `novalidate` y mensajes de éxito/error, más listado de clientes registrados.
- `ventas`: formulario de venta (selector de cliente, renglones con libro, cantidad, precio unitario como texto no editable y subtotal, botones «Agregar renglón» / «Quitar» por renglón, resumen con total y botón «Confirmar venta») con `novalidate` y mensajes de éxito/error. Sin botón «Limpiar venta».
- `historial`: listado de ventas (identificador, fecha, cliente, total) con detalle por venta en `<details>`/`<summary>` inline y mensaje de lista vacía.

### Modified Capabilities

(none — el proyecto todavía no tiene specs; `openspec list --specs` devuelve vacío)

## Impact

- Archivos nuevos: `index.html`, `css/styles.css` (vacío), artefactos de este change en `openspec/changes/ejercicio-1-html/` y, al archivar, 6 specs nuevas en `openspec/specs/`.
- Sin impacto en código existente: `css/` y `js/` están vacías, no hay nada que romper.
- Condiciona a los Ejercicios 2–6: el CSS (Ejercicio 2) estiliza las clases aquí definidas sin tocar el HTML; el JS (Ejercicios 3–5) usa los `id` de campos, tablas y mensajes; la persistencia y verificación (Ejercicio 6) asumen estos datos de ejemplo. Cambiar `id` o clases después del archive cuesta retrabajo en cadena.
- Fuera de alcance explícito: estilos, JavaScript, validaciones, cálculos, `localStorage`, ABM de editoriales/géneros/autores (son opciones fijas de los `select`), edición o borrado de registros, y edición de precios.
