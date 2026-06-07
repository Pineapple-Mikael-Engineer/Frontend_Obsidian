---
title: La Cascada CSS
aliases:
  - cascada css
  - cascade
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# La Cascada CSS

> [!definicion]
> La **cascada** es el algoritmo que el navegador usa para decidir qué declaración aplica cuando varias reglas compiten por el mismo elemento y propiedad. No es solo "la última gana": el algoritmo evalúa cuatro factores en orden, y solo pasa al siguiente cuando el anterior produce un empate.

## Los cuatro factores de la cascada (en orden)

1. **Origen + Importancia** — ¿De dónde viene la declaración y lleva `!important`?
2. **Capas (`@layer`)** — Dentro del mismo origen, ¿en qué capa está?
3. **Especificidad** — ¿Qué selector es más específico?
4. **Orden de aparición** — A igual todo lo anterior, ¿qué declaración viene después?

Entender este orden permite predecir el resultado sin necesidad de experimentar.

## Diagrama de resolución

```
¿Tienen el mismo origen e importancia?
    No → gana el de mayor precedencia de origen/importancia
    Sí → ¿Están en capas @layer distintas?
             No → ¿Cuál tiene más especificidad?
                      No hay empate → gana el más específico
                      Empate → gana el que aparece después (orden de origen)
             Sí → gana el de la capa con más prioridad
```

## Por qué se llama "cascada"

La metáfora es una cascada de agua: las declaraciones "caen" desde múltiples fuentes (UA, usuario, autor) y, cuando se solapan, solo la de mayor prioridad llega al fondo (al elemento). El resto se descarta.

## Las capas del origen

Los estilos provienen de tres orígenes distintos, cada uno con su propia prioridad base:

- **User-Agent (UA)**: los estilos por defecto del navegador (`<h1>` en negrita, `<a>` en azul).
- **Usuario**: estilos aplicados por el usuario del navegador (hoja de estilos de accesibilidad, modo de alto contraste).
- **Autor**: los estilos que escribe el desarrollador web.

En situación normal (sin `!important`), los estilos de autor sobrescriben a los del usuario, que sobrescriben a los del UA. Con `!important`, la prioridad se invierte en los dos primeros niveles (ver nota de `!important`).

## El papel de @layer en la cascada

`@layer` introduce un nivel adicional entre especificidad y orden de origen: las capas se apilan y las capas declaradas **después** tienen prioridad dentro de los estilos normales. Esto permite que un selector de baja especificidad en una capa posterior gane a uno de alta especificidad en una capa anterior, sin tocar `!important`.

## Notas relacionadas

- [[01 Orden de Origen]] — el cuarto factor: lo que viene después gana.
- [[02 Importancia (!important)]] — el factor que invierte el orden natural.
- [[03 Orígenes de Estilo (UA, usuario, autor)]] — las tres fuentes de estilos.
- [[03 Capas (@layer)/index]] — el segundo factor: capas explícitas.
- [[01 Especificidad/index]] — el tercer factor: peso del selector.
