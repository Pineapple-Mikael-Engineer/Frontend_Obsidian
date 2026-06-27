---
title: input type="password" — Texto oculto
aliases:
  - input password
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: ninguno
vacio: true
draft: false
order: 2
---

# input password

> [!definicion]
> `<input type="password">` es un campo de texto cuyo contenido se **oculta** (puntos o asteriscos) mientras se escribe, para proteger la entrada de miradas ajenas. Funcionalmente es como [[01 input text | text]], pero con enmascaramiento visual.

```html
<label for="clave">Contraseña</label>
<input type="password" id="clave" name="clave"
       autocomplete="current-password" minlength="8" required />
```

## autocomplete: el atributo crítico

El valor de `autocomplete` orienta a los gestores de contraseñas y es muy importante aquí:

| Valor | Cuándo |
|-------|--------|
| `current-password` | Login (contraseña existente) |
| `new-password` | Registro o cambio (contraseña nueva; activa el generador del navegador) |
| `one-time-code` | Códigos OTP de un solo uso |

```html
<!-- Registro: ayuda al navegador a sugerir y guardar una contraseña fuerte -->
<input type="password" autocomplete="new-password" />
```

## El enmascaramiento no es cifrado

> [!warning] password solo oculta visualmente
> `type="password"` oculta el texto **en pantalla**, pero el valor viaja igual que cualquier otro campo. La protección real depende de:
> - Enviar el formulario por **HTTPS** (sin él, la contraseña va en claro por la red).
> - Usar `method="post"` (nunca `GET`, que la dejaría en la URL).
>
> El enmascaramiento es contra el "mirón por encima del hombro", no un mecanismo de seguridad de transmisión.

## Mostrar/ocultar la contraseña

Un patrón de usabilidad habitual es un botón "ojo" que alterna el tipo con JavaScript:

```js
boton.addEventListener("click", () => {
  campo.type = campo.type === "password" ? "text" : "password";
});
```

## Buenas prácticas

> [!tip] Recomendaciones
> - `autocomplete="current-password"` en login y `new-password` en registro/cambio.
> - Sirve siempre el formulario por HTTPS y con `method="post"`.
> - No bloquees el pegado ni el gestor de contraseñas (`autocomplete="off"` perjudica la seguridad real).
> - Ofrece un botón de mostrar/ocultar para reducir errores de tecleo.

## Errores comunes

> [!warning] Trampas
> - **`autocomplete="off"`** en contraseñas: estorba a los gestores y empeora la práctica de seguridad.
> - **Creer que enmascarar es proteger**: sin HTTPS la contraseña viaja en claro.
> - **`method="get"`** con contraseñas: queda en la URL y el historial.

## Notas relacionadas

- [[01 input text]] — el campo de texto base, sin enmascarar.
- [[01 Atributos Comunes de input]] — `autocomplete`, `minlength`, etc.
- [[01 Contenedor de Formulario (form)]] — `method="post"` y HTTPS para el envío.
