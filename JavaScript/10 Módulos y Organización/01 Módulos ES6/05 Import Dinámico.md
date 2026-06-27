---
title: Import Dinámico — import() bajo demanda
aliases:
  - import dinámico
  - dynamic import
  - import()
  - code splitting
  - lazy loading módulos
tags:
  - javascript
  - api/concepto
  - modulos
objeto: global
tipo: funcion
retorna: Promise<Module>
asincrono: true
draft: false
order: 5
---

# Import Dinámico

> [!definicion]
> `import(ruta)` es la forma dinámica del sistema de módulos ESM. Devuelve una `Promise` que se resuelve con el objeto módulo (equivalente al namespace import `* as`). A diferencia del `import` estático, puede usarse en cualquier lugar del código: dentro de funciones, event listeners, condicionales o bucles. Permite cargar módulos bajo demanda en lugar de en el arranque.

```js
// Carga básica
const modulo = await import("./feature.js");
modulo.iniciar();

// Con destructuring
const { iniciar, configurar } = await import("./feature.js");
iniciar();

// El default export se accede como .default
const { default: parsear } = await import("./parser.js");
parsear("texto");
```

## Casos de uso

**Code splitting** — cargar una funcionalidad pesada solo cuando el usuario la necesita, reduciendo el JS inicial.

```js
boton.addEventListener("click", async () => {
  const { renderGrafico } = await import("./grafico.js");
  renderGrafico(datos);
});
```

**Carga condicional** — importar módulos según el entorno, configuración o permisos.

```js
let logger;
if (process.env.NODE_ENV === "development") {
  logger = await import("./logger-debug.js");
} else {
  logger = await import("./logger-prod.js");
}
```

**Internacionalización (i18n)** — cargar solo el idioma del usuario.

```js
const idioma = navigator.language.slice(0, 2); // "es", "en", "fr"
const { default: t } = await import(`./locales/${idioma}.js`);
document.title = t("titulo_app");
```

> [!warning]
> Las rutas template literal en `import()` como `` import(`./locales/${idioma}.js`) `` impiden que los bundlers conozcan los módulos en tiempo de compilación. Vite y Webpack hacen un análisis estático limitado: incluyen todos los archivos del patrón en el bundle, lo que puede ser el efecto deseado (incluir todos los idiomas disponibles) pero también puede generar chunks inesperados. Preferir patrones predecibles.

**Lazy loading de componentes UI** — en frameworks (o vanilla) cargar un componente solo al navegar a cierta ruta o al mostrar un modal.

```js
async function mostrarModal() {
  const { Modal } = await import("./components/Modal.js");
  const modal = new Modal({ titulo: "Confirmación" });
  modal.abrir();
}
```

## Comportamiento con bundlers

Con Vite, Webpack o Rollup, cada `import()` dinámico genera un **chunk separado**: un archivo JS adicional que se descarga solo cuando se ejecuta el `import()`. El bundler analiza el código estáticamente y divide el output.

```js
// Webpack — comentario mágico para nombrar el chunk
const modulo = await import(/* webpackChunkName: "grafico" */ "./grafico.js");

// Vite — nombre automático basado en el archivo; también acepta el comentario
```

En Vite, el nombre del chunk sigue el patrón `[name]-[hash].js` por defecto. Los comentarios mágicos de Webpack también son reconocidos por Vite.

## Resolución de rutas

`import()` respeta las mismas reglas que `import` estático: rutas relativas, absolutas o bare specifiers mapeados por un [[01 Módulos ES6/04 Import Maps | Import Map]]. El módulo importado dinámicamente se evalúa una sola vez: si ya fue cargado, la Promise resuelve con el módulo cacheado sin hacer una nueva petición de red.

```js
// Primera llamada — descarga y evalúa el módulo
const m1 = await import("./utils.js");

// Segunda llamada — devuelve el módulo cacheado (sin red)
const m2 = await import("./utils.js");

console.log(m1 === m2); // → true (mismo objeto módulo)
```

## Manejo de errores

Si la ruta no existe o hay un error de red/evaluación, la Promise se rechaza. Se gestiona con `try/catch` o `.catch()`.

```js
try {
  const { plugin } = await import("./plugins/experimental.js");
  plugin.activar();
} catch (err) {
  console.warn("Plugin no disponible:", err.message);
  // La app continúa sin el plugin
}
```

> [!tip]
> Para precargar un módulo dinámico (informar al browser sin ejecutarlo aún), usar `<link rel="modulepreload" href="./feature.js">` en el HTML. Cuando el `import()` se ejecute, el módulo ya estará en caché y el tiempo de respuesta percibido será mínimo.

## Notas relacionadas

- [[01 Módulos ES6/index | Módulos ES6]]
- [[01 Módulos ES6/02 import | import]]
- [[01 Módulos ES6/03 Módulos en el Navegador (type=module) | Módulos en el Navegador (type=module)]]
- [[01 Módulos ES6/04 Import Maps | Import Maps]]
- [[10 Módulos y Organización/03 Bundling (concepto) | Bundling (concepto)]]
