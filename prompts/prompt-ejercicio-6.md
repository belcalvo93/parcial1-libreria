# PROMPT SDD — Ejercicio 6: Persistencia, pruebas, README y .zip (TPI Librería)

Responsable: Joaquín. Change: `ejercicio-6-persistencia`.

## ROL
Actuá como **orquestador SDD (OpenSpec)**, no como ejecutor.
El CLI `openspec` es la única fuente de verdad: verificá con `openspec list --json` y `openspec status --change "ejercicio-6-persistencia" --json`.
No encadenes fases sin mi confirmación. Hablame en rioplatense (voseo).

## CONTEXTO DEL PROYECTO
- Fuente: `docs/Repaso Primer Parcial.pdf`, Ejercicio 6 (pág. 6) + Formato de entrega (pág. 7). Vale 10 puntos: persistencia 4, pruebas manuales registradas 3, documentación y entrega 3.
- Estado: Ej. 1 a 4 archivados. `DATOS` global en `js/datos.js`; `window.renderCatalogo` en `js/catalogo.js`; altas en `js/altas.js`. El Ej. 5 (ventas e historial) es de Bruno y puede llegar en paralelo: no le pises el trabajo.

Alcance técnico estricto:
- Solo HTML, CSS y JS nativos. Sin librerías, frameworks, CDNs, fetch ni servidor. `index.html` con doble clic.
- Persistir solo libros, clientes y ventas. Editoriales, géneros y autores siguen fijos en `js/datos.js`.

## REGLAS DURAS
- ✅ La persistencia va en `js/persistencia.js`, cargado **entre** `datos.js` y `catalogo.js` (única línea nueva en `index.html`).
- ✅ Claves `paginaViva.libros`, `paginaViva.clientes`, `paginaViva.ventas` (JSON). Primer inicio guarda ejemplos; después se **reemplazan** los arrays, nunca se concatenan.
- ✅ `window.guardarDatos()` se llama tras cada alta válida (`js/altas.js`) y al confirmar una venta (Ej. 5).
- ✅ `try/catch` si `localStorage` no está disponible o el JSON está roto.
- ❌ No tocar `css/` ni los changes archivados. ❌ Nada de `innerHTML`.
- ❌ No marcar un caso manual como aprobado sin ejecutarlo: si falla o falta, se registra el comportamiento real.

## PLAN
1. **Propose:** proposal, specs `persistencia` y `documentacion`, design (claves, reemplazo, contrato con el Ej. 5) y tasks. **Pará acá.**
2. **Apply:** `js/persistencia.js`, `<script>` en `index.html`, `persistir()` en `js/altas.js`, `README.md`.
3. **Verificación:** correr los 8 casos de la consigna en el navegador y anotar el resultado observado en el README.
4. **Archive:** cuando el Ej. 5 esté integrado y los 8 casos registrados. Después: `.zip` de entrega.

## CRITERIOS DE ACEPTACIÓN
- [ ] Primer inicio: `localStorage` queda con 6 libros, 3 clientes y las ventas iniciales.
- [ ] Recargar varias veces no duplica ejemplos, altas ni ventas.
- [ ] Un alta válida queda guardada al instante; una rechazada no guarda nada.
- [ ] Cambiar solo el precio en `paginaViva.libros` cambia el catálogo pero no los importes del historial.
- [ ] JSON roto en una clave: la página carga sin errores y esa colección vuelve a los ejemplos.
- [ ] README con todo lo de la pág. 7 y los 8 casos con resultado observado.
- [ ] `git diff --stat`: `js/persistencia.js`, `js/altas.js`, `index.html` (+1 línea), `README.md`, `openspec/`, `prompts/`. `css/` intacto.
