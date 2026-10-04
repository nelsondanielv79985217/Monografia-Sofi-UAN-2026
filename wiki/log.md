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

## [2026-10-04] ingest | Tres PDFs subidos a `main` el 2026-10-04

- Se fusionó `origin/main` en la rama de la wiki. Se extrajo el texto (`texto/`) y se escribieron las fichas de:
  - `perforacion-faringea-lesion-columna-cervical-trauma-cerrado` — Wang et al. (2025), Am J Case Rep. Reporte de caso. Única fuente específica para 3.3.1.
  - `infecciones-relacionadas-con-fracturas-trauma-maxilofacial-metaanalisis` — Van der Cruyssen et al. (2025), J Clin Med. Profilaxis antibiótica en fracturas maxilofaciales: para 5.15 solo como extrapolación declarada.
  - `videolaringoscopia-vs-tecnica-ciega-sonda-eco-transesofagica-pediatria` — Singh et al. (2025), Indian J Anaesth. Lesión faríngea iatrogénica pediátrica; no aporta a la monografía.
- Actualizados `conceptos/lesiones-aerodigestivas.md`, `conceptos/profilaxis-antibiotica.md`, el mapa de `sintesis/estructura-de-la-monografia.md` (regenerado: excluye las líneas donde una ficha declara que no aporta, y el ATLS en inglés), `sintesis/contradicciones-y-vacios.md` e `index.md`.

## [2026-10-04] lint | Revisión mecánica de la wiki y cobertura de la estructura

- Verificado: 0 enlaces rotos; 0 páginas huérfanas (las de `texto/` se enlazan desde el front matter de su ficha); marcadores de página de `texto/` correlativos en los 45 archivos; los 560 pares "(p. N; PDF M)" de las fichas coinciden con la etiqueta de página de `texto/`; transcripción de `Estructura del trabajo.docx` idéntica al DOCX.
- Citas textuales: muestreo automático de las comillas de las fichas contra `texto/`; las diferencias halladas se deben a saltos de línea y columnas intercaladas de la extracción, no a citas alteradas.
- Corregido: "pp." con página única → "p." (5 casos); página faltante en la cita "nearly 100%" de la ficha de *Trauma* (p. 528); `index.md` ya no dice que los PDF de ATLS en español y *Trauma* "llegan con el PR #1" (ya están en `main`); el mapa fuentes ↔ secciones ya no cuenta la guía APA ni Sampieri como sustento de secciones clínicas (1.1, 1.2, 4.1, 4.2, 4.6) y tiene una tabla aparte para Resumen, Objetivos, Conclusiones, Referencias y Anexos.
- Reportado sin corregir (requiere decisión o lectura): ver el informe del hilo de lint.

## [2026-10-04] query | Decisiones tras el lint y primeros borradores

- PR #2 y PR #3 fusionados en `main`.
- Metadatos bibliográficos completados desde Crossref (decisión del usuario) en 6 fichas: Ranwa (n.º 1), Noh y Choi (nombre de revista), Nowicki (n.º 1), Kose (n.º 3), Van der Cruyssen (n.º 4 y revista), Blitzer (revista; sin número). En Harris et al. (2012) Crossref da pp. 235–239 frente a 240–244 del PDF: se dejó `[INCIERTO]` y no se cambió. El resto no se pudo consultar: la red del entorno rechazó api.crossref.org.
- Nuevos borradores en `monografia/`: introducción, metodología de la revisión (con la búsqueda pendiente de la autora) y 1.7 Clasificación. Citas verificadas en `texto/`.
- Registradas en `CLAUDE.md` la estructura ampliada y la definición operativa de 7.4.

## [2026-10-04] lint | Metadatos bibliográficos desde Crossref (resto de las referencias)

- Consulta a api.crossref.org por DOI (y por título, autores y año para los cuatro artículos sin DOI en el PDF), autorizada por el usuario solo para datos bibliográficos. Cada ficha completada tiene la viñeta "Metadatos completados desde el registro Crossref…" y `last_updated` al día.
- Número de fascículo añadido: Qureshi (4), Kohan y Wirth (1), Mohamed (1), Bell (4), Siau (7), de Bakker (4), Munera (3), Zakaria (11), Ahmad y Singh (5), Olding (3), Kasbekar (1), Borsetto (9), Simpson (1), Teixeira (1), Krausz (1), Isaza-Restrepo (1), Loss (1).
- Nombre completo de la revista: Qureshi (*Operative Techniques in Otolaryngology–Head and Neck Surgery*), Harris, Cohnert, Siau, Munera, Biffl y Weigelt (*The American Journal of Surgery*).
- DOI añadido: Hadjizacharia, Weigelt, Fogelman y Biffl. Página final: Weigelt (622), Fogelman (596), Mandavia (225). Crossref confirma el volumen 91 de Fogelman (el PDF decía "9r").
- Sin número en Crossref (se retira el `[INCIERTO]` o se deja sin número): Abdou, Blitzer, Wang, Haran.
- Discrepancias Crossref ↔ PDF, marcadas `[INCIERTO]` y no sobrescritas: Harris et al. (pp. 235–239 frente a 240–244; no se usó el n.º 4), Laher et al. (pp. 17–20 frente a 165–168 "Online first"), Mandavia et al. (DOI del PDF 10.1067/mem.2000.104888 frente al de Crossref 10.1016/S0196-0644(00)70071-0) y Al-Thani et al. (Crossref pone a El-Menyar primero y solo la p. 154). Diferencias de año en línea frente a impreso (Mohamed, Siau, Kasbekar) se anotaron sin cambiar el año del PDF.
- Listas "Referencias de esta sección" de `monografia/` actualizadas con los mismos datos.

## [2026-10-04] lint | Decisiones del usuario sobre Mandavia y Laher

- Mandavia et al. (2000): la referencia usa el DOI vigente de Crossref, 10.1016/S0196-0644(00)70071-0, en lugar del DOI del PDF.
- Laher et al. (2023): la referencia usa las pp. 17–20 de Crossref (versión publicada). Las citas en texto con pp. 165–168 (ficha y conceptos) no se cambiaron porque vienen del PDF "Online first" y la equivalencia de páginas no está verificada; quedan marcadas `[INCIERTO]` en la ficha.

## [2026-10-04] query | Exportación de Mendeley (.ris) y lista maestra de referencias

- El usuario aportó una exportación RIS de Mendeley con 47 registros. Se cotejó por DOI y título con las fichas: 45 corresponden a fichas existentes y coinciden con los datos ya tomados de Crossref. No aportó datos nuevos, así que no se cambió ninguna ficha.
- Se descartaron como fuente los errores de importación de Mendeley: autores y revista de Hadjizacharia, "Lather" y "Trauma Surgery" en Laher, autores personales e ISBN truncado en ATLS, y SP = EP. El detalle está en `/mnt/project-files/.notes/inputs.md`.
- Singerman et al. (2025) está en Mendeley pero no tiene PDF en el repo: no se puede citar su contenido mientras no se suba.
- Nueva `monografia/referencias.md`: lista maestra APA 7 generada desde las fichas, con "y" entre autores y la mayúscula inicial tras dos puntos en los títulos. Excluye el ATLS en inglés y la guía APA.

## [2026-10-04] query | Capítulo 1 (1.1–1.6 y 1.8–1.10) y borrador 3 en Word

- Nuevo `monografia/cap1-generalidades-y-anatomia.md`, con las Tablas 2 (frecuencia y mortalidad del trauma penetrante) y 5 (zonas según Sperry et al., 2021, p. 522). Se verificaron 147 citas con página contra `texto/`. Se corrigieron nueve: el alcance de Olding (p. 133, marcado `[INCIERTO]`), el alcance de Ahmad y Singh (trauma cerrado de cuello, no laringe), los rangos de Siau (pp. 2126–2127), Al-Thani (pp. 154–155) y Sims y Reilly (pp. 5–6), "proyectil" en Loss, la nota de la Tabla 2, el "8 %" de Isaza-Restrepo y el género supuesto de Kose.
- Convención nueva: las citas textuales de fuentes en inglés se traducen al español (traducción propia), entre comillas y con página. Se aplicó en 1.7 y en la Introducción, incluidas dos traducciones de la autora corregidas (Steenburg, 2021, p. 353; Loss et al., 2025, p. 1). La guía APA del proyecto no trata las traducciones, así que la convención queda pendiente de validar con el director(a).
- Tablas renumeradas en orden de aparición: las de 1.7 pasan a ser las Tablas 3 y 4.
- Vacíos declarados: no hay datos de lesión de la glándula tiroides ni una descripción de fascias y espacios profundos del cuello. Laher et al. (2023) no se cita hasta resolver su paginación.
- Borrador 3 en Word, a partir del borrador 2 de la autora: `/mnt/project-files/monografia/word/Monografia-trauma-de-cuello-borrador-3.docx`.
