---
title: "Definición de Fotogramas — La regla @keyframes"
aliases:
  - keyframes definition
tags:
  - css
  - api/concepto
  - animacion
draft: false
---

# Definición de Fotogramas

> [!definicion]
> La regla `@keyframes` define los **fotogramas** de una animación: los estados por los que pasa el elemento, expresados como **porcentajes** del 0% (inicio) al 100% (fin). Entre fotogramas, el navegador **interpola** los valores. Una vez definida, se aplica con [[02 animation-name y duration | `animation-name`]].

```css
@keyframes deslizar {
  0%   { transform: translateX(-100%); opacity: 0; }
  100% { transform: translateX(0);     opacity: 1; }
}
```

## La sintaxis

```css
@keyframes <nombre> {
  <punto>% { /* estado en ese momento */ }
  <punto>% { /* otro estado */ }
}
```

| Punto | Significa |
|-------|-----------|
| `0%` o `from` | El inicio de la animación |
| `100%` o `to` | El final |
| `50%`, `25%`… | Puntos intermedios |

## from/to vs. porcentajes

Para animaciones de **dos** estados, `from`/`to` son atajos de `0%`/`100%`:

```css
/* Equivalentes */
@keyframes girar { from { transform: rotate(0); } to { transform: rotate(360deg); } }
@keyframes girar { 0% { transform: rotate(0); } 100% { transform: rotate(360deg); } }
```

Para **varios** pasos, se usan porcentajes:

```css
@keyframes saltar {
  0%, 100% { transform: translateY(0); }     /* mismo estado al inicio y al final */
  50%      { transform: translateY(-20px); } /* arriba a mitad */
}
```

## Agrupar fotogramas con el mismo estado

> [!tip] Varios porcentajes en un fotograma
> Se pueden agrupar puntos que comparten el mismo estado, separados por comas. Útil para "pausas" o estados repetidos:
> ```css
> @keyframes parpadeo {
>   0%, 50%, 100% { opacity: 1; }   /* visible en esos momentos */
>   25%, 75%      { opacity: 0; }   /* oculto en esos */
> }
> ```

## La interpolación entre fotogramas

> [!info] El navegador rellena los huecos
> Solo defines **estados clave**; el navegador **interpola** los valores intermedios. En `0% { scale(1) }` y `100% { scale(2) }`, el navegador calcula todos los tamaños entre 1 y 2 a lo largo de la duración. Solo se interpolan [[05 Propiedades Animables | propiedades animables]]; cuanto más distantes los fotogramas, más interpolación.

## Una propiedad puede entrar a mitad

Si una propiedad solo aparece en algunos fotogramas, fuera de ellos toma su valor normal. Conviene definir el estado completo en los fotogramas clave para evitar saltos inesperados.

## Nombrar y reutilizar

> [!tip] Una @keyframes, varios usos
> La `@keyframes` se define **una vez** con un nombre y se puede **aplicar a muchos elementos** con distintas duraciones y curvas:
> ```css
> @keyframes aparecer { from { opacity: 0; } to { opacity: 1; } }
> .titulo { animation: aparecer 0.5s; }
> .texto  { animation: aparecer 0.8s 0.2s; }   /* misma animación, otro tiempo/delay */
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - `from`/`to` para dos estados; porcentajes para varios pasos.
> - Agrupa porcentajes con el mismo estado (`0%, 100%`).
> - Define el estado completo en los fotogramas clave para evitar saltos.
> - Anima `transform`/`opacity` dentro de los keyframes por rendimiento.
> - Nombra las animaciones de forma descriptiva (`deslizar`, `pulso`) y reutilízalas.

## Errores comunes

> [!warning] Trampas
> - **Animar propiedades no animables** dentro de los keyframes.
> - **Saltos** por no definir una propiedad en todos los fotogramas clave.
> - **Definir `@keyframes` pero olvidar `animation-name`**: no se aplica.

## Notas relacionadas

- [[02 animation-name y duration]] — aplicar la animación definida.
- [[05 Propiedades Animables]] — qué se puede interpolar.
- [[04 animation-iteration-count]] — repetir la secuencia.
