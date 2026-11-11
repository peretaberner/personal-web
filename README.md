# www.peretaberner.eu

Web personal de **Pere A. Taberner**.
Usa la plantilla [academic-website-template](https://github.com/sbryngelson/academic-website-template) (Jekyll, licencia MIT).

Cada cambio que guardes en la rama **`source`** vuelve a publicar la web automáticamente en 1–2 minutos (pestaña **Actions**).
La forma más sencilla de editar: abre el archivo en github.com, pulsa el lápiz ✏️, cambia el texto y pulsa **Commit changes**.

## Dónde está cada cosa

| Quiero cambiar…                                  | Archivo                                   |
|--------------------------------------------------|-------------------------------------------|
| Nombre, cargo, foto, enlaces, color, menú        | `_config.yml`                             |
| Portada (texto, etiquetas, premio destacado)     | `_pages/home.md`                          |
| Noticias de la portada                           | `_data/news.yml`                          |
| Research (artículos, working papers, capítulos…) | `assets/ref.bib`                          |
| Policy Reports                                   | `_data/policy_reports.yml`                |
| Resume: formación y experiencia                  | `_data/pi.yml`                            |
| Resume: workshops y seminarios                   | `_data/talks.yml`                         |
| Resume: premios y becas                          | `_data/awards.yml`                        |
| Dissemination / Links / Contact                  | `_pages/dissemination.md`, `links.md`, `contact.md` |
| Entradas del blog                                | `_posts/AAAA-MM-DD-titulo.md`             |
| Imágenes del blog                                | `images/uploads/`                         |
| PDFs propios (CV, papers)                        | `papers/`                                 |

## Añadir un trabajo a *Research*

Añade una entrada en `assets/ref.bib`. El **tipo** decide la sección:
`@article` → Peer-Reviewed · `@techreport` → Working Papers · `@incollection` → Book Chapters ·
`@inproceedings` → Conference Papers · `@unpublished` → Work in Progress.

```bibtex
@article{apellido2026titulo,
  selected = {true},          % opcional: aparece en la portada
  author = {Apellido, A. and Taberner, Pere A.},
  title = {Título del artículo},
  journal = {Nombre de la revista},
  volume = {1}, number = {2}, pages = {1--20},
  year = {2026},
  doi = {10.xxxx/xxxxx},
  url = {https://enlace-al-paper},
  blog = {https://enlace-a-un-post}, blogname = {Nada es Gratis},   % opcional
  award = {Premio…}, awardurl = {https://…},                        % opcional
  abstract = {Texto del resumen.},
}
```

Escribe tu nombre siempre como `Taberner, Pere A.` para que salga en negrita.
Si subes el PDF a `papers/`, añade `file = {nombre.pdf}` y aparecerá un botón **PDF**.

## Añadir un *Policy Report*

Copia un bloque de `_data/policy_reports.yml` y cambia los datos:

```yaml
- year: 2026
  title: "Título en inglés"
  title_local: "Títol en català"      # opcional
  client: "Ajuntament de …"
  role: "co-authorship"
  language: "Catalan"
  summary: "Resumen…"
  links:
    - label: "Read report"
      url: "https://…"
```

## Escribir una entrada del blog

Crea `_posts/2026-12-01-titulo-de-la-entrada.md`:

```markdown
---
layout: post
title: "Título de la entrada"
date: 2026-12-01
tags: [Stata, R]
---

Texto en Markdown. Imágenes: ![Descripción](/images/uploads/mi-imagen.png)
```

La dirección será `www.peretaberner.eu/titulo-de-la-entrada/`.

## Probar la web en tu ordenador (opcional)

Con Ruby instalado: `bundle install` y después `bundle exec jekyll serve`; abre <http://localhost:4000>.
