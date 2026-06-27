---
title: Fases del Evento — Índice
aliases:
  - Event phases
  - fases de propagación
  - capture bubble target
tags:
  - javascript
  - api/concepto
  - eventos
draft: false
order: 4
---

# Fases del Evento

> [!definicion]
> Todo evento DOM recorre tres fases en orden estricto: **captura** (el evento desciende desde `window` hasta el `target`), **target** (el evento llega al elemento que lo originó) y **burbujeo** (el evento asciende desde el `target` de vuelta hasta `window`). Los listeners se ejecutan según la fase en la que están registrados.

```
window
  └── document
        └── <html>
              └── <body>
                    └── <main>
                          └── <ul>           ← listener de captura
                                └── <li>     ← target (clic aquí)
                          ↑
                    └── <main>               ← listener de burbujeo
```

## Las tres fases

| # | Fase | `e.eventPhase` | Dirección | Listeners que se ejecutan |
|---|---|---|---|---|
| 1 | Captura | 1 (`CAPTURING_PHASE`) | window → target | `capture: true` |
| 2 | Target | 2 (`AT_TARGET`) | — | `capture: true` y `capture: false` (en orden de registro) |
| 3 | Burbujeo | 3 (`BUBBLING_PHASE`) | target → window | `capture: false` (default) |

## Diagrama de propagación

```
CAPTURA (1)              TARGET (2)              BURBUJEO (3)
window                                           window
  ↓                                                ↑
document                                         document
  ↓                                                ↑
<html>                                           <html>
  ↓                                                ↑
<body>                                           <body>
  ↓                                                ↑
<div>          →→→→→→→   <button>   →→→→→→→     <div>
                          (click)
```

El recorrido completo se realiza aunque no haya ningún listener registrado en los nodos intermedios.

## Subsecciones

| # | Nota | Cubre |
|---|---|---|
| 01 | Captura | Descenso de window al target, uso de `capture: true`, casos de uso |
| 02 | Target | El evento en su origen, `eventPhase === 2`, orden de listeners |
| 03 | Burbujeo | Ascenso al document, qué eventos no burbujean, delegación |

## Notas relacionadas

- [[01 Captura|Captura]]
- [[02 Target|Target]]
- [[03 Burbujeo|Burbujeo]]
- [[05 Delegación de Eventos|Delegación de Eventos]]
- [[03 El Objeto Event/index|El Objeto Event]]
- [[02 Registro de Eventos/index|Registro de Eventos]]
