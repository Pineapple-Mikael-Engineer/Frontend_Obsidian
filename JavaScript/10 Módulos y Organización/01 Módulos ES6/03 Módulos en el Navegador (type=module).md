---
title: Módulos en el Navegador — type=module
aliases:
  - type=module
  - script type module
  - ESM en browser
  - modulepreload
  - nomodule
tags:
  - javascript
  - api/concepto
  - modulos
objeto: global
tipo: concepto
draft: false
order: 3
---

# Módulos en el Navegador (type=module)

> [!definicion]
> Para usar ESM en el navegador se carga el punto de entrada con `<script type="module">`. El motor reconoce el archivo como módulo: aplica scope de módulo, modo estricto implícito, carga diferida y resolución relativa de rutas. Cada módulo importado se descarga y evalúa una sola vez, independientemente de cuántos scripts lo importen.

```html
<!-- index.html -->
<script type="module" src="main.js"></script>

<!-- También inline -->
<script type="module">
  import { suma } from "./math.js";
  console.log(suma(1, 2)); // → 3
</script>
```

## Comportamiento de type="module"

**Modo estricto implícito** — equivalente a poner `"use strict"` al principio de cada archivo. No se puede usar `with`, las variables no declaradas lanzan `ReferenceError`, etc.

**Diferido por defecto (defer)** — el script se descarga en paralelo con el HTML pero se ejecuta después de que el documento esté parseado. No bloquea el renderizado. Equivale a `<script defer>` en scripts clásicos.

**Ejecución única** — si `math.js` es importado desde `main.js` y desde `utils.js`, el motor lo evalúa una sola vez y los dos importadores comparten el mismo binding. Esto garantiza singletons de módulo.

**Scope de módulo** — las variables declaradas en el módulo no son accesibles desde `window`. `window.miVar = 1` debe hacerse explícitamente si se quiere global.

**Resolución relativa** — las rutas de `import` se resuelven desde la URL del archivo que hace el import, no desde el documento HTML.

**CORS obligatorio** — el navegador carga módulos con fetch (modo CORS). Un módulo en `file://` lanza un error de CORS. Se necesita un servidor HTTP local.

## Diferencias entre modos de script

| Característica | `<script>` | `<script defer>` | `<script type="module">` |
|----------------|------------|------------------|--------------------------|
| Scope global | Sí | Sí | No (scope de módulo) |
| Modo estricto | No | No | Sí (implícito) |
| Bloquea HTML | Sí | No | No |
| Orden de ejecución | En orden DOM | Orden DOM tras parse | Orden DOM tras parse |
| Ejecución única | No aplica | No aplica | Sí (por módulo) |
| Requiere servidor | No | No | Sí (CORS) |
| Soporta import | No | No | Sí |

## Atributos adicionales

**`crossorigin`** — permite cargar módulos desde otro origen con credenciales (cookies, certificados). Equivale a `crossorigin="anonymous"` si no se especifica valor; `crossorigin="use-credentials"` para incluir credenciales.

```html
<script type="module" crossorigin src="https://cdn.ejemplo.com/lib.js"></script>
```

**`<link rel="modulepreload">`** — precarga módulos en el parser (antes de que el JS los importe) sin ejecutarlos. Mejora el rendimiento al eliminar la cascada de peticiones de red que genera el grafo de módulos.

```html
<link rel="modulepreload" href="./math.js">
<link rel="modulepreload" href="./parser.js">
<script type="module" src="main.js"></script>
```

**`<script nomodule>`** — fallback para navegadores sin soporte ESM. Los navegadores que soportan `type="module"` ignoran `nomodule`; los que no soportan ESM ignoran `type="module"` y ejecutan `nomodule`.

```html
<script type="module" src="main.modern.js"></script>
<script nomodule src="main.bundle.js"></script>
```

## Cómo funciona por dentro

Cuando el parser encuentra `<script type="module" src="main.js">`, el navegador:
1. Descarga `main.js` sin bloquear el parseo del HTML.
2. Parsea `main.js` y detecta sus `import` estáticos.
3. Descarga recursivamente todos los módulos del grafo (pueden ir en paralelo).
4. Espera a que el HTML esté completamente parseado.
5. Evalúa los módulos en orden de dependencia (hojas primero).
6. Ejecuta `main.js`.

Sin `<link rel="modulepreload">` esto genera una cascada: el navegador no sabe qué módulos necesita hasta que descarga y parsea cada archivo. El preload corta la cascada informando al navegador del grafo completo desde el HTML.

> [!tip]
> En desarrollo con Vite o cualquier dev server moderno, el servidor ya sirve los módulos con las cabeceras CORS correctas y gestiona el grafo de dependencias. El `type="module"` es el mecanismo nativo que Vite aprovecha para Hot Module Replacement (HMR) sin bundling durante desarrollo.

> [!warning]
> Un `<script type="module">` inline con `import` de rutas relativas resuelve esas rutas respecto al **documento HTML**, no respecto al script inline. Para evitar confusión, usar siempre `src` externo o `import.meta.url` para rutas relativas en scripts inline.

## Notas relacionadas

- [[01 Módulos ES6/index | Módulos ES6]]
- [[01 Módulos ES6/02 import | import]]
- [[01 Módulos ES6/04 Import Maps | Import Maps]]
- [[10 Módulos y Organización/03 Bundling (concepto) | Bundling (concepto)]]
