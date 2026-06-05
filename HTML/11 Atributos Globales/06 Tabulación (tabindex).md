---
title: tabindex (global) — Control del foco por teclado
aliases:
  - tabindex global
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Tabulación (tabindex)

> [!definicion]
> `tabindex` controla si un elemento es **enfocable** por teclado y en qué orden. Como atributo global, se puede poner en cualquier elemento. Sus valores útiles son `0` (hacer enfocable en orden natural) y `-1` (enfocable solo por JavaScript); los positivos se evitan. Su papel en la accesibilidad se desarrolla en [[01 tabindex | la nota de tabindex (A11Y)]].

```html
<div tabindex="0" role="button">Enfocable por teclado</div>
<section tabindex="-1" id="destino">Enfocable solo por JS</section>
```

## Resumen de valores

| `tabindex` | Efecto |
|------------|--------|
| `0` | Enfocable con `Tab`, en el orden del DOM |
| `-1` | Enfocable solo programáticamente (`.focus()`), no con `Tab` |
| positivo | Fuerza un orden explícito — **antipatrón, no usar** |

## Cuándo usarlo

- **`0`**: dar foco a un elemento normalmente no enfocable que actúa como control (un widget a medida). Innecesario en botones/enlaces, que ya son enfocables.
- **`-1`**: enfocar por código una zona tras una acción (un [[03 Skip Links | skip link]] hacia `<main tabindex="-1">`, mover el foco a un mensaje).

## Por qué no positivos

> [!warning] tabindex positivo rompe el orden
> Un `tabindex` mayor que 0 salta al frente del orden de tabulación, antes que el flujo natural, creando una secuencia de foco confusa e inmantenible. Si el orden está mal, **reordena el HTML**. El desarrollo completo está en [[01 tabindex | tabindex (Navegación por teclado)]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Solo `0` y `-1`.
> - No lo añadas a elementos ya enfocables (botones, enlaces, campos).
> - Un elemento con `tabindex="0"` y rol de widget necesita también manejo de teclado.

## Errores comunes

> [!warning] Trampas
> - **Valores positivos**: rompen la navegación.
> - **`tabindex="0"` redundante** en controles nativos.
> - **Hacer enfocable** un elemento sin rol ni teclado.

## Notas relacionadas

- [[01 tabindex]] — el desarrollo completo desde la accesibilidad.
- [[02 Gestión de Foco (focus, blur)]] — mover el foco con JavaScript.
- [[07 Tecla de Acceso (accesskey)]] — otro atributo de teclado.
