---
title: Pseudoclases de validación — Estilar campos válidos e inválidos
aliases:
  - validation pseudo-classes
  - pseudoclases de validación
tags:
  - html
  - api/concepto
  - formularios
draft: false
---

# Pseudoclases de Validación

> [!definicion]
> CSS ofrece **pseudoclases** que reflejan el estado de validación de un campo, permitiendo estilarlo en verde/rojo según sea válido o no. La validación la define HTML; el **aspecto** del estado se delega a CSS. Estas pseudoclases pertenecen al curso de CSS; aquí se documentan por su relación directa con los formularios.

```css
input:invalid { border-color: #f38ba8; }
input:valid   { border-color: #a6e3a1; }
input:required { /* … */ }
```

## Las pseudoclases principales

| Pseudoclase | Coincide cuando el campo… |
|-------------|---------------------------|
| `:valid` | Cumple todas sus restricciones |
| `:invalid` | Incumple alguna restricción |
| `:required` | Tiene el atributo `required` |
| `:optional` | No es obligatorio |
| `:in-range` / `:out-of-range` | Está dentro / fuera de `min`–`max` |
| `:user-invalid` | Es inválido **y** el usuario ya interactuó (más reciente) |

## El problema de :invalid prematuro

> [!warning] :invalid se activa antes de escribir
> `:invalid` coincide **desde el primer momento**, incluso antes de que el usuario toque el campo: un `required` vacío ya es inválido al cargar. Marcar todo en rojo de entrada es mala UX. Soluciones:
> - **`:user-invalid`** (moderna): solo se activa tras la interacción del usuario.
> - Combinar con `:not(:placeholder-shown)` o `:focus` para no marcar campos intactos.
> ```css
> /* Solo marca en rojo tras interactuar */
> input:user-invalid { border-color: #f38ba8; }
> ```

## Feedback accesible, no solo color

> [!info] El color no basta
> Marcar un error solo con color (borde rojo) excluye a quien no lo distingue (daltonismo) y viola WCAG. El estado de error debe comunicarse también con **texto** (un mensaje), un **icono**, y atributos ARIA (`aria-invalid="true"`, `aria-describedby` apuntando al mensaje):
> ```html
> <input aria-invalid="true" aria-describedby="err-email" />
> <p id="err-email">El correo no es válido.</p>
> ```

## Estos selectores viven en CSS

El desarrollo completo de pseudoclases, especificidad y la cascada pertenece al **curso de CSS**. Esta nota documenta su conexión con la validación de formularios; la referencia detallada está allí.

## Buenas prácticas

> [!tip] Recomendaciones
> - Prefiere `:user-invalid` (o combina con `:focus`/`:not(:placeholder-shown)`) para no marcar campos intactos.
> - Comunica el error con texto e iconos, no solo con color.
> - Añade `aria-invalid` y `aria-describedby` para la accesibilidad del error.
> - Estila también `:valid` para dar feedback positivo cuando aporte.

## Errores comunes

> [!warning] Trampas
> - **`:invalid` global** que pinta todo en rojo al cargar.
> - **Solo color** para señalar errores: inaccesible.
> - **Olvidar `aria-invalid`/mensaje**: el lector de pantalla no se entera del error.

## Notas relacionadas

- [[01 Validación Nativa HTML5]] — el sistema que determina válido/inválido.
- [[03 Restricciones (required, min, max, step, maxlength)]] — lo que estas pseudoclases reflejan.
- [[05 Constraint Validation API]] — controlar el estado de validez desde JS.
