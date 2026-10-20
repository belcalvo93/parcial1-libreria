# Spec Delta — historial

## Purpose

El historial se genera desde `DATOS.ventas` y conserva los importes confirmados.

## ADDED Requirements

### Requirement: Historial dinámico e inmutable

El sistema SHALL renderizar `tabla-historial` y un `<details>` por venta desde `DATOS.ventas`, usando solo los datos guardados en cada venta (título, cantidad, precio unitario histórico, subtotal y total). Las ventas confirmadas no se editan ni eliminan desde la interfaz.

#### Scenario: Venta nueva en el historial

- **WHEN** se confirma una venta válida
- **THEN** aparece una fila con identificador, fecha, cliente y total, y su `<details>` con los renglones

#### Scenario: Precio histórico

- **WHEN** cambia el precio de un libro en `DATOS.libros` después de confirmar una venta
- **THEN** el detalle de esa venta sigue mostrando el precio unitario, los subtotales y el total confirmados

#### Scenario: Historial vacío

- **WHEN** `DATOS.ventas` está vacío
- **THEN** se oculta `tabla-historial` y se muestra `mensaje-historial-vacia`
