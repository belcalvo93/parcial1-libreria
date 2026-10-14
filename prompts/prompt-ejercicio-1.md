# PROMPT SDD — Ejercicio 1: Estructura y pantallas HTML (TPI Librería)

## ROL
Actuá como **orquestador SDD (OpenSpec)**, no como ejecutor.
Tu trabajo es coordinar, delegar a sub-agentes y sintetizar resultados.
El CLI `openspec` es la única fuente de verdad del estado: verificá con `openspec list --json` y `openspec status --change "ejercicio-1-html" --json`. No adivines artefactos.
No encadenes fases sin mi confirmación.
Hablame en rioplatense (voseo).

## CONTEXTO DEL PROYECTO
- Materia: Programación III — Repaso Primer Parcial. TPI: frontend de una librería.
- Fuente: `docs/Repaso Primer Parcial.pdf`, Ejercicio 1 (pág. 2). Vale 20 puntos: 10 vistas y navegación + 10 semántica, formularios y estados.
- Proyecto: `C:\Users\beluu\Desktop\Parcial1`. Ya existen `docs/` (con el PDF), `css/` y `js/` vacías. Es un repo de Git.
- Trabajo en grupo y en cadena: después de este ejercicio, otro integrante hace el Ejercicio 2 (CSS) y otro el Ejercicio 3 (JS con datos y filtro).

Alcance técnico estricto:
- Solo HTML, CSS y JS nativos. Sin librerías, frameworks ni CDNs.
- Sin API, base de datos, servidor ni login real.
- Recursos locales. Entrada: `index.html`, que se abre con doble clic, sin npm.

Requerimiento funcional — 5 vistas con navegación clara (varias páginas o secciones de una misma página):
1. **Catálogo:** título de la librería, buscador por título y listado de libros. Cada libro muestra título, ISBN, editorial, género, autores y precio. Si no tiene género: «Sin clasificar».
2. **Libros:** formulario de alta (título, ISBN, precio, editorial, género con opción «Sin clasificar», autores con selección múltiple) + listado de libros registrados.
3. **Clientes:** formulario de alta (nombre completo y email) + listado de clientes registrados.
4. **Ventas:** selección de cliente, composición de renglones (libro, cantidad, precio unitario, subtotal) y resumen con total.
5. **Historial:** listado de ventas (identificador, fecha, cliente, total) y acceso a sus detalles.

Requerimientos no funcionales HTML:
- Etiquetas semánticas: `header`, `nav`, `main`, `section`, `footer`.
- Cada campo con `label` asociado (`for` / `id`). Botones con texto que describe la acción.
- Tablas con `thead` y `th`.
- Mensajes previstos en el HTML para: listas vacías, búsqueda sin resultados, operación correcta y operación rechazada. Dejarlos con el atributo `hidden` y un `id` claro, para que el JS del Ejercicio 3 los muestre u oculte.
- Datos estáticos de ejemplo coherentes con el Ejercicio 3: 6 libros, 2 editoriales, 2 géneros, 3 autores, 3 clientes (al menos un libro sin género). Al menos una venta de ejemplo en el historial.

Límites de este ejercicio (NO hacer):
- **No dar estilos.** El CSS es el Ejercicio 2. Solo enlazar en todas las páginas un archivo `css/styles.css` vacío.
- **No escribir JavaScript.** La carpeta `js/` queda para el Ejercicio 3.
- Usar **nombres de clase consistentes y reutilizables** para elementos equivalentes (por ejemplo `card`, `form`, `btn`, `tabla`, `mensaje`, `mensaje--error`, `mensaje--ok`), así el Ejercicio 2 los estiliza sin tocar el HTML.
- No meter `openspec/` en el `.gitignore`: la carpeta tiene que quedar en el repositorio.

## PLAN
Después de cada fase, mostrame el resultado y preguntame: Continuar / Ajustar / Parar.

1. **Inicialización:** verificá `openspec --version`. Si no existe `openspec/`, corré `openspec init`. Si ya existe, preguntame si reusar.
2. **Explore:** leé el Ejercicio 1 del PDF y devolveme un mapa de vistas vs. campos vs. mensajes de estado.
3. **Propose:** creá el change `ejercicio-1-html` con `proposal.md`, specs, `design.md` (multipágina o secciones, convención de nombres de archivos y clases, dónde va cada mensaje) y `tasks.md`. **Pará acá: lo tengo que revisar con el profe antes del apply.**
4. **Apply:** solo cuando yo confirme. Implementá siguiendo `tasks.md`.
5. **Verificación:** listame qué revisar a mano en el navegador para comprobar cada criterio de aceptación.
6. **Archive:** solo cuando yo confirme que todo funciona. Sincronizá specs y archivá el change.

## CRITERIOS DE ACEPTACIÓN
- [ ] `index.html` es la entrada y abre con doble clic, sin npm ni servidor.
- [ ] Existen las 5 vistas y se puede ir de cualquiera a cualquiera desde el `nav`.
- [ ] La navegación funciona solo con teclado (Tab y Enter).
- [ ] Hay `header`, `nav`, `main` y `footer` en todas las vistas.
- [ ] Cada input, select y textarea tiene un `label` con `for` que coincide con su `id`.
- [ ] Todos los botones dicen qué hacen (nada de "Enviar" o "OK" a secas).
- [ ] Todas las tablas tienen `thead` con `th`.
- [ ] El catálogo muestra título, ISBN, editorial, género, autores y precio; un libro aparece como «Sin clasificar».
- [ ] Existen los mensajes de lista vacía, sin resultados, éxito y error, con `hidden` e `id`.
- [ ] Los datos de ejemplo cumplen: 6 libros, 2 editoriales, 2 géneros, 3 autores, 3 clientes, al menos 1 venta.
- [ ] No hay estilos propios ni JavaScript; `css/styles.css` está enlazado y vacío.
- [ ] No se usa ninguna librería, framework ni CDN.
- [ ] La carpeta `openspec/` queda versionada en Git.

Empezá por el paso 1 y mostrame el estado con `openspec status`.
