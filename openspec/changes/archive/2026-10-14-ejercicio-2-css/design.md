# Design

## Context

El Ejercicio 1 dejó `index.html` completo (5 secciones `.card`, 3 formularios `.form` con grupos `.form__grupo`, botones `.btn`/`.btn--peligro`, 5 tablas `.tabla` + 1 anidada en `<details>`, 11 mensajes `.mensaje` con `hidden`) y `css/styles.css` vacío pero ya enlazado. Ver `proposal.md` — Why.

Restricciones que condicionan el diseño: el HTML **no se modifica** (es la base del Ejercicio 3, que dependerá de los mismos ids), no se escribe JavaScript, sin librerías/CDNs/fuentes externas, todo el CSS vive en `css/styles.css`, y la interfaz debe comprobarse a 320 px y 1280 px sin scroll horizontal de la página.

Inventario relevante para el estilo (confirmado leyendo `index.html`):

- 3 formularios reales (`#form-libro`, `#form-cliente`, `#form-venta`); la búsqueda del Catálogo es un `.form__grupo` suelto dentro de la `.card`, fuera de cualquier `<form>`.
- El renglón de venta es un `.form__grupo` **compuesto**: label+`select` de libro, label+`input[type=number]`, dos `<p>` con `<output>` (precio unitario y subtotal) y el botón «Quitar renglón 1» (`.btn--peligro`), todo dentro del mismo grupo.
- Controles: `input[type=search|text|number|email]`, `select`, `select[multiple]`, `<output>`, `<details>/<summary>`.
- No hay ningún `<img>` en el documento.
- Los 11 mensajes ya traen texto visible en el HTML (el JS solo quitará/pondrá `hidden`).

## Goals / Non-Goals

**Goals:**

- Identidad visual propia y coherente, declarada una sola vez como variables CSS en `:root`.
- Componentes reutilizados por clase (`.card`, `.form`/`.form__grupo`, `.btn`, `.tabla`, `.mensaje`, renglón de venta) sin reglas por `id`.
- Layout con Flexbox/Grid y adaptación mobile-first verificable a 320 px y 1280 px, sin scroll horizontal de la página.
- Accesibilidad básica: foco visible, errores legibles sin color, `hidden` respetado.

**Non-Goals:**

- Comportamiento dinámico (validaciones, filtros, mostrar/ocultar mensajes): Ejercicio 3, en `js/`, fuera de este change.
- Cualquier cambio en `index.html` (ver «Observaciones sobre el HTML» más abajo: van como propuestas a Belén, no como tareas).
- Modo oscuro, animaciones elaboradas o personalización por el usuario.
- Tests automatizados (no hay runner en el proyecto; la verificación es manual en el navegador).

## Decisions

### D1. Identidad visual declarada en `:root`

Paleta (papel + tinta, coherente con una librería) y sistema de tokens:

```css
:root {
  /* Color */
  --color-fondo: #f5f2ec;          /* papel */
  --color-superficie: #ffffff;
  --color-texto: #26313b;          /* tinta */
  --color-texto-suave: #5a6a76;
  --color-borde: #d9d3c8;
  --color-primario: #245b8c;       /* azul tinta, enlaces y foco principal */
  --color-primario-oscuro: #1a4266;
  --color-primario-claro: #e8f0f8;
  --color-peligro: #b3261e;        /* botón quitar */
  --color-peligro-oscuro: #8c1d17;
  --color-error-fondo: #fdecea;
  --color-error-texto: #7a1b12;
  --color-error-borde: #c0392b;
  --color-ok-fondo: #e9f7ee;
  --color-ok-texto: #14532d;
  --color-ok-borde: #2e8b57;
  --color-foco: #0b57d0;           /* anillo de foco, contraste AA sobre fondos claros */

  /* Tipografía: solo fuentes del sistema */
  --font-sans: system-ui, -apple-system, "Segoe UI", Roboto,
               "Helvetica Neue", Arial, sans-serif;
  --font-mono: ui-monospace, "Cascadia Mono", Consolas, "Liberation Mono", monospace;
  --fs-base: 1rem;                 /* 16px: legible también a 320 px */
  --fs-sm: 0.875rem;
  --fs-lg: 1.25rem;
  --fs-titulo: clamp(1.5rem, 4vw, 2rem);

  /* Espaciado (escala de 4px) */
  --esp-1: 0.25rem; --esp-2: 0.5rem; --esp-3: 0.75rem;
  --esp-4: 1rem;    --esp-5: 1.5rem; --esp-6: 2rem;

  /* Forma y elevación */
  --radio: 8px; --radio-sm: 4px;
  --sombra: 0 1px 3px rgba(0, 0, 0, 0.12);
}
```

**Por qué:** una sola fuente de verdad hace que la coherencia sea estructural y no una cuestión de recordar valores. **Alternativa descartada:** valores literales en cada regla — se rechaza por duplicación y porque la consigna pide «identidad propia y coherente» explícitable.

Identidad por componente:

- **Tarjetas (`.card`)**: fondo `--color-superficie`, borde `1px solid --color-borde`, `border-radius: var(--radio)`, sombra sutil, padding `var(--esp-5)`, título `h2` con borde inferior o color primario.
- **Formularios**: `.form` con `display: grid` y `gap`; cada `.form__grupo` con `display: flex; flex-direction: column; gap` y label en peso 600 `--color-texto`.
- **Botones**: `.btn` sólido primario, `border-radius: var(--radio-sm)`, padding `var(--esp-2) var(--esp-4)`, `min-height: 44px` (target táctil); `.btn--peligro` en rojo con variante oscura al hover.
- **Mensajes**: `.mensaje` con padding, borde redondeado y `border-left` ancho; `--error` y `--ok` cambian fondo/texto/borde **y** llevan marcador no cromático (ver D6).

### D2. Adaptación: mobile-first con un breakpoint

- **Base (≤ 47.99rem ≈ 320 px):** una sola columna, `body` con `box-sizing` global (`*, *::before, *::after { box-sizing: border-box }`), tipografía base 16 px, `main` con `padding` mínimo, nav con `flex-wrap: wrap`.
- **`@media (min-width: 48rem)` (768 px → cubre el chequeo de 1280 px):** header en fila, nav en fila con `gap`, `.form` en dos columnas con `grid-template-columns: repeat(2, minmax(0, 1fr))`, ancho de contenido con `max-width: 72rem; margin-inline: auto`.
- **Flexbox/Grid:** `header` y `nav ul` con Flexbox (envoltura en mobile, fila en desktop); `main` como Grid de una columna con `gap` (apila las `.card`); `.form` como Grid (2 columnas en desktop); `.form__grupo` como Flexbox columna; el renglón de venta como Grid propio (D4).
- **Sin scroll horizontal de la página:** se evita por construcción, no ocultándolo: `max-width: 0`/`minmax(0, …)` en columnas de grid para que los hijos flex/grid no desborden, `overflow-wrap: anywhere` en celdas y textos largos, `max-width: 100%` en controles (`select`, `input`, `output`), y scroll interno en las tablas (D3). **No** se usa `body { overflow-x: hidden }` porque enmascararía desbordes en lugar de resolverlos; la verificación se hace con la barra de scroll del navegador a 320 px.
- **Por qué un solo breakpoint:** la consigna solo pide comprobar 320 px y 1280 px; un punto intermedio (48rem) deja un diseño de una columna por debajo y de dos por encima, y es suficiente para ambos chequeos. **Alternativa descartada:** breakpoint específico en 320 px (`max-width`) — el mobile-first ya cubre ese ancho y evita duplicar reglas.

### D3. Tablas: `display: block` + `overflow-x` en el propio `.tabla` (decisión con costo)

**Decisión:**

```css
.tabla {
  display: block;
  width: 100%;
  max-width: 100%;
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}
```

**Por qué:** el HTML no tiene un `<div>` contenedor alrededor de ninguna tabla y no puede modificarse; para que «si una tabla necesita más espacio, se desplaza dentro de su propio contenedor», el único candidato disponible es el propio elemento `<table class="tabla">`. Con `display: block` el `<table>` se convierte en el contenedor con scroll y su caja interna (thead/tbody) se auto-ajusta al ancho del contenido.

**Costo (conocido y aceptado):**

1. Cambia el modelo de layout de la tabla: al pasar a `display: block`, la tabla interna resultante ya no estira sus columnas al ancho del contenedor cuando el contenido es angosto (las columnas dejan de llenar el ancho disponible en `#tabla-clientes`, por ejemplo).
2. Riesgo de accesibilidad: algunos pares navegador + lector de pantalla pierden la semántica de tabla (`table`/`row`/`cell`) cuando el elemento `table` tiene `display` distinto de `table`. Los `th scope="col"` del HTML siguen presentes, lo que compensa parcialmente.

**Mitigación:**

- Aplicar `width: 100%` sobre `.tabla` para que al menos el contenedor con scroll ocupe todo el ancho de la tarjeta, y reservar `min-width` razonable solo si una columna queda ilegiblemente aplastada.
- No tocar `display` de `thead`, `tbody`, `tr` ni `th/td`: el cambio se limita al elemento `table`, que es el mínimo para obtener el scroll.
- Verificación manual de semántica (inspección con lector de pantalla / árbol de accesibilidad) junto con el chequeo de 320 px.
- `overflow-x: auto` genera scroll **solo** cuando el contenido lo necesita: a 1280 px las tablas caben y no aparece scroll interno.

**Alternativa descartada (propuesta a Belén, NO implementar):** envolver cada tabla en `<div class="tabla-contenedor">` con `overflow-x: auto` y dejar el `<table>` con `display: table` natural. Sería la solución limpia: conserva el modelo de tabla, la semántica y el llenado de ancho. Implica tocar `index.html` en 5 lugares (tabla de catálogo, de libros, de clientes, de historial y la anidada en `<details>`), cambio mínimo pero fuera del alcance de este ejercicio. **Se registra como propuesta docente**, no como tarea.

### D4. Renglón de venta como componente propio

**Selector elegido: `.form .form__grupo:has(output)`** (con reglas complementarias para los hijos: `label`, `input[type=number]`, `select`, `p`, `button`).

```css
.form .form__grupo:has(output) { display: grid; /* componente renglón */ }
```

**Justificación:**

- **Clase o estructura, nunca `id`:** `#venta-libro-1` o `#venta-quitar-1` quedarían atados a un renglón concreto; el Ejercicio 3 creará renglones dinámicos y el estilo debe aplicarse a cualquiera.
- **`output` como marcador estructural único:** dentro de `.form`, el único `.form__grupo` que contiene `<output>` es el renglón de venta (los dos `<p>` con precio unitario y subtotal). El grupo de cliente y el grupo compuesto del catálogo no lo cumplen. Es una condición semánticamente fiel al componente («grupo que muestra cálculos»).
- **Soporte de `:has()`:** disponible de forma estable en Chrome/Edge 105+ (2022), Safari 15.4+ (2022) y Firefox 121+ (dic 2023); el 2026 ya cubre prácticamente todos los navegadores de escritorio y móviles. Para el entorno de evaluación (abrir `index.html` con doble clic en un navegador actual) es seguro.
- **Degradación elegante:** si `:has()` no estuviera soportado, el selector no matchea y el grupo cae en el estilo genérico de `.form__grupo` (flex columna, una pila vertical). Sigue siendo legible a 320 px y a 1280 px; solo se pierde la disposición en varias columnas. Es decir, el fallo de soporte degrada la estética, nunca la legibilidad.

**Alternativas descartadas:**

- `.form__grupo > p` — matchea, pero es frágil: cualquier grupo con un párrafo (ayuda, error inline) entraría en el componente; además el «Total» es un `<p>` hijo directo del `<form>`, no del grupo, lo que invita a confusiones de alcance.
- Selectores estructurales largos (`.form__grupo > select + input + p + p + button`) — acoplados al orden exacto de los hijos; el Ejercicio 3 puede reordenar o insertar nodos y el estilo se rompe en silencio.
- Clase nueva `.renglon` en el HTML — es la opción más explícita y robusta, pero exige modificar `index.html`, prohibido aquí (propuesta residual a Belén si se quisiera, junto con la de D3).

**Layout del componente:**

- **320 px:** Grid de 1 columna (o 2 con `minmax(0, 1fr)` para libro/cantidad), etiqueta sobre control, los dos `<output>` en su propia fila (monoespaciados con `--font-mono`, fondo `--color-primario-claro`), botón «Quitar renglón 1» a ancho completo.
- **1280 px:** Grid de varias columnas: selector de libro ocupa 1–2 columnas, cantidad 1, precio unitario y subtotal en columnas propias, botón de quitar alineado al final; `align-items: end` para alinear los controles con la línea de base de las etiquetas.
- El «Total» (`<p>` hijo directo del form) y los botones generales se estiran con `grid-column: 1 / -1` cuando el form está en dos columnas.

### D5. `[hidden] { display: none !important; }`

Regla explícita y documentada al inicio del archivo, después del reset:

```css
[hidden] { display: none !important; }
```

**Por qué es necesaria:** cualquier regla posterior que asigne `display` a `.mensaje` (por ejemplo `.mensaje { display: block; … }` o un futuro `display: flex` para alinear un icono) **sobrescribe** el `display: none` que el user agent aplica vía el atributo `hidden`, y los 11 mensajes ocultos aparecerían al cargar la página sin JavaScript — violando el escenario «Mensaje oculto por defecto» del spec `estructura-navegacion` y el spec `presentacion`. El `!important` es deliberado: es la única regla del archivo donde se justifica, porque protege un contrato del HTML contra toda cascada.

### D6. Accesibilidad de la capa de presentación

- **Foco:** regla global `:focus-visible { outline: 3px solid var(--color-foco); outline-offset: 2px; }` para `a`, `button`, `input`, `select`, `summary` y `output` si recibe foco. No se usa `outline: none` en ninguna parte; si un control necesita un foco estilado distinto, se reemplaza por otro anillo visible, nunca por supresión.
- **Errores sin depender del color:** el HTML ya trae el texto («No se pudo agregar el libro. Revisá los datos.») — requisito 6 cubierto de base. El CSS lo refuerza con un elemento **no cromático**: `.mensaje--error::before { content: "⚠ " }` (glifo textual, legible en escala de grises y anunciado por lectores de pantalla) más `border-left: 4px solid`. `.mensaje--ok::before { content: "✓ " }` diferencia el éxito también sin color. Si se prefiriera cero `content`, la alternativa es el borde lateral más un prefijo ya existente en el texto; se elige `::before` porque no toca el HTML.
- **Imágenes:** no hay `<img>` en `index.html`, por lo que el requisito de `alt` **no aplica** en este ejercicio (constancia explícita en el spec `presentacion` y aquí). Si en el futuro se agregan imágenes: informativas con `alt` descriptivo, decorativas con `alt=""`.
- **Tamaño y legibilidad:** `--fs-base: 16px` para texto y controles, `min-height: 44px` en `.btn` y campos, para que a 320 px todo sea tocable y legible.

### D7. Estructura del archivo `css/styles.css`

Orden de las secciones (misma que seguirá `tasks.md`):

1. Regla `[hidden]` + comentario de propósito.
2. `:root` — tokens (D1).
3. Reset base y tipografía del `body` (system font stack, `line-height` 1.5, color de fondo/texto).
4. Header / nav / footer.
5. `main` y `.card`.
6. `.form` y `.form__grupo` genéricos.
7. Componente renglón de venta (D4).
8. Controles: `input`, `select`, `select[multiple]`, `output`, `details`/`summary`.
9. `.btn` / `.btn--peligro`.
10. `.tabla` (D3).
11. `.mensaje` / `.mensaje--error` / `.mensaje--ok`.
12. `:focus-visible` (D6).
13. Media query `min-width: 48rem` (D2).

Un solo archivo (exigencia de la consigna), organizado por capas para que sea reutilizable y auditable.

## Observaciones sobre el HTML (no se modifica)

Ambas son **propuestas para Belén**, no tareas de este change:

1. **El pie dice «Ejercicio 1: estructura HTML».** Ya no refleja el estado real de la página (queda un texto desactualizado una vez presentada la versión con CSS). Propuesta: actualizarlo a algo neutro del TPI (p. ej. «Librería Página Viva — TPI Programación III») o pasarlo por ejercicio. Mientras tanto, el CSS solo le da estilo; el texto se lee tal cual.
2. **La búsqueda del Catálogo es un `.form__grupo` suelto, fuera de cualquier `<form>`.** Funciona (el botón es `type="button"` y no necesita submit), pero rompe la equivalencia «mismo componente = mismo árbol»: el estilo `.form__grupo` se aplica igual por clase, aunque visualmente ese bloque no está envuelto en un `.form` como los otros tres. Propuesta: envolverlo en `<form class="form" novalidate>` o agregarle la clase `form` al contenedor. En este ejercicio se cubre asegurando que `.form__grupo` funcione **tanto dentro como fuera** de `.form` (el selector de grupo no depende del ancestro).

## Risks / Trade-offs

- [`display: block` en `.tabla` puede quitar semántica de tabla en algunos lectores de pantalla] → mitigación D3: cambios mínimos, `th scope="col"` intactos, verificación manual del árbol de accesibilidad; alternativa limpia registrada como propuesta a Belén.
- [`:has()` sin soporte en un navegador muy viejo] → degradación elegante al layout genérico de `.form__grupo` (D4); nada se vuelve ilegible.
- [`!important` en `[hidden]`] → regla única y comentada; si alguien escribe `.mensaje { display: … }` sin respetarla, el bug sería visible de inmediato al cargar la página (mensajes visibles sin JS), fácil de detectar en la verificación manual.
- [`content: "⚠"` duplicado si el JS futuro agrega su propio prefijo] → el JS del Ejercicio 3 solo quita `hidden` (previsto en los specs), no escribe texto; si lo hiciera, se quita el `::before` en ese punto.
- [Ajustes finos de alineación solo visibles en el navegador real] → tasks.md incluye verificación manual a 320 px y 1280 px antes de dar por terminado el ejercicio.

## Migration Plan

No hay despliegue: `index.html` se abre con doble clic. El cambio es aditivo sobre un archivo vacío (`css/styles.css`), por lo que el rollback es `git checkout -- css/styles.css`. Riesgo de regresión hacia el Ejercicio 3: ninguno, porque no se altera ningún `id`, clase, texto ni estructura del HTML.

## Open Questions

- Ninguna que afecte specs, enfoque ni desglose de tareas. Las dos dudas sobre el HTML (pie desactualizado, `.form__grupo` del catálogo) están resueltas como propuestas a Belén en «Observaciones sobre el HTML».
