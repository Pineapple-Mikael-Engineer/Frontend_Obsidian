---
title: Preferencias — prefers-color-scheme, prefers-reduced-motion
aliases:
  - prefers-color-scheme
  - prefers-reduced-motion
  - dark mode
tags:
  - css
  - api/concepto
  - responsive
draft: false
---

# Preferencias (prefers-color-scheme, prefers-reduced-motion)

> [!definicion]
> Las features de **preferencia del usuario** detectan ajustes que la persona configuró en su sistema: si prefiere **modo oscuro** (`prefers-color-scheme`), **menos animaciones** (`prefers-reduced-motion`), más contraste, etc. Permiten respetar las preferencias del usuario sin que tenga que configurar nada en el sitio.

```css
@media (prefers-color-scheme: dark) {
  :root { --fondo: #1e1e2e; --texto: #cdd6f4; }
}
@media (prefers-reduced-motion: reduce) {
  * { animation: none; transition: none; }
}
```

## prefers-color-scheme: modo oscuro

> [!tip] Modo oscuro que respeta el sistema
> `prefers-color-scheme: dark` se activa si el usuario tiene el **modo oscuro** activado en su sistema operativo. La forma limpia de implementarlo es definir los colores como [[10 Variables CSS/index | variables]] y cambiarlos en el media query:
> ```css
> :root {
>   --fondo: #ffffff;
>   --texto: #1e1e2e;
> }
> @media (prefers-color-scheme: dark) {
>   :root {
>     --fondo: #1e1e2e;
>     --texto: #cdd6f4;
>   }
> }
> body { background: var(--fondo); color: var(--texto); }
> ```
> Con solo redefinir las variables, todo el sitio cambia de tema. Detalle en [[06 Temas Dinámicos]].

## color-scheme: el complemento

> [!tip] Declara color-scheme para los controles nativos
> Además del media query, conviene declarar la propiedad `color-scheme` para que los **controles nativos** (scrollbars, inputs, selects) adopten el tema correcto:
> ```css
> :root { color-scheme: light dark; }   /* el navegador adapta los controles nativos */
> ```
> Sin ella, los scrollbars y campos pueden quedar claros sobre un fondo oscuro.

## prefers-reduced-motion: respetar la accesibilidad

> [!warning] Reducir el movimiento es accesibilidad, no opcional
> Algunas personas sufren mareo o malestar con las **animaciones** (trastornos vestibulares). Quienes activan "reducir movimiento" en su sistema esperan menos animación. `prefers-reduced-motion: reduce` debe respetarse:
> ```css
> @media (prefers-reduced-motion: reduce) {
>   *, *::before, *::after {
>     animation-duration: 0.01ms !important;
>     animation-iteration-count: 1 !important;
>     transition-duration: 0.01ms !important;
>     scroll-behavior: auto !important;
>   }
> }
> ```
> Es un requisito de accesibilidad (WCAG 2.3.3), no un extra. Las animaciones esenciales pueden mantenerse suaves, pero los efectos decorativos llamativos deben desactivarse.

## Otras preferencias

| Feature | Detecta |
|---------|---------|
| `prefers-contrast` | Preferencia de más/menos contraste |
| `prefers-reduced-transparency` | Reducir transparencias |
| `prefers-reduced-data` | Modo de ahorro de datos |
| `forced-colors` | Modo de alto contraste de Windows |

## Buenas prácticas

> [!tip] Recomendaciones
> - Implementa modo oscuro con variables + `prefers-color-scheme`, y declara `color-scheme`.
> - **Respeta** `prefers-reduced-motion`: desactiva animaciones decorativas para quien lo pide.
> - Ofrece, si quieres, un interruptor manual de tema que sobrescriba la preferencia del sistema.
> - Considera las demás preferencias (contraste, datos) para una experiencia inclusiva.

## Errores comunes

> [!warning] Trampas
> - **Ignorar `prefers-reduced-motion`**: animaciones que marean a usuarios sensibles.
> - **Modo oscuro sin `color-scheme`**: scrollbars/inputs claros sobre fondo oscuro.
> - **Colores hardcodeados** en vez de variables: el modo oscuro se vuelve inmanejable.

## Notas relacionadas

- [[06 Temas Dinámicos]] — modo oscuro con variables.
- [[03 Animar transform y opacity]] — animaciones que `reduced-motion` desactiva.
- [[10 Variables CSS/index]] — la base de los temas.
