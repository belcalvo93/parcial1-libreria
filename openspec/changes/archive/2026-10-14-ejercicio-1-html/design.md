# Design — ejercicio-1-html

## Context

Ver `proposal.md` (Why). Estado actual: el repo tiene `docs/` (consigna en PDF), `prompts/prompt-ejercicio-1.md`, carpetas `css/` y `js/` vacías, y 6 specs delta en este change (`estructura-navegacion`, `catalogo`, `libros`, `clientes`, `ventas`, `historial`). Restricciones duras: una sola página `index.html` con 5 `<section id>` y `nav` de anclas; tablas con `thead`/`th`; `<details>`/`<summary>` para el detalle de ventas; precio unitario como texto no editable; editoriales/géneros/autores fijos como opciones de `select`; formularios con `type` correctos y `novalidate`; un contenedor de error y uno de éxito por formulario con `hidden` e `id`; sin botón «Limpiar venta»; sin CSS, sin JS, sin librerías ni CDNs.

## Goals / Non-Goals

- Goals: estructura HTML navegable por anclas y teclado; formularios y tablas listos para que el JS de los Ejercicios 3–5 los use por `id`; clases reutilizables para el CSS del Ejercicio 2; datos de ejemplo coherentes (6 libros, 2 editoriales, 2 géneros, 3 autores, 3 clientes, ≥1 venta, ≥1 libro «Sin clasificar»).
- Non-Goals (lo resuelve otro ejercicio): estilos (Ejercicio 2), renderizado y filtro por JS (Ejercicio 3), validaciones y altas dinámicas (Ejercicio 4), cálculos y confirmación de ventas (Ejercicio 5), `localStorage` y README de verificación (Ejercicio 6).

## Decisions

### 1. Una sola página con secciones y nav de anclas (decisión vinculante 1)

Se usa un único `index.html` con `<section id="catalogo">`, `<section id="libros">`, `<section id="clientes">`, `<section id="ventas">`, `<section id="historial">`, y un `nav` con cinco enlaces `href="#..."`. Alternativa considerada (varias páginas HTML): se descarta porque duplica `header`/`nav`/`footer`, complica mantener `id` únicos para el JS y exige resolver rutas relativas al abrir con doble clic.

### 2. Convención de nombres

- Archivos: `index.html`, `css/styles.css` (vacío), `js/` reservada sin contenido en este change.
- `id` en kebab-case por dominio: formularios (`form-libro`, `form-cliente`, `form-venta`), campos (`libro-titulo`, `libro-isbn`, `libro-precio`, `libro-editorial`, `libro-genero`, `libro-autores`, `cliente-nombre`, `cliente-email`, `venta-cliente`, `venta-libro-{n}`, `venta-cantidad-{n}`), tablas (`tabla-catalogo`, `tabla-libros`, `tabla-clientes`, `tabla-ventas`, `tabla-historial`), buscador (`catalogo-busqueda`), mensajes (`mensaje-<dominio>-vacia`, `mensaje-catalogo-sin-resultados`, `mensaje-error-<dominio>`, `mensaje-ok-<dominio>` con dominio en `libro | cliente | venta`).
- Clases reutilizables para el Ejercicio 2 (sin tocar el HTML): `card`, `form`, `form__grupo`, `btn`, `btn--peligro` (botones «Quitar»), `tabla`, `mensaje`, `mensaje--error`, `mensaje--ok`. Los `id` son para el JS, las clases para el CSS: no se mezclan.

### 3. Tablas y detalle inline (decisiones 2 y 3)

Catálogo, listados, renglones de venta e historial son `<table>` con `thead`/`th`. El detalle de cada venta va en `<details>`/`<summary>` dentro de su fila o bloque, sin navegar. Alternativa (página de detalle por venta): se descarta por la decisión de página única y porque el historial debe consultarse sin salir.

### 4. Formularios pensados para el JS futuro (decisiones 4, 7, 8, 9)

- `type` correctos (`search`, `text`, `email`, `number`) con `min` en cantidades y precios, pero cada `<form>` lleva `novalidate`: el navegador no valida, los mensajes los pone el JS del Ejercicio 4/5.
- El precio unitario del renglón es texto no editable (`<output>` o `<span>` con `id`), nunca un `input`: así se expresa que se copia del catálogo y que el historial conserva el valor confirmado.
- Por formulario (`form-libro`, `form-cliente`, `form-venta`): un contenedor de error (`role="alert"`) y uno de éxito, ubicados inmediatamente después del formulario (o dentro, al pie), con `hidden` e `id` (`mensaje-error-*`, `mensaje-ok-*`). Los mensajes de lista vacía y sin resultados van junto a su tabla con `hidden` e `id` (`mensaje-*-vacia`, `mensaje-catalogo-sin-resultados`).
- Solo «Agregar renglón», «Quitar» (por renglón) y «Confirmar venta»: no hay «Limpiar venta» porque la limpieza la hace la confirmación del Ejercicio 5.

### 5. Datos de ejemplo coherentes (decisión 6, nombre fijo)

Nombre: «Librería Página Viva». Conjunto mínimo coherente: 2 editoriales, 2 géneros, 3 autores (las opciones de los `select` salen de acá, sin ABM), 6 libros (cada uno con editorial y ≥1 autor existentes; género opcional pero existente si se asigna; al menos uno sin género → «Sin clasificar»), 3 clientes y ≥1 venta (identificador, fecha, cliente existente, renglones con precio unitario igual al del catálogo, total igual a la suma de subtotales). Estos mismos valores los reutiliza el JS del Ejercicio 3 como conjunto inicial.

## Risks / Trade-offs

- [Duplicación catálogo/listado de libros] → Mitigación: en este change es HTML estático a propósito; el Ejercicio 3 lo genera desde un único array y elimina la duplicación.
- [`id` inestables rompen la cadena] → Mitigación: la convención de `id` de §2 queda congelada al archivar; cualquier renombre posterior exige coordinar con el integrante del Ejercicio 3.
- [Tabla ancha en 320 px] → Lo resuelve el Ejercicio 2 con contenedor de desplazamiento propio; este change solo asegura estructura de tabla correcta.
- [`novalidate` muestra formularios sin validación nativa] → Es lo pedido: evita mensajes del navegador en inglés y deja el control al JS del Ejercicio 4.

## Migration Plan

No hay migración: el change solo agrega `index.html` y `css/styles.css` vacío sobre carpetas vacías. Rollback: borrar esos archivos. Al archivar, las 6 specs delta pasan a `openspec/specs/` como verdad para los Ejercicios 2–6.
