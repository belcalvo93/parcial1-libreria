# PROMPT — Ejercicio 6: Persistencia, verificación y documentación (TPI Librería)

Responsable: Joaquín. Change: `ejercicio-6-persistencia`. Armado con la plantilla de prompts del Parcial 1.

---

## Rol

Actuá como desarrollador frontend, con experiencia en HTML, CSS y JavaScript nativos sin frameworks y responsabilidad sobre la persistencia en `localStorage`, la verificación manual y la documentación de entrega de la librería, sin pisar el trabajo de los otros ejercicios.

## Metodología

- Metodología a utilizar: SDD con OpenSpec (`explore` → `propose` → `apply` → `archive`), un change por ejercicio.
- Herramientas y convenciones: CLI `openspec` como fuente de verdad del estado (`openspec status --change "ejercicio-6-persistencia" --json`, `openspec validate ejercicio-6-persistencia`), Git con rama propia `ejercicio-6-persistencia` y PR a `main`. Artefactos en español.
- Respetá las fases SPEC → PLAN → IMPLEMENTACIÓN: la SPEC son `proposal.md`, `design.md` y `specs/` del change; el PLAN es `tasks.md`; la IMPLEMENTACIÓN es el `apply`.
- No escribas código ni modifiques el producto antes de completar la especificación y obtener mi aprobación explícita de ella.
- Si el CLI `openspec` no está disponible, avisame y proponé una alternativa antes de seguir.

## Contexto del proyecto

- Nombre o identificador: Librería Página Viva (TPI frontend, Programación III).
- Propósito y usuarios: catálogo, altas de libros y clientes, ventas simuladas e historial, para practicar HTML, CSS y JS. Datos ficticios.
- Ubicación o repositorio: https://github.com/belcalvo93/parcial1-libreria
- Estado actual verificado: Ej. 1 a 4 archivados en `openspec/changes/archive/`. `DATOS` global en `js/datos.js` (libros, clientes, editoriales, géneros, autores); `window.renderCatalogo` en `js/catalogo.js`; altas en `js/altas.js`. Todo vive en memoria: al recargar se pierde lo cargado. No existe `README.md`. El Ej. 5 (Bruno, rama `ejercicio-5-ventas`) agrega `js/ventas.js` y `DATOS.ventas` en paralelo.
- Fuentes de requisitos: `docs/Repaso Primer Parcial.pdf`, Ejercicio 6 (pág. 6) y Formato de entrega (pág. 7); specs vigentes en `openspec/specs/`.
- Tecnologías y dependencias permitidas: HTML, CSS y JavaScript nativos, script clásico (sin `type="module"`), `localStorage`.
- Tecnologías y dependencias excluidas: librerías, frameworks, CDNs, npm, `fetch`, servidor, base de datos.
- Punto de entrada o componente afectado: `index.html` (orden de los `<script>`), `js/altas.js`, raíz del repositorio (`README.md`).
- Condiciones del entorno: `index.html` se abre con doble clic (`file://`). En Chrome/Edge todas las páginas `file://` comparten el mismo `localStorage`. Se comprueba a 320 px y 1280 px.

## Alcance estricto

### Objetivo de esta entrega

Conservar libros, clientes y ventas entre recargas sin duplicar datos, registrar los 8 casos manuales de la consigna y documentar la entrega en el README y el `.zip`.

### Incluido

- `js/persistencia.js` con carga y guardado de `DATOS.libros`, `DATOS.clientes` y `DATOS.ventas`, y `window.guardarDatos()`.
- En `index.html`, solo `<script src="js/persistencia.js"></script>` entre `datos.js` y `catalogo.js`.
- En `js/altas.js`, llamar a `guardarDatos()` después de cada alta válida.
- `README.md` con todo lo que pide la pág. 7 y la tabla de casos manuales con su resultado observado.
- Comando para generar el `.zip` de entrega.

### Excluido

- Pantalla para editar precios (no se exige).
- Persistir editoriales, géneros y autores (no tienen alta).
- Lógica de ventas e historial (Ej. 5): solo se acuerda el contrato.
- Cambios en `css/` y en los changes archivados de otros ejercicios.

No agregues funcionalidades, dependencias, integraciones ni refactorizaciones ajenas a esta entrega. Si detectás una necesidad fuera de alcance, describila como propuesta pendiente; no la implementes sin autorización.

## Requisitos

| ID | Grupo | Requisito verificable |
| --- | --- | --- |
| R-01 | Datos y estados | Primer inicio: sin claves guardadas, se usan los ejemplos de `js/datos.js` y se guardan en `paginaViva.libros`, `paginaViva.clientes` y `paginaViva.ventas`. |
| R-02 | Datos y estados | Siguientes aperturas: los arrays de `DATOS` se reemplazan por lo guardado antes de cualquier render; nunca se concatenan, así no se duplican ejemplos ni ventas. |
| R-03 | Funcional | Después de cada alta válida y de cada venta confirmada se llama a `window.guardarDatos()`; lo rechazado no guarda nada. |
| R-04 | Datos y estados | Cambiar solo el precio de un libro en `paginaViva.libros` cambia el catálogo pero no los importes del historial. |
| R-05 | Calidad | Si `localStorage` no está disponible la app sigue en memoria; si una clave tiene JSON dañado, esa colección vuelve a los ejemplos sin romper la página. |
| R-06 | Estructura e interfaces | Contrato con el Ej. 5: ventas en `DATOS.ventas`, renglones con copia de título y `precioUnitario`, `guardarDatos()` al confirmar, id `V-00N` desde `DATOS.ventas.length`, selectores de venta poblados desde `DATOS` al cargar. |
| R-07 | Documentación | README con integrantes, apertura sin npm, navegación, funcionalidades implementadas y pendientes, datos iniciales, funcionamiento y límites de `localStorage`, ubicación y estructura del catálogo guardado, casos manuales y procedencia de imágenes. |
| R-08 | Documentación | Los 8 casos manuales se registran con el resultado observado; un caso no ejecutado o fallido se registra con su comportamiento real, nunca como aprobado. |
| R-09 | Entrega | `.zip` con `index.html`, `css/`, `js/` y `README.md`, sin dependencias ni herramientas. |
| R-10 | Convenciones | Mismo estilo que `js/altas.js` (IIFE, `"use strict"`, `var`, funciones chicas); sin `innerHTML`; ids y clases sin renombrar. |

## Fase 1 — SPEC

Hacé esta fase primero. Leé el Ejercicio 6 y el Formato de entrega de la consigna, `index.html`, `js/` y las specs vigentes antes de definir la solución.

Generá con `/opsx-propose ejercicio-6-persistencia` el change en `openspec/changes/ejercicio-6-persistencia/` (`proposal.md`, `design.md`, `specs/persistencia/spec.md`, `specs/documentacion/spec.md`) con:

1. Problema, objetivo y resultado esperado.
2. Contexto verificado y referencias concretas a las fuentes consultadas.
3. Alcance incluido y exclusiones explícitas.
4. Requisitos y escenarios observables, incluidos los casos límite (primer inicio, recargas repetidas, JSON dañado, almacenamiento no disponible).
5. Claves, estructura guardada y funciones expuestas en `window`.
6. Contrato con el Ej. 5.
7. Restricciones técnicas y convenciones.
8. Criterios de aceptación vinculados a los requisitos.
9. Supuestos, información pendiente y decisiones que requieren mi respuesta.

Si falta una decisión que impide especificar correctamente, preguntame y esperá la respuesta.

Al terminar, presentá la especificación para revisión y detenete.

## Fase 2 — PLAN

Cuando autorice avanzar, completá `openspec/changes/ejercicio-6-persistencia/tasks.md` con un checklist de tareas atómicas. Para cada tarea indicá el resultado esperado, el requisito que cubre, los archivos a modificar, su orden y la comprobación que la da por terminada.

Si el plan exige cambiar la especificación, proponé el ajuste y esperá mi aprobación antes de implementar.

## Fase 3 — IMPLEMENTACIÓN

Iniciá esta fase únicamente después de mi aprobación explícita de la especificación y mi autorización para implementar (`/opsx-apply ejercicio-6-persistencia`).

- Ejecutá las tareas de `tasks.md` dentro del alcance aprobado.
- Aplicá el estilo de `js/altas.js`, comentarios cortos solo donde algo no sea obvio.
- No hay test runner: verificá en el navegador los 8 casos manuales de la consigna, partiendo sin datos guardados, y registrá el resultado observado en el README.
- Usá solo HTML, CSS y JS nativos.
- Si aparece un impedimento o una contradicción, avisame antes de ampliar el alcance.

Entregá commits chicos por fase en la rama `ejercicio-6-persistencia` (`Ej6: propose ...`, `Ej6: ...`, `Ej6: archive ...`) y un PR a `main`, junto con un resumen de cambios, comprobaciones realizadas, limitaciones y pendientes. Archivá con `/opsx-archive ejercicio-6-persistencia` solo cuando el Ej. 5 esté integrado y los 8 casos estén registrados.

## Criterios de aceptación

- [ ] CA-01 — Primer inicio: `localStorage` queda con 6 libros, 3 clientes y la V-001 (R-01).
- [ ] CA-02 — Recargar varias veces y cerrar y reabrir el navegador no duplica ejemplos, altas ni ventas (R-02).
- [ ] CA-03 — Un alta válida o una venta confirmada queda guardada al instante; una rechazada no guarda nada (R-03).
- [ ] CA-04 — Cambiar solo el precio en `paginaViva.libros` cambia el catálogo pero no el detalle de la venta (R-04).
- [ ] CA-05 — JSON dañado en una clave: la página carga sin errores y esa colección vuelve a los ejemplos (R-05).
- [ ] CA-06 — Mergeado con `ejercicio-5-ventas`: tras recargar, los libros y clientes nuevos están en los selectores de venta y la siguiente venta no repite id (R-06).
- [ ] CA-07 — README con todo lo de la pág. 7 y los 8 casos con resultado observado (R-07, R-08).
- [ ] CA-08 — `.zip` que, descomprimido en cualquier carpeta, abre `index.html` con doble clic y funciona (R-09).
- [ ] CA-09 — `grep -r innerHTML js/` sin resultados; `git diff --stat` sin cambios en `css/` (R-10).
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
