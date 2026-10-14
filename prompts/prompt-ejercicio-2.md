# Guía y prompt SDD — Ejercicio 2: Presentación con CSS y uso adaptable (TPI Librería)

Responsable: Elías. Depende del Ejercicio 1 (Belén), que ya está en `main` (`index.html` + `css/styles.css` vacío).

## CÓMO USAR ESTA GUÍA CON OPENCODE

1. Abrí la carpeta del repo en OpenCode y creá tu rama desde `main` actualizado:
   ```bash
   git checkout main && git pull
   git checkout -b ejercicio-2-css
   ```
2. Pegá el bloque **PROMPT** de abajo en OpenCode. Trabaja en fases y te pregunta Continuar / Ajustar / Parar después de cada una.
3. Orden de comandos que corresponde (están en `.opencode/commands/`):
   - `/opsx-explore` → mapa de componentes y clases del HTML vs. requisitos del PDF.
   - `/opsx-propose ejercicio-2-css` → genera proposal, specs, design y tasks. **Frená y revisá.**
   - `/opsx-apply ejercicio-2-css` → escribe `css/styles.css` siguiendo `tasks.md`.
   - Verificación manual (checklist del final).
   - `/opsx-archive ejercicio-2-css` → solo cuando todo funcione.
4. Commits chicos por fase y push de tu rama. Pull request a `main` para avisarle al equipo (Hernán sigue con el Ejercicio 3).

## QUÉ HAY EN EL HTML (lo que ya existe y tu CSS debe cubrir)

Estructura: `header` (h1) → `nav[aria-label]` con `ul/li/a` a `#catalogo`, `#libros`, `#clientes`, `#ventas`, `#historial` → `main` con 5 `section.card` → `footer`.

| Clase / elemento | Dónde aparece | Qué estilizar |
|---|---|---|
| `.card` | las 5 `section` | tarjeta: fondo, borde/sombra, padding, separación |
| `.form` | `#form-libro`, `#form-cliente`, `#form-venta` | layout de formulario (Flexbox/Grid) |
| `.form__grupo` | label + control; también el buscador del catálogo y el renglón de venta | label arriba del control, espaciado |
| `input`, `select`, `select[multiple]` | formularios y buscador | ancho 100%, padding, borde, tamaño táctil |
| `.btn`, `.btn--peligro` | botones (Buscar, Agregar, Quitar renglón, Confirmar) | botón primario y variante de peligro |
| `.tabla` (+ `thead th`, `td`) | catálogo, libros, clientes, historial y detalle dentro de `details` | encabezados, filas, scroll propio |
| `.mensaje`, `.mensaje--error`, `.mensaje--ok` | `<p hidden>` bajo cada sección | estados; **ver aviso de `hidden` abajo** |
| `output` | precio, subtotal y total en Ventas | resaltar valores |
| `details` / `summary` | detalle de venta en Historial | cursor, foco, separación |

## PUNTOS DE ATENCIÓN (detectados al leer `index.html`)

1. **`hidden` vs. `display`:** si le das `display` a `.mensaje`, pisa el atributo `hidden` y los mensajes quedan siempre visibles. Agregá `[hidden] { display: none !important; }` o no definas `display` en `.mensaje`.
2. **Tablas a 320 px:** el HTML no tiene un contenedor para el scroll. Sin tocar el HTML, usá `.tabla { display: block; overflow-x: auto; max-width: 100%; }`. Si preferís un `div.tabla-contenedor`, es un cambio en `index.html` (archivo de Belén): consultalo con ella antes.
3. **Errores sin depender del color:** los mensajes ya tienen texto («No se pudo…»). Sumá un indicador además del color (borde grueso/ícono por `::before` con texto como «Error:» / «Listo:»).
4. **Foco visible:** estilá `:focus-visible` en `a`, `button`, `input`, `select`, `summary` con un contorno bien contrastado. No uses `outline: none`.
5. **Imágenes:** el HTML no tiene `img`. El requisito de texto alternativo queda sin aplicar; no agregues imágenes decorativas innecesarias.
6. **Sin scroll horizontal de página:** revisá `box-sizing: border-box`, `max-width: 100%` en controles y que el `nav` envuelva (`flex-wrap`).
7. **Regla del equipo:** no tocar `index.html` salvo acuerdo. Todo el estilo va en `css/styles.css` (archivos externos, sin `<style>` ni `style=""`).

## PROMPT PARA PEGAR EN OPENCODE

```
# PROMPT SDD — Ejercicio 2: Presentación con CSS y uso adaptable (TPI Librería)

## ROL
Actuá como orquestador SDD (OpenSpec), no como ejecutor. Coordiná, delegá a sub-agentes y sintetizá resultados.
El CLI `openspec` es la única fuente de verdad: verificá con `openspec list --json` y `openspec status --change "ejercicio-2-css" --json`. No adivines artefactos.
No encadenes fases sin mi confirmación. Hablame en rioplatense (voseo).

## CONTEXTO
- Materia: Programación III — Repaso Primer Parcial. TPI: frontend de una librería.
- Fuente: `docs/Repaso Primer Parcial.pdf`, Ejercicio 2 (pág. 3), 20 puntos. Para este ejercicio rige lo que dice el Ejercicio 2, no la Etapa 1.
- El Ejercicio 1 ya está archivado: `index.html` (5 vistas, clases `card`, `form`, `form__grupo`, `btn`, `btn--peligro`, `tabla`, `mensaje`, `mensaje--error`, `mensaje--ok`) y `css/styles.css` vacío, ya enlazado. Specs vigentes en `openspec/specs/`.
- Trabajo en cadena: este ejercicio es solo de CSS. Después viene el Ejercicio 3 (JS) de otro integrante.

## ALCANCE TÉCNICO
- Solo HTML, CSS y JS nativos. Sin librerías, frameworks ni CDNs (tampoco Google Fonts ni íconos externos: usá fuentes del sistema).
- Todo el estilo va en `css/styles.css` (archivo externo). Sin `<style>` ni `style=""`.
- NO modificar `index.html` ni escribir JavaScript. Si algo del HTML te impide cumplir un requisito, no lo cambies: listalo como propuesta para consultar con Belén.

## REQUISITOS (del PDF)
1. Identidad visual propia y coherente: colores, tipografía, espaciado, tarjetas, formularios y botones. Definila con variables CSS (`:root`).
2. Clases reutilizables para elementos equivalentes (no estilos por id).
3. Layout con Flexbox o Grid.
4. Comprobado a 320 px y 1280 px: textos y controles legibles, sin desplazamiento horizontal de toda la página. Si una tabla necesita más espacio, se desplaza dentro de su propio contenedor.
5. Foco visible en todos los controles; recorrido completo con teclado.
6. Mensajes de error expresados con texto, no solo con color.
7. Imágenes informativas con `alt`: el HTML actual no tiene imágenes; confirmalo y dejalo documentado.

## CUIDADOS CONOCIDOS
- Los mensajes usan el atributo `hidden`: tu CSS no debe pisarlo (`[hidden] { display: none !important; }`).
- Las tablas no tienen contenedor: resolvé el scroll propio sin tocar el HTML (`.tabla` en bloque con `overflow-x: auto`) y justificá la decisión en `design.md`.
- Hay `<output>`, `<details>/<summary>` y un `<select multiple>` que necesitan estilo propio.

## PLAN
Después de cada fase, mostrame el resultado y preguntame: Continuar / Ajustar / Parar.
1. Verificá `openspec --version` y que `openspec/` exista; verificá que estoy en la rama `ejercicio-2-css`.
2. Explore: leé el Ejercicio 2 del PDF y `index.html`; devolveme un mapa de componentes/clases vs. requisitos.
3. Propose: creá el change `ejercicio-2-css` (proposal, specs, design con paleta, tipografía, escala de espaciado, breakpoints y estrategia de tablas, y tasks). Parás acá para que lo revise.
4. Apply: solo cuando confirme. Implementá siguiendo `tasks.md`.
5. Verificación: listame qué revisar a mano en el navegador (320 px y 1280 px, Tab, mensajes sin `hidden`).
6. Archive: solo cuando confirme. Sincronizá specs y archivá el change.
```

## CHECKLIST DE VERIFICACIÓN MANUAL (para el paso 5)

- [ ] `css/styles.css` tiene contenido y está enlazado; no hay `<style>` ni `style=""` en `index.html`.
- [ ] A 320 px (DevTools → modo responsive): sin scroll horizontal de la página; las tablas se desplazan dentro de su propio contenedor.
- [ ] A 1280 px: el contenido está ordenado y no se estira de borde a borde sin control.
- [ ] Tab recorre nav, buscador, botones, selects, inputs y `summary` con foco bien visible.
- [ ] Quitando `hidden` desde DevTools a `mensaje--error` y `mensaje--ok`: se distinguen por texto/indicador y no solo por color.
- [ ] Con `hidden` puesto, los mensajes no se ven.
- [ ] Botones, tarjetas, formularios y tablas se ven iguales en las 5 vistas (mismas clases = mismo estilo).
- [ ] `index.html` sin cambios (`git diff --stat` solo muestra `css/styles.css` y `openspec/`).
