---
title: Efectos Secundarios — Aislar lo impuro en los bordes del sistema
aliases:
  - efectos secundarios
  - side effects
  - efecto secundario
tags:
  - javascript
  - api/concepto
  - funcional
draft: false
order: 3
---

# Efectos Secundarios

> [!definicion]
> Un **efecto secundario** es cualquier interacción observable de una función con el mundo exterior a su par entrada/salida: leer o escribir estado global, mutar un argumento recibido, producir I/O (console, red, DOM, filesystem, base de datos), lanzar excepciones o alterar el flujo del programa de forma no declarada en la firma. Los efectos secundarios son **inevitables** en aplicaciones reales — el objetivo de la programación funcional es **aislarlos y controlarlos**, no eliminarlos.

```js
// Función con cuatro efectos secundarios en seis líneas
function procesarPedido(pedido) {
  console.log("Procesando:", pedido.id);  // I/O
  pedido.estado = "procesado";            // mutación de argumento
  totalGlobal += pedido.importe;          // escritura a estado global
  fetch("/api/pedidos", { method: "POST", body: JSON.stringify(pedido) }); // red
}

// Versión con núcleo puro y efectos aislados
const calcularPedido = (pedido) => ({    // puro: transforma sin mutar
  ...pedido,
  estado: "procesado",
  procesadoEn: new Date().toISOString(),
});

// Los efectos quedan en el borde:
const pedidoProcesado = calcularPedido(pedido);
console.log("Procesando:", pedido.id);
await fetch("/api/pedidos", { method: "POST", body: JSON.stringify(pedidoProcesado) });
```

## Catálogo de efectos secundarios

| Tipo | Ejemplo | Problema |
|---|---|---|
| Mutación de argumento | `arr.push(x)`, `obj.x = 1` | El caller queda afectado |
| Escritura a estado global | `window.x = 1`, `modulo.estado = ...` | Acoplamiento invisible entre funciones |
| Lectura de estado mutable externo | `let x = global.contador` | No determinista |
| I/O de consola | `console.log`, `console.error` | No testeable unitariamente sin spy |
| I/O de red | `fetch`, `XMLHttpRequest` | Asíncrono, fallable, no determinista |
| DOM | `document.querySelector`, `.innerHTML =` | Dependencia del entorno |
| Lanzar excepciones | `throw new Error(...)` | Rompe el flujo normal |
| Timers | `setTimeout`, `setInterval` | Dependen del tiempo real |

## Por qué los efectos son inevitables

Una aplicación sin efectos secundarios no haría nada observable: no mostraría UI, no guardaría datos, no respondería al usuario. El objetivo no es eliminar los efectos sino **mover el código con efectos lo más lejos posible del núcleo de la lógica de negocio**.

## Técnica 1 — Functional Core, Imperative Shell

La arquitectura más usada en FP pragmático: el núcleo del sistema es puramente funcional (cálculos, transformaciones, validaciones), y los efectos quedan en la "cáscara" imperativa que llama al núcleo.

```js
// ── NÚCLEO PURO ──────────────────────────────────────────
const validarEmail = email => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);

const prepararRegistro = (datos, ahora) => {
  if (!validarEmail(datos.email)) return { ok: false, error: "Email inválido" };
  return { ok: true, usuario: { ...datos, creadoEn: ahora, activo: true } };
};

// ── CÁSCARA IMPURA ────────────────────────────────────────
async function registrar(datos) {
  const resultado = prepararRegistro(datos, Date.now()); // núcleo puro
  if (!resultado.ok) {
    console.error(resultado.error);                       // efecto: I/O
    return;
  }
  await db.usuarios.insertar(resultado.usuario);          // efecto: DB
  console.log("Usuario creado:", resultado.usuario.email); // efecto: I/O
}
```

`prepararRegistro` es testeable sin ningún mock: recibe datos y tiempo, retorna un objeto.

## Técnica 2 — Inyección de dependencias para efectos

Pasar los efectos como funciones hace que el núcleo sea testeable sustituyendo los efectos reales por versiones controladas:

```js
// La función recibe el efecto como parámetro, no lo invoca directamente
const notificarUsuario = (usuario, enviarEmail) => {
  if (!usuario.activo) return { enviado: false, motivo: "inactivo" };
  enviarEmail(usuario.email, "Bienvenido");
  return { enviado: true };
};

// En producción
notificarUsuario(usuario, emailService.enviar);

// En tests — sin red
const emailFake = jest.fn();
notificarUsuario({ activo: true, email: "x@x.com" }, emailFake);
expect(emailFake).toHaveBeenCalled();
```

## Técnica 3 — Empujar los efectos hacia los bordes del pipeline

En lugar de mezclar transformación y efecto, separar en fases: primero calcular todo, luego ejecutar los efectos al final.

```js
// Mezclado: difícil de testear
const procesarItems = items => {
  return items.map(item => {
    const resultado = calcular(item);
    console.log("Procesado:", item.id); // efecto en medio del pipeline
    return resultado;
  });
};

// Separado: el pipeline es puro; los logs van después
const procesarItems = items => {
  const resultados = items.map(calcular); // puro
  resultados.forEach((r, i) => console.log("Procesado:", items[i].id)); // efectos al final
  return resultados;
};
```

## Mutación de argumentos — el efecto secundario más frecuente

Modificar un objeto recibido como argumento afecta al caller, porque en JS los objetos se pasan por referencia (se copia la referencia, no el objeto):

```js
// Efecto secundario silencioso
function agregarPermiso(usuario, permiso) {
  usuario.permisos.push(permiso); // muta el array DENTRO del objeto del caller
  return usuario;
}

const u = { nombre: "Ana", permisos: ["leer"] };
agregarPermiso(u, "escribir");
console.log(u.permisos); // → ["leer", "escribir"]  — el caller quedó afectado

// Sin efecto secundario
function agregarPermiso(usuario, permiso) {
  return { ...usuario, permisos: [...usuario.permisos, permiso] };
}
```

## Excepciones como efectos secundarios

Lanzar un error rompe el flujo normal y puede propagarse hasta un handler inesperado. Una alternativa funcional es retornar un objeto `Result` que expresa éxito o fracaso explícitamente:

```js
// Con excepción: el efecto se propaga de forma invisible
const dividir = (a, b) => {
  if (b === 0) throw new Error("División por cero");
  return a / b;
};

// Con Result — el efecto queda contenido en el valor de retorno
const dividir = (a, b) => {
  if (b === 0) return { ok: false, error: "División por cero" };
  return { ok: true, valor: a / b };
};

const resultado = dividir(10, 0);
if (!resultado.ok) console.error(resultado.error);
```

El patrón `Result` / `Either` es la base de la gestión de errores en FP; en JS se implementa manualmente o con librerías como `neverthrow` o `effect`.

## Cómo funciona por dentro

Desde la perspectiva del motor JS (V8, SpiderMonkey), no existe la distinción entre función pura e impura — el runtime ejecuta todo. La distinción es semántica y architectural. El motor no puede optimizar automáticamente funciones puras de manera diferente a las impuras a nivel de JIT (aunque sí elimina código muerto en casos triviales). La responsabilidad de separar efectos recae enteramente en el diseño del código.

> [!tip] Buenas prácticas
> - Diseñar el núcleo de la aplicación (lógica de negocio, validaciones, cálculos) como un conjunto de funciones puras sin efectos.
> - Mantener los efectos (DB, red, DOM, logging) en la capa más externa posible — handlers de eventos, controllers, middlewares.
> - Hacer explícitos los efectos en los nombres de función cuando sea necesario: `guardarUsuario`, `notificarPorEmail`, `actualizarDOM` — el nombre indica que habrá un efecto.
> - En tests, preferir funciones puras al núcleo y reservar los mocks para la cáscara impura.

> [!warning] Errores comunes
> - **Mutar argumentos de tipo objeto o array**: es el bug de aliasing más frecuente en JS — el caller no espera que su dato cambie. Siempre crear una copia antes de modificar.
> - **`console.log` en medio de funciones de cálculo**: contamina el núcleo puro con I/O y dificulta la composición.
> - **Efectos dentro de `Array.map`**: si el callback de `map` tiene efectos secundarios, el comportamiento se mezcla con la transformación — usar `forEach` cuando el propósito sea el efecto, `map` cuando sea la transformación.
> - **Creer que `async`/`await` aísla los efectos**: la asincronía no hace pura a una función que escribe en DB o muta estado — solo cambia cuándo ocurre el efecto.

## Notas relacionadas

- [[01 Funciones Puras]] — la ausencia de efectos secundarios es la segunda condición de la pureza.
- [[02 Inmutabilidad]] — evitar la mutación de argumentos es la forma más directa de eliminar efectos.
- [[04 Composición de Funciones]] — las funciones sin efectos son las que se componen limpiamente.
