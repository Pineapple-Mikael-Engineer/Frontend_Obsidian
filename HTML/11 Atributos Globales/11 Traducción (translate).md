---
title: translate — Permitir o no la traducción automática
aliases:
  - translate attribute
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

# Traducción (translate)

> [!definicion]
> `translate` indica si el contenido de un elemento **debe traducirse** cuando se usa la traducción automática (Google Translate, el traductor del navegador) o si debe **dejarse tal cual**. Sirve para proteger nombres propios, marcas, código y términos que no deben alterarse.

```html
<p>Visita <span translate="no">Recetas de la Abuela</span> hoy.</p>
```

## Valores

| Valor | Efecto |
|-------|--------|
| `yes` (por defecto) | El contenido se puede traducir |
| `no` | El contenido **no** debe traducirse |

## Qué proteger con translate="no"

> [!info] Lo que no debe traducirse
> Por defecto, los traductores intentan traducir todo. `translate="no"` protege lo que perdería sentido o se rompería al traducirse:
> | Contenido | Por qué no traducir |
> |-----------|---------------------|
> | Nombres de marca/producto | "Recetas de la Abuela" no es "Grandma's Recipes Inc." |
> | Nombres propios de personas | No se traducen |
> | Código y comandos | `git push` traducido rompería las instrucciones |
> | Nombres de usuario | Identificadores literales |
> | Términos técnicos específicos | A veces deben quedar en el original |

```html
<p>Ejecuta <code translate="no">npm install</code> en la terminal.</p>
```

## Hereda a los descendientes

`translate` se **hereda**: un `translate="no"` en un contenedor protege todo su contenido. Esto permite marcar bloques enteros (un fragmento de código, una tabla de comandos) de una vez.

## Buenas prácticas

> [!tip] Recomendaciones
> - Marca con `translate="no"` nombres de marca, código, comandos y nombres propios.
> - Aprovecha la herencia para proteger bloques completos.
> - No abuses: la mayoría del contenido **sí** debe poder traducirse (deja el valor por defecto).
> - Combínalo con [[04 Idioma y Dirección (lang, dir) | `lang`]] para fragmentos que sí están en otro idioma (que el traductor debe entender, no que no deba tocar).

## Errores comunes

> [!warning] Trampas
> - **No proteger el código**: instrucciones traducidas que dejan de funcionar.
> - **Traducir marcas**: el nombre del producto cambia y confunde.
> - **Abusar de `translate="no"`**: impide traducir contenido que sí debería traducirse.

## Notas relacionadas

- [[04 Idioma y Dirección (lang, dir)]] — para marcar el idioma de un fragmento.
- [[17 Código (code)]] — un caso típico de `translate="no"`.
