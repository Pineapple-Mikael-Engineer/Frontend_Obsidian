---
title: contenteditable — Hacer editable el contenido
aliases:
  - contenteditable
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Editable (contenteditable)

> [!definicion]
> `contenteditable` convierte cualquier elemento en **editable**: el usuario puede escribir, borrar y dar formato directamente sobre el contenido, como en un procesador de texto. Es la base de los **editores de texto enriquecido** (WYSIWYG) en la web.

```html
<div contenteditable="true">
  Edita este texto directamente.
</div>
```

## Valores

| Valor | Efecto |
|-------|--------|
| `true` (o vacío) | El contenido es editable |
| `false` | No editable |
| `plaintext-only` | Editable, pero **solo texto plano** (sin formato) |

## Para qué se usa

El caso principal es construir **editores enriquecidos**: el usuario escribe y aplica negrita, listas, enlaces, y el HTML del elemento refleja el resultado. Frameworks de editor (como los de comentarios, blogs, notas) se apoyan en `contenteditable`.

## Las complicaciones

> [!warning] contenteditable es difícil de domar
> Aunque parece simple, `contenteditable` es notoriamente **complicado** de usar bien:
> - El HTML que genera varía entre navegadores (diferentes etiquetas para un salto de línea, formato inconsistente).
> - Gestionar la **selección y el cursor** con JavaScript es delicado.
> - Pegar contenido externo trae HTML "sucio" que hay que sanear.
> - Sin saneamiento, es una vía de **XSS** si el contenido se guarda y se reinyecta.
>
> Por eso los editores serios usan **librerías** (ProseMirror, Slate, TipTap, Quill) que abstraen estos problemas, en lugar de manejar `contenteditable` a pelo.

## Seguridad

> [!warning] Sanear siempre el HTML resultante
> El contenido que el usuario crea (o pega) en un `contenteditable` es HTML arbitrario. Si se guarda y se vuelve a mostrar sin **sanear**, un usuario malicioso podría inyectar scripts (XSS). Todo HTML generado por el usuario debe limpiarse (con una librería de saneamiento) antes de guardarlo o reinyectarlo.

## contenteditable vs. textarea

| | `contenteditable` | `<textarea>` |
|--|-------------------|-----------|
| Formato enriquecido | Sí (HTML) | No (solo texto plano) |
| Complejidad | Alta | Baja |
| Riesgo XSS | Sí (hay que sanear) | Mínimo |
| Uso | Editores WYSIWYG | Texto plano (comentarios, mensajes) |

Para texto plano, `<textarea>` es mucho más simple y seguro. `contenteditable` solo cuando se necesita formato enriquecido.

## Buenas prácticas

> [!tip] Recomendaciones
> - Para texto plano, usa `<textarea>`, no `contenteditable`.
> - Para editores enriquecidos, apóyate en una **librería**, no en `contenteditable` crudo.
> - **Sanea siempre** el HTML resultante antes de guardarlo o mostrarlo (anti-XSS).
> - `plaintext-only` si quieres edición sin formato pero sobre un elemento normal.

## Errores comunes

> [!warning] Trampas
> - **Construir un editor desde cero** con `contenteditable`: mil casos límite.
> - **No sanear** el HTML del usuario: agujero de XSS.
> - **Usarlo para texto plano** donde `<textarea>` bastaría.

## Notas relacionadas

- [[03 Área de Texto (textarea)]] — la alternativa simple para texto plano.
- [[10 Corrección Ortográfica (spellcheck)]] — se combina con campos editables.
