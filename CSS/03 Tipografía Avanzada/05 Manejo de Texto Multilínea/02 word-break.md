---
title: word-break — Reglas de corte de palabra
aliases:
  - word-break
tags:
  - css
  - api/propiedad
  - tipografia
propiedad: word-break
grupo: tipografia
valor_inicial: normal
hereda: true
animable: false
draft: false
---

# word-break

> [!definicion]
> `word-break` define **cómo se rompen las palabras** al final de la línea. A diferencia de [[01 overflow-wrap y word-wrap | `overflow-wrap`]] (que parte solo cuando no cabe), `word-break: break-all` parte **en cualquier carácter**, y `keep-all` impide romper palabras en idiomas CJK.

```css
.codigo { word-break: break-all; }   /* parte en cualquier carácter */
```

## Valores

| Valor | Efecto |
|-------|--------|
| `normal` | Reglas de ruptura por defecto (en espacios) |
| `break-all` | Rompe entre **cualquier** par de caracteres si hace falta |
| `keep-all` | **No** rompe palabras CJK (chino/japonés/coreano) |
| `break-word` | Como `overflow-wrap: break-word` (heredado, desaconsejado) |

## break-all: corte agresivo

> [!warning] break-all rompe aunque la palabra cupiera
> `break-all` parte el texto **en cualquier carácter** para llenar la línea, incluso partiendo palabras que cabrían enteras en la línea siguiente. Es **más agresivo** que `overflow-wrap: break-word` (que respeta las palabras mientras quepan):
> ```
> overflow-wrap: break-word →  "palabra | larga"      (respeta palabras)
> word-break: break-all      →  "pala | bra lar | ga"  (corta donde sea)
> ```
> `break-all` se usa para contenido sin estructura de palabras (códigos, hashes) o cuando se quiere un bloque justificado denso. Para texto de lectura normal, suele ser demasiado.

## keep-all: proteger palabras CJK

`keep-all` es lo contrario: en chino, japonés y coreano, el texto puede romperse entre cualquier carácter por defecto (no hay espacios entre palabras). `keep-all` lo **impide**, manteniendo juntas las "palabras" CJK, útil para nombres propios o términos que no deben partirse:

```css
.nombre-cjk { word-break: keep-all; }
```

## word-break vs. overflow-wrap

| Quiero… | Propiedad |
|---------|-----------|
| Partir palabras largas **solo si no caben** | `overflow-wrap: break-word` |
| Partir en **cualquier** carácter (agresivo) | `word-break: break-all` |
| **No** partir palabras CJK | `word-break: keep-all` |
| Partir con **guion** según idioma | `hyphens: auto` |

Para la mayoría de texto, `overflow-wrap` es la opción correcta; `word-break` es para casos específicos.

## Buenas prácticas

> [!tip] Recomendaciones
> - Para texto normal, prefiere [[01 overflow-wrap y word-wrap | `overflow-wrap: break-word`]] (respeta palabras).
> - `break-all` solo para códigos, hashes o bloques densos sin estructura de palabras.
> - `keep-all` para proteger palabras CJK que no deben partirse.
> - Combina con `hyphens` para cortes con guion más legibles en idiomas occidentales.

## Errores comunes

> [!warning] Trampas
> - **`break-all` en texto de lectura**: corta palabras de forma fea e innecesaria.
> - **Confundirlo con `overflow-wrap`**: uno es agresivo, el otro de último recurso.
> - **`break-word`** (valor heredado de `word-break`): usa `overflow-wrap` en su lugar.

## Notas relacionadas

- [[01 overflow-wrap y word-wrap]] — corte de último recurso, preferible para texto.
- [[03 hyphens]] — partir con guion según el idioma.
- [[02 word-break]] — (esta nota).
