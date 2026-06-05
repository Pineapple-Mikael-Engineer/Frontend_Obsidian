---
title: <meta robots> — Control de indexación
aliases:
  - meta robots
  - noindex nofollow
tags:
  - html
  - api/elemento
  - metadatos
elemento: meta
categoria: metadatos
rol_implicito: ninguno
vacio: true
draft: false
---

# Robots (meta robots)

> [!definicion]
> `<meta name="robots">` indica a los buscadores **si pueden indexar** la página y **seguir sus enlaces**. Es el control fino de la indexación a nivel de página, complementario al archivo `robots.txt` (que actúa a nivel de sitio).

```html
<meta name="robots" content="index, follow" />
```

## Directivas principales

| Directiva | Efecto |
|-----------|--------|
| `index` / `noindex` | Permite / impide indexar la página |
| `follow` / `nofollow` | Permite / impide seguir los enlaces de la página |
| `noarchive` | No guardar copia en caché |
| `nosnippet` | No mostrar fragmento en los resultados |
| `max-snippet:n` | Limitar la longitud del fragmento |

El valor por defecto (sin la etiqueta) es `index, follow`: indexar y seguir. Solo hace falta declararla para **restringir**.

## El caso clave: noindex

> [!info] Cuándo usar noindex
> `noindex` saca una página de los resultados de búsqueda. Útil para:
> - Páginas de agradecimiento ("gracias por tu compra").
> - Resultados de búsqueda interna (contenido duplicado/baja calidad).
> - Páginas de administración o privadas.
> - Versiones de prueba o staging.
> ```html
> <meta name="robots" content="noindex, follow" />
> ```
> `noindex, follow` saca la página del índice pero deja que el rastreador siga sus enlaces (para descubrir otras páginas).

## robots.txt vs. meta robots vs. noindex

> [!warning] No bloquees en robots.txt lo que quieres desindexar
> Hay una confusión peligrosa:
> - **`robots.txt`** (archivo): impide **rastrear** (que el bot visite la URL).
> - **`meta robots noindex`**: impide **indexar** (que aparezca en resultados).
>
> Si bloqueas una URL en `robots.txt`, Google **no puede leer** su `<meta noindex>`, así que podría indexarla igualmente (sin contenido, solo la URL). Para desindexar de verdad: **permite** el rastreo y usa `noindex`.

## Dirigir a buscadores concretos

Se puede dirigir a un bot específico cambiando el `name`:

```html
<meta name="googlebot" content="noindex" />   <!-- solo Google -->
```

## Buenas prácticas

> [!tip] Recomendaciones
> - No declares `robots` si quieres el comportamiento por defecto (index, follow).
> - Usa `noindex` para páginas que no deben aparecer en búsquedas.
> - Para desindexar, **no** bloquees en robots.txt: deja rastrear y pon `noindex`.
> - Combina con [[11 Enlace Canónico (link rel canonical) | canonical]] para gestionar duplicados.

## Errores comunes

> [!warning] Trampas
> - **`noindex` en producción por error** (heredado de staging): desindexa el sitio.
> - **Bloquear en robots.txt** una URL que se quiere desindexar: impide leer el `noindex`.
> - **Confundir rastreo con indexación**: son cosas distintas.

## Notas relacionadas

- [[11 Enlace Canónico (link rel canonical)]] — para duplicados, complementa a robots.
- [[03 Verificación de Sitio]] — otro uso de `<meta>` para buscadores.
- [[index]] — el resto de metadatos estándar.
