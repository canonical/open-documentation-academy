# Markup languages for documentation

## Presenter

[Vladimir Izmalkov](https://www.linkedin.com/in/vaizmalkov/)

---

<iframe width="560" height="315" src="https://www.youtube.com/embed/C0V0A8GGfAs?si=gQpDaRBp5fXw9Og7" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

---

Markup languages for documentation fall into two broad categories: lightweight markup languages and Extensible Markup Language (XML)-based formats. These tools serve the same goal but take different approaches to content creation and management.

## Lightweight markup languages

Lightweight markup languages are simple, text-based formats designed to make writing easier. They focus on human readability while providing sufficient structure for conversion into various output formats. There are three main lightweight markup languages used for documentation: Markdown, reStructuredText, and AsciiDoc.

### Markdown

Markdown has been adopted across countless documentation projects. The format processes through hundreds of different tools, from static site generators like [Jekyll](https://jekyllrb.com/) and [Hugo](https://gohugo.io/) to documentation platforms like [GitBook](https://www.gitbook.com/) and [Docusaurus](https://docusaurus.io/).

Markdown offers several advantages:

* Its simple syntax requires minimal learning time and allows writers to focus on content rather than formatting.
* The format is widely adopted across messaging platforms, content management systems (CMS), and development tools.
* Writers can embed HTML directly when they need advanced formatting options.
* The extensive ecosystem makes integration straightforward for most projects.

Markdown has some limitations that affect larger documentation projects. Some of these limitations are:

* The core specification provides limited features compared to other formats.
* Tables are problematic since they offer no advanced formatting options like merged cells or complex layouts.
* Markdown lacks built-in `include` functionality, which limits the ability to reuse content across multiple files without external tools or extensions.
* Advanced formatting often requires HTML embedding, making content harder to maintain.

Despite the limitations, Markdown now has different flavours to address some of these issues. Each flavour adds new features but also creates potential compatibility problems between implementations:

* GitHub Flavoured Markdown (GFM)
* MultiMarkdown
* Markdown Extra

### reStructuredText

reStructuredText processes primarily through [Sphinx](https://www.sphinx-doc.org/en/master/) and Docutils.

reStructuredText offers significant advantages for teams comfortable with Python ecosystems:

* The format supports extensive customisation through Python code.
* It maintains consistent standardisation, unlike Markdown's fragmented ecosystem.
* It offers rich features out of the box that exceed Markdown's basic capabilities.

However, reStructuredText has some limitations:

* It requires more verbose syntax and uses more special symbols than Markdown.
* Teams often find the syntax confusing when switching between Markdown and reStructuredText because they appear similar but use different conventions.
* Tool support remains more limited than Markdown's ecosystem.
* Advanced features often require Python programming knowledge.

### AsciiDoc

AsciiDoc is commonly processed using [Asciidoctor](https://asciidoctor.org/), a popular processor, and [Antora](https://antora.org/), a documentation site generator that builds on Asciidoctor.

AsciiDoc is the most feature-rich out-of-the-box solution among lightweight markup languages. The format has the following advantages:

* It provides extensive table formatting options, including merged cells, nested tables, and complex layouts.
* Its syntax is easy to read, particularly for code blocks and complex structures.
* It maintains strong standardisation through its official specification while allowing macro extensions through Antora.

AsciiDoc also has some limitations:

* It's less common compared to Markdown and reStructuredText.
* It has limited support.

## XML-based formats

XML was originally designed for data storage and transmission rather than human-readable writing. Due to this, documentation formats built on XML inherit its verbosity and complexity characteristics. These formats transform XML's data-focused structure into frameworks suitable for technical writing and content management.

They are best for large, complex, or compliance-heavy documentation, but can be costly and might lock you into a specific vendor.

### Darwin Information Typing Architecture (DITA)

DITA focuses on structured, topic-based content creation. The format’s modular approach supports content reuse across multiple documents. DITA also excels at conditional publishing, which lets teams include or exclude content based on build conditions. It's best suited for large-scale operations with complex documentation sets, where teams need to manage intricate content relationships.

### DocBook

DocBook targets traditional publishing workflows for books and articles. It optimises for structured content that flows from beginning to end in a linear fashion. DocBook supports cross-media publishing and offers extensive customisation options for teams that need professional publishing capabilities. The format works well for book-style documentation but lacks DITA's modular, topic-based approach.

### S1000D

S1000D serves specialised compliance requirements in regulated industries. The format follows rigid structural requirements and supports comprehensive compliance documentation needs. Teams in regulated environments often find S1000D mandatory rather than optional. The format includes its own version control systems (VCS) and data models for managing complex technical documentation that must meet specific industry formats.

## The docs-as-code approach

Docs-as-code remains the dominant approach in both lightweight markup languages and XML-based solutions. The docs-as-code process has three main components:

* **Source code:** This includes all the documentation content, images, and other necessary resources. All these files are stored in a repository, preferably using a VCS.
* **Builder:** This tool takes the source code and applies a design template to create the final, published documentation.
* **Published documentation:** This is the final result generated through the builder’s assembly.

A key aspect of this process is that when you change the source content, the published documentation also changes. However, the builder itself usually remains the same.

## Markup language choice at Canonical

Canonical supports its wide range of software and active community by choosing formats carefully. The company mainly uses Markdown because it's popular and easy for contributors to use.

Some Canonical projects use Discourse, a platform that natively supports Markdown, as the main repository for their docs. Other projects use Sphinx and [Read the Docs](https://about.readthedocs.com/). Sphinx is the primary tool for reStructuredText, but a plug-in called [MyST](https://mystmd.org/) allows the use of Markdown within the Sphinx framework. This plug-in allows them to leverage Markdown's simplicity while still using a powerful builder like Sphinx.
