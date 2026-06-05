---
title: input type="checkbox" — Casilla de verificación
aliases:
  - input checkbox
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: checkbox
vacio: true
draft: false
---

# input checkbox

> [!definicion]
> `<input type="checkbox">` es una **casilla** que el usuario marca o desmarca. Sirve para opciones binarias independientes (aceptar términos) o para **seleccionar varias** de una lista. A diferencia de [[10 input radio | radio]], cada casilla es independiente.

```html
<input type="checkbox" id="acepto" name="acepto" value="si" required />
<label for="acepto">Acepto los términos</label>
```

## Atributos propios

| Atributo | Efecto |
|----------|--------|
| `checked` | La casilla aparece marcada de inicio |
| `value` | Valor que se envía **si está marcada** (por defecto `"on"`) |
| `required` | Obliga a marcarla (útil en "aceptar términos") |

## Qué se envía

> [!info] Solo se envían las casillas marcadas
> Una casilla **desmarcada no envía nada** (ni siquiera su `name`). Una marcada envía `name=value`. Por eso, para saber en el servidor si estaba marcada, se comprueba la **presencia** de la clave, no un valor `false`. Para un grupo de selección múltiple, se repite el mismo `name`:
> ```html
> <input type="checkbox" name="intereses" value="musica" id="m">
> <label for="m">Música</label>
> <input type="checkbox" name="intereses" value="cine" id="c">
> <label for="c">Cine</label>
> ```
> El servidor recibe `intereses=musica&intereses=cine` con las marcadas.

## El estado indeterminado

Una casilla tiene un tercer estado visual, **indeterminado** (un guion en vez de tilde), que solo se activa por JavaScript (`elemento.indeterminate = true`). Es habitual en un "seleccionar todo" cuando solo algunos hijos están marcados. No se envía como valor distinto: es puramente visual.

## Asociación con label

Cada casilla necesita su `<label>` asociado, lo que además **amplía el área clicable** (se puede pulsar el texto, no solo el cuadradito):

```html
<input type="checkbox" id="newsletter" name="newsletter" />
<label for="newsletter">Suscribirme al boletín</label>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Da `value` explícito; no dependas del `"on"` por defecto.
> - Asocia siempre un `<label>` (área clicable + accesibilidad).
> - Para selección múltiple, usa el mismo `name` en todas las casillas del grupo.
> - En el servidor, comprueba la presencia de la clave, no un `false`.

## Errores comunes

> [!warning] Trampas
> - **Esperar que una casilla desmarcada envíe `false`**: no envía nada.
> - **Casilla sin `<label>`**: inaccesible y con área clicable mínima.
> - **Confundir checkbox con radio**: usa radio cuando solo se puede elegir una opción.

## Notas relacionadas

- [[10 input radio]] — para elegir una sola opción de un grupo.
- [[07 Etiquetas y Agrupación/02 Agrupación de Campos (fieldset)]] — agrupar casillas relacionadas.
- [[01 Atributos Comunes de input]] — `checked`, `value`, `required`.
