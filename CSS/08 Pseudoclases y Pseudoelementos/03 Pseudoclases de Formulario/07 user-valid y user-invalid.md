---
title: user-valid y user-invalid — Validación tras la interacción
aliases:
  - ":user-valid"
  - ":user-invalid"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: ":user-invalid"
grupo: pseudoclase
draft: false
---

# :user-valid e :user-invalid

> [!definicion]
> `:user-invalid` y `:user-valid` son como [[05 valid e invalid | `:invalid`/`:valid`]], pero se activan **solo después de que el usuario haya interactuado** con el campo (escrito y/o salido de él). Resuelven el problema del feedback prematuro: no marcan en rojo un campo vacío antes de que el usuario lo toque.

```css
input:user-invalid { border-color: #f38ba8; }
input:user-valid { border-color: #a6e3a1; }
```

## El problema que resuelven

> [!tip] Validar cuando toca, no antes
> El defecto de `:invalid` era marcar campos en rojo **al cargar**, antes de escribir. `:user-invalid` espera a que el usuario haya **interactuado** (normalmente tras editar y desenfocar, o al intentar enviar):
> ```css
> /* ✅ solo marca en rojo TRAS la interacción del usuario */
> input:user-invalid {
>   border-color: #f38ba8;
> }
> ```
> El campo `required` vacío **no** aparece en rojo de entrada; solo si el usuario lo deja vacío tras tocarlo o al enviar. Es el comportamiento que el usuario espera de un formulario bien hecho.

## El reemplazo de los apaños

> [!info] Adiós a los trucos con :not(:placeholder-shown)
> Antes de `:user-invalid`, había que **simular** este comportamiento con combinaciones frágiles:
> ```css
> /* El apaño antiguo */
> input:not(:placeholder-shown):invalid { border-color: red; }
> ```
> `:user-invalid` lo hace de forma **nativa y correcta**, replicando la heurística del navegador sobre "cuándo es apropiado mostrar el error". Es a `:invalid` lo que [[04 focus-visible | `:focus-visible`]] es a `:focus`: la versión que sabe **cuándo** mostrar el feedback.

## El paralelo con focus-visible

> [!info] La misma filosofía
> `:user-invalid` y `:focus-visible` comparten idea: el navegador aplica un **heurístico** para decidir cuándo el feedback es **útil** en vez de molesto. Igual que `:focus-visible` muestra el anillo solo con teclado, `:user-invalid` muestra el error solo tras la interacción. Ambos hacen las interfaces menos agresivas sin perder funcionalidad.

## Ejemplo completo

```css
/* Feedback de validación amable */
input:user-invalid {
  border-color: #f38ba8;
}
input:user-valid {
  border-color: #a6e3a1;
}
/* Mostrar el mensaje de error solo tras interactuar, con :has() */
.campo:has(input:user-invalid) .error {
  display: block;
}
```

## Soporte

> [!info] Soporte moderno, ya generalizado
> `:user-invalid`/`:user-valid` tienen **buen soporte** en navegadores modernos. Para navegadores antiguos, el respaldo es el apaño con `:invalid` + `:not(:placeholder-shown)`. En la web actual, son la forma recomendada de validación con CSS.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `:user-invalid`/`:user-valid` en vez de `:invalid`/`:valid` para feedback amable.
> - Combínalos con `:has()` para mostrar mensajes de error en el grupo tras interactuar.
> - No marques la validación solo con color: añade iconos y mensajes claros.
> - Da un respaldo con `:not(:placeholder-shown):invalid` para navegadores antiguos si hace falta.

## Errores comunes

> [!warning] Trampas
> - **Seguir usando `:invalid`** y marcar campos en rojo al cargar.
> - **Feedback solo por color**: inaccesible.
> - **No mostrar mensaje** de qué corregir.

## Notas relacionadas

- [[05 valid e invalid]] — la versión que se activa de inmediato.
- [[04 focus-visible]] — la misma filosofía aplicada al foco.
- [[03 has()]] — mostrar el error en el grupo del campo.
