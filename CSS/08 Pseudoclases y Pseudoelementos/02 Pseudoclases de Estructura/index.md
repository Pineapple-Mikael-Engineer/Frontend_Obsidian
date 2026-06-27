---
title: Pseudoclases de Estructura — Seleccionar por posición
aliases:
  - structural pseudo-classes
tags:
  - css
  - api/concepto
  - selectores
draft: false
order: 2
---

# Pseudoclases de Estructura

> [!definicion]
> Las pseudoclases **estructurales** seleccionan elementos por su **posición** dentro del árbol del DOM: el primero, el último, los pares/impares, el que ocupa cierto lugar. Permiten estilar según la estructura sin añadir clases, algo esencial para listas, tablas y rejillas.

```css
li:first-child { font-weight: bold; }
tr:nth-child(even) { background: #f5f5f5; }   /* filas alternas */
```

## Mapa de la subsección

- [[01 first-child y last-child]] — el primero y el último hijo.
- [[02 first-of-type y last-of-type]] — el primero/último de su tipo.
- [[03 nth-child()]] — el enésimo (con fórmulas).
- [[04 nth-of-type()]] — el enésimo de su tipo.
- [[05 only-child y only-of-type]] — hijo único.
- [[06 empty]] — elementos sin contenido.
- [[07 root]] — la raíz del documento (`:root`).

## child vs. of-type: la distinción clave

> [!warning] :nth-child cuenta TODOS los hermanos; :nth-of-type solo los del mismo tipo
> La confusión más habitual de esta sección. Dadas estas etiquetas:
> ```html
> <div><h2>Título</h2><p>A</p><p>B</p></div>
> ```
> - **`p:first-child`** → **no selecciona nada** (el primer hijo es el `<h2>`, no un `<p>`).
> - **`p:first-of-type`** → selecciona el `<p>A</p>` (el primer `<p>`).
>
> `:nth-child` cuenta **todos** los hijos sin importar su etiqueta; `:nth-of-type` cuenta solo los del **mismo tipo**. Elegir mal es la causa nº 1 de "mi selector no funciona". Detalle en [[01 first-child y last-child]] y [[03 nth-child()]].

## La potencia de las fórmulas

> [!tip] nth-child(an+b) selecciona patrones
> [[03 nth-child() | `:nth-child()`]] acepta **fórmulas** (`2n`, `3n+1`, `odd`, `even`) que seleccionan patrones repetidos: filas alternas, cada tercer elemento, todos menos el primero. Es lo que da a las pseudoclases estructurales su verdadero poder:
> ```css
> li:nth-child(odd) { }        /* impares */
> li:nth-child(3n) { }         /* cada tercero */
> li:nth-child(n+4) { }        /* del cuarto en adelante */
> ```

## Notas relacionadas

- [[03 nth-child()]] — la más potente, con fórmulas.
- [[01 first-child y last-child]] — las más usadas.
- [[04 Pseudoclases Funcionales/index]] — `:not()`, `:is()` para combinar.
