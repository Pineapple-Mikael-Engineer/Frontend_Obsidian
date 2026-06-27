---
title: location.reload, assign y replace
aliases:
  - location.reload
  - location.assign
  - location.replace
  - navegación programática
tags:
  - javascript
  - api/objeto
  - browser
draft: false
order: 2
---

# `location.reload`, `assign` y `replace`

> [!definicion]
> `window.location` expone tres métodos para navegar y recargar la página de forma programática. La diferencia crítica entre ellos es su efecto sobre el historial del navegador: `assign()` añade una nueva entrada (el usuario puede volver atrás), `replace()` sobreescribe la entrada actual (no se puede volver), y `reload()` recarga la página actual en su lugar.

```js
location.reload();                     // recarga la página actual
location.assign("https://ejemplo.com/nueva"); // navega; añade al historial
location.replace("https://ejemplo.com/nueva"); // navega; reemplaza en historial
location.href = "https://ejemplo.com/nueva";  // equivalente a assign()
```

## Comparativa de métodos

| Método | Añade al historial | Cuándo usar |
|---|---|---|
| `location.reload()` | No (recarga la misma entrada) | Refrescar datos cuando no hay SPA |
| `location.assign(url)` | Sí | Navegar a otra página con retorno posible |
| `location.href = url` | Sí (igual que assign) | Forma abreviada de assign |
| `location.replace(url)` | No (sobreescribe la actual) | Redirecciones donde el usuario no debe volver |

## `location.reload()`

Recarga la página actual. Equivale al botón de refresco del navegador:

```js
location.reload(); // recarga normal — puede servir desde caché
```

`location.reload(true)` forzaba una recarga sin caché en browsers antiguos (ignorando el caché del browser), pero **este argumento está obsoleto y es ignorado por todos los browsers modernos**. Para forzar una recarga limpia, el usuario puede usar `Ctrl+Shift+R` (o `Cmd+Shift+R` en macOS). Desde JavaScript no hay forma estándar de forzar una recarga sin caché.

## `location.assign(url)`

Navega a la URL indicada y **añade una nueva entrada** al historial de la sesión. El usuario puede volver a la página anterior pulsando el botón de "atrás":

```js
// Después de una acción exitosa en un formulario
function irAConfirmacion(idPedido) {
  location.assign(`/pedidos/${idPedido}/confirmacion`);
}
// El usuario puede pulsar "atrás" y volver al formulario
```

## `location.replace(url)`

Navega a la URL indicada y **reemplaza la entrada actual** del historial. La página anterior desaparece del historial — el usuario no puede volver con el botón de "atrás":

```js
// Redirección de autenticación: no queremos que "atrás" regrese al login
function irAlDashboard() {
  location.replace("/dashboard");
}
```

## Receta: redirección tras login

El caso de uso más común de `replace` es la redirección de login → dashboard. Si el usuario ya tiene sesión activa y llega al login, se le redirige al dashboard reemplazando la entrada del historial, de modo que el botón "atrás" en el dashboard no devuelva al login:

```js
async function iniciarSesion(email, password) {
  const res = await fetch("/api/login", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ email, password })
  });

  if (res.ok) {
    // replace: el usuario no debe poder volver al formulario de login pulsando "atrás"
    location.replace("/dashboard");
  } else {
    mostrarError("Credenciales incorrectas");
  }
}
```

## Cuándo NO usar `location` para navegar

En una SPA (Single Page Application), navegar con `location.assign()` o `location.replace()` **descarta completamente el estado de la app y recarga todo el JS y HTML**. Para cambiar de vista dentro de una SPA sin recargar, la herramienta correcta es `history.pushState()`:

```js
// En una SPA: INCORRECTO — recarga la aplicación completa
location.href = "/perfil";

// En una SPA: CORRECTO — cambia la URL sin recargar
history.pushState({ vista: "perfil" }, "", "/perfil");
renderizarVista("perfil");
```

## Cómo funciona por dentro

Cuando se llama a `assign()` o se asigna a `location.href`, el motor del browser genera un evento de navegación equivalente al clic en un enlace: dispara `beforeunload` en la página actual (permitiendo mostrar un diálogo de confirmación si hay cambios sin guardar), añade la entrada al historial de la sesión y carga la nueva URL. `replace()` sigue el mismo flujo pero en lugar de añadir una entrada nueva, modifica la entrada actual de la lista del historial (cambia su URL y su estado). `reload()` es equivalente a navegar a `location.href` desde el mismo punto, respetando el estado `POST` si la página se cargó mediante un formulario POST (el browser mostrará un diálogo de confirmación en ese caso).

> [!tip]
> Para decidir entre `assign` y `replace`: si la página de origen tiene valor para el usuario (puede querer volver a ella), usa `assign`. Si la página de origen es transitoria (formulario ya enviado, página de splash, login con sesión ya activa), usa `replace` para mantener el historial limpio.

> [!warning]
> `location.reload()` no acepta argumentos útiles en browsers modernos. El argumento booleano `forceReload` fue eliminado de la especificación. Si la lógica depende de recargar sin caché (por ejemplo, después de un deploy), considera alternativas como añadir un parámetro de versión a la URL o invalidar el Service Worker.

## Notas relacionadas

- [[01 Propiedades de location|Propiedades de location]] — propiedades de `location` y la clase `URL`
- [[../04 History/02 pushState y replaceState|pushState y replaceState]] — cambiar la URL sin recargar en SPAs
- [[../04 History/03 Evento popstate|Evento popstate]] — cómo detectar la navegación hacia atrás/adelante
