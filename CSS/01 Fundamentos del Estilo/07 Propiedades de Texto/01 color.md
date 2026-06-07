---
title: color — Color del texto
aliases:
  - color
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: color
grupo: tipografia
valor_inicial: "depende del navegador (suele negro)"
hereda: true
animable: true
draft: false
---

# color

> [!definicion]
> La propiedad `color` define el **color del texto** de un elemento (y, por defecto, de su contenido en línea). Es heredable, así que se propaga a los descendientes, y es el valor al que apunta la palabra clave `currentColor`.

```css
p { color: #1e1e2e; }
a { color: oklch(0.7 0.15 300); }
```

## Tabla de comportamiento

| Aspecto | Valor |
|---------|-------|
| Valor inicial | Depende del navegador (normalmente negro) |
| Hereda | **Sí** |
| Animable | Sí (interpola entre colores) |
| Acepta | Cualquier formato de color |

## color afecta a más que el texto

> [!info] color es la referencia de currentColor
> `color` no solo tiñe el texto: es el valor que toma [[01 Palabras Clave de Color | `currentColor`]]. Por eso bordes, sombras, viñetas de lista (`::marker`), subrayados (`text-decoration`) e iconos SVG con `fill: currentColor` siguen al `color` del elemento. Cambiar `color` puede actualizar todos ellos de golpe:
> ```css
> .chip {
>   color: #cba6f7;
>   border: 1px solid currentColor;   /* el borde iguala al texto */
> }
> ```

## Herencia: el color base

Como se hereda, declarar `color` en `body` fija el color de texto de todo el documento, sobrescribible localmente:

```css
body { color: #313244; }       /* texto base */
.silenciado { color: #9399b2; } /* más tenue donde aplique */
```

## Contraste y accesibilidad

> [!warning] El contraste es un requisito, no un gusto
> El color del texto debe tener **suficiente contraste** con su fondo. Las WCAG exigen una ratio mínima de **4.5:1** para texto normal y **3:1** para texto grande (nivel AA). Un texto gris claro sobre blanco puede ser bonito pero ilegible para muchos usuarios, e incumple la norma. Herramientas de DevTools y validadores comprueban la ratio; los formatos perceptuales como [[06 LAB y LCH | OKLCH]] ayudan a controlarla.

## color vs. background-color

`color` es el primer plano (el texto); [[01 background-color | `background-color`]] es el fondo. Ambos definen el par cuyo contraste importa. Conviene declararlos **juntos**: poner solo uno puede dejar texto invisible si el otro cambia (p. ej. en modo oscuro del usuario).

## Buenas prácticas

> [!tip] Recomendaciones
> - Define el `color` base en `body` y deriva variantes con [[07 color-mix() | `color-mix()`]] o variables.
> - Aprovecha `currentColor` para sincronizar bordes/iconos con el texto.
> - Verifica el contraste (mínimo 4.5:1 para texto normal).
> - Declara `color` y `background-color` juntos para evitar combinaciones ilegibles.

## Errores comunes

> [!warning] Trampas
> - **Contraste insuficiente**: texto bonito pero ilegible e inaccesible.
> - **Definir solo `color` o solo `background`**: riesgo en modo oscuro/temas.
> - **Olvidar que se hereda**: cambiar `color` en un contenedor afecta a todo dentro.

## Notas relacionadas

- [[05 Colores/index]] — los formatos que acepta `color`.
- [[01 background-color]] — el fondo, su pareja de contraste.
- [[01 Palabras Clave de Color]] — `currentColor`, que depende de `color`.
