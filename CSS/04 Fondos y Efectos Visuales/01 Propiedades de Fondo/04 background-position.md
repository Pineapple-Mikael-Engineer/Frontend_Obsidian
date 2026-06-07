---
title: background-position — Posición de la imagen de fondo
aliases:
  - background-position
tags:
  - css
  - api/propiedad
  - fondos
propiedad: background-position
grupo: fondo
valor_inicial: "0% 0%"
hereda: false
animable: true
draft: false
---

# background-position

> [!definicion]
> `background-position` define **dónde se ancla** la imagen de fondo dentro de la caja: el punto de partida desde el que se coloca (y se repite, si aplica). Acepta palabras clave (`center`, `top`…), longitudes y porcentajes.

```css
.hero { background-position: center; }
.icono { background-position: 1rem center; }
```

## Formas de valor

| Valor | Significa |
|-------|-----------|
| `center` | Centrada en ambos ejes |
| `top` / `bottom` / `left` / `right` | Anclada a ese lado |
| `left top` | Dos palabras: horizontal · vertical |
| `20px 40px` | Longitudes: x · y desde la esquina superior izquierda |
| `25% 75%` | Porcentajes (ver el cálculo especial abajo) |
| `right 1rem bottom 1rem` | Con offset desde un lado concreto |

## El cálculo de los porcentajes es especial

> [!info] El % alinea puntos, no esquinas
> El porcentaje en `background-position` no es como en otras propiedades: **alinea el punto X% de la imagen con el punto X% del contenedor**. `background-position: 50% 50%` (= `center`) hace coincidir el **centro** de la imagen con el **centro** de la caja; `100% 100%` alinea la esquina inferior derecha de la imagen con la de la caja. Por eso `center` siempre centra perfectamente, sin importar el tamaño de la imagen.

## El offset desde un lado

Una forma potente: anclar con una distancia desde un lado concreto (no solo desde arriba-izquierda):

```css
/* Imagen a 1rem del borde inferior derecho */
background-position: right 1rem bottom 1rem;
```

Muy útil para iconos o sellos en una esquina con margen.

## Ejemplos de uso

```css
/* Foto de hero centrada (con cover) */
.hero { background-size: cover; background-position: center; }

/* Icono de búsqueda a la izquierda de un input */
.buscador {
  background-image: url("lupa.svg");
  background-repeat: no-repeat;
  background-position: 0.75rem center;
  padding-left: 2.5rem;
}

/* Retrato que prioriza la parte superior (caras) al recortar */
.avatar-card { background-size: cover; background-position: top; }
```

## La sinergia con cover

> [!tip] cover + position: center, la pareja clásica
> Con `background-size: cover`, la imagen se recorta para llenar la caja, y `background-position` decide **qué parte se ve**. `center` es el valor seguro por defecto; pero para retratos conviene `top` (no cortar caras) y para paisajes a veces `center` o un porcentaje concreto:
> ```css
> .card-persona { background-size: cover; background-position: top; }
> ```

## Animar la posición

`background-position` es **animable**, lo que permite efectos de parallax sutil o patrones en movimiento:

```css
@keyframes deslizar { to { background-position: 100% 0; } }
.banda { animation: deslizar 10s linear infinite; }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `center` como valor seguro por defecto con `cover`.
> - `top` para fotos de personas (no cortar caras al recortar).
> - Usa el offset desde un lado (`right 1rem bottom 1rem`) para esquinas con margen.
> - Combina con `background-repeat: no-repeat` para imágenes únicas.

## Errores comunes

> [!warning] Trampas
> - **Recorte que corta lo importante** (caras, logos): ajusta la posición.
> - **Confundir el cálculo del `%`**: alinea puntos, no es un offset simple.
> - **Posición sin `no-repeat`**: con repetición, define el origen del mosaico, no una posición única.

## Notas relacionadas

- [[05 background-size]] — `cover`, con el que `position` elige el recorte visible.
- [[03 background-repeat]] — la repetición, cuyo origen marca la posición.
- [[08 Shorthand background]] — posición junto al resto.
