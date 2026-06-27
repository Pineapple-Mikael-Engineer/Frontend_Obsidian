---
title: Combinador Hermano General (~) — Todos los hermanos siguientes
aliases:
  - general sibling combinator
tags:
  - css
  - api/selector
  - arquitectura
selector: hermano-general
draft: false
order: 4
---

# Hermano General (~)

> [!definicion]
> El combinador de **hermano general** (`~`) selecciona **todos** los elementos hermanos que vienen **después** de otro (no necesariamente el inmediato). `A ~ B` significa "todo `B` hermano que aparezca tras un `A`".

```css
h2 ~ p { color: gray; }
/* todos los <p> hermanos que vengan después de un <h2> */
```

```html
<h2>Título</h2>
<p>Afectado</p>      <!-- ✅ -->
<span>x</span>
<p>Afectado</p>      <!-- ✅ aunque no sea el inmediato -->
```

## ~ vs. +

| | `A + B` (adyacente) | `A ~ B` (general) |
|--|---------------------|-------------------|
| Cuántos | Solo el inmediato siguiente | Todos los siguientes |
| Hermanos | Sí | Sí |
| Dirección | Hacia adelante | Hacia adelante |

`+` es un caso particular de `~` restringido al primero. Ambos solo miran **hacia adelante** entre hermanos.

## Casos de uso

| Patrón | Selector |
|--------|----------|
| Atenuar todo lo que sigue a un elemento | `.activo ~ *` |
| Estilar hermanos posteriores a un estado | `input:checked ~ .panel` |
| Resaltar los días posteriores en un calendario | `.hoy ~ .dia` |

## Interacciones sin JS

Combinado con `:checked` o `:hover`, `~` permite que un control afecte a **varios** hermanos posteriores, base de menús, pestañas y galerías "solo CSS":

```css
#tab1:checked ~ .panel-1 { display: block; }
```

## Solo hacia adelante

> [!warning] No selecciona hermanos anteriores
> Como `+`, el combinador `~` solo mira **hacia adelante**: no hay forma con combinadores clásicos de seleccionar hermanos **anteriores**. Si necesitas reaccionar a un elemento que viene **después** afectando a uno anterior, hace falta [[03 has()|`:has()`]] (p. ej. `label:has(~ input:checked)`) o JavaScript.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `~` cuando quieras afectar a varios hermanos posteriores, no solo al inmediato.
> - Combínalo con `:checked`/`:hover` para interacciones sin JS sobre varios elementos.
> - Para afectar hermanos anteriores, recurre a `:has()`.

## Errores comunes

> [!warning] Trampas
> - **Esperar que afecte a hermanos anteriores**: solo posteriores.
> - **Confundir `~` con `+`**: el primero son todos, el segundo solo el inmediato.

## Notas relacionadas

- [[03 Hermano Adyacente (+)]] — solo el inmediato siguiente.
- [[03 has()]] — seleccionar hacia atrás/arriba.
- [[index]] — el conjunto de combinadores.
