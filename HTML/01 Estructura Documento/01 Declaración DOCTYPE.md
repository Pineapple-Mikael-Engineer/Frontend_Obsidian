---
title: <!DOCTYPE html> — Declaración de tipo de documento
aliases:
  - doctype
  - document type declaration
tags:
  - html
  - api/concepto
  - estructura
elemento: doctype
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
---

# Declaración DOCTYPE

> [!definicion]
> `<!DOCTYPE html>` es la **primera línea** de todo documento HTML. No es un elemento ni una
> etiqueta: es una instrucción al navegador para que renderice en **modo estándar** (standards
> mode) en lugar del modo de compatibilidad heredado (quirks mode).

```html
<!DOCTYPE html>
<html lang="es">
  <!-- … -->
</html>
```

Debe ir **antes** de `<html>`, sin nada que la preceda (ni comentarios, ni espacios en blanco
significativos, ni una línea vacía). Es insensible a mayúsculas: `<!doctype html>` es igualmente
válido.

## Modos de renderizado

| Modo | Se activa cuando | Efecto |
|------|------------------|--------|
| Estándar | El doctype está presente y correcto | El navegador sigue las especificaciones modernas |
| Quirks | Falta el doctype o es inválido | Emula bugs de navegadores de los 90 (box model roto, etc.) |
| Casi-estándar | Doctypes antiguos concretos | Difiere solo en el alto de línea de imágenes en celdas |

El síntoma clásico de **quirks mode** es que `width`/`height` incluyen el `padding` y el `border`
(box model heredado de IE5), descuadrando todo el layout.

> [!info] Por qué es tan corto en HTML5
> Los doctypes de HTML4/XHTML eran cadenas largas con una URL a un DTD (`<!DOCTYPE html PUBLIC
> "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">`). HTML5 abandonó el DTD:
> `<!DOCTYPE html>` es el **mínimo** que basta para activar el modo estándar. No valida nada, solo
> conmuta el modo.

> [!warning] No omitirlo
> Sin doctype, el navegador entra en quirks mode de forma silenciosa: la página se ve, pero el
> layout se comporta de manera impredecible y distinta entre navegadores. Es un bug difícil de
> diagnosticar porque no hay ningún mensaje de error.

## Notas relacionadas

- [[02 Elemento Raíz (html)]] — lo que sigue inmediatamente al doctype.
- [[index]] — el esqueleto completo del documento.
