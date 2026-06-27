---
title: Comentarios — Línea, bloque y JSDoc
aliases:
  - comentarios JS
  - JSDoc
  - comentarios de línea
  - comentarios de bloque
tags:
  - javascript
  - api/concepto
  - sintaxis
objeto: global
tipo: concepto
draft: false
order: 3
---

# Comentarios

> [!definicion]
> Los comentarios son texto ignorado por el motor JS durante la ejecución. Existen tres formas: `//` (línea), `/* */` (bloque) y `/** */` (JSDoc). Su propósito es documentar el **porqué** no obvio del código — invariantes ocultas, workarounds, decisiones de diseño — no el *qué*, que el código ya expresa por sí mismo.

```js
// Comentario de línea: desde // hasta el final de la línea

/* Comentario de bloque:
   puede ocupar varias líneas */

/**
 * Comentario JSDoc: documenta funciones, clases y módulos.
 * @param {number} radio - El radio del círculo.
 * @returns {number} El área calculada.
 */
function areaCirculo(radio) {
  return Math.PI * radio ** 2;
}
```

## Comentario de línea `//`

El contenido desde `//` hasta el fin de la línea queda excluido del parsing. Puede aparecer al final de una línea de código o en su propia línea.

```js
const TAX_RATE = 0.19; // IVA Colombia 2024

// Workaround: la API retorna null en lugar de [] para listas vacías
const items = response.data ?? [];
```

## Comentario de bloque `/* */`

Abarca desde `/*` hasta el siguiente `*/`. No es anidable: el primer `*/` que encuentre cierra el bloque.

```js
/* Este bloque de código se desactivó el 2024-03
   al migrar a la nueva API de pagos */
// doOldPayment(amount, currency);

/*
 * Convención opcional de formato:
 * usar * al inicio de cada línea para legibilidad.
 */
```

> [!warning]
> Los bloques `/* */` no son anidables. Intentar comentar código que ya contiene `/* */` cierra el bloque en el primer `*/` encontrado, dejando el resto fuera del comentario como código activo (y posiblemente inválido).
> ```js
> /* comentario externo
>   /* comentario interno */   ← cierra AQUÍ el externo
>   resto del código */        ← esto es código, no comentario → SyntaxError
> ```

## JSDoc `/** */`

El formato `/** */` es el estándar de documentación para APIs públicas en JavaScript. Herramientas como TypeScript (inferencia de tipos), VSCode (IntelliSense) y generadores como TypeDoc lo consumen directamente.

```js
/**
 * Calcula el precio con impuesto incluido.
 *
 * @param {number} base - Precio base sin impuesto.
 * @param {number} [tasa=0.19] - Tasa de impuesto (opcional, default 0.19).
 * @returns {number} Precio total con impuesto.
 * @throws {RangeError} Si `base` es negativo.
 *
 * @example
 * precioConIVA(100);       // → 119
 * precioConIVA(100, 0.05); // → 105
 */
function precioConIVA(base, tasa = 0.19) {
  if (base < 0) throw new RangeError("base no puede ser negativa");
  return base * (1 + tasa);
}
```

### Etiquetas JSDoc más usadas

| Etiqueta | Propósito | Ejemplo |
|---|---|---|
| `@param` | Documenta un parámetro | `@param {string} nombre` |
| `@returns` | Documenta el valor de retorno | `@returns {boolean}` |
| `@throws` | Documenta errores que puede lanzar | `@throws {TypeError}` |
| `@type` | Anota el tipo de una variable | `@type {number[]}` |
| `@typedef` | Define un tipo personalizado | `@typedef {Object} Config` |
| `@property` | Documenta propiedad de un typedef | `@property {string} host` |
| `@deprecated` | Marca como obsoleto | `@deprecated Usar `nuevaFn` en su lugar` |
| `@example` | Añade un ejemplo ejecutable | ver arriba |
| `@see` | Referencia a otro símbolo o URL | `@see {otraFuncion}` |
| `@since` | Versión desde la que existe | `@since 2.0.0` |

### Anotar tipos complejos con JSDoc

```js
/**
 * @typedef {Object} Usuario
 * @property {number} id
 * @property {string} nombre
 * @property {string[]} roles
 */

/**
 * @param {Usuario} usuario
 * @returns {boolean}
 */
function esAdmin(usuario) {
  return usuario.roles.includes("admin");
}

/** @type {Map<string, number>} */
const cache = new Map();

/** @type {(x: number) => number} */
const cuadrado = x => x * x;
```

> [!info]
> TypeScript puede leer anotaciones JSDoc en archivos `.js` puro cuando se activa `"checkJs": true` en `tsconfig.json`. Esto permite obtener verificación de tipos sin migrar a `.ts`.

## Cuándo comentar (y cuándo no)

### Comentar el PORQUÉ, no el QUÉ

```js
// ❌ Innecesario — el código ya lo dice
i++; // incrementa i en 1

// ✓ Útil — explica la razón no obvia
// El índice empieza en 1, no en 0, porque el primer elemento es el header
for (let i = 1; i < rows.length; i++) { }

// ✓ Workaround para un bug conocido del proveedor
// Safari < 16 no soporta structuredClone; usar JSON.parse/stringify como fallback
const copia = typeof structuredClone === "function"
  ? structuredClone(obj)
  : JSON.parse(JSON.stringify(obj));

// ✓ Invariante no obvia
// Esta función DEBE llamarse antes de que el DOM esté listo (se invoca desde el <head>)
function preInit() { }
```

### Comentarios de desactivación temporal

Herramientas de linting como ESLint entienden comentarios especiales para suprimir advertencias puntuales.

```js
// eslint-disable-next-line no-console
console.log("Solo para depuración en staging");

/* eslint-disable @typescript-eslint/no-explicit-any */
function legacyAdapter(data: any) { return data; }
/* eslint-enable @typescript-eslint/no-explicit-any */

// @ts-ignore  ← TypeScript ignora la línea siguiente
const resultado = funcionMalTipada();

// @ts-expect-error  ← mejor: falla si NO hay error (autocontrolado)
const resultado2 = funcionMalTipada();
```

> [!tip]
> Preferir `@ts-expect-error` sobre `@ts-ignore`: si el error que se esperaba suprimir desaparece (porque se corrigió el tipo), `@ts-expect-error` genera una advertencia recordando que la supresión ya no es necesaria.

### El "comentario muerto" — código comentado

```js
// ❌ Antipatrón: código comentado en el fuente
// function viejaImplementacion(x) {
//   return x * 1.5;
// }

// ✓ Correcto: eliminar y usar git para recuperar si es necesario
// git log --all -S "viejaImplementacion" --source -- ruta/archivo.js
```

El código comentado acumula deuda cognitiva: el lector no sabe si es relevante, si estaba roto, o cuándo se desactivó. Git conserva todo el historial; el código muerto no aporta.

> [!tip] Buenas prácticas
> - Documentar con JSDoc toda función pública o exportada de un módulo.
> - Comentar cualquier decisión no obvia: workarounds, algoritmos no estándar, contratos implícitos.
> - Mantener los comentarios sincronizados con el código — un comentario desactualizado es peor que ninguno.
> - Usar `// TODO:` y `// FIXME:` con nombre y fecha para rastrear deuda técnica: `// TODO(Ana, 2024-03): migrar a fetch API cuando se deprece XHR`.

> [!warning] Errores comunes
> - Intentar anidar `/* */`: cierra en el primer `*/`, deja código expuesto.
> - Comentar el *qué* en lugar del *porqué*: añade ruido sin aportar información.
> - Dejar código comentado sin fecha ni contexto: nadie sabrá si puede eliminarse.
> - Usar `@ts-ignore` en lugar de `@ts-expect-error`: la supresión no se autocontrola.
> - Olvidar actualizar JSDoc cuando cambia la firma de una función: los IDEs y TypeScript mostrarán tipos incorrectos.

## Notas relacionadas

- [[01 Declaraciones y Expresiones]]
- [[04 Modo Estricto (use strict)]]
- [[11 Manejo de Errores y Depuración/04 Depuración/01 Métodos de console | Métodos de console]]
- [[11 Manejo de Errores y Depuración/01 Tipos de Errores/index | Tipos de Errores]]
