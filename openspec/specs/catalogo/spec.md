# catalogo Specification

## Purpose
Presenta el catálogo público de la librería con buscador por título y tabla de libros, incluyendo el caso «Sin clasificar» y los mensajes que el filtro del Ejercicio 3 va a necesitar.

## Requirements

### Requirement: Encabezado del catálogo y buscador por título

El sistema SHALL mostrar en la sección de catálogo el título de la librería («Librería Página Viva») y un campo de búsqueda por título con `type="search"`, `label` asociado y un botón con texto descriptivo.

#### Scenario: Buscador presente con etiqueta

- **WHEN** se visita la sección de catálogo
- **THEN** hay un `label` asociado al campo de búsqueda, el campo es `type="search"` y existe un botón que describe la acción de buscar

### Requirement: Tabla de libros con columnas completas

El sistema SHALL mostrar el catálogo en una tabla con `thead`/`th` y una fila por libro con título, ISBN, editorial, género, autores y precio.

#### Scenario: Columnas del catálogo

- **WHEN** se inspecciona la tabla del catálogo
- **THEN** su `thead` contiene encabezados para título, ISBN, editorial, género, autores y precio, y cada fila muestra esos seis datos

### Requirement: Libro sin género como Sin clasificar

El sistema SHALL mostrar el texto «Sin clasificar» en la celda de género de todo libro que no tenga género asignado, y los datos de ejemplo SHALL incluir al menos un libro en esa condición.

#### Scenario: Género ausente visible

- **WHEN** se lee la fila de un libro sin género en el catálogo de ejemplo
- **THEN** su celda de género dice «Sin clasificar»

### Requirement: Mensajes de catálogo vacío y sin resultados

El sistema SHALL incluir un mensaje de lista vacía y un mensaje de búsqueda sin resultados en la sección de catálogo, ambos con `hidden` e `id` claro.

#### Scenario: Mensajes ocultos listos para el filtro

- **WHEN** se carga la página sin JavaScript
- **THEN** existen los mensajes de catálogo vacío y de búsqueda sin resultados, ambos ocultos con `hidden` y con `id` identificable

### Requirement: Render dinámico del catálogo desde datos locales

El sistema SHALL renderizar las filas de `#tabla-catalogo tbody` desde `DATOS.libros` al cargar la página, reemplazando el HTML estático, insertando cada celda como texto con `textContent` (nunca `innerHTML`).

#### Scenario: Catálogo renderizado al cargar

- **WHEN** se abre la página y el DOM está listo
- **THEN** `#tabla-catalogo tbody` muestra 6 filas generadas desde `DATOS.libros`

#### Scenario: Libro sin género visible en el render dinámico

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
