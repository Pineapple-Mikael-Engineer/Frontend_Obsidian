---
title: <wbr> — Oportunidad de salto de línea
aliases:
  - wbr
  - word break opportunity
tags:
  - html
  - api/elemento
  - semantica
elemento: wbr
categoria: fraseo
rol_implicito: ninguno
vacio: true
draft: false
order: 25
---

# Ruptura de Palabra (wbr)

> [!definicion]
> `<wbr>` marca un **punto donde el navegador puede romper la línea si hace falta**, dentro de una palabra o cadena muy larga. No fuerza el salto (a diferencia de [[02 Saltos de Línea (br) | `<br>`]]): solo da permiso. Es un elemento void.

```html
<p>https://ejemplo.com/<wbr>ruta/<wbr>muy/<wbr>larga/<wbr>que/<wbr>desborda</p>
```

Si la URL cabe, no se rompe; si desborda el contenedor, el navegador puede partirla en uno de los `<wbr>` en vez de desbordar o partir en un punto feo.

## El problema: cadenas largas sin espacios

URLs, rutas, identificadores o palabras técnicas muy largas sin espacios no tienen dónde romperse y pueden **desbordar** su contenedor (overflow horizontal) o forzar un scroll. `<wbr>` indica puntos de ruptura razonables sin insertar ningún carácter visible.

## wbr vs. br vs. CSS

| Herramienta | Qué hace |
|-------------|----------|
| `<br>` | **Fuerza** un salto de línea siempre |
| `<wbr>` | **Permite** un salto solo si hace falta, en ese punto |
| `overflow-wrap: break-word` (CSS) | Permite romper en cualquier punto, automáticamente |
| `hyphens: auto` (CSS) | Inserta guiones según las reglas del idioma |

## wbr vs. el guión suave

Existe un equivalente como entidad de carácter, el *soft hyphen* `&shy;`, pero con una diferencia clave:

| Mecanismo | Al romper la línea |
|-----------|--------------------|
| `<wbr>` | No añade ningún carácter visible |
| `&shy;` | Añade un **guión** en el punto de ruptura |

`<wbr>` es preferible para URLs y rutas (un guión las haría incorrectas); `&shy;` es para palabras donde el guión de partición es ortográficamente correcto.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<wbr>` para URLs, rutas y cadenas técnicas largas que puedan desbordar.
> - Para texto general, a menudo es más simple `overflow-wrap: break-word` en CSS, que no requiere insertar elementos.
> - No insertes muchos `<wbr>` en texto normal: solo donde una cadena larga lo necesite.

## Errores comunes

> [!warning] Confundir wbr con br
> `<wbr>` **no** salta siempre: solo cuando la línea lo necesita. Quien espera un salto garantizado debe usar `<br>`. Y para controlar el ajuste de líneas en bloques de texto, casi siempre es mejor el CSS (`overflow-wrap`, `word-break`, `hyphens`) que sembrar el HTML de `<wbr>`.

## Notas relacionadas

- [[02 Saltos de Línea (br)]] — el salto **forzado**, frente al opcional de `<wbr>`.
- [[01 Enlaces (a)]] — las URLs largas que suelen necesitar `<wbr>`.
