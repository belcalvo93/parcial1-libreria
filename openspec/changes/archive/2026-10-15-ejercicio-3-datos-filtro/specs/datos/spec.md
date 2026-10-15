# Spec Delta — datos

## Purpose

Capa de datos estáticos que el JavaScript del Ejercicio 3 usa para renderizar el catálogo y filtrar por título. Debe ser coherente con los datos hardcodeados en el HTML.

## ADDED Requirements

### Requirement: Objeto DATOS con arrays coherentes

El sistema SHALL proveer un objeto global `DATOS` en `js/datos.js` que contiene los arrays `editoriales`, `generos`, `autores`, `libros` y `clientes` (sin `ventas` — queda para el Ej. 5), con exactamente los mismos datos que están en el HTML de ejemplo.

#### Scenario: Datos coherentes con el HTML

- **WHEN** se lee `DATOS.libros`
- **THEN** contiene 6 libros con los valores congelados del design archivado del Ej. 1 (§5), cada uno con `id`, `titulo`, `isbn`, `precio`, `editorialId`, `generoId` y `autorIds` (array, 1 o más entradas)

#### Scenario: Libro con dos autores

- **WHEN** se lee el libro “Manual para perder trenes”
- **THEN** su `autorIds` contiene 2 ids existentes en `DATOS.autores`

#### Scenario: Libro sin género

- **WHEN** se lee `DATOS.libros` buscando el libro sin género
- **THEN** ese libro tiene `generoId: null` para que el render muestre "Sin clasificar"

#### Scenario: Editoriales, géneros y autores

- **WHEN** se leen `DATOS.editoriales`, `DATOS.generos` y `DATOS.autores`
- **THEN** contienen exactamente 2 editoriales, 2 géneros y 3 autores (los mismos que el HTML)

#### Scenario: Clientes

- **WHEN** se lee `DATOS.clientes`
- **THEN** contiene los 3 clientes congelados (Valentina Ríos, Martín Aguirre, Sofía Benítez) con `id`, `nombre` y `email` estables
