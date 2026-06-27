---
title: text-stroke — Trazo o contorno de las letras
aliases:
  - text-stroke
  - webkit-text-stroke
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: -webkit-text-stroke
grupo: tipografia
valor_inicial: "0"
hereda: false
animable: false
draft: false
order: 3
---

# text-stroke

> [!definicion]
> `text-stroke` (mayormente con el prefijo `-webkit-`) dibuja un **trazo o contorno** alrededor de cada letra, con un grosor y color definidos. Combinado con un relleno transparente, crea texto "hueco" (solo contorno); con relleno, refuerza los bordes.

```css
.titulo {
  -webkit-text-stroke: 1px #cba6f7;
  color: white;
}
```

## Sintaxis

```css
-webkit-text-stroke: <grosor> <color>;
```

| Propiedad | Controla |
|-----------|----------|
| `-webkit-text-stroke-width` | El grosor del trazo |
| `-webkit-text-stroke-color` | Su color |
| `-webkit-text-stroke` | Shorthand de ambas |

## Texto hueco (solo contorno)

> [!tip] El efecto "outline text"
> Para texto que es **solo contorno**, sin relleno, se combina `-webkit-text-stroke` con un relleno transparente:
> ```css
> .hueco {
>   color: transparent;                /* sin relleno */
>   -webkit-text-stroke: 2px #cba6f7;  /* solo el contorno */
> }
> ```
> Un efecto popular en titulares y diseños llamativos. Conviene un grosor moderado: demasiado, y el texto se vuelve ilegible.

## El prefijo y el soporte

> [!warning] Aún requiere -webkit-
> Pese a su antigüedad, `text-stroke` **sigue requiriendo el prefijo `-webkit-`** en todos los navegadores (no se ha estandarizado sin prefijo). El soporte es amplio, pero es de las pocas propiedades modernas que aún viven con prefijo. Conviene dar un **respaldo**: un `color` sólido por si el trazo no se aplica, para que el texto siga siendo legible.

## stroke vs. text-shadow para contornos

| Técnica | Resultado |
|---------|-----------|
| `-webkit-text-stroke` | Contorno limpio y uniforme |
| Varias `text-shadow` | Contorno simulado (4+ sombras), más pesado |

`text-stroke` da un contorno más limpio que el truco de las cuatro `text-shadow`, aunque este último tiene mejor soporte histórico.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo para titulares de efecto (texto hueco, bordes reforzados), no para cuerpo.
> - Grosor moderado: el contorno grueso emborrona y resta legibilidad.
> - Da un `color` de respaldo por si el `-webkit-text-stroke` no aplica.
> - Para contornos en navegadores sin soporte, considera el truco de `text-shadow`.

## Errores comunes

> [!warning] Trampas
> - **Olvidar el prefijo `-webkit-`**: no funciona sin él.
> - **Trazo grueso** que hace el texto ilegible.
> - **Sin respaldo**: si falla, texto transparente invisible.

## Notas relacionadas

- [[01 text-shadow]] — contornos simulados con sombras.
- [[02 text-emphasis]] — otro efecto tipográfico.
- [[03 border-color]] — el contorno de cajas, distinto del de texto.
