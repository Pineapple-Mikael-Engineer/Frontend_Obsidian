---
title: Estructuras de Datos — JavaScript
aliases:
  - JS Estructuras de Datos
  - colecciones javascript
tags:
  - javascript
  - estructuras-datos
draft: false
order: 4
---

# Estructuras de Datos

> [!definicion]
> JavaScript ofrece cinco familias de estructuras de datos: el **Array** clásico de valores ordenados por índice; **Map** para pares clave-valor con claves de cualquier tipo; **Set** para colecciones de valores únicos; **WeakMap y WeakSet** para metadatos asociados a objetos sin impedir su recolección por el GC; y los **Typed Arrays** para manipulación de memoria binaria de bajo nivel. Elegir la estructura correcta impacta directamente en la corrección, rendimiento y gestión de memoria del código.

## Cuándo usar cada estructura

| Estructura | Cuándo usarla | Complejidad clave |
|------------|---------------|-------------------|
| `Array` | Lista ordenada; acceso por índice numérico; mutación con push/pop/splice | get O(1), push/pop O(1), insert/delete medio O(n) |
| `Map` | Clave-valor con claves de cualquier tipo; iteración ordenada; size en O(1) | get/set/has/delete O(1) |
| `Set` | Valores únicos; deduplicación; pertenencia en O(1) | has/add/delete O(1) |
| `WeakMap` | Metadatos privados ligados al ciclo de vida de un objeto; caché que no previene GC | get/set/has/delete O(1) |
| `WeakSet` | Marcar objetos como "vistos" o "procesados" sin impedir que sean recolectados | has/add/delete O(1) |
| Typed Arrays | Datos binarios de bajo nivel: WebGL, audio, protocolos de red, Canvas píxeles | acceso O(1), copia masiva via `set` |

## Comparativa rápida Map vs Object

| Aspecto | Map | Object literal |
|---------|-----|----------------|
| Tipo de clave | cualquier valor | solo string o Symbol |
| Tiene size nativo | sí (`map.size`) | no (requiere `Object.keys().length`) |
| Herencia prototipo | no | sí (`__proto__`, `toString`…) |
| Orden de iteración | inserción | inserción (strings no-enteros) + enteros primero |
| Mejor para | maps dinámicos, frecuentes inserciones/eliminaciones | registros de estructura fija |

## Comparativa rápida Set vs Array para pertenencia

| Operación | Set | Array |
|-----------|-----|-------|
| `has(v)` / `includes(v)` | O(1) | O(n) |
| Garantía de unicidad | automática | manual |
| Acceso por índice | no | sí |
| Operaciones de conjuntos | nativas (ES2025) | manual con filter/spread |

## Árbol de decisión

```
¿Necesito acceso por índice numérico?
├─ Sí → Array
└─ No
   ├─ ¿Pares clave-valor?
   │   ├─ ¿La clave puede ser no-string? → Map
   │   ├─ ¿El objeto puede morir y quiero limpiar automáticamente? → WeakMap
   │   └─ ¿Estructura fija de campos? → Object literal
   ├─ ¿Solo valores únicos?
   │   ├─ ¿Objetos pueden morir? → WeakSet
   │   └─ Si no → Set
   └─ ¿Memoria binaria / datos numéricos de bajo nivel? → Typed Arrays
```

## Subsecciones

- [[01 Arrays/index|01 Arrays]]
- [[02 Map/index|02 Map]]
- [[03 Set/index|03 Set]]
- [[04 WeakMap y WeakSet/index|04 WeakMap y WeakSet]]
- [[05 Typed Arrays/index|05 Typed Arrays]]
