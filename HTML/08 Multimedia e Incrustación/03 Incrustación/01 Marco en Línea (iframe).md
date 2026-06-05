---
title: <iframe> — Marco en línea
aliases:
  - iframe element
  - inline frame
tags:
  - html
  - api/elemento
  - multimedia
elemento: iframe
categoria: incrustado
rol_implicito: ninguno
vacio: false
draft: false
---

# Marco en Línea (iframe)

> [!definicion]
> `<iframe>` (inline frame) incrusta **otra página web completa** dentro del documento actual: un mapa, un vídeo de YouTube, un widget de terceros, un formulario externo. Es una ventana a otro documento con su propio contexto de navegación.

```html
<iframe src="https://www.youtube.com/embed/VIDEO_ID"
        title="Vídeo: tutorial de HTML"
        width="560" height="315"
        loading="lazy" allowfullscreen></iframe>
```

## Atributos

| Atributo | Descripción |
|----------|-------------|
| `src` | URL del documento a incrustar |
| `title` | **Descripción accesible** del contenido (obligatorio para a11y) |
| `width` / `height` | Dimensiones del marco |
| `loading` | `lazy` difiere la carga hasta acercarse |
| `sandbox` | Restringe lo que el contenido incrustado puede hacer |
| `allow` | Concede permisos concretos (cámara, micrófono, fullscreen) |
| `referrerpolicy` | Qué información de referencia se envía |
| `srcdoc` | HTML en línea a mostrar (en vez de `src`) |

## El title es obligatorio para accesibilidad

> [!warning] iframe sin title es inaccesible
> Un `<iframe>` necesita un `title` que describa su contenido ("Mapa de la ubicación", "Vídeo del tutorial"). Los lectores de pantalla lo anuncian para que el usuario sepa qué hay en ese marco antes de entrar. Sin `title`, anuncian un marco anónimo, desorientador.

## sandbox: contener el contenido

> [!info] sandbox restringe los permisos
> Incrustar contenido de terceros es arriesgado: ese documento podría ejecutar scripts, enviar formularios o abrir ventanas. `sandbox` lo **encierra**, quitándole capacidades que se reconceden una a una:
> ```html
> <iframe src="…" sandbox="allow-scripts allow-same-origin"></iframe>
> ```
> | Valor de `sandbox` | Permite |
> |--------------------|---------|
> | (vacío) | Máxima restricción: sin scripts, formularios, popups |
> | `allow-scripts` | Ejecutar JavaScript |
> | `allow-forms` | Enviar formularios |
> | `allow-same-origin` | Tratar el contenido como del mismo origen |
> | `allow-popups` | Abrir ventanas |
>
> Se concede solo lo imprescindible. `sandbox=""` (vacío) es lo más seguro para contenido no confiable.

## allow: permisos de funciones

`allow` controla el acceso a APIs sensibles (Permissions Policy):

```html
<iframe src="…" allow="camera; microphone; fullscreen"></iframe>
```

## Rendimiento

> [!tip] loading="lazy" en iframes
> Los iframes (sobre todo de vídeos y mapas) son **pesados**: cargan una página entera. `loading="lazy"` evita descargarlos hasta que el usuario se acerca, mejorando mucho la carga inicial. Para incrustaciones muy pesadas (YouTube), una técnica común es mostrar una miniatura y cargar el iframe solo al hacer clic.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon **siempre** un `title` descriptivo.
> - Usa `sandbox` para contenido de terceros, concediendo el mínimo de permisos.
> - `loading="lazy"` para iframes bajo el pliegue.
> - Define `width`/`height` o una proporción CSS para evitar saltos.

## Errores comunes

> [!warning] Trampas
> - **Sin `title`**: inaccesible.
> - **Contenido no confiable sin `sandbox`**: riesgo de seguridad.
> - **iframes pesados sin `lazy`**: penalizan la carga.
> - **Esperar incrustar cualquier sitio**: muchos lo bloquean con `X-Frame-Options`/CSP.

## Notas relacionadas

- [[index]] — el panorama de la incrustación y la seguridad.
- [[02 Objeto Incrustado (object)]] — alternativa para PDF y recursos.
