---
title: "Propiedad style"
aliases:
  - element.style
  - CSSStyleDeclaration inline
  - estilos inline JS
tags:
  - javascript
  - api/propiedad
  - dom
draft: false
order: 1
---

# Propiedad `style`

> [!definicion]
> `element.style` es un objeto `CSSStyleDeclaration` que representa los **estilos inline** del elemento — los que se escribirían en el atributo `style=""` del HTML. Permite leer y escribir propiedades CSS directamente desde JavaScript, pero solo refleja los estilos inline: no ve los estilos que vienen de clases, hojas de estilo externas o herencia.

```js
const el = document.querySelector(".caja");

// Escritura directa
el.style.backgroundColor = "steelblue";
el.style.marginTop = "16px";
el.style.transform = "translateX(100px)";

// Lectura (solo devuelve valor si hay estilo inline)
console.log(el.style.backgroundColor); // "steelblue"
console.log(el.style.color);           // "" — si el color viene de una clase CSS
```

## Nombres de propiedades: camelCase

Las propiedades CSS con guión se traducen a camelCase. La relación es mecánica: eliminar los guiones y capitalizar la letra siguiente.

| CSS | JavaScript |
|---|---|
| `background-color` | `backgroundColor` |
| `margin-top` | `marginTop` |
| `border-radius` | `borderRadius` |
| `z-index` | `zIndex` |
| `font-size` | `fontSize` |
| `-webkit-transform` | `webkitTransform` |

Los prefijos de vendor también siguen la misma regla. En la práctica, los prefijos son poco necesarios en navegadores modernos.

## Métodos de `CSSStyleDeclaration`

Además del acceso directo por propiedad, el objeto expone métodos que permiten trabajar con nombres CSS en su forma original (con guiones) y con variables CSS custom properties.

### `setProperty(nombre, valor, prioridad?)`

La única forma de establecer una **variable CSS** desde JavaScript. También sirve para cualquier propiedad usando el nombre CSS con guiones.

```js
// Variable CSS custom property
el.style.setProperty("--color-primario", "#3b82f6");
el.style.setProperty("--espaciado", "1.5rem");

// Propiedad normal con nombre CSS
el.style.setProperty("background-color", "coral");

// Con !important
el.style.setProperty("color", "red", "important");
```

### `getPropertyValue(nombre)`

Lee el valor de una propiedad inline, usando el nombre CSS con guiones. Es la única forma de leer una variable CSS desde `style` (los estilos heredados o de clases no aparecen aquí — para eso está `getComputedStyle`).

```js
const color = el.style.getPropertyValue("background-color"); // "coral"
const variable = el.style.getPropertyValue("--color-primario"); // "#3b82f6"
```

### `removeProperty(nombre)`

Elimina un estilo inline. Devuelve el valor que tenía.

```js
el.style.removeProperty("background-color");
el.style.removeProperty("--color-primario");
```

## Recetas comunes

### Establecer múltiples estilos de una vez

```js
Object.assign(el.style, {
  color: "white",
  backgroundColor: "steelblue",
  padding: "8px 16px",
  borderRadius: "4px",
});
```

`Object.assign` es más limpio que repetir `el.style.propiedad = valor` cuando hay varios cambios. No reemplaza el atributo `style` completo — fusiona propiedad por propiedad.

### Alternar visibilidad con `style`

```js
el.style.display = el.style.display === "none" ? "" : "none";
// Asignar "" restaura el valor de la hoja de estilo (elimina el estilo inline)
```

Asignar `""` (cadena vacía) a una propiedad la elimina del atributo `style`, devolviendo el control a la cascada CSS.

### Modificar transformaciones para animación simple

```js
let x = 0;
function animar() {
  x += 2;
  el.style.transform = `translateX(${x}px)`;
  if (x < 200) requestAnimationFrame(animar);
}
requestAnimationFrame(animar);
```

Para animaciones complejas o de rendimiento crítico, es preferible usar CSS transitions/animations y solo agregar/quitar clases desde JS — el navegador puede optimizarlas en el compositor sin pasar por el hilo principal.

### Variables CSS y tematización dinámica

```js
// Cambiar el tema de toda la página
document.documentElement.style.setProperty("--color-acento", "#f59e0b");
document.documentElement.style.setProperty("--fuente-base", "1.125rem");
```

Modificar variables en `:root` afecta a todos los elementos que las usan — es la forma más eficiente de tematización dinámica desde JS.

## Cómo funciona por dentro

`element.style` es un objeto `CSSStyleDeclaration` que actúa como espejo bidireccional del atributo `style` del elemento. Cuando se escribe `el.style.color = "red"`, el motor:

1. Actualiza el objeto `CSSStyleDeclaration` en memoria.
2. Serializa el resultado al atributo `style` del elemento en el DOM (`style="color: red;"`).
3. Marca el elemento para recálculo de estilos (*style recalculation*).
4. En el próximo frame, el motor de layout recalcula los estilos en cascada, dando prioridad al estilo inline (especificidad máxima salvo `!important` en hoja de estilo).

El estilo inline siempre gana en especificidad sobre cualquier regla de clase o selector (a menos que la regla tenga `!important`). Esto hace que `element.style` sea una herramienta de fuerza bruta — conveniente para cambios dinámicos, pero que puede dificultar el mantenimiento si se usa en exceso.

> [!tip]
> Prefiere agregar/quitar clases CSS con `classList` para la mayoría de los cambios de apariencia. Reserva `element.style` para valores verdaderamente dinámicos que no se pueden prever en CSS (posiciones calculadas, colores generados, variables custom properties).

> [!warning]
> Leer `element.style.propiedad` cuando el estilo viene de una clase CSS o hoja de estilo siempre devuelve `""`. Para leer el valor visual real del elemento — el que el usuario ve — usa `window.getComputedStyle(el)`.

## Notas relacionadas

- [[02 getComputedStyle]]
- [[index|Modificar Estilos — Índice]]
- [[../04 Modificar Atributos/01 getAttribute y setAttribute|getAttribute y setAttribute]]
