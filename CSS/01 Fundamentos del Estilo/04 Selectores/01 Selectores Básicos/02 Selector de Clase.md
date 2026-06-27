---
title: Selector de Clase — Apuntar por atributo class
aliases:
  - class selector
tags:
  - css
  - api/selector
  - arquitectura
selector: clase
draft: false
order: 2
---

# Selector de Clase

> [!definicion]
> El selector de **clase** apunta a los elementos que tienen una `class` concreta, con la sintaxis `.nombre`. Es el selector **más usado** de CSS: reutilizable, de especificidad media y manejable, y desacoplado del tipo de elemento.

```css
.aviso { color: crimson; }
```
```html
<p class="aviso">Texto</p>
<span class="aviso">Otro</span>   <!-- la misma clase, varios elementos -->
```

## Por qué es el preferido

| Ventaja | Detalle |
|---------|---------|
| Reutilizable | Muchos elementos comparten una clase |
| Especificidad media | Gana al tipo, pero es fácil de gestionar (no tan alta como un id) |
| Desacoplada del HTML | `.boton` funciona en `<a>`, `<button>` o `<div>` |
| Combinable | Un elemento puede tener varias clases |

## Varias clases por elemento

Un elemento puede llevar **varias** clases (separadas por espacios en el HTML), combinando estilos:

```html
<button class="btn btn-primario btn-grande">Comprar</button>
```
```css
.btn          { padding: 0.5rem 1rem; border: none; }
.btn-primario { background: #cba6f7; }
.btn-grande   { font-size: 1.2rem; }
```

Esta composición es la base de las metodologías modernas: clases pequeñas y combinables.

## Encadenar clases (sin espacio)

Escribir dos clases pegadas exige que el elemento tenga **ambas**:

```css
.btn.activo { outline: 2px solid; }
/* solo elementos con class que incluya btn Y activo */
```

Distinto de `.btn .activo` (con espacio), que es un descendiente — ver [[02 Combinadores/index | combinadores]].

## Nombrar clases

> [!tip] Nombra por función, no por aspecto
> Una clase debería describir **qué es** o **qué hace** el elemento, no cómo se ve:
> - ✅ `.alerta`, `.boton-primario`, `.tarjeta` (función).
> - ❌ `.rojo`, `.texto-grande`, `.margen-20` (aspecto, salvo en utility-first).
>
> Nombrar por función permite cambiar el estilo (de rojo a naranja) sin renombrar la clase en todo el HTML. Metodologías como [[01 BEM | BEM]] formalizan esta idea.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa clases como gancho principal de estilado.
> - Compón estilos con varias clases pequeñas y reutilizables.
> - Nombra por función (salvo en enfoques utility-first, que usan clases de aspecto).
> - Mantén la especificidad baja: evita encadenar muchas clases en cascada.

## Errores comunes

> [!warning] Trampas
> - **Nombres por aspecto** (`.rojo`) que mienten al cambiar el estilo.
> - **Confundir `.a.b` (mismo elemento) con `.a .b` (descendiente)**.
> - **Una clase gigante** por elemento en vez de varias componibles.

## Notas relacionadas

- [[01 Selector de Tipo]] — para estilos base, no componentes.
- [[03 Selector de ID]] — único y de alta especificidad (usar con cautela).
- [[01 BEM]] — una metodología de nomenclatura de clases.
