# Librería Página Viva — TPI frontend

Programación III · Repaso Parcial 1 · Prof. Matías Torres
Frontend de una librería hecho solo con HTML, CSS y JavaScript nativos: catálogo con filtro, altas de libros y clientes, ventas simuladas con historial y persistencia en `localStorage`.

## Integrantes

| Integrante | Ejercicio |
|---|---|
| Belén Calvo | Ej. 1 — Estructura HTML · Ej. 4 — apply y archive de altas |
| Elías Tello | Ej. 2 — Presentación con CSS |
| Hernán Gonzalez | Ej. 3 — Datos locales y filtro · Ej. 4 — propose de altas |
| Bruno Fiouchetta | Ej. 5 — Venta e historial |
| Joaquín Moran | Ej. 6 — Persistencia, pruebas, README y `.zip` |

## Cómo abrirlo

1. Descomprimí el `.zip` (o cloná el repositorio) en cualquier carpeta.
2. Hacé doble clic en `index.html`. Se abre en el navegador con `file://`.

No hace falta npm, servidor ni conexión a internet: no hay dependencias.

## Navegación

Es una sola página con cinco secciones. El menú superior lleva a cada una con enlaces internos (`#catalogo`, `#libros`, `#clientes`, `#ventas`, `#historial`). Todo se recorre con el teclado: `Tab` avanza, `Shift+Tab` retrocede, `Enter` activa enlaces y botones.

| Vista | Qué tiene |
|---|---|
| Catálogo | Buscador por título y tabla con título, ISBN, editorial, género (o «Sin clasificar»), autores y precio |
| Libros | Formulario de alta y listado de libros |
| Clientes | Formulario de alta y listado de clientes |
| Ventas | Cliente, renglones (libro, cantidad, precio unitario, subtotal), total y confirmación |
| Historial | Ventas confirmadas con su detalle de renglones |

## Funcionalidades

**Implementadas**

- Catálogo generado con JavaScript desde los datos locales; filtro por título sin distinguir mayúsculas, con coincidencia parcial y recorte de espacios; mensajes de catálogo vacío y sin resultados (Ej. 3).
- Alta de libros con selectores generados desde los datos (editorial, género con «Sin clasificar», autores múltiples) y validaciones: título e ISBN obligatorios, ISBN único sin espacios ni guiones, precio numérico mayor que cero, editorial y autores existentes, al menos un autor (Ej. 4).
- Alta de clientes con validaciones: nombre y email obligatorios, formato de email, email único sin distinguir mayúsculas ni espacios exteriores (Ej. 4).
- Después de un alta, el listado, el catálogo y los selectores de venta se actualizan sin recargar (Ej. 4 y 5).
- Venta simulada: cliente existente, renglones que se agregan y quitan, cantidades enteras mayores que cero, sin libros repetidos; subtotal, total con dos decimales y confirmación que limpia el formulario (Ej. 5).
- Historial con identificador, fecha, cliente y total, y el detalle de renglones en un desplegable. El precio unitario se copia al confirmar, así que el historial no cambia si después cambia el catálogo (Ej. 5).
- Persistencia de libros, clientes y ventas en `localStorage`, sin duplicar ejemplos ni ventas al recargar (Ej. 6).
- Diseño adaptable a 320 px y 1280 px, foco visible y tablas con desplazamiento propio (Ej. 2).

**Pendientes**

- No hay pendientes del alcance de la consigna. No se editan ni eliminan registros ni ventas (la consigna no lo exige).

## Datos iniciales

Están en `js/datos.js`, en el objeto global `DATOS`:

- `libros` (6), `clientes` (3) y `ventas` (1, la V-001): son los ejemplos del primer inicio y luego se guardan en `localStorage`.
- `editoriales` (2), `generos` (2), `autores` (3): son fijos, no tienen alta y no se guardan.

Cada libro se relaciona por `id` con una editorial y uno o más autores; `generoId: null` se muestra como «Sin clasificar».

## Persistencia con localStorage

`js/persistencia.js` se carga justo después de `js/datos.js`, antes de que se dibuje nada:

- **Primer inicio:** no hay nada guardado, así que usa los ejemplos de `js/datos.js` y los guarda.
- **Siguientes aperturas:** reemplaza los arrays de `DATOS` por lo guardado. Nunca suma los ejemplos a lo guardado, por eso recargar no duplica nada.
- **Después de cada alta válida** (y de cada venta confirmada) se llama a `guardarDatos()`.

### Claves y estructura

| Clave | Contenido (JSON) |
|---|---|
| `paginaViva.libros` | Catálogo guardado: array de libros |
| `paginaViva.clientes` | Array de clientes |
| `paginaViva.ventas` | Array de ventas confirmadas |

Ejemplo de un libro dentro de `paginaViva.libros`:

```json
{
  "id": "el-mapa-de-las-horas-lentas",
  "titulo": "El mapa de las horas lentas",
  "isbn": "9789500100011",
  "precio": 1500,
  "editorialId": "faro-austral",
  "generoId": "fantasia-urbana",
  "autorIds": ["lucia-ferreyra"]
}
```

Cada venta dentro de `paginaViva.ventas` guarda copia del título y del precio unitario, por eso el historial no depende del catálogo:

```json
{
  "id": "V-001",
  "fecha": "2026-10-14",
  "clienteId": "valentina-rios",
  "clienteNombre": "Valentina Ríos",
  "renglones": [
    { "libroId": "el-mapa-de-las-horas-lentas", "titulo": "El mapa de las horas lentas", "cantidad": 2, "precioUnitario": 1500, "subtotal": 3000 },
    { "libroId": "cronicas-del-patio-interior", "titulo": "Crónicas del patio interior", "cantidad": 1, "precioUnitario": 2000, "subtotal": 2000 }
  ],
  "total": 5000
}
```

### Límites

- Los datos quedan **en ese navegador, en ese perfil y para esa ubicación de apertura**. Otro navegador, otro perfil, otra computadora o abrir el proyecto desde otra ruta puede mostrar los datos de ejemplo.
- **No se comparte información entre usuarios**: no es una base de datos ni hay servidor.
- En Chrome y Edge todas las páginas abiertas con `file://` comparten el mismo almacenamiento, aunque estén en carpetas distintas (lo comprobamos). Por eso las claves llevan el prefijo `paginaViva.`.
- En una ventana de incógnito los datos se borran al cerrarla; borrar los datos de navegación también los borra.
- Cualquiera puede ver y editar estos datos desde las herramientas del navegador: las validaciones son de práctica, no seguridad.
- Si `localStorage` no está disponible, la página funciona igual pero los cambios se pierden al recargar. Si una clave tiene JSON dañado, esa colección vuelve a los ejemplos.

### Volver a los datos de ejemplo

Herramientas del navegador (`F12`) → **Aplicación** → **Almacenamiento local** → `file://` → borrar las tres claves `paginaViva.*`. O en la consola:

```js
["libros", "clientes", "ventas"].forEach(function (c) { localStorage.removeItem("paginaViva." + c); });
```

### Comprobación del precio histórico (en una copia del proyecto)

1. Confirmar una venta y anotar precio unitario, subtotales y total.
2. `F12` → **Aplicación** → **Almacenamiento local** → `file://` → clave `paginaViva.libros`.
3. Editar el valor: cambiar **solo** el `precio` de ese libro. No tocar `paginaViva.ventas`.
4. Recargar la página y abrir el detalle de la venta en Historial: debe conservar los importes anotados. El catálogo muestra el precio nuevo.

## Casos manuales

Probados el 19/10/2026 abriendo `index.html` con `file://` en Chromium, con los Ej. 1 a 6 integrados y partiendo sin datos guardados (claves `paginaViva.*` borradas).

| # | Caso | Resultado esperado | Resultado observado |
|---|---|---|---|
| 1 | Búsqueda con una parte del título en mayúsculas; luego búsqueda vacía y texto inexistente | Coincidencias sin distinguir mayúsculas; catálogo completo; mensaje sin resultados | ✅ «MAPA» muestra solo «El mapa de las horas lentas»; «   PATIO  » encuentra «Crónicas del patio interior». Vacía: los 6 libros, sin mensajes. «zzzz inexistente»: se oculta la tabla y aparece «No se encontraron libros con ese título.» |
| 2 | Alta de libro con ISBN existente y luego con ISBN nuevo | El duplicado se rechaza; el válido aparece en catálogo y selección de venta | ✅ `978-950-0100-01-1` (mismo ISBN que un ejemplo, con guiones): «Ya existe un libro con ese ISBN.», no se agrega. `978-950-0100-07-3`: aparece en el catálogo, en Libros y en el selector de venta, sin recargar |
| 3 | Alta de cliente con email inválido, email existente en mayúsculas y email nuevo | Se rechazan los dos primeros; el nuevo aparece en listado y selección | ✅ `ana.lopez@`: «El email no tiene un formato válido.». `  VALENTINA.RIOS@EJEMPLO.COM `: «Ya existe un cliente con ese email.». `ana.lopez@ejemplo.com`: aparece en Clientes y en el selector de venta |
| 4 | Venta sin cliente, cantidad cero, negativa o decimal; libro repetido | No se confirma; el duplicado no crea otro renglón | ✅ Sin cliente: «Elegí un cliente para la venta.». Cantidad `0`, `-1` y `1.5`: «La cantidad del renglón 1 tiene que ser un número entero mayor que cero.». Elegir en el renglón 2 el mismo libro del renglón 1: «Ese libro ya está en otro renglón…», el selector vuelve al libro anterior y siguen siendo 2 renglones. Ninguna venta se guarda |
| 5 | Venta válida: dos unidades a 1500 y una a 2000 | Subtotales 3000 y 2000; total 5000; detalle conservado en el historial | ✅ Subtotales 3000.00 y 2000.00, total 5000.00. Al confirmar: «Venta V-002 confirmada correctamente.», el formulario se limpia y el historial muestra V-002 con su detalle (2 × 1500.00 = 3000.00 y 1 × 2000.00 = 2000.00) |
| 6 | Venta confirmada y cambio exclusivo del precio del catálogo guardado, en una copia del proyecto | Tras recargar, el precio unitario, los subtotales y el total de la venta conservan los valores confirmados | ✅ Se cambió solo el precio de «El mapa de las horas lentas» a 9999 en `paginaViva.libros`, sin tocar `paginaViva.ventas`. Tras recargar, el catálogo muestra 9999.00 y el detalle de V-002 sigue con 1500.00, 3000.00 y total 5000.00 |
| 7 | Recarga después de altas y venta | Se recuperan datos sin duplicar ejemplos ni ventas | ✅ Tras 3 recargas y tras cerrar y reabrir el navegador: 7 libros, 4 clientes y las ventas V-001 y V-002 una sola vez. El libro y el cliente nuevos siguen en los selectores de venta. La siguiente venta confirmada es V-003 (no se repiten ids) |
| 8 | Navegación por teclado y anchos de 320 px y 1280 px | Foco visible, controles utilizables y tabla con desplazamiento propio | ✅ Con `Tab` se recorren menú, buscador, formularios, renglones de venta, botones y desplegables del historial, todos con foco visible. A 320 px y 1280 px no hay desplazamiento horizontal de la página; las tablas se desplazan dentro de su contenedor |

## Imágenes

El proyecto no usa imágenes ajenas.

## Estructura

```
index.html            Entrada (una página, cinco secciones)
css/styles.css        Estilos propios (Ej. 2)
js/datos.js           Datos iniciales: objeto DATOS (Ej. 3)
js/persistencia.js    localStorage: carga y guardado (Ej. 6)
js/catalogo.js        Catálogo y filtro (Ej. 3)
js/altas.js           Altas de libros y clientes (Ej. 4)
js/ventas.js          Venta simulada e historial (Ej. 5)
openspec/             Specs y changes de cada ejercicio (SDD con OpenSpec)
prompts/              Prompts usados para cada ejercicio
docs/                 Consigna
```

Trabajamos con SDD (OpenSpec): cada ejercicio es un change (`ejercicio-N-...`) con propuesta, specs, diseño y tareas, que se archiva antes de pasar al siguiente.

## Entrega

El `.zip` lleva solo lo necesario para abrir el proyecto (sin dependencias ni herramientas). Desde la carpeta del repositorio:

```bash
git archive --format=zip -o parcial1-libreria.zip HEAD index.html css js README.md
```
