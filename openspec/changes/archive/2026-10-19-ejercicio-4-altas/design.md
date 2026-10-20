# Design — ejercicio-4-altas

## Context

Ver `proposal.md` (Why). Estado actual: Ej. 3 archivado (`DATOS` en `js/datos.js`, `renderCatalogo` en `js/catalogo.js` expuesto en `window`, catálogo dinámico con filtro). `form-libro` (`libro-titulo`, `libro-isbn`, `libro-precio`, `libro-editorial`, `libro-genero` con opción `sin-clasificar`, `libro-autores` múltiple), `form-cliente` (`cliente-nombre`, `cliente-email`), tablas `tabla-libros` / `tabla-clientes`, y mensajes `mensaje-error-libro`, `mensaje-ok-libro`, `mensaje-error-cliente`, `mensaje-ok-cliente`, `mensaje-libros-vacia`, `mensaje-clientes-vacia` — todos con `hidden`. Restricción dura: `hidden` solo se toca por atributo (`el.hidden = true/false`), nunca `style.display`; ids y clases intactos; `textContent`, nunca como HTML.

## Goals / Non-Goals

- Goals: selects poblados desde `DATOS`; validaciones del PDF pág. 4; alta solo si todo pasa; listados y opciones actualizados sin recargar; errores visibles cerca del formulario.
- Non-Goals: editar/eliminar, ventas/historial dinámicos (Ej. 5), localStorage (Ej. 6), ABM de editoriales/géneros/autores.

## Decisions

### 1. Un solo archivo `js/altas.js`

Toda la lógica de altas (libros + clientes + renders de sus tablas) va en un archivo, cargado después de `datos.js` y `catalogo.js`. Funciones chicas con una responsabilidad: `normalizarISBN`, `normalizarEmail`, `emailValido`, `validarLibro`, `validarCliente`, `renderTablaLibros`, `renderTablaClientes`, `poblarSelects`, `agregarOpcionVenta`, `mostrarMensaje`.

### 2. Normalizaciones (comparaciones justas)

- `normalizarISBN(valor)`: quita espacios y guiones (`valor.replace(/[\s-]/g, "")`). El ISBN se compara normalizado (unicidad) pero se guarda tal como lo escribió el usuario (recortado con `trim`).
- `normalizarEmail(valor)`: `valor.trim().toLowerCase()`. Unicidad de email con esta forma.
- `emailValido(valor)`: `/^[^\s@]+@[^\s@]+\.[^\s@]+$/` (formato válido sin librerías; el `type="email"` del HTML no valida porque el form lleva `novalidate`).
- Precio: `Number(valor)` finito y `> 0`. ISBN/título/nombre con `trim` no vacíos.
- `id` del nuevo registro: slug kebab-case del título/nombre; si ya existe, sufijo `-2`, `-3`, etc. (estable y único dentro de `DATOS`).

### 3. Validaciones de libro (todas deben pasar)

1. Título no vacío (con `trim`). 2. ISBN no vacío (con `trim`). 3. ISBN normalizado no existe en `DATOS.libros`. 4. Precio numérico y > 0. 5. `editorialId` existe en `DATOS.editoriales`. 6. ≥1 autor y todos existen en `DATOS.autores`. 7. Género: `sin-clasificar` → `generoId: null`; otro valor debe existir en `DATOS.generos`.

### 4. Validaciones de cliente (todas deben pasar)

1. Nombre no vacío (con `trim`). 2. Email no vacío (con `trim`). 3. Formato válido. 4. Email normalizado no existe en `DATOS.clientes`.

### 5. Envío con `preventDefault` y mensajes

Ambos forms escuchan `submit` y llaman `event.preventDefault()` primero. Si hay errores: se muestra el `<p>` de error con el detalle (primera falla o todas unidas) vía `textContent`, se oculta el de éxito, y no se agrega nada. Si todo pasa: push a `DATOS`, re-render de la tabla + `renderCatalogo` con el filtro vigente + nueva `<option>` en el select de venta correspondiente, `form.reset()`, se muestra el `<p>` de éxito y se oculta el de error. El texto original del `<p>` de error se guarda al inicio y se restaura al ocultarlo.

### 6. Poblado inicial y convivencias

Al `DOMContentLoaded`: poblar `libro-editorial`, `libro-genero` (+ `sin-clasificar`), `libro-autores` desde `DATOS` (reconstruye `<option>` con `createElement` + `textContent`, preservando el orden del HTML), renderizar `tabla-libros` y `tabla-clientes`, y conectar submits. Solo se agregan `<option>` a `venta-libro-1` / `venta-cliente` ante altas nuevas: la lógica de esos controles sigue siendo del Ej. 5. Sin clases ni estilos inline nuevos.

## Risks / Trade-offs

- [HTML estático como fallback] → Igual que el Ej. 3: el JS reemplaza al renderizar; sin JS se ve el ejemplo.
- [Un solo `<p>` de error por form] → El detalle va como texto del `<p>` existente (JS, no cambia el HTML); no se agregan contenedores.
- [Precio como número] → Se guarda `Number` y se muestra con 2 decimales, coherente con el catálogo.

## Migration Plan

No hay migración. El cambio agrega `js/altas.js` y una línea `<script>` en `index.html`. Rollback: revertir el commit.
