---
title: Modos de Mezcla — mix-blend-mode y background-blend-mode
aliases:
  - blend modes
  - mix-blend-mode
tags:
  - css
  - api/propiedad
  - fondos
propiedad: mix-blend-mode
grupo: fondo
valor_inicial: normal
hereda: false
animable: false
draft: false
---

# Modos de Mezcla (mix-blend-mode, background-blend-mode)

> [!definicion]
> Los **modos de mezcla** definen cómo se **combinan** los colores de capas superpuestas, como en Photoshop: multiplicar, pantalla, superponer, etc. `mix-blend-mode` mezcla un **elemento** con lo que tiene detrás; `background-blend-mode` mezcla las **capas de fondo** de un mismo elemento entre sí.

```css
.titulo { mix-blend-mode: difference; }   /* el texto invierte el color del fondo */
.foto { background-blend-mode: multiply; } /* mezcla imagen y color de fondo */
```

## Las dos propiedades

| Propiedad | Mezcla… |
|-----------|---------|
| `mix-blend-mode` | El **elemento** con el contenido que hay **detrás** (otros elementos) |
| `background-blend-mode` | Las **capas de fondo** de un mismo elemento entre sí |

## Los modos principales

| Modo | Efecto |
|------|--------|
| `normal` | Sin mezcla (por defecto) |
| `multiply` | Oscurece (multiplica colores); blanco no afecta |
| `screen` | Aclara (lo opuesto a multiply); negro no afecta |
| `overlay` | Aumenta el contraste |
| `difference` | Resta colores (efecto de inversión) |
| `darken` / `lighten` | Toma el color más oscuro/claro |
| `color`, `hue`, `saturation`, `luminosity` | Mezclan componentes HSL concretos |

## Recetas comunes

```css
/* Texto que se adapta al fondo (legible sobre claro y oscuro) */
.titulo-overlay {
  color: white;
  mix-blend-mode: difference;   /* invierte: visible sobre cualquier fondo */
}

/* Tinte de color sobre una foto en escala de grises */
.foto-tintada {
  background: url("foto.jpg"), #cba6f7;
  background-blend-mode: luminosity;   /* la foto da el brillo, el color el tono */
}

/* Efecto "duotono" */
.duotono {
  background: url("foto.jpg"), linear-gradient(#1e1e2e, #cba6f7);
  background-blend-mode: screen;
}
```

## difference para texto adaptativo

> [!tip] Texto legible sobre cualquier fondo con difference
> Un truco elegante: `mix-blend-mode: difference` con texto blanco hace que el texto **invierta** el color de lo que tiene detrás, garantizando contraste sobre fondos claros y oscuros sin saber cuál habrá:
> ```css
> .titulo { color: white; mix-blend-mode: difference; }
> ```
> Sobre negro se ve blanco; sobre blanco se ve negro; sobre una foto, siempre contrasta.

## Crea un contexto de apilamiento y aísla

> [!warning] isolation: isolate para acotar la mezcla
> `mix-blend-mode` mezcla con **todo** lo que hay detrás, lo que puede dar resultados no deseados si hay varias capas. La propiedad `isolation: isolate` en un contenedor padre **aísla** la mezcla a ese grupo, evitando que se mezcle con el fondo de la página:
> ```css
> .grupo { isolation: isolate; }   /* la mezcla interior no afecta al fondo general */
> ```

## Rendimiento

Los modos de mezcla, sobre todo en elementos grandes o animados, tienen **coste de composición**. Úsalos con mesura y prueba el rendimiento, especialmente en móvil.

## Buenas prácticas

> [!tip] Recomendaciones
> - `mix-blend-mode: difference` para texto que contrasta sobre cualquier fondo.
> - `background-blend-mode` para tintar fotos y efectos duotono.
> - Usa `isolation: isolate` para acotar la mezcla a un grupo.
> - Mide el rendimiento si los aplicas a elementos grandes o animados.

## Errores comunes

> [!warning] Trampas
> - **Mezcla que se "escapa"** al fondo de la página: usa `isolation: isolate`.
> - **Confundir las dos propiedades**: `mix` mezcla con lo de detrás, `background` mezcla capas internas.
> - **Resultados impredecibles** sobre fondos complejos: prueba en contexto.

## Notas relacionadas

- [[09 Múltiples Fondos]] — las capas que `background-blend-mode` mezcla.
- [[02 mask]] — otra forma de componer capas.
- [[01 filter]] — efectos de color por elemento, comparables.
