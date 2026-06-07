---
title: Estructura Modular de CSS
aliases:
  - estructura modular css
  - css modular
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Estructura Modular

> [!definicion]
> La **estructura modular** organiza el CSS en archivos separados según su función o el componente al que pertenecen. Cada archivo es una unidad cohesiva que puede importarse de forma independiente. Es el punto de partida para cualquier proyecto que supere los 200-300 líneas de CSS y la base del patrón 7-1.

```
styles/
├── main.css            ← punto de entrada (solo importaciones)
├── base/
│   ├── reset.css
│   └── typography.css
├── tokens/
│   └── variables.css
├── layout/
│   ├── grid.css
│   └── header.css
├── components/
│   ├── btn.css
│   ├── card.css
│   ├── nav.css
│   └── modal.css
└── utilities/
    └── helpers.css
```

## El archivo de entrada

El punto de entrada (`main.css` o `index.css`) **solo contiene importaciones**, en el orden correcto. No tiene estilos propios. Es el mapa del CSS del proyecto.

```css
/* main.css — solo @import, en orden de cascada */
@layer reset, tokens, base, layout, components, utilities;

@import "base/reset.css" layer(reset);
@import "tokens/variables.css" layer(tokens);
@import "base/typography.css" layer(base);
@import "layout/grid.css" layer(layout);
@import "layout/header.css" layer(layout);
@import "components/btn.css" layer(components);
@import "components/card.css" layer(components);
@import "components/nav.css" layer(components);
@import "utilities/helpers.css" layer(utilities);
```

## Estructura de un archivo de componente

Cada archivo de componente es autocontenido: tiene todo lo que ese componente necesita, sin importar nada externo (salvo las variables del token layer, que están disponibles por ser una capa anterior).

```css
/* components/btn.css */
.btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  border-radius: var(--radius-md);
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s, opacity 0.2s;
}

.btn--primario {
  background: var(--color-primary);
  color: white;
  border: none;
}

.btn--primario:hover { background: var(--color-primary-dark); }
.btn--primario:disabled { opacity: 0.5; pointer-events: none; }

.btn--secundario {
  background: transparent;
  color: var(--color-primary);
  border: 1px solid currentColor;
}

.btn--icono {
  padding: 0.5rem;
  aspect-ratio: 1;
}
```

## Criterios para dividir en archivos

No existe una regla universal, pero algunos criterios útiles:

- **Por componente** (lo más habitual): un archivo por cada bloque BEM o componente de UI (`btn.css`, `card.css`, `modal.css`).
- **Por página** (para estilos muy específicos): `home.css`, `perfil.css`, `checkout.css`. Solo si los estilos no son reutilizables.
- **Por feature/función**: si el proyecto está organizado por features, los estilos van con la feature (`feature/auth/auth.css`).
- **Umbral de tamaño**: un archivo con más de 200-300 líneas suele ser señal de que tiene demasiadas responsabilidades.

## CSS Modules (en frameworks de componentes)

En proyectos con React, Vue o Svelte, los CSS Modules o los estilos de componente (`*.module.css`, `<style scoped>`) son la versión automática de la estructura modular: cada componente tiene su propio archivo de estilos, y el compilador garantiza el aislamiento mediante hashing de nombres de clase.

```css
/* Btn.module.css */
.btn { /* ... */ }           /* se compila a .Btn_btn_x3f9 — aislado automáticamente */
.btn--primario { /* ... */ }
```

```jsx
import styles from './Btn.module.css';
<button className={styles.btn}>Acción</button>
```

La lógica es la misma que en la estructura modular manual, pero el aislamiento lo garantiza la herramienta.

## Recetas comunes

### Estructura mínima para un proyecto mediano

```
styles/
├── main.css
├── reset.css
├── variables.css
└── components/
    ├── btn.css
    ├── card.css
    └── nav.css
```

### Estructura escalable con @layer

```css
/* main.css */
@layer reset, tokens, base, components, utilities;

@import "reset.css" layer(reset);
@import "variables.css" layer(tokens);
@import "typography.css" layer(base);
@import "components/btn.css" layer(components);
@import "components/card.css" layer(components);
@import "utilities.css" layer(utilities);
```

## Cómo funciona por dentro

En el navegador, todos los `@import` se resuelven antes de que el CSS empiece a aplicarse. El parser los encola como peticiones HTTP adicionales y las procesa en el orden declarado. Con un bundler (Vite, webpack, Parcel), los `@import` se resuelven en tiempo de build: el resultado es un único archivo CSS consolidado donde el orden de importación determina el orden de las declaraciones.

## Buenas prácticas

> [!tip] Recomendaciones
> - El archivo de entrada debe ser solo importaciones: sin estilos propios.
> - Nombra los archivos igual que el componente BEM que contienen: `card.css` para `.card`.
> - Usa `@layer` en el punto de entrada para declarar el orden de prioridad explícitamente.
> - En producción, usa un bundler: los `@import` en cascada son bloqueantes en el navegador.
> - Un archivo por componente es el granulado correcto: ni un archivo para todo, ni un archivo por selector.

## Errores comunes

> [!warning] Trampas
> - **Estilos en el punto de entrada**: el `main.css` con estilos mezclados con importaciones es difícil de mantener.
> - **Importaciones circulares**: `a.css` importa `b.css` que importa `a.css` — los bundlers lo detectan, pero conviene evitarlos estructuralmente.
> - **Archivos gigantes sin dividir**: un `components.css` de 3000 líneas es equivalente a no tener estructura.
> - **@import en producción sin bundler**: cada importación es una petición HTTP adicional, bloqueante para el render.

## Notas relacionadas

- [[index]] — la organización de archivos en contexto.
- [[02 Patrón 7-1]] — la extensión de la estructura modular para proyectos grandes con Sass.
- [[03 Capas (@layer)/02 Importar a Capas]] — cómo usar @import con layer para controlar la cascada.
- [[04 Metodologías de Nomenclatura/01 BEM]] — los nombres de componente que guían la estructura de archivos.
