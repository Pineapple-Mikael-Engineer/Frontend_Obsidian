---
title: <map> y <area> — Mapas de imagen
aliases:
  - image map
  - map area
tags:
  - html
  - api/elemento
  - multimedia
elemento: map
categoria: incrustado
rol_implicito: ninguno
vacio: false
draft: false
order: 3
---

# Mapa de Imagen (map, area)

> [!definicion]
> Un mapa de imagen define **zonas clicables** con distintas formas dentro de una sola imagen, cada una enlazando a un destino distinto. Lo forman `<map>` (el conjunto de zonas) y varios `<area>` (cada región), asociados a un [[01 Imagen (img) | `<img>`]] mediante el atributo `usemap`.

```html
<img src="mapa-europa.png" alt="Mapa de Europa" usemap="#paises" />
<map name="paises">
  <area shape="rect" coords="34,44,90,80" href="/es" alt="España" />
  <area shape="circle" coords="180,90,30" href="/fr" alt="Francia" />
</map>
```

## La conexión img ↔ map

El `<img>` referencia el mapa con `usemap="#nombre"`, y el `<map>` lleva ese `name`:

```html
<img usemap="#zonas" … />
<map name="zonas"> … </map>
```

## Atributos de area

| Atributo | Descripción |
|----------|-------------|
| `shape` | Forma de la zona: `rect`, `circle`, `poly`, `default` |
| `coords` | Coordenadas que definen la forma (en píxeles) |
| `href` | Destino del enlace de esa zona |
| `alt` | Texto alternativo de la zona (**obligatorio** si tiene `href`) |

### Formas y sus coordenadas

| `shape` | `coords` |
|---------|----------|
| `rect` | `x1,y1,x2,y2` (esquinas opuestas) |
| `circle` | `cx,cy,radio` |
| `poly` | `x1,y1,x2,y2,…` (vértices del polígono) |
| `default` | Toda la imagen restante |

## Limitaciones y alternativas modernas

> [!warning] Los mapas de imagen son frágiles
> Los image maps tienen problemas serios:
> - Las `coords` son **píxeles fijos**: no se adaptan si la imagen se redimensiona (no responsivo).
> - Difíciles de mantener y de hacer accesibles.
>
> Por eso hoy rara vez se usan. Las alternativas modernas son mejores: **SVG** con enlaces (`<a>` dentro del [[02 Gráficos Vectoriales (svg) | SVG]]) para zonas clicables escalables, o superponer elementos posicionados con CSS sobre la imagen.

## Accesibilidad

Cada `<area>` con `href` necesita un `alt` que describa su destino, igual que un enlace. Sin él, las zonas son inaccesibles para los lectores de pantalla.

## Buenas prácticas

> [!tip] Recomendaciones
> - Para zonas clicables nuevas, prefiere SVG con enlaces (escala y es accesible) o capas CSS.
> - Si usas image map, pon `alt` en cada `<area>` con destino.
> - Recuerda que las `coords` no se reescalan: el mapa rompe en diseño responsivo.

## Errores comunes

> [!warning] Trampas
> - **Usarlo en diseño responsivo**: las coordenadas fijas no se adaptan.
> - **`<area>` sin `alt`**: zonas inaccesibles.
> - **Coordenadas desfasadas** tras cambiar el tamaño de la imagen.

## Notas relacionadas

- [[01 Imagen (img)]] — la imagen que el mapa hace clicable.
- [[02 Gráficos Vectoriales (svg)]] — la alternativa moderna para zonas clicables.
- [[19 input image]] — comparte el mecanismo histórico de coordenadas.
