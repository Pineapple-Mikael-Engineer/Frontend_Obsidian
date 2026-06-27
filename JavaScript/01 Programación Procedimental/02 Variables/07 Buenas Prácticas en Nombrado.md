---
title: Buenas Prácticas en Nombrado — Convenciones de identificadores en JavaScript
aliases:
  - naming conventions
  - convenciones de nombrado
  - buenas prácticas nombres JS
tags:
  - javascript
  - api/concepto
  - variables
tipo: concepto
draft: false
order: 6
---

# Buenas Prácticas en Nombrado

> [!definicion]
> Las convenciones de nombrado en JavaScript son acuerdos del ecosistema sobre cómo formatear identificadores según su tipo y propósito. Seguirlas hace que el código sea legible por cualquier desarrollador JS sin necesidad de documentación adicional, ya que el nombre comunica el tipo y el comportamiento de la entidad.

## Convenciones por tipo de entidad

### `camelCase` — variables y funciones

La forma estándar para variables, parámetros y funciones. Primera palabra en minúscula, las siguientes capitalizadas, sin separadores.

```js
let nombreUsuario = "Ana García";
let contadorDeIntentos = 0;
const listaDeProductos = [];

function calcularPrecioTotal(items, descuento) { /* … */ }
function obtenerUsuarioActivo() { /* … */ }
function validarFormulario(datos) { /* … */ }
```

### `PascalCase` — clases y constructores

Las clases y funciones constructoras (invocadas con `new`) usan PascalCase. El uso de mayúscula inicial señala que la función construye un objeto.

```js
class Usuario {
  constructor(nombre, email) {
    this.nombre = nombre;
    this.email = email;
  }
}

class CarritoDeCompras {
  agregarItem(producto) { /* … */ }
}

const cuenta = new Usuario("Ana", "ana@email.com");
```

### `SCREAMING_SNAKE_CASE` — constantes de valor fijo

Los valores que representan **constantes de dominio** (no cambian nunca y tienen significado semántico global) se escriben en mayúsculas con guiones bajos.

```js
const MAX_INTENTOS = 3;
const URL_BASE = "https://api.ejemplo.com";
const TIMEOUT_MS = 5000;
const COLORES = Object.freeze({
  PRIMARIO: "#0066CC",
  SECUNDARIO: "#FF6600"
});
```

No toda variable `const` merece este formato — solo las que son literalmente constantes de configuración o dominio. Una variable `const` que almacena el resultado de un cálculo sigue siendo `camelCase`.

```js
const maxIntentos = configuracion.intentos; // camelCase — viene de datos dinámicos
const MAX_INTENTOS = 3;                     // SCREAMING — literal fijo
```

## Nombres descriptivos

El nombre debe comunicar el **qué** sin requerir contexto adicional. Los nombres cortos o abreviados obligan al lector a buscar la declaración.

```js
// ❌ Opaco:
const l = obtenerUsuarios();
const c = 0;
const fn = (x) => x * 1.21;

// ✅ Descriptivo:
const listaDeUsuarios = obtenerUsuarios();
const contadorDeErrores = 0;
const aplicarIVA = (precioBase) => precioBase * 1.21;
```

**Excepción aceptada:** variables de bucle corto (`i`, `j`, `k`) y parámetros de funciones matemáticas de propósito obvio (`x`, `y`, `n`).

## Prefijos por convención

Ciertos prefijos comunican el tipo o el rol de una variable de forma instantánea:

| Prefijo | Señala | Ejemplo |
|---|---|---|
| `is` / `has` / `can` | booleano | `isActivo`, `hasPermiso`, `canEditar` |
| `get` / `set` | accesor | `getUsuario()`, `setConfig()` |
| `on` / `handle` | manejador de evento | `onSubmit`, `handleClick` |
| `fetch` / `load` | operación asíncrona de datos | `fetchProductos()`, `loadConfig()` |
| `_` (guion bajo) | "privado por convención" (legado) | `_cache`, `_id` |

```js
let isMenuAbierto = false;
let hasRolAdmin = usuario.roles.includes("admin");

function onFormSubmit(event) { /* … */ }
function handleKeyDown(event) { /* … */ }

async function fetchPedidos(usuarioId) { /* … */ }
```

El prefijo `_` para indicar propiedades "privadas" es una convención de ES5. En clases modernas se usan campos privados reales con `#`:

```js
class Contador {
  #valor = 0;           // privado real (no accesible desde fuera)
  _nombre = "legado";   // "privado" solo por convención

  incrementar() { this.#valor++; }
}
```

## Nombres que describen el "qué", no el "cómo"

El nombre de una función debe expresar **qué produce o qué hace para el llamador**, no los detalles de implementación que pueden cambiar.

```js
// ❌ Describe el cómo (implementación):
function filtrarArrayPorEstado(arr, estado) { /* … */ }
function recorrerNodosYExtraerTexto(nodo) { /* … */ }

// ✅ Describe el qué (intención):
function obtenerUsuariosActivos(usuarios) { /* … */ }
function extraerTextoDelDOM(nodo) { /* … */ }
```

## Qué evitar

- **Abreviaciones crípticas:** `usrMgr`, `calc`, `tmp`, `proc`, `mgr`. Una excepción: abreviaciones técnicas del dominio que todos conocen (`url`, `id`, `api`, `dto`).
- **Nombres de una sola letra** fuera de bucles y funciones matemáticas: `a`, `b`, `x` sin contexto semántico.
- **Nombres que mienten sobre el tipo:** `const listaUsuarios = {}` (es un objeto, no una lista) o `function getUserData()` que en realidad solo valida y no retorna datos.
- **Verbos en sustantivos y viceversa:** variables con nombres de acción (`const calcular`) o funciones con nombres de datos (`function precio()`).
- **Prefijos redundantes:** `const objUsuario = {}` (ya se sabe que es un objeto), `const strNombre = ""` (notación húngara, no es idiomática en JS).

```js
// ❌ Nombres que mienten:
const data = obtenerUsuarios();     // demasiado genérico
const listaNombres = { ana: 1 };    // no es una lista
function esActivo() { return 42; }  // retorna número, no booleano

// ✅ Claros y honestos:
const usuarios = obtenerUsuarios();
const frecuenciaPorNombre = { ana: 1 };
function obtenerPuntuacion() { return 42; }
```

## Consistencia interna del proyecto

Más importante que seguir cualquier convención específica es ser **consistente dentro del proyecto**. Si el equipo decide usar `handle` en lugar de `on` para eventos, todos deben seguir ese acuerdo. Las guías de estilo (ESLint, Prettier, guías de empresa) codifican estas decisiones.

> [!tip] Buenas prácticas
> Antes de abreviar, preguntarse: ¿entendería este nombre alguien que no escribió este código? Si no, expandir. En código de dominio complejo, un glosario de términos del negocio en el README evita que cada developer invente sus propias abreviaciones.

> [!warning] Errores comunes
> **Reutilizar nombres genéricos:** usar `data`, `result`, `temp`, `obj`, `arr` repetidamente en el mismo módulo dificulta búsquedas y el razonamiento sobre el flujo. Cada variable merece un nombre que la distinga de las demás en su ámbito.
> **Inconsistencia en verbos:** mezclar `get`/`fetch`/`load`/`retrieve` para la misma clase de operación. Elegir uno y aplicarlo uniformemente.

## Notas relacionadas

- [[01 var]]
- [[02 let]]
- [[03 const]]
- [[index|Variables]]
