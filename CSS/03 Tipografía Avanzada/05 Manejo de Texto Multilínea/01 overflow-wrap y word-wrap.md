---
title: overflow-wrap y word-wrap — Partir palabras largas que desbordan
aliases:
  - overflow-wrap
  - word-wrap
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: overflow-wrap
grupo: tipografia
valor_inicial: normal
hereda: true
animable: false
draft: false
---

# overflow-wrap y word-wrap

> [!definicion]
> `overflow-wrap` permite **partir una palabra larga** (que normalmente no se rompería) cuando **no cabe** en su contenedor, evitando que desborde. `word-wrap` es el nombre antiguo de la misma propiedad (alias heredado). Es la defensa más común contra URLs y palabras largas que rompen el layout.

```css
.celda { overflow-wrap: break-word; }
```

## Valores

| Valor | Efecto |
|-------|--------|
| `normal` | Solo rompe en espacios/guiones naturales (por defecto) |
| `break-word` | Rompe una palabra **solo si no cabe** de otro modo |
| `anywhere` | Como `break-word`, pero también cuenta para el cálculo de tamaño mínimo |

## El problema que resuelve

> [!info] Una URL larga rompe el contenedor
> Por defecto, el navegador solo rompe líneas en espacios. Una **palabra muy larga sin espacios** (una URL, un email, un hash, un identificador) no tiene dónde romperse, así que **desborda** su contenedor, causando scroll horizontal o cortando contenido:
> ```css
> /* Sin overflow-wrap: "https://ejemplo.com/ruta-muy-larga..." se sale */
> .comentario { overflow-wrap: break-word; }   /* ahora se parte y cabe */
> ```

## overflow-wrap vs. word-break

> [!info] La diferencia de agresividad
> Ambas parten palabras, pero con criterios distintos:
> - **`overflow-wrap: break-word`**: parte una palabra **solo si no cabe de ninguna otra forma** (último recurso). Respeta las palabras mientras quepan.
> - **`word-break: break-all`**: parte **en cualquier carácter** agresivamente, aunque la palabra cupiera. Más drástico.
>
> Para texto normal, `overflow-wrap: break-word` es lo deseable: solo interviene cuando hace falta. Detalle en [[02 word-break | word-break]].

## anywhere vs. break-word

La diferencia sutil entre `break-word` y `anywhere` está en el cálculo del **ancho mínimo** del contenedor:
- `break-word`: el contenedor puede seguir reservando el ancho de la palabra larga para su tamaño mínimo.
- `anywhere`: la palabra larga **no** cuenta para el ancho mínimo, lo que permite que el contenedor se encoja más (útil en flex/grid).

Para la mayoría de casos, `break-word` basta; `anywhere` ayuda en contenedores que deben encoger al máximo.

## La receta defensiva

> [!tip] Aplica overflow-wrap a contenido de usuario
> En cualquier contenedor que muestre **texto de usuario** (comentarios, nombres, mensajes), conviene `overflow-wrap: break-word` de forma preventiva, para que ninguna URL o palabra larga rompa el diseño:
> ```css
> .contenido-usuario { overflow-wrap: break-word; }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - `overflow-wrap: break-word` en contenedores de texto dinámico/usuario.
> - Prefiérelo a `word-break: break-all` para texto normal (menos agresivo).
> - Combínalo con [[03 hyphens | `hyphens: auto`]] para cortes más naturales con guion.
> - Usa `word-wrap` solo si necesitas el nombre heredado por compatibilidad.

## Errores comunes

> [!warning] Trampas
> - **No aplicarlo** a contenido de usuario: URLs y palabras largas desbordan.
> - **Usar `word-break: break-all`** donde `overflow-wrap` bastaría: corta palabras innecesariamente.
> - **Confundir los nombres**: `word-wrap` = `overflow-wrap` (alias).

## Notas relacionadas

- [[02 word-break]] — corte más agresivo, en cualquier carácter.
- [[03 hyphens]] — partir con guion según el idioma.
- [[13 white-space]] — controla si el texto ajusta o no.
