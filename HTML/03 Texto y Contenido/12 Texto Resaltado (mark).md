---
title: <mark> — Texto resaltado por relevancia
aliases:
  - mark
  - highlight
tags:
  - html
  - api/elemento
  - semantica
elemento: mark
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Texto Resaltado (mark)

> [!definicion]
> `<mark>` resalta texto **relevante en el contexto actual**, como el rotulador fluorescente sobre
> un papel. El caso canónico: marcar los términos buscados dentro de un resultado. El navegador lo
> muestra con fondo amarillo por defecto.

```html
<p>Resultados para "agua":
   El <mark>agua</mark> cubre el 71 % de la Tierra.</p>
```

## Usos típicos

| Uso | Contexto |
|-----|----------|
| Coincidencias de búsqueda | Resaltar el término buscado en el texto |
| Pasaje relevante para el lector | Destacar una frase clave en una cita |
| Referencia desde otro punto | "Nótese la línea marcada" |

> [!info] Relevancia contextual, no importancia
> `<mark>` no dice que el texto sea importante en sí (eso es
> [[04 Énfasis Fuerte (strong) | `<strong>`]]), sino que es **pertinente ahora**, por una búsqueda o
> referencia externa. El resaltado puede tener sentido para un lector y no para otro.

> [!tip] Estilo personalizable
> El amarillo por defecto se cambia con CSS (`mark { background: #fef08a; color: inherit; }`).
> Mantener buen contraste entre el texto y el fondo del resaltado por accesibilidad.

## Notas relacionadas

- [[04 Énfasis Fuerte (strong)]] — importancia intrínseca, no contextual.
- [[index]] — el resto del marcado de texto.
