---
title: <kbd> — Entrada del usuario por teclado
aliases:
  - kbd
  - keyboard input
tags:
  - html
  - api/elemento
  - semantica
elemento: kbd
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
---

# Entrada de Teclado (kbd)

> [!definicion]
> `<kbd>` representa la **entrada del usuario**: una tecla, un atajo, un comando que la persona teclea. Se rinde en tipografía monoespaciada por defecto. Es la contraparte de [[20 Salida de Programa (samp) | `<samp>`]] (lo que el sistema devuelve).

```html
<p>Pulsa <kbd>Ctrl</kbd> + <kbd>C</kbd> para copiar.</p>
```

## Representar atajos de teclado

El uso más común es documentar combinaciones de teclas, con un `<kbd>` por tecla:

```html
<p>Guardar: <kbd>Ctrl</kbd>+<kbd>S</kbd></p>
<p>Abrir DevTools: <kbd>F12</kbd></p>
<p>Cambiar de pestaña: <kbd>Alt</kbd>+<kbd>Tab</kbd></p>
```

Anidar `<kbd>` dentro de otro `<kbd>` está contemplado para agrupar una combinación como una unidad, aunque en la práctica un `<kbd>` por tecla con `+` entre medias es lo habitual y más legible.

## Estilo de "tecla"

`<kbd>` se suele estilar para que parezca una tecla física, lo que mejora mucho la legibilidad de los atajos:

```css
kbd {
  font-family: monospace;
  background: #313244;
  border: 1px solid #45475a;
  border-radius: 0.3em;
  padding: 0.1em 0.4em;
  box-shadow: 0 1px 0 #11111b;
}
```

## El grupo entrada-salida-código

| Elemento | Representa |
|----------|------------|
| `<kbd>` | Lo que el usuario **teclea** |
| `<samp>` | Lo que el sistema **devuelve** |
| `<code>` | **Código fuente** literal |
| `<var>` | Una **variable** o marcador |

Los cuatro son monoespaciados pero distintos; ver [[17 Código (code) | code]], [[19 Variable (var) | var]] y [[20 Salida de Programa (samp) | samp]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Un `<kbd>` por tecla; une combinaciones con `+`.
> - Estílalo como tecla física para que los atajos se lean de un vistazo.
> - Úsalo también para comandos que el usuario escribe (`<kbd>npm start</kbd>`), no solo teclas.

## Errores comunes

> [!warning] kbd para salida o código
> Confundir `<kbd>` (entrada del usuario) con `<samp>` (salida del sistema) o `<code>` (código fuente) invierte el significado. Si el texto es lo que la persona **pulsa o escribe**, es `<kbd>`; si es lo que la máquina responde, es `<samp>`.

## Notas relacionadas

- [[20 Salida de Programa (samp)]] — la contraparte: la salida del sistema.
- [[17 Código (code)]] — código fuente, no entrada del usuario.
