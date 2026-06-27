---
title: <strong> — Importancia, seriedad o urgencia
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
order: 4
---

# Énfasis Fuerte (strong)

> [!definicion]
> `<strong>` marca contenido de **gran importancia, seriedad o urgencia**: una advertencia, una palabra clave crítica, una instrucción que no debe pasarse por alto. El navegador lo muestra en **negrita** por defecto, pero su valor es semántico: declara "esto importa", no "esto va en negrita".

```html
<p><strong>Aviso:</strong> no apague el equipo durante la actualización.</p>
```

## strong vs. b: el par que más se confunde

Ambos se renderizan en negrita, así que se confunden constantemente. La diferencia es de significado y de comportamiento ante la asistencia:

| Elemento | Significado | Lector de pantalla |
|----------|-------------|--------------------|
| `<strong>` | Importancia semántica real | Puede enfatizar el tono al leerlo |
| `<b>` | Solo negrita visual, sin importancia | Lo lee con voz normal |

La pregunta para decidir: *¿este texto es más **importante** que el resto, o solo quiero que **resalte** visualmente?* Si es importancia, `<strong>`; si es puro estilo, [[06 Negrita sin Énfasis (b) | `<b>`]] o, mejor, una clase CSS.

## La importancia se acumula

Anidar `<strong>` dentro de `<strong>` **aumenta el grado de importancia relativa** del fragmento interno. Es válido, aunque poco frecuente:

```html
<p><strong>Importante: lea <strong>todas</strong> las instrucciones.</strong></p>
```

Aquí "todas" es lo más importante dentro de un bloque ya importante.

## Ejemplos en contexto

```html
<!-- Advertencia de seguridad -->
<p><strong>No comparta su contraseña</strong> con nadie.</p>

<!-- Palabra clave crítica en una explicación -->
<p>Para cancelar, debe hacerlo <strong>antes de 24 horas</strong>.</p>

<!-- Etiqueta de urgencia -->
<p><strong>Última hora:</strong> se ha pospuesto el evento.</p>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Reserva `<strong>` para contenido que de verdad es más importante, no para cualquier negrita.
> - Si necesitas negrita por diseño (un nombre de producto, una palabra resaltada sin más peso), usa `<b>` o `font-weight` en CSS.
> - No abuses: si todo es importante, nada lo es. Un par de `<strong>` por bloque destacan; veinte se vuelven ruido.

## Errores comunes

> [!warning] strong por estética
> Marcar texto como `<strong>` solo "porque quiero negrita" diluye su significado y confunde a las tecnologías de asistencia, que pueden enfatizarlo al leer. La negrita puramente decorativa es `font-weight: bold` en CSS o, si necesitas un elemento, `<b>`. La regla: `<strong>` comunica importancia; el grosor de la letra es otra cosa.

## Notas relacionadas

- [[05 Énfasis (em)]] — el énfasis de **tono** (cursiva), su pareja semántica.
- [[06 Negrita sin Énfasis (b)]] — la negrita sin importancia semántica.
- [[12 Texto Resaltado (mark)]] — para relevancia contextual, no importancia intrínseca.
