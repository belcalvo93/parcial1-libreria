# Spec Delta — libros

## Purpose

Permite dar de alta libros mediante un formulario con tipos correctos y listar los libros registrados en una tabla, dejando los mensajes que las validaciones del Ejercicio 4 van a usar.

## ADDED Requirements

### Requirement: Formulario de alta de libro con tipos correctos

El sistema SHALL proveer en la sección de libros un `<form>` con `novalidate` y los campos título (`type="text"`), ISBN (`type="text"`), precio (`type="number"` con `min`), editorial (`select` con las editoriales fijas), género (`select` con los géneros fijos más la opción «Sin clasificar») y autores (`select` múltiple con los autores fijos), cada campo con su `label` (`for`/`id`), más un botón de alta con texto descriptivo.

#### Scenario: Campos del formulario de libro

- **WHEN** se inspecciona el formulario de alta de libro
- **THEN** existen los seis campos con sus `label` asociados, los `type` y el `min` indicados, el formulario lleva `novalidate` y el botón describe la acción de agregar el libro

#### Scenario: Opciones fijas sin ABM

- **WHEN** se revisan las opciones de editorial, género y autores
- **THEN** corresponden exactamente a los datos de ejemplo (2 editoriales, 2 géneros más «Sin clasificar», 3 autores) sin pantallas de alta, baja o modificación para esas entidades

### Requirement: Mensajes de éxito y error del formulario de libro

El sistema SHALL incluir junto al formulario de libro un contenedor de error y uno de éxito, cada uno con `hidden` e `id` claro, para que el JS del Ejercicio 4 muestre allí sus mensajes.

#### Scenario: Contenedores de mensaje junto al formulario

- **WHEN** se carga la sección de libros sin JavaScript
- **THEN** existen el contenedor de error y el de éxito junto al formulario, ambos ocultos con `hidden` y con `id` descriptivo

### Requirement: Listado de libros registrados

El sistema SHALL mostrar en la sección de libros un listado en tabla con `thead`/`th` con los libros registrados (datos de ejemplo iniciales) y un mensaje de lista vacía con `hidden` e `id`.

#### Scenario: Listado inicial con ejemplos

- **WHEN** se visita la sección de libros
- **THEN** la tabla muestra los 6 libros de ejemplo y existe el mensaje de lista vacía oculto con `hidden` e `id`
