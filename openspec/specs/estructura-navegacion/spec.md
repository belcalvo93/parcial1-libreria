# estructura-navegacion Specification

## Purpose
Define la cáscara global de una sola página del TPI: encabezado, navegación por anclas, contenido principal, pie y convenciones de clases, mensajes y recursos que usan los Ejercicios 2 a 6.

## Requirements

### Requirement: Página única con cinco secciones navegables

El sistema SHALL presentar un único `index.html` que se abre con doble clic (sin npm ni servidor) y contiene un `header` con el nombre de la librería, un `nav` con enlaces de ancla a las cinco secciones, un `main` con cinco `<section id>` (catálogo, libros, clientes, ventas, historial) y un `footer`.

#### Scenario: Navegar entre vistas con anclas

- **WHEN** la persona usuaria activa el enlace «Ventas» del `nav`
- **THEN** el navegador desplaza la vista a la sección de ventas sin recargar la página

#### Scenario: Navegación completa solo con teclado

- **WHEN** la persona usuaria recorre la página con Tab y activa enlaces con Enter
- **THEN** puede llegar desde cualquier vista a cualquier otra vista usando solo el teclado

#### Scenario: Apertura sin servidor

- **WHEN** se abre `index.html` con doble clic desde el sistema de archivos
- **THEN** las cinco vistas y la navegación funcionan sin npm, sin servidor y sin conexión

### Requirement: Etiquetas semánticas y controles accesibles

El sistema SHALL usar etiquetas semánticas (`header`, `nav`, `main`, `section`, `footer`), asociar cada campo con un `label` cuyo `for` coincide con el `id` del campo, usar botones cuyo texto describe la acción (nunca «Enviar» u «OK» a secas) y usar tablas con `thead` y `th` en todas las vistas.

#### Scenario: Campo con etiqueta asociada

- **WHEN** se inspecciona cualquier `input`, `select` o `textarea` de la página
- **THEN** existe un `label` con atributo `for` igual al `id` del campo

#### Scenario: Botones descriptivos

- **WHEN** se listan todos los `<button>` de la página
- **THEN** cada uno tiene un texto que describe su acción (por ejemplo «Agregar renglón», «Confirmar venta»)

### Requirement: Mensajes previstos con hidden e id

El sistema SHALL incluir en el HTML todos los mensajes de estado que el JS mostrará u ocultará (listas vacías, búsqueda sin resultados, éxito y error por formulario), cada uno con el atributo `hidden` y un `id` claro, ubicado junto al formulario o listado que le corresponde.

#### Scenario: Mensaje oculto por defecto

- **WHEN** se carga la página sin ejecutar JavaScript
- **THEN** ningún mensaje de estado es visible porque todos llevan `hidden`, y cada uno tiene un `id` único y descriptivo

### Requirement: Hoja de estilos enlazada y cero dependencias

El sistema SHALL enlazar `css/styles.css` desde el HTML como la única hoja de estilos externa, con toda la presentación contenida en ese archivo, sin estilos en línea ni bloques `<style>` en el documento, sin código JavaScript y sin librerías, frameworks ni CDNs.

#### Scenario: Enlace a la hoja de estilos externa

- **WHEN** se inspecciona el `head` del documento y el archivo `css/styles.css`
- **THEN** existe el `<link>` a `css/styles.css`, la presentación vive en ese archivo y el HTML no contiene reglas propias (`<style>` ni atributos `style`)

#### Scenario: Sin dependencias externas

- **WHEN** se revisan las etiquetas `<link>`, `<script>` e `<img>` del proyecto
- **THEN** ninguna referencia apunta a una URL externa, CDN, librería o framework

### Requirement: Pie de página con identificación de la librería

El sistema SHALL mostrar en el `footer` de `index.html` el texto «© 2026 Librería Página Viva» como único contenido.

#### Scenario: Texto del pie de página

- **WHEN** se inspecciona el `footer` de `index.html`
- **THEN** su contenido es «© 2026 Librería Página Viva» y ya no menciona «TPI Programación III»
