---
title: Combinador Hijo Directo (>) — Solo hijos inmediatos
aliases:
  - child combinator
tags:
  - css
  - api/selector
  - arquitectura
selector: hijo-directo
draft: false
order: 2
---

# Hijo Directo (>)

> [!definicion]
> El combinador de **hijo directo** (`>`) selecciona elementos que son hijos **inmediatos** de otro: exactamente un nivel de anidación, no nietos ni más profundos. `A > B` significa "todo `B` que sea hijo directo de un `A`".

```css
ul > li { list-style: square; }
/* solo los <li> hijos directos del <ul>, no los de sublistas */
```

```html
<ul>
  <li>Directo</li>            <!-- ✅ -->
  <li>Directo
    <ul><li>Anidado</li></ul>  <!-- ❌ NO afecta (es nieto) -->
  </li>
</ul>
```

## Por qué preferirlo al descendente

> [!tip] Más predecible y acotado
> `>` da control preciso: afecta **solo** al primer nivel. Esto es muy útil cuando hay anidación que no quieres tocar:
> - Estilar los ítems de un menú **sin** afectar a los submenús.
> - Estilar los hijos directos de un grid/flex container, no sus descendientes.
> ```css
> .menu > li { border-bottom: 1px solid; }   /* solo el nivel superior */
> ```
> El descendente (`.menu li`) afectaría también a los submenús anidados.

## Casos típicos

| Caso | Selector |
|------|----------|
| Hijos directos de un contenedor flex/grid | `.contenedor > *` |
| Ítems de primer nivel de un menú | `.nav > li` |
| Párrafos directos de un artículo (no los de citas) | `.articulo > p` |

## Combinar con el universal

`A > *` selecciona **todos** los hijos directos de `A`, útil para layout:

```css
.fila > * { flex: 1; }   /* cada hijo directo crece por igual */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `>` cuando la anidación importe y quieras acotar a un nivel.
> - Ideal para contenedores con descendientes que no deben heredar el estilo.
> - `> *` para aplicar a todos los hijos directos sin nombrarlos.

## Errores comunes

> [!warning] Trampas
> - **Esperar que afecte a nietos**: solo hijos directos.
> - **Usar descendente** (`A B`) cuando querías solo el primer nivel, afectando a anidados.

## Notas relacionadas

- [[01 Descendente]] — a cualquier profundidad (más amplio).
- [[03 Hermano Adyacente (+)]] · [[04 Hermano General (~)]] — relaciones entre hermanos.
- [[index]] — el conjunto de combinadores.
