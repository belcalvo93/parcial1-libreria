# documentacion Specification

## Purpose
Documenta cómo usar y verificar el TPI y define el paquete de entrega.

## Requirements

### Requirement: README de entrega

El sistema SHALL incluir un `README.md` en la raíz que identifique a los integrantes y explique: cómo abrir `index.html` sin npm, la navegación entre vistas, las funcionalidades implementadas y pendientes, la ubicación de los datos iniciales, el funcionamiento y los límites de `localStorage` (depende del navegador, del perfil y de la ubicación de apertura; no comparte información entre usuarios), la ubicación y estructura del catálogo guardado y la procedencia de imágenes ajenas si existieran.

#### Scenario: Reproducir la comprobación del precio histórico

- **WHEN** el docente lee el README
- **THEN** encuentra la clave `paginaViva.libros`, la estructura de cada libro y los pasos para modificar solo su precio desde las herramientas del navegador

### Requirement: Casos manuales registrados

El sistema SHALL registrar en el README los 8 casos manuales de la consigna con su resultado esperado y el resultado observado. Un caso no ejecutado o que falla SHALL registrarse con su comportamiento real, nunca como aprobado.

#### Scenario: Caso pendiente

- **WHEN** una funcionalidad necesaria para un caso todavía no está integrada
- **THEN** el resultado observado indica «Pendiente» y el motivo

### Requirement: Paquete de entrega sin dependencias

El sistema SHALL poder empaquetarse en un `.zip` con `index.html`, `css/`, `js/` y `README.md`, sin dependencias instaladas ni herramientas de compilación.

#### Scenario: Generar el zip

- **WHEN** se ejecuta el comando de empaquetado documentado en el README
- **THEN** se obtiene un `.zip` que, descomprimido en cualquier carpeta, abre `index.html` con doble clic y funciona
