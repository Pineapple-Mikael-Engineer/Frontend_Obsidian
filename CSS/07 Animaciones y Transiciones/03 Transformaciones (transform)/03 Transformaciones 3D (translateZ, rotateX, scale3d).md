---
title: Transformaciones 3D — translateZ, rotateX, rotateY, scale3d
aliases:
  - 3d transforms
  - rotateX
  - rotateY
tags:
  - css
  - api/funcion
  - animacion
draft: false
---

# Transformaciones 3D (translateZ, rotateX, scale3d)

> [!definicion]
> Las transformaciones **3D** añaden el eje **Z** (profundidad) a las 2D: mover hacia/desde el espectador (`translateZ`), rotar alrededor de los ejes horizontal y vertical (`rotateX`, `rotateY`) y escalar en 3D. Requieren [[04 perspective y perspective-origin | `perspective`]] para que la profundidad se perciba.

```css
.tarjeta { transform: rotateY(180deg); }   /* voltea sobre el eje vertical */
```

## Las funciones 3D

| Función | Efecto |
|---------|--------|
| `translateZ(50px)` | Acerca/aleja del espectador |
| `translate3d(x, y, z)` | Traslación en los tres ejes |
| `rotateX(45deg)` | Gira sobre el eje horizontal (cae hacia atrás/adelante) |
| `rotateY(45deg)` | Gira sobre el eje vertical (puerta giratoria) |
| `rotateZ(45deg)` | Gira en el plano (= `rotate` 2D) |
| `scale3d(sx, sy, sz)` | Escala en 3D |

## Hace falta perspective

> [!warning] Sin perspective, el 3D se ve plano
> Una transformación 3D sin **`perspective`** no se percibe como 3D: `rotateY(45deg)` sin perspectiva solo "aplasta" el elemento horizontalmente, sin sensación de profundidad. La `perspective` define la distancia del espectador y es lo que da la ilusión 3D:
> ```css
> .escena { perspective: 800px; }        /* en el contenedor */
> .escena .tarjeta:hover { transform: rotateY(20deg); }   /* ahora se ve en 3D */
> ```
> Detalle en [[04 perspective y perspective-origin]].

## El efecto estrella: voltear una tarjeta

> [!tip] La tarjeta que se voltea (flip card)
> El uso 3D más popular: una tarjeta con cara frontal y trasera que se voltea al pasar el ratón:
> ```css
> .escena { perspective: 1000px; }
> .tarjeta {
>   transform-style: preserve-3d;        /* mantiene el 3D de los hijos */
>   transition: transform 0.6s;
> }
> .escena:hover .tarjeta { transform: rotateY(180deg); }
> .cara-frontal, .cara-trasera { backface-visibility: hidden; }   /* ocultar la cara que da la espalda */
> .cara-trasera { transform: rotateY(180deg); }
> ```
> Requiere tres piezas: `perspective` (la escena), [[06 transform-style | `transform-style: preserve-3d`]] (mantener el 3D) y [[07 backface-visibility | `backface-visibility: hidden`]] (ocultar la cara trasera).

## translateZ y el rendimiento

> [!info] El "hack" de translateZ(0)
> `translateZ(0)` (o `translate3d(0,0,0)`) se usaba como **truco** para forzar la aceleración por GPU de un elemento (promoverlo a su propia capa de composición). Hoy [[02 will-change | `will-change`]] es la forma correcta y explícita de hacerlo; el `translateZ(0)` es un hack heredado que conviene reconocer pero no abusar.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon `perspective` en el **contenedor** para que las transformaciones 3D de los hijos se vean.
> - Para flip cards: `perspective` + `preserve-3d` + `backface-visibility: hidden`.
> - Usa `will-change` (no `translateZ(0)`) para forzar la GPU si hace falta.
> - Mide el rendimiento: el 3D complejo puede ser caro en móvil.

## Errores comunes

> [!warning] Trampas
> - **Transformaciones 3D sin `perspective`**: se ven planas/aplastadas.
> - **Olvidar `transform-style: preserve-3d`** en estructuras 3D anidadas.
> - **Abusar de `translateZ(0)`** como hack de rendimiento (usa `will-change`).

## Notas relacionadas

- [[04 perspective y perspective-origin]] — la profundidad imprescindible.
- [[06 transform-style]] · [[07 backface-visibility]] — las piezas del flip card.
- [[01 Transformaciones 2D (translate, rotate, scale, skew)]] — las 2D base.
