---
title: <object> — Objeto incrustado genérico
aliases:
  - object element
tags:
  - html
  - api/elemento
  - multimedia
elemento: object
categoria: incrustado
rol_implicito: ninguno
vacio: false
draft: false
order: 2
---

# Objeto Incrustado (object)

> [!definicion]
> `<object>` incrusta un **recurso externo genérico**: un PDF, una imagen, otra página, o (históricamente) un plugin. Su rasgo distintivo es que admite **contenido de respaldo** entre sus etiquetas, que se muestra si el recurso no puede cargarse.

```html
<object data="manual.pdf" type="application/pdf" width="600" height="800">
  <p>No se puede mostrar el PDF.
     <a href="manual.pdf">Descárgalo aquí</a>.</p>
</object>
```

## Atributos

| Atributo | Descripción |
|----------|-------------|
| `data` | URL del recurso a incrustar (equivale al `src`) |
| `type` | Tipo MIME del recurso (`application/pdf`, `image/svg+xml`) |
| `width` / `height` | Dimensiones |
| `name` | Nombre del objeto |

## El contenido de respaldo

> [!info] Fallback integrado
> A diferencia del [[01 Marco en Línea (iframe) | `<iframe>`]], el contenido entre `<object>` y `</object>` actúa como **respaldo**: se muestra solo si el navegador no puede renderizar el recurso. Es ideal para PDFs: si el navegador los incrusta, se ven; si no, el usuario ve un enlace de descarga.
> ```html
> <object data="doc.pdf" type="application/pdf">
>   <a href="doc.pdf">Descargar el documento</a>   <!-- respaldo -->
> </object>
> ```

## object vs. iframe

| | `<object>` | `<iframe>` |
|--|-----------|------------|
| Contenido de respaldo | Sí (entre etiquetas) | No |
| Uso típico hoy | PDF, recursos con fallback | Páginas, vídeos, widgets |
| `sandbox` | No | Sí |
| Frecuencia de uso | Bajo | Alto |

Para incrustar páginas y servicios web, `<iframe>` es el estándar. `<object>` sobrevive sobre todo para PDFs con respaldo.

## El pasado: plugins

`<object>` se usaba para incrustar plugins (Flash, Java applets), hoy **extintos** por seguridad. Ese uso está obsoleto; el elemento permanece para recursos como PDF.

## Buenas prácticas

> [!tip] Recomendaciones
> - Úsalo para PDFs y recursos donde el contenido de respaldo aporte (enlace de descarga).
> - Indica siempre el `type` MIME.
> - Para incrustar páginas web o servicios, prefiere `<iframe>`.

## Errores comunes

> [!warning] Trampas
> - **Sin contenido de respaldo**: se pierde la principal ventaja de `<object>`.
> - **Usarlo para plugins**: obsoleto e inseguro.
> - **Omitir `type`**: el navegador puede no saber cómo tratar el recurso.

## Notas relacionadas

- [[01 Marco en Línea (iframe)]] — el estándar para incrustar páginas.
- [[03 Incrustación Genérica (embed)]] — la otra alternativa, más simple.
