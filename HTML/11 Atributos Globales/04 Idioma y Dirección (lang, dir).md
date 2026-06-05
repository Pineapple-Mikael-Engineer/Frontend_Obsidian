---
title: lang y dir — Idioma y dirección del texto
aliases:
  - lang
  - dir
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

# Idioma y Dirección (lang, dir)

> [!definicion]
> `lang` declara el **idioma** del contenido y `dir` su **dirección** de escritura. Como atributos globales, se aplican a todo el documento desde [[02 Elemento Raíz (html) | `<html>`]] o a **fragmentos** concretos cuando difieren del idioma principal.

```html
<html lang="es" dir="ltr">
  <p>La expresión <span lang="fr">déjà vu</span> es común.</p>
</html>
```

## lang: el idioma

`lang` usa códigos **BCP 47** (`es`, `en`, `pt-BR`). Su impacto va mucho más allá de lo decorativo:

| Afecta a | Cómo |
|----------|------|
| Lectores de pantalla | Pronunciación correcta del texto |
| Tipografía y guionado | `hyphens: auto` depende del idioma |
| Corrector ortográfico | Usa el diccionario adecuado |
| Comillas de `<q>` | `«»` en español, `""` en inglés |
| Traducción automática | Decide si ofrecer traducir |
| CSS `:lang()` | Estilar por idioma |

A nivel de documento se pone en `<html>`; en un fragmento, en el elemento concreto (lo que hereda el resto). Detalle del nivel documento en [[02 Elemento Raíz (html) | `<html>`]].

## dir: la dirección

`dir` fija la dirección base del texto:

| Valor | Significado |
|-------|-------------|
| `ltr` | Izquierda → derecha (por defecto) |
| `rtl` | Derecha → izquierda (árabe, hebreo, persa) |
| `auto` | El navegador deduce la dirección del primer carácter fuerte |

> [!warning] dir="rtl" invierte el layout, no solo alinea
> `dir="rtl"` no es "alinear a la derecha": cambia la **direccionalidad lógica** del documento. Invierte el orden de las columnas flex/grid, el inicio/fin de los márgenes lógicos (`margin-inline-start`), la posición de las viñetas y scrollbars. Para idiomas RTL es imprescindible; para "alinear a la derecha" por estética, usa `text-align: right` en CSS.

## auto para contenido de usuario

`dir="auto"` es útil para texto de **origen desconocido** (nombres de usuario, comentarios): el navegador detecta la dirección según el primer carácter fuerte. Se relaciona con [[24 Texto Bidireccional (bdo, bdi) | `<bdi>`]] para aislar fragmentos bidireccionales.

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara `lang` en `<html>` (idioma principal) y en fragmentos en otro idioma.
> - Usa `dir="rtl"` solo cuando el idioma lo requiera, no para alinear.
> - `dir="auto"` para contenido de usuario de dirección desconocida.
> - Mantén `lang` coherente con el contenido real.

## Errores comunes

> [!warning] Trampas
> - **`lang` heredado** de una plantilla en otro idioma.
> - **`dir="rtl"` para alinear** texto a la derecha (usa CSS).
> - **No marcar fragmentos** en otro idioma con `lang`.

## Notas relacionadas

- [[02 Elemento Raíz (html)]] — `lang`/`dir` a nivel de documento.
- [[24 Texto Bidireccional (bdo, bdi)]] — manejo fino de texto bidireccional.
- [[01 Idioma (html lang)]] — `lang` desde el SEO internacional.
