# historial Specification

## Purpose
Muestra el historial de ventas confirmadas con sus datos descriptivos y el detalle de renglones inline, incluyendo al menos una venta de ejemplo coherente con el catálogo.

## Requirements

### Requirement: Listado de ventas con identificador, fecha, cliente y total

El sistema SHALL mostrar en la sección de historial una tabla con `thead`/`th` donde cada venta presenta identificador, fecha, cliente y total, más un mensaje de lista vacía con `hidden` e `id`.

#### Scenario: Venta de ejemplo en el historial

- **WHEN** se visita la sección de historial
- **THEN** la tabla muestra al menos 1 venta de ejemplo con identificador, fecha, cliente y total, y existe el mensaje de historial vacío oculto con `hidden` e `id`

### Requirement: Detalle por venta con details y summary

El sistema SHALL presentar el detalle de cada venta (renglones con libro, cantidad, precio unitario histórico y subtotal) en un `<details>` con `<summary>` inline dentro de la fila o bloque de esa venta, de modo que se consulta sin salir de la página.

#### Scenario: Consulta del detalle inline

- **WHEN** la persona usuaria activa el `<summary>` de una venta
- **THEN** se despliega su detalle con renglones, cantidades, precios unitarios y subtotales, sin navegar a otra página

#### Scenario: Precio histórico conservado en el detalle

- **WHEN** se lee el detalle de la venta de ejemplo
- **THEN** cada renglón muestra el precio unitario como valor histórico copiado al confirmar la venta, coherente con el precio del catálogo de ejemplo
