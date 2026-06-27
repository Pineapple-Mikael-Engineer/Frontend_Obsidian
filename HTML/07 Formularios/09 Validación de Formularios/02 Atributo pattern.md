---
title: Atributo pattern — Validar con expresiones regulares
aliases:
  - pattern
  - regex validation
tags:
  - html
  - api/atributo
  - formularios
elemento: input
categoria: interactivo
rol_implicito: ninguno
vacio: true
draft: false
order: 2
---

# Atributo pattern

> [!definicion]
> `pattern` valida el contenido de un campo de texto contra una **expresión regular**. Si el valor no coincide con el patrón completo, el campo es inválido. Permite imponer formatos que ningún `type` cubre: códigos postales, referencias, nombres de usuario con reglas concretas.

```html
<input type="text" pattern="[0-9]{5}" title="5 dígitos" required />
```

## Cómo se interpreta el patrón

> [!info] El patrón debe cubrir TODO el valor
> A diferencia de un `match` parcial, `pattern` exige que la **expresión completa** coincida con **todo** el valor (como si estuviera anclado con `^...$` implícitos). No hay que escribir `^` ni `$`:
> ```html
> <!-- Coincide solo si TODO el valor son 5 dígitos -->
> <input pattern="[0-9]{5}" />   <!-- "12345" ✅  "12345a" ❌ -->
> ```

## El atributo title es clave

`pattern` por sí solo da un mensaje de error genérico. El atributo `title` aporta la explicación que aparece en el mensaje, indicando qué formato se espera:

```html
<input type="text"
       pattern="[A-Za-z0-9_]{3,16}"
       title="Entre 3 y 16 caracteres: letras, números o guion bajo." />
```

> [!warning] Sin title, el usuario no sabe qué falla
> Un `pattern` sin `title` muestra "Coincide con el formato solicitado" sin decir cuál es. Pon **siempre** un `title` que describa el formato en lenguaje claro.

## Ejemplos útiles

| Formato | `pattern` |
|---------|-----------|
| 5 dígitos (código postal) | `[0-9]{5}` |
| Teléfono de 9 cifras | `[0-9]{9}` |
| Usuario (letras, números, _) | `[A-Za-z0-9_]{3,16}` |
| Solo letras y espacios | `[A-Za-zÁÉÍÓÚáéíóúñÑ ]+` |
| Hex de color | `#[0-9A-Fa-f]{6}` |

## Combinar con type

`pattern` se suma a la validación del `type`. Es especialmente útil con [[03 Tipos de input/05 input tel | `type="tel"`]] (que no valida formato) y para restringir [[03 Tipos de input/03 input email | `email`]]/[[03 Tipos de input/06 input url | `url`]] a dominios concretos:

```html
<input type="tel" pattern="\+?[0-9 ]{9,15}" title="9 a 15 cifras, opcional +" />
<input type="email" pattern=".+@empresa\.com" title="Correo @empresa.com" />
```

## Limitaciones

| Limitación | Detalle |
|------------|---------|
| Solo tipos de texto | `text`, `tel`, `email`, `url`, `password`, `search` |
| No en number/date/checkbox | Esos validan por sus propios medios |
| Sintaxis JavaScript | Usa la sintaxis de regex de JS, sin las barras `/.../` |

## Buenas prácticas

> [!tip] Recomendaciones
> - Acompaña **siempre** `pattern` con un `title` que explique el formato.
> - No necesitas `^`/`$`: el patrón ya se ancla al valor completo.
> - Úsalo sobre todo con `tel` y para restringir dominios/formatos específicos.
> - Recuerda que es saltable: revalida el formato también en el servidor.

## Errores comunes

> [!warning] Trampas
> - **`pattern` sin `title`**: mensaje de error inútil.
> - **Añadir `^...$`** pensando que hace falta: el patrón ya cubre todo el valor (y `^/$` extra pueden romperlo).
> - **Patrón demasiado estricto** que rechaza valores válidos (acentos, formatos internacionales).

## Notas relacionadas

- [[03 Restricciones (required, min, max, step, maxlength)]] — otras restricciones de validación.
- [[03 Tipos de input/05 input tel]] — el tipo que más necesita `pattern`.
- [[01 Validación Nativa HTML5]] — el sistema en el que `pattern` encaja.
