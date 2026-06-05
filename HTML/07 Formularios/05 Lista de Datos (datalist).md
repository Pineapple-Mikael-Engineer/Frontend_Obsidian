---
title: <datalist> — Sugerencias de autocompletado
aliases:
  - datalist
tags:
  - html
  - api/elemento
  - formularios
elemento: datalist
categoria: interactivo
rol_implicito: listbox
vacio: false
draft: false
---

# Lista de Datos (datalist)

> [!definicion]
> `<datalist>` ofrece una **lista de sugerencias** para un campo de texto, sin restringir lo que el usuario puede escribir. Combina lo mejor de un campo libre y un desplegable: el usuario ve opciones, pero puede teclear un valor que no esté en la lista. Se asocia a un `<input>` mediante el atributo `list`.

```html
<label for="navegador">Navegador favorito</label>
<input type="text" id="navegador" name="navegador" list="navegadores" />
<datalist id="navegadores">
  <option value="Firefox"></option>
  <option value="Chrome"></option>
  <option value="Safari"></option>
</datalist>
```

## datalist vs. select

> [!info] La diferencia clave: ¿lista cerrada o abierta?
> | | `<datalist>` | `<select>` |
> |--|--------------|------------|
> | ¿Se puede escribir un valor fuera de la lista? | **Sí** | No |
> | Las opciones son… | **Sugerencias** | Las únicas válidas |
> | Filtrado al escribir | Sí (autocompletado) | No |
>
> Usa `<datalist>` cuando quieras **sugerir** sin obligar (ciudades frecuentes, pero admitir cualquiera). Usa [[01 Selección (select) | `<select>`]] cuando el valor **deba** ser uno de la lista.

## La conexión input ↔ datalist

El enlace se hace con el atributo `list` del `<input>` apuntando al `id` del `<datalist>`:

```html
<input list="sugerencias" />
<datalist id="sugerencias">
  <option value="…"></option>
</datalist>
```

El `<datalist>` en sí **no se muestra**: solo aporta las opciones. El navegador las presenta como autocompletado del campo.

## Compatible con varios tipos de input

`datalist` funciona no solo con `text`, sino con `email`, `url`, `number`, `range` (donde dibuja marcas/ticks), `date`, etc. En cada tipo, las opciones se ofrecen del modo adecuado.

## Buenas prácticas

> [!tip] Recomendaciones
> - Usa `<datalist>` para sugerir valores frecuentes sin restringir.
> - Si el valor debe ser obligatoriamente de una lista, usa `<select>`.
> - Las `<option>` de un datalist llevan el valor en `value` y suelen ir vacías (sin texto entre etiquetas).
> - Recuerda que el usuario puede ignorar las sugerencias: valida en servidor.

## Errores comunes

> [!warning] Trampas
> - **Esperar que restrinja** la entrada: no lo hace; el usuario puede escribir cualquier cosa.
> - **`list` apuntando a un `id` inexistente**: no aparecen sugerencias.
> - **Confundirlo con `<select>`**: son para casos distintos (sugerir vs. obligar).

## Notas relacionadas

- [[01 Selección (select)]] — cuando el valor debe ser de la lista.
- [[03 Tipos de input/01 input text]] — el campo que recibe las sugerencias.
- [[03 Tipos de input/18 input range]] — donde el datalist dibuja marcas.
