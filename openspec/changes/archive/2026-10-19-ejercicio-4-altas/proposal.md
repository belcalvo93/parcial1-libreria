# Proposal — ejercicio-4-altas

## Why

El Ejercicio 4 (15 pts) pide altas de libros y clientes con validaciones desde los formularios ya maquetados (`form-libro`, `form-cliente`). Hoy los selects están hardcodeados en el HTML y `tabla-libros` / `tabla-clientes` son estáticas: sin esta capa, lo que el usuario carga no entra a `DATOS` ni se refleja en el catálogo ni en las opciones de venta del Ejercicio 5.

## What Changes

- Se crea `js/altas.js` con la lógica de altas: poblar selects desde `DATOS`, validar, `preventDefault` en ambos submits, agregar a `DATOS` solo si todo pasa, y actualizar listados y opciones sin recargar.
- Selects generados desde `DATOS`: `libro-editorial`, `libro-genero` (más opción `sin-clasificar` → “Sin clasificar” = ausencia de género) y `libro-autores` (múltiple). Al dar de alta un libro o cliente se agregan sus opciones a `venta-libro-1` y `venta-cliente` (las usa el Ej. 5 de Bruno; solo se agrega `<option>`, no se toca su lógica).
- `tabla-libros` y `tabla-clientes` pasan a render dinámico desde `DATOS` (igual técnica que el catálogo del Ej. 3: `createElement` + `textContent`).
- Errores cerca del formulario con los `<p>` existentes (`mensaje-error-libro`, `mensaje-ok-libro`, `mensaje-error-cliente`, `mensaje-ok-cliente`), solo con `hidden` (nunca `style.display`). El `<p>` de error muestra el detalle específico vía `textContent`; al ocultarse restaura su texto original.
- Se enlaza `<script src="js/altas.js"></script>` al final de `index.html`, después de los dos scripts del Ej. 3. Nada más del HTML se toca. No se toca `css/`.

## Capabilities

### New Capabilities

- `alta-libros`: formulario `form-libro` con validaciones (título/ISBN no vacíos; ISBN único sin espacios ni guiones; precio numérico > 0; editorial y autores existentes con ≥1 autor; género existente o sin asignar) y render de `tabla-libros` + opciones relacionadas.
- `alta-clientes`: formulario `form-cliente` con validaciones (nombre/email no vacíos; email con formato válido; email único con trim y case-insensitive) y render de `tabla-clientes` + opción en `venta-cliente`.

## Impact

- Archivos nuevos: `js/altas.js`.
- Archivo modificado: `index.html` (solo +1 línea `<script>` antes de `</body>`). No se toca `css/styles.css`.
- Reutiliza `DATOS` (`js/datos.js`) y `renderCatalogo` (`js/catalogo.js`, expuesto en `window`); los ids y clases del HTML no cambian.
- Fuera de alcance: editar/eliminar registros (no se exige), ventas e historial (Ejercicio 5), localStorage (Ejercicio 6).
