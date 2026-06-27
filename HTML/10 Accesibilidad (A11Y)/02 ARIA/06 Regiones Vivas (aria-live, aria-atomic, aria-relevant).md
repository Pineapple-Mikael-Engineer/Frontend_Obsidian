---
title: Regiones Vivas — aria-live, aria-atomic, aria-relevant
aliases:
  - live regions
  - aria-live
tags:
  - html
  - api/concepto
  - a11y
draft: false
order: 6
---

# Regiones Vivas (aria-live, aria-atomic, aria-relevant)

> [!definicion]
> Una **región viva** (live region) es una zona cuyo contenido cambia dinámicamente y cuyos cambios deben **anunciarse** automáticamente al usuario de lector de pantalla, sin que él tenga que ir a buscarlos: una notificación, un mensaje de error, el resultado de una búsqueda, un contador del carrito.

```html
<div aria-live="polite" id="estado"></div>
```

```js
document.getElementById("estado").textContent = "Producto añadido al carrito.";
// El lector de pantalla lo anuncia automáticamente
```

## El problema que resuelve

> [!info] Lo que cambia sin recargar pasa desapercibido
> En una página dinámica (JavaScript actualiza partes sin recargar), un usuario que ve la pantalla nota los cambios al instante. Pero un usuario de lector de pantalla, que está enfocado en otra parte, **no se entera** de que apareció un mensaje de éxito o un error: el lector solo lee lo que está enfocado. Las regiones vivas resuelven esto: el lector **anuncia** los cambios de esa zona aunque el foco esté en otro sitio.

## aria-live: la urgencia

| Valor | Comportamiento |
|-------|----------------|
| `off` | No se anuncia (por defecto) |
| `polite` | Se anuncia **al terminar** lo que el lector esté diciendo (no interrumpe) |
| `assertive` | Se anuncia **inmediatamente**, interrumpiendo (solo para urgencias) |

```html
<div aria-live="polite">Cambios normales (resultados, estado)</div>
<div aria-live="assertive">Errores críticos, alertas urgentes</div>
```

> [!warning] assertive interrumpe: úsalo con cuidado
> `assertive` corta lo que el lector esté leyendo. Es apropiado para errores críticos o alertas urgentes, pero abusar de él (anunciar de golpe cada cambio menor) bombardea al usuario. La mayoría de actualizaciones son `polite`.

## aria-atomic y aria-relevant

| Propiedad | Controla |
|-----------|----------|
| `aria-atomic` | Si se anuncia **toda** la región (`true`) o solo lo que cambió (`false`, por defecto) |
| `aria-relevant` | Qué tipos de cambio se anuncian (`additions`, `removals`, `text`) |

`aria-atomic="true"` es útil cuando el mensaje solo tiene sentido completo: "Quedan 3 artículos" debe leerse entero, no solo el "3" que cambió.

## Roles con live region implícita

Algunos roles ya son regiones vivas sin declarar `aria-live`:

| Rol / elemento | Equivale a |
|----------------|------------|
| `role="status"` (y `<output>`) | `aria-live="polite"` |
| `role="alert"` | `aria-live="assertive"` |
| `role="log"` | `aria-live="polite"` (registro acumulativo) |

Por eso [[01 Resultado de Cálculo (output) | `<output>`]] anuncia sus cambios solo: tiene `role="status"`.

## La región debe existir antes

> [!warning] Crea la región vacía de antemano
> Un fallo habitual: insertar un `<div aria-live>` **con el mensaje ya dentro** justo en el momento. Muchos lectores no lo anuncian, porque la región no existía cuando se "encendió". Lo correcto es tener la región **vacía en el DOM desde el inicio** y luego **insertar el texto**; así el cambio se detecta y se anuncia.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `polite` para casi todo; `assertive` solo para urgencias reales.
> - Ten la región viva en el DOM desde el principio (vacía) y actualiza su texto.
> - Para mensajes que deben leerse completos, `aria-atomic="true"`.
> - Aprovecha `role="status"`/`role="alert"` (o `<output>`) en vez de configurar `aria-live` a mano.

## Errores comunes

> [!warning] Trampas
> - **Insertar la región ya con el mensaje**: puede no anunciarse.
> - **`assertive` para todo**: satura e interrumpe constantemente.
> - **Demasiadas regiones vivas**: cada cambio compite por ser anunciado.

## Notas relacionadas

- [[01 Resultado de Cálculo (output)]] — un elemento con live region implícita.
- [[05 Estados ARIA (expanded, hidden, checked, selected)]] — el estado dinámico, su complemento.
- [[05 Formularios Accesibles]] — los mensajes de error como regiones vivas.
