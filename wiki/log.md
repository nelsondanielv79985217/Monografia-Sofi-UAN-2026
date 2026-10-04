# Log

Registro cronológico, append-only. No editar entradas pasadas — solo agregar al final.

## [2026-10-04] setup | Creación de la wiki y conversión de los PDFs a Markdown

- Por pedido del usuario ("procesa los archivos del repo de acuerdo a como lo hemos hecho con los otros repos" y "organiza la wiki del repo con la estructura misma que hemos empleado"), se replicó el esquema de la wiki de `programaGradoInnovacion` (rama `claude/llm-wiki-setup-nht7xw`): `CLAUDE.md` en la raíz, `wiki/index.md`, `wiki/log.md`, `fuentes/`, `conceptos/`, `entidades/`, `sintesis/`.
- Diferencia respecto del modelo: se agregó la capa `wiki/texto/` con el **texto completo** de cada PDF, página por página, para leer Markdown en vez de PDF y poder citar en APA 7 con página exacta.
- Método de extracción: `pdftotext` (poppler) por página; OCR con tesseract 5 (eng/spa) solo en páginas sin texto o con codificación de fuente ilegible (detectadas por proporción de palabras funcionales). Cada página lleva `<!-- pagina-pdf: N | etiqueta: X -->`; las páginas OCR llevan la nota `[OCR: ...]` y las vacías `[SIN TEXTO EXTRAÍBLE: ...]`.
- Resultado: 41 archivos en `texto/` para 42 PDFs. `Penetrating neck trauma_a comprehensive review.pdf` tiene texto idéntico a `Penetrating neck trauma a comprehensive review.pdf` (md5 distinto), así que comparte `texto/trauma-penetrante-cuello-revision-integral.md`.
- OCR completo: `Changing incidence and managment of penetrating neck injuries.pdf` (5/5 páginas; la capa de texto del PDF tenía fuentes con codificación ilegible). OCR parcial: ATLS (10 páginas de figuras/formularios), Sampieri (3), Suture like a surgeon (1). Páginas sin texto útil: ATLS 43, Sampieri 9, otras 3 (en blanco, portadas o figuras).
- **PDFs incompletos detectados:** 10 PDFs tienen una sola página de artículos más largos (p. ej. `Management of Penetrating Neck Injuries.pdf`, J Oral Maxillofac Surg 65:691-705, trae solo la p. 691). Sus fichas lo declaran.
- `Estructura del trabajo.docx` se transcribió en `sintesis/estructura-de-la-monografia.md`.
- Los PDFs y el DOCX originales no se modificaron.

## [2026-10-04] setup | Dos libros nuevos agregados desde otro hilo del proyecto

- Otro hilo del proyecto subió al repo (rama `claude/project-thread-cbouir`, PR #1, sin fusionar todavía) versiones reducidas de dos libros: `ATLS_Soporte_Vital_Avanzado_en_Trauma_Manual_del_Curso_para_Estudiantes_reducido.pdf` (443 págs.) y `Trauma_9th_Edition_Feliciano_reducido.pdf` (1441 págs.).
- El texto se extrajo de los originales sin comprimir (`/mnt/project-files/`), con la misma cantidad de páginas que las versiones reducidas (verificado con `pdfinfo`): `texto/atls-soporte-vital-avanzado-trauma-manual-estudiantes.md` (OCR: 1 pág.; sin texto: 40) y `texto/trauma-9th-edition-feliciano.md` (sin texto: 37). El front matter registra `extraido_de`.
