---
title: Import Maps — Mapear bare specifiers a URLs en el navegador
aliases:
  - import map
  - importmap
  - bare specifiers
tags:
  - javascript
  - api/concepto
  - modulos
objeto: global
tipo: concepto
draft: false
---

# Import Maps

> [!definicion]
> Los Import Maps son un mecanismo del navegador que permite mapear **bare specifiers** (nombres de paquete sin ruta, como `"lodash"`) a URLs reales. Se declaran como `<script type="importmap">` en el HTML. Eliminan la necesidad de un bundler solo para resolver nombres de paquetes en ESM nativo del browser.

```html
<script type="importmap">
{
  "imports": {
    "lodash": "https://cdn.jsdelivr.net/npm/lodash-es@4/lodash.js",
    "react": "/node_modules/react/index.js",
    "utils/": "/src/utils/"
  }
}
</script>

<script type="module">
  import _ from "lodash";                   // → resuelto a la URL del CDN
  import { formatear } from "utils/date.js"; // → /src/utils/date.js
  import { parsear } from "utils/xml.js";   // → /src/utils/xml.js
</script>
```

El mapeo `"utils/"` (con barra final) actúa como **prefijo**: cualquier specifier que empiece por `utils/` resuelve sustituyendo el prefijo por la URL base indicada.

## Estructura del Import Map

```json
{
  "imports": {
    "lodash": "https://cdn.jsdelivr.net/npm/lodash-es@4/lodash.js",
    "mi-lib": "/src/lib/index.js"
  },
  "scopes": {
    "/legacy/": {
      "lodash": "https://cdn.jsdelivr.net/npm/lodash-es@3/lodash.js"
    }
  }
}
```

**`"imports"`** — mapeos globales que aplican a todos los módulos de la página.

**`"scopes"`** — mapeos que solo aplican cuando el módulo importador tiene una URL dentro del scope indicado. Sirve para gestionar conflictos de versiones: un módulo en `/legacy/` puede usar `lodash@3` mientras el resto usa `lodash@4`.

## Soporte de navegadores

| Navegador | Versión mínima |
|-----------|---------------|
| Chrome | 89+ |
| Firefox | 108+ |
| Safari | 16.4+ |
| Edge | 89+ |

Para soporte en navegadores más antiguos: el polyfill **`es-module-shims`** intercepta la carga de módulos y aplica el Import Map por software.

```html
<script async src="https://ga.jspm.io/npm:es-module-shims@1/dist/es-module-shims.js"></script>
<script type="importmap"> { ... } </script>
<script type="module"> import "lodash"; </script>
```

## Cuándo usar

- **Desarrollo sin bundler** — proyectos pequeños, demos, prototipos, cursos. Se importan paquetes desde CDN con nombres legibles.
- **Override en testing** — se puede reemplazar un módulo de producción por un mock cambiando solo el Import Map en el HTML de test.
- **Vite en desarrollo** — Vite gestiona su propio Import Map interno durante el dev server para servir dependencias de `node_modules` como ESM sin configuración manual.

> [!warning]
> Solo puede haber **un** `<script type="importmap">` por página. Debe declararse **antes** de cualquier `<script type="module">` que use los specifiers mapeados — el navegador aplica el mapa solo a los imports que se resuelven después de leerlo. En la práctica: poner el importmap al inicio del `<head>`.

> [!tip]
> Los Import Maps solo afectan a `import` estático y a `import()` dinámico en módulos ES. No afectan a `<script src>`, `<link href>` ni `fetch()`.

## Cómo funciona por dentro

El Import Map es procesado por el navegador como parte del módulo de especificación WHATWG "HTML". Antes de resolver cualquier `import`, el motor consulta el mapa: si el specifier coincide con una entrada en `"imports"` (o con un prefijo), sustituye el specifier por la URL mapeada y lanza la petición fetch con CORS. Si no hay coincidencia, lanza `TypeError: Failed to resolve module specifier`.

## Notas relacionadas

- [[01 Módulos ES6/index | Módulos ES6]]
- [[01 Módulos ES6/02 import | import]]
- [[01 Módulos ES6/03 Módulos en el Navegador (type=module) | Módulos en el Navegador (type=module)]]
- [[10 Módulos y Organización/03 Bundling (concepto) | Bundling (concepto)]]
