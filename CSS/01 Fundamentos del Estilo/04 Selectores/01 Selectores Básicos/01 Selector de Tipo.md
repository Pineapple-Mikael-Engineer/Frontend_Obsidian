---
title: Selector de Tipo — Apuntar por nombre de elemento
aliases:
  - type selector
  - element selector
tags:
  - css
  - api/selector
  - arquitectura
selector: tipo
draft: false
---

# Selector de Tipo

> [!definicion]
> El selector de **tipo** (o de elemento) apunta a **todos** los elementos de un tipo dado, usando su nombre de etiqueta sin más. Es el selector de menor especificidad.

```css
p  { line-height: 1.6; }     /* todos los <p> */
h1 { font-size: 2rem; }      /* todos los <h1> */
a  { color: #cba6f7; }       /* todos los <a> */
```

## Cuándo usarlo

| Buen uso | Por qué |
|----------|---------|
| Estilos base de un elemento | Tipografía de `body`, `p`, enlaces, listas |
| Reset / normalización | `* `, `h1`–`h6`, `ul`, etc. |
| Estilos por defecto de un componente | `button`, `input` dentro de un contexto |

El selector de tipo es ideal para los **estilos de base** de un sitio: cómo se ven los párrafos, los títulos y los enlaces por defecto, antes de añadir clases.

## Especificidad baja: ventaja e inconveniente

> [!info] Fácil de sobrescribir (a propósito)
> El selector de tipo tiene **baja especificidad** (cuenta como un "elemento"). Esto es bueno para los estilos base: cualquier clase los sobrescribe sin pelear. Pero también significa que no conviene usarlo para estilos que deban "ganar": una clase siempre los vencerá.
> ```css
> a       { color: blue; }    /* base, especificidad baja */
> .destacado { color: red; }  /* gana fácilmente sobre el tipo */
> ```

## No escala bien para componentes

> [!warning] Estilar por tipo acopla el CSS al HTML
> Usar selectores de tipo para componentes (`div`, `span`, `section`) es frágil: si cambias el elemento HTML, el estilo deja de aplicarse, y afectas a **todos** los elementos de ese tipo, no solo a los que querías. Para componentes, usa [[02 Selector de Clase | clases]]. El tipo es para estilos base y de reset.

## Combinar con clase

Se combina con una clase (sin espacio) para acotar a elementos de un tipo concreto **con** esa clase:

```css
button.primario { background: #cba6f7; }
/* solo los <button> con class="primario" */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa el selector de tipo para **estilos base** (tipografía, enlaces, reset).
> - Para componentes y variantes, usa clases.
> - Aprovecha su baja especificidad: los estilos base no deben "ganar" a las clases.

## Errores comunes

> [!warning] Trampas
> - **Estilar componentes por tipo** (`div`, `span`): frágil y de alcance excesivo.
> - **Esperar que un selector de tipo gane** a una clase: tiene menos especificidad.

## Notas relacionadas

- [[02 Selector de Clase]] — para componentes y variantes.
- [[04 Selector Universal]] — `*`, aún más amplio.
- [[01 Cálculo de Especificidad]] — el peso bajo del tipo.
