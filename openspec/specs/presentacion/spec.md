# presentacion Specification

## Purpose
Define la capa de presentación de la página única de la librería: identidad visual propia (color, tipografía, espaciado), componentes reutilizados por clase (tarjetas, formularios, botones, mensajes, renglón de venta), organización con Flexbox o Grid, adaptación a 320 px y 1280 px sin desplazamiento horizontal de la página y accesibilidad básica de la interfaz (foco visible, mensajes con texto, alternativas de imagen).

## Requirements

### Requirement: Identidad visual en hoja de estilos externa

El sistema SHALL definir toda la presentación en `css/styles.css` (archivo externo ya enlazado), con una identidad visual coherente expresada como variables CSS en `:root` (paleta de colores con valores hex, tipografía, escala de espaciado, radios y sombras), usando únicamente fuentes del sistema y sin librerías, frameworks, CDNs ni recursos externos.

#### Scenario: Variables de identidad en `:root`

- **WHEN** se inspecciona `css/styles.css`
- **THEN** el selector `:root` declara variables CSS para colores, tipografía y espaciado, y todas las reglas posteriores las reutilizan en lugar de repetir valores sueltos

#### Scenario: Cero dependencias externas de estilo

- **WHEN** se revisan las reglas de `css/styles.css` y las etiquetas del documento
- **THEN** no hay `@import`, `url()` hacia recursos remotos, `@font-face` propias ni fuentes distintas de las del sistema, y no existen estilos en línea ni bloques `<style>` en `index.html`

### Requirement: Clases reutilizadas para elementos equivalentes

El sistema SHALL aplicar las mismas clases a los elementos equivalentes de las cinco secciones (`.card` en cada `<section>`, `.form` y `.form__grupo` en cada formulario y grupo de campos, `.btn` y `.btn--peligro` en los botones, `.tabla` en cada tabla, `.mensaje` con `--error` y `--ok` en cada mensaje) de modo que dos elementos con la misma clase se presentan de manera consistente sin estilos repetidos por sección.

#### Scenario: Formularios visualmente equivalentes

- **WHEN** se comparan el formulario de libros, el de clientes y el de ventas
- **THEN** comparten tipografía, espaciado, bordes y fondos de campo porque provienen de las mismas reglas `.form` / `.form__grupo`, sin reglas específicas por `id` de formulario

#### Scenario: Estilos por clase, no por identificador

- **WHEN** se recorre `css/styles.css`
- **THEN** las reglas se dirigen a clases y estructuras, y no seleccionan elementos por sus `id` (`#form-libro`, `#tabla-libros`, etc.)

### Requirement: Organización del contenido con Flexbox o Grid

El sistema SHALL organizar al menos la navegación, los formularios y la grilla de secciones con Flexbox o Grid, de modo que los elementos se alineen y distribuyan según el ancho disponible.

#### Scenario: Navegación y botones con Flexbox

- **WHEN** se muestra la página en un ancho de 1280 px
- **THEN** los enlaces del `nav` y los botones de cada formulario se alinean con Flexbox (o Grid), sin depender de `float`, `table` o desplazamiento manual con márgenes

#### Scenario: Grilla de campos adaptable

- **WHEN** se redimensiona la ventana entre 320 px y 1280 px
- **THEN** los grupos de formulario pasan de una columna a varias según el espacio, usando Flexbox o Grid con envoltura o `repeat(auto-fit, …)`

### Requirement: Interfaz legible a 320 px sin desplazamiento horizontal

El sistema SHALL presentar a un ancho de 320 px textos y controles legibles, sin que la página entera genere desplazamiento horizontal (`scroll` de `body` o `html` en el eje X).

#### Scenario: Sin scroll horizontal en 320 px

- **WHEN** se visualiza `index.html` con un viewport de 320 px
- **THEN** no aparece barra de desplazamiento horizontal de la página y todo el contenido de las cinco secciones es accesible

#### Scenario: Controles legibles en 320 px

- **WHEN** se inspecciona la página a 320 px
- **THEN** los textos y los campos (`input`, `select`, `output`, botones) conservan un tamaño legible y ningún control se recorta ni queda fuera de su contenedor

### Requirement: Interfaz legible a 1280 px

El sistema SHALL presentar a un ancho de 1280 px un contenido organizado y legible, aprovechando el ancho disponible sin dispersar el texto en líneas demasiado largas.

#### Scenario: Composición equilibrada en 1280 px

- **WHEN** se visualiza `index.html` con un viewport de 1280 px
- **THEN** las secciones ocupan un ancho máximo razonable y quedan centradas o distribuidas, y las tablas y formularios usan el ancho disponible sin desbordar su contenedor

### Requirement: Tablas desplazables dentro de su propio contenedor

El sistema SHALL envolver cada `.tabla` en un contenedor `.tabla-contenedor` que concentre el desplazamiento horizontal, de modo que la `<table>` conserve su `display` nativo de tabla y la página completa no genere desplazamiento horizontal.

#### Scenario: Envoltorio en el HTML y en el JS

- **WHEN** se inspeccionan `index.html` y el detalle generado por `js/ventas.js`
- **THEN** cada `<table class="tabla">` es hija directa de un `<div class="tabla-contenedor">`, y las tablas conservan sus ids, clases y contenido

#### Scenario: Tabla ancha a 320 px

- **WHEN** se muestra la tabla del catálogo (6 columnas) en un viewport de 320 px
- **THEN** el `.tabla-contenedor` presenta scroll horizontal propio y la página no muestra scroll horizontal

#### Scenario: Tabla cómoda a 1280 px

- **WHEN** se muestra la misma tabla en un viewport de 1280 px
- **THEN** todas las columnas caben en el contenedor y no hay desplazamiento interno

#### Scenario: Display nativo de tabla

- **WHEN** se inspecciona el `display` calculado de una `.tabla`
- **THEN** vale `table` (no `block`) y las reglas de tabla se aplican a la `<table>`, mientras que `.tabla-contenedor` aporta `width:100%`, `max-width:100%`, `overflow-x:auto` y `-webkit-overflow-scrolling:touch`

#### Scenario: Celdas sin cortar palabras

- **WHEN** una tabla excede el ancho disponible
- **THEN** las celdas conservan `white-space:nowrap`, de modo que el ancho mínimo de cada columna es el de su contenido y el texto no se parte letra por letra

### Requirement: Foco visible en todos los controles

El sistema SHALL mostrar un indicador de foco visible y contrastante cuando los enlaces, botones, campos de formulario, `select` y `summary` reciben foco por teclado, sin eliminar el contorno de foco de ningún control interactivo.

#### Scenario: Recorrido con Tab

- **WHEN** la persona usuaria recorre la página con la tecla Tab
- **THEN** cada enlace, botón, campo, `select` y `summary` enfocado muestra un foco claramente perceptible (anillo, borde o cambio de fondo con contraste)

#### Scenario: Foco con mouse sin recargar

- **WHEN** se hace clic sobre un control
- **THEN** el indicador de foco no deja la interfaz ilegible ni oculta contenido (se usa `:focus-visible` o un foco estilado consistente)

### Requirement: Mensajes de estado visibles como texto, no solo por color

El sistema SHALL presentar los mensajes `.mensaje` con su texto legible y reforzar el estado de error con un elemento no cromático (marcador textual, icono de texto o borde) además del color, de modo que el error se entiende aunque no se distingan los colores.

#### Scenario: Error comprensible sin distinguir color

- **WHEN** se muestra un `.mensaje--error` (por ejemplo «No se pudo agregar el libro. Revisá los datos.»)
- **THEN** el mensaje incluye su texto y un refuerzo no dependiente del color (p. ej. marcador «⚠» o prefijo textual y borde), de modo que se reconoce como error en escala de grises

#### Scenario: Éxito diferenciado del error

- **WHEN** se muestran `.mensaje--ok` y `.mensaje--error` uno junto al otro
- **THEN** ambos son distinguibles también por su marcador o texto, no únicamente por el color de fondo o de letra

### Requirement: Mensajes ocultos permanecen ocultos hasta que el JS los muestre

El sistema SHALL respetar el atributo `hidden` de los mensajes de estado: ninguno de los 11 mensajes `hidden` del HTML será visible sin JavaScript, y cualquier regla que asigne `display` a `.mensaje` no SHALL anular el ocultamiento nativo.

#### Scenario: Carga sin JavaScript

- **WHEN** se abre `index.html` con CSS cargado y sin JS
- **THEN** los mensajes de vacío, sin resultados, error y éxito de las cinco secciones no ocupan espacio ni se ven, pese a que `.mensaje` tiene reglas de `display`

#### Scenario: JS posterior quita `hidden`

- **WHEN** el JS del Ejercicio 3 elimina el atributo `hidden` de un mensaje
- **THEN** el mensaje se muestra con el estilo `.mensaje` correspondiente (`--error` o `--ok`)

### Requirement: Renglón de venta como componente con distribución propia

El sistema SHALL distribuir el grupo de renglón de venta (selector de libro, cantidad, precio unitario, subtotal y botón «Quitar renglón 1») como componente propio con Flexbox o Grid, legible a 320 px y ordenado a 1280 px, seleccionado por clase o estructura y nunca por `id`.

#### Scenario: Renglón a 320 px

- **WHEN** se muestra el formulario de ventas en un viewport de 320 px
- **THEN** etiquetas, controles, los dos `<output>` y el botón «Quitar renglón 1» se apilan en una o dos columnas, cada uno legible y sin desbordar el formulario

#### Scenario: Renglón a 1280 px

- **WHEN** se muestra el mismo formulario en un viewport de 1280 px
- **THEN** el renglón se organiza en varias columnas (libro y cantidad juntos, precio y subtotal visibles, botón de quitar alineado) aprovechando el ancho disponible

#### Scenario: Selección sin depender de identificadores

- **WHEN** se busca en `css/styles.css` la regla que da estilo al renglón de venta
- **THEN** el selector se basa en la clase del grupo o en su estructura interna (p. ej. presencia de `<output>`), sin usar `#venta-libro-1`, `#venta-cantidad-1` ni otros `id`

### Requirement: Imágenes informativas con texto alternativo y decorativas sin texto redundante

El sistema SHALL dotar de `alt` descriptivo a toda imagen informativa y de `alt` vacío (o equivalente decorativo) a las puramente decorativas, de modo que ninguna imagen decorativa repita información innecesaria para lectores de pantalla.

#### Scenario: Página actual sin imágenes

- **WHEN** se inspecciona `index.html` buscando elementos `<img>`
- **THEN** no existe ninguna imagen, por lo que el requisito no aplica en este ejercicio y queda asentado así en el diseño

#### Scenario: Incorporación futura de imágenes

- **WHEN** en una iteración posterior se agregan imágenes a la página
- **THEN** las informativas llevan `alt` con la información que aportan y las decorativas llevan `alt=""` para no duplicar el texto visible
