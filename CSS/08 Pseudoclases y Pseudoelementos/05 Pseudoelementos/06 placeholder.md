---
title: placeholder — El texto de relleno de los campos
aliases:
  - "::placeholder"
tags:
  - css
  - api/pseudo
  - selectores
propiedad: "::placeholder"
grupo: pseudoelemento
draft: false
---

# ::placeholder

> [!definicion]
> `::placeholder` estiliza el **texto de marcador de posición** (placeholder) de los campos de entrada: la pista gris que aparece cuando el campo está vacío (`<input placeholder="...">`). Permite ajustar su color y estilo para que combine con el diseño.

```css
input::placeholder { color: #999; font-style: italic; }
```

## Personalizar el placeholder

```css
::placeholder {
  color: #9ca3af;
  opacity: 1;   /* algunos navegadores reducen la opacidad; resetéala */
}
```

> [!info] Resetea la opacidad en Firefox
> Firefox aplica por defecto una **opacidad reducida** al placeholder. Para que tu color sea fiel, conviene declarar `opacity: 1` explícitamente. Sin ello, tu `color` se ve más claro de lo esperado.

## El placeholder NO sustituye al label

> [!warning] El placeholder no es una etiqueta
> El error de usabilidad/accesibilidad más grave: usar el placeholder **en lugar** de un [[02 Etiquetas (label) | `<label>`]]. Problemas:
> - **Desaparece** al escribir: el usuario pierde la referencia de qué iba el campo.
> - **Bajo contraste**: el gris del placeholder suele ser difícil de leer.
> - **Los lectores de pantalla** no siempre lo anuncian como etiqueta.
> - Confunde un campo "con placeholder" con uno ya **relleno**.
>
> El placeholder es una **pista adicional** (un ejemplo de formato), no la etiqueta. Cada campo necesita su `<label>` real:
> ```html
> <label for="email">Correo</label>
> <input id="email" placeholder="nombre@ejemplo.com">
> ```

## El contraste del placeholder

> [!warning] Que no sea tan claro que no se lea
> Un placeholder demasiado claro (gris muy tenue) falla las pautas de contraste y es difícil de leer. Aunque debe distinguirse del texto real (más apagado), tiene que ser **legible**. Busca un gris medio, no uno casi invisible.

## El truco :placeholder-shown

> [!tip] Detectar si el campo está vacío
> Relacionada: la pseudoclase [[index | `:placeholder-shown`]] selecciona el campo **mientras muestra el placeholder** (está vacío). Es la base del patrón "floating label" (la etiqueta que sube al escribir) y de validar solo campos con contenido:
> ```css
> /* Etiqueta flotante: sube cuando el campo tiene contenido */
> .campo input:not(:placeholder-shown) + label { transform: translateY(-1.2em) scale(0.85); }
> /* Validar solo si el usuario escribió algo */
> input:not(:placeholder-shown):invalid { border-color: red; }
> ```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `::placeholder` para ajustar el color, pero **mantén el contraste** legible.
> - Declara `opacity: 1` para evitar que Firefox lo aclare.
> - **Nunca** uses el placeholder como sustituto del `<label>`.
> - Aprovecha `:placeholder-shown` para floating labels y validación condicional.

## Errores comunes

> [!warning] Trampas
> - **Placeholder en vez de label**: grave problema de accesibilidad y usabilidad.
> - **Placeholder ilegible** por bajo contraste.
> - **Olvidar `opacity: 1`** y que el color se vea más claro en Firefox.

## Notas relacionadas

- [[02 Etiquetas (label)]] — la etiqueta que el placeholder NO sustituye.
- [[06 user-valid y user-invalid]] — validación que usa `:placeholder-shown`.
- [[index]] — `:placeholder-shown`, la pseudoclase relacionada.
