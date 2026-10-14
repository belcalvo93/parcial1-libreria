# Design — ejercicio-1-html

## Context

Ver `proposal.md` (Why). Estado actual: el repo tiene `docs/` (consigna en PDF), `prompts/prompt-ejercicio-1.md`, carpetas `css/` y `js/` vacías, y 6 specs delta en este change (`estructura-navegacion`, `catalogo`, `libros`, `clientes`, `ventas`, `historial`). Restricciones duras: una sola página `index.html` con 5 `<section id>` y `nav` de anclas; tablas con `thead`/`th`; `<details>`/`<summary>` para el detalle de ventas; precio unitario como texto no editable; editoriales/géneros/autores fijos como opciones de `select`; formularios con `type` correctos y `novalidate`; un contenedor de error y uno de éxito por formulario con `hidden` e `id`; sin botón «Limpiar venta»; sin CSS, sin JS, sin librerías ni CDNs.

## Goals / Non-Goals

- Goals: estructura HTML navegable por anclas y teclado; formularios y tablas listos para que el JS de los Ejercicios 3–5 los use por `id`; clases reutilizables para el CSS del Ejercicio 2; datos de ejemplo coherentes (6 libros, 2 editoriales, 2 géneros, 3 autores, 3 clientes, ≥1 venta, ≥1 libro «Sin clasificar»).
- Non-Goals (lo resuelve otro ejercicio): estilos (Ejercicio 2), renderizado y filtro por JS (Ejercicio 3), validaciones y altas dinámicas (Ejercicio 4), cálculos y confirmación de ventas (Ejercicio 5), `localStorage` y README de verificación (Ejercicio 6).

## Decisions

### 1. Una sola página con secciones y nav de anclas (decisión vinculante 1)

Se usa un único `index.html` con `<section id="catalogo">`, `<section id="libros">`, `<section id="clientes">`, `<section id="ventas">`, `<section id="historial">`, y un `nav` con cinco enlaces `href="#..."`. Alternativa considerada (varias páginas HTML): se descarta porque duplica `header`/`nav`/`footer`, complica mantener `id` únicos para el JS y exige resolver rutas relativas al abrir con doble clic.

### 2. Convención de nombres e inventario de `id` congelado

- Archivos: `index.html`, `css/styles.css` (vacío), `js/` reservada sin contenido en este change.
- `id` en kebab-case por dominio. Inventario cerrado (congelado al archivar, el JS de los Ejercicios 3–5 los usa tal cual):
  - Secciones: `catalogo`, `libros`, `clientes`, `ventas`, `historial`.
  - Formularios: `form-libro`, `form-cliente`, `form-venta`.
  - Campos catálogo: `catalogo-busqueda` (`type="search"`), botón `catalogo-buscar`.
  - Campos libro: `libro-titulo`, `libro-isbn`, `libro-precio`, `libro-editorial`, `libro-genero`, `libro-autores`.
  - Campos cliente: `cliente-nombre`, `cliente-email`.
  - Campos venta: `venta-cliente`, y por renglón `{n}` (1-based): `venta-libro-{n}`, `venta-cantidad-{n}`, `venta-precio-{n}` (`<output>`, no editable), `venta-subtotal-{n}` (`<output>`), `venta-quitar-{n}` (botón «Quitar» del renglón). Resumen: `venta-total` (`<output>`). Botones: `venta-agregar` («Agregar renglón»), `venta-confirmar` («Confirmar venta»).
  - Tablas (4, sin `tabla-ventas`: ventas no lleva tabla, lleva renglones + resumen): `tabla-catalogo`, `tabla-libros`, `tabla-clientes`, `tabla-historial`.
  - Mensajes (11, todos con `hidden`): `mensaje-catalogo-vacia`, `mensaje-catalogo-sin-resultados`, `mensaje-libros-vacia`, `mensaje-clientes-vacia`, `mensaje-historial-vacia`, `mensaje-error-libro`, `mensaje-ok-libro`, `mensaje-error-cliente`, `mensaje-ok-cliente`, `mensaje-error-venta`, `mensaje-ok-venta`.
- Clases reutilizables para el Ejercicio 2 — contrato con el Ej. 2 (sin tocar el HTML): `card`, `form`, `form__grupo`, `btn`, `btn--peligro` (botones «Quitar»), `tabla`, `mensaje`, `mensaje--error`, `mensaje--ok`. Los `id` son para el JS, las clases para el CSS: no se mezclan.

### 3. Tablas y detalle inline (decisiones 2 y 3)

Catálogo, listados, renglones de venta e historial son `<table>` con `thead`/`th`. El detalle de cada venta va en `<details>`/`<summary>` dentro de su fila o bloque, sin navegar. Alternativa (página de detalle por venta): se descarta por la decisión de página única y porque el historial debe consultarse sin salir.

### 4. Formularios pensados para el JS futuro (decisiones 4, 7, 8, 9)

- `type` correctos (`search`, `text`, `email`, `number`) con `min` en cantidades y precios, pero cada `<form>` lleva `novalidate`: el navegador no valida, los mensajes los pone el JS del Ejercicio 4/5.
- El precio unitario del renglón es texto no editable (`<output>` o `<span>` con `id`), nunca un `input`: así se expresa que se copia del catálogo y que el historial conserva el valor confirmado.
- Por formulario (`form-libro`, `form-cliente`, `form-venta`): un contenedor de error (`role="alert"`) y uno de éxito, ubicados inmediatamente después del formulario (o dentro, al pie), con `hidden` e `id` (`mensaje-error-*`, `mensaje-ok-*`). Los mensajes de lista vacía y sin resultados van junto a su tabla con `hidden` e `id` (`mensaje-*-vacia`, `mensaje-catalogo-sin-resultados`).
- Solo «Agregar renglón», «Quitar» (por renglón) y «Confirmar venta»: no hay «Limpiar venta» porque la limpieza la hace la confirmación del Ejercicio 5.

### 5. Datos de ejemplo congelados (decisión 6, nombre fijo)

Nombre: «Librería Página Viva». Valores ficticios, congelados al archivar; el JS del Ejercicio 3 los reutiliza como conjunto inicial. Todos los ISBN son inventados, de 13 dígitos y distintos.

Editoriales (2, opciones fijas de `libro-editorial`, sin ABM):
- Editorial Faro Austral
- Ediciones Puente Alto

Géneros (2, opciones fijas de `libro-genero` más «Sin clasificar», sin ABM):
- Fantasía Urbana
- Ensayo Breve

Autores (3, opciones fijas de `libro-autores` múltiple, sin ABM):
- Lucía Ferreyra
- Tomás Iriarte
- Camila Duarte

Libros (6; columnas: título | ISBN | precio | editorial | género | autores):
1. El mapa de las horas lentas | 9789500100011 | 1500.00 | Editorial Faro Austral | Fantasía Urbana | Lucía Ferreyra — *renglón 1 de V-001*
2. Crónicas del patio interior | 9789500100028 | 2000.00 | Ediciones Puente Alto | Ensayo Breve | Tomás Iriarte — *renglón 2 de V-001*
3. Noche de vidrio molido | 9789500100035 | 1750.00 | Editorial Faro Austral | Sin clasificar | Camila Duarte
4. Manual para perder trenes | 9789500100042 | 2300.00 | Ediciones Puente Alto | Fantasía Urbana | Lucía Ferreyra, Tomás Iriarte — *el de 2 autores*
5. El oficio de la marea | 9789500100059 | 3100.00 | Editorial Faro Austral | Ensayo Breve | Camila Duarte
6. Inventario de luces mínimas | 9789500100066 | 2800.00 | Ediciones Puente Alto | Sin clasificar | Tomás Iriarte

Clientes (3; nombre completo | email):
1. Valentina Ríos | valentina.rios@ejemplo.com — *cliente de V-001*
2. Martín Aguirre | martin.aguirre@ejemplo.com
3. Sofía Benítez | sofia.benitez@ejemplo.com

Venta V-001 (única venta de ejemplo; cliente 1, fecha 2026-10-14):
- Renglón 1: El mapa de las horas lentas × 2 @ 1500.00 = subtotal 3000.00
- Renglón 2: Crónicas del patio interior × 1 @ 2000.00 = subtotal 2000.00
- Total: 5000.00 (= 3000.00 + 2000.00; cada precio unitario coincide con el catálogo).

Reglas de coherencia: cada libro tiene editorial y ≥1 autor de las listas; el género es opcional pero si existe es uno de los 2; la venta usa cliente y libros existentes con precios copiados del catálogo y total igual a la suma de subtotales.

## Datos → HTML (dónde va cada valor)

- `tabla-catalogo` y `tabla-libros`: las 6 filas de libros con sus 6 columnas.
- `libro-editorial` / `libro-genero` (+«Sin clasificar») / `libro-autores`: opciones salen de las listas de arriba.
- `tabla-clientes` y `venta-cliente`: los 3 clientes.
- `form-venta` (renglón de ejemplo): `venta-libro-1` = El mapa de las horas lentas, `venta-cantidad-1` = 2, `venta-precio-1` = 1500.00, `venta-subtotal-1` = 3000.00, `venta-total` = 5000.00.
- `tabla-historial` + `<details>`/`<summary>` de V-001: identificador V-001, fecha 2026-10-14, cliente Valentina Ríos, total 5000.00, con sus 2 renglones y subtotales.

## Risks / Trade-offs

- [Duplicación catálogo/listado de libros] → Mitigación: en este change es HTML estático a propósito; el Ejercicio 3 lo genera desde un único array y elimina la duplicación.
- [`id` inestables rompen la cadena] → Mitigación: la convención de `id` de §2 queda congelada al archivar; cualquier renombre posterior exige coordinar con el integrante del Ejercicio 3.
- [Tabla ancha en 320 px] → Lo resuelve el Ejercicio 2 con contenedor de desplazamiento propio; este change solo asegura estructura de tabla correcta.
- [`novalidate` muestra formularios sin validación nativa] → Es lo pedido: evita mensajes del navegador en inglés y deja el control al JS del Ejercicio 4.

## Migration Plan

No hay migración: el change solo agrega `index.html` y `css/styles.css` vacío sobre carpetas vacías. Rollback: borrar esos archivos. Al archivar, las 6 specs delta pasan a `openspec/specs/` como verdad para los Ejercicios 2–6.
