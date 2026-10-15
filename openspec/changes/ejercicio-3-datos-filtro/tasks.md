# Tasks — ejercicio-3-datos-filtro

> Alcance estricto: solo JS para datos y filtro. No tocar `index.html` salvo agregar los `<script>` al final. No tocar `css/styles.css`. Prohibido `innerHTML`: solo `textContent` / `createElement`.

## 1. Datos

- [ ] 1.1 Crear `js/datos.js` con el objeto `DATOS` (editoriales, generos, autores, libros, clientes) con los valores congelados del design archivado del Ej. 1 (§5)
- [ ] 1.2 Verificar que `DATOS.libros` tiene 6 libros con `id`, `titulo`, `isbn`, `precio`, `editorialId`, `generoId`, `autorIds[]`; uno con `generoId: null` y uno (“Manual para perder trenes”) con 2 `autorIds`

## 2. Render dinámico

- [ ] 2.1 Crear `js/catalogo.js` con `renderCatalogo(filtro = "")` que vacía `#tabla-catalogo tbody` y crea `tr`/`td` con `createElement` + `textContent` (mismo orden de columnas que el HTML, precio con 2 decimales)
- [ ] 2.2 Verificar que al cargar la página (`DOMContentLoaded` → `renderCatalogo()`) se renderizan 6 filas
- [ ] 2.3 Verificar que el libro sin género muestra "Sin clasificar" y el de 2 autores muestra “Lucía Ferreyra, Tomás Iriarte”
- [ ] 2.4 Verificar con `grep -r innerHTML js/` que no hay ningún uso de `innerHTML`

## 3. Filtro en tiempo real

- [ ] 3.1 Conectar `#catalogo-busqueda` con evento `input` y `#catalogo-buscar` con `click` para llamar a `renderCatalogo(valor)`
- [ ] 3.2 Verificar que el filtro es case-insensitive y parcial (“MAPA” y “mapa” encuentran “El mapa de las horas lentas”)
- [ ] 3.3 Verificar `trim`: “  mapa  ” se comporta igual que “mapa”
- [ ] 3.4 Verificar que si no hay resultados se muestra `#mensaje-catalogo-sin-resultados` y se oculta la tabla
- [ ] 3.5 Verificar que si el filtro está vacío (o solo espacios) se muestran todos los libros y se ocultan los mensajes

## 4. Mensajes de estado

- [ ] 4.1 Controlar `#mensaje-catalogo-vacia` (si no hay libros)
- [ ] 4.2 Controlar `#mensaje-catalogo-sin-resultados` (si el filtro no coincide)
- [ ] 4.3 Verificar que los mensajes tienen `hidden` cuando no corresponden

## 5. Enlace de scripts

- [ ] 5.1 Agregar `<script src="js/datos.js">` y `<script src="js/catalogo.js">` antes de `</body>` en `index.html`
- [ ] 5.2 Verificar que no se modificó nada más del HTML

## 6. Verificación integral

- [ ] 6.1 Repasar los criterios de aceptación del Ejercicio 3 del PDF
- [ ] 6.2 Verificar en navegador que el filtro funciona en tiempo real
- [ ] 6.3 Validar con `openspec validate --changes "ejercicio-3-datos-filtro"`
