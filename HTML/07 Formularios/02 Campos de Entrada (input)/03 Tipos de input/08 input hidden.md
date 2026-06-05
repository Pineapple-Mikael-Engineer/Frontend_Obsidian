---
title: input type="hidden" — Dato oculto del formulario
aliases:
  - input hidden
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: ninguno
vacio: true
draft: false
---

# input hidden

> [!definicion]
> `<input type="hidden">` envía un dato con el formulario **sin mostrarlo** al usuario ni permitirle editarlo. Sirve para llevar valores de control que el servidor necesita pero que la persona no debe ver ni cambiar: un identificador de registro, un token, el estado de un asistente por pasos.

```html
<form action="/actualizar" method="post">
  <input type="hidden" name="id_usuario" value="4821" />
  <label for="nombre">Nombre</label>
  <input type="text" id="nombre" name="nombre" value="Ana" />
  <button type="submit">Guardar</button>
</form>
```

## Usos típicos

| Uso | Ejemplo |
|-----|---------|
| Identificador del registro a editar | `<input type="hidden" name="id" value="42">` |
| Token CSRF de seguridad | `<input type="hidden" name="_csrf" value="…">` |
| Paso actual de un formulario multipágina | `<input type="hidden" name="paso" value="2">` |
| Valor calculado en cliente | Total, coordenadas, zona horaria |

## hidden no es seguro ni privado

> [!warning] "Oculto" significa invisible, no protegido
> Un `type="hidden"` **no** está cifrado ni protegido: su valor está en el HTML, visible en "ver código fuente" y en DevTools, y **editable** por cualquiera antes de enviar. Por eso:
> - **Nunca** pongas datos sensibles (precios reales, permisos, secretos) confiando en que el usuario no los verá.
> - El servidor debe **revalidar** todo lo que llegue por un campo hidden, como cualquier otro dato. Un atacante puede cambiar `precio=10` por `precio=0`.
> - Para tokens CSRF, el valor protege porque el servidor lo verifica, no porque esté "oculto".

## hidden vs. otras formas de pasar datos

| Mecanismo | Cuándo |
|-----------|--------|
| `type="hidden"` | Dato que viaja con un envío de formulario clásico |
| Atributo `data-*` | Dato para JavaScript, no para enviar |
| Variable JS / estado | Dato que solo vive en el cliente |

Los atributos `data-*` se desarrollan en [[12 Atributos de Datos (data-*)]].

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo para identificadores y tokens que el servidor necesita en el envío.
> - **Revalida siempre** en el servidor: el valor es manipulable.
> - No metas información sensible asumiendo que está oculta.
> - Para datos solo de cliente (JS), prefiere `data-*` o el estado de la app.

## Errores comunes

> [!warning] Trampas
> - **Confiar en el valor** sin revalidar: es editable por el usuario.
> - **Guardar secretos** en hidden: están en el código fuente.
> - **Usar hidden para pasar datos a JS** en vez de `data-*`: hidden es para el envío del formulario.

## Notas relacionadas

- [[12 Atributos de Datos (data-*)]] — para datos destinados a JavaScript, no al envío.
- [[01 Contenedor de Formulario (form)]] — el formulario que transporta el campo.
- [[09 Validación de Formularios/index]] — por qué revalidar siempre en servidor.
