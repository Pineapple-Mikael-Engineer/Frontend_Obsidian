---
title: tel: — Enlaces que inician una llamada
aliases:
  - tel
  - phone link
tags:
  - html
  - api/concepto
  - navegacion
draft: false
---

# Enlaces Telefónicos (tel)

> [!definicion]
> Un enlace con `href="tel:…"` inicia una **llamada** al número indicado. En móviles abre la aplicación de teléfono con el número marcado; en escritorio puede lanzar Skype, FaceTime u otra app de llamadas configurada.

```html
<a href="tel:+34911234567">Llámanos: 911 234 567</a>
```

## Formato del número

> [!tip] Usa formato internacional E.164
> El valor de `tel:` debería ir en **formato internacional**: el signo `+`, el código de país y el número, **sin espacios ni guiones** en el `href` (aunque el texto visible sí los lleve para legibilidad):
> ```html
> <a href="tel:+34911234567">+34 911 234 567</a>
> ```
> El formato E.164 (`+` país + número) garantiza que la llamada funcione desde cualquier país, independientemente de la configuración local del dispositivo.

## href vs. texto visible

El `href` lleva el número "limpio" para marcar; el contenido del enlace lo muestra formateado para personas:

```html
<a href="tel:+34911234567">911 23 45 67</a>
```

Los caracteres permitidos en el `href` incluyen dígitos, `+`, y opcionalmente pausas (`,`) y extensiones (`;ext=123`):

```html
<a href="tel:+34911234567;ext=42">Centralita, ext. 42</a>
```

## Cuándo tiene sentido

| Contexto | ¿Útil tel:? |
|----------|-------------|
| Sitio visto en móvil | Muy útil: un toque y llama |
| Tarjeta de contacto / negocio local | Sí, mejora la conversión |
| Solo escritorio sin app de llamadas | El clic puede no hacer nada |

## Accesibilidad

> [!info] El número debe ser legible
> El texto del enlace debe mostrar el número de forma legible para todos, no solo el `href`. Un lector de pantalla anuncia el contenido del enlace; si pones solo un icono de teléfono, añade `aria-label` con el número o una descripción ("Llamar al 911 23 45 67").

## Buenas prácticas

> [!tip] Recomendaciones
> - `href` en formato E.164 (`+` país, sin espacios); texto visible formateado para leer.
> - Especialmente valioso en sitios con tráfico móvil y negocios locales.
> - Inclúyelo dentro de [[10 Dirección (address) | `<address>`]] cuando sea el contacto del sitio.

## Errores comunes

> [!warning] Trampas
> - **Espacios o guiones en el `href`**: algunos sistemas no los toleran; van solo en el texto visible.
> - **Número sin código de país**: falla al marcar desde el extranjero o con roaming.
> - **Olvidar `tel:`**: sin el esquema, el navegador trata el número como una ruta.

## Notas relacionadas

- [[01 Enlaces (a)]] — el elemento `<a>` que soporta el esquema `tel:`.
- [[03 Enlaces a Correo (mailto)]] — el esquema equivalente para correo.
- [[10 Dirección (address)]] — el contenedor semántico para datos de contacto.
