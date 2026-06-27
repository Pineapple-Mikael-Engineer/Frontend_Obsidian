---
title: background-repeat — Repetición de la imagen de fondo
aliases:
  - background-repeat
tags:
  - css
  - api/propiedad
  - fondos
propiedad: background-repeat
grupo: fondo
valor_inicial: repeat
hereda: false
animable: false
draft: false
order: 3
---

# background-repeat

> [!definicion]
> `background-repeat` controla si la imagen de fondo se **repite** para llenar la caja (mosaico) y cómo. Por defecto repite en ambos ejes (`repeat`), lo que es ideal para patrones pero indeseable para una foto única.

```css
.patron { background-repeat: repeat; }     /* mosaico (por defecto) */
.hero   { background-repeat: no-repeat; }  /* una sola vez */
```

## Valores

| Valor | Efecto |
|-------|--------|
| `repeat` | Repite en horizontal y vertical (por defecto) |
| `no-repeat` | No repite: una sola imagen |
| `repeat-x` | Repite solo en horizontal |
| `repeat-y` | Repite solo en vertical |
| `space` | Repite sin recortar, repartiendo el hueco entre copias |
| `round` | Repite escalando las copias para que quepan enteras |

## El valor por defecto sorprende

> [!warning] Sin no-repeat, la foto se vuelve mosaico
> El valor inicial es `repeat`, pensado para patrones. Por eso, al poner una **foto** de fondo sin especificar, se ve **repetida en mosaico**, casi nunca lo deseado:
> ```css
> .hero { background-image: url("foto.jpg"); }   /* ❌ se repite en mosaico */
> .hero { background-image: url("foto.jpg"); background-repeat: no-repeat; }  /* ✅ */
> ```
> `no-repeat` es casi obligatorio para imágenes únicas (lo incluye la receta `cover`).

## space y round: repetición sin cortes

> [!info] Dos formas de repetir sin recortar
> `repeat` puede **cortar** la última copia en el borde. Para evitarlo:
> - **`space`**: coloca tantas copias enteras como quepan y **reparte el espacio sobrante** entre ellas (huecos uniformes).
> - **`round`**: **escala** las copias ligeramente para que quepa un número entero sin huecos (deforma un poco).
>
> Útiles para patrones de iconos o texturas donde no quieres copias cortadas.

## Ejemplos de uso

```css
/* Patrón de textura que llena el fondo */
.textura { background-image: url("ruido.png"); background-repeat: repeat; }

/* Banda decorativa horizontal arriba */
.banda { background-image: url("ola.svg"); background-repeat: repeat-x; }

/* Foto de hero, una sola vez */
.hero { background-repeat: no-repeat; background-size: cover; }

/* Iconos repartidos sin cortar */
.galeria { background-image: url("estrella.svg"); background-repeat: space; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon `no-repeat` siempre que el fondo sea una imagen única (foto, hero).
> - Deja `repeat` para patrones y texturas pensados para repetirse (que "casen" en los bordes, *seamless*).
> - Usa `space`/`round` para repetir sin copias cortadas.
> - Para patrones, combina con [[04 background-position | `background-position`]] y `background-size` para controlar la cuadrícula.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `no-repeat`** en una foto: aparece en mosaico.
> - **Patrón que no casa** en los bordes: se nota la "costura" al repetir.
> - **Esperar que `space`/`round` funcionen** sin un `background-size` que deje sitio para repetir.

## Notas relacionadas

- [[04 background-position]] — dónde empieza la repetición.
- [[05 background-size]] — el tamaño de cada copia.
- [[08 Shorthand background]] — declarar la repetición junto al resto.
