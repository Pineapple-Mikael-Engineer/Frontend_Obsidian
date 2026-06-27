---
title: font-display — Qué se muestra mientras la fuente carga
aliases:
  - font-display
  - FOUT
  - FOIT
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-display
grupo: tipografia
valor_inicial: auto
hereda: false
animable: false
draft: false
order: 2
---

# font-display

> [!definicion]
> `font-display` controla **qué texto se muestra mientras una fuente web descarga**: si se deja un hueco invisible (FOIT) o se muestra una fuente de respaldo que luego se cambia (FOUT). Es la herramienta clave para que el texto sea legible cuanto antes.

```css
@font-face {
  font-family: "Inter";
  src: url("inter.woff2") format("woff2");
  font-display: swap;   /* muestra respaldo y cambia al cargar */
}
```

## Los dos problemas: FOIT y FOUT

> [!info] Texto invisible vs. salto de fuente
> Mientras la fuente web descarga, hay dos comportamientos posibles:
> - **FOIT** (Flash of Invisible Text): el texto queda **invisible** hasta que la fuente carga. Si la fuente tarda, el usuario ve un hueco en blanco.
> - **FOUT** (Flash of Unstyled Text): se muestra una **fuente de respaldo** del sistema y, al cargar la web, se **cambia** (un pequeño "salto" visual).
>
> `font-display` decide cuál de los dos ocurre y por cuánto tiempo.

## Los valores

| Valor | Periodo de bloqueo | Comportamiento |
|-------|--------------------|----------------|
| `auto` | (lo decide el navegador) | Normalmente como `block` |
| `block` | ~3s invisible | FOIT: oculta el texto un tiempo, luego respaldo |
| `swap` | 0s | FOUT: respaldo inmediato, cambia al cargar |
| `fallback` | ~100ms invisible | Breve FOIT, luego respaldo; si tarda mucho, se queda en respaldo |
| `optional` | ~100ms | Como fallback, pero el navegador puede **no usar** la fuente si la red es lenta |

## swap: el más recomendado en general

> [!tip] swap para que el texto sea legible ya
> `swap` es la opción más común: muestra el texto en la **fuente de respaldo de inmediato** (cero tiempo invisible) y lo cambia a la fuente web cuando carga. El usuario nunca ve un hueco en blanco; el coste es un pequeño salto al cambiar la tipografía. Para contenido de lectura, priorizar la legibilidad inmediata suele valer más que evitar el salto.

## optional: el mejor para rendimiento

> [!tip] optional cuando el rendimiento manda
> `optional` da un margen mínimo y, si la fuente no llega a tiempo (red lenta, primera visita), **usa el respaldo y no cambia**, descargando la fuente para la próxima visita (queda en caché). Elimina el salto por completo a costa de que la primera carga pueda no ver la fuente de marca. Ideal cuando el rendimiento y la estabilidad visual son prioritarios.

## Reducir el salto: ajustar el respaldo

> [!info] Métricas de fuente para un FOUT suave
> El salto de `swap` se minimiza si la fuente de respaldo tiene **métricas parecidas** a la web (tamaño, altura). Propiedades como `size-adjust`, `ascent-override` y `descent-override` (en una `@font-face` de respaldo) permiten ajustar el respaldo para que el cambio sea casi imperceptible.

## Buenas prácticas

> [!tip] Recomendaciones
> - `swap` para texto de lectura: legible de inmediato.
> - `optional` cuando el rendimiento y la estabilidad visual sean prioritarios.
> - Evita `block`/`auto` para texto importante: puede dejar huecos invisibles.
> - Combina con `<link rel="preload">` de la fuente crítica para acortar la espera.

## Errores comunes

> [!warning] Trampas
> - **`block`/`auto`** en texto de lectura: FOIT que deja huecos en blanco.
> - **No declarar `font-display`**: el navegador decide (suele ser FOIT).
> - **Ignorar el salto** de `swap`: ajusta el respaldo si molesta.

## Notas relacionadas

- [[01 src y format]] — la fuente que tarda en cargar.
- [[02 font-family]] — la pila de respaldo que se ve mientras tanto.
- [[index]] — la regla `@font-face`.
