---
title: Patrones de Diseño — Índice
aliases:
  - design patterns javascript
  - patrones de diseño js
tags:
  - javascript
  - api/concepto
  - patrones
draft: false
order: 3
---

# Patrones de Diseño

> [!definicion]
> Los **patrones de diseño** son soluciones reutilizables a problemas recurrentes de estructuración de código. No son librerías ni fragmentos copiables: son plantillas conceptuales que se adaptan al contexto. En JavaScript del lado del cliente, los patrones más relevantes son los que gestionan la unicidad de instancias, la comunicación entre componentes desacoplados y la organización de módulos.

La clasificación clásica del libro GoF (Gang of Four) distingue tres familias: **creacionales** (cómo se crean los objetos), **estructurales** (cómo se componen) y **de comportamiento** (cómo se comunican). Los dos cubiertos aquí pertenecen a las familias creacional y de comportamiento respectivamente.

## Contenido de la sección

| # | Patrón | Familia | Qué resuelve |
|---|---|---|---|
| 01 | Singleton | Creacional | Garantiza una única instancia global; acceso centralizado a config, logger, store |
| 02 | Observer (Pub-Sub) | Comportamiento | Comunicación desacoplada entre emisores y suscriptores; base de cualquier sistema de eventos |

## Notas relacionadas

- [[01 Singleton|Singleton]]
- [[02 Observer (Pub-Sub)|Observer (Pub-Sub)]]
- [[../03 IIFE|IIFE]]
- [[../04 Memoización|Memoización]]
