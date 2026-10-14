# Spec Delta — clientes

## Purpose

Permite dar de alta clientes con nombre y email mediante un formulario con tipos correctos y listar los clientes registrados, dejando los mensajes que las validaciones del Ejercicio 4 van a usar.

## ADDED Requirements

### Requirement: Formulario de alta de cliente con tipos correctos

El sistema SHALL proveer en la sección de clientes un `<form>` con `novalidate` y los campos nombre completo (`type="text"`) y email (`type="email"`), cada uno con su `label` (`for`/`id`), más un botón de alta con texto descriptivo. No se exige campo de apellido separado.

#### Scenario: Campos del formulario de cliente

- **WHEN** se inspecciona el formulario de alta de cliente
- **THEN** existen nombre completo y email con sus `label` asociados, los `type` indicados, el formulario lleva `novalidate` y el botón describe la acción de agregar el cliente

### Requirement: Mensajes de éxito y error del formulario de cliente

El sistema SHALL incluir junto al formulario de cliente un contenedor de error y uno de éxito, cada uno con `hidden` e `id` claro, para que el JS del Ejercicio 4 muestre allí sus mensajes.

#### Scenario: Contenedores de mensaje junto al formulario

- **WHEN** se carga la sección de clientes sin JavaScript
- **THEN** existen el contenedor de error y el de éxito junto al formulario, ambos ocultos con `hidden` y con `id` descriptivo

### Requirement: Listado de clientes registrados

El sistema SHALL mostrar en la sección de clientes un listado en tabla con `thead`/`th` con los 3 clientes de ejemplo y un mensaje de lista vacía con `hidden` e `id`.

#### Scenario: Listado inicial con ejemplos

- **WHEN** se visita la sección de clientes
- **THEN** la tabla muestra los 3 clientes de ejemplo y existe el mensaje de lista vacía oculto con `hidden` e `id`
