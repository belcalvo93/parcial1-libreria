# ventas Specification

## Purpose
Permite componer una venta eligiendo cliente y renglones con cantidades, mostrando precio unitario, subtotal y total, y deja los mensajes que las validaciones del Ejercicio 5 van a usar.

## Requirements

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

### Requirement: Composición de la venta con renglones dinámicos

El sistema SHALL poblar `venta-cliente` y los selectores de libro desde `DATOS`, permitir agregar y quitar renglones y cambiar cantidades antes de confirmar, y no admitir el mismo libro en dos renglones.

#### Scenario: Agregar y quitar renglones

- **WHEN** se hace clic en «Agregar renglón» y luego en «Quitar» de ese renglón
- **THEN** el renglón aparece con un libro que no estaba en la venta y luego desaparece, y el total se actualiza

#### Scenario: Libro repetido

- **WHEN** se elige en un renglón un libro que ya está en otro renglón
- **THEN** se muestra `mensaje-error-venta` informando el duplicado, el selector vuelve al libro anterior y no se crea otro renglón

#### Scenario: Opciones actualizadas tras un alta

- **WHEN** se da de alta un libro o un cliente
- **THEN** aparece en los selectores de venta sin recargar

### Requirement: Cálculo de subtotales y total

El sistema SHALL mostrar en cada renglón el precio unitario del catálogo y el subtotal (cantidad × precio), y el total como suma de subtotales, todos con dos decimales.

#### Scenario: Venta de ejemplo

- **WHEN** se cargan dos unidades de un libro de 1500 y una de un libro de 2000
- **THEN** los subtotales son 3000.00 y 2000.00 y el total 5000.00

### Requirement: Confirmación validada de la venta

El sistema SHALL, con `preventDefault`, rechazar la confirmación sin cliente, sin renglones, con cantidades que no sean enteros mayores que cero o con libros repetidos. Si todo pasa, SHALL registrar en `DATOS.ventas` la venta con identificador `V-00N`, fecha, cliente, renglones con copia de título y precio unitario, y total; llamar a `window.guardarDatos()` si existe; limpiar el formulario y mostrar `mensaje-ok-venta`.

#### Scenario: Venta sin cliente

- **WHEN** se confirma con «Elegí un cliente» seleccionado
- **THEN** no se registra la venta y se muestra `mensaje-error-venta`

#### Scenario: Cantidad inválida

- **WHEN** se confirma con una cantidad 0, negativa o decimal
- **THEN** no se registra la venta y se muestra `mensaje-error-venta`

#### Scenario: Venta válida

- **WHEN** se confirma una venta con cliente y renglones válidos
- **THEN** se agrega a `DATOS.ventas` con el siguiente identificador, aparece en el historial, el formulario vuelve a su estado inicial y se muestra `mensaje-ok-venta`
