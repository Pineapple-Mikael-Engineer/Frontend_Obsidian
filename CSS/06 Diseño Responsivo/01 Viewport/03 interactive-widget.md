---
title: interactive-widget — El teclado virtual y el viewport
aliases:
  - interactive-widget
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# interactive-widget

> [!definicion]
> `interactive-widget` (una directiva del [[01 meta viewport | `<meta viewport>`]]) controla **cómo afecta el teclado virtual** al viewport en móvil cuando aparece: si lo encoge, lo solapa, o no lo cambia. Resuelve los saltos de layout molestos al enfocar un campo de texto en el móvil.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, interactive-widget=resizes-content" />
```

## El problema: el teclado descuadra el layout

> [!info] Al abrir el teclado, el viewport cambia
> En móvil, cuando el usuario toca un campo de texto, aparece el **teclado virtual**, que ocupa media pantalla. Esto puede:
> - **Tapar** el campo enfocado (queda detrás del teclado).
> - **Encoger** el viewport, descuadrando elementos con `100vh`/`fixed`.
> - Provocar **saltos** de layout incómodos.
>
> `interactive-widget` da control sobre este comportamiento.

## Valores

| Valor | Comportamiento al aparecer el teclado |
|-------|----------------------------------------|
| `resizes-visual` | Encoge el viewport **visual** (por defecto en muchos navegadores) |
| `resizes-content` | Encoge el viewport de **layout** (el contenido se reajusta) |
| `overlays-content` | El teclado **se superpone** sin cambiar el viewport |

## resizes-content para layouts predecibles

`interactive-widget=resizes-content` hace que el viewport de **layout** (sobre el que se calcula el CSS) se reduzca cuando aparece el teclado, así que un elemento con `100dvh` o `position: fixed; bottom: 0` se ajusta correctamente sobre el teclado, en vez de quedar oculto detrás:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, interactive-widget=resizes-content" />
```

## Relación con dvh

> [!tip] Combinar con unidades de viewport dinámico
> Junto con las [[04 Viewport Dinámico (svh, lvh, dvh) | unidades dinámicas]] (`dvh`), `interactive-widget` ayuda a que las interfaces se comporten bien con el teclado abierto: una barra de acciones fija en `bottom: 0` con `dvh` y `resizes-content` se mantiene visible sobre el teclado, no escondida tras él.

## Soporte

> [!info] Característica reciente
> `interactive-widget` es una directiva **relativamente nueva**; su soporte está creciendo. Sin ella, el comportamiento del teclado depende del navegador (a menudo `resizes-visual`). Para formularios móviles con campos cerca del borde inferior, conviene conocerla aunque el soporte aún no sea universal.

## Buenas prácticas

> [!tip] Recomendaciones
> - Considera `interactive-widget=resizes-content` en interfaces con campos cerca del borde inferior.
> - Combínalo con `dvh` para barras fijas que deban quedar sobre el teclado.
> - Prueba el comportamiento del teclado en móviles reales (varía entre navegadores).
> - Como soporte aún crece, no dependas de ella para layouts críticos sin probar.

## Errores comunes

> [!warning] Trampas
> - **Campos tapados por el teclado** sin gestionar el viewport.
> - **`100vh` con el teclado abierto**: descuadre; usa `dvh` y/o `resizes-content`.
> - **Asumir soporte universal**: es una directiva reciente.

## Notas relacionadas

- [[01 meta viewport]] — la etiqueta que contiene la directiva.
- [[04 Viewport Dinámico (svh, lvh, dvh)]] — las unidades que se combinan.
- [[05 Formularios Accesibles]] — formularios móviles usables.
