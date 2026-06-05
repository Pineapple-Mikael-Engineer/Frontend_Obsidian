---
title: Tipos de input — Los 22 valores de type
aliases:
  - input types
  - tipos de input
tags:
  - html
  - api/concepto
  - formularios
draft: false
---

# Tipos de input

> [!definicion]
> El atributo `type` transforma el [[index | `<input>`]] en uno de **22 controles** distintos. Esta carpeta dedica una nota a cada uno, centrada en lo que ese tipo aporta de específico (su teclado móvil, su validación, sus atributos propios). Lo común a todos vive en [[01 Atributos Comunes de input | Atributos comunes]].

```html
<input type="email" />
<input type="number" min="0" max="100" />
<input type="checkbox" />
<input type="color" />
```

## Catálogo por familias

**Texto:**

- [[01 input text]] — texto libre de una línea.
- [[02 input password]] — texto oculto.
- [[03 input email]] — correo, con validación y teclado de correo.
- [[06 input url]] — URL, con validación y teclado de URL.
- [[05 input tel]] — teléfono, con teclado numérico (sin validación).
- [[07 input search]] — búsqueda, con botón de borrado.

**Numérico:**

- [[04 input number]] — número con flechas de incremento.
- [[18 input range]] — deslizador entre un mínimo y un máximo.

**Fecha y hora:**

- [[12 input date]] · [[13 input datetime-local]] · [[14 input month]] · [[15 input week]] · [[16 input time]].

**Selección:**

- [[09 input checkbox]] — casilla de verificación (sí/no, multiselección).
- [[10 input radio]] — botón de opción (una de varias).

**Especiales:**

- [[08 input hidden]] — dato oculto que viaja con el formulario.
- [[11 input file]] — subida de archivos.
- [[17 input color]] — selector de color.
- [[19 input image]] — botón de envío con imagen.

**Botones:**

- [[20 input submit]] — envía el formulario.
- [[21 input reset]] — restablece los campos.
- [[22 input button]] — botón genérico (acción con JS).

## El valor por defecto: text

Si se omite `type`, o se pone un valor no reconocido, el navegador lo trata como `type="text"`. Por eso elegir un tipo más específico es siempre una mejora sobre el comportamiento por defecto.

## Por qué el tipo importa tanto

> [!info] Tres beneficios de elegir bien el tipo
> 1. **Teclado móvil adecuado**: `email`, `tel`, `number`, `url` muestran teclados optimizados.
> 2. **Validación gratis**: `email`/`url`/`number` validan su formato sin código.
> 3. **UI nativa**: `date` abre un calendario, `color` una paleta, `range` un deslizador.
>
> Detalle en [[02 Validación Específica por Tipo | Validación por tipo]].

## Notas relacionadas

- [[01 Atributos Comunes de input]] — lo compartido por todos los tipos.
- [[02 Validación Específica por Tipo]] — la validación que cada tipo activa.
- [[03 Restricciones (required, min, max, step, maxlength)]] — límites aplicables a los tipos.
