---
title: Arquitectura y Metodologías CSS
aliases:
  - arquitectura css
  - metodologías css
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 9
---

# Arquitectura y Metodologías CSS

> [!definicion]
> La arquitectura CSS es el conjunto de decisiones sobre cómo **organizar, nombrar y ordenar** los estilos de un proyecto para que sean mantenibles, predecibles y escalables. Abarca desde el mecanismo interno del navegador (especificidad, cascada) hasta las convenciones de equipo (BEM, 7-1).

## Por qué importa la arquitectura

CSS es permisivo: cualquier cosa funciona en proyectos pequeños. El problema emerge cuando el proyecto crece: reglas que se pisan entre sí, `!important` para arreglar `!important`, refactors que rompen páginas no relacionadas. La arquitectura previene ese deterioro.

Los tres problemas recurrentes que la arquitectura resuelve son:
- **Colisiones de especificidad**: dos selectores apuntan al mismo elemento y gana el incorrecto.
- **Orden impredecible**: cambiar el orden de los archivos CSS rompe el diseño.
- **Nombres sin semántica**: `.azul`, `.izquierda`, clases que describen apariencia en vez de función.

## Las capas del problema

| Capa | Concepto | Herramienta |
|---|---|---|
| Motor del navegador | Cómo se resuelven los conflictos | Especificidad + Cascada |
| Control de origen | Qué estilos tienen prioridad | `@layer` |
| Nomenclatura | Cómo se llaman las clases | BEM / SMACSS / OOCSS / Utility-First |
| Organización | Cómo se distribuyen los archivos | Estructura modular / Patrón 7-1 |

## El orden de resolución del navegador

Cuando dos reglas compiten por el mismo elemento, el navegador aplica este orden (de mayor a menor prioridad):

1. **Origen + `!important`** — las declaraciones `!important` del autor ganan a todo excepto a `!important` del usuario.
2. **Capas (`@layer`)** — las capas declaradas después tienen prioridad sobre las anteriores (para estilos normales, al revés que `!important`).
3. **Especificidad** — el selector más específico gana.
4. **Orden de aparición** — a igual especificidad, la última declaración gana.

Entender este orden es la base de toda arquitectura CSS: permite predecir qué regla gana sin necesidad de `!important`.

## Metodologías de nomenclatura

Las metodologías de nomenclatura son convenciones para nombrar clases que evitan colisiones y comunican estructura:

- **BEM** (Block Element Modifier) — la más extendida; nombres descriptivos y sin ambigüedad.
- **SMACSS** — categoriza las reglas en cinco tipos (Base, Layout, Module, State, Theme).
- **OOCSS** — separa estructura y apariencia en clases distintas y componibles.
- **Utility-First** — clases de una sola propiedad (`flex`, `mt-4`); popularizado por Tailwind CSS.

No son excluyentes: muchos proyectos combinan BEM para componentes con alguna utilidad puntual.

## Organización de archivos

La organización de archivos determina qué se importa y en qué orden. El **Patrón 7-1** es el estándar de proyectos Sass medianos: siete carpetas temáticas y un archivo `main.scss` que las importa en orden controlado.

## Notas relacionadas

- [[01 Especificidad/index]] — cómo se calcula quién gana.
- [[02 Cascada/index]] — el mecanismo de resolución de conflictos.
- [[03 Capas (@layer)/index]] — capas para controlar el orden sin especificidad.
- [[04 Metodologías de Nomenclatura/index]] — BEM, SMACSS, OOCSS, Utility-First.
- [[05 Organización de Archivos/index]] — modular y patrón 7-1.
