---
title: <legend> — Título de un fieldset
aliases:
  - legend element
tags:
  - html
  - api/elemento
  - formularios
elemento: legend
categoria: interactivo
rol_implicito: ninguno
vacio: false
draft: false
order: 3
---

# Leyenda de Grupo (legend)

> [!definicion]
> `<legend>` es el **título** de un [[02 Agrupación de Campos (fieldset) | `<fieldset>`]]: describe de qué trata el grupo de controles. Debe ser el **primer hijo** del `<fieldset>`. Funciona como la "etiqueta del grupo", el equivalente del [[01 Etiqueta de Campo (label) | `<label>`]] para un conjunto.

```html
<fieldset>
  <legend>Talla de camiseta</legend>
  <input type="radio" id="s" name="talla" value="S" />
  <label for="s">S</label>
  <input type="radio" id="m" name="talla" value="M" />
  <label for="m">M</label>
</fieldset>
```

## Cómo lo usa la asistencia

> [!info] El legend se anuncia con cada opción
> Los lectores de pantalla **combinan** el texto del `<legend>` con la etiqueta de cada control del grupo: al enfocar la primera opción, anuncian "Talla de camiseta, S, opción 1 de 2". Así el usuario sabe en todo momento a qué pregunta está respondiendo, aunque navegue opción por opción. Sin `<legend>`, solo oiría "S", "M"… sin el contexto.

## Reglas

| Regla | Detalle |
|-------|---------|
| Posición | **Primer hijo** del `<fieldset>`, antes de los controles |
| Cantidad | Uno por `<fieldset>` |
| Contenido | Texto y elementos de fraseo (admite `<strong>`, etc.) |

## Estilado

El `<legend>` tiene un posicionamiento por defecto peculiar (incrustado en el borde del fieldset) que a veces complica el diseño. Se controla con CSS:

```css
legend {
  padding: 0;
  font-weight: 600;
  margin-bottom: 0.5rem;
}
```

## legend vs. label

| Elemento | Etiqueta… |
|----------|-----------|
| `<label>` | Un **control** individual |
| `<legend>` | Un **grupo** de controles (dentro de un `<fieldset>`) |

No son intercambiables: un control suelto usa `<label>`; un grupo de opciones usa `<fieldset>` + `<legend>`.

## Buenas prácticas

> [!tip] Recomendaciones
> - Pon un `<legend>` en **todo** `<fieldset>`, como primer hijo.
> - Redáctalo como la pregunta del grupo ("Método de pago", "¿Aceptas las condiciones?").
> - Ajusta su posición/estilo con CSS si el comportamiento por defecto no encaja.

## Errores comunes

> [!warning] Trampas
> - **`<fieldset>` sin `<legend>`**: el grupo pierde su título accesible.
> - **`<legend>` fuera de su posición** (no primer hijo): marcado inválido.
> - **Usar un `<label>` o `<h3>` como "título de grupo"**: se ve, pero no se asocia semánticamente al fieldset.

## Notas relacionadas

- [[02 Agrupación de Campos (fieldset)]] — el contenedor que `<legend>` titula.
- [[01 Etiqueta de Campo (label)]] — la etiqueta de un control individual.
