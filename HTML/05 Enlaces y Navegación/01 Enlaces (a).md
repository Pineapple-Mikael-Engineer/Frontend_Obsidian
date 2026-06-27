---
title: <a> — Enlaces de hipertexto
aliases:
  - anchor
  - enlace
  - hyperlink
tags:
  - html
  - api/elemento
  - navegacion
elemento: a
categoria: fraseo
rol_implicito: link
vacio: false
draft: false
order: 1
---

# Enlaces (a)

> [!definicion]
> `<a>` (anchor) crea un **hipervínculo** a otro recurso: una página, una sección, un archivo, un correo o un teléfono. El destino lo fija el atributo `href`. Es el elemento que define la web como red de documentos conectados.

```html
<a href="https://ejemplo.com">Visita Ejemplo</a>
```

## Atributos

| Atributo | Valores | Descripción |
|----------|---------|-------------|
| `href` | URL, `#id`, `mailto:`, `tel:` | Destino del enlace |
| `target` | `_self` · `_blank` · `_parent` · `_top` | Dónde abrir el destino |
| `rel` | `noopener`, `noreferrer`, `nofollow`… | Relación con el destino |
| `download` | (vacío o nombre de archivo) | Descarga el recurso en lugar de navegar |
| `hreflang` | código de idioma | Idioma del recurso enlazado |
| `type` | tipo MIME | Tipo de contenido del destino |

## target: dónde se abre

| `target` | Efecto |
|----------|--------|
| `_self` | En la misma pestaña (por defecto) |
| `_blank` | En una pestaña/ventana nueva |
| `_parent` / `_top` | En el frame padre o en la ventana completa (con iframes) |

> [!warning] target="_blank" necesita rel
> Abrir en pestaña nueva con `target="_blank"` tiene una trampa de seguridad histórica: la página destino podía acceder a `window.opener` y manipular la pestaña original (ataque *tabnabbing*). Los navegadores modernos ya añaden `noopener` implícito, pero conviene ser explícito:
> ```html
> <a href="https://externo.com" target="_blank" rel="noopener noreferrer">Sitio externo</a>
> ```
> `noopener` corta el acceso a `window.opener`; `noreferrer` además oculta la cabecera `Referer`.

## rel: la relación con el destino

| `rel` | Para qué |
|-------|----------|
| `noopener` | Impide que el destino acceda a `window.opener` (seguridad) |
| `noreferrer` | No envía la cabecera `Referer` |
| `nofollow` | Indica a los buscadores que no transmitan autoridad SEO |
| `sponsored` | Marca enlaces patrocinados/de pago |
| `ugc` | Contenido generado por usuarios (comentarios) |

## download: forzar descarga

Con `download`, el navegador descarga el recurso en vez de abrirlo. Un valor opcional renombra el archivo:

```html
<a href="/docs/manual.pdf" download>Descargar manual</a>
<a href="/img/grafico.png" download="ventas-2026.png">Descargar gráfico</a>
```

> [!info] download solo funciona en el mismo origen
> Por seguridad, `download` solo respeta el renombrado y la descarga forzada para recursos del **mismo origen** (o `blob:`/`data:`). Para archivos de otro dominio, el navegador puede ignorar el atributo y navegar normalmente.

## Texto de enlace accesible

> [!info] El texto debe describir el destino
> El contenido del `<a>` es lo que los lectores de pantalla anuncian y lo que se lista al navegar por enlaces. Debe describir **adónde lleva**, fuera de contexto:
> - ❌ `<a href="...">haz clic aquí</a>`, `<a href="...">leer más</a>` (no dicen nada al listarlos).
> - ✅ `<a href="...">Descargar el informe anual 2026</a>`.
>
> Un enlace solo de icono necesita texto accesible (`aria-label` o texto oculto visualmente).

## Enlaces que envuelven bloques

Desde HTML5, `<a>` puede envolver **contenido de bloque** (una tarjeta entera: imagen + título + texto), no solo texto en línea:

```html
<a href="/producto/1" class="tarjeta">
  <img src="foto.jpg" alt="Zapatillas rojas" />
  <h3>Zapatillas rojas</h3>
  <p>49,99 €</p>
</a>
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Texto de enlace descriptivo, que tenga sentido fuera de contexto.
> - `target="_blank"` siempre con `rel="noopener"` (y `noreferrer` si procede).
> - Usa rutas relativas para enlaces internos; absolutas solo para recursos externos.
> - Marca como `nofollow`/`sponsored`/`ugc` los enlaces que lo requieran (SEO, contenido de usuarios).

## Errores comunes

> [!warning] Trampas
> - **"Haz clic aquí"** como texto: inútil para la accesibilidad y el SEO.
> - **`target="_blank"` sin `rel`** en enlaces externos: riesgo de tabnabbing (aunque mitigado en navegadores modernos).
> - **Usar un `<a>` sin `href`** como botón: un `<a>` sin destino no es accesible por teclado como enlace; para acciones usa [[06 Botones (button) | `<button>`]].
> - **`<a href="#">`** para disparar JavaScript: usa `<button>` para acciones; los enlaces son para navegar.

## Notas relacionadas

- [[02 Anclas Internas (id)]] — enlazar a secciones con `#id`.
- [[03 Enlaces a Correo (mailto)]] · [[04 Enlaces Telefónicos (tel)]] — destinos especiales.
- [[06 Botones (button)]] — para acciones, en lugar de enlaces falsos.
