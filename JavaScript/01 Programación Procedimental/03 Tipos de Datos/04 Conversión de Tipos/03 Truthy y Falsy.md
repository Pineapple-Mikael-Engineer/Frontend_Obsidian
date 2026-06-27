---
title: Truthy y Falsy en JavaScript
aliases:
  - truthy falsy js
  - valores falsy javascript
tags:
  - javascript
  - api/concepto
  - tipos
draft: false
order: 3
---

# Truthy y Falsy

> [!definicion]
> En JavaScript, todo valor tiene un equivalente booleano implícito. Los valores **falsy** son los que se convierten a `false` en un contexto booleano (condición de `if`, `while`, operador `!`, etc.). Todo lo demás es **truthy**. Hay exactamente 8 valores falsy; el resto del universo de valores es truthy.

```js
if (0)         { /* no entra */ }
if ("")        { /* no entra */ }
if (null)      { /* no entra */ }
if (undefined) { /* no entra */ }
if ([])        { /* SÍ entra — array vacío es truthy */ }
if ({})        { /* SÍ entra — objeto vacío es truthy */ }
if ("false")   { /* SÍ entra — string no vacío es truthy */ }
```

## Los 8 valores falsy

| Valor | Tipo | Nota |
|---|---|---|
| `false` | boolean | el falso literal |
| `0` | number | cero |
| `-0` | number | cero negativo |
| `0n` | bigint | cero bigint |
| `""` | string | string vacío |
| `null` | null | ausencia de objeto |
| `undefined` | undefined | no inicializado |
| `NaN` | number | Not a Number |

Todo valor que no aparezca en esta tabla es **truthy**, incluyendo `[]`, `{}`, `"0"`, `"false"`, `-1`, `Infinity`, y cualquier función o clase.

## Truthy sorprendentes

```js
Boolean([])        // → true  — array vacío es truthy
Boolean({})        // → true  — objeto vacío es truthy
Boolean("0")       // → true  — string no vacío es truthy
Boolean("false")   // → true  — idem
Boolean(-1)        // → true  — cualquier número no cero
Boolean(Infinity)  // → true
Boolean(new Boolean(false)) // → true — el OBJETO Boolean es truthy
```

## Usos prácticos

### Guard con &&

Ejecuta el lado derecho solo si el lado izquierdo es truthy:

```js
// Renderizar solo si hay usuario
usuario && mostrarPerfil(usuario);

// Acceder a propiedad solo si el objeto existe
const nombre = usuario && usuario.nombre;
```

### Valor por defecto con ||

Devuelve el primer valor truthy, o el último si ninguno lo es:

```js
const nombre = usuario.nombre || "Anónimo";
const puerto = config.puerto || 3000;
```

> [!warning] Trampa de || con valores válidos
> `||` trata `0`, `""` y `false` como falsy. Si esos son valores válidos, el fallback se activará incorrectamente.
> ```js
> const cantidad = pedido.cantidad || 1;
> // Si pedido.cantidad === 0 (legítimo), devuelve 1 — BUG
> ```
> Usa `??` (nullish coalescing) para fallbacks que solo actúen con `null`/`undefined`:
> ```js
> const cantidad = pedido.cantidad ?? 1; // 0 se respeta
> ```

### Doble NOT para conversión explícita

```js
!!0          // → false
!!""         // → false
!![]         // → true
!!"hola"     // → true
!!null       // → false
```

## La trampa del objeto Boolean wrapper

`new Boolean(false)` crea un **objeto**, y los objetos son siempre truthy, aunque envuelvan `false`:

```js
const b = new Boolean(false);
if (b) {
  console.log("Entra — b es un objeto truthy");  // se ejecuta
}
Boolean(b); // → true
```

Nunca se debe usar `new Boolean()`. Para conversión explícita, usar `Boolean(valor)` o `!!valor`.

## Cómo funciona por dentro

El algoritmo **ToBoolean** del spec ECMAScript es una tabla de consulta: comprueba si el valor pertenece a la lista de falsy y devuelve `false`; para cualquier otro valor devuelve `true`. No hay coerción encadenada como en `ToNumber` o `ToString` — es una consulta directa. Esta simplicidad lo hace predecible una vez que se memorizan los 8 falsy.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `||` para valores por defecto cuando `0`, `""` y `false` no son valores válidos en tu contexto.
> - Usa `??` cuando `0`, `""` o `false` son valores legítimos que no deben caer en el fallback.
> - Usa `!!` para convertir explícitamente a boolean cuando necesitas un boolean real.
> - Sé explícito en las condiciones cuando el tipo no está claro: `if (lista.length > 0)` es más legible que `if (lista.length)`.

## Errores comunes

> [!warning] Trampas
> - **Confundir array vacío con falsy**: `[]` es truthy. Para saber si un array está vacío: `array.length === 0`.
> - **Usar || cuando el valor 0 es válido**: `cantidad || 1` convierte `0` a `1`. Usa `?? 1`.
> - **`new Boolean(false)` como falsy**: es truthy porque es un objeto. Nunca uses `new Boolean()`.
> - **`"false"` como falsy**: cualquier string no vacío es truthy, incluido `"false"`, `"0"`, `"null"`.

## Notas relacionadas

- [[04 Lógicos]] — `&&`, `||`, `!` usan la evaluación truthy/falsy.
- [[07 Nullish Coalescing (??)]] — alternativa a `||` que solo actúa con null/undefined.
- [[02 Conversión Implícita]] — cómo el motor aplica ToBoolean automáticamente.
- [[04 Igualdad (== vs ===)]] — cómo falsy interactúa con `==`.
