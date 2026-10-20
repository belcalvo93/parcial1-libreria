# alta-libros Specification

## Purpose
Permite registrar libros desde el formulario con validaciones y verlos al instante en los listados y opciones, sin recargar la página.

## Requirements

### Requirement: Selectores de libro generados desde datos locales

El sistema SHALL poblar `libro-editorial`, `libro-genero` y `libro-autores` desde `DATOS` al cargar, incluyendo la opción `sin-clasificar` (“Sin clasificar”) en género.

#### Scenario: Opciones iniciales desde DATOS

- **WHEN** se abre la página y el DOM está listo
- **THEN** los tres selectores muestran exactamente las editoriales, géneros (+ “Sin clasificar”) y autores de `DATOS`

### Requirement: Validación de alta de libro

El sistema SHALL rechazar el alta si el título o el ISBN están vacíos, si el ISBN normalizado (sin espacios ni guiones) ya existe, si el precio no es numérico o no es mayor que cero, si la editorial o algún autor no existen, si no hay al menos un autor, o si el género asignado no existe.

#### Scenario: ISBN duplicado con distinto formato

- **WHEN** se intenta agregar un libro cuyo ISBN normalizado coincide con uno existente (p. ej. con guiones o espacios)
- **THEN** no se agrega y se muestra `mensaje-error-libro`

#### Scenario: Precio inválido

- **WHEN** el precio está vacío, no es numérico o es menor o igual que cero
- **THEN** no se agrega y se muestra `mensaje-error-libro`

#### Scenario: Sin autores

- **WHEN** no se selecciona ningún autor
- **THEN** no se agrega y se muestra `mensaje-error-libro`

### Requirement: Alta de libro actualiza sin recargar

El sistema SHALL, con `preventDefault` en el submit y solo si todas las validaciones pasan, agregar el libro a `DATOS` (género `sin-clasificar` → `generoId: null`), re-renderizar `tabla-libros` y el catálogo, agregar la opción a `venta-libro-1`, limpiar el formulario y mostrar `mensaje-ok-libro`.

#### Scenario: Alta válida de libro

- **WHEN** se envía `form-libro` con un ISBN nuevo y precio válido
- **THEN** el libro aparece en `tabla-libros`, en el catálogo y en `venta-libro-1`, y se muestra `mensaje-ok-libro` sin recargar
