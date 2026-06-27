---
title: <textarea> — Texto multilínea
aliases:
  - textarea
tags:
  - html
  - api/elemento
  - formularios
elemento: textarea
categoria: interactivo
rol_implicito: textbox
vacio: false
draft: false
order: 2
---

# Área de Texto (textarea)

> [!definicion]
> `<textarea>` es un campo de **texto de varias líneas**: comentarios, descripciones, mensajes. A diferencia de [[03 Tipos de input/01 input text | `<input type="text">`]] (una línea), permite saltos de línea y crece en altura. No es void: su valor inicial va **entre las etiquetas**, no en un atributo `value`.

```html
<label for="mensaje">Mensaje</label>
<textarea id="mensaje" name="mensaje" rows="5" maxlength="500"
          placeholder="Escribe tu mensaje…"></textarea>
```

## El valor va entre las etiquetas

> [!warning] textarea no usa value
> A diferencia de `<input>`, el contenido inicial de un `<textarea>` se escribe **entre la apertura y el cierre**, no en un atributo:
> ```html
> <!-- ✅ valor inicial entre etiquetas -->
> <textarea name="bio">Texto inicial</textarea>
>
> <!-- ❌ value no funciona en textarea -->
> <textarea name="bio" value="Texto inicial"></textarea>
> ```
> Además, **todo** lo que haya entre las etiquetas (incluidos espacios y saltos) forma parte del valor, así que `<textarea></textarea>` debe ir pegado para empezar vacío.

## Atributos propios

| Atributo | Efecto |
|----------|--------|
| `rows` | Altura visible en líneas de texto |
| `cols` | Ancho visible en caracteres (mejor controlar con CSS) |
| `maxlength` / `minlength` | Límite de caracteres |
| `wrap` | Cómo se ajusta el texto al enviar (`soft` por defecto, `hard`) |
| `placeholder` | Texto de ejemplo |
| `readonly` / `required` / `disabled` | Estado del control |

## Redimensionado y altura automática

Por defecto, el usuario puede redimensionar el `<textarea>` arrastrando la esquina. Se controla con CSS `resize`:

```css
textarea { resize: vertical; }   /* solo vertical; "none" lo bloquea */
```

Para que crezca automáticamente con el contenido hace falta JavaScript (ajustar la altura al `scrollHeight` en cada `input`), ya que no hay forma puramente CSS robusta.

## El atributo wrap

| `wrap` | Efecto al enviar |
|--------|------------------|
| `soft` (por defecto) | El texto se ajusta visualmente, pero se envía sin saltos añadidos |
| `hard` | Inserta saltos de línea reales donde el texto se ajusta (requiere `cols`) |

## Buenas prácticas

> [!tip] Recomendaciones
> - Valor inicial entre las etiquetas; deja `<textarea></textarea>` pegado para vacío.
> - Controla el tamaño con CSS (`width`, `min-height`, `resize`), no solo con `cols`/`rows`.
> - Usa `maxlength` y, si ayuda, muestra un contador de caracteres con JS.
> - Asocia siempre un `<label>`.

## Errores comunes

> [!warning] Trampas
> - **Usar `value`** en lugar del contenido entre etiquetas.
> - **Espacios/saltos** entre las etiquetas que aparecen como valor inicial inesperado.
> - **Bloquear el redimensionado** (`resize: none`) sin necesidad, perjudicando la usabilidad.

## Notas relacionadas

- [[03 Tipos de input/01 input text]] — para texto de una sola línea.
- [[01 Atributos Comunes de input]] — atributos de estado compartidos.
- [[07 Etiquetas y Agrupación/01 Etiqueta de Campo (label)]] — la etiqueta del campo.
