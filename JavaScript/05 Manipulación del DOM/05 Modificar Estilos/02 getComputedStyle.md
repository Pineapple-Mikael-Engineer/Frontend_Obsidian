---
title: "getComputedStyle"
aliases:
  - window.getComputedStyle
  - estilos calculados
  - computed style JS
tags:
  - javascript
  - api/metodo
  - dom
draft: false
order: 2
---

# `getComputedStyle`

> [!definicion]
> `window.getComputedStyle(elemento, pseudoElemento?)` devuelve un objeto `CSSStyleDeclaration` de **solo lectura** que contiene todos los estilos aplicados al elemento una vez resuelta la cascada completa: hojas de estilo externas, estilos inline, herencia, estilos del navegador y valores calculados. Es la única forma de leer el valor real de cualquier propiedad CSS tal como el navegador la renderiza.

```js
const el = document.querySelector(".caja");
const estilos = window.getComputedStyle(el);

console.log(estilos.color);          // "rgb(30, 30, 30)"
console.log(estilos.fontSize);       // "16px"
console.log(estilos.display);        // "flex"
console.log(estilos.marginTop);      // "24px"  — aunque la hoja dijera "1.5rem"
```

## Valores resueltos

`getComputedStyle` no devuelve los valores tal como están escritos en CSS — devuelve los valores **resueltos** que el navegador usa realmente:

- `font-size: 1em` → `"16px"` (convertido a píxeles absolutos)
- `color: red` → `"rgb(255, 0, 0)"` (siempre en formato `rgb()` o `rgba()`)
- `width: 50%` → `"400px"` (porcentaje resuelto a píxeles)
- `margin: 0 auto` → `"0px"` / `"120px"` (shorthand expandido, valores individuales)

Esto es importante cuando se necesita el valor numérico real para cálculos de layout.

## Formas de acceso

```js
const cs = getComputedStyle(el); // `window` es implícito

// Acceso por nombre camelCase (como element.style)
const color = cs.color;
const fontSize = cs.fontSize;

// Acceso con getPropertyValue (nombre CSS con guiones)
const bg = cs.getPropertyValue("background-color");
const size = cs.getPropertyValue("font-size");

// Variables CSS custom properties
const acento = cs.getPropertyValue("--color-acento");
const espaciado = cs.getPropertyValue("--espaciado");
```

Para variables CSS, `getPropertyValue` con el nombre completo (incluyendo `--`) es la única forma que funciona.

## Pseudoelementos

El segundo argumento opcional permite leer los estilos de pseudoelementos `::before` y `::after`:

```js
const pseudo = getComputedStyle(el, "::before");
console.log(pseudo.content);   // '"★"'  — el valor de content incluyendo comillas
console.log(pseudo.color);     // "rgb(255, 99, 71)"
console.log(pseudo.width);     // "auto"
```

Útil para extraer información de content generado, dimensiones calculadas de decoraciones, etc.

## Tabla comparativa: `style` vs `getComputedStyle`

| Aspecto | `element.style` | `getComputedStyle(el)` |
|---|---|---|
| Lectura | Solo estilos inline | Todos los estilos aplicados (cascada completa) |
| Escritura | Sí | No (solo lectura) |
| Fuente de estilos | Atributo `style` del elemento | Hojas de estilo + inline + herencia + UA |
| Formato de valores | Como se escribió en CSS | Valores resueltos (`px`, `rgb()`, etc.) |
| Variables CSS (lectura) | Solo si están en el atributo `style` | Sí, con `getPropertyValue("--var")` |
| Pseudoelementos | No | Sí (segundo argumento) |
| Provoca reflow | No | Sí — acceder fuerza un layout |

## Recetas comunes

### Leer dimensiones calculadas

```js
const cs = getComputedStyle(el);
const ancho = parseFloat(cs.width);   // número en px
const alto = parseFloat(cs.height);
// Alternativa más directa para dimensiones: getBoundingClientRect()
```

### Detectar si un elemento está oculto

```js
function estaOculto(el) {
  const cs = getComputedStyle(el);
  return cs.display === "none" || cs.visibility === "hidden" || cs.opacity === "0";
}
```

### Leer una variable CSS en JS para cálculos

```js
const raiz = document.documentElement;
const espaciado = getComputedStyle(raiz).getPropertyValue("--espaciado-base").trim();
// trim() porque el valor puede tener espacios alrededor
const valor = parseFloat(espaciado); // convertir a número si es necesario
```

### Verificar el valor real antes de una animación

```js
// Conocer la posición actual para una transición de 0 a "auto" (no animable directamente)
const alturaActual = getComputedStyle(el).height; // "0px" o "143px"
el.style.height = alturaActual;  // fijar el valor inline antes de animar
el.offsetHeight;                 // forzar reflow para que la transición la detecte
el.style.height = "0px";
```

## Cómo funciona por dentro

`getComputedStyle` fuerza al motor de renderizado a ejecutar un **reflow** (recalculación del layout): el navegador resuelve la cascada CSS completa para el elemento, aplica herencia, calcula los valores relativos (em, %, etc.) en píxeles absolutos y devuelve ese estado consistente.

Este proceso es costoso. Cada llamada a `getComputedStyle` (o al acceso de propiedades de layout como `offsetWidth`, `scrollTop`, `getBoundingClientRect`) invalida el layout cacheado y obliga al navegador a recalcularlo. En un bucle que alterna lecturas y escrituras de estilos, esto genera **thrashing de layout**: decenas o cientos de reflows por frame.

```js
// MAL: lectura y escritura alternadas = reflow en cada iteración
elementos.forEach(el => {
  const alto = getComputedStyle(el).height; // lectura → reflow
  el.style.height = (parseFloat(alto) + 10) + "px"; // escritura → invalida layout
});

// BIEN: leer todo primero, luego escribir
const alturas = elementos.map(el => parseFloat(getComputedStyle(el).height));
elementos.forEach((el, i) => { el.style.height = (alturas[i] + 10) + "px"; });
```

> [!tip]
> Cuando necesites múltiples propiedades del mismo elemento, llama a `getComputedStyle` una vez y guarda la referencia del objeto devuelto. El objeto refleja el estado en el momento de la llamada — acceder a sus propiedades después no provoca reflows adicionales (a menos que hayas modificado el DOM entre medias).

> [!warning]
> `getComputedStyle` siempre provoca un reflow. Nunca lo llames dentro de bucles que también modifiquen estilos o el DOM. Para medir dimensiones y posiciones de forma más eficiente en animaciones, prefiere `getBoundingClientRect()` y lee todo de una vez antes de cualquier escritura.

## Notas relacionadas

- [[01 Propiedad style]]
- [[index|Modificar Estilos — Índice]]
- [[../08 Dimensiones y Posiciones/index|Dimensiones y Posiciones]]
