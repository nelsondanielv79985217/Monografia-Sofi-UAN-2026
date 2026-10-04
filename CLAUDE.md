# CLAUDE.md — Schema de la LLM Wiki

Este archivo define cómo Claude debe mantener la wiki de este repositorio. Replica el esquema usado en la wiki del repositorio `programaGradoInnovacion` (rama `claude/llm-wiki-setup-nht7xw`), adaptado a una monografía médica. Se ajusta con el uso: si algo no funciona en la práctica, se discute con el usuario y se corrige aquí.

## Propósito

Este repo es la base bibliográfica de la monografía **"Trauma de cuello"** (UAN, 2026). Las fuentes son PDFs (artículos clínicos, el manual ATLS en inglés y en español, el libro *Trauma* 9.ª ed., la guía APA 7 y el libro de metodología de Sampieri) y un DOCX con la estructura del trabajo. En vez de releer los PDFs en cada sesión, se mantiene una wiki en Markdown (`/wiki`) con:

1. el **texto completo** de cada PDF con marcadores de página (para citar con página exacta sin abrir el PDF), y
2. fichas, conceptos, entidades y síntesis que acumulan la lectura entre sesiones.

## Reglas de fidelidad (prevalecen sobre todo lo demás)

- **Fuente exclusiva:** el contenido de la monografía sale solo de los documentos del repo. Nada de conocimiento general ni búsquedas externas como sustento.
- **Toda afirmación lleva cita con página.** En las fichas se escribe `(p. N)` usando la **paginación impresa** de la publicación; si no se puede determinar, se usa `(p. PDF N)` y se dice explícitamente.
- **Sin datos = decirlo.** Si algo no está en las fuentes: "esto no está en las fuentes del proyecto". Nunca completar, inferir ni redondear.
- **Regla de incertidumbre:** lo dudoso se marca `[INCIERTO: ...]` en el cuerpo y/o `status: incierto` en el front matter.
- **Opinión propia marcada como tal** (`[NOTA DE CLAUDE: ...]`), separada de lo que dice la fuente.
- **Cobertura parcial:** varios PDFs contienen solo la primera página de un artículo más largo. La ficha lo declara y no afirma nada sobre las páginas ausentes.

## Decisiones del usuario (2026-10-04)

- **Numeración:** los títulos de la monografía usan la numeración de `Estructura del trabajo.docx` (1.1, 1.2…), aunque la guía APA del repo sugiere no numerarlos.
- **ATLS:** se cita solo el manual en español (`fuentes/atls-soporte-vital-avanzado-trauma-manual-estudiantes.md`), con su paginación impresa. La ficha en inglés se conserva como consulta, pero no se cita ni va en las referencias.
- **PDFs parciales:** se cita únicamente lo que está en las páginas presentes, sin inferir el resto del artículo.
- ***Suture like a surgeon*:** eliminado del repo y de las referencias.

## Decisiones del usuario (2026-10-04, tras el lint)

- **Metadatos bibliográficos:** se permite completar *solo* datos de la referencia (número de fascículo, nombre completo de la revista, DOI) desde el registro del DOI (Crossref) u otra fuente bibliográfica externa. Nunca contenido. Cada ficha completada lo dice en una viñeta "Metadatos completados desde…".
- **Estructura ampliada:** se añaden INTRODUCCIÓN (antes de OBJETIVOS) y METODOLOGÍA DE LA REVISIÓN (después de OBJETIVOS), sin numerar, al resto de la numeración de `Estructura del trabajo.docx`.
- **1.7 Clasificación:** se redacta como síntesis de los sistemas que sí hay en las fuentes (mecanismo, zonas, presentación clínica, Denver, Schaefer-Fuhrman), cada uno con su cita.
- **7.4 Lesiones cervicales complejas — definición operativa de la autora:** lesión que afecta varias estructuras vitales del cuello a la vez, con alto riesgo de hemorragia masiva, asfixia o daño neurológico permanente. Es definición propia, no de una fuente: en la monografía va marcada como tal.

## Borradores de la monografía (`/monografia`)

Los textos redactados de la monografía van en `monografia/`, un archivo por sección (`00-introduccion.md`, `00-metodologia.md`, `cap1-1.7-clasificacion.md`…). Llevan front matter `type: borrador`. Cada cita debe estar verificada en `wiki/texto/`. Los vacíos se marcan `[PENDIENTE DE LA AUTORA: …]` y las opiniones propias `[NOTA DE LA AUTORA: …]`. En citas y referencias se usa "y" entre autores (ejemplos de la guía APA en español, pp. 40, 48–49), aunque las fichas usen "&".

## Arquitectura de 3 capas (no mezclar)

1. **Fuentes crudas** — los PDFs/DOCX de la raíz del repo. **Inmutables**: se leen, nunca se editan, mueven ni borran sin pedido explícito del usuario.
2. **Wiki** (`/wiki`) — páginas `.md` generadas y mantenidas por Claude.
3. **Schema** (este archivo).

## Estructura de `/wiki`

```
/wiki
  index.md       # catálogo de todas las páginas, por categoría
  log.md         # log cronológico append-only
  texto/         # TEXTO COMPLETO de cada PDF, página por página (capa de lectura/cita)
  fuentes/       # una ficha por PDF: referencia APA, cobertura, resumen, hallazgos con página
  conceptos/     # temas transversales que cruzan varias fuentes (con links a fuentes/)
  entidades/     # instituciones, registros o autores recurrentes
  sintesis/      # cruces que vale la pena conservar (ej. mapa fuentes ↔ capítulos)
```

Tipos de página (`type`): `texto`, `fuente`, `concepto`, `entidad`, `sintesis`, `metodo` (para Sampieri y la guía APA 7: herramientas, no tema clínico).

### Capa `texto/`

- Generada automáticamente: `pdftotext` (poppler) por página; OCR con tesseract **solo** en páginas sin texto o con codificación de fuente ilegible. Esas páginas llevan la nota `> [OCR: ...]` y se listan en `paginas_ocr` del front matter. Las páginas que tampoco dieron texto por OCR llevan `> [SIN TEXTO EXTRAÍBLE: ...]` y se listan en `paginas_sin_texto`.
- Cada página empieza con `<!-- pagina-pdf: N | etiqueta: X -->` y `## [Página PDF N de M · etiqueta de página del PDF: X]`. La etiqueta viene de los metadatos del PDF y **puede no coincidir** con la paginación impresa: para APA se verifica con el encabezado/pie visible en el texto.
- No se edita a mano (salvo para corregir un error de OCR verificado contra el PDF, registrándolo en `log.md`).
- Para buscar: `grep -n "término" wiki/texto/*.md` y luego leer el bloque de página correspondiente.

## Convenciones de nombres

- Un archivo por página: `wiki/<carpeta>/<slug>.md`. El mismo slug en `texto/` y `fuentes/` para un mismo PDF.
- Slug: ASCII kebab-case en español, sin tildes ni mayúsculas, derivado del título.
- No duplicar páginas; buscar antes de crear un concepto/entidad.
- Links: rutas relativas Markdown estándar, ej. `[Zonas del cuello](../conceptos/zonas-anatomicas-del-cuello.md)`.

## Front matter (obligatorio)

```yaml
---
title: "Título legible"
type: texto | fuente | concepto | entidad | sintesis | metodo
tags: [tag1, tag2]
fuente_pdf: "nombre-exacto-del-archivo.pdf"   # solo en texto/fuente/metodo
texto_completo: "../texto/<slug>.md"            # solo en fuente/metodo
status: pendiente-ingest | ingerido | incierto
last_updated: YYYY-MM-DD
---
```

## Plantilla de ficha (`fuentes/`)

1. **Referencia bibliográfica (APA 7)** — construida solo con datos visibles en el PDF; campos faltantes marcados `[INCIERTO: ...]`.
2. **Cobertura del PDF** — páginas incluidas vs. paginación del artículo; páginas OCR/sin texto.
3. **Resumen / tesis central.**
4. **Metodología** — diseño, población, periodo, lugar, n (con página).
5. **Hallazgos clave** — cifras y conclusiones textuales con `(p. N)`.
6. **Aporte a la monografía** — secciones de `Estructura del trabajo.docx` a las que sirve (ver `sintesis/estructura-de-la-monografia.md`).
7. **Conceptos y entidades** — links.
8. **Limitaciones / vacíos** — lo que la fuente no dice o no permite afirmar.

## `index.md` y `log.md`

- `index.md`: catálogo por categoría. Fila: `- [Título](ruta) — resumen de una línea. (status)`.
- `log.md`: **append-only**. Entrada: `## [YYYY-MM-DD] tipo | Nombre` + viñetas. `tipo` ∈ `{setup, ingest, query, lint}`.

## Flujo INGEST

Por defecto, **una fuente a la vez** con revisión del usuario antes de la siguiente; el procesamiento en lote solo por pedido explícito.

1. Si es un PDF nuevo: generar `texto/<slug>.md` con el mismo método (ver log del setup).
2. Leer el texto completo; escribir/actualizar la ficha en `fuentes/` siguiendo la plantilla.
3. Actualizar conceptos/entidades afectados, `index.md` y `log.md`.

## Flujo QUERY (redacción de la monografía)

1. Buscar en `index.md` y `sintesis/estructura-de-la-monografia.md` qué fuentes tocan la sección.
2. Leer fichas y conceptos; **verificar cada cifra/cita en `texto/`** (página exacta) antes de usarla.
3. Citar en APA 7 con página impresa. Si la sección no tiene sustento en las fuentes, decirlo.
4. Si la respuesta vale la pena conservar, proponer guardarla en `sintesis/`.

## Flujo LINT

Revisar: contradicciones entre páginas, `status: incierto` sin resolver, páginas huérfanas, conceptos mencionados sin página propia, citas sin página, y cifras de fichas que no coinciden con `texto/`. Reportar antes de corregir, salvo pedido de corrección automática.

## Evolución del schema

Este archivo se ajusta con el uso real. Si una convención no sirve, se dice y se corrige aquí.
