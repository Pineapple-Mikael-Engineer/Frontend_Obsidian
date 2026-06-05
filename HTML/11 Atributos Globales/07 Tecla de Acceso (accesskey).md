---
title: accesskey — Atajo de teclado para un elemento
aliases:
  - accesskey
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

# Tecla de Acceso (accesskey)

> [!definicion]
> `accesskey` asigna un **atajo de teclado** a un elemento (un enlace, un botón, un campo): al pulsar la combinación, el elemento recibe el foco o se activa. La intención es buena, pero en la práctica `accesskey` es **problemático** y rara vez se recomienda.

```html
<button accesskey="g">Guardar</button>
<!-- se activa con Alt+G (o la combinación según navegador/SO) -->
```

## La combinación depende del sistema

| Navegador / SO | Combinación |
|----------------|-------------|
| Firefox (Windows/Linux) | `Alt + Shift + tecla` |
| Chrome (Windows/Linux) | `Alt + tecla` |
| macOS | `Ctrl + Opt + tecla` |
| Internet Explorer | `Alt + tecla` |

Esta **inconsistencia** es ya un primer problema: el usuario no sabe qué combinación usar, y no hay forma estándar de descubrir qué atajos existen.

## Por qué se desaconseja

> [!warning] accesskey choca con atajos existentes
> Los problemas son serios:
> - **Conflictos**: la tecla elegida puede chocar con un atajo del **navegador**, del **sistema operativo** o del **lector de pantalla**, anulándolo o causando comportamientos inesperados.
> - **No descubrible**: nada indica al usuario qué atajos hay (salvo que se documenten aparte).
> - **Inconsistente** entre plataformas.
> - **Problemático para lectores de pantalla**, que usan muchas teclas con modificadores.
>
> Por todo esto, la mayoría de guías de accesibilidad **recomiendan evitarlo**.

## Alternativas

Para atajos de teclado en una aplicación web, es preferible implementarlos con JavaScript (escuchando `keydown`), eligiendo combinaciones que no choquen, **documentándolas** visiblemente y permitiendo personalizarlas. Así se controla el conflicto y la descubribilidad, cosa que `accesskey` no permite.

## Buenas prácticas

> [!tip] Recomendaciones
> - En general, **evita `accesskey`**.
> - Si lo usas, elige teclas que no choquen con atajos comunes y **documéntalas**.
> - Para atajos reales en una app, impleméntalos con JS, con combinaciones seguras y visibles.

## Errores comunes

> [!warning] Trampas
> - **Chocar con atajos** del navegador, SO o lector de pantalla.
> - **No documentar** los atajos: nadie los descubre.
> - **Asumir una combinación fija**: varía por plataforma.

## Notas relacionadas

- [[06 Tabulación (tabindex)]] — el otro atributo de teclado.
- [[03 Navegación por Teclado/index]] — la navegación por teclado en general.
