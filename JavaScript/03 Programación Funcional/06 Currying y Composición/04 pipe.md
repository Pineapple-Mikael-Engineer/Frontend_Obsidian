---
title: pipe — Composición de funciones (izquierda a derecha)
aliases:
  - pipe
  - pipeline de funciones
  - function pipeline
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
order: 4
---

# pipe

> [!definicion]
> `pipe(f, g, h)(x)` aplica las funciones de **izquierda a derecha**: primero `f(x)`, luego `g` sobre ese resultado, y finalmente `h`. El resultado es `h(g(f(x)))`. El orden de lectura refleja el orden de ejecución, lo que lo hace más intuitivo que [[03 compose]] para la mayoría de los contextos en JavaScript.

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const trim      = s => s.trim();
const minusc    = s => s.toLowerCase();
const slug      = s => s.replace(/\s+/g, "-");

const toSlug = pipe(trim, minusc, slug);
// Lee: primero trim, luego minúsculas, luego slug

toSlug("  Hola Mundo  ");  // "hola-mundo"
```

## Implementación

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);
```

`reduce` itera el array de funciones de izquierda a derecha, acumulando el resultado de aplicar cada función al valor anterior. El valor inicial del acumulador es `x`.

## Diferencia con compose

`pipe` y [[03 compose]] hacen lo mismo pero en **orden inverso**. `pipe(f, g)` ≡ `compose(g, f)`.

```js
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);
const pipe    = (...fns) => x => fns.reduce((v, f) => f(v), x);

const doble  = x => x * 2;
const sumar1 = x => x + 1;

compose(doble, sumar1)(3);  // sumar1(3)=4 → doble(4)=8  — derecha a izquierda
pipe(sumar1, doble)(3);     // sumar1(3)=4 → doble(4)=8  — izquierda a derecha, mismo resultado
```

La preferencia por `pipe` en JavaScript se debe a que describe los pasos en el orden cronológico en que los humanos los narran: "primero hago A, luego B, luego C".

## Recetas

### Procesamiento de datos de API

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const normalizar = datos => datos.map(d => ({
  id: d.user_id,
  nombre: d.full_name.trim(),
  activo: d.status === "active"
}));

const filtrarActivos = datos => datos.filter(d => d.activo);

const ordenarPorNombre = datos => datos.toSorted((a, b) =>
  a.nombre.localeCompare(b.nombre)
);

const tomarPrimeros = n => datos => datos.slice(0, n);

const procesarUsuarios = pipe(
  normalizar,
  filtrarActivos,
  ordenarPorNombre,
  tomarPrimeros(10)
);

// fetch("...").then(procesarUsuarios)
```

### Middleware pattern

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const agregarTimestamp = req => ({ ...req, timestamp: Date.now() });
const validarToken = req => {
  if (!req.headers.auth) throw new Error("Sin autorización");
  return req;
};
const parsearBody = req => ({ ...req, body: JSON.parse(req.rawBody) });
const enriquecerContexto = req => ({ ...req, usuario: buscarUsuario(req.headers.auth) });

const procesarRequest = pipe(
  agregarTimestamp,
  validarToken,
  parsearBody,
  enriquecerContexto
);
```

### Pipeline de validación con cortocircuito

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const validar = reglas => valor => {
  for (const regla of reglas) {
    const error = regla(valor);
    if (error) return { ok: false, error };
  }
  return { ok: true, valor };
};

const noVacio    = v => !v || v.length === 0 ? "Campo requerido" : null;
const minLen     = n => v => v.length < n ? `Mínimo ${n} caracteres` : null;
const esEmail    = v => !/@/.test(v) ? "Formato de email inválido" : null;

const validarEmail = validar([noVacio, minLen(5), esEmail]);

validarEmail("");               // { ok: false, error: "Campo requerido" }
validarEmail("ab");             // { ok: false, error: "Mínimo 5 caracteres" }
validarEmail("nodogs");         // { ok: false, error: "Formato de email inválido" }
validarEmail("user@site.com");  // { ok: true, valor: "user@site.com" }
```

## pipe asíncrono

Para pipelines donde alguna función devuelve una Promesa:

```js
const pipeAsync = (...fns) => x => fns.reduce(
  (promise, f) => promise.then(f),
  Promise.resolve(x)
);

const obtenerUsuario   = id => fetch(`/api/users/${id}`).then(r => r.json());
const enriquecerPerfil = usuario => fetch(`/api/profiles/${usuario.id}`).then(r => r.json())
  .then(perfil => ({ ...usuario, perfil }));
const formatearUI      = datos => ({
  nombre: datos.nombre,
  avatar: datos.perfil.avatar,
  bio: datos.perfil.bio
});

const obtenerPerfilUI = pipeAsync(obtenerUsuario, enriquecerPerfil, formatearUI);

obtenerPerfilUI(42).then(console.log);
```

## El operador pipeline `|>` (propuesta TC39)

La propuesta de pipeline operator (Stage 2 a fecha de ES2024) haría nativo en JavaScript el equivalente de `pipe`:

```js
// Propuesta — aún no es estándar
const resultado = valor
  |> trim(%)
  |> minusc(%)
  |> slug(%);

// Hoy equivale a
const resultado = pipe(trim, minusc, slug)(valor);
```

El símbolo `%` es el "topic reference" — el valor que fluye de izquierda a derecha. La propuesta no ha alcanzado Stage 3 y no debe usarse en producción sin transpilación (Babel plugin disponible).

## Cómo funciona por dentro

```js
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);
```

`fns.reduce` recorre el array de funciones de izquierda a derecha. El valor inicial `v = x` se convierte en el resultado de `f(v)` en cada iteración, propagando el resultado a través de la cadena. Al terminar, `v` contiene el valor final. Es la imagen especular de [[03 compose]] que usa `reduceRight`.

> [!tip] Buenas prácticas
> - Preferir `pipe` sobre `compose` en la mayoría de proyectos JavaScript — el orden izquierda-a-derecha coincide con el orden de lectura y con el patrón habitual de `método1().método2().método3()`.
> - Nombrar cada función intermedia con un verbo que describa la transformación: `normalizar`, `filtrar`, `formatear`. El pipeline se convierte así en documentación ejecutable.
> - Para pipelines async, usar `pipeAsync` explícitamente en lugar de mezclar Promesas y valores síncronos — mantiene la composición predecible.
> - Extraer el pipeline en una variable con nombre: `const procesarOrden = pipe(validar, calcularTotal, aplicarDescuento)` en lugar de usarlo inline — facilita el test unitario de la composición.

> [!warning] Errores comunes
> - **Funciones que retornan `undefined`:** si cualquier función en el pipeline no retorna explícitamente, el paso siguiente recibe `undefined` y el error puede propagarse silenciosamente varios pasos más adelante.
> - **Funciones no unarias en la cadena:** `pipe` pasa un único valor entre funciones. Si una función en el medio espera dos argumentos, aplicar currying o partial application primero.
> - **Mezclar sincrono y async sin `pipeAsync`:** `pipe(f, promiseFn, g)(x)` hace que `g` reciba un objeto `Promise`, no el valor resuelto. Usar `pipeAsync` o `async/await` explícitamente.
> - **Asumir que `pipe` existe en el estándar:** `pipe` es una utilidad que debe implementarse o importarse (Ramda, fp-ts, lodash/fp). No está en `Array.prototype` ni en el objeto global.

## Notas relacionadas

- [[index|Currying y Composición — Índice]]
- [[03 compose]]
- [[01 Currying]]
- [[02 Partial Application]]
