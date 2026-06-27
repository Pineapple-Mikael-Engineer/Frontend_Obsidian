---
title: <meta viewport> — Control del viewport en móviles
aliases:
  - viewport
  - responsive meta tag
tags:
  - html
  - api/elemento
  - responsive
elemento: meta
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
order: 2
---

# Viewport (meta viewport)

> [!definicion]
> `<meta name="viewport">` indica al navegador móvil cómo ajustar el **área visible** (viewport) al
> ancho real del dispositivo. Sin ella, el móvil asume un viewport virtual de ~980 px y **escala la
> página entera hacia abajo** para que quepa, dejándola diminuta y obligando a hacer zoom para leer.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

Esta línea es la configuración estándar y suficiente para diseño responsivo: el ancho del viewport
iguala al del dispositivo y la página carga sin zoom.

## Viewport de layout vs. viewport visual

Para entender por qué hace falta esta etiqueta conviene distinguir dos "viewports":

- **Viewport de layout** — el ancho que el navegador usa para calcular el CSS (dónde rompen las
  media queries, qué es el 100 %).
- **Viewport visual** — lo que cabe en pantalla en cada momento (cambia con el zoom).

En escritorio coinciden. En móvil, sin la etiqueta, el navegador fija el viewport de layout en
~980 px (para que las webs antiguas no responsivas "quepan") y luego reduce todo visualmente. La
etiqueta `width=device-width` iguala el viewport de layout al ancho físico, que es lo que el diseño
responsivo espera.

## Directivas de `content`

| Directiva | Valor típico | Efecto |
|-----------|--------------|--------|
| `width` | `device-width` | Iguala el viewport de layout al ancho del dispositivo |
| `initial-scale` | `1.0` | Nivel de zoom al cargar (1 = sin zoom) |
| `minimum-scale` | `1.0` | Zoom mínimo permitido |
| `maximum-scale` | `5.0` | Zoom máximo permitido |
| `user-scalable` | `yes` / `no` | Si el usuario puede hacer pinch-zoom |
| `viewport-fit` | `cover` | Extiende el contenido bajo el notch en móviles con muesca |

### viewport-fit y el notch

En dispositivos con muesca (iPhone X+), `viewport-fit=cover` hace que el contenido ocupe toda la
pantalla, incluida la zona del notch. Se combina con las variables CSS `env(safe-area-inset-*)` para
no colocar contenido importante bajo la muesca:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover" />
```

## Por qué es el cimiento del responsive

Sin esta etiqueta, las **media queries** de CSS no se comportan como se espera en móvil: una regla
`@media (max-width: 600px)` puede no activarse nunca porque el viewport reportado es 980, no el ancho
real del teléfono. Es el prerrequisito de cualquier
[[03 Flexbox/index | layout]] adaptable (delegado a CSS).

## Accesibilidad

> [!warning] No deshabilites el zoom
> `user-scalable=no` o `maximum-scale=1.0` impiden ampliar la página con pinch-zoom. Es una
> **barrera de accesibilidad grave** para personas con baja visión y viola el criterio **WCAG 1.4.4
> (Resize Text)**. La configuración accesible deja el zoom libre:
> ```html
> <!-- ❌ bloquea el zoom -->
> <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no" />
> <!-- ✅ permite ampliar -->
> <meta name="viewport" content="width=device-width, initial-scale=1.0" />
> ```
> Salvo casos muy concretos (un mapa interactivo con su propio zoom), nunca se restringe.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa la línea estándar `width=device-width, initial-scale=1.0` y no la toques salvo necesidad real.
> - No fijes `maximum-scale` ni `user-scalable=no`.
> - Si diseñas para móviles con notch, añade `viewport-fit=cover` y usa `env(safe-area-inset-*)`.

## Errores comunes

> [!warning] Trampas
> - **Olvidarla**: el síntoma es una web responsiva que en móvil se ve "alejada" y minúscula, con
>   las media queries aparentemente ignoradas.
> - **Confundir `initial-scale` con `width`**: `initial-scale` controla el zoom inicial, `width`
>   controla el ancho de cálculo; se complementan.

## Notas relacionadas

- [[01 Codificación de Caracteres (meta charset)]] — el otro `<meta>` que va arriba del todo.
- [[index]] — el orden del `<head>`.
