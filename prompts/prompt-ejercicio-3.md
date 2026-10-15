# PROMPT SDD — Ejercicio 3: Datos locales, catálogo y filtro (TPI Librería)

Responsable: Hernán. Reutiliza el change existente `ejercicio-3-datos-filtro` (no crear uno nuevo).

## ROL
Actuá como **orquestador SDD (OpenSpec)**, no como ejecutor.
Tu trabajo es coordinar, delegar a sub-agentes y sintetizar resultados.
El CLI `openspec` es la única fuente de verdad del estado: verificá con `openspec list --json` y `openspec status --change "ejercicio-3-datos-filtro" --json`. No adivines artefactos.
No encadenes fases sin mi confirmación.
Hablame en rioplatense (voseo).

## CONTEXTO DEL PROYECTO
- Materia: Programación III — Repaso Primer Parcial. TPI: frontend de una librería (“Librería Página Viva”).
- Fuente: `docs/Repaso Primer Parcial.pdf`, Ejercicio 3 (pág. 3). Vale 15 puntos: modelo y ejemplos 5, renderizado 5, filtro y estados 5.
- Estado: Ejercicio 1 archivado en `openspec/changes/archive/2026-10-14-ejercicio-1-html/` (`index.html` con 5 secciones, 6 libros, 2 editoriales, 2 géneros, 3 autores, 3 clientes, venta V-001). Ejercicio 2 archivado (`css/styles.css` completo). Specs vigentes en `openspec/specs/`.
- Datos congelados (única fuente): `openspec/changes/archive/2026-10-14-ejercicio-1-html/design.md` §5 (6 libros con título | ISBN | precio | editorial | género | autores; 2 editoriales; 2 géneros + “Sin clasificar”; 3 autores; 3 clientes). Usá esos mismos, no inventes otros.
- Inventario de `id` congelado (el JS los usa tal cual, no los renombres): `tabla-catalogo`, `catalogo-busqueda`, `catalogo-buscar`, `mensaje-catalogo-vacia`, `mensaje-catalogo-sin-resultados` (lista completa en el design del Ej. 1, §2).
- Trabajo en cadena: después viene el Ejercicio 4 (altas, otro change `ejercicio-4-altas`) y el Ejercicio 5 de Bruno (ventas). No le pises el trabajo a nadie.

Alcance técnico estricto:
- Solo HTML, CSS y JS nativos. Sin librerías, frameworks ni CDNs.
- Sin fetch, sin API, sin base de datos, sin servidor. Entrada: `index.html` con doble clic, sin npm.
- No se administran altas, bajas ni modificaciones de editoriales, géneros o autores en este ejercicio.

## REGLAS DURAS (no negociables)
- ✅ Todo tu código va en `js/`. Estructura acordada: `js/datos.js` (capa de datos `DATOS`) + `js/catalogo.js` (render + filtro). Si necesitás un tercer archivo, justificàlo en `design.md` antes.
- ✅ En `index.html` solo podés agregar al final, antes de `</body>`, las líneas `<script src="js/datos.js"></script>` y `<script src="js/catalogo.js"></script>`. Nada más sin que yo te lo pida.
- ❌ No tocar `css/` bajo ningún concepto.
- ❌ No tocar los changes archivados del Ej. 1 y 2.
- ✅ Los datos que escribe el usuario se insertan como texto (`textContent`, nunca `innerHTML`).
- ✅ El filtro por título: insensible a mayúsculas, coincidencia parcial, recorta espacios de los extremos (`trim`). Vacía → todo. Sin coincidencias → mensaje.
- ✅ No romper el diseño adaptable: el JS no agrega `<style>`, ni `style=""`, ni clases nuevas que alteren 320/1280 px. Las tablas ya tienen scroll propio (`.tabla`); el render debe generar `tr`/`td` planos con el mismo orden de columnas del HTML.

## PLAN
Después de cada fase, mostrame el resultado y preguntame: Continuar / Ajustar / Parar.

1. **Verificación:** `openspec --version`, rama `ejercicio-3-datos-filtro`, `index.html` con estilos y “El mapa de las horas lentas” (si ves “Cien años de soledad”, frenar y avisar). `git status` limpio salvo el change.
2. **Explore:** leé el Ejercicio 3 del PDF y el §5 del design archivado; devolveme mapa datos → `id` del HTML → mensajes.
3. **Propose:** corregí el change `ejercicio-3-datos-filtro` (proposal, specs `datos` y `catalogo`, `design` con modelo `autorIds: []`, `generoId: null` → “Sin clasificar”, estrategia `textContent` + `trim`, y `tasks`). **Pará acá: lo reviso antes del apply.**
4. **Apply:** solo cuando yo confirme. Implementá siguiendo `tasks.md`: `js/datos.js`, `js/catalogo.js` (`renderCatalogo(filtro)`), wiring `input` + click en `catalogo-buscar`, toggle `hidden` en mensajes, `DOMContentLoaded` → render completo.
5. **Verificación:** lista qué revisar a mano en el navegador para cada criterio de aceptación.
6. **Archive:** solo cuando yo confirme que todo funciona. Sincronizá specs y archivá el change. Después: `git add .`, `git commit -m "Ej3: datos locales, catálogo y filtro"`, `git pull`, `git push`.

## CRITERIOS DE ACEPTACIÓN
- [ ] Los 5 arrays (libros, editoriales, géneros, autores, clientes) existen en `js/datos.js` con `id` estables y los mismos valores del design archivado (6 libros, 2 editoriales, 2 géneros, 3 autores, 3 clientes; un libro con `generoId: null`; “Manual para perder trenes” con 2 autores).
- [ ] El catálogo (`tabla-catalogo`) se genera 100% con JS a partir del array al cargar (6 filas, mismo orden de columnas que el HTML).
- [ ] El libro sin género muestra “Sin clasificar”.
- [ ] El filtro ignora mayúsculas (“MAPA” encuentra “El mapa de las horas lentas”).
- [ ] El filtro admite coincidencia parcial (“mapa”, “patio”, “luz”).
- [ ] El filtro recorta espacios (“  mapa  ” funciona).
- [ ] Búsqueda vacía → muestra todo el catálogo y oculta mensajes.
- [ ] Sin coincidencias → muestra `mensaje-catalogo-sin-resultados` y oculta la tabla.
- [ ] Sin libros en el array → muestra `mensaje-catalogo-vacia`.
- [ ] Toda inserción en el DOM usa `textContent`. No hay `innerHTML` en `js/`.
- [ ] Escribís en `input` y filtra en tiempo real; el botón `catalogo-buscar` también filtra (por teclado con Enter/Tab funciona).
- [ ] A 320 px y 1280 px no hay scroll horizontal de página; las tablas conservan su scroll propio.
- [ ] `git diff --stat` del apply solo muestra `js/` + `openspec/` + las 2 líneas `<script>` en `index.html`. `css/` intacto.
