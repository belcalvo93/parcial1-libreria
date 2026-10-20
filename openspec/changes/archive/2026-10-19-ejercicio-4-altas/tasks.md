# Tasks — ejercicio-4-altas

> Alcance estricto: solo JS para altas. `index.html` solo +1 línea `<script>`. No tocar `css/`. Prohibido `innerHTML` y `style.display`: solo `textContent`/`createElement` y `hidden`.

## 1. Poblado inicial desde DATOS

- [x] 1.1 Al `DOMContentLoaded`, poblar `libro-editorial`, `libro-genero` (+ `sin-clasificar`) y `libro-autores` desde `DATOS` con `createElement` + `textContent`
- [x] 1.2 Renderizar `tabla-libros` y `tabla-clientes` desde `DATOS` (6 filas y 3 filas, mismo orden de columnas que el HTML)
- [x] 1.3 Verificar que `grep -r innerHTML js/` y `grep -r "style.display" js/` no devuelven nada

## 2. Alta de libros

- [x] 2.1 Conectar `submit` de `form-libro` con `preventDefault` como primera línea
- [x] 2.2 Validar título/ISBN no vacíos, ISBN único normalizado, precio numérico > 0, editorial/autores existentes con ≥1 autor, género existente o `sin-clasificar`
- [x] 2.3 Rechazo: mostrar `mensaje-error-libro` con el detalle y ocultar `mensaje-ok-libro`, sin agregar nada
- [x] 2.4 Alta válida: push a `DATOS.libros` (`sin-clasificar` → `generoId: null`, `id` slug único), re-render de `tabla-libros` + `renderCatalogo` con el filtro vigente, `<option>` en `venta-libro-1`, `form.reset()`, mostrar `mensaje-ok-libro`
- [x] 2.5 Verificar casos: ISBN existente con guiones se rechaza; ISBN nuevo aparece en catálogo y en `venta-libro-1`

## 3. Alta de clientes

- [x] 3.1 Conectar `submit` de `form-cliente` con `preventDefault` como primera línea
- [x] 3.2 Validar nombre/email no vacíos, email con formato válido, email único normalizado (trim + minúsculas)
- [x] 3.3 Rechazo: mostrar `mensaje-error-cliente` con el detalle y ocultar `mensaje-ok-cliente`, sin agregar nada
- [x] 3.4 Alta válida: push a `DATOS.clientes` (`id` slug único), re-render de `tabla-clientes`, `<option>` en `venta-cliente`, `form.reset()`, mostrar `mensaje-ok-cliente`
- [x] 3.5 Verificar casos: email inválido y email existente en mayúsculas se rechazan; email nuevo aparece en listado y en `venta-cliente`

## 4. Enlace y verificación integral

- [x] 4.1 Agregar `<script src="js/altas.js"></script>` después de los scripts del Ej. 3 y verificar que nada más del HTML cambió
- [x] 4.2 Probar en navegador: altas válidas e inválidas de ambos forms, mensajes con `hidden`, sin recarga, responsive 320/1280 intacto
- [x] 4.3 Validar con `openspec validate --changes "ejercicio-4-altas"`
