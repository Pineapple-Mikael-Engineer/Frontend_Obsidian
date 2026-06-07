---
title: transition-duration — Cuánto dura la transición
aliases:
  - transition-duration
tags:
  - css
  - api/propiedad
  - animacion
propiedad: transition-duration
grupo: animacion
valor_inicial: "0s"
hereda: false
animable: false
draft: false
---

# transition-duration

> [!definicion]
> `transition-duration` define **cuánto tiempo** tarda la transición de un estado a otro, en segundos (`s`) o milisegundos (`ms`). Es la única parte **obligatoria** del shorthand `transition`: sin duración, no hay transición visible (cambio instantáneo).

```css
.x { transition-duration: 0.2s; }   /* o 200ms */
```

## La duración es obligatoria

> [!warning] Sin duración, no hay animación
> El valor por defecto es `0s` (instantáneo). Por eso una `transition` sin duración **no anima**: el cambio es brusco. La duración debe ser distinta de cero:
> ```css
> .x { transition: background; }        /* ❌ 0s por defecto: cambio instantáneo */
> .x { transition: background 0.2s; }   /* ✅ */
> ```

## s vs. ms

`0.2s` y `200ms` son equivalentes. La convención varía; ambos son válidos:

```css
transition-duration: 0.3s;
transition-duration: 300ms;
```

## Cuánto dura una buena transición

> [!tip] Las duraciones que se sienten naturales
> Hay un rango de duraciones que se perciben **naturales** para la interfaz:
> | Duración | Sensación | Uso |
> |----------|-----------|-----|
> | 100–150ms | Muy rápida | Feedback inmediato (hover de botón, active) |
> | 200–300ms | Equilibrada | La mayoría de transiciones de UI |
> | 300–500ms | Notable | Entradas/salidas de paneles, modales |
> | > 500ms | Lenta | Solo efectos deliberados; arriesgado |
>
> Por debajo de ~100ms apenas se nota; por encima de ~500ms se siente **lento** y entorpece. La mayoría de las microinteracciones viven en 150–300ms.

## Duraciones distintas por propiedad

Con varias propiedades, cada una puede tener su duración (entrada y salida asimétricas son comunes):

```css
.menu {
  transition: opacity 0.2s, transform 0.3s;
}
```

A veces se quiere que la **salida** sea más rápida que la entrada, definiendo distintas duraciones en el estado base y el activo.

## Buenas prácticas

> [!tip] Recomendaciones
> - 150–300ms para la mayoría de las transiciones de UI.
> - Más cortas (100–150ms) para feedback inmediato (hover, active).
> - Evita duraciones largas (> 500ms) salvo efectos deliberados.
> - Define duraciones en variables (`--dur-rapida: 0.15s`) para coherencia.

## Errores comunes

> [!warning] Trampas
> - **Olvidar la duración**: el cambio es instantáneo (0s por defecto).
> - **Transiciones demasiado lentas**: la interfaz se siente perezosa.
> - **Misma duración para todo** cuando entrada/salida deberían diferir.

## Notas relacionadas

- [[03 transition-timing-function]] — la curva que acompaña a la duración.
- [[04 transition-delay]] — el retardo antes de empezar.
- [[index]] — el shorthand completo.
