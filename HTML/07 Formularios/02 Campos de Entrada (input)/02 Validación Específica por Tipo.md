---
title: Validación específica por tipo de input
aliases:
  - type validation
  - validación por tipo
tags:
  - html
  - api/concepto
  - formularios
elemento: input
categoria: interactivo
rol_implicito: ninguno
vacio: true
draft: false
---

# Validación Específica por Tipo

> [!definicion]
> Más allá de las restricciones genéricas (`required`, `min`, `max`), **cada `type` de [[index | `<input>`]] aporta su propia validación implícita**: un `email` exige formato de correo, un `url` exige formato de URL, un `number` rechaza letras. Elegir el `type` correcto activa esta validación gratis, sin escribir una sola expresión regular.

```html
<input type="email" required />   <!-- exige texto con formato de correo -->
<input type="url" />              <!-- exige formato de URL -->
<input type="number" min="1" />   <!-- solo números, ≥ 1 -->
```

## Qué valida cada tipo por sí mismo

| `type` | Validación implícita |
|--------|----------------------|
| `email` | Formato de correo (`texto@dominio`); con `multiple`, lista separada por comas |
| `url` | Formato de URL absoluta (con esquema) |
| `number` | Solo valores numéricos; respeta `min`/`max`/`step` |
| `tel` | **Ninguna** por defecto (los formatos varían por país) |
| `date`, `time`, etc. | Fecha/hora válida; respeta `min`/`max`/`step` |
| `color` | Siempre un color válido (`#rrggbb`) |
| `range` | Siempre dentro de `min`–`max` |

## tel no valida: la excepción que sorprende

> [!info] Por qué tel no valida formato
> Cabría esperar que `type="tel"` validara números de teléfono, pero **no lo hace**: los formatos telefónicos varían tanto entre países (longitud, prefijos, espacios) que el navegador no impone ninguno. Lo que `type="tel"` sí aporta es el **teclado numérico** en móvil. Para validar el formato, se combina con [[09 Validación de Formularios/02 Atributo pattern | `pattern`]]:
> ```html
> <input type="tel" pattern="[0-9]{9}" placeholder="9 dígitos" />
> ```

## El tipo cambia el teclado en móvil

Una ventaja clave, a menudo más útil que la validación, es que `type` determina el **teclado virtual** que aparece en móvil:

| `type` | Teclado en móvil |
|--------|------------------|
| `email` | Teclado con `@` y `.` visibles |
| `tel` | Teclado numérico de teléfono |
| `number` | Teclado numérico |
| `url` | Teclado con `/` y `.com` |
| `text` | Teclado completo estándar |

Elegir bien el `type` mejora la experiencia móvil aunque no se use la validación.

## La validación nativa es solo la primera capa

> [!warning] Validar también en el servidor
> La validación por tipo ocurre en el **navegador** (cliente) y es una ayuda a la usabilidad, no una garantía de seguridad: se puede saltar (desactivando JS, con `novalidate`, o enviando la petición directamente). **Todo dato debe revalidarse en el servidor.** La validación nativa mejora la experiencia; la del servidor protege los datos. Detalle en [[09 Validación de Formularios/01 Validación Nativa HTML5 | Validación nativa]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Elige siempre el `type` más específico que encaje con el dato.
> - Combina el tipo con `pattern` cuando necesites un formato concreto (sobre todo en `tel`).
> - No confíes solo en la validación del navegador: revalida en servidor.
> - Aprovecha el teclado móvil que cada tipo activa, aunque no valides.

## Errores comunes

> [!warning] Trampas
> - **Esperar que `tel` valide**: no lo hace; usa `pattern`.
> - **Usar `text` para correos/números**: pierdes validación y teclado adecuado.
> - **Fiarte solo del cliente**: la validación nativa es saltable.

## Notas relacionadas

- [[01 Atributos Comunes de input]] — las restricciones genéricas que se combinan con el tipo.
- [[09 Validación de Formularios/01 Validación Nativa HTML5]] — el sistema de validación completo.
- [[03 Tipos de input/index]] — cada tipo y su validación concreta.
