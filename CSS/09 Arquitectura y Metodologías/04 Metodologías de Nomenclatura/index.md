---
title: Metodologías de Nomenclatura CSS
aliases:
  - metodologías css
  - nomenclatura css
tags:
  - css
  - api/concepto
  - arquitectura
draft: false
order: 4
---

# Metodologías de Nomenclatura CSS

> [!definicion]
> Las **metodologías de nomenclatura** son convenciones para nombrar clases CSS que previenen colisiones, comunican estructura y hacen el código mantenible en proyectos grandes. No son estándares de la especificación CSS: son acuerdos de equipo que se aplican de forma consistente.

## Por qué se necesitan convenciones

Sin convenciones, los nombres de clase tienden a:
- Ser demasiado genéricos (`.box`, `.item`, `.red`) y colisionar entre componentes.
- Mezclar semántica de estructura, estado y apariencia sin distinción.
- No comunicar relaciones padre-hijo entre clases.
- Crecer en especificidad conforme los equipos necesitan sobreescribir estilos.

Las metodologías resuelven esto de formas distintas:

| Metodología | Enfoque | Especificidad típica |
|---|---|---|
| BEM | Nombres descriptivos con convención `bloque__elemento--modificador` | Una clase (0,1,0) |
| SMACSS | Categorizar reglas en 5 tipos | Variable por categoría |
| OOCSS | Separar estructura de apariencia | Una clase por responsabilidad |
| Utility-First | Una clase por propiedad CSS | Una clase (0,1,0) |

## Comparativa rápida

**BEM** es la más extendida en componentes: cada bloque es autocontenido, los elementos del bloque llevan el prefijo del bloque, y los modificadores describen variantes. El HTML resultante tiene clases largas pero claras.

**SMACSS** organiza el CSS en categorías, no los nombres: Base (elementos sin clase), Layout (contenedores mayores), Module (componentes reutilizables), State (estados `is-`, `has-`) y Theme (variantes de color/apariencia). Es más una guía de organización que de nomenclatura estricta.

**OOCSS** introduce el concepto de "objeto visual": separar la estructura (tamaño, posición) de la apariencia (color, fuente) en clases distintas que se componen. Más difícil de mantener en equipos sin experiencia.

**Utility-First** (Tailwind CSS) lleva OOCSS al extremo: cada clase hace exactamente una cosa. El HTML tiene muchas clases, el CSS tiene muy pocas. Opinionado pero muy consistente en proyectos con diseño sistemático.

## No son excluyentes

En la práctica, los proyectos combinan metodologías:
- BEM para los componentes propios.
- Algunas utilidades puntuales (`.flex`, `.hidden`) inspiradas en Utility-First.
- SMACSS para organizar los archivos (qué va en base, qué en componentes).

## Notas relacionadas

- [[01 BEM]] — Block Element Modifier, la metodología más extendida.
- [[02 SMACSS]] — organización por categorías de regla.
- [[03 OOCSS]] — separación de estructura y apariencia.
- [[04 Utility-First]] — una clase, una propiedad; la filosofía de Tailwind.
