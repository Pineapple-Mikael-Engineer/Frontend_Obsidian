---
title: Pseudoclases de Formulario — Estilar según el estado de los campos
aliases:
  - form pseudo-classes
tags:
  - css
  - api/concepto
  - selectores
draft: false
order: 3
---

# Pseudoclases de Formulario

> [!definicion]
> Las pseudoclases de **formulario** estilizan los campos según su **estado**: marcado (`:checked`), deshabilitado (`:disabled`), requerido (`:required`), válido (`:valid`), fuera de rango (`:out-of-range`). Permiten dar feedback visual de validación y estado **sin JavaScript**, reaccionando a lo que el navegador ya sabe del formulario.

```css
input:required { border-left: 3px solid #cba6f7; }
input:invalid { border-color: #f38ba8; }
input:checked + label { font-weight: bold; }
```

## Mapa de la subsección

- [[01 checked e indeterminate]] — casillas y radios marcados.
- [[02 disabled y enabled]] — campos deshabilitados/habilitados.
- [[03 read-only y read-write]] — solo lectura vs. editable.
- [[04 required y optional]] — campos obligatorios u opcionales.
- [[05 valid e invalid]] — validación del valor.
- [[06 in-range y out-of-range]] — valores numéricos dentro/fuera de rango.
- [[07 user-valid y user-invalid]] — validación tras la interacción del usuario.

## Validación sin JavaScript

> [!tip] El navegador valida; el CSS reacciona
> El gran valor: el navegador **ya valida** los campos según sus atributos HTML (`required`, `type="email"`, `min`/`max`, `pattern`), y estas pseudoclases permiten **reaccionar** a esa validación con CSS, sin escribir lógica en JS:
> ```css
> input:invalid { border-color: #f38ba8; }
> input:valid { border-color: #a6e3a1; }
> ```
> Combinado con la [[03 Validación HTML5/index | validación de HTML]], se obtiene feedback de formulario instantáneo y accesible con muy poco código.

## El problema de :invalid prematuro

> [!warning] :valid/:invalid se aplican desde el primer momento
> Un problema clásico: `:invalid` se activa **nada más cargar** la página, antes de que el usuario escriba. Un campo `required` vacío aparece "en rojo" de entrada, lo que es confuso y agresivo. La solución moderna es [[07 user-valid y user-invalid | `:user-invalid`]], que solo se activa **tras** la interacción del usuario. Es la diferencia entre un formulario que regaña antes de tiempo y uno que avisa cuando toca. Detalle en [[05 valid e invalid]] y [[07 user-valid y user-invalid]].

## Notas relacionadas

- [[05 valid e invalid]] — la validación básica.
- [[07 user-valid y user-invalid]] — validación tras interactuar (recomendada).
- [[03 Validación HTML5/index]] — los atributos HTML que disparan la validación.
