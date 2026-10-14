# Spec Delta — ventas

## Purpose

Permite componer una venta eligiendo cliente y renglones con cantidades, mostrando precio unitario, subtotal y total, y deja los mensajes que las validaciones del Ejercicio 5 van a usar.

## ADDED Requirements

### Requirement: Selección de cliente y composición de renglones

El sistema SHALL proveer en la sección de ventas un `<form>` con `novalidate` que incluye un `select` de cliente (con los 3 clientes de ejemplo), una zona de renglones donde cada renglón tiene selector de libro, cantidad (`type="number"` con `min`), precio unitario como texto no editable y subtotal como texto, un botón «Agregar renglón», un botón «Quitar» por renglón, un resumen con total y un botón «Confirmar venta». No SHALL existir botón «Limpiar venta».

#### Scenario: Estructura del formulario de venta

- **WHEN** se inspecciona el formulario de ventas
- **THEN** existen el selector de cliente, al menos un renglón con libro, cantidad, precio unitario no editable y subtotal, los botones «Agregar renglón», «Quitar» y «Confirmar venta», el resumen con total, el formulario lleva `novalidate` y no hay ningún botón de limpiar venta

#### Scenario: Precio unitario no editable copiado del catálogo

- **WHEN** se lee el precio unitario de un renglón de ejemplo
- **THEN** es texto no editable cuyo valor coincide con el precio de ese libro en el catálogo

### Requirement: Mensajes de éxito y error del formulario de venta

El sistema SHALL incluir junto al formulario de venta un contenedor de error y uno de éxito, cada uno con `hidden` e `id` claro, para que el JS del Ejercicio 5 muestre allí sus mensajes.

#### Scenario: Contenedores de mensaje junto al formulario

- **WHEN** se carga la sección de ventas sin JavaScript
- **THEN** existen el contenedor de error y el de éxito junto al formulario, ambos ocultos con `hidden` y con `id` descriptivo
