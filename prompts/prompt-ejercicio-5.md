# PROMPT — Ejercicio 5: Venta simulada e historial (TPI Librería)

Responsable: Bruno. Change: `ejercicio-5-ventas`. Armado con la plantilla de prompts del Parcial 1.

---

## Rol

Actuá como desarrollador frontend, con experiencia en HTML, CSS y JavaScript nativos sin frameworks y responsabilidad sobre la lógica de ventas e historial de la librería, sin pisar el trabajo de los otros ejercicios.

## Metodología

- Metodología a utilizar: SDD con OpenSpec (`explore` → `propose` → `apply` → `archive`), un change por ejercicio.
- Herramientas y convenciones: CLI `openspec` como fuente de verdad del estado (`openspec status --change "ejercicio-5-ventas" --json`, `openspec validate ejercicio-5-ventas`), Git con rama propia `ejercicio-5-ventas` y PR a `main`. Artefactos en español.
- Respetá las fases SPEC → PLAN → IMPLEMENTACIÓN: la SPEC son `proposal.md`, `design.md` y `specs/` del change; el PLAN es `tasks.md`; la IMPLEMENTACIÓN es el `apply`.
- No escribas código ni modifiques el producto antes de completar la especificación y obtener mi aprobación explícita de ella.
- Si el CLI `openspec` no está disponible, avisame y proponé una alternativa antes de seguir.

## Contexto del proyecto

- Nombre o identificador: Librería Página Viva (TPI frontend, Programación III).
- Propósito y usuarios: catálogo, altas de libros y clientes, ventas simuladas e historial, para practicar HTML, CSS y JS. Datos ficticios.
- Ubicación o repositorio: https://github.com/belcalvo93/parcial1-libreria
- Estado actual verificado: Ej. 1 a 4 archivados en `openspec/changes/archive/`. `DATOS` global en `js/datos.js` (sin ventas); `window.renderCatalogo` en `js/catalogo.js`; altas en `js/altas.js`, que hoy agrega opciones a `venta-libro-1` y `venta-cliente`. `form-venta` y `tabla-historial` son maqueta estática del Ej. 1 (renglón 1 y venta V-001). El Ej. 6 (Joaquín, rama `ejercicio-6-persistencia`) agrega `js/persistencia.js` y `window.guardarDatos()`.
- Fuentes de requisitos: `docs/Repaso Primer Parcial.pdf` y la Consigna Primer Parcial (Ejercicio 5, mismo texto en ambas); specs vigentes `openspec/specs/ventas` y `openspec/specs/historial`; `openspec/changes/ejercicio-6-persistencia/design.md` §6 (contrato con el Ej. 5).
- Tecnologías y dependencias permitidas: HTML, CSS y JavaScript nativos, script clásico (sin `type="module"`).
- Tecnologías y dependencias excluidas: librerías, frameworks, CDNs, npm, `fetch`, servidor, base de datos.
- Punto de entrada o componente afectado: `index.html`, secciones `#ventas` e `#historial`.
- Condiciones del entorno: `index.html` se abre con doble clic (`file://`). Se comprueba a 320 px y 1280 px.

## Alcance estricto

### Objetivo de esta entrega

Venta simulada funcional e historial dinámico con precio histórico, integrables con la persistencia del Ej. 6.

### Incluido

- `js/ventas.js` con composición de la venta, cálculos, validaciones, confirmación e historial.
- `ventas` en `DATOS` (`js/datos.js`) con la V-001 de ejemplo del Ej. 1.
- En `js/altas.js`, refrescar los selectores de venta después de cada alta.
- En `index.html`, solo `<script src="js/ventas.js"></script>` al final.

### Excluido

- Stock, cobros, facturación fiscal.
- Editar o eliminar ventas confirmadas.
- Persistencia en `localStorage`, README y `.zip` (Ej. 6).
- Cambios en `css/` y en los changes archivados de otros ejercicios.

No agregues funcionalidades, dependencias, integraciones ni refactorizaciones ajenas a esta entrega. Si detectás una necesidad fuera de alcance, describila como propuesta pendiente; no la implementes sin autorización.

## Requisitos

| ID | Grupo | Requisito verificable |
| --- | --- | --- |
| R-01 | Funcional | Se elige un cliente existente y al menos un renglón con un libro existente y cantidad entera mayor que cero. |
| R-02 | Funcional | No se admite el mismo libro en dos renglones: se informa el duplicado y no se crea otro renglón. |
| R-03 | Funcional | Antes de confirmar se pueden cambiar cantidades y quitar renglones. |
| R-04 | Funcional | Cada renglón muestra libro, cantidad, precio unitario y subtotal; subtotal = cantidad × precio, total = suma de subtotales, con dos decimales. |
| R-05 | Datos y estados | No se confirma sin cliente, sin renglones o con cantidades inválidas (0, negativa, decimal, vacía); el error se muestra en `mensaje-error-venta`. |
| R-06 | Datos y estados | Al confirmar se registra en `DATOS.ventas` identificador `V-00N`, fecha, cliente (id y nombre), renglones con copia de título y `precioUnitario`, y total. |
| R-07 | Funcional | El historial muestra identificador, fecha, cliente y total, y un `<details>` por venta con sus renglones, leídos solo de la venta guardada. Sin botones de editar ni eliminar. |
| R-08 | Funcional | La confirmación limpia el formulario y muestra `mensaje-ok-venta`. |
| R-09 | Estructura e interfaces | Contrato con el Ej. 6: `DATOS.ventas`, `window.guardarDatos()` después del `push` (si existe), id desde `DATOS.ventas.length`, selectores de venta poblados desde `DATOS` al cargar. |
| R-10 | Calidad | Texto insertado con `textContent`/`createElement`, nunca `innerHTML`; mensajes con `hidden`, nunca `style.display`; cada campo con `label` asociado. |
| R-11 | Compatibilidad | Funciona abriendo `index.html` con doble clic; sin desplazamiento horizontal de la página a 320 px; recorrible con teclado. |
| R-12 | Convenciones | Mismo estilo que `js/altas.js` (IIFE, `"use strict"`, `var`, funciones chicas); ids y clases del Ej. 1 sin renombrar; renglones con la estructura `.form__grupo` + `<output>` que estiliza el Ej. 2. |

Datos de ejemplo: la V-001 del Ej. 1 (Valentina Ríos, 2 × «El mapa de las horas lentas» a 1500 y 1 × «Crónicas del patio interior» a 2000, total 5000). Son ficticios.

## Fase 1 — SPEC

Hacé esta fase primero. Leé el Ejercicio 5 de la consigna, las specs vigentes de `ventas` e `historial`, `index.html`, `js/` y el design del Ej. 6 antes de definir la solución.

Generá con `/opsx-propose ejercicio-5-ventas` el change en `openspec/changes/ejercicio-5-ventas/` (`proposal.md`, `design.md`, `specs/ventas/spec.md`, `specs/historial/spec.md`) con:

1. Problema, objetivo y resultado esperado.
2. Contexto verificado y referencias concretas a las fuentes consultadas.
3. Alcance incluido y exclusiones explícitas.
4. Requisitos y escenarios observables, incluidos los casos límite (cantidad 0, negativa, decimal, libro repetido, venta sin renglones).
5. Modelo de la venta guardada y funciones expuestas en `window`.
6. Estados y manejo de errores (mensajes de error y éxito).
7. Restricciones técnicas y convenciones.
8. Criterios de aceptación vinculados a los requisitos.
9. Supuestos, información pendiente y decisiones que requieren mi respuesta.

Si falta una decisión que impide especificar correctamente, preguntame y esperá la respuesta.

Al terminar, presentá la especificación para revisión y detenete.

## Fase 2 — PLAN

Cuando autorice avanzar, completá `openspec/changes/ejercicio-5-ventas/tasks.md` con un checklist de tareas atómicas. Para cada tarea indicá el resultado esperado, el requisito que cubre, los archivos a modificar, su orden y la comprobación que la da por terminada.

Si el plan exige cambiar la especificación, proponé el ajuste y esperá mi aprobación antes de implementar.

## Fase 3 — IMPLEMENTACIÓN

Iniciá esta fase únicamente después de mi aprobación explícita de la especificación y mi autorización para implementar (`/opsx-apply ejercicio-5-ventas`).

- Ejecutá las tareas de `tasks.md` dentro del alcance aprobado.
- Aplicá el estilo de `js/altas.js`, comentarios cortos solo donde algo no sea obvio.
- No hay test runner: verificá cada tarea en el navegador con los casos manuales de la consigna (casos 4 a 7 de la tabla del Ej. 6) y registrá el resultado observado.
- Usá solo HTML, CSS y JS nativos.
- No implementes persistencia, README ni nada de los ejercicios excluidos.
- Si aparece un impedimento o una contradicción, avisame antes de ampliar el alcance.

Entregá commits chicos por fase en la rama `ejercicio-5-ventas` (`Ej5: propose ...`, `Ej5: apply ...`, `Ej5: archive ...`) y un PR a `main`, junto con un resumen de cambios, comprobaciones realizadas, limitaciones y pendientes. Archivá con `/opsx-archive ejercicio-5-ventas` solo cuando todo funcione.

## Criterios de aceptación

- [ ] CA-01 — Venta sin cliente, sin renglones o con cantidad 0, negativa, decimal o vacía: no se confirma y se muestra el error (R-01, R-05).
- [ ] CA-02 — Elegir un libro que ya está en otro renglón: se informa, el selector vuelve al libro anterior y no aparece otro renglón (R-02).
- [ ] CA-03 — 2 × 1500 + 1 × 2000: subtotales 3000.00 y 2000.00, total 5000.00 (R-04).
- [ ] CA-04 — Venta válida: queda en `DATOS.ventas` con el siguiente `V-00N`, aparece en el historial con su detalle, el formulario se limpia y se ve el mensaje de éxito (R-06, R-07, R-08).
- [ ] CA-05 — Cambiar el precio de un libro después de confirmar no cambia el detalle de esa venta (R-06, R-07).
- [ ] CA-06 — Libro y cliente dados de alta aparecen en los selectores de venta sin recargar (R-09).
- [ ] CA-07 — Mergeado con `ejercicio-6-persistencia`: tras recargar, la venta sigue con sus importes, sin duplicar ventas, y los libros y clientes nuevos están en los selectores (R-09).
- [ ] CA-08 — `grep -r innerHTML js/` y `grep -r "style.display" js/` sin resultados; 320 px sin desplazamiento horizontal; orden de tabulación lógico (R-10, R-11).
- [ ] Se cubre todo el alcance aprobado y no se incorpora trabajo excluido.
- [ ] Se respetan las tecnologías, dependencias y convenciones permitidas.
- [ ] Las comprobaciones exigidas se ejecutaron y sus resultados se informan con evidencia.
- [ ] Se distinguen los controles aprobados, fallidos, omitidos y pendientes; ningún control no ejecutado se presenta como aprobado.
- [ ] Los supuestos, placeholders pendientes y limitaciones quedan identificados.

## Restricciones anti-alucinación

- No inventes requisitos, funcionalidades, archivos existentes, APIs, contratos, datos reales ni resultados de comprobaciones.
- Diferenciá los hechos verificados, los supuestos y las propuestas. Atribuí los hechos a las fuentes consultadas.
- Si falta un dato no bloqueante, usá `{{DATO_PENDIENTE: descripción}}` y declaralo como pendiente.
- Si el dato faltante afecta el alcance o una decisión esencial, preguntá antes de avanzar.
- No afirmes haber leído una fuente, ejecutado una herramienta o validado un resultado si no ocurrió.
- Usá datos ficticios solo cuando estén autorizados y señalalos como ejemplos.
- Ante requisitos contradictorios, señalá la contradicción y pedí una decisión.
- No declares completa la entrega si quedan criterios sin verificar.

## Instrucción de inicio

Empezá ahora únicamente por la **Fase 1 — SPEC**. Generá el change, presentalo para revisión y esperá mi respuesta. No avances automáticamente a PLAN ni IMPLEMENTACIÓN.
