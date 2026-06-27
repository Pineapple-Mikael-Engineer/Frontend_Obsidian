---
title: alt en Imágenes — Escribir buenos textos alternativos
aliases:
  - alt text
  - texto alternativo
tags:
  - html
  - api/atributo
  - a11y
elemento: img
categoria: incrustado
rol_implicito: ninguno
vacio: false
draft: false
order: 1
---

# alt en Imágenes

> [!definicion]
> El atributo `alt` de [[01 Imagen (img) | `<img>`]] es la **alternativa textual** de una imagen: lo que el lector de pantalla anuncia y lo que se muestra si la imagen no carga. Escribirlo bien —ni de más ni de menos— es una habilidad central de la accesibilidad.

```html
<img src="perro.jpg" alt="Un golden retriever corriendo por la playa" />
```

## La regla por tipo de imagen

| Tipo de imagen | Qué poner en `alt` |
|----------------|--------------------|
| **Informativa** | Describe la información que aporta |
| **Decorativa** | `alt=""` (vacío, pero presente) |
| **Funcional** (enlace/botón) | Describe la **acción o destino** |
| **Compleja** (gráfico, mapa) | Resumen breve + descripción larga aparte |
| **De texto** (una cita en imagen) | El texto exacto que contiene |

## Informativa: el contenido relevante

Describe lo que importa **en contexto**, no cada detalle:

```html
<!-- En un artículo sobre razas de perro -->
<img src="x.jpg" alt="Golden retriever adulto de pelaje dorado, de perfil" />

<!-- La misma foto en una noticia sobre una playa -->
<img src="x.jpg" alt="Perro jugando en una playa concurrida al atardecer" />
```

## Decorativa: alt vacío, no ausente

> [!warning] alt="" vacío ≠ sin alt
> Para una imagen puramente decorativa (un adorno, una textura), el `alt` debe estar **presente pero vacío** (`alt=""`):
> - **`alt=""`** → el lector de pantalla la **ignora** por completo. Correcto.
> - **sin `alt`** → el lector puede leer el **nombre del archivo** (`IMG_2034.jpg`), un ruido inútil. Incorrecto.
>
> El `alt` vacío es una decisión explícita ("esto no aporta información"), no un olvido.

## Funcional: la acción, no la imagen

Si la imagen es un enlace o un botón, el `alt` describe **adónde lleva o qué hace**:

```html
<a href="/"><img src="logo.png" alt="Inicio — Recetas de la Abuela" /></a>
<button><img src="lupa.png" alt="Buscar" /></button>
```

No `alt="logo"` ni `alt="lupa"`: eso describe el dibujo, no la función.

## Lo que no debe hacer un alt

> [!warning] Errores de redacción
> - **Empezar con "Imagen de…"**: el lector ya anuncia "imagen"; es redundante.
> - **Repetir la [[11 Figura (figure, figcaption) | `<figcaption>`]]**: el `alt` describe lo visual, la leyenda da contexto; no se duplican.
> - **Keyword stuffing**: meter palabras clave de SEO en el `alt` perjudica al usuario y a la larga al SEO.
> - **Describir de más**: en una decorativa, cualquier texto es ruido.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pregúntate: *¿qué información perdería alguien que no ve esta imagen?* Eso es el `alt`.
> - Decorativa → `alt=""`. Funcional → la acción. Informativa → el contenido.
> - Sé conciso; describe lo relevante en contexto, no cada píxel.
> - Para gráficos complejos, resumen en `alt` y descripción larga en el texto o `aria-describedby`.

## Errores comunes

> [!warning] Trampas
> - **Sin `alt`** en decorativas: el lector lee el nombre del archivo.
> - **`alt` redundante** ("Imagen de un perro") o que repite la leyenda.
> - **`alt="logo"`** en una imagen funcional en vez de su destino.

## Notas relacionadas

- [[01 Imagen (img)]] — el elemento y su atributo `alt`.
- [[02 Texto para Iconos]] — el caso de los iconos y botones de icono.
- [[11 Figura (figure, figcaption)]] — `alt` vs. `<figcaption>`.
