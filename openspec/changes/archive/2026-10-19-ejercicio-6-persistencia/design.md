# Design — ejercicio-6-persistencia

## Context

Ver `proposal.md`. Estado: Ej. 1 a 4 archivados. `DATOS` (global en `js/datos.js`) tiene `editoriales`, `generos`, `autores`, `libros`, `clientes`; `ventas` la agrega el Ej. 5 (Bruno). `js/catalogo.js` y `js/altas.js` leen `DATOS` en su `DOMContentLoaded`. Restricciones de siempre: JS nativo, `file://` con doble clic, sin `innerHTML`, sin tocar `css/`.

## Goals / Non-Goals

- Goals: conservar libros, clientes y ventas entre recargas; primer inicio con ejemplos; recarga sin duplicados; README con casos manuales y límites de `localStorage`; `.zip` de entrega.
- Non-Goals: editar precios desde la interfaz, compartir datos entre usuarios/navegadores, persistir editoriales, géneros y autores.

## Decisions

### 1. Un archivo `js/persistencia.js` cargado entre `datos.js` y `catalogo.js`

Script clásico que se ejecuta apenas se carga (no espera `DOMContentLoaded`). Así, cuando `catalogo.js`, `altas.js` y el JS de ventas renderizan, `DATOS` ya tiene lo guardado. Es la única línea de `index.html` que cambia.

### 2. Una clave por colección con prefijo `paginaViva.`

| Clave | Contenido |
|---|---|
| `paginaViva.libros` | array de libros (`id`, `titulo`, `isbn`, `precio`, `editorialId`, `generoId`, `autorIds`) |
| `paginaViva.clientes` | array de clientes (`id`, `nombre`, `email`) |
| `paginaViva.ventas` | array de ventas confirmadas (estructura del Ej. 5) |

Claves separadas permiten la comprobación guiada del docente: modificar solo el precio en `paginaViva.libros` sin tocar `paginaViva.ventas`. El prefijo evita choques: en Chrome/Edge todas las páginas `file://` comparten el mismo `localStorage` (verificado en Chromium).

### 3. Reemplazar, no sumar

`cargarDatos()`: por cada colección, si hay un array guardado válido, `DATOS[nombre] = guardado`; si no hay nada (primer inicio) se quedan los ejemplos. Al final llama a `guardarDatos()`, que en el primer inicio deja los ejemplos escritos. Como nunca se concatena, recargar N veces no duplica nada.

### 4. `guardarDatos()` explícito y expuesto en `window`

Se llama después de cada alta válida (`altas.js`, helper `persistir()` que verifica que la función exista) y después de confirmar una venta (Ej. 5). Mismo patrón que `window.renderCatalogo` del Ej. 3. Se descartó guardar en `beforeunload`/`pagehide` porque si el navegador se cierra mal se pierde el último cambio.

### 5. Tolerancia a fallos

- `localStorage` no disponible (modo privado, permisos): `almacenamientoDisponible()` lo detecta con `try/catch`; la app sigue en memoria y avisa por consola.
- JSON roto o que no es array: esa colección vuelve a los ejemplos y se reescribe.
- Cuota llena al guardar: `guardarDatos()` devuelve `false` y la app sigue andando.

### 6. Contrato con el Ejercicio 5 (ventas)

- Ventas en `DATOS.ventas` (si `datos.js` no la define, `persistencia.js` la inicializa en `[]`).
- Cada renglón guarda copia de `precioUnitario` y del título del libro: el historial no consulta el precio actual del catálogo (precio histórico).
- Al confirmar: `DATOS.ventas.push(venta)` y `window.guardarDatos()`.
- El identificador `V-00N` se calcula a partir de `DATOS.ventas` (ya recuperado), para no repetir ids tras recargar.
- Los selectores de venta (`venta-cliente`, `venta-libro-*`) se pueblan desde `DATOS` al cargar, para que aparezcan los libros y clientes recuperados.

## Risks / Trade-offs

- [Datos por navegador, perfil y ubicación] → Se documenta en el README; no es una base de datos compartida.
- [JSON roto pierde esa colección] → Aceptable para un TPI; se vuelve a un estado conocido en vez de romper la página.
- [Edición manual del precio en DevTools] → El catálogo muestra el precio nuevo; las ventas conservan el histórico porque lo copian.

## Migration Plan

Primer inicio después del cambio = primer inicio de la app (no hay datos previos que migrar). Para volver a los ejemplos: borrar las claves `paginaViva.*` (ver README). Rollback: revertir el commit.
