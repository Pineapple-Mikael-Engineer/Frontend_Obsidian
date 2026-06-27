---
title: active — Mientras se pulsa
aliases:
  - ":active"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":active"
grupo: pseudoclase
draft: false
order: 2
---

# :active

> [!definicion]
> `:active` selecciona un elemento mientras se está **pulsando** (entre que se presiona y se suelta el ratón/dedo). Da el feedback de "presionado" que hace que un botón se sienta físico: se hunde, se oscurece, se encoge ligeramente.

```css
.boton:active { transform: scale(0.97); }
```

## El feedback de "presionado"

> [!tip] Hacer que un botón se sienta físico
> `:active` simula la respuesta táctil de un botón real que se hunde al pulsarlo. Un ligero `scale` o un cambio de sombra basta para que la interacción se sienta sólida:
> ```css
> .btn {
>   transition: transform 0.1s;
> }
> .btn:active {
>   transform: scale(0.97) translateY(1px);   /* se hunde un poco */
> }
> ```
> La transición debe ser **muy corta** (50–100ms): el feedback de pulsación debe ser inmediato.

## Funciona en táctil

> [!info] :active sí responde al toque
> A diferencia de [[01 hover | `:hover`]], `:active` **sí** funciona en táctil: se activa durante el toque. Por eso es el feedback de pulsación más fiable en móvil (el hover no sirve ahí). Es la pseudoclase para confirmar visualmente que un toque se registró.

## El orden LVHA

> [!warning] :active va el último en los enlaces
> En la secuencia [[index | LVHA]] de los enlaces, `:active` debe ir el **último** para que se vea al pulsar (si fuera antes que `:hover`, el hover lo pisaría):
> ```css
> a:link { } a:visited { } a:hover { } a:active { }   /* :active al final */
> ```

## active dura poco

`:active` solo está activo **mientras se mantiene pulsado**, así que el efecto es momentáneo. No sirve para estados persistentes (para eso, una clase con JS o `:focus`). Es puro feedback instantáneo de la pulsación.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:active` para feedback de "presionado" (scale, hundimiento).
> - Transición muy corta (50–100ms): el feedback debe ser inmediato.
> - Es el feedback de pulsación fiable en táctil (donde el hover falla).
> - En enlaces, decláralo el último (orden LVHA).

## Errores comunes

> [!warning] Trampas
> - **Transición lenta** en `:active`: el feedback se siente retardado.
> - **`:active` antes de `:hover`** en enlaces: el hover lo pisa.
> - **Esperar persistencia**: `:active` solo dura mientras se pulsa.

## Notas relacionadas

- [[01 hover]] — el estado previo (ratón encima).
- [[03 focus]] — el estado tras pulsar (foco).
- [[01 Transformaciones 2D (translate, rotate, scale, skew)]] — `scale` para el hundimiento.
