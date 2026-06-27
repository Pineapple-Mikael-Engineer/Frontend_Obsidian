---
title: input type="email" — Dirección de correo
aliases:
  - input email
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: textbox
vacio: true
draft: false
order: 3
---

# input email

> [!definicion]
> `<input type="email">` es un campo de texto especializado en **direcciones de correo**: valida que el valor tenga formato de email (`texto@dominio`) y, en móvil, muestra un teclado con `@` y `.` accesibles.

```html
<label for="correo">Correo electrónico</label>
<input type="email" id="correo" name="correo"
       autocomplete="email" required placeholder="tu@correo.com" />
```

## Validación de formato

El navegador valida que el valor parezca un correo (contiene `@`, hay algo antes y después). Es una comprobación **sintáctica básica**, no verifica que el correo exista:

| Valor | ¿Válido? |
|-------|----------|
| `ana@x.com` | Sí |
| `ana@x` | Sí (el dominio sin TLD se acepta) |
| `ana.com` | No (falta `@`) |
| ` ` (vacío) | Sí si no es `required` |

## El atributo multiple

Con `multiple`, el campo acepta **varios correos** separados por comas, todos validados:

```html
<input type="email" multiple placeholder="a@x.com, b@y.com" />
```

## Restringir el dominio con pattern

Para exigir un dominio concreto (correos corporativos), se combina con [[02 Atributo pattern | `pattern`]]:

```html
<input type="email" pattern=".+@empresa\.com" title="Debe ser un correo @empresa.com" />
```

## Validación real solo en servidor

> [!warning] El formato no garantiza existencia
> `type="email"` confirma que el texto **parece** un correo, no que **exista** ni que el usuario lo controle. La verificación real es enviar un correo de confirmación con un enlace/código. La validación del cliente mejora la usabilidad; la confirmación por correo prueba la propiedad.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `type="email"` siempre para correos: validación + teclado móvil adecuado.
> - Añade `autocomplete="email"` para el rellenado automático.
> - Para confirmar el correo de verdad, envía un email de verificación.
> - Usa `pattern` solo si necesitas restringir el dominio.

## Errores comunes

> [!warning] Trampas
> - **Usar `text` para correos**: pierdes validación y teclado.
> - **Confiar en que el correo existe** por pasar la validación: hay que confirmarlo.
> - **Doble campo "repite tu email"**: molesto; mejor un botón de mostrar o confirmación posterior.

## Notas relacionadas

- [[02 Validación Específica por Tipo]] — cómo valida cada tipo.
- [[06 input url]] — el equivalente para direcciones web.
- [[02 Atributo pattern]] — restringir el dominio.
