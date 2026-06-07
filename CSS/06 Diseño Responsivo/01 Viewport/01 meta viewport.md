---
title: meta viewport — La etiqueta imprescindible del responsive
aliases:
  - meta viewport css
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# meta viewport

> [!definicion]
> `<meta name="viewport" content="width=device-width, initial-scale=1.0">` indica al navegador móvil que ajuste el **viewport al ancho real** del dispositivo, sin escalar la página. Es la línea sin la cual ningún diseño responsivo funciona en móvil.

```html
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
</head>
```

## Las directivas de content

| Directiva | Valor típico | Efecto |
|-----------|--------------|--------|
| `width` | `device-width` | El viewport iguala el ancho del dispositivo |
| `initial-scale` | `1.0` | Sin zoom inicial |
| `minimum-scale` | `1.0` | Zoom mínimo |
| `maximum-scale` | `5.0` | Zoom máximo |
| `user-scalable` | `yes` / `no` | Si se permite el pinch-zoom |
| `viewport-fit` | `cover` | Cubrir bajo el notch del móvil |

El detalle de `viewport-fit` está en [[02 viewport-fit]].

## La línea estándar

> [!tip] width=device-width, initial-scale=1.0
> Esta combinación es la configuración correcta para el 99% de los casos y no debería faltar en ninguna página:
> ```html
> <meta name="viewport" content="width=device-width, initial-scale=1.0" />
> ```
> `width=device-width` hace que el viewport de layout (sobre el que se calcula el CSS) sea el ancho físico del dispositivo; `initial-scale=1.0` evita el zoom de partida. Con esto, las media queries comparan contra el ancho real del móvil.

## Nunca deshabilitar el zoom

> [!warning] user-scalable=no viola la accesibilidad
> Es tentador "bloquear" el zoom (`user-scalable=no` o `maximum-scale=1.0`) para que la interfaz no se descuadre, pero impedir ampliar la página es una **barrera de accesibilidad grave** para personas con baja visión, y viola el criterio **WCAG 1.4.4**. La configuración accesible deja el zoom libre:
> ```html
> <!-- ❌ bloquea el zoom -->
> <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
> <!-- ✅ -->
> <meta name="viewport" content="width=device-width, initial-scale=1.0" />
> ```

## Es el prerrequisito de las media queries

> [!info] Sin viewport, las media queries fallan en móvil
> El vínculo crucial con el CSS responsivo: las [[02 Media Queries/index | media queries]] de ancho (`@media (max-width: 600px)`) se comparan contra el **viewport de layout**. Sin la etiqueta, ese viewport es ~980px en móvil, así que las media queries de móvil **nunca se activan**, y el diseño responsivo "no funciona" pese a estar bien escrito. La etiqueta es lo primero que se revisa cuando el responsive falla.

## Buenas prácticas

> [!tip] Recomendaciones
> - Incluye `width=device-width, initial-scale=1.0` en **toda** página.
> - **No** deshabilites el zoom (`user-scalable=no`).
> - Añade `viewport-fit=cover` si diseñas para móviles con notch.
> - Si las media queries "no funcionan" en móvil, revisa primero esta etiqueta.

## Errores comunes

> [!warning] Trampas
> - **Olvidarla**: la página se ve diminuta y las media queries no se activan en móvil.
> - **Bloquear el zoom**: barrera de accesibilidad.
> - **Asumir que el framework la pone**: verifícalo al escribir HTML a mano.

## Notas relacionadas

- [[02 Media Queries/index]] — lo que depende de un viewport bien configurado.
- [[02 viewport-fit]] — el notch.
- [[02 Viewport (meta viewport)]] — la etiqueta desde HTML.
