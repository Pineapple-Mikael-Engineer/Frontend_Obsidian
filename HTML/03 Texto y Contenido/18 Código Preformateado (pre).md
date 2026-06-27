---
title: <pre> — Texto preformateado
aliases:
  - pre
  - preformatted text
tags:
  - html
  - api/elemento
  - semantica
elemento: pre
categoria: flujo
rol_implicito: ninguno
vacio: false
draft: false
order: 18
---

# Código Preformateado (pre)

> [!definicion]
> `<pre>` muestra el texto **tal cual está escrito**: conserva espacios, tabulaciones y saltos de línea en lugar de colapsarlos como hace el HTML normal. Se rinde en tipografía monoespaciada. Es el contenedor estándar de bloques de código de varias líneas.

```html
<pre><code>function saludar(nombre) {
  return `Hola, ${nombre}`;
}</code></pre>
```

## Por qué pre + code juntos

`<pre>` y [[17 Código (code) | `<code>`]] resuelven problemas distintos y complementarios:

| Elemento | Qué aporta |
|----------|------------|
| `<pre>` | Preserva el **formato** (espacios, tabulaciones, saltos de línea) |
| `<code>` | Aporta el **significado** semántico (esto es código fuente) |

Para un bloque de código se combinan: `<pre><code>…</code></pre>`. `<pre>` por sí solo preserva el formato pero no dice que sea código; `<code>` por sí solo da semántica pero no preserva los saltos.

## La indentación del HTML se ve

> [!warning] Cuidado con la sangría del archivo
> Como `<pre>` respeta todos los espacios, la **sangría del propio archivo HTML** aparece en pantalla:
> ```html
> <div>
>     <pre><code>línea 1
>     línea 2</code></pre>   <!-- esta indentación SE VE -->
> </div>
> ```
> Por eso el contenido de un `<pre>` suele escribirse **pegado al margen izquierdo** del archivo, rompiendo la indentación visual del código fuente, para que no aparezcan espacios sobrantes al renderizar.

## Escapar caracteres especiales

Igual que en `<code>`, los caracteres `<`, `>` y `&` deben escaparse como entidades (`&lt;`, `&gt;`, `&amp;`) o el navegador los interpreta como etiquetas. Mostrar un bloque con `<div>` requiere `&lt;div&gt;`.

## Otros usos más allá del código

`<pre>` sirve para cualquier texto donde el **espaciado es el contenido**:

```html
<!-- Arte ASCII -->
<pre>
  /\_/\
 ( o.o )
  > ^ <
</pre>

<!-- Salida de terminal -->
<pre>$ ls -la
total 8
drwxr-xr-x  2 user  staff  64 jun  4 12:00 .</pre>
```

## Estilo

Conviene permitir el desplazamiento horizontal para líneas largas, en lugar de que rompan el layout:

```css
pre {
  overflow-x: auto;
  padding: 1rem;
  background: #11111b;
  border-radius: 0.5rem;
}
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Combina siempre `<pre>` con `<code>` para bloques de código.
> - Escribe el contenido pegado al margen para no heredar la sangría del archivo.
> - Escapa `<`, `>`, `&`.
> - Añade `overflow-x: auto` en CSS para líneas largas.

## Errores comunes

> [!warning] Trampas
> - **Indentar el contenido** dentro del HTML: esos espacios se ven.
> - **`<pre>` sin `<code>`** para código: pierde la semántica (válido para arte ASCII o salida, no para código).
> - **No escapar** los caracteres especiales.

## Notas relacionadas

- [[17 Código (code)]] — se anida dentro para el significado semántico.
- [[20 Salida de Programa (samp)]] — para representar salida de programa.
