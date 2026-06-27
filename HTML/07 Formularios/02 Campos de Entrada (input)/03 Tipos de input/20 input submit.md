---
title: input type="submit" — Botón de envío
aliases:
  - input submit
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: button
vacio: true
draft: false
order: 20
---

# input submit

> [!definicion]
> `<input type="submit">` es un **botón que envía el formulario**. Su texto se define con el atributo `value`. Es la forma clásica de crear el botón de envío, aunque hoy [[06 Botones (button) | `<button type="submit">`]] suele preferirse por su flexibilidad.

```html
<input type="submit" value="Registrarse" />
```

## input submit vs. button

| | `<input type="submit">` | `<button type="submit">` |
|--|-------------------------|--------------------------|
| Texto | Atributo `value` (solo texto) | **Contenido HTML** (texto, iconos, `<img>`) |
| Estilo del contenido | Limitado | Total (puede llevar elementos dentro) |
| Es void | Sí | No |

```html
<!-- input: texto plano vía value -->
<input type="submit" value="Enviar" />

<!-- button: contenido rico -->
<button type="submit"><svg>…</svg> Enviar</button>
```

## El name y value en el envío

Si el botón submit tiene `name`, su `value` **se envía** junto al resto. Esto permite distinguir **qué botón** envió un formulario con varios:

```html
<button type="submit" name="accion" value="guardar">Guardar</button>
<button type="submit" name="accion" value="publicar">Publicar</button>
```

El servidor recibe `accion=guardar` o `accion=publicar` según cuál se pulsó.

## Enviar con Enter

Pulsar **Enter** en un campo de texto del formulario activa el botón de envío, aunque no se haga clic. Es un comportamiento de accesibilidad esperado; por eso todo formulario debería tener un botón submit aunque también se maneje con JavaScript.

## Buenas prácticas

> [!tip] Recomendaciones
> - Prefiere `<button type="submit">` por su flexibilidad (iconos, estilo).
> - Da un texto de acción claro ("Crear cuenta"), no genérico ("Enviar") cuando puedas.
> - Usa `name`/`value` en el botón si necesitas distinguir varias acciones de envío.

## Errores comunes

> [!warning] Trampas
> - **Texto vago** ("Enviar") cuando un verbo específico ayudaría ("Pagar 29,99 €").
> - **Querer un icono dentro**: `input submit` no lo permite; usa `<button>`.
> - **Sin botón submit** confiando solo en JS: rompe el envío con Enter y la accesibilidad.

## Notas relacionadas

- [[06 Botones (button)]] — la alternativa flexible y recomendada.
- [[21 input reset]] — el botón de restablecer.
- [[01 Contenedor de Formulario (form)]] — el formulario que se envía.
