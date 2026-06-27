---
title: <dl>, <dt>, <dd> — Lista de definiciones
aliases:
  - dl
  - dt
  - dd
  - description list
tags:
  - html
  - api/elemento
  - semantica
elemento: dl
categoria: flujo
rol_implicito: ninguno
vacio: false
draft: false
order: 4
---

# Listas de Definición (dl, dt, dd)

> [!definicion]
> `<dl>` (description list) agrupa pares de **término y descripción**: `<dt>` es el término y `<dd>` su definición o valor. Es la estructura para glosarios, listas de metadatos, preguntas y respuestas, o cualquier conjunto de pares clave–valor.

```html
<dl>
  <dt>HTML</dt>
  <dd>Lenguaje de marcado para estructurar contenido web.</dd>

  <dt>CSS</dt>
  <dd>Lenguaje para dar estilo a los documentos HTML.</dd>
</dl>
```

## Los tres elementos

| Elemento | Significa | Rol |
|----------|-----------|-----|
| `<dl>` | El contenedor de la lista de descripciones | `list` (implícito en varios navegadores) |
| `<dt>` | Term: el término o nombre | term |
| `<dd>` | Description: su definición o valor | definition |

## Relaciones flexibles entre dt y dd

La asociación no es solo uno a uno; un término puede tener varias descripciones y varias claves pueden compartir descripción:

```html
<dl>
  <!-- Un término, varias descripciones -->
  <dt>Café</dt>
  <dd>Bebida caliente de granos tostados.</dd>
  <dd>Color marrón oscuro.</dd>

  <!-- Varios términos, una descripción -->
  <dt>Firefox</dt>
  <dt>Chrome</dt>
  <dd>Navegadores web.</dd>
</dl>
```

## Usos típicos más allá del glosario

| Uso | `<dt>` | `<dd>` |
|-----|--------|--------|
| Glosario | Término | Definición |
| Metadatos / ficha | Etiqueta del dato | Valor |
| FAQ | Pregunta | Respuesta |
| Diálogo / créditos | Hablante / rol | Línea / nombre |

```html
<!-- Ficha de producto -->
<dl>
  <dt>Precio</dt>   <dd>29,99 €</dd>
  <dt>Stock</dt>    <dd>En existencias</dd>
  <dt>Envío</dt>    <dd>24-48 h</dd>
</dl>
```

## Estructura con div (opcional)

HTML permite envolver cada par `<dt>`/`<dd>` en un `<div>` para facilitar el estilado (por ejemplo, con flexbox o grid por par), sin romper la semántica:

```html
<dl>
  <div>
    <dt>Precio</dt>
    <dd>29,99 €</dd>
  </div>
</dl>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<dl>` para pares clave–valor reales (glosarios, metadatos, fichas), no para cualquier lista.
> - Aprovecha las relaciones múltiples (varios `<dd>` por `<dt>` o viceversa) cuando el contenido lo pida.
> - Para maquetar por pares, envuelve cada uno en `<div>`.
> - Marca el término definido con [[16 Definiciones (dfn) | `<dfn>`]] si además es su definición canónica.

## Errores comunes

> [!warning] Trampas
> - **Usar `<dl>` para listas normales**: si no hay relación término–descripción, usa `<ul>`/`<ol>`.
> - **Texto suelto** entre `<dt>` y `<dd>` fuera de ellos: solo se permiten `<dt>`, `<dd>` (y `<div>` envolvente).
> - **Maquetar tablas de datos** con `<dl>`: para datos tabulares de filas y columnas, usa una [[06 Tablas/index | tabla]].

## Notas relacionadas

- [[02 Listas No Ordenadas (ul)]] — para listas sin pares clave–valor.
- [[16 Definiciones (dfn)]] — para marcar el término que se define.
- [[06 Tablas/index]] — para datos de filas y columnas, no pares.
