---
title: <strong> — Importancia o seriedad
aliases:
  - strong
  - strong importance
tags:
  - html
  - api/elemento
  - semantica
elemento: strong
categoria: fraseo
rol_implicito: strong
vacio: false
draft: false
---

# Énfasis Fuerte (strong)

> [!definicion]
> `<strong>` marca contenido de **gran importancia, seriedad o urgencia**: una advertencia, una
> palabra clave crítica. El navegador lo muestra en **negrita** por defecto, pero su valor es
> semántico: "esto importa".

```html
<p><strong>Aviso:</strong> no apague el equipo durante la actualización.</p>
```

## strong vs. b

| Elemento | Significado | Lector de pantalla |
|----------|-------------|--------------------|
| `<strong>` | Importancia semántica | Puede enfatizar el tono |
| [[06 Negrita sin Énfasis (b) | `<b>`]] | Solo negrita visual, sin importancia | Lee normal |

Ambos se ven en negrita. La pregunta para elegir: *¿este texto es más **importante**, o solo quiero
que **resalte**?* Si es importancia, `<strong>`; si es puro estilo, `<b>` o una clase CSS.

> [!info] La importancia se acumula
> Anidar `<strong>` dentro de `<strong>` aumenta el grado de importancia relativa del fragmento
> interno. Es válido, aunque poco frecuente.

> [!warning] No para poner en negrita por estética
> Marcar texto como `<strong>` solo "porque quiero negrita" diluye su significado y confunde a las
> tecnologías de asistencia. La negrita decorativa es `font-weight: bold` en CSS.

## Notas relacionadas

- [[05 Énfasis (em)]] — el énfasis de **tono** (cursiva), el otro elemento semántico.
- [[06 Negrita sin Énfasis (b)]] — la negrita sin importancia semántica.
