---
title: <ol> — Lista ordenada
aliases:
  - ol
  - ordered list
tags:
  - html
  - api/elemento
  - semantica
elemento: ol
categoria: flujo
rol_implicito: list
vacio: false
draft: false
order: 1
---

# Listas Ordenadas (ol)

> [!definicion]
> `<ol>` crea una lista **ordenada**: una secuencia donde el orden de los elementos importa (pasos de una receta, un ranking, instrucciones numeradas). El navegador numera automáticamente cada [[03 Elementos de Lista (li) | `<li>`]].

```html
<ol>
  <li>Precalentar el horno a 180 °C.</li>
  <li>Mezclar los ingredientes secos.</li>
  <li>Añadir los huevos y batir.</li>
</ol>
```

## Cuándo ol y no ul

El criterio es simple: *¿cambiaría el significado si reordeno los ítems?* Si sí, es `<ol>`; si no, es [[02 Listas No Ordenadas (ul) | `<ul>`]]. Una lista de pasos (1, 2, 3) sí cambia si se reordena; una lista de ingredientes sueltos, no.

## Atributos

| Atributo | Valores | Descripción |
|----------|---------|-------------|
| `type` | `1` · `a` · `A` · `i` · `I` | Estilo de numeración (números, letras, romanos) |
| `start` | número entero | Número desde el que empieza la cuenta |
| `reversed` | (booleano) | Numera en orden descendente |

### type: estilo de marcador

| `type` | Resultado |
|--------|-----------|
| `1` | 1, 2, 3 (por defecto) |
| `a` / `A` | a, b, c / A, B, C |
| `i` / `I` | i, ii, iii / I, II, III (romanos) |

```html
<ol type="a">
  <li>Primera opción</li>   <!-- a. -->
  <li>Segunda opción</li>   <!-- b. -->
</ol>
```

### start y reversed

```html
<!-- Empezar en 5 -->
<ol start="5">
  <li>Quinto paso</li>   <!-- 5. -->
  <li>Sexto paso</li>    <!-- 6. -->
</ol>

<!-- Cuenta atrás (top 3) -->
<ol reversed>
  <li>Tercera mejor</li> <!-- 3. -->
  <li>Segunda mejor</li> <!-- 2. -->
  <li>La mejor</li>      <!-- 1. -->
</ol>
```

### value en un li concreto

Un [[03 Elementos de Lista (li) | `<li>`]] individual puede fijar su número con `value`, y los siguientes continúan desde ahí:

```html
<ol>
  <li>Uno</li>
  <li value="10">Diez</li>  <!-- salta a 10 -->
  <li>Once</li>             <!-- 11 -->
</ol>
```

## Semántica vs. estilo del número

> [!info] El número es contenido, no solo CSS
> A diferencia de las viñetas de `<ul>` (decorativas), la **numeración de `<ol>` tiene significado**: el "paso 3" es el tercero. Aunque CSS puede cambiar el aspecto del marcador con `list-style-type`, el orden lo aporta el elemento. Por eso `type`/`start`/`reversed` son atributos HTML y no solo CSS: afectan al valor semántico de la cuenta.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<ol>` cuando el orden o la numeración formen parte del significado.
> - Usa `start`/`reversed`/`value` cuando la numeración real lo requiera (continuar una lista partida, un ranking inverso).
> - El estilo del marcador (color, formato) se ajusta con CSS (`list-style`, contadores), pero el orden es del HTML.

## Errores comunes

> [!warning] Trampas
> - **Usar `<ol>` para ítems sin orden** solo porque quieres números: si reordenarlos no cambia nada, es `<ul>` con `list-style` numérico si acaso.
> - **Numerar a mano** ("1. ", "2. " como texto dentro de `<li>`): deja que `<ol>` numere; así `start`/`reversed` y la accesibilidad funcionan.

## Notas relacionadas

- [[02 Listas No Ordenadas (ul)]] — para conjuntos sin orden.
- [[03 Elementos de Lista (li)]] — el ítem y su atributo `value`.
- [[05 Listas Anidadas]] — combinar listas ordenadas y no ordenadas.
