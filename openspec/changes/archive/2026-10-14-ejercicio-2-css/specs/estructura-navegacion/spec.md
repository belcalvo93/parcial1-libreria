# Spec Delta

## REMOVED Requirements

### Requirement: Hoja de estilos vacía enlazada y cero dependencias

**Reason**: El Ejercicio 2 implementa la presentación completa en `css/styles.css`, por lo que el archivo deja de estar vacío y la exigencia de «archivo vacío» queda contradicha por el estado real del sistema.

**Migration**: Reemplazado por el requisito «Hoja de estilos enlazada y cero dependencias» de este mismo delta, que conserva el enlace al archivo externo y la prohibición de dependencias externas, y agrega la exigencia de que toda la presentación viva en ese archivo sin estilos en línea.

## ADDED Requirements

### Requirement: Hoja de estilos enlazada y cero dependencias

El sistema SHALL enlazar `css/styles.css` desde el HTML como la única hoja de estilos externa, con toda la presentación contenida en ese archivo, sin estilos en línea ni bloques `<style>` en el documento, sin código JavaScript y sin librerías, frameworks ni CDNs.

#### Scenario: Enlace a la hoja de estilos externa

- **WHEN** se inspecciona el `head` del documento y el archivo `css/styles.css`
- **THEN** existe el `<link>` a `css/styles.css`, la presentación vive en ese archivo y el HTML no contiene reglas propias (`<style>` ni atributos `style`)

#### Scenario: Sin dependencias externas

- **WHEN** se revisan las etiquetas `<link>`, `<script>` e `<img>` del proyecto
- **THEN** ninguna referencia apunta a una URL externa, CDN, librería o framework
