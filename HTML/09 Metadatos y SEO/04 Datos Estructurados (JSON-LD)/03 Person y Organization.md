---
title: Schema Person y Organization — Autores y empresas
aliases:
  - person schema
  - organization schema
tags:
  - html
  - api/concepto
  - metadatos
draft: false
---

# Person y Organization

> [!definicion]
> `Person` describe a una **persona** (un autor, un profesional) y `Organization` a una **entidad** (empresa, medio, marca). Aparecen como tipos independientes o, muy a menudo, **anidados** dentro de otros (el `author` de un [[02 Article | `Article`]], el `publisher` de un contenido).

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Recetas de la Abuela",
  "url": "https://sitio.com",
  "logo": "https://sitio.com/logo.png",
  "sameAs": [
    "https://twitter.com/RecetasAbuela",
    "https://www.instagram.com/recetasabuela"
  ]
}
</script>
```

## Person

| Propiedad | Para qué |
|-----------|----------|
| `name` | Nombre de la persona |
| `url` | Su página o perfil |
| `image` | Foto |
| `jobTitle` | Cargo |
| `worksFor` | La `Organization` donde trabaja |
| `sameAs` | Perfiles oficiales (redes, Wikipedia) |

## Organization

| Propiedad | Para qué |
|-----------|----------|
| `name` | Nombre de la entidad |
| `url` | Sitio oficial |
| `logo` | Logotipo (clave para el Knowledge Panel) |
| `sameAs` | Perfiles oficiales en redes y Wikipedia |
| `contactPoint` | Datos de contacto (teléfono, tipo) |

## sameAs: conectar identidades

> [!info] sameAs construye la entidad
> La propiedad `sameAs` enlaza a los **perfiles oficiales** de la persona u organización (Twitter, LinkedIn, Instagram, Wikipedia, Wikidata). Ayuda a Google a confirmar que se trata de la misma entidad en toda la web, lo que alimenta el **Knowledge Panel** (el recuadro de información a la derecha de los resultados) y la desambiguación.

## Anidamiento típico

Lo más habitual es usarlos **dentro** de otro tipo, no sueltos:

```json
"author": { "@type": "Person", "name": "Ana López", "url": "https://sitio.com/ana" },
"publisher": { "@type": "Organization", "name": "Diario X", "logo": "…" }
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Para empresas, define una `Organization` con `logo` y `sameAs` (alimenta el Knowledge Panel).
> - Para autores, una `Person` con `url`, `image` y `sameAs`.
> - Anídalos como `author`/`publisher` en artículos y otros tipos.
> - `sameAs` solo a perfiles **oficiales** y verificables.

## Errores comunes

> [!warning] Trampas
> - **`sameAs` a perfiles no oficiales** o ajenos: confunde la entidad.
> - **`Organization` sin `logo`**: pierde un requisito para varios rich results.
> - **Datos de contacto inventados** que no están en la página.

## Notas relacionadas

- [[02 Article]] — donde se anidan como `author`/`publisher`.
- [[09 LocalBusiness]] — un subtipo de `Organization` con dirección y horario.
- [[01 Schema.org Básico]] — la sintaxis y el anidamiento.
