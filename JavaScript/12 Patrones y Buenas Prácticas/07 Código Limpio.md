---
title: Código Limpio
aliases:
  - clean code javascript
  - principios de código limpio js
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
---

# Código Limpio

> [!definicion]
> **Código limpio** es código que cualquier desarrollador puede leer, entender y modificar sin necesidad de explicación adicional. En JavaScript, los principios de Robert C. Martin (Clean Code) se aplican con las particularidades del lenguaje: tipado dinámico, funciones como ciudadanos de primera clase y un ecosistema donde el código que se escribe hoy puede ser heredado mañana.

## Nombres significativos

Los nombres son la documentación más consultada del código. Un nombre incorrecto es más dañino que ningún nombre.

```js
// Mal
const d = new Date();
const arr = usuarios.filter(u => u.f);
function proc(x) { return x * 1.21; }

// Bien
const fechaActual = new Date();
const usuariosActivos = usuarios.filter(u => u.activo);
function aplicarIVA(precioBase) { return precioBase * 1.21; }
```

Convenciones: variables booleanas con prefijo `es`, `tiene`, `puede` (`esAdmin`, `tienePermisos`, `puedeEditar`). Funciones con verbo (`obtener`, `calcular`, `actualizar`, `validar`). Constantes de configuración en SCREAMING_SNAKE_CASE (`MAX_REINTENTOS`, `API_BASE_URL`).

## Funciones pequeñas y de un solo propósito

Una función que requiere varios comentarios internos para explicarse es una función que debe dividirse.

```js
// Mal — múltiples responsabilidades
function procesarFormulario(e) {
  e.preventDefault();
  const nombre = document.querySelector("#nombre").value.trim();
  if (!nombre || nombre.length < 2) {
    document.querySelector("#error").textContent = "Nombre inválido";
    return;
  }
  fetch("/api/usuarios", { method: "POST", body: JSON.stringify({ nombre }) })
    .then(r => r.json())
    .then(u => {
      document.querySelector("#lista").innerHTML += `<li>${u.nombre}</li>`;
    });
}

// Bien — cada función hace una cosa
function obtenerCampo(selector) {
  return document.querySelector(selector).value.trim();
}

function validarNombre(nombre) {
  return nombre.length >= 2;
}

async function crearUsuario(datos) {
  const res = await fetch("/api/usuarios", { method: "POST", body: JSON.stringify(datos) });
  return res.json();
}

function agregarUsuarioALista(usuario) {
  document.querySelector("#lista").innerHTML += `<li>${usuario.nombre}</li>`;
}

async function manejarFormulario(e) {
  e.preventDefault();
  const nombre = obtenerCampo("#nombre");
  if (!validarNombre(nombre)) { mostrarError("Nombre inválido"); return; }
  const usuario = await crearUsuario({ nombre });
  agregarUsuarioALista(usuario);
}
```

## Guard clauses sobre else anidado

Salir temprano de la función en los casos de error aplana la estructura y reduce la carga cognitiva.

```js
// Mal — pirámide de if/else
function procesarPago(usuario, monto) {
  if (usuario) {
    if (usuario.activo) {
      if (monto > 0) {
        return ejecutarPago(usuario, monto);
      } else {
        return { error: "Monto inválido" };
      }
    } else {
      return { error: "Usuario inactivo" };
    }
  } else {
    return { error: "Usuario no encontrado" };
  }
}

// Bien — guard clauses
function procesarPago(usuario, monto) {
  if (!usuario)         return { error: "Usuario no encontrado" };
  if (!usuario.activo)  return { error: "Usuario inactivo" };
  if (monto <= 0)       return { error: "Monto inválido" };
  return ejecutarPago(usuario, monto);
}
```

## Constantes nombradas sobre magic numbers

```js
// Mal
if (intentos > 3) reiniciar();
setTimeout(cerrarSesion, 900000);

// Bien
const MAX_REINTENTOS = 3;
const TIMEOUT_SESION_MS = 15 * 60 * 1000; // 15 minutos

if (intentos > MAX_REINTENTOS) reiniciar();
setTimeout(cerrarSesion, TIMEOUT_SESION_MS);
```

## Preferir inmutabilidad

Los datos que no se mutan son más predecibles, más fáciles de rastrear y compatibles con patrones como undo/redo y comparación por referencia.

```js
// Mutar — difícil de rastrear
function agregarItem(carrito, item) {
  carrito.items.push(item); // modifica el argumento
  carrito.total += item.precio;
}

// Inmutable — devuelve nuevo estado
function agregarItem(carrito, item) {
  return {
    ...carrito,
    items: [...carrito.items, item],
    total: carrito.total + item.precio,
  };
}
```

## Evitar efectos secundarios ocultos

Una función que por su nombre parece calcular o consultar, pero que además modifica el DOM, hace peticiones de red o escribe en localStorage, viola el **principio de mínima sorpresa**.

```js
// Mal — nombre de consulta, efecto de escritura oculto
function obtenerUsuario(id) {
  const usuario = cache[id];
  registrarAcceso(id); // efecto oculto
  document.title = usuario.nombre; // efecto oculto
  return usuario;
}

// Bien — efectos explícitos y separados
function obtenerUsuario(id) { return cache[id]; }
function registrarAcceso(id) { /* ... */ }
function actualizarTitulo(nombre) { document.title = nombre; }
```

## Comentarios que explican el porqué, no el qué

```js
// Mal — el comentario repite el código
// Multiplica precio por 1.21
const precioConIVA = precio * 1.21;

// Bien — explica el porqué, el qué ya es legible
// IVA general en España: 21% (RD 1/2010, Art. 91)
const IVA_GENERAL = 0.21;
const precioConIVA = precio * (1 + IVA_GENERAL);
```

Los comentarios buenos documentan: decisiones de diseño no obvias, workarounds de bugs de terceros, referencias a especificaciones externas, y advertencias de rendimiento no evidentes.

## Antipatrones comunes y su refactoring

| Antipatrón | Code smell | Solución |
|---|---|---|
| Función de 200 líneas con todo | God Function | Dividir por responsabilidad; cada función cabe en pantalla |
| Variables `a`, `b`, `tmp`, `arr` | Nombres crípticos | Nombres descriptivos del dominio |
| `if (condicion) { return true; } else { return false; }` | Retorno booleano redundante | `return condicion;` |
| Callback dentro de callback dentro de callback | Callback hell | Promesas o async/await |
| Estado global mutable compartido | Shared mutable state | Módulo con acceso controlado o inyección |
| Número literal sin nombre | Magic number | Constante nombrada en SCREAMING_SNAKE_CASE |
| `// TODO: arreglar esto` sin ticket | Deuda técnica oculta | Crear issue; si es crítico, arreglarlo ahora |

## Consistencia como principio de equipo

El estilo consistente reduce la carga cognitiva al leer código de otros. Herramientas que automatizan la consistencia:

- **Prettier**: formatea automáticamente el código al guardar. Elimina debates sobre comillas simples/dobles, punto y coma, indentación.
- **ESLint**: detecta antipatrones, código muerto, variables no usadas y reglas de estilo configurables.
- **EditorConfig**: sincroniza configuración básica del editor entre todos los miembros del equipo.

> [!tip]
> El test de legibilidad: si un desarrollador nuevo puede entender el propósito de una función leyendo solo su nombre y firma (sin leer el cuerpo), el nombre es correcto. Si necesita leer el cuerpo para entender el contrato, el nombre es mejorable.

> [!warning]
> Refactorizar hacia código limpio sin cobertura de tests es arriesgado: las mejoras de legibilidad pueden introducir regresiones silenciosas. El orden correcto es: escribir tests que describan el comportamiento actual → refactorizar → verificar que los tests pasan.

## Notas relacionadas

- [[06 Separación de Responsabilidades|Separación de Responsabilidades]]
- [[05 Patrones de Diseño/index|Patrones de Diseño]]
- [[04 Memoización|Memoización]]
