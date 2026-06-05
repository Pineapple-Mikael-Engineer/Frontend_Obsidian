---
title: <samp> — Salida de un programa
aliases:
  - samp
  - sample output
tags:
  - html
  - api/elemento
  - semantica
elemento: samp
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Salida de Programa (samp)

> [!definicion]
> `<samp>` representa la **salida** de un programa o sistema informático: un mensaje de error, el resultado de un comando, lo que la pantalla devuelve. Se rinde en tipografía monoespaciada por defecto. Es la contraparte de [[21 Entrada de Teclado (kbd) | `<kbd>`]] (lo que el usuario escribe).

```html
<p>El programa respondió: <samp>Archivo no encontrado.</samp></p>
```

## El ciclo entrada-salida

`<kbd>` y `<samp>` modelan los dos lados de la interacción con un sistema:

| Elemento | Lado | Ejemplo |
|----------|------|---------|
| `<kbd>` | Lo que el **usuario introduce** | <kbd>ls -la</kbd> |
| `<samp>` | Lo que el **sistema devuelve** | <samp>total 8</samp> |

```html
<p>Escribe <kbd>whoami</kbd> y el sistema responde <samp>noelia</samp>.</p>
```

## samp en bloque

Para salidas de varias líneas (un volcado de terminal completo), se combina con [[18 Código Preformateado (pre) | `<pre>`]] para preservar el formato:

```html
<pre><samp>$ npm test
✓ suma dos números
✗ resta correctamente
1 failing</samp></pre>
```

## kbd anidado dentro de samp

La especificación contempla un matiz fino: un `<kbd>` dentro de un `<samp>` representa una entrada **mostrada por el sistema** (p. ej., el sistema indica qué tecla pulsar). Es un caso poco frecuente pero ilustra que estos elementos se componen.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<samp>` para mensajes y resultados que produce el sistema.
> - Para salidas multilínea, envuélvelo en `<pre>`.
> - Diferéncialo de `<code>` (código fuente) y `<kbd>` (entrada del usuario): los tres son monoespaciados pero significan cosas distintas.

## Errores comunes

> [!warning] samp como monoespaciado genérico
> Los cuatro elementos técnicos (`code`, `var`, `samp`, `kbd`) se ven monoespaciados, así que es tentador usar cualquiera. Pero significan cosas distintas: `<samp>` es **salida del sistema**, no código fuente (`<code>`) ni entrada del usuario (`<kbd>`). Elegir el correcto mantiene la precisión semántica.

## Notas relacionadas

- [[21 Entrada de Teclado (kbd)]] — la contraparte: la entrada del usuario.
- [[17 Código (code)]] — código fuente, no salida.
- [[18 Código Preformateado (pre)]] — para salidas de varias líneas.
