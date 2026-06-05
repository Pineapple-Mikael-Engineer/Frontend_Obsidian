---
title: title (atributo) — Información emergente (tooltip)
aliases:
  - title attribute
  - tooltip
tags:
  - html
  - api/atributo
  - atributos
elemento: global
categoria: ninguna
rol_implicito: ninguno
vacio: false
draft: false
---

# Información Emergente (title)

> [!definicion]
> El **atributo** `title` ofrece información **adicional** sobre un elemento, que el navegador muestra como un **tooltip** al pasar el cursor. No confundir con el **elemento** [[03 Título del Documento (title) | `<title>`]] (el título de la pestaña): son cosas distintas con el mismo nombre.

```html
<abbr title="Organización Mundial de la Salud">OMS</abbr>
<button title="Guardar los cambios">💾</button>
```

## Sus serias limitaciones

> [!warning] No dependas del title para información importante
> El atributo `title` tiene problemas que lo hacen poco fiable como canal de información:
> - **No funciona en táctil**: en móviles y tablets no hay "pasar el cursor", así que el tooltip **nunca aparece**.
> - **No es accesible por teclado**: un usuario que navega con `Tab` no puede invocarlo (salvo en algunos casos).
> - **Soporte irregular en lectores de pantalla**: no todos lo anuncian, y la configuración varía.
> - **No estilizable**: el aspecto del tooltip lo decide el navegador.
>
> Por eso, información **esencial** nunca debe ir solo en el `title`.

## Usos aceptables

| Uso | Aceptable porque… |
|-----|-------------------|
| Forma larga de una abreviatura | Es un apoyo, no el único canal |
| Aclaración secundaria de un enlace | Información complementaria, no crítica |
| `title` en un `<iframe>` | Aquí **sí** es obligatorio y accesible (nombre del marco) |

## La excepción: iframe

En [[01 Marco en Línea (iframe) | `<iframe>`]], `title` **sí** es un requisito de accesibilidad importante: es el nombre accesible del marco, que los lectores de pantalla anuncian. Es el caso donde `title` no es opcional ni secundario.

## Alternativas mejores

Para tooltips reales y accesibles, se construyen con CSS/JS y ARIA (`aria-describedby` apuntando a un elemento con el texto), que funcionan en táctil y por teclado. Para nombrar controles, [[04 Propiedades ARIA (label, labelledby, describedby) | `aria-label`]] es más fiable que `title`.

## Buenas prácticas

> [!tip] Recomendaciones
> - No pongas información **esencial** solo en `title` (no llega en táctil ni teclado).
> - Úsalo como apoyo secundario (forma larga de abreviaturas, aclaraciones menores).
> - En `<iframe>`, `title` **sí** es obligatorio.
> - Para tooltips accesibles de verdad, usa un componente con ARIA, no `title`.

## Errores comunes

> [!warning] Trampas
> - **Información crítica en `title`**: invisible en móvil.
> - **`title` como sustituto de `<label>`/`aria-label`**: poco fiable.
> - **Confundir el atributo `title` con el elemento `<title>`**.

## Notas relacionadas

- [[03 Título del Documento (title)]] — el **elemento** `<title>`, no confundir.
- [[15 Abreviaturas (abbr)]] — el uso más común del atributo `title`.
- [[01 Marco en Línea (iframe)]] — donde `title` sí es obligatorio.
