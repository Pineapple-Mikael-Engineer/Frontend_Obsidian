---
title: Constraint Validation API — Validación desde JavaScript
aliases:
  - constraint validation
  - setCustomValidity
tags:
  - html
  - api/concepto
  - formularios
draft: false
order: 5
---

# Constraint Validation API

> [!definicion]
> La **Constraint Validation API** es la interfaz de JavaScript que da control fino sobre la validación nativa: comprobar la validez de un campo, leer por qué falla, y definir **mensajes y reglas personalizadas**. Es el puente entre la validación declarativa de HTML y la lógica a medida. Su desarrollo completo pertenece al curso de JavaScript; aquí se documenta su conexión con los formularios.

```js
const campo = document.querySelector("#clave2");
if (campo.value !== clave1.value) {
  campo.setCustomValidity("Las contraseñas no coinciden.");
} else {
  campo.setCustomValidity("");   // "" = válido
}
```

## Métodos y propiedades clave

| Miembro | Qué hace |
|---------|----------|
| `setCustomValidity(msg)` | Define un mensaje de error a medida; `""` marca el campo como válido |
| `checkValidity()` | Devuelve `true`/`false` según el campo (o form) sea válido |
| `reportValidity()` | Como `checkValidity()`, pero además **muestra** el mensaje al usuario |
| `validity` | Objeto con el detalle de qué falla (`valueMissing`, `typeMismatch`, `patternMismatch`…) |
| `validationMessage` | El texto del mensaje de error actual |

## El objeto validity

`validity` desglosa **por qué** un campo es inválido, lo que permite mensajes específicos:

| Propiedad de `validity` | Verdadera cuando… |
|-------------------------|-------------------|
| `valueMissing` | Falta un valor en un `required` |
| `typeMismatch` | El formato no encaja con el `type` (email, url) |
| `patternMismatch` | No cumple el `pattern` |
| `rangeUnderflow` / `rangeOverflow` | Por debajo de `min` / por encima de `max` |
| `tooShort` / `tooLong` | Longitud fuera de `minlength`/`maxlength` |

## El caso estrella: validación entre campos

> [!info] Lo que HTML no puede solo
> La validación nativa comprueba cada campo aislado, pero no puede expresar reglas **entre campos** (que dos contraseñas coincidan, que una fecha de fin sea posterior a la de inicio). Ahí entra `setCustomValidity()`:
> ```js
> function comprobar() {
>   const ok = clave1.value === clave2.value;
>   clave2.setCustomValidity(ok ? "" : "Las contraseñas no coinciden.");
> }
> clave2.addEventListener("input", comprobar);
> ```
> El navegador integra este mensaje personalizado en su flujo de validación nativo (bloquea el envío, muestra el globo), combinando lo mejor de ambos mundos.

## Acordarse de limpiar el mensaje

> [!warning] setCustomValidity("") para revalidar
> Un campo con un mensaje personalizado **no vacío** se considera siempre inválido. Hay que llamar a `setCustomValidity("")` cuando la condición se cumple, o el campo quedará bloqueado para siempre aunque el usuario lo corrija. Es el error más común con esta API.

## Pertenece a JavaScript

El detalle de eventos, manejo del DOM y patrones de validación en vivo vive en el **curso de JavaScript**. Esta nota cubre su papel dentro del sistema de validación de formularios.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa la API para reglas entre campos y mensajes a medida; deja lo simple a la validación declarativa.
> - Limpia el mensaje con `setCustomValidity("")` cuando el campo pase a ser válido.
> - Usa `validity` para dar mensajes específicos según el tipo de fallo.
> - Recuerda: sigue siendo validación de cliente; revalida en servidor.

## Errores comunes

> [!warning] Trampas
> - **No limpiar** el mensaje con `""`: el campo queda inválido permanentemente.
> - **Reimplementar todo en JS** ignorando la validación nativa, que ya cubre lo básico.
> - **Confiar en ella como seguridad**: es cliente; el servidor debe revalidar.

## Notas relacionadas

- [[01 Validación Nativa HTML5]] — el sistema declarativo que esta API extiende.
- [[04 Pseudoclases de Validación]] — el estilado del estado de validez.
- [[03 Tipos de input/02 input password]] — el caso típico de "confirmar contraseña".
