# Design — ejercicio-5-ventas

## Context

Ver `proposal.md`. Estado: Ej. 1 a 4 archivados. `DATOS` global en `js/datos.js`; `js/catalogo.js` expone `renderCatalogo`; `js/altas.js` hace las altas. En el HTML ya están `form-venta` (`venta-cliente`, renglón 1 con `venta-libro-1`, `venta-cantidad-1`, `venta-precio-1`, `venta-subtotal-1`, `venta-quitar-1`), `venta-agregar`, `venta-total`, `venta-confirmar`, `mensaje-error-venta`, `mensaje-ok-venta`, `tabla-historial` y `mensaje-historial-vacia`. El Ej. 6 (rama `ejercicio-6-persistencia`) carga `js/persistencia.js` antes del resto y expone `window.guardarDatos()`.

## Goals / Non-Goals

- Goals: venta con cliente y renglones válidos, sin libros repetidos; cálculos con dos decimales; confirmación que registra id, fecha, cliente, renglones y total con precio histórico; historial con detalle; formulario limpio y mensaje tras confirmar.
- Non-Goals: stock, cobro, editar o eliminar ventas, persistencia (es del Ej. 6).

## Decisions

### 1. Estado en un array `renglones` y DOM generado desde ahí

`renglones = [{ libroId, cantidad }]`, con `cantidad` como texto tal cual lo escribió la persona. Agregar, quitar o refrescar opciones vuelve a generar todos los renglones (ids `venta-libro-N`, `venta-cantidad-N`, etc., numerados por posición). Escribir una cantidad solo actualiza importes, para no perder el foco del campo. Cada `<output>` lleva `for` con los campos de los que se calcula, y el `for` de `venta-total` se actualiza con los renglones vigentes.

Los renglones se insertan como hijos directos de `form-venta`, antes de `venta-agregar`, con la misma estructura del Ej. 1 (`.form__grupo` con `<output>`), así el CSS del Ej. 2 los toma sin cambios. Al cargar se quita el renglón estático.

### 2. Modelo de venta (lo que se guarda)

```js
{
  id: "V-002",
  fecha: "2026-10-19",
  clienteId: "valentina-rios",
  clienteNombre: "Valentina Ríos",
  renglones: [
    { libroId: "el-mapa-de-las-horas-lentas", titulo: "El mapa de las horas lentas", cantidad: 2, precioUnitario: 1500, subtotal: 3000 }
  ],
  total: 3000
}
```

`titulo`, `clienteNombre` y `precioUnitario` son copias: el historial se lee solo con esto, aunque después cambie el catálogo. El `id` se calcula como `V-` + `DATOS.ventas.length + 1` con tres dígitos; como no se borran ventas, no se repite tras recargar. La fecha es la local en `AAAA-MM-DD`.

### 3. Validaciones

- Cantidad válida: solo dígitos y mayor que cero (`/^\d+$/`). Así `0`, `-1`, `1.5` y vacío se rechazan.
- Libro repetido: al cambiar el selector de un renglón, si ese libro ya está en otro se muestra el error y se vuelve al libro anterior. «Agregar renglón» elige el primer libro libre; si no queda ninguno, se informa.
- Al confirmar, en orden: cliente existente, al menos un renglón, y por renglón libro existente, cantidad válida y sin repetidos. Se muestra el primer error en `mensaje-error-venta`.

### 4. Historial

`renderHistorial()` vacía `#tabla-historial tbody`, saca los `<details>` que son hijos directos de `#historial` y los vuelve a generar desde `DATOS.ventas`: una fila por venta y un `<details>` (insertado antes de `mensaje-historial-vacia`, así toma el espaciado de `.card`) con `<summary>` «Ver detalle de V-00N» y su tabla de renglones. Sin ventas se oculta la tabla y se muestra `mensaje-historial-vacia`. Todo con `createElement` + `textContent`.

### 5. Convivencia con Ej. 4 y Ej. 6

- `window.actualizarSelectsVenta()` re-puebla `venta-cliente` (manteniendo la elección) y los renglones. La llama `altas.js` después de cada alta.
- Después del `push` se llama `window.guardarDatos()` solo si existe, así la rama funciona con o sin el Ej. 6 mergeado.

## Risks / Trade-offs

- [Re-generar renglones] → Se pierde el foco al agregar o quitar; se lleva el foco al selector nuevo o al botón «Agregar renglón».
- [Id por cantidad de ventas] → Si alguien borra ventas a mano en `localStorage` podría repetirse; la interfaz no permite borrar.

## Migration Plan

Sin migración. Rollback: revertir los commits del change.
