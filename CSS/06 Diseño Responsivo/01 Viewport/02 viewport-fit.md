---
title: viewport-fit — Cubrir la zona del notch
aliases:
  - viewport-fit
  - safe-area-inset
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# viewport-fit

> [!definicion]
> `viewport-fit=cover` (una directiva del [[01 meta viewport | `<meta viewport>`]]) hace que el contenido ocupe **toda la pantalla**, incluida la zona del **notch** (la muesca) y las esquinas redondeadas de los móviles modernos. Se acompaña de las variables `env(safe-area-inset-*)` para no colocar contenido importante bajo la muesca.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover" />
```

## El problema del notch

> [!info] La pantalla ya no es un rectángulo limpio
> Los móviles con muesca (iPhone X+) y esquinas redondeadas tienen zonas donde el contenido puede quedar **tapado** por el notch o la barra de gestos. Por defecto, el navegador deja márgenes de seguridad (un área inutilizada). `viewport-fit=cover` permite que el fondo y los elementos a pantalla completa lleguen **hasta los bordes**, aprovechando toda la pantalla.

## Las safe-area-insets

> [!tip] env() para respetar las zonas seguras
> Con `viewport-fit=cover`, el contenido importante (texto, botones) podría quedar bajo el notch. Las variables de entorno `env(safe-area-inset-*)` dan la distancia segura a cada borde, para añadir padding solo donde hace falta:
> ```css
> .barra {
>   padding-top: env(safe-area-inset-top);       /* baja del notch */
>   padding-bottom: env(safe-area-inset-bottom); /* sube de la barra de gestos */
>   padding-left: env(safe-area-inset-left);
>   padding-right: env(safe-area-inset-right);
> }
> ```
> Así el **fondo** llega a los bordes (estética a pantalla completa), pero el **contenido** respeta las zonas seguras.

## env() con valor de respaldo

`env()` acepta un valor por defecto para navegadores sin notch (donde los insets son 0 o no existen):

```css
padding-top: env(safe-area-inset-top, 1rem);   /* 1rem si no hay inset */
```

## Caso de uso típico

```css
/* Cabecera fija que respeta el notch */
.navbar {
  position: fixed; top: 0; inset-inline: 0;
  padding-top: env(safe-area-inset-top);
  /* el fondo cubre el notch; el contenido baja */
}

/* Botón de acción que sube sobre la barra de gestos */
.fab {
  position: fixed;
  bottom: calc(1rem + env(safe-area-inset-bottom));
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `viewport-fit=cover` si quieres contenido/fondo a pantalla completa en móviles con notch.
> - Aplica `env(safe-area-inset-*)` como padding para que el contenido no quede bajo la muesca.
> - Da un valor de respaldo en `env()` para dispositivos sin notch.
> - Combínalo con `position: fixed` en barras y botones flotantes.

## Errores comunes

> [!warning] Trampas
> - **`viewport-fit=cover` sin `safe-area-insets`**: el contenido queda bajo el notch.
> - **Olvidar el inset inferior** (barra de gestos) en botones flotantes.
> - **Esperar efecto** en móviles sin notch: los insets valen 0.

## Notas relacionadas

- [[01 meta viewport]] — la etiqueta que contiene `viewport-fit`.
- [[04 fixed]] — las barras/botones fijos que usan los insets.
- [[04 Viewport Dinámico (svh, lvh, dvh)]] — otro ajuste del viewport en móvil.
