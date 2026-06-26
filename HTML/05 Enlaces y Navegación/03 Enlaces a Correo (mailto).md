---
title: "mailto: — Enlaces que abren el cliente de correo"
aliases:
  - mailto
  - email link
tags:
  - html
  - api/concepto
  - navegacion
draft: false
---

# Enlaces a Correo (mailto)

> [!definicion]
> Un enlace con `href="mailto:…"` abre el **cliente de correo** del usuario con un mensaje nuevo predefinido. Es un esquema de URL especial que el sistema operativo asocia a la aplicación de correo (Gmail, Outlook, Mail…).

```html
<a href="mailto:hola@sitio.com">Escríbenos</a>
```

## Prerrellenar el mensaje

Se pueden añadir destinatario, asunto y cuerpo mediante parámetros tras `?`, separados por `&`:

| Parámetro | Para qué |
|-----------|----------|
| `subject` | Asunto del correo |
| `body` | Cuerpo del mensaje |
| `cc` | Con copia |
| `bcc` | Con copia oculta |

```html
<a href="mailto:soporte@sitio.com?subject=Consulta%20sobre%20pedido&body=Hola,%20mi%20pedido%20es...">
  Contactar con soporte
</a>
```

Varios destinatarios se separan con comas: `mailto:a@x.com,b@x.com`.

## Codificación de los parámetros

> [!warning] Los espacios y caracteres especiales van codificados
> El valor de `subject` y `body` debe ir **URL-encoded**: los espacios como `%20`, los saltos de línea como `%0A`, los acentos codificados. Escribir espacios literales rompe el enlace en muchos clientes:
> ```html
> <!-- ✅ codificado -->
> <a href="mailto:x@y.com?subject=Hola%20mundo&body=L%C3%ADnea%201%0AL%C3%ADnea%202">…</a>
> ```
> En JavaScript, `encodeURIComponent()` genera esta codificación.

## Limitaciones

> [!info] Depende del cliente configurado
> `mailto:` abre la aplicación de correo **predeterminada del usuario**. Si no tiene ninguna configurada (común en escritorio con webmail), el clic puede no hacer nada o abrir un programa inesperado. Por eso, para formularios de contacto serios, suele preferirse un `<form>` real que envíe los datos a un servidor, dejando `mailto:` para "escríbenos" informales.

## Protección anti-spam

Publicar una dirección en texto plano la expone a los robots que rastrean correos. Estrategias habituales: ofuscar con JavaScript, usar un formulario de contacto, o aceptar el riesgo para direcciones de baja sensibilidad. No hay solución perfecta solo con HTML.

## Buenas prácticas

> [!tip] Recomendaciones
> - Codifica siempre `subject` y `body` (usa `encodeURIComponent`).
> - El texto del enlace puede ser la propia dirección (`hola@sitio.com`) o una acción ("Escríbenos").
> - Para contacto formal o de alto volumen, complementa con un `<form>` en servidor.
> - Coloca los datos de contacto dentro de [[10 Dirección (address) | `<address>`]] cuando sean del autor/sitio.

## Errores comunes

> [!warning] Trampas
> - **Parámetros sin codificar**: espacios y acentos rompen el asunto/cuerpo.
> - **Asumir que siempre funciona**: sin cliente de correo configurado, el enlace puede fallar.
> - **Olvidar `mailto:`** y poner solo la dirección en `href`: navegaría a una página inexistente.

## Notas relacionadas

- [[01 Enlaces (a)]] — el elemento `<a>` que soporta el esquema `mailto:`.
- [[04 Enlaces Telefónicos (tel)]] — el esquema equivalente para llamadas.
- [[10 Dirección (address)]] — dónde marcar el contacto del autor/sitio.
