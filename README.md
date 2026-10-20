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
- Después de un alta, el listado, el catálogo y el selector de venta se actualizan sin recargar (Ej. 4).
- Venta simulada e historial dinámicos: cliente y renglones, cálculo de precio unitario, subtotal y total, validaciones (cliente, cantidad entera y libro repetido), confirmación sin recargar y detalle de cada venta (Ej. 5).
- Selectores de venta poblados desde `DATOS`, incluidos los libros y clientes nuevos o recuperados (Ej. 5 y 6).
- Persistencia de libros, clientes y ventas en `localStorage`, sin duplicar ejemplos ni ventas al recargar (Ej. 6).
- Diseño adaptable a 320 px y 1280 px, foco visible y tablas con desplazamiento propio (Ej. 2).

**Pendientes**

- No se editan ni eliminan registros (la consigna no lo exige).

## Datos iniciales

Están en `js/datos.js`, en el objeto global `DATOS`:

- `libros` (6), `clientes` (3), `ventas` (1: V-001, con su detalle): son los ejemplos del primer inicio y luego se guardan en `localStorage`.
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

Probados el 19/10/2026 en Chrome con Live Server (`http://127.0.0.1:5500`) para los casos 4 a 7 y con `file://` en Chromium para los casos 1 a 3 y 8.

| # | Caso | Resultado esperado | Resultado observado |
|---|---|---|---|
| 1 | Búsqueda con una parte del título en mayúsculas; luego búsqueda vacía y texto inexistente | Coincidencias sin distinguir mayúsculas; catálogo completo; mensaje sin resultados | ✅ «MAPA» muestra solo «El mapa de las horas lentas»; «   PATIO  » encuentra «Crónicas del patio interior». Vacía: los 6 libros, sin mensajes. «zzzz inexistente»: se oculta la tabla y aparece «No se encontraron libros con ese título.» |
| 2 | Alta de libro con ISBN existente y luego con ISBN nuevo | El duplicado se rechaza; el válido aparece en catálogo y selección de venta | ✅ `978-950-0100-01-1` (mismo ISBN que un ejemplo, con guiones): «Ya existe un libro con ese ISBN.», no se agrega. `978-950-0100-07-3`: aparece en el catálogo, en Libros y en el selector de venta, sin recargar |
| 3 | Alta de cliente con email inválido, email existente en mayúsculas y email nuevo | Se rechazan los dos primeros; el nuevo aparece en listado y selección | ✅ `ana.lopez@`: «El email no tiene un formato válido.». `  VALENTINA.RIOS@EJEMPLO.COM `: «Ya existe un cliente con ese email.». `ana.lopez@ejemplo.com`: aparece en Clientes y en el selector de venta |
| 4 | Venta sin cliente, cantidad cero, negativa o decimal; libro repetido | No se confirma; el duplicado no crea otro renglón | ✅ Venta sin cliente: «Elegí un cliente para la venta.». Cantidad `0`, `-1` o `1,5`: «La cantidad del renglón 1 tiene que ser un número entero mayor que cero.». Mismo libro en dos renglones: «Ese libro ya está en otro renglón. Cambiá la cantidad de ese renglón.» y el select vuelve al libro anterior, sin crear un renglón duplicado |
| 5 | Venta válida: dos unidades a 1500 y una a 2000 | Subtotales 3000 y 2000; total 5000; detalle conservado en el historial | ✅ El mapa de las horas lentas × 2 (subtotal 3000.00) + Crónicas del patio interior × 1 (subtotal 2000.00): total 5000.00. «Venta V-002 confirmada correctamente.» y aparece en el Historial con su detalle |
| 6 | Venta confirmada y cambio exclusivo del precio del catálogo guardado, en una copia del proyecto | Tras recargar, el precio unitario, los subtotales y el total de la venta conservan los valores confirmados | ✅ Se cambió en `localStorage` el precio de El mapa de las horas lentas a 9999 y se recargó: el catálogo muestra 9999.00 y el detalle de V-002 conserva 1500.00 / 3000.00 / total 5000.00 |
| 7 | Recarga después de altas y venta | Se recuperan datos sin duplicar ejemplos ni ventas | ✅ Se agregó un libro (El principito), un cliente (Ana Pérez) y una venta nueva; después de recargar 3 veces hay 7 libros, 4 clientes y las ventas V-001 a V-003, sin duplicados |
| 8 | Navegación por teclado y anchos de 320 px y 1280 px | Foco visible, controles utilizables y tabla con desplazamiento propio | ✅ Con `Tab` se recorren menú, buscador, botón y tabla con foco visible. A 320 px y 1280 px no hay desplazamiento horizontal de la página; la tabla se desplaza dentro de su contenedor |

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
js/ventas.js          Venta e historial (Ej. 5)
openspec/             Specs y changes de cada ejercicio (SDD con OpenSpec)
prompts/              Prompts usados para cada ejercicio
docs/                 Consigna
```

Trabajamos con SDD (OpenSpec): cada ejercicio es un change (`ejercicio-N-...`) con propuesta, specs, diseño y tareas, que se archiva antes de pasar al siguiente.

## Entrega

El `.zip` de entrega debe incluir **todo el repositorio**, incluida la carpeta `openspec/` (lo pide el profesor). Desde la carpeta del repositorio:

```bash
git archive --format=zip -o parcial1-libreria.zip HEAD
```
