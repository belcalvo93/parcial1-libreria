# Design — ajustes-correccion

## Context

Ver `proposal.md`. Estado actual: los seis ejercicios están archivados y aprobados. En la sección 10 de `css/styles.css`, `.tabla` hace de contenedor con `display:block` y `overflow-x:auto`, lo que anula el `display` nativo de la `<table>`. El HTML tiene cinco tablas y `js/ventas.js` genera una tabla por venta en `crearDetalle`. Restricciones de siempre: HTML/CSS/JS nativos, apertura con doble clic, sin `innerHTML`.

## Goals / Non-Goals

**Goals:**

- Mover el scroll horizontal de la tabla a un contenedor dedicado, sin cambiar la semántica ni el contenido de las tablas.
- Dejar `.tabla` con su `display` nativo y el resto de sus reglas de presentación.
- Actualizar el texto del pie de página.

**Non-Goals:**

- Rediseñar tablas, cambiar columnas o contenido, ni tocar la lógica de ocultamiento con `tabla.hidden`.
- Modificar otros archivos `.js` o los changes archivados.

## Decisions

### 1. `.tabla-contenedor` como único responsable del scroll

El contenedor recibe `width:100%`, `max-width:100%`, `overflow-x:auto` y `-webkit-overflow-scrolling:touch`. Así el desborde se resuelve en el div y la `<table>` puede comportarse como tabla.

Se descartó mantener `display:block` en `.tabla` (el problema que reportó el profesor) y se descartó `overflow-x` en ambos niveles, porque duplicaría el scroll y podría recortar sombras o bordes.

### 2. Envoltorio estático en `index.html`, envoltorio dinámico en `ventas.js`

Las cuatro tablas de sección y la tabla estática del detalle de V-001 se envuelven a mano en el HTML sin tocar ids, clases ni contenido. En `crearDetalle` se crea el div con `createElement("div")`, se le asigna `className = "tabla-contenedor"`, se agrega la tabla dentro y el div (no la tabla) se agrega al `<details>`.

Se descartó `innerHTML` por la restricción del proyecto y se descartó envolver en el render general de `ventas.js`, porque solo `crearDetalle` construye HTML dinámico con tablas.

### 3. `.tabla` conserva `width:100%`, `border-collapse:collapse` y celdas `nowrap`

Al quitar `display:block` y `overflow-x`, la tabla vuelve a su `display` nativo y se le agrega `border-collapse:collapse` para que los bordes de celdas no se dupliquen. `white-space:nowrap` en `th`/`td` se mantiene: es lo que fuerza el ancho mínimo por contenido y hace que el scroll del contenedor tenga sentido a 320 px.

### 4. `hidden` sigue en la tabla

`catalogo.js`, `ventas.js` y los demás scripts ocultan la tabla con `tabla.hidden`. El atributo `hidden` aplica al elemento `<table>` con independencia de su contenedor, así que no hay que cambiar ningún `.js` ni mover el atributo al div.

## Risks / Trade-offs

- [El contenedor agrega un nivel de DOM] → Sin impacto funcional: los selectores existentes apuntan a las tablas por `id` y siguen funcionando.
- [El `hidden` deja el div vacío ocupando espacio si el contenedor tiene padding] → `.tabla-contenedor` no define padding ni borde, por lo que un div con la tabla oculta no aporta altura visible.
- [Tablas angostas a 1280 px] → `width:100%` en la tabla mantiene el ancho completo de la tarjeta; el contenedor no genera scroll si el contenido cabe.

## Migration Plan

Cambio directo sobre el HTML, CSS y `crearDetalle`; no hay datos que migrar. Rollback: revertir el commit del change.
