---
title: Selector de ID — Apuntar por atributo id
aliases:
  - id selector
tags:
  - css
  - api/selector
  - arquitectura
selector: id
draft: false
---

# Selector de ID

> [!definicion]
> El selector de **id** apunta al **único** elemento que tiene un `id` concreto, con la sintaxis `#nombre`. Funciona, pero tiene la **especificidad más alta** de los selectores normales, lo que lo hace problemático para estilar y poco recomendable frente a las clases.

```css
#cabecera { background: #1e1e2e; }
```
```html
<header id="cabecera">…</header>
```

## El problema de la especificidad

> [!warning] El id pesa demasiado
> Un `#id` tiene una especificidad muy superior a la de las clases: una regla con id **gana** a casi cualquier regla con clases, por muchas que tengan. Esto desencadena "guerras de especificidad": para sobrescribir un estilo puesto con `#id`, necesitas otro `#id` o, peor, `!important`. Por eso, **para estilar**, las clases son casi siempre mejores.
> ```css
> #menu a   { color: blue; }   /* especificidad alta */
> .menu a   { color: red; }    /* NO gana: la clase pierde frente al id */
> ```

## Para qué sí sirve el id

> [!info] El id es útil, pero no para estilar
> El atributo `id` es valioso para **otras** cosas, no para CSS:
> - **Anclas**: `<a href="#seccion">` salta al `id`.
> - **Asociación de formularios**: `<label for="id">`, `aria-labelledby`.
> - **JavaScript**: `getElementById`, apuntar a un único elemento.
>
> Para estilar ese mismo elemento, suele preferirse darle también una clase y usar la clase en el CSS.

## id vs. class

| | `#id` | `.clase` |
|--|-------|----------|
| Unicidad | Único por página | Reutilizable |
| Especificidad | Alta (problemática) | Media (manejable) |
| Recomendado para estilar | No | Sí |
| Recomendado para anclas/JS/labels | Sí | — |

## Buenas prácticas

> [!tip] Recomendaciones
> - **Estila con clases**, no con `#id`.
> - Reserva el `id` para anclas, asociación de etiquetas y JS.
> - Si necesitas estilar un elemento único, dale una clase y úsala en el CSS.
> - Si usas `#id` en CSS, sé consciente de la especificidad que introduces.

## Errores comunes

> [!warning] Trampas
> - **Estilar por `#id`** y luego no poder sobrescribirlo sin `!important`.
> - **`id` duplicado**: inválido; el selector apunta a varios y rompe JS/anclas.

## Notas relacionadas

- [[02 Selector de Clase]] — la alternativa recomendada para estilar.
- [[01 Cálculo de Especificidad]] — por qué el id pesa tanto.
- [[01 Identificación (id, class)]] — `id`/`class` desde HTML.
