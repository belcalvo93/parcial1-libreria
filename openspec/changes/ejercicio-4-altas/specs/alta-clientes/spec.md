# Spec Delta — alta-clientes

## Purpose

Permite registrar clientes desde el formulario con validaciones y verlos al instante en el listado y opciones, sin recargar la página.

## ADDED Requirements

### Requirement: Validación de alta de cliente

El sistema SHALL rechazar el alta si el nombre o el email están vacíos, si el email no tiene formato válido, o si el email normalizado (sin espacios exteriores y en minúsculas) ya existe.

#### Scenario: Email inválido

- **WHEN** se envía `form-cliente` con un email sin formato válido
- **THEN** no se agrega y se muestra `mensaje-error-cliente`

#### Scenario: Email duplicado en mayúsculas

- **WHEN** se intenta agregar un email existente escrito con otras mayúsculas o espacios en los extremos
- **THEN** no se agrega y se muestra `mensaje-error-cliente`

### Requirement: Alta de cliente actualiza sin recargar

El sistema SHALL, con `preventDefault` en el submit y solo si todas las validaciones pasan, agregar el cliente a `DATOS`, re-renderizar `tabla-clientes`, agregar la opción a `venta-cliente`, limpiar el formulario y mostrar `mensaje-ok-cliente`.

#### Scenario: Alta válida de cliente

- **WHEN** se envía `form-cliente` con nombre y email nuevo válido
- **THEN** el cliente aparece en `tabla-clientes` y en `venta-cliente`, y se muestra `mensaje-ok-cliente` sin recargar
