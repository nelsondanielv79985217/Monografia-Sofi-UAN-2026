# Log

Registro cronológico, append-only. No editar entradas pasadas — solo agregar al final.

## [2026-10-04] setup | Creación de la wiki y conversión de los PDFs a Markdown

- Por pedido del usuario ("procesa los archivos del repo de acuerdo a como lo hemos hecho con los otros repos" y "organiza la wiki del repo con la estructura misma que hemos empleado"), se replicó el esquema de la wiki de `programaGradoInnovacion` (rama `claude/llm-wiki-setup-nht7xw`): `CLAUDE.md` en la raíz, `wiki/index.md`, `wiki/log.md`, `fuentes/`, `conceptos/`, `entidades/`, `sintesis/`.
- Diferencia respecto del modelo: se agregó la capa `wiki/texto/` con el **texto completo** de cada PDF, página por página, para leer Markdown en vez de PDF y poder citar en APA 7 con página exacta.
- Método de extracción: `pdftotext` (poppler) por página; OCR con tesseract 5 (eng/spa) solo en páginas sin texto o con codificación de fuente ilegible (detectadas por proporción de palabras funcionales). Cada página lleva `<!-- pagina-pdf: N | etiqueta: X -->`; las páginas OCR llevan la nota `[OCR: ...]` y las vacías `[SIN TEXTO EXTRAÍBLE: ...]`.
- Resultado: 41 archivos en `texto/` para 42 PDFs. `Penetrating neck trauma_a comprehensive review.pdf` tiene texto idéntico a `Penetrating neck trauma a comprehensive review.pdf` (md5 distinto), así que comparte `texto/trauma-penetrante-cuello-revision-integral.md`.
- OCR completo: `Changing incidence and managment of penetrating neck injuries.pdf` (5/5 páginas; la capa de texto del PDF tenía fuentes con codificación ilegible). OCR parcial: ATLS (10 páginas de figuras/formularios), Sampieri (3), Suture like a surgeon (1). Páginas sin texto útil: ATLS 43, Sampieri 9, otras 3 (en blanco, portadas o figuras).
- **PDFs incompletos detectados:** varios PDFs tienen solo la primera página de artículos más largos (el detalle, verificado en las fichas, está en la entrada de ingest siguiente) (p. ej. `Management of Penetrating Neck Injuries.pdf`, J Oral Maxillofac Surg 65:691-705, trae solo la p. 691). Sus fichas lo declaran.
- `Estructura del trabajo.docx` se transcribió en `sintesis/estructura-de-la-monografia.md`.
- Los PDFs y el DOCX originales no se modificaron.

## [2026-10-04] setup | Dos libros nuevos agregados desde otro hilo del proyecto

- Otro hilo del proyecto subió al repo (rama `claude/project-thread-cbouir`, PR #1, sin fusionar todavía) versiones reducidas de dos libros: `ATLS_Soporte_Vital_Avanzado_en_Trauma_Manual_del_Curso_para_Estudiantes_reducido.pdf` (443 págs.) y `Trauma_9th_Edition_Feliciano_reducido.pdf` (1441 págs.).
- El texto se extrajo de los originales sin comprimir (`/mnt/project-files/`), con la misma cantidad de páginas que las versiones reducidas (verificado con `pdfinfo`): `texto/atls-soporte-vital-avanzado-trauma-manual-estudiantes.md` (OCR: 1 pág.; sin texto: 40) y `texto/trauma-9th-edition-feliciano.md` (sin texto: 37). El front matter registra `extraido_de`.

## [2026-10-04] ingest | Procesamiento en lote de las 43 fuentes (excepción al flujo "una por vez")

Por pedido explícito del usuario ("procesa los archivos del repo" y "organiza la wiki del repo con la estructura misma"), se escribieron en lote las fichas de todas las fuentes, siguiendo la plantilla de `CLAUDE.md`. Cada cifra de "Hallazgos clave" se verificó con grep contra `texto/`. Las cifras que no aparecían de forma literal se eliminaron, salvo cálculos propios marcados `[NOTA DE CLAUDE]`.

- Fichas: 3 libros (ATLS en inglés y español, *Trauma* 9.ª ed.), 37 artículos clínicos y 3 obras de método (`type: metodo`: Sampieri, guía APA 7, *Suture like a surgeon*).
- PDFs parciales (solo primera página o resumen), confirmados en las fichas: Bell 2007, Biffl 1997, Blitzer 2020, Fogelman 1956, Kohan 2014, Mandavia 2000, Munera 2009, Olding 2019, Qureshi 2020, Reyna-Sepúlveda 2022 y Weigelt 1987.
- Slug renombrado: `trauma-penetrante-cuello-y-otorrinolaringologo` → `trauma-penetrante-cuello-necesidad-exploracion-quirurgica` (en `texto/` y `fuentes/`), para que corresponda al título del artículo.
- Corrección: en la ficha de Cohnert et al., la página del shunt intraluminal pasó de (pp. 383, 385) a (pp. 384–385), verificada en `texto/`.

## [2026-10-04] ingest | Conceptos, entidades y síntesis

- Se unificaron los slugs de concepto y entidad propuestos en las fichas (sinónimos fusionados; los temas de una sola fuente quedaron como texto sin enlace). Se crearon 28 conceptos y 14 entidades, con cita de autor-año y página en cada afirmación y cifras verificadas en `texto/`.
- Síntesis: `sintesis/estructura-de-la-monografia.md` (mapa fuentes ↔ secciones, generado desde "Aporte a la monografía" de cada ficha) y `sintesis/contradicciones-y-vacios.md`.
- `index.md` reescrito con el catálogo completo.
- Lint: 0 enlaces rotos entre páginas de la wiki y 0 páginas huérfanas.

## [2026-10-04] setup | Decisiones del usuario y retiro de *Suture like a surgeon*

- Decisiones registradas en `CLAUDE.md`, `sintesis/contradicciones-y-vacios.md`, `conceptos/normas-apa-7.md`, `entidades/atls.md` y en las dos fichas ATLS:
  - se usa la numeración de `Estructura del trabajo.docx`;
  - se cita solo el ATLS en español;
  - de los PDFs parciales se cita únicamente lo presente.
- Por pedido del usuario se eliminaron `Suture like a surgeon.pdf` (raíz), `texto/suture-like-a-surgeon.md` y `fuentes/suture-like-a-surgeon.md`, junto con sus referencias en `index.md` y en el mapa de `sintesis/estructura-de-la-monografia.md` (conteos recalculados). Las entradas anteriores de este log que lo mencionan se conservan, porque el log es append-only.
