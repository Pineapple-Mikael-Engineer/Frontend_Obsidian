---
title: background-image — Imágenes y gradientes de fondo
aliases:
  - background-image
tags:
  - css
  - api/propiedad
  - fondos
propiedad: background-image
grupo: fondo
valor_inicial: none
hereda: false
animable: false
draft: false
---

# background-image

> [!definicion]
> `background-image` coloca una o varias **imágenes** de fondo en una caja. La "imagen" puede ser un archivo (`url()`), un **gradiente** (que CSS trata como imagen) o un patrón generado. Es la base de fondos decorativos, texturas y overlays.

```css
.hero  { background-image: url("foto.jpg"); }
.degradado { background-image: linear-gradient(135deg, #cba6f7, #89dceb); }
```

## Tipos de valor

| Valor | Qué es |
|-------|--------|
| `url("ruta")` | Un archivo de imagen |
| `linear-gradient(...)` | Un [[01 linear-gradient | gradiente lineal]] |
| `radial-gradient(...)` | Un [[02 radial-gradient | gradiente radial]] |
| `conic-gradient(...)` | Un [[03 conic-gradient | gradiente cónico]] |
| `image-set(...)` | Varias resoluciones (responsive) |
| `none` | Sin imagen (por defecto) |

## background-image vs. <img>

> [!info] Cuándo fondo y cuándo img
> Una imagen de **contenido** (una foto del producto, una ilustración con significado) va en un [[01 Imagen (img) | `<img>`]] con su `alt`, accesible. Una imagen **decorativa** (una textura, un patrón, un fondo de hero) va en `background-image`, porque:
> - No necesita texto alternativo (es decorativa).
> - Se controla con `cover`/`position`/`repeat` (imposible con `<img>` sin CSS extra).
> - No se imprime por defecto.
>
> Regla: ¿la imagen **informa**? → `<img>`. ¿Solo **decora**? → `background-image`.

## Ejemplos de uso

```css
/* Hero a pantalla completa con overlay oscuro */
.hero {
  background-image:
    linear-gradient(rgb(0 0 0 / 0.4), rgb(0 0 0 / 0.4)),  /* overlay */
    url("paisaje.jpg");                                    /* foto debajo */
  background-size: cover;
  background-position: center;
}

/* Patrón de puntos generado (sin archivo) */
.puntos {
  background-image: radial-gradient(#cba6f7 1px, transparent 1px);
  background-size: 20px 20px;
}

/* Icono de fondo en un input de búsqueda */
.buscador {
  background-image: url("lupa.svg");
  background-repeat: no-repeat;
  background-position: 0.75rem center;
  padding-left: 2.5rem;
}
```

## Gradiente sobre imagen (overlay)

> [!tip] El truco del overlay para texto legible
> Para poner texto sobre una foto manteniéndolo legible, se apila un gradiente semitransparente **encima** de la imagen (la primera capa va arriba):
> ```css
> background-image:
>   linear-gradient(rgb(0 0 0 / 0.5), rgb(0 0 0 / 0.5)),   /* oscurece */
>   url("foto.jpg");
> ```
> El gradiente uniforme oscurece la foto, garantizando contraste para el texto blanco encima. Detalle en [[09 Múltiples Fondos]].

## Optimización

> [!warning] El fondo no tiene lazy-loading nativo
> A diferencia de `<img loading="lazy">`, una `background-image` se descarga según las reglas CSS y **no** tiene carga diferida nativa fácil. Para fondos pesados, conviene: usar formatos modernos (WebP/AVIF vía `image-set()`), un color de respaldo, y para imágenes muy grandes valorar `<img>` con `object-fit: cover` (que sí soporta `lazy`).

## Buenas prácticas

> [!tip] Recomendaciones
> - `background-image` para imágenes **decorativas**; `<img>` para las informativas.
> - Combina con `background-size: cover` + `position: center` + `no-repeat` para fondos que llenan.
> - Usa gradientes como overlay para texto legible sobre fotos.
> - Da un `background-color` de respaldo y usa formatos modernos.

## Errores comunes

> [!warning] Trampas
> - **Imagen informativa como fondo**: inaccesible (sin `alt`).
> - **Olvidar `no-repeat`**: la imagen se repite en mosaico.
> - **Sin `background-size`**: la imagen se muestra a su tamaño real, recortada o pequeña.

## Notas relacionadas

- [[05 background-size]] — `cover`/`contain` para ajustar la imagen.
- [[09 Múltiples Fondos]] — apilar overlay + imagen.
- [[02 Gradientes/index]] — gradientes como imagen de fondo.
