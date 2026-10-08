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

## [2026-10-05] ingest | Ejemplo de redacción sugerido (Monografía: Sepsis)

- La autora subió a `main` el archivo `Ejemplo de redaccion sugerido.pdf` (26 pp.), una monografía de sepsis de la UAN, para que sirva de modelo de redacción. Se trajo a la rama con un merge de `main`.
- Nuevos archivos:
  - `texto/ejemplo-redaccion-sugerido-monografia-sepsis.md`, generado con pdftotext por página. Las tablas en imagen (pp. 8, 13, 17, 22 y 26) no se transcribieron porque tesseract no está instalado.
  - `fuentes/ejemplo-redaccion-sugerido-monografia-sepsis.md` (type: metodo; no se cita).
  - `sintesis/guia-de-redaccion-de-la-monografia.md`, que separa lo que se adopta del modelo de lo que no se copia, con un ejemplo de reescritura del primer párrafo de 1.3.
- `CLAUDE.md`: la sección de borradores remite a la nueva guía de redacción.
- Pendiente de la autora: decidir entre texto justificado y alineado a la izquierda, y si se reescribe el capítulo 1 con este estilo.

## [2026-10-05] query | Borrador 4: capítulo 1 en síntesis y con extensión mínima

- La base fue el borrador 3 con los ajustes de la autora: 1.1 acortada, nota de la Tabla 2 retirada y tabla de contenido actualizada.
- El capítulo 1 se reescribió con la pauta de `sintesis/guia-de-redaccion-de-la-monografia.md`: párrafo temático, cita parentética agrupada, conectores de consecuencia y un numeral por idea. El cuerpo pasó de 5860 a 2525 palabras, sin contar las tablas.
- No se añadió ningún dato nuevo. Todas las cifras y páginas vienen del texto verificado en el borrador 3.
- Las citas de 1.1 se restituyeron. En la edición de la autora habían quedado sin página la de Simpson y la de Zakaria.
- Dejaron de citarse Olding, Kasbekar, Mohamed, Hadjizacharia, de Bakker y Noh, que volvieron al Anexo A. La lista de referencias se recalculó a partir de las citas del texto: quedan 25 obras.
- Se eliminó un título vacío bajo "Objetivo general".
- Archivo: `/mnt/project-files/monografia/word/Monografia-trauma-de-cuello-borrador-4.docx`.

## [2026-10-05] ingest | Quiroga-Centeno et al. (2022) y Trouboul y De Gracia (cap. 3, Manual de Cirugía del Trauma)

- La autora subió a `main` `2128_stamped.pdf` y `06.Capítulo 3.pdf`, que se trajeron con un merge.
- Se generaron `texto/` con pdftotext por página y las dos fichas.
- Quiroga-Centeno et al. (2022):
  - la página impresa es la página PDF + 619;
  - los metadatos se completaron desde Crossref; el fascículo no consta en el PDF ni en Crossref;
  - el registro es de trauma general y agrupa cabeza y cuello.
- Trouboul y De Gracia:
  - la página impresa es la página PDF + 24;
  - año, editor y editorial son inciertos y no se completaron desde la web, porque no hay un registro bibliográfico verificable.
- Ficha de Isaza-Restrepo et al. (2020): se añadieron los datos de la Tabla 1 (p. 3), transcritos de la imagen de la página. Incluye un 87.9 % de hombres y un 85 % de heridas por arma blanca.

## [2026-10-05] query | Borrador 5: 1.2 Epidemiología y 1.7 Clasificación

- Se partió del borrador 4 editado por la autora.
- En 1.2, la autora había escrito dos párrafos sin cita, con estilo de título. Se verificaron contra las fuentes y se reescribieron como texto con cita y página:
  - "8 al 10 %" en Bogotá: la fuente dice 8 % en un solo hospital (Isaza-Restrepo et al., 2020, p. 2).
  - "75 al 80 %" de hombres: es 87.9 % en el cuello en Bogotá y 78.1 % en el trauma general de Bucaramanga.
  - "16 a 35 años": esto no está en las fuentes del proyecto; se usó la edad media de 29.22 años.
  - "hasta 70 %" por arma blanca: no está en las fuentes; Isaza-Restrepo et al. dan un 85 %.
- En 1.7 se añadió Trouboul y De Gracia:
  - dos criterios de clasificación y cuatro grupos de estructuras (pp. 25–26);
  - zonas de Roon, con la incidencia de la II y la mortalidad de la I (pp. 25–26);
  - pacientes estables e inestables (p. 31);
  - vía aérea laríngea y traqueal (p. 33).
- Se restituyó la cita de la escala SLIC (Stahel et al., 2021, p. 552), que se había perdido en la edición, y se quitó una "x" suelta del título de 1.7.1.
- Archivo: `/mnt/project-files/monografia/word/Monografia-trauma-de-cuello-borrador-5.docx`.

## [2026-10-08] query | Revisión de nuevos archivos cargados en main
- El commit 945ccf9 de `main` subió a la raíz `Monografia-trauma-de-cuello-borrador-1.docx` a `borrador-4.docx`. No se cargaron fuentes nuevas (PDF ni RIS).
- Los borradores 2, 3 y 4 son idénticos (md5) a las versiones que la autora ya había enviado y que se procesaron en los borradores 3, 4 y 5.
- El borrador 1 es la versión editada por la autora (nombre, facultad, universidad e índice). Esos datos de portada ya están en los borradores posteriores.
- El borrador vigente sigue siendo `/mnt/project-files/monografia/word/Monografia-trauma-de-cuello-borrador-5.docx`. No se modificó la wiki.

## [2026-10-08] setup | Borradores movidos a `borradores/`
- A pedido de la autora, `Monografia-trauma-de-cuello-borrador-1.docx` a `borrador-4.docx` se movieron de la raíz a `borradores/`, sin modificar su contenido. Las fuentes (PDF, RIS y `Estructura del trabajo.docx`) siguen en la raíz.

## [2026-10-08] lint | Trouboul y Laher
- Trouboul y De Gracia: la autora confirmó que el título del manual es *Manual de cirugía del trauma*. El manual no aparece en Crossref. La referencia conserva "s. f." y marca la editorial como `[PENDIENTE DE LA AUTORA]`. La fecha de creación del PDF (2015) no se usó como año de publicación.
- Laher et al.: el pie de página de las pp. PDF 1–4 dice 17, 18, 19 y 20, el mismo rango que Crossref da para la versión publicada (61(3), 17–20). La cabecera "Online first" dice 165–168. Se presentó la decisión a la autora.
- Decisión de la autora (2026-10-08): Laher et al. se cita con las pp. 17–20 en la referencia y en el texto. Se convirtieron las citas de la ficha, de 14 conceptos y de una entidad (165→17, 166→18, 167→19, 168→20). Una conversión errónea de Noh y Choi (2022, p. 168) se detectó y se revirtió. Se quitaron los `[INCIERTO]` de paginación de la ficha de Laher y se actualizó `sintesis/contradicciones-y-vacios.md`.
