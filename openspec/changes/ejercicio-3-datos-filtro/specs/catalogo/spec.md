# Spec Delta — catalogo

## Purpose

El catálogo pasa de HTML estático a render dinámico desde `DATOS.libros` con filtro por título en tiempo real. Los mensajes de estado se controlan desde JS.

## ADDED Requirements

### Requirement: Render dinámico del catálogo con texto seguro

El sistema SHALL renderizar las filas de `#tabla-catalogo tbody` desde `DATOS.libros` al cargar la página, reemplazando el HTML estático, insertando cada celda como texto con `textContent` (nunca `innerHTML`).

#### Scenario: Catálogo renderizado al cargar

- **WHEN** se abre la página y el DOM está listo
- **THEN** `#tabla-catalogo tbody` muestra 6 filas generadas desde `DATOS.libros`

#### Scenario: Libro sin género visible

- **WHEN** se renderiza el libro con `generoId: null`
- **THEN** su celda de género muestra "Sin clasificar"

### Requirement: Filtro por título en tiempo real

El sistema SHALL filtrar las filas del catálogo mientras la persona usuaria escribe en `#catalogo-busqueda`.

#### Scenario: Filtro en tiempo real

- **WHEN** la persona usuaria escribe en `#catalogo-busqueda`
- **THEN** solo se muestran los libros cuyo título contiene el texto buscado (parcial, case-insensitive, con `trim` en los extremos)

#### Scenario: Filtro con espacios en los extremos

- **WHEN** la búsqueda es `"  mapa  "`
- **THEN** se comporta igual que `"mapa"` (encuentra “El mapa de las horas lentas”)

#### Scenario: Botón buscar filtra igual que escribir

- **WHEN** se hace click en `#catalogo-buscar`
- **THEN** se aplica el mismo filtro con el valor actual de `#catalogo-busqueda`

#### Scenario: Búsqueda sin resultados

- **WHEN** el filtro no coincide con ningún libro
- **THEN** se muestra `#mensaje-catalogo-sin-resultados` y se oculta la tabla

#### Scenario: Búsqueda vacía muestra todo

- **WHEN** el campo de búsqueda está vacío
- **THEN** se muestran todos los libros y se ocultan los mensajes de estado
