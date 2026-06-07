---
title: unicode-bidi — Control del algoritmo bidireccional
aliases:
  - unicode-bidi
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: unicode-bidi
grupo: tipografia
valor_inicial: normal
hereda: false
animable: false
draft: false
---

# unicode-bidi

> [!definicion]
> `unicode-bidi` controla cómo el elemento participa en el **algoritmo bidireccional** (bidi) de Unicode, que decide el orden visual del texto cuando se mezclan idiomas de distinta dirección (árabe dentro de español, etc.). Va de la mano de [[02 direction | `direction`]] y rara vez se usa directamente: el HTML lo gestiona mejor.

```css
.aislar { unicode-bidi: isolate; }
```

## Valores

| Valor | Efecto |
|-------|--------|
| `normal` | El elemento sigue el algoritmo bidi normal (por defecto) |
| `embed` | Crea un nivel de incrustación con su `direction` |
| `isolate` | **Aísla** el contenido: su dirección no afecta al exterior |
| `bidi-override` | Fuerza el orden según `direction`, ignorando el bidi |
| `isolate-override` | Combina aislamiento y override |
| `plaintext` | Calcula la dirección por el contenido |

## El problema: texto mixto desordenado

> [!info] Por qué existe el algoritmo bidi
> Cuando se mezclan caracteres LTR y RTL (un nombre árabe en una frase española, un número en texto hebreo), el navegador aplica el **algoritmo bidi de Unicode** para ordenarlos visualmente. Funciona bien en general, pero alrededor de puntuación, números y nombres de origen desconocido puede **reordenar mal** el texto (un paréntesis o un número saltan al lado equivocado). `unicode-bidi` permite ajustar ese comportamiento.

## isolate: el valor útil

> [!tip] isolate aísla contenido de dirección desconocida
> El valor más práctico es `isolate`: **aísla** un fragmento para que su dirección no "contamine" el texto que lo rodea. Es la solución CSS al mismo problema que resuelve el elemento HTML [[24 Texto Bidireccional (bdo, bdi) | `<bdi>`]]: insertar un nombre de usuario o contenido de dirección desconocida sin que desordene los números o etiquetas a su alrededor.

## Mejor resuélvelo en el HTML

> [!warning] Prefiere bdi/bdo y dir="auto"
> Igual que con `direction`, el manejo bidireccional se resuelve **mejor en el HTML** que con `unicode-bidi`:
> - [[24 Texto Bidireccional (bdo, bdi) | `<bdi>`]] aísla automáticamente (equivale a `unicode-bidi: isolate`).
> - `<bdo dir="rtl">` fuerza una dirección (equivale a `bidi-override`).
> - `dir="auto"` detecta la dirección del contenido.
>
> Estos elementos llevan el `unicode-bidi` adecuado incorporado, así que rara vez hay que escribir la propiedad CSS a mano. Conocerla ayuda a entender qué hacen `<bdi>`/`<bdo>` por dentro.

## Buenas prácticas

> [!tip] Recomendaciones
> - Para aislar contenido bidi, usa `<bdi>` (HTML) en lugar de `unicode-bidi: isolate` directo.
> - Para texto de usuario de dirección desconocida, `<bdi>` o `dir="auto"`.
> - Reserva `unicode-bidi` en CSS para casos donde no puedas cambiar el HTML.
> - Combínalo siempre con un `direction` coherente.

## Errores comunes

> [!warning] Trampas
> - **Manejar bidi solo con CSS** cuando `<bdi>`/`<bdo>` sería más limpio.
> - **`bidi-override`** sin necesidad: rompe el orden natural del texto.
> - **Olvidar aislar** contenido de usuario: números y etiquetas saltan de lado.

## Notas relacionadas

- [[24 Texto Bidireccional (bdo, bdi)]] — la solución HTML, preferida.
- [[02 direction]] — la dirección que `unicode-bidi` matiza.
- [[04 Idioma y Dirección (lang, dir)]] — `dir="auto"` para detección automática.
