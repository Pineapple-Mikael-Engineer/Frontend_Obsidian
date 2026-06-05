---
title: <html> — Elemento raíz del documento
aliases:
  - html element
  - root element
tags:
  - html
  - api/elemento
  - estructura
elemento: html
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Elemento Raíz (html)

> [!definicion]
> `<html>` es el **nodo raíz** del árbol DOM: todo el documento cuelga de él. Contiene exactamente
> dos hijos, en este orden: [[03 Cabecera (head)/index | `<head>`]] (metadatos) y
> [[04 Cuerpo (body) | `<body>`]] (contenido visible). Es el ancestro común de todo lo demás y el
> punto donde se fija el idioma de la página.

```html
<!DOCTYPE html>
<html lang="es" dir="ltr">
  <head> … </head>
  <body> … </body>
</html>
```

## Atributos clave

| Atributo | Valores | Descripción |
|----------|---------|-------------|
| `lang` | código BCP 47 (`es`, `en`, `es-MX`, `pt-BR`) | Idioma principal del documento |
| `dir` | `ltr` · `rtl` · `auto` | Dirección base del texto |
| `xmlns` | `http://www.w3.org/1999/xhtml` | Solo necesario al servir como XHTML real (raro) |

### El atributo `lang` en profundidad

`lang` no es decorativo: es uno de los ajustes con mayor impacto por carácter escrito de toda la
plataforma. Gobierna:

- **Pronunciación** del lector de pantalla. Una página en español con `lang="en"` se lee con fonética
  inglesa, volviéndola casi ininteligible.
- **Selección de tipografía y reglas de guionado** (`hyphens: auto` en CSS depende del idioma).
- **Traducción automática**: Chrome decide si ofrecer "Traducir esta página" comparando `lang` con
  el idioma del usuario.
- **Comillas tipográficas** de [[14 Citas en Línea (q) | `<q>`]] y el corrector ortográfico.
- **Pseudo-selector CSS** `:lang(es)`, útil para estilos por idioma.

El valor sigue el estándar **BCP 47**: una etiqueta de idioma (`es`) opcionalmente seguida de región
(`es-419` para español latinoamericano) u otras subetiquetas. Para la mayoría de sitios basta el
idioma a secas (`es`, `en`).

Para fragmentos en otro idioma, `lang` se sobrescribe localmente porque es un
[[04 Idioma y Dirección (lang, dir) | atributo global]]:

```html
<html lang="es">
  …
  <p>La expresión <span lang="fr">savoir-faire</span> es de uso común.</p>
</html>
```

### El atributo `dir`

Fija la dirección base de escritura. `ltr` (izquierda→derecha) es el valor por defecto implícito;
`rtl` (derecha→izquierda) es necesario para árabe, hebreo o persa, e **invierte el layout completo**:
el texto, el orden de las columnas flex/grid y la posición de scrollbars. `auto` deja que el
navegador deduzca la dirección del primer carácter fuerte, útil para contenido generado por usuarios.

## html como raíz de estilo

En CSS, el selector `:root` apunta a este mismo `<html>`, pero con **mayor especificidad** que el
selector de tipo `html`. Es el lugar canónico para declarar variables CSS globales (custom
properties), porque desde la raíz se heredan a todo el árbol:

```css
:root {
  --color-marca: #cba6f7;
  --espaciado-base: 1rem;
}
```

También es el elemento sobre el que actúa la unidad `rem` (root em): `1rem` siempre equivale al
`font-size` de `<html>`, independientemente de anidamientos.

## Accesibilidad

> [!info] El primer ajuste de a11y
> `lang` en `<html>` es el criterio **WCAG 3.1.1 (nivel A)**: el más barato de cumplir de toda la
> norma —un solo atributo— y de los de mayor impacto. Sin él, los lectores de pantalla adivinan el
> idioma (a menudo mal) y la experiencia para usuarios ciegos se degrada por completo. Es lo primero
> que se revisa en cualquier auditoría de accesibilidad.

## Buenas prácticas

> [!tip] Recomendaciones
> - Declara siempre `lang` con el idioma real del contenido principal.
> - Usa `dir="rtl"` solo cuando el idioma lo requiera; no es un interruptor de "alinear a la derecha"
>   (eso es `text-align` en CSS).
> - Declara las custom properties globales en `:root`, no en `body`, para que `rem` y la herencia
>   partan del nivel más alto.

## Errores comunes

> [!warning] Trampas frecuentes
> - **Olvidar `lang`** o copiarlo de una plantilla en otro idioma (`lang="en"` en una web en
>   español): rompe la accesibilidad de forma invisible para quien ve la pantalla.
> - **Confundir `dir="rtl"` con alineación**: cambia la direccionalidad lógica del documento entero,
>   no solo el alineado.
> - Poner contenido o estilos como hijos directos de `<html>` fuera de `<head>`/`<body>`: el parser
>   lo reubica, casi nunca como se espera.

## Notas relacionadas

- [[01 Declaración DOCTYPE]] — la línea que lo precede.
- [[03 Cabecera (head)/index]] — su primer hijo.
- [[04 Cuerpo (body)]] — su segundo hijo.
- [[04 Idioma y Dirección (lang, dir)]] — `lang`/`dir` como atributos globales reutilizables.
