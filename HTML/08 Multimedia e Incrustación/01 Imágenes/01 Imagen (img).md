---
title: <img> — Insertar una imagen
aliases:
  - img element
  - image
tags:
  - html
  - api/elemento
  - multimedia
elemento: img
categoria: incrustado
rol_implicito: img
vacio: true
draft: false
---

# Imagen (img)

> [!definicion]
> `<img>` inserta una imagen en el documento. Es un elemento void cuyos dos atributos esenciales son `src` (la ruta de la imagen) y `alt` (su descripción textual). El `alt` no es opcional: es la base de la accesibilidad de la imagen.

```html
<img src="/img/atardecer.jpg" alt="Atardecer naranja sobre el mar en calma"
     width="800" height="600" loading="lazy" />
```

## Atributos

| Atributo | Descripción |
|----------|-------------|
| `src` | Ruta de la imagen (obligatorio) |
| `alt` | Texto alternativo (obligatorio; vacío `alt=""` si es decorativa) |
| `width` / `height` | Dimensiones en píxeles (reservan espacio, evitan saltos) |
| `loading` | `lazy` (diferida) o `eager` (inmediata) |
| `decoding` | `async`, `sync`, `auto` (cómo se decodifica) |
| `srcset` | Varias resoluciones para pantallas distintas |
| `sizes` | Qué tamaño ocupará la imagen según el viewport |

## El atributo alt en profundidad

> [!info] Cómo escribir un buen alt
> El `alt` describe la imagen para quien no la ve (lectores de pantalla) y se muestra si la imagen no carga. Reglas:
> - **Imagen informativa**: describe su contenido y función. `alt="Gráfico: las ventas crecen cada trimestre"`, no `alt="gráfico"`.
> - **Imagen decorativa** (un adorno sin información): `alt=""` **vacío pero presente**, para que el lector la ignore en vez de leer el nombre del archivo.
> - **Imagen que es un enlace/botón**: el `alt` describe el **destino o la acción**, no la imagen.
> - No empieces con "Imagen de…": el lector ya anuncia que es una imagen.

| Tipo de imagen | `alt` |
|----------------|-------|
| Informativa | Descripción del contenido |
| Decorativa | `alt=""` (vacío) |
| Enlace/botón | Destino o acción |
| Compleja (gráfico) | Resumen + descripción larga aparte |

## Dimensiones explícitas: evitar el salto de layout

> [!warning] Pon siempre width y height
> Sin `width`/`height`, el navegador no sabe cuánto espacio reservar hasta que la imagen carga, y el contenido **salta** al aparecer (mal CLS, *Cumulative Layout Shift*). Indicar las dimensiones —aunque luego CSS las ajuste— reserva el hueco con la proporción correcta:
> ```html
> <img src="foto.jpg" alt="…" width="800" height="600" />
> ```
> ```css
> img { max-width: 100%; height: auto; }   /* responsivo manteniendo la proporción */
> ```

## Carga diferida (lazy loading)

`loading="lazy"` retrasa la descarga de imágenes fuera de la pantalla hasta que el usuario se acerca a ellas, ahorrando ancho de banda y acelerando la carga inicial:

```html
<img src="abajo.jpg" alt="…" loading="lazy" />
```

> [!tip] lazy sí, pero no en lo visible
> Aplica `loading="lazy"` a imágenes **bajo el pliegue** (las que requieren scroll). Para la imagen principal visible al cargar (hero), usa `loading="eager"` (o nada): diferir lo visible retrasa el primer pintado.

## Imágenes responsivas (resumen)

Para servir distinta resolución según la pantalla se usan `srcset` y `sizes`, o el elemento [[02 Imágenes Responsivas (picture, source) | `<picture>`]]:

```html
<img src="foto-800.jpg"
     srcset="foto-400.jpg 400w, foto-800.jpg 800w, foto-1600.jpg 1600w"
     sizes="(max-width: 600px) 100vw, 800px"
     alt="…" />
```

El navegador elige el archivo más adecuado al ancho real y la densidad de pantalla.

## Buenas prácticas

> [!tip] Recomendaciones
> - `alt` **siempre**: descriptivo si informa, vacío (`alt=""`) si decora.
> - `width`/`height` explícitos para evitar saltos de layout.
> - `loading="lazy"` en imágenes bajo el pliegue; `eager` en la principal.
> - Usa `srcset`/`sizes` o `<picture>` para servir el peso adecuado a cada pantalla.
> - Elige el [[04 Formatos de Imagen | formato]] óptimo (WebP/AVIF para fotos).

## Errores comunes

> [!warning] Trampas
> - **Sin `alt`**: imagen inaccesible; con `alt` ausente, el lector puede leer el nombre del archivo.
> - **`alt` que empieza por "imagen de"** o repite la `<figcaption>`.
> - **Sin dimensiones**: el contenido salta al cargar (mal CLS).
> - **`lazy` en la imagen principal**: retrasa el primer pintado.

## Notas relacionadas

- [[02 Imágenes Responsivas (picture, source)]] — servir distintas versiones.
- [[04 Formatos de Imagen]] — elegir el formato óptimo.
- [[11 Figura (figure, figcaption)]] — envolver la imagen con leyenda.
- [[01 alt en Imágenes]] — el `alt` desde la accesibilidad (A11Y).
