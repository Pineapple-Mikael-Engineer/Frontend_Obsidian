---
title: classList — API para manipular clases CSS individualmente
aliases:
  - classList
  - DOMTokenList
  - Element.classList
tags:
  - javascript
  - api/metodo
  - dom
objeto: Element
tipo: metodo
retorna: DOMTokenList
muta: true
asincrono: false
draft: false
---

# classList

> [!definicion]
> `elemento.classList` devuelve un `DOMTokenList` live vinculado al atributo `class` del elemento. Expone métodos para añadir, eliminar, alternar y comprobar clases individuales sin necesidad de manipular el string completo. Es la API moderna para gestión de clases; reemplaza el patrón manual con `className`.

```js
const card = document.querySelector(".card");

card.classList.add("activo");           // añade una clase
card.classList.remove("borrador");      // elimina una clase
card.classList.toggle("destacado");     // alterna — añade si no está, elimina si está
console.log(card.classList.contains("activo")); // → true
```

## Métodos principales

### add y remove

```js
// Añadir una o varias clases — ignora duplicados
el.classList.add("visible");
el.classList.add("tema-oscuro", "animado", "visible"); // varias a la vez; "visible" se ignora

// Eliminar — no lanza error si la clase no existe
el.classList.remove("oculto");
el.classList.remove("tema-oscuro", "animado"); // varias a la vez
```

### toggle

```js
// Sin segundo argumento — añade si no está, elimina si está
const quedoAnadida = el.classList.toggle("activo"); // → true si se añadió, false si se eliminó

// Con segundo argumento booleano — fuerza el estado
el.classList.toggle("activo", true);              // siempre añade (como add)
el.classList.toggle("activo", false);             // siempre elimina (como remove)
el.classList.toggle("oscuro", preferenciaDark);   // estado controlado por variable
```

### contains y replace

```js
// Comprobar presencia
if (el.classList.contains("modal-abierto")) {
  cerrarModal();
}

// Reemplazar una clase por otra — devuelve true si la clase original existía
const exito = el.classList.replace("estado-cargando", "estado-listo");
console.log(exito); // → false si "estado-cargando" no estaba presente
```

## Recetas comunes

```js
// Toggle de tema oscuro
document.querySelector("#toggle-tema").addEventListener("click", () => {
  document.body.classList.toggle("dark-mode");
});

// Activar un tab: desactivar todos, activar el seleccionado
function activarTab(tabSeleccionado) {
  document.querySelectorAll(".tab").forEach(t => t.classList.remove("activo"));
  tabSeleccionado.classList.add("activo");
}

// Mostrar/ocultar loader
function setLoading(estado) {
  document.querySelector(".spinner").classList.toggle("visible", estado);
  document.querySelector("form").classList.toggle("deshabilitado", estado);
}

// Comprobar múltiples clases
["error", "advertencia"].some(c => el.classList.contains(c));
```

## Iteración

`DOMTokenList` es iterable:

```js
// for...of
for (const clase of el.classList) {
  console.log(clase);
}

// forEach
el.classList.forEach((clase, indice) => {
  console.log(indice, clase);
});

// Convertir a array
const clases = [...el.classList];
// → ["card", "activo", "destacado"]

// Número de clases
console.log(el.classList.length); // → 3

// Acceso por índice
console.log(el.classList.item(0)); // → "card"
console.log(el.classList[1]);      // → "activo"
```

## Cómo funciona por dentro

`DOMTokenList` es una vista live sobre el atributo `class` del elemento. Cualquier cambio a través de `classList` se refleja inmediatamente en `el.className` y viceversa:

```js
el.className = "a b c";
console.log(el.classList.length); // → 3

el.classList.add("d");
console.log(el.className); // → "a b c d"

el.className = "x y";
console.log([...el.classList]); // → ["x", "y"]
```

El `DOMTokenList` gestiona duplicados automáticamente — `add` no añade una clase que ya existe; `className = "a a b"` puede crear duplicados en el string pero `classList.add("a")` los evita.

> [!warning]
> `classList.toggle(clase, condicion)` no está disponible en IE11 (solo en IE11+ con polyfill). En código que deba soportar navegadores muy antiguos, usar la forma sin segundo argumento y controlar el estado manualmente. En navegadores modernos, el segundo argumento es parte del estándar y fiable.

> [!tip]
> Para aplicar o quitar un conjunto de clases según un estado booleano, `toggle` con segundo argumento es más limpio que un bloque `if/else` con `add`/`remove`.

## Notas relacionadas

- [[03 Atributos Directos]]
- [[05 dataset]]
- [[05 Modificar Estilos/index | Modificar Estilos]]
- [[04 Modificar Atributos/index | Modificar Atributos]]
