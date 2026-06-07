---
title: outline — Contorno que no ocupa espacio
aliases:
  - outline
  - focus ring
tags:
  - css
  - api/propiedad
  - fondos
propiedad: outline
grupo: box-model
valor_inicial: none
hereda: false
animable: false
draft: false
---

# outline

> [!definicion]
> `outline` dibuja un **contorno** alrededor de un elemento, **por fuera** del borde. Su rasgo distintivo: **no ocupa espacio** ni afecta al layout (no desplaza otros elementos). Su uso fundamental es el **anillo de foco** que marca el elemento enfocado por teclado.

```css
:focus-visible {
  outline: 2px solid #cba6f7;
  outline-offset: 2px;
}
```

## Sintaxis (como border)

```css
outline: <grosor> <estilo> <color>;
```

| Sub-propiedad | Controla |
|---------------|----------|
| `outline-width` | Grosor |
| `outline-style` | Estilo (`solid`, `dashed`, `dotted`…) |
| `outline-color` | Color (`auto` usa el color del sistema) |
| `outline-offset` | Separación entre el outline y el borde |

## outline vs. border

> [!info] Las diferencias clave
> | | `outline` | `border` |
> |--|-----------|----------|
> | ¿Ocupa espacio? | **No** | Sí |
> | ¿Desplaza el layout? | No | Sí |
> | ¿Sigue `border-radius`? | Hoy sí (antes no) | Sí |
> | ¿Por lado? | No (es uniforme) | Sí |
> | Uso principal | Foco de teclado | Bordes de diseño |
>
> Como no ocupa espacio, aparecer/desaparecer un `outline` **no causa saltos**, a diferencia de un `border`. Por eso es perfecto para el foco.

## El anillo de foco: NO lo elimines

> [!warning] outline: none sin reemplazo rompe la accesibilidad
> El error de accesibilidad más extendido: quitar el outline (`outline: none` o `outline: 0`) porque "se ve feo", dejando la web **inutilizable por teclado** (el usuario no sabe dónde está el foco). Si el outline por defecto no encaja con el diseño, **reemplázalo** por uno propio, nunca lo elimines:
> ```css
> /* ❌ NUNCA esto sin alternativa */
> :focus { outline: none; }
>
> /* ✅ Reemplazar por uno visible */
> :focus-visible {
>   outline: 2px solid #cba6f7;
>   outline-offset: 2px;
> }
> ```

## focus-visible: el foco solo con teclado

> [!tip] Usa :focus-visible, no :focus
> La pseudoclase [[04 focus-visible | `:focus-visible`]] muestra el anillo **solo** cuando el foco viene del teclado, no al hacer clic con el ratón (donde el anillo molesta a algunos). Es la forma moderna de tener un foco accesible sin el "anillo persistente" tras un clic:
> ```css
> :focus-visible { outline: 2px solid #cba6f7; outline-offset: 2px; }
> ```

## outline-offset

`outline-offset` separa el contorno del elemento, dando aire al anillo de foco (más elegante) o creando efectos de doble borde:

```css
.boton:focus-visible { outline: 2px solid currentColor; outline-offset: 4px; }
```

Acepta valores negativos (el outline se dibuja hacia dentro).

## Otros usos

| Uso | Por qué outline |
|-----|------------------|
| Anillo de foco | No desplaza el layout |
| Depuración de layout | `* { outline: 1px solid red; }` sin romper el diseño |
| Doble borde decorativo | `border` + `outline` con offset |

> [!tip] Depurar layouts con outline (no border)
> Para ver las cajas de todos los elementos al depurar, `outline` es mejor que `border` porque **no altera el layout** (un border de 1px en todo descuadraría todo):
> ```css
> * { outline: 1px solid red; }   /* ver cajas sin romper el layout */
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - **Nunca** elimines el outline de foco sin un reemplazo visible.
> - Usa `:focus-visible` para mostrar el anillo solo con teclado.
> - `outline-offset` para separar el anillo y darle aire.
> - Para depurar cajas, `outline` (no `border`) evita descuadrar el layout.
> - Asegura contraste suficiente del anillo de foco (visible sobre el fondo).

## Errores comunes

> [!warning] Trampas
> - **`outline: none`** sin alternativa: inaccesible por teclado.
> - **Usar `:focus`** en vez de `:focus-visible`: el anillo persiste tras clics de ratón.
> - **Anillo de bajo contraste**: invisible sobre ciertos fondos.

## Notas relacionadas

- [[04 focus-visible]] — mostrar el outline solo con teclado.
- [[03 focus]] — el estado de foco en general.
- [[04 Border/index]] — el borde normal, que sí ocupa espacio.
