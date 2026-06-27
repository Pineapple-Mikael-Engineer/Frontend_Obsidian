---
title: nth-of-type() — El enésimo de su tipo
aliases:
  - ":nth-of-type"
  - nth-of-type
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":nth-of-type"
grupo: pseudoclase
draft: false
order: 4
---

# :nth-of-type()

> [!definicion]
> `:nth-of-type(n)` selecciona elementos según su posición **entre los hermanos del mismo tipo**, con las mismas fórmulas que [[03 nth-child() | `:nth-child()`]] (número, `odd`/`even`, `an+b`). La diferencia: cuenta **solo** los elementos de la misma etiqueta, ignorando los demás.

```css
p:nth-of-type(2) { color: #cba6f7; }   /* el segundo <p>, sin contar otros elementos */
```

## child vs. of-type, de nuevo

> [!info] Cuenta solo los del mismo tipo
> La distinción clave respecto a `:nth-child`, con un ejemplo:
> ```html
> <div>
>   <h2>Título</h2>
>   <p>A</p>     <!-- of-type: 1   |   child: 2 -->
>   <img>
>   <p>B</p>     <!-- of-type: 2   |   child: 4 -->
> </div>
> ```
> - `p:nth-child(2)` → el `<p>A</p>` (es el 2º **hijo**).
> - `p:nth-of-type(2)` → el `<p>B</p>` (es el 2º **`<p>`**).
>
> `:nth-of-type` numera solo los `<p>`, saltándose el `<h2>` y el `<img>`. Es más robusto cuando hay tipos mezclados, porque no depende de qué otros elementos haya entre medias.

## Cuándo preferir of-type

> [!tip] Más predecible con contenido mixto
> `:nth-of-type` es preferible cuando el contenedor tiene elementos **variados** y quieres seleccionar por la posición **de un tipo concreto**:
> ```css
> /* Las imágenes pares de un artículo, sin que los párrafos descuadren la cuenta */
> article img:nth-of-type(even) { float: right; }
>
> /* El primer y el tercer <section> */
> section:nth-of-type(2n+1) { background: #fafafa; }
> ```
> Como no le afectan los elementos de otros tipos intercalados, el resultado es más predecible.

## nth-last-of-type

Igual que [[03 nth-child() | `:nth-last-child`]], existe `:nth-last-of-type()` que cuenta desde el final, pero solo los del mismo tipo:

```css
p:nth-last-of-type(1) { margin-bottom: 0; }   /* el último <p> */
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:nth-of-type` cuando el contenedor mezcle tipos y quieras contar uno concreto.
> - Es más robusto que `:nth-child` ante cambios en el contenido (elementos intercalados).
> - Las fórmulas `an+b` funcionan igual que en `:nth-child`.
> - `:nth-last-of-type` para contar desde el final por tipo.

## Errores comunes

> [!warning] Trampas
> - **Confundir el conteo** con `:nth-child` cuando hay tipos mezclados.
> - **Usar `:nth-child`** y que el resultado cambie al añadir un elemento de otro tipo.

## Notas relacionadas

- [[03 nth-child()]] — la versión que cuenta todos los hermanos.
- [[02 first-of-type y last-of-type]] — los casos primero/último de tipo.
- [[index]] — la distinción child vs. of-type.
