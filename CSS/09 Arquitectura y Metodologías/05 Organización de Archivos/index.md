---
title: Organización de Archivos CSS
aliases:
  - organización css
  - estructura de archivos css
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
---

# Organización de Archivos CSS

> [!definicion]
> La **organización de archivos CSS** define cómo se distribuyen los estilos entre archivos y carpetas, y en qué orden se importan. Una buena organización hace que sea obvio dónde vive cada regla, previene importaciones circulares y hace que el orden de cascada sea predecible sin necesidad de revisar el código.

## Por qué importa el orden de importación

Los archivos CSS se procesan en el orden en que se importan. Esto afecta a la cascada: cuando dos declaraciones tienen la misma especificidad, la que viene después en el orden de importación gana. Una organización predecible evita sorpresas y hace que los resets, los tokens y las utilidades nunca entren en conflicto con los componentes.

El esquema habitual: lo más genérico primero, lo más específico después.

```
Resets / Normalize
  ↓
Design Tokens (variables)
  ↓
Estilos base (tipografía, elementos HTML)
  ↓
Layout (grids, contenedores)
  ↓
Componentes
  ↓
Utilidades (overrides finales)
```

## Los dos enfoques principales

| Enfoque | Cuándo usar |
|---|---|
| Estructura modular | Proyectos pequeños y medianos sin preprocesador |
| Patrón 7-1 | Proyectos medianos y grandes con Sass/PostCSS |

Con `@layer`, el orden de importación pierde importancia para la prioridad (ya que las capas se declaran explícitamente), pero la organización sigue siendo relevante para la legibilidad y el mantenimiento.

## Notas relacionadas

- [[01 Estructura Modular]] — organización por tipo de regla en archivos separados.
- [[02 Patrón 7-1]] — siete carpetas, un archivo de entrada; el estándar de Sass.
- [[03 Capas (@layer)/index]] — cómo @layer reemplaza la dependencia del orden de archivos.
- [[04 Metodologías de Nomenclatura/02 SMACSS]] — las categorías SMACSS mapean directamente a carpetas.
