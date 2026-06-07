---
title: Utility-First CSS
aliases:
  - utility-first
  - tailwind css
  - atomic css
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Utility-First CSS

> [!definicion]
> **Utility-First** es una metodología donde cada clase CSS hace **exactamente una cosa**: una propiedad, un valor. En lugar de construir componentes con clases semánticas (`.card`, `.btn--primario`), se compone el diseño aplicando directamente las clases de utilidad en el HTML. Tailwind CSS es el framework más popular que implementa este enfoque.

```html
<!-- BEM: una clase semántica, estilos en CSS -->
<button class="btn btn--primario btn--grande">Guardar</button>

<!-- Utility-First: múltiples utilidades, diseño en el HTML -->
<button class="flex items-center px-6 py-3 bg-blue-600 text-white rounded-lg font-semibold hover:bg-blue-700 focus:ring-2">
  Guardar
</button>
```

## Los principios del enfoque

### 1. Clases de una sola responsabilidad

Cada clase hace exactamente una cosa. No hay acoplamiento de responsabilidades en la misma clase.

```css
/* Utilidades propias (sin framework) */
.flex { display: flex; }
.items-center { align-items: center; }
.gap-4 { gap: 1rem; }
.p-4 { padding: 1rem; }
.mt-2 { margin-top: 0.5rem; }
.text-sm { font-size: 0.875rem; }
.text-gray-700 { color: #374151; }
.rounded { border-radius: 4px; }
.border { border: 1px solid #e5e7eb; }
.hidden { display: none; }
```

### 2. El diseño vive en el HTML

El HTML pasa a ser la fuente de verdad del diseño. No hay saltos al CSS para entender cómo se ve un elemento; todo está en el atributo `class` del HTML.

```html
<!-- El CSS no dice nada sobre cómo se ve la card. El HTML lo describe todo. -->
<div class="flex flex-col gap-4 p-6 bg-white border border-gray-200 rounded-xl shadow-sm">
  <h2 class="text-xl font-bold text-gray-900">Título</h2>
  <p class="text-gray-600 leading-relaxed">Descripción del contenido.</p>
  <a class="inline-flex items-center text-blue-600 font-medium hover:underline" href="#">
    Leer más →
  </a>
</div>
```

### 3. El CSS no crece

Con utility-first, el CSS es un conjunto fijo de clases (o se genera en build time solo con las usadas). Añadir una nueva página o componente no añade más líneas al CSS: reutiliza las utilidades existentes. En proyectos grandes, el CSS resultante puede ser de 10-50KB en vez de cientos de KB de CSS semántico.

## Ventajas

### No hay guerras de especificidad

Todas las utilidades tienen la misma especificidad `(0,1,0)`. No hay IDs, no hay anidaciones, no hay colisiones. Si necesitas sobrescribir, añades otra utilidad (o la mismsa con un valor diferente).

### Iteración rápida

No hay que saltar entre HTML y CSS para hacer un cambio. Todo el diseño está en el HTML: ajustar un espaciado, un color o un tamaño es editar el atributo `class`.

### Sin "CSS muerto"

En enfoques tradicionales, el CSS acumula reglas para componentes que se eliminaron pero cuyo CSS quedó. Con utility-first + un proceso de purga (como el de Tailwind CSS), solo se genera el CSS de las clases realmente usadas en el HTML.

## Desventajas y críticas

### HTML verboso y difícil de leer

El principal argumento en contra: el atributo `class` se vuelve una cadena larga que requiere esfuerzo para leer. Con Tailwind esto se mitiga con la extensión de editor y con las convenciones del orden de clases, pero el problema existe.

### Repetición de patrones

Si diez botones tienen las mismas 12 clases, esa lista se repite diez veces en el HTML. Tailwind lo mitiga con la directiva `@apply` para extraer componentes frecuentes, pero muchos en la comunidad prefieren usar componentes de framework (React, Vue) en vez de `@apply`.

### Dificulta la colaboración diseñador-desarrollador

Un diseñador que inspecciona el HTML ve muchas clases crípticas (o nombres de utilidad) en vez de un nombre de componente descriptivo.

## Cuándo tiene sentido

Utility-First encaja especialmente bien en:
- Proyectos con un **sistema de diseño** fijo (Tailwind genera las utilidades a partir de tokens de diseño).
- Equipos donde el **HTML es el artefacto principal** (no hay un diseño en Figma que evolucione constantemente).
- **Prototipos y proyectos personales** donde la velocidad de iteración es más importante que la legibilidad del HTML.
- Proyectos con **SSR/SSG** donde el HTML generado es el principal canal (Astro, Next.js, SvelteKit).

No encaja bien en:
- Proyectos donde el CSS lo mantiene un equipo distinto al del HTML/JS.
- Sistemas donde el diseño cambia radicalmente entre versiones (las clases del HTML se quedan desactualizadas).

## Tailwind CSS — utility-first con sistema de diseño

Tailwind no es solo "clases de utilidad": genera las utilidades a partir de un archivo de configuración (`tailwind.config.js`) que define la escala de espaciado, la paleta de colores, los breakpoints, etc. Esto garantiza consistencia: no hay "16px aquí, 18px allá"; hay una escala de valores fija.

```js
// tailwind.config.js (simplificado)
module.exports = {
  theme: {
    spacing: { 0: '0', 1: '0.25rem', 2: '0.5rem', 4: '1rem', /* ... */ },
    colors: { blue: { 500: '#3b82f6', 600: '#2563eb' }, /* ... */ },
  }
}
```

```html
<!-- p-4 = padding: 1rem (de la escala); bg-blue-600 = background: #2563eb -->
<button class="p-4 bg-blue-600 text-white rounded">Enviar</button>
```

## Utility-First con CSS propio (sin framework)

Se puede implementar un sistema de utilidades sin Tailwind, con un conjunto reducido de clases de uso frecuente. La clave es limitar el conjunto para no recrear un framework desde cero:

```css
/* Utilidades propias — conjunto acotado */
.flex { display: flex; }
.inline-flex { display: inline-flex; }
.grid { display: grid; }
.hidden { display: none; }
.sr-only { position: absolute; width: 1px; height: 1px; padding: 0; overflow: hidden; clip: rect(0,0,0,0); }
.items-center { align-items: center; }
.justify-between { justify-content: space-between; }
.gap-2 { gap: 0.5rem; }
.gap-4 { gap: 1rem; }
.mt-auto { margin-top: auto; }
.truncate { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
```

Un conjunto de 20-30 utilidades bien elegidas complementa BEM sin necesidad de adoptar un framework completo.

## Recetas comunes

### Centrado perfecto (Utility-First)

```html
<div class="flex items-center justify-center min-h-screen">
  Centrado vertical y horizontal
</div>
```

### Grid responsivo

```html
<ul class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
  <!-- items -->
</ul>
```

### Botones con variantes

```html
<button class="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">
  Primario
</button>
<button class="px-4 py-2 border border-blue-600 text-blue-600 rounded hover:bg-blue-50">
  Secundario
</button>
```

## Cómo funciona por dentro

En Tailwind, cada clase de utilidad corresponde exactamente a una declaración CSS (o a un pequeño conjunto). El CLI de Tailwind escanea el HTML/JS en busca de clases que coincidan con los patrones registrados en la configuración y genera solo el CSS necesario. En modo `JIT` (just-in-time), el CSS se genera en tiempo real al guardar el archivo.

## Buenas prácticas

> [!tip] Recomendaciones
> - Extrae los patrones repetidos a **componentes de framework** (React, Vue, Svelte) en vez de usar `@apply` — es más idiomático.
> - Mantén un **orden de clases consistente**: layout → tipografía → colores → estados — facilita leer el `class`.
> - Usa el **plugin de Prettier de Tailwind** para ordenar clases automáticamente.
> - Si builds tus propias utilidades, define un número **acotado**: más de 50 utilidades propias es una señal de que deberías usar un framework.

## Errores comunes

> [!warning] Trampas
> - **Repetir largas listas de clases sin extraer a componente**: el HTML se vuelve imposible de mantener.
> - **Mezclar utility-first con semántica arbitraria**: `.card` con `mt-4 p-6` tiene la responsabilidad de ambos enfoques, sin las ventajas de ninguno.
> - **No purgar el CSS**: generar todas las utilidades sin usar puede producir un CSS de 3MB.
> - **Usar `@apply` extensivamente**: convierte utility-first en BEM pero con nombres peores.

## Notas relacionadas

- [[index]] — las metodologías en contexto.
- [[01 BEM]] — la alternativa semántica por componentes.
- [[03 OOCSS]] — el antecedente conceptual de utility-first.
- [[10 Variables CSS/index]] — los design tokens que alimentan los sistemas de utilidades.
