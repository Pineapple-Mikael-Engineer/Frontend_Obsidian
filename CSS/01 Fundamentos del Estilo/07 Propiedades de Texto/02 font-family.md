---
title: font-family — La tipografía del texto
aliases:
  - font-family
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: font-family
grupo: tipografia
valor_inicial: "depende del navegador"
hereda: true
animable: false
draft: false
order: 2
---

# font-family

> [!definicion]
> `font-family` define la **tipografía** del texto mediante una **lista de fuentes** en orden de preferencia: el navegador usa la primera disponible y, si ninguna lo está, recurre a una **familia genérica** de respaldo. Esa lista se llama *font stack*.

```css
body {
  font-family: "Inter", system-ui, -apple-system, sans-serif;
}
```

## La pila de fuentes (font stack)

> [!info] Por qué una lista y no una fuente
> El navegador prueba las fuentes **de izquierda a derecha** y usa la primera que tenga disponible (instalada o cargada). Por eso se listan varias, terminando **siempre** en una familia genérica que garantiza un respaldo:
> ```css
> font-family: "Georgia", "Times New Roman", serif;
> /*            preferida   respaldo concreto   genérica (último recurso) */
> ```
> Si "Georgia" no está, prueba "Times New Roman"; si tampoco, el navegador usa **su** fuente serif por defecto.

## Las familias genéricas

| Genérica | Aspecto | Ejemplos reales |
|----------|---------|------------------|
| `serif` | Con remates | Georgia, Times |
| `sans-serif` | Sin remates | Arial, Helvetica |
| `monospace` | Ancho fijo | Consolas, Menlo |
| `cursive` | Manuscrita | (varía) |
| `fantasy` | Decorativa | (varía) |
| `system-ui` | La fuente del sistema operativo | San Francisco, Segoe, Roboto |

## Comillas en los nombres

> [!warning] Nombres con espacios van entre comillas
> Un nombre de fuente con **espacios** o caracteres especiales debe ir entre comillas; las genéricas y nombres de una palabra, no:
> ```css
> font-family: "Times New Roman", Georgia, serif;   /* ✅ */
> font-family: Times New Roman, serif;               /* ❌ frágil */
> ```

## La pila del sistema (system font stack)

> [!tip] system-ui para rendimiento y nativez
> Una pila popular usa la **fuente nativa** de cada sistema operativo: no requiere descargar nada (cero impacto en carga) y el texto se ve "nativo" en cada plataforma:
> ```css
> font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
> ```
> `system-ui` resuelve esto en una palabra en navegadores modernos; los demás son respaldos para versiones antiguas.

## Cargar fuentes propias

Para usar una fuente que no está en el sistema del usuario, hay que **cargarla** con [[01 Fuentes Web (@font-face)/index | `@font-face`]] (o un servicio como Google Fonts) y luego nombrarla en `font-family`. La fuente debe estar disponible o el navegador caerá al respaldo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Termina **siempre** la pila en una familia genérica (`sans-serif`, `serif`…).
> - Pon entre comillas los nombres con espacios.
> - Considera la pila del sistema (`system-ui`) cuando no necesites una fuente de marca: es gratis y rápida.
> - Define `font-family` en `body` (se hereda) y sobrescribe solo donde cambie (código en `monospace`, etc.).

## Errores comunes

> [!warning] Trampas
> - **Sin genérica de respaldo**: si la fuente falla, el navegador elige una impredecible.
> - **Nombres con espacios sin comillas**.
> - **Una sola fuente** sin alternativas: frágil si no carga.

## Notas relacionadas

- [[01 Fuentes Web (@font-face)/index]] — cargar fuentes propias.
- [[03 font-size]] · [[04 font-weight]] — las otras propiedades de fuente.
- [[02 font-display]] — qué pasa mientras la fuente carga.
