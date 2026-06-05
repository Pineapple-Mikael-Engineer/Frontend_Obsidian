---
title: input type="url" — Dirección web
aliases:
  - input url
tags:
  - html
  - api/elemento
  - formularios
elemento: input
categoria: interactivo
rol_implicito: textbox
vacio: true
draft: false
---

# input url

> [!definicion]
> `<input type="url">` es para **direcciones web**. Valida que el valor tenga formato de URL **absoluta** (con esquema, p. ej. `https://`) y, en móvil, muestra un teclado con `/`, `.` y `.com`.

```html
<label for="web">Sitio web</label>
<input type="url" id="web" name="web" placeholder="https://ejemplo.com" />
```

## Exige esquema

La validación de `url` requiere una URL **absoluta** con esquema. Esto sorprende: `ejemplo.com` sin `https://` **no es válido**:

| Valor | ¿Válido? |
|-------|----------|
| `https://ejemplo.com` | Sí |
| `http://localhost:3000` | Sí |
| `ejemplo.com` | **No** (falta el esquema) |
| `ftp://archivos.com` | Sí (cualquier esquema válido) |

## Ayudar al usuario a poner el esquema

Como muchos usuarios escriben `ejemplo.com` sin `https://`, conviene guiarlos con el `placeholder` y, si hace falta, normalizar con JavaScript al enviar (anteponer `https://` si falta). Restringir el esquema se hace con [[02 Atributo pattern | `pattern`]]:

```html
<!-- Solo https -->
<input type="url" pattern="https://.*" title="Debe empezar por https://" />
```

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `type="url"` para enlaces: validación de formato + teclado adecuado.
> - Indica el formato con un `placeholder` que incluya `https://`.
> - Si aceptas solo `https`, restríngelo con `pattern`.
> - Considera normalizar (añadir esquema) con JS para no frustrar al usuario.

## Errores comunes

> [!warning] Trampas
> - **Esperar que `ejemplo.com` valide**: necesita esquema.
> - **Usar `text`**: pierdes validación y teclado.
> - **Validación de formato ≠ URL accesible**: que sea sintácticamente válida no significa que el recurso exista.

## Notas relacionadas

- [[03 input email]] — el otro tipo con validación de formato.
- [[02 Atributo pattern]] — restringir el esquema o el dominio.
- [[01 Enlaces (a)]] — el elemento que usa estas URLs.
