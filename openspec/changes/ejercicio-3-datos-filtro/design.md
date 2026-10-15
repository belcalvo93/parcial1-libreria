# Design — ejercicio-3-datos-filtro

## Context

Ver `proposal.md` (Why). Estado actual: `index.html` con 5 vistas y datos hardcodeados en el HTML (Ejercicio 1), `css/styles.css` completo (Ejercicio 2). Restricciones duras: no tocar `index.html` salvo agregar los `<script>` al final, no tocar `css/styles.css`, no cambiar `id` ni clases del HTML. Los mensajes `#mensaje-...` vienen con `hidden` y el JS solo los muestra/oculta.

## Goals / Non-Goals

- Goals: render dinámico del catálogo desde arrays JS + filtro por título en tiempo real + datos coherentes con el HTML.
- Non-Goals: validaciones, altas (Ejercicio 4), ventas/historial dinámicos (Ejercicio 5), localStorage, ABM de editoriales/géneros/autores.

## Decisions

### 1. Estructura de datos

Cada entidad tiene `id` estable (kebab-case). Referencias por id, nunca por nombre:

- `DATOS.editoriales`: `[{ id, nombre }]` — 2 (`faro-austral`, `puente-alto`).
- `DATOS.generos`: `[{ id, nombre }]` — 2 (`fantasia-urbana`, `ensayo-breve`). No incluye “Sin clasificar”: la ausencia se representa con `null`.
- `DATOS.autores`: `[{ id, nombre }]` — 3 (`lucia-ferreyra`, `tomas-iriarte`, `camila-duarte`).
- `DATOS.libros`: `[{ id, titulo, isbn, precio, editorialId, generoId | null, autorIds: [] }]`. 6 libros con los valores congelados del design archivado del Ej. 1 (§5). “Manual para perder trenes” lleva `autorIds` con 2 entradas; los libros sin género llevan `generoId: null`.
- `DATOS.clientes`: `[{ id, nombre, email }]` — 3 clientes congelados. (Las ventas quedan para el Ej. 5 de Bruno: este change no crea `DATOS.ventas` ni toca historial.)

### 2. Render dinámico con textContent (nunca innerHTML)

`renderCatalogo(filtro = "")` vacía `#tabla-catalogo tbody` y por cada libro filtrado crea `tr`/`td` con `document.createElement` asignando cada celda con `textContent`. Orden de columnas igual al HTML: título | ISBN | editorial (nombre resuelto por `editorialId`) | género (nombre resuelto o “Sin clasificar” si `null`) | autores (nombres unidos con “, ”) | precio con 2 decimales. Al cargar (`DOMContentLoaded`) se llama `renderCatalogo()` sin filtro. No se agregan clases, estilos inline ni columnas nuevas: el CSS adaptable del Ej. 2 sigue intacto.

### 3. Filtro case-insensitive con trim

Normalización en ambos lados: `libro.titulo.toLowerCase().includes(filtro.trim().toLowerCase())`. Coincidencia parcial. Si el texto recortado está vacío, muestra todo. Si no hay resultados, muestra `#mensaje-catalogo-sin-resultados` y oculta la tabla (no la destruye). Eventos: `input` en `#catalogo-busqueda` (tiempo real) + `click` en `#catalogo-buscar` (teclado/Enter incluido). El botón no es decorativo: repite el mismo `renderCatalogo` con el valor actual.

### 4. Mensajes de estado

- `#mensaje-catalogo-vacia`: se muestra si `DATOS.libros` está vacío.
- `#mensaje-catalogo-sin-resultados`: se muestra si el filtro no coincide con ningún libro.
- Ambos se controlan con `elemento.hidden = true/false`.

### 5. Enlace de scripts

Se agregan antes de `</body>`:
```html
<script src="js/datos.js"></script>
<script src="js/catalogo.js"></script>
```

## Risks / Trade-offs

- [Duplicación catálogo/listado de libros en el HTML estático] → El JS reemplaza el HTML estático al renderizar. El HTML estático queda como fallback si el JS no carga.
- [ID inestables] → No se tocan los IDs del HTML existente.

## Migration Plan

No hay migración de datos. El cambio solo agrega `js/datos.js`, `js/catalogo.js` y modifica `index.html` para enlazar los scripts. Rollback: revertir el commit.
