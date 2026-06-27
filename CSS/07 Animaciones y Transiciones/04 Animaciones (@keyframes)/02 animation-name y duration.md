---
title: animation-name y animation-duration — Qué animación y cuánto dura
aliases:
  - animation-name
  - animation-duration
tags:
  - css
  - api/propiedad
  - animacion
propiedad: animation-name
grupo: animacion
valor_inicial: none
hereda: false
animable: false
draft: false
order: 2
---

# animation-name y animation-duration

> [!definicion]
> `animation-name` indica **qué** [[01 Definición de Fotogramas | `@keyframes`]] aplicar a un elemento, y `animation-duration` **cuánto** dura un ciclo completo de la animación. Son las dos propiedades **esenciales**: sin nombre no hay animación, y sin duración (≠ 0) no se ve.

```css
.spinner {
  animation-name: girar;
  animation-duration: 1s;
}
```

## Las dos imprescindibles

> [!warning] Sin nombre y duración, no hay animación
> - **`animation-name`** debe coincidir **exactamente** con el nombre de una `@keyframes` definida. Si no coincide (o falta), no pasa nada.
> - **`animation-duration`** vale `0s` por defecto, así que sin ella la animación es instantánea (no se ve).
> ```css
> .x { animation-name: pulso; }                       /* ❌ falta duración: 0s */
> .x { animation-name: pulso; animation-duration: 1s; } /* ✅ */
> ```

## El shorthand junta lo demás

En el shorthand `animation`, name y duration suelen ir primero:

```css
animation: girar 1s linear infinite;
/*         name  dur  timing count */
```

## Conectar con @keyframes

`animation-name` es el puente entre la definición y la aplicación:

```css
@keyframes deslizar { from { transform: translateX(-100%); } to { transform: translateX(0); } }

.panel {
  animation-name: deslizar;     /* ← debe coincidir con el nombre de @keyframes */
  animation-duration: 0.4s;
}
```

## Duraciones según el tipo

| Animación | Duración típica |
|-----------|-----------------|
| Spinner / giro continuo | 0.8–1.5s por vuelta |
| Entrada de un elemento | 0.3–0.5s |
| "Latido" / pulso de atención | 1–2s |
| Animación decorativa lenta (fondo) | 5–20s |

Las mismas pautas que las [[02 transition-duration | duraciones de transición]]: cortas para UI reactiva, largas para efectos ambientales.

## Aplicar la misma animación a varios elementos

```css
.titulo, .subtitulo, .texto { animation-name: aparecer; }
.titulo    { animation-duration: 0.4s; }
.subtitulo { animation-duration: 0.5s; animation-delay: 0.1s; }
.texto     { animation-duration: 0.6s; animation-delay: 0.2s; }
```

Una `@keyframes` reutilizada con distintas duraciones/delays da un efecto escalonado.

## Buenas prácticas

> [!tip] Recomendaciones
> - El `animation-name` debe coincidir **exactamente** con la `@keyframes`.
> - Nunca olvides `animation-duration` (0s por defecto = invisible).
> - Reutiliza una `@keyframes` con distintas duraciones/delays para efectos escalonados.
> - Define las duraciones en variables para coherencia.

## Errores comunes

> [!warning] Trampas
> - **Nombre que no coincide** con ninguna `@keyframes`: no anima.
> - **Olvidar la duración**: la animación es instantánea.
> - **Typo en el nombre** (mayúsculas/minúsculas importan en algunos contextos).

## Notas relacionadas

- [[01 Definición de Fotogramas]] — la `@keyframes` que `animation-name` referencia.
- [[03 animation-timing-function y delay]] — la curva y el retardo.
- [[index]] — el shorthand `animation`.
