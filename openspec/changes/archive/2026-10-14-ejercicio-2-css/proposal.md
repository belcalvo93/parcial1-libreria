# Proposal

## Why

El Ejercicio 1 dejó `index.html` con la estructura semántica completa, pero `css/styles.css` está vacío: la página se renderiza con los estilos por defecto del navegador, sin identidad visual, sin componentes reutilizables y sin comprobar a 320 px / 1280 px. El Ejercicio 2 de la consigna (20 pts) pide exactamente eso: presentación con CSS y uso adaptable, con accesibilidad básica.

## What Changes

- Se implementa la totalidad de la presentación en `css/styles.css` (único archivo externo, ya enlazado por el HTML): identidad visual propia expresada con variables CSS en `:root` (paleta con hex, tipografía de sistema, escala de espaciado, radios, sombras).
- Componentes reutilizados por clase para elementos equivalentes: `.card`, `.form` / `.form__grupo`, `.btn` / `.btn--peligro`, `.tabla`, `.mensaje` / `.mensaje--error` / `.mensaje--ok`, más un componente propio para el renglón de venta compuesto (selector de libro, cantidad, dos `<output>` y botón «Quitar renglón 1»).
- Organización del contenido con Flexbox (header, nav, formularios, botones, mensajes) y Grid (grillas de formularios y del renglón de venta).
- Adaptación: la interfaz se verifica a 320 px y 1280 px, con textos y controles legibles y sin desplazamiento horizontal de la página; las tablas que necesitan más espacio se desplazan dentro de su propio contenedor (`overflow-x` en el propio `.tabla`).
- Accesibilidad básica: foco visible (`:focus-visible`) en enlaces, botones, campos, `select` y `summary`; mensajes de error expresados con texto (no solo color); regla explícita `[hidden] { display: none !important; }` para que las reglas de `.mensaje` no pisen el comportamiento nativo de `hidden`.
- Restricciones respetadas: **no se modifica `index.html`**, no se escribe JavaScript, sin librerías/frameworks/CDNs/fuentes externas; solo `css/styles.css`.
- El requisito de texto alternativo de imágenes queda documentado como **no aplicable**: el HTML no contiene ningún `<img>` (las tablas son la información visual); se deja constancia en design.md.

## Capabilities

### New Capabilities

- `presentacion`: identidad visual, componentes reutilizables (tarjetas, formularios, botones, mensajes, renglón de venta), organización con Flexbox/Grid, adaptación a 320 px y 1280 px sin scroll horizontal, y accesibilidad básica de la capa de presentación (foco visible, mensajes con texto, imágenes alternativas cuando existan).

### Modified Capabilities

- `estructura-navegacion`: el requisito «Hoja de estilos vacía enlazada y cero dependencias» queda obsoleto: `css/styles.css` deja de estar vacío al implementarse este ejercicio. Se modifica para exigir el enlace al archivo con la presentación, sin estilos en línea ni `<style>`, y manteniendo cero dependencias externas.

## Impact

- **Código**: únicamente `css/styles.css` (de vacío a completo). `index.html`, `js/` y los ids/clases existentes no cambian (el Ejercicio 3 dependerá de esos mismos ids).
- **Specs**: nuevo `openspec/specs/presentacion/spec.md` al archivar; delta MODIFIED en `openspec/specs/estructura-navegacion/spec.md`.
- **Dependencias**: ninguna nueva (sin CDN, sin fuentes externas, solo fuentes del sistema).
- **Fuera de alcance**: JavaScript (Ejercicio 3, otra persona), cambios en el HTML (requerirían propuesta al docente).
