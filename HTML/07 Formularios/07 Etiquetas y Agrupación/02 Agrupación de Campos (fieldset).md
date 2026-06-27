---
title: <fieldset> — Agrupar controles relacionados
aliases:
  - fieldset element
tags:
  - html
  - api/elemento
  - formularios
elemento: fieldset
categoria: interactivo
rol_implicito: group
vacio: false
draft: false
order: 2
---

# Agrupación de Campos (fieldset)

> [!definicion]
> `<fieldset>` agrupa **controles de formulario relacionados** bajo un mismo título (su [[03 Leyenda de Grupo (legend) | `<legend>`]]). Es esencial para grupos de [[03 Tipos de input/10 input radio | radios]] o [[03 Tipos de input/09 input checkbox | checkboxes]], donde el conjunto necesita una pregunta común. El navegador lo dibuja con un borde por defecto.

```html
<fieldset>
  <legend>Método de pago</legend>
  <input type="radio" id="tarjeta" name="pago" value="tarjeta" />
  <label for="tarjeta">Tarjeta</label>
  <input type="radio" id="paypal" name="pago" value="paypal" />
  <label for="paypal">PayPal</label>
</fieldset>
```

## Por qué los grupos de radios lo necesitan

> [!info] El legend da contexto al grupo
> Un grupo de radios sin `<fieldset>`/`<legend>` deja al usuario de lector de pantalla sin saber **a qué pregunta** responden las opciones: oiría "Tarjeta, opción 1 de 2" sin el contexto "Método de pago". El `<legend>` actúa como etiqueta del grupo entero, igual que un `<label>` etiqueta un control individual. Es el patrón obligado para preguntas con varias opciones.

## Atributos útiles

| Atributo | Efecto |
|----------|--------|
| `disabled` | Deshabilita **todos** los controles del grupo a la vez |
| `name` | Nombre del grupo (uso poco frecuente) |
| `form` | Asocia el fieldset a un `<form>` por su `id` |

### disabled en cascada

`disabled` en el `<fieldset>` desactiva todos sus controles de una vez, muy útil para deshabilitar una sección entera del formulario (por ejemplo, mientras se procesa el envío):

```html
<fieldset disabled>
  <legend>Datos de envío</legend>
  <!-- todos estos controles quedan deshabilitados -->
</fieldset>
```

## Estilar el fieldset

El borde por defecto a menudo no encaja con el diseño; se reestiliza o elimina con CSS, sin renunciar a la semántica:

```css
fieldset { border: none; padding: 0; margin: 0; }
legend { font-weight: 600; padding: 0; }
```

## Cuándo usarlo (y cuándo no)

| Usar `<fieldset>` | No hace falta |
|-------------------|---------------|
| Grupos de radios/checkboxes | Un campo de texto suelto con su `<label>` |
| Secciones temáticas de un formulario largo | Un formulario corto de un par de campos |
| Agrupar para deshabilitar en bloque | — |

## Buenas prácticas

> [!tip] Recomendaciones
> - Envuelve **siempre** los grupos de radios/checkboxes en `<fieldset>` + `<legend>`.
> - Usa `disabled` en el fieldset para desactivar secciones completas.
> - Reestiliza el borde con CSS si no encaja, pero conserva el elemento.
> - No abuses: un campo individual no necesita fieldset, le basta su `<label>`.

## Errores comunes

> [!warning] Trampas
> - **Grupo de radios sin `<fieldset>`/`<legend>`**: pierde el contexto de la pregunta.
> - **`<fieldset>` sin `<legend>`**: el grupo queda sin título accesible.
> - **Usar un `<div>` con borde** en lugar de `<fieldset>`: se ve igual pero sin semántica de grupo.

## Notas relacionadas

- [[03 Leyenda de Grupo (legend)]] — el título obligatorio del grupo.
- [[03 Tipos de input/10 input radio]] — el caso de uso por excelencia.
- [[01 Etiqueta de Campo (label)]] — para controles individuales (no grupos).
