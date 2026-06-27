---
title: style — Estilo en línea
aliases:
  - inline style
  - atributo style
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
order: 2
---

# Estilo en Línea (style)

> [!definicion]
> El atributo `style` aplica reglas CSS directamente a **un** elemento. Funciona, pero se considera **mala práctica** en general: mezcla presentación con estructura, no se reutiliza y tiene una especificidad altísima. La presentación debería vivir en CSS (vía [[01 Identificación (id, class) | clases]]), no en el HTML.

```html
<p style="color: crimson; font-size: 1.2rem;">Texto destacado</p>
```

## Por qué evitarlo

| Problema | Detalle |
|----------|---------|
| Mezcla capas | Mete CSS en el HTML (estructura ≠ presentación) |
| No se reutiliza | Hay que repetirlo en cada elemento |
| Especificidad altísima | Gana a casi cualquier regla de la hoja, difícil de sobrescribir |
| Sin media queries ni `:hover` | El atributo `style` no admite estados ni responsive |
| Ensucia el marcado | Dificulta leer la estructura |

## La alternativa: clases

```html
<!-- ❌ inline -->
<p style="color: crimson; font-size: 1.2rem;">Texto</p>

<!-- ✅ clase + CSS -->
<p class="destacado">Texto</p>
```
```css
.destacado { color: crimson; font-size: 1.2rem; }
```

Con clases, el estilo se reutiliza, se mantiene en un sitio, admite `:hover`/media queries y se sobrescribe con normalidad.

## Cuándo sí tiene sentido

> [!info] Los pocos usos legítimos
> `style` no está prohibido; hay casos donde es la herramienta correcta:
> - **Valores dinámicos calculados con JS**: una barra de progreso (`style="width: 73%"`), una posición (`style="left: 120px"`) que cambia en tiempo real.
> - **Variables CSS por elemento**: `style="--color: #cba6f7"`.
> - **Email HTML**: muchos clientes de correo no soportan CSS externo, así que el inline es necesario.
> - **Prototipos rápidos** de usar y tirar.
>
> Fuera de esos casos, clase + CSS.

## style ≠ <style>

No confundir el **atributo** `style` (estilo de un elemento) con el **elemento** [[08 Estilos Internos (style) | `<style>`]] (un bloque de reglas CSS en el `<head>`). Son cosas distintas con el mismo nombre.

## Buenas prácticas

> [!tip] Recomendaciones
> - Por defecto, estila con **clases** y CSS, no con `style`.
> - Reserva `style` para valores dinámicos calculados por JS, variables CSS y email.
> - Para alternar apariencia, cambia clases con JS, no escribas estilos inline.

## Errores comunes

> [!warning] Trampas
> - **Estilar todo inline**: imposible de mantener y reutilizar.
> - **Pelear con la especificidad** del inline desde la hoja (necesitarías `!important`).
> - **Confundir `style` con `<style>`**.

## Notas relacionadas

- [[01 Identificación (id, class)]] — `class`, la alternativa correcta.
- [[08 Estilos Internos (style)]] — el elemento `<style>`, no confundir.
