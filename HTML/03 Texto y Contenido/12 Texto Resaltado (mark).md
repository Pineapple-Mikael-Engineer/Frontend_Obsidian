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
order: 12
---

# Texto Resaltado (mark)

> [!definicion]
> `<mark>` resalta texto **relevante en el contexto actual**, como un rotulador fluorescente sobre el papel. Su caso canónico: marcar los términos buscados dentro de un resultado. El navegador lo muestra con fondo amarillo por defecto. La relevancia es contextual, no intrínseca.

```html
<p>Resultados para "agua": El <mark>agua</mark> cubre el 71 % de la Tierra.</p>
```

## Usos típicos

| Uso | Contexto |
|-----|----------|
| Coincidencias de búsqueda | Resaltar el término buscado dentro del texto encontrado |
| Pasaje relevante para el lector | Destacar la frase clave de una cita citada por un tercero |
| Referencia desde otro punto | "Nótese la línea resaltada" |
| Cambio reciente destacado | Señalar lo nuevo en una página de novedades |

## Relevancia contextual, no importancia

> [!info] La diferencia con strong
> `<mark>` no dice que el texto sea importante **en sí mismo** (eso es [[04 Énfasis Fuerte (strong) | `<strong>`]]), sino que es **pertinente ahora**, por una búsqueda, una referencia o el contexto desde el que se lee. El mismo texto puede estar resaltado para un lector (que buscó esa palabra) y no para otro. La importancia de `<strong>` es del autor; la relevancia de `<mark>` es del contexto de lectura.

## Ejemplo: resaltar una búsqueda

```html
<!-- El usuario buscó "reciclaje" -->
<article>
  <p>El <mark>reciclaje</mark> de plástico reduce la contaminación.
     Programas de <mark>reciclaje</mark> existen en la mayoría de ciudades.</p>
</article>
```

Este resaltado normalmente lo inserta JavaScript al procesar la búsqueda, envolviendo las coincidencias en `<mark>`.

## Estilo personalizable

El amarillo por defecto se cambia con CSS. Conviene mantener **buen contraste** entre el texto y el fondo del resaltado por accesibilidad:

```css
mark {
  background: #fef08a;
  color: #1e1e2e;   /* contraste alto sobre el resaltado */
  padding: 0 0.15em;
  border-radius: 0.2em;
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<mark>` para relevancia contextual (búsquedas, referencias), no para importancia general.
> - Asegura contraste suficiente entre el texto y el color de fondo del resaltado.
> - Si resaltas dinámicamente coincidencias, envuelve solo el fragmento exacto en `<mark>`.

## Errores comunes

> [!warning] mark como rotulador permanente
> Resaltar con `<mark>` texto que es importante para todos los lectores (no por un contexto puntual) confunde su significado: para importancia intrínseca está `<strong>`. Otro descuido: usar un amarillo con texto claro encima, dejando el resaltado ilegible.

## Notas relacionadas

- [[04 Énfasis Fuerte (strong)]] — importancia intrínseca, no contextual.
- [[09 Subrayado (u)]] — otra forma de destacar, de uso mucho más limitado.
