---
title: animation-iteration-count — Cuántas veces se repite
aliases:
  - animation-iteration-count
  - infinite
tags:
  - css
  - api/propiedad
  - animacion
propiedad: animation-iteration-count
grupo: animacion
valor_inicial: "1"
hereda: false
animable: false
draft: false
---

# animation-iteration-count

> [!definicion]
> `animation-iteration-count` define **cuántas veces** se repite la animación: una vez (por defecto), un número concreto, o **infinitamente** (`infinite`). Es lo que distingue una entrada de un solo uso de un spinner que gira sin parar.

```css
.spinner { animation: girar 1s linear infinite; }   /* sin parar */
.pulso   { animation: pulso 0.5s ease 3; }           /* 3 veces */
```

## Valores

| Valor | Efecto |
|-------|--------|
| `1` | Una vez (por defecto) |
| `<número>` | Ese número de veces (`3`, `5`) |
| `infinite` | Indefinidamente, hasta que se detenga |
| `<decimal>` | Repeticiones parciales (`2.5` = dos y media) |

## infinite: el bucle

`infinite` es el valor estrella para animaciones continuas: spinners, "latidos" de atención, fondos animados, indicadores de carga:

```css
.cargando { animation: pulso 1.5s ease-in-out infinite; }
```

## Repeticiones decimales

> [!info] Un número decimal corta a mitad de ciclo
> Un valor **decimal** detiene la animación a mitad de un ciclo: `2.5` ejecuta dos repeticiones completas y media. Útil cuando quieres que termine en un punto intermedio de la secuencia:
> ```css
> animation-iteration-count: 1.5;   /* termina a mitad del segundo ciclo */
> ```

## infinite necesita una animación que "cierre"

> [!warning] Un bucle que salta si no vuelve al inicio
> Para que una animación `infinite` se repita **sin saltos**, su fotograma final (`100%`) debe coincidir con el inicial (`0%`), o el elemento "saltará" al reiniciar cada ciclo:
> ```css
> /* ✅ vuelve al inicio: bucle suave */
> @keyframes pulso { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.1); } }
> /* ❌ termina distinto del inicio: salta al reiniciar */
> @keyframes mal { 0% { transform: scale(1); } 100% { transform: scale(1.5); } }
> ```
> Para una rotación continua (`0%` a `360deg`), funciona porque 360° = 0° visualmente. Para otros casos, [[05 animation-direction | `alternate`]] ayuda (va y vuelve).

## Detener un bucle infinite

Un `infinite` se detiene con [[07 animation-play-state | `animation-play-state: paused`]] o quitando la animación. No hay un "número de veces dinámico" sin JS.

## Buenas prácticas

> [!tip] Recomendaciones
> - `infinite` para spinners, indicadores de carga y efectos ambientales.
> - Asegúrate de que el `100%` coincida con el `0%` (o usa `alternate`) para bucles sin saltos.
> - Un número concreto para animaciones de atención que no deben repetirse para siempre.
> - Respeta `prefers-reduced-motion`: desactiva los bucles para quien lo pide.

## Errores comunes

> [!warning] Trampas
> - **Bucle que "salta"** porque el fin no coincide con el inicio.
> - **`infinite` decorativo** sin respetar `prefers-reduced-motion` (mareo).
> - **Olvidar que `1` es el valor por defecto**: la animación solo corre una vez si no se dice `infinite`.

## Notas relacionadas

- [[05 animation-direction]] — `alternate` para bucles que van y vuelven.
- [[07 animation-play-state]] — pausar un bucle.
- [[06 Preferencias (prefers-color-scheme, prefers-reduced-motion)]] — desactivar bucles por accesibilidad.
