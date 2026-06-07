---
title: read-only y read-write — Solo lectura vs. editable
aliases:
  - ":read-only"
  - ":read-write"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":read-only"
grupo: pseudoclase
draft: false
---

# :read-only y :read-write

> [!definicion]
> `:read-only` selecciona campos que **no se pueden editar** pero sí enfocar y enviar (con el atributo `readonly`). `:read-write` selecciona los editables. A diferencia de `:disabled`, un campo `readonly` participa en el envío del formulario.

```css
input:read-only { background: #f5f5f5; border-style: dashed; }
```

## readonly vs. disabled

> [!info] La diferencia que importa al enviar
> | | `:read-only` (readonly) | `:disabled` (disabled) |
> |--|------------------------|------------------------|
> | ¿Se edita? | No | No |
> | ¿Se enfoca? | **Sí** | No |
> | ¿Se envía con el formulario? | **Sí** | No |
> | ¿Se puede seleccionar/copiar el texto? | Sí | A menudo no |
>
> Usa `readonly` para mostrar un valor que el usuario **no debe cambiar pero sí enviar** (un total calculado, un código generado); `disabled` para un campo que **no aplica** en ese momento.

## Estilar campos de solo lectura

```css
/* Un campo readonly se ve "fijo" pero no apagado */
input:read-only {
  background: #f0f0f0;
  border-style: dashed;
  color: #555;
  cursor: default;
}
```

A diferencia de `:disabled`, no conviene atenuarlo tanto: el valor es relevante (se va a enviar) y debe leerse bien.

## read-write: lo editable

`:read-write` selecciona los campos editables (la mayoría). También aplica a elementos con `contenteditable`:

```css
/* Resaltar las zonas editables */
[contenteditable]:read-write:focus { outline: 2px solid #cba6f7; }
```

## Casos de uso

```css
/* Campo de resultado calculado: visible pero no editable */
.total:read-only { font-weight: bold; background: transparent; border: none; }

/* Mostrar visualmente qué campos puede tocar el usuario */
input:read-write { background: white; }
input:read-only { background: #f5f5f5; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `readonly` para valores que el usuario no edita pero deben enviarse.
> - Estila los campos `:read-only` como "fijos" pero **legibles** (no tan apagados como disabled).
> - Distingue visualmente lo editable (`:read-write`) de lo fijo (`:read-only`).
> - `:read-write` también aplica a elementos `contenteditable`.

## Errores comunes

> [!warning] Trampas
> - **Usar `disabled`** cuando el valor debe enviarse: usa `readonly`.
> - **Atenuar `readonly` como si fuera `disabled`**: su valor es relevante.
> - **Olvidar que `readonly` sí se enfoca**: gestiónalo en la navegación por teclado.

## Notas relacionadas

- [[02 disabled y enabled]] — la alternativa que no se envía.
- [[index]] — los estados de formulario.
- [[06 Personalización de Controles (accent-color, appearance)]] — estilizar campos.
