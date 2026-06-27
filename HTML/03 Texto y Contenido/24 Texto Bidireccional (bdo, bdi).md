---
title: <bdo> y <bdi> — Control de dirección del texto
aliases:
  - bdo
  - bdi
  - bidirectional text
tags:
  - html
  - api/elemento
  - i18n
elemento: bdi
categoria: fraseo
rol_implicito: ninguno
vacio: false
draft: false
order: 24
---

# Texto Bidireccional (bdo, bdi)

> [!definicion]
> Ambos manejan texto **bidireccional** (idiomas que se escriben de derecha a izquierda, como árabe o hebreo, mezclados con texto de izquierda a derecha). `<bdo>` **fuerza** una dirección concreta; `<bdi>` **aísla** un fragmento para que su dirección no afecte al texto que lo rodea.

```html
<!-- bdo: forzar dirección -->
<p><bdo dir="rtl">esto se invierte</bdo></p>

<!-- bdi: aislar contenido de dirección desconocida -->
<p>Usuario: <bdi>إيان</bdi> (3 mensajes)</p>
```

## El problema: el algoritmo bidi

Los navegadores aplican un algoritmo Unicode (UBA) que decide la dirección de cada fragmento según sus caracteres. Cuando se **mezclan** idiomas de distinta dirección, ese algoritmo puede reordenar el texto de forma inesperada, sobre todo alrededor de signos de puntuación y números. `<bdo>` y `<bdi>` dan control sobre ese comportamiento.

## bdo: override de dirección

`<bdo dir="…">` **anula** el algoritmo y fuerza la dirección indicada para su contenido:

| `dir` | Efecto |
|-------|--------|
| `ltr` | Fuerza izquierda → derecha |
| `rtl` | Fuerza derecha → izquierda |

Es un caso de uso raro y específico (mostrar texto deliberadamente invertido, o corregir contenido mal codificado).

## bdi: aislamiento bidireccional

`<bdi>` es el más útil en la práctica. Aísla un fragmento de **dirección desconocida** —típicamente contenido generado por usuarios— para que no "contamine" la dirección del texto circundante:

```html
<!-- Sin bdi: un nombre en árabe puede desordenar el "(3 mensajes)" -->
<p>Usuario: إيان (3 mensajes)</p>

<!-- Con bdi: el nombre queda aislado, el resto se mantiene correcto -->
<p>Usuario: <bdi>إيان</bdi> (3 mensajes)</p>
```

> [!info] Cuándo es imprescindible bdi
> Siempre que insertes en tu interfaz **texto de origen desconocido** (nombres de usuario, comentarios, títulos externos) junto a texto fijo. Sin `<bdi>`, un nombre en árabe o hebreo puede reordenar visualmente los números, etiquetas o paréntesis que lo acompañan. Es una salvaguarda barata contra layouts rotos en aplicaciones multilingües.

## Buenas prácticas

> [!tip] Recomendaciones
> - Envuelve en `<bdi>` todo contenido de usuario o de origen desconocido que se mezcle con texto propio.
> - Reserva `<bdo>` para los casos puntuales en que necesitas forzar una dirección concreta.
> - Combina con el atributo [[04 Idioma y Dirección (lang, dir) | `dir`]] y `lang` para texto multilingüe estructural.

## Errores comunes

> [!warning] Trampas
> - **Olvidar `<bdi>`** en nombres de usuario multilingües: el síntoma son números o paréntesis que aparecen en el lado equivocado.
> - **Usar `<bdo>` donde se quería `<bdi>`**: `<bdo>` fuerza una dirección fija; `<bdi>` solo aísla sin imponer.

## Notas relacionadas

- [[04 Idioma y Dirección (lang, dir)]] — los atributos globales `lang` y `dir`.
- [[02 Elemento Raíz (html)]] — `dir` a nivel de documento.
