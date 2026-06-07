---
title: transform-origin — El punto sobre el que se transforma
aliases:
  - transform-origin
tags:
  - css
  - api/propiedad
  - animacion
propiedad: transform-origin
grupo: animacion
valor_inicial: "center"
hereda: false
animable: true
draft: false
---

# transform-origin

> [!definicion]
> `transform-origin` define el **punto de pivote** sobre el que se aplica una transformación: el centro de rotación, el punto desde el que escala, el ancla del skew. Por defecto es el **centro** del elemento; cambiarlo altera por completo cómo se ve la transformación.

```css
.x {
  transform-origin: top left;   /* rota/escala desde la esquina superior izquierda */
  transform: rotate(15deg);
}
```

## El efecto sobre rotate y scale

> [!info] El pivote cambia el resultado
> El mismo `rotate(45deg)` se ve **muy distinto** según el origen:
> - `center` (por defecto): gira sobre su centro, quedándose en su sitio.
> - `top left`: gira sobre la esquina superior izquierda, "barriendo" hacia un lado.
> - `bottom`: gira sobre el borde inferior, como una puerta abatible.
>
> Igual con `scale`: con origen `center` crece en todas direcciones; con `left` crece solo hacia la derecha (el lado izquierdo queda anclado).

## Valores

| Valor | Pivote |
|-------|--------|
| `center` | El centro (por defecto) |
| `top left`, `bottom right`… | Las esquinas/lados con palabras clave |
| `0 0` | La esquina superior izquierda |
| `50% 100%` | Centro horizontal, borde inferior |
| `20px 30px` | Coordenadas concretas |
| (tercer valor) | El eje Z, para transformaciones 3D |

## Casos de uso

```css
/* Un menú que se despliega "creciendo" desde arriba */
.dropdown {
  transform: scaleY(0);
  transform-origin: top;        /* crece hacia abajo desde el borde superior */
  transition: transform 0.2s;
}
.dropdown.abierto { transform: scaleY(1); }

/* Una aguja de reloj que gira sobre su base */
.aguja {
  transform-origin: bottom center;   /* pivota sobre el extremo inferior */
}

/* Un elemento que rota desde una esquina */
.esquina { transform-origin: top left; transform: rotate(10deg); }
```

## El despliegue desde un borde

> [!tip] scaleY + transform-origin para menús
> Un patrón elegante: un menú que se despliega creciendo desde su borde superior (no desde el centro), usando `scaleY` con `transform-origin: top`:
> ```css
> .menu { transform: scaleY(0); transform-origin: top; transition: transform 0.2s; }
> .menu.abierto { transform: scaleY(1); }
> ```
> El menú "crece" hacia abajo de forma natural, en vez de expandirse desde el centro. Cambiar el origen a `bottom` lo haría crecer hacia arriba.

## Buenas prácticas

> [!tip] Recomendaciones
> - Ajusta `transform-origin` cuando la transformación deba pivotar sobre un borde, no el centro.
> - `transform-origin: top` + `scaleY` para menús que se despliegan desde arriba.
> - Para agujas/manecillas, ancla el pivote en su base.
> - Recuerda que afecta a rotate, scale y skew por igual.

## Errores comunes

> [!warning] Trampas
> - **Olvidar cambiar el origen**: la rotación/escala pivota sobre el centro cuando querías un borde.
> - **Un despliegue desde el centro** cuando se esperaba desde un borde (ajusta el origen).

## Notas relacionadas

- [[01 Transformaciones 2D (translate, rotate, scale, skew)]] — las transformaciones que pivotan.
- [[04 perspective y perspective-origin]] — el origen de la perspectiva 3D.
- [[index]] — `transform` en general.
