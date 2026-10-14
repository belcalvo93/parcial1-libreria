# Tasks

## 1. Base, tokens y reglas de protección

- [ ] 1.1 Escribir al inicio de `css/styles.css` la regla `[hidden] { display: none !important; }` con comentario que explica que protege los 11 mensajes `hidden` de las reglas de `display` de `.mensaje` (verificación: abriendo `index.html` sin JS, ningún `.mensaje` es visible ni ocupa espacio)
- [ ] 1.2 Declarar en `:root` los tokens de `design.md` D1: paleta con hex (`--color-fondo`, `--color-superficie`, `--color-texto`, `--color-texto-suave`, `--color-borde`, `--color-primario` y variantes, `--color-peligro`, `--color-error-*`, `--color-ok-*`, `--color-foco`), `--font-sans`/`--font-mono` de sistema, escala `--esp-1…--esp-6`, `--fs-*`, `--radio`, `--sombra` (verificación: `grep -c "var(--" css/styles.css` da > 0 y el archivo no contiene colores literales fuera de `:root` ni `@font-face`/`@import`)
- [ ] 1.3 Escribir el reset base: `*, *::before, *::after { box-sizing: border-box }`, `body` con `margin: 0`, `font-family: var(--font-sans)`, `font-size: var(--fs-base)`, `line-height: 1.5`, `color` y `background` de tokens, e `img { max-width: 100% }` (verificación: al abrir `index.html` la página usa la tipografía del sistema y no hay scroll horizontal por defecto a 1280 px)

## 2. Estructura global: header, nav, footer y tarjetas

- [ ] 2.1 Estilizar `header` e `h1` con Flexbox centrado, título con `--fs-titulo` y color primario (verificación: el encabezado se ve centrado y legible a 320 px y 1280 px)
- [ ] 2.2 Estilizar `nav ul` con Flexbox `flex-wrap: wrap` y `gap`, enlaces con `--color-primario` y subrayado sutil al hover; verificar que el foco visible de la tarea 8.1 se aplica a los enlaces (verificación: recorrer los 5 enlaces con Tab a 320 px y confirmar que se ven todos y cada uno muestra su anillo de foco)
- [ ] 2.3 Estilizar `main` como Grid de una columna con `gap`, `max-width` razonable y `margin-inline: auto` (verificación: a 1280 px las cinco `.card` quedan centradas con ancho máximo; a 320 px ocupan todo el ancho sin desbordar)
- [ ] 2.4 Estilizar `.card` (fondo, borde, `border-radius`, sombra, padding) y sus `h2` como identidad de tarjeta reutilizada en las 5 secciones (verificación: las 5 secciones lucen idénticas entre sí sin reglas por `id` de sección)
- [ ] 2.5 Estilizar `footer` y su `p` (tipografía pequeña, `--color-texto-suave`, fondo coherente) sin alterar el texto «Ejercicio 1: estructura HTML» (verificación: el pie conserva literalmente su texto y queda legible; la discrepancia queda registrada como propuesta a Belén en `design.md`)

## 3. Formularios y grupos

- [ ] 3.1 Estilizar `.form` con `display: grid` y `gap`, y `.form__grupo` con Flexbox columna (`label` en peso 600, `gap` entre etiqueta y control), aplicable tanto dentro como fuera de `.form` (verificación: los 3 formularios y el `.form__grupo` suelto del Catálogo comparten espaciado y tipografía; no existen reglas con `#form-libro`, `#form-cliente` ni `#form-venta`)
- [ ] 3.2 Estilizar los controles base `input`, `select` y `button` dentro de grupos: `width: 100%`, `max-width: 100%`, `min-height: 44px`, `padding`, `border`, `border-radius`, fondo de superficie y `font: inherit` (verificación: a 320 px ningún campo se recorta ni provoca scroll horizontal de la página)

## 4. Controles especiales

- [ ] 4.1 Estilizar `input[type="search"]`, `input[type="number"]`, `input[type="email"]` y `input[type="text"]` de forma consistente, con `appearance` natural (verificación: los campos de catálogo, libros, clientes y ventas se ven como una misma familia de controles)
- [ ] 4.2 Estilizar `select` simple y `select[multiple]` (autores): fondo, borde y `min-height` distinto para el múltiple con suficiente altura para varias opciones (verificación: el `select multiple` de autores muestra al menos 3 opciones visibles sin desbordar su grupo)
- [ ] 4.3 Estilizar `output` con `--font-mono`, fondo `--color-primario-claro`, `padding` y `border-radius` (verificación: `#venta-precio-1`, `#venta-subtotal-1` y `#venta-total` se distinguen de los campos editables y caben a 320 px)
- [ ] 4.4 Estilizar `details` y `summary` del historial: `summary` con cursor y foco visible, `details` con espaciado interno; la tabla anidada dentro de `details` conserva la clase `.tabla` y su scroll propio (verificación: abrir/cerrar «Ver detalle de V-001» con ratón y con teclado y ver su tabla con scroll interno a 320 px)

## 5. Componente renglón de venta

- [ ] 5.1 Crear el componente con el selector estructural `.form .form__grupo:has(output)` en `display: grid` (sin ningún `id` en el selector) y aplicar `grid-column: 1 / -1` al grupo para que ocupe el ancho completo del formulario en dos columnas (verificación: `grep "venta-" css/styles.css` no devuelve coincidencias mientras el grupo se estila correctamente)
- [ ] 5.2 Disponer los hijos del renglón: label+select de libro y label+cantidad en columnas, los dos `<output>` (precio unitario y subtotal) en filas/columnas propias, y el botón `.btn--peligro` «Quitar renglón 1» alineado (verificación: a 320 px todo se apila legible; a 1280 px libro, cantidad, precio, subtotal y botón se distribuyen en varias columnas aprovechando el ancho)

## 6. Botones

- [ ] 6.1 Estilizar `.btn`: fondo primario, texto blanco, `border-radius`, `min-height: 44px`, padding, variante `:hover`/`:active` en `--color-primario-oscuro` (verificación: los 5 `.btn` de la página —buscar, libro, cliente, venta-agregar, venta-confirmar— lucen idénticos)
- [ ] 6.2 Estilizar `.btn--peligro` encadenado sobre `.btn` (fondo `--color-peligro`, hover oscuro) para «Quitar renglón 1» (verificación: el botón de quitar es claramente distinguible del `.btn` común sin recurrir solo al texto)

## 7. Tablas

- [ ] 7.1 Implementar la estrategia de `design.md` D3 en `.tabla`: `display: block; width: 100%; max-width: 100%; overflow-x: auto;` sin tocar el `display` de `thead`, `tbody`, `tr` ni celdas (verificación: a 320 px la tabla de catálogo (6 columnas) hace scroll **dentro de su propio contenedor** y la página no muestra scroll horizontal; a 1280 px no hay scroll interno)
- [ ] 7.2 Dar identidad a celdas y encabezados usando selectores descendientes de `.tabla` (`th`, `td`, filas alternadas si se desea) con `--color-borde`, fondo de encabezado y `overflow-wrap` para textos largos (verificación: las 5 tablas + la anidada en `<details>` comparten estilo y ninguna celda desborda su contenedor)

## 8. Mensajes, foco y accesibilidad

- [ ] 8.1 Escribir la regla global `:focus-visible` con anillo visible (`outline: 3px solid var(--color-foco); outline-offset: 2px`) para `a`, `button`, `input`, `select`, `summary`; verificar que no existe ningún `outline: none` sin reemplazo (verificación: recorrer la página completa solo con Tab a 320 px y a 1280 px y ver un foco perceptible en cada control)
- [ ] 8.2 Estilizar `.mensaje` (fondo, padding, borde redondeado, `border-left` ancho) con reglas de `display` compatibles con la tarea 1.1 (verificación: cargando sin JS, los 11 mensajes siguen invisibles aunque `.mensaje` tenga `display` declarado)
- [ ] 8.3 Estilizar `.mensaje--error` con `--color-error-*` **y** el refuerzo no cromático `::before` con marcador textual «⚠ »; `.mensaje--ok` con `--color-ok-*` y marcador «✓ » (verificación: quitando manualmente el atributo `hidden` con las DevTools, el error se reconoce como error también simulando escala de grises / desactivando color, y `--ok` se distingue de `--error` sin mirar el color)
- [ ] 8.4 Confirmar el caso de imágenes: revisar `index.html` y asentar que no hay `<img>`, por lo que no hay `alt` que escribir (verificación: `grep -c "<img" index.html` devuelve 0)

## 9. Adaptación 320 px y 1280 px

- [ ] 9.1 Escribir el media query `@media (min-width: 48rem)`: header y nav en fila, `.form` en `grid-template-columns: repeat(2, minmax(0, 1fr))`, ancho máximo de contenido centrado (verificación: a 1280 px los formularios ocupan 2 columnas y a 320 px vuelven a 1, sin reglas duplicadas fuera del media query)
- [ ] 9.2 Revisar desbordes: aplicar `min-width: 0` a hijos de grid/flex, `max-width: 100%` a `select`/`input`/`output` y `overflow-wrap: anywhere` donde hagan falta celdas o textos largos (verificación: con DevTools, ningún elemento de las 5 secciones excede el ancho del viewport a 320 px)

## 10. Verificación manual integral

- [ ] 10.1 Abrir `index.html` con doble clic (sin servidor, sin red) y verificar en el navegador: (a) viewport 320 px → sin barra de scroll horizontal de la página, textos y controles legibles, tabla con scroll interno propio; (b) viewport 1280 px → composición equilibrada, tablas sin scroll interno; (c) recorrido completo con Tab → foco visible en enlaces, botones, campos, `select` y `summary`; (d) sin JS → los 11 mensajes ocultos; forzando visibilidad de un `.mensaje--error` → texto + marcador «⚠»; (e) ninguna referencia externa (sin CDN, sin fuentes externas, sin `<style>` ni `style=""` en el HTML); (f) `js/` e `index.html` intactos (`git status` sin cambios fuera de `css/styles.css` y `openspec/`)
- [ ] 10.2 Correr `openspec validate ejercicio-2-css` y confirmar que pasa sin errores (verificación: salida del CLI sin errores de validación)

## Workflow follow-up

- Archivar el change (`/opsx:archive ejercicio-2-css`) después de la revisión, sincronizando el delta de `presentacion` como spec principal y el MODIFIED de `estructura-navegacion`.
- Dejar las dos propuestas a Belén (texto del footer, envolver la búsqueda del catálogo en `.form`, y opcionalmente el `<div class="tabla-contenedor">` de D3) pendientes de aprobación docente; no implementarlas en este change.
