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
> etiqueta: es una instrucción al *parser* del navegador para que renderice en **modo estándar**
> (standards mode) y no en el modo de compatibilidad heredado (quirks mode). Es lo único que separa
> una página moderna de una que emula los bugs de los navegadores de los años 90.

```html
<!DOCTYPE html>
<html lang="es">
  <head> … </head>
  <body> … </body>
</html>
```

Debe ir **antes** de `<html>`, sin nada que la preceda: ni comentarios, ni espacios significativos,
ni una línea en blanco. Es insensible a mayúsculas (`<!doctype html>` es igualmente válido) y es un
token sin etiqueta de cierre.

## Los tres modos de renderizado

El navegador elige un modo de renderizado **al empezar a parsear**, en función de lo que encuentre
(o no) al inicio del documento. La decisión es irreversible para esa carga.

### Modo estándar (standards mode)

Se activa con `<!DOCTYPE html>` presente y correcto. El navegador aplica las especificaciones CSS y
HTML modernas al pie de la letra: box model estándar, herencia correcta, `line-height` predecible.
Es el modo que se quiere **siempre**.

### Modo quirks (quirks mode)

Se activa cuando **falta** el doctype, está mal escrito, o se usa uno muy antiguo. El navegador
emula deliberadamente comportamientos rotos de Internet Explorer 5 para no romper webs de hace
décadas. Síntomas característicos:

- El **box model heredado**: `width` y `height` incluyen `padding` y `border`, descuadrando todo
  cálculo de tamaños.
- `line-height` y el espaciado vertical de imágenes en celdas se comportan distinto.
- Porcentajes de altura y centrado que "no funcionan" sin razón aparente.

### Modo casi-estándar (almost standards mode)

Un punto intermedio que algunos doctypes antiguos activan. Es idéntico al estándar **salvo** en el
cálculo del alto de línea de imágenes dentro de celdas de tabla. En la práctica rara vez importa,
pero explica por qué a veces hay un par de píxeles de diferencia bajo una imagen.

| Modo | Se activa cuando | Box model | Recomendado |
|------|------------------|-----------|-------------|
| Estándar | `<!DOCTYPE html>` correcto | `content-box` estándar | ✅ siempre |
| Casi-estándar | Doctypes transicionales antiguos | Estándar salvo imágenes en celdas | ⚠️ legado |
| Quirks | Sin doctype o inválido | Heredado de IE5 (roto) | 🚫 nunca |

## Por qué en HTML5 es tan corto

Los doctypes de HTML4 y XHTML eran cadenas largas que referenciaban una **DTD** (Document Type
Definition), una gramática formal contra la que validar el documento:

```html
<!-- HTML 4.01 Strict -->
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN"
  "http://www.w3.org/TR/html4/strict.dtd">

<!-- XHTML 1.0 Transitional -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

HTML5 abandonó el modelo de DTD: el lenguaje ya no se valida contra una gramática externa.
`<!DOCTYPE html>` es, literalmente, **la cadena más corta que basta** para que el parser active el
modo estándar. No describe ni valida nada del contenido: es un simple interruptor de modo. Nadie
debería escribir los doctypes largos hoy; se incluyen aquí solo para reconocerlos en código heredado.

## Cómo lo decide el parser

El algoritmo es más tolerante de lo que parece: el navegador compara el doctype con una lista
interna. `<!DOCTYPE html>` siempre da modo estándar. Ciertos doctypes antiguos concretos dan
casi-estándar. Cualquier cosa irreconocible —o la ausencia total— cae en quirks. Por eso un error de
tipeo silencioso (`<!DOCTPE html>`) no da ningún mensaje de error: simplemente la página entra en
quirks y "se comporta raro".

## Buenas prácticas

> [!tip] Reglas prácticas
> - **Escríbelo siempre**, como primerísima línea, en minúscula o como prefieras.
> - **Nunca** los doctypes largos de HTML4/XHTML en proyectos nuevos.
> - Si una página "se ve descuadrada solo en este navegador" y el CSS parece correcto, **revisa lo
>   primero el doctype**: es el sospechoso número uno de un box model roto.
> - Verifica el modo activo en DevTools → consola: `document.compatMode` devuelve `"CSS1Compat"`
>   (estándar) o `"BackCompat"` (quirks).

## Errores comunes

> [!warning] Lo que rompe el modo estándar
> - **Omitirlo**: la página entra en quirks de forma silenciosa, sin error visible. Es un bug
>   difícil de diagnosticar porque "todo parece bien" en el código.
> - **Cualquier cosa antes del doctype**: un BOM mal configurado, una etiqueta PHP que imprime un
>   espacio, o una línea en blanco generada por una plantilla pueden empujar contenido por delante y
>   tirar la página a quirks.
> - **Confiar en que el framework lo pone**: la mayoría lo hacen, pero al escribir HTML a mano es
>   responsabilidad propia.

## Notas relacionadas

- [[02 Elemento Raíz (html)]] — lo que sigue inmediatamente al doctype.
- [[index]] — el esqueleto completo del documento y por qué este orden es fijo.
