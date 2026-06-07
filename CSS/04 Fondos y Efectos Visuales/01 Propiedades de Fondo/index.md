---
title: Propiedades de Fondo — Color, imagen y su colocación
aliases:
  - background properties
tags:
  - css
  - api/concepto
  - fondos
draft: false
---

# Propiedades de Fondo

> [!definicion]
> El **fondo** de una caja se controla con una familia de propiedades `background-*`: el **color**, una o varias **imágenes**, cómo se **repiten**, dónde se **posicionan**, a qué **tamaño**, si se fijan al hacer scroll, y hasta dónde se extienden. El atajo `background` las agrupa.

```css
.hero {
  background-color: #1e1e2e;
  background-image: url("foto.jpg");
  background-size: cover;
  background-position: center;
}
```

## Mapa de la subsección

- [[01 background-color]] — el color de fondo.
- [[02 background-image]] — imágenes y gradientes de fondo.
- [[03 background-repeat]] — repetir, no repetir, patrones (`space`, `round`).
- [[04 background-position]] — dónde se ancla la imagen.
- [[05 background-size]] — `cover`, `contain`, tamaños concretos.
- [[06 background-attachment]] — fija o desplazable con el scroll.
- [[07 background-clip y background-origin]] — hasta dónde llega y desde dónde.
- [[08 Shorthand background]] — todo en una declaración.
- [[09 Múltiples Fondos]] — varias capas apiladas.

## El fondo cubre contenido + padding

> [!info] El área que pinta el fondo
> Por defecto, el fondo (color e imagen) se pinta bajo el **contenido y el padding**, hasta el borde exterior. Esto se ajusta con [[07 background-clip y background-origin | `background-clip`]]: se puede limitar al contenido, o extender bajo el borde. Por eso el padding "se ve" del color de fondo, mientras que el margin nunca.

## La receta más común: imagen que cubre

> [!tip] El patrón cover + center
> La combinación que más se usa para un fondo de imagen (un hero, una tarjeta) que debe **llenar** su caja sin deformarse:
> ```css
> .hero {
>   background-image: url("foto.jpg");
>   background-size: cover;       /* llena la caja, recorta si hace falta */
>   background-position: center;  /* centra el recorte */
>   background-repeat: no-repeat; /* sin mosaico */
> }
> ```
> `cover` escala la imagen para cubrir todo, `center` la centra, `no-repeat` evita el efecto mosaico. Es tan común que conviene memorizarla.

## Notas relacionadas

- [[01 background-color]] — el fondo más simple.
- [[05 background-size]] — `cover`/`contain`, clave para imágenes.
- [[08 Shorthand background]] — agrupar todas las propiedades.
