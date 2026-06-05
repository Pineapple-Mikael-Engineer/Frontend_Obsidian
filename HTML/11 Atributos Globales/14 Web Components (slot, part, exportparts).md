---
title: slot, part, exportparts — Atributos de Web Components
aliases:
  - web components attributes
  - slot part
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Web Components (slot, part, exportparts)

> [!definicion]
> Los **Web Components** permiten crear elementos HTML **propios y reutilizables** (`<mi-componente>`) con su lógica y estilo encapsulados. Tres atributos globales sirven de puente entre el componente y el documento que lo usa: `slot` (dónde va el contenido), `part` y `exportparts` (qué se puede estilar desde fuera). El desarrollo completo de los Web Components pertenece al curso de JavaScript.

```html
<tarjeta-producto>
  <span slot="titulo">Camiseta</span>
  <span slot="precio">29,99 €</span>
</tarjeta-producto>
```

## El contexto: Shadow DOM

> [!info] Por qué hacen falta estos atributos
> Un Web Component encapsula su estructura interna en un **Shadow DOM**: un árbol "privado" que el CSS y el JS de fuera no pueden tocar directamente. Esto da aislamiento (los estilos del componente no se filtran ni se rompen), pero crea dos necesidades: **inyectar contenido** desde fuera (`slot`) y **permitir estilar** partes concretas desde fuera (`part`). Estos atributos resuelven esa frontera.

## slot: inyectar contenido

`<slot>` (dentro del componente) define un hueco; el atributo `slot` (en el contenido externo) indica en qué hueco colocarse:

```html
<!-- Plantilla interna del componente -->
<slot name="titulo"></slot>
<slot name="precio"></slot>

<!-- Uso: el contenido se reparte en los slots por nombre -->
<tarjeta-producto>
  <span slot="titulo">Camiseta</span>
  <span slot="precio">29,99 €</span>
</tarjeta-producto>
```

Un `<slot>` sin nombre recibe el contenido que no tiene `slot` asignado (el slot por defecto).

## part y exportparts: estilar desde fuera

El Shadow DOM bloquea el CSS externo, pero el componente puede **exponer** partes estilables con `part`, que se seleccionan desde fuera con el pseudo-elemento `::part()`:

```html
<!-- Dentro del componente -->
<button part="boton-comprar">Comprar</button>
```
```css
/* Desde fuera */
tarjeta-producto::part(boton-comprar) { background: #cba6f7; }
```

`exportparts` re-expone partes de un componente **anidado** hacia un nivel superior, propagando esa capacidad de estilado a través de varias capas de Shadow DOM.

## Pertenece a JavaScript

> [!info] Aquí solo los atributos HTML
> Crear un Web Component (definir la clase, `customElements.define`, el ciclo de vida, el Shadow DOM) es trabajo de **JavaScript**. Esta nota cubre solo los **atributos HTML** que actúan en la frontera (`slot`, `part`, `exportparts`); el desarrollo completo está en el curso de JS.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `slot` con nombre para componentes con varias zonas de contenido.
> - Expón con `part` solo las partes que de verdad quieras que sean estilables desde fuera.
> - Usa `exportparts` para propagar partes de componentes anidados.
> - Aprovecha los Web Components para encapsular UI reutilizable, no para todo.

## Errores comunes

> [!warning] Trampas
> - **Esperar que el CSS externo entre** en el Shadow DOM sin `part`: está aislado.
> - **Contenido sin `slot`** cuando el componente tiene slots con nombre: cae en el slot por defecto (o ninguno).
> - **Sobre-exponer partes**: rompe el encapsulamiento que justifica el componente.

## Notas relacionadas

- [[01 Identificación (id, class)]] — el estilado normal, frente al de `::part()`.
- [[index]] — el resto de atributos globales.
