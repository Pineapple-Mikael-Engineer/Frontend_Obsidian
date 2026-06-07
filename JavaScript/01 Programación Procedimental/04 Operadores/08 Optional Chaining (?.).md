---
title: Optional Chaining (?.) — acceso seguro a propiedades y métodos
aliases:
  - optional chaining
  - operador ?.
  - encadenamiento opcional
tags:
  - javascript
  - api/operador
  - operadores
objeto: global
tipo: operador
retorna: el valor accedido, o undefined si el receptor es nullish
muta: false
asincrono: false
draft: false
---

# Optional Chaining `?.`

> [!definicion] Optional Chaining
> `?.` cortocircuita a `undefined` si el operando izquierdo es `null` o `undefined`, en lugar de lanzar `TypeError`. Funciona en tres formas: acceso a propiedad (`obj?.prop`), acceso por corchete (`obj?.[expr]`) y llamada a función/método (`func?.()`).

```js
const user = null;

// Sin ?. → TypeError
user.name;  // TypeError: Cannot read properties of null

// Con ?. → undefined (sin excepción)
user?.name;  // → undefined
```

## Las tres formas

```js
const obj = {
  name: "Ana",
  address: null,
  greet() { return "Hola"; }
};

// 1. Acceso a propiedad con punto
obj?.name           // → "Ana"
obj?.address?.city  // → undefined  (address es null, cortocircuita)
obj?.age            // → undefined  (age no existe)

// 2. Acceso por corchete (nombre dinámico o índice)
const key = "name";
obj?.[key]          // → "Ana"
const arr = [1, 2, 3];
arr?.[0]            // → 1
arr?.[10]           // → undefined  (índice fuera de rango)

// 3. Llamada a función o método (puede no existir)
obj.greet?.()       // → "Hola"
obj.nonexistent?.() // → undefined  (método no existe)
const fn = null;
fn?.()              // → undefined  (fn es null)
```

## Encadenamiento: cortocircuito en cadena

Cuando `?.` cortocircuita, todo el resto de la cadena de accesos a la derecha se omite. Un solo `null`/`undefined` en cualquier punto devuelve `undefined` para toda la expresión.

```js
const data = {
  user: {
    profile: null
  }
};

data?.user?.profile?.settings?.theme
// Pasos:
// data?.user        → { profile: null }  (existe)
// ?.profile         → null               (existe, pero es null)
// ?.settings        → undefined          (cortocircuita aquí)
// ?.theme           → undefined          (nunca se evalúa)

// Todos estos devuelven undefined sin lanzar error:
null?.a?.b?.c
undefined?.x?.[0]?.()
```

## Combinación con `??` — el patrón canónico

`?.` devuelve `undefined` cuando falla. `??` convierte ese `undefined` en un valor por defecto. Juntos forman el patrón estándar de acceso seguro con fallback.

```js
const config = {
  server: null
};

const host   = config?.server?.host ?? "localhost";
const port   = config?.server?.port ?? 3000;
const secure = config?.server?.tls?.enabled ?? false;

// → "localhost", 3000, false
// Sin ?? ni ?.: habría que escribir 3 niveles de comprobación manual
```

## Con métodos opcionales

`?.()` solo llama la función si el receptor no es nullish. Útil para callbacks opcionales, plugins y APIs con comportamiento configurable.

```js
// Callback opcional en una función
function process(data, options = {}) {
  const result = transform(data);
  options.onComplete?.(result);   // solo si onComplete existe
  options.onError?.(null);        // solo si onError existe
  return result;
}

// Método del DOM que puede no existir en todos los navegadores
element.animate?.([{ opacity: 0 }, { opacity: 1 }], 300);

// Plugin hook opcional
plugin.beforeSave?.();
save(data);
plugin.afterSave?.();
```

## Con arrays y Map

```js
// Acceso a array posiblemente null
const first = arr?.[0];
const last  = arr?.[arr.length - 1];

// Método de Map/Set que puede no estar inicializado
cache?.get(key)
cache?.set(key, value)

// Obtener propiedad de elemento
const items = response?.data?.items;
const count = items?.length ?? 0;
```

## Cuándo NO usar `?.`

`?.` está diseñado para **valores opcionales por diseño**. Si una propiedad debe existir siempre, usar `?.` oculta bugs: el acceso fallido devuelve `undefined` silenciosamente en lugar de lanzar un error que señalaría el problema.

```js
// Si user SIEMPRE debe tener id, esto oculta un bug:
const id = user?.id;  // undefined si user es null/undefined, sin advertencia

// Mejor así (lanza error si user no existe):
const id = user.id;  // TypeError visible si user es null/undefined

// Regla: ?.  para "puede no existir por diseño"
//        .   para "debe existir; si no existe, es un bug"

// Ejemplo: propiedad de respuesta de API (puede ser null en algunos endpoints)
const avatar = response?.user?.avatar?.url ?? "/default-avatar.png";

// Ejemplo: propiedad de usuario autenticado (debe existir si llegó a este punto)
const userId = currentUser.id;  // sin ?. (si es null aquí, hay un bug previo)
```

## Cómo funciona por dentro

El motor evalúa el operando izquierdo. Si el resultado es `null` o `undefined` (comprobación de identidad, sin coerción), devuelve `undefined` sin evaluar el resto de la expresión. En caso contrario, evalúa el acceso a propiedad, corchete o llamada normalmente.

En Babel y herramientas de transpilación pre-ES2020, `?.` se compila a comprobaciones ternarias:

```js
// a?.b?.c compila a:
const _a = a;
const _ab = _a === null || _a === void 0 ? void 0 : _a.b;
const result = _ab === null || _ab === void 0 ? void 0 : _ab.c;
```

Desde V8 8.4 (Chrome 85), `?.` está implementado de forma nativa y optimizado.

> [!tip] Buenas prácticas
> - Combinar siempre `?.` con `??` para evitar que `undefined` se propague silenciosamente como valor de retorno.
> - Preferir `?.` sobre el patrón `obj && obj.prop && obj.prop.sub` (menos preciso: `&&` trata `0` y `""` como falsos).
> - No usar `?.` en propiedades que deben existir: mejor que el error sea visible.
> - Para TypeScript, `?.` también actúa como type guard parcial, pero no elimina la necesidad de validación completa en los bordes del sistema.

> [!warning] Errores comunes
> **`?.` no protege contra propiedades inexistentes que no son nullish:** `obj?.nonexistent` devuelve `undefined` (no lanza error), pero si se pasa `undefined` a una función que lo rechaza, el error ocurre después. El punto de fallo queda alejado del origen.
>
> **Usar `?.` en el lado izquierdo de una asignación:** `obj?.prop = value` es un SyntaxError. `?.` solo aplica a lecturas, no a escrituras.
>
> **`func?.()` vs `func?.()`:** si `func` existe pero no es una función, `func?.()` lanza TypeError igualmente (`func is not a function`). Solo protege contra `null`/`undefined`, no contra el tipo incorrecto.

## Notas relacionadas

- [[07 Nullish Coalescing (??)]] — `??` convierte el `undefined` producido por `?.` en un valor por defecto
- [[04 Lógicos]] — `&&` como alternativa antigua a `?.`; diferencias de coerción
- [[09 Operador in]] — comprobar si una propiedad existe antes de acceder (alternativa explícita)
