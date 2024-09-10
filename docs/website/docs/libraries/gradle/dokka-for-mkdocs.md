# Dokka for MkDocs (Experimental)

[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) is our preferred tool for writing documentation websites (including this one!). It is often used to write detailed tutorials or articles describing the overview of a project.

[Dokka](https://github.com/Kotlin/dokka) is Kotlin's documentation generator, similar to Javadoc and Doxygen. Since it extracts the documentation from the code, it is much more precise and exhaustive than handwritten articles. However, it can be overwhelming to users who do not know the project already.

These two tools are aimed for very different use-cases, but using both results in having two different documentation websites, which leads to repeated information, or users simply only reading one.

Dokka for MkDocs is a custom output format for Dokka, allowing it to embed the extracted documentation directly into a Material for MkDocs website.

<div class="grid cards" markdown>

- [Repository](https://gitlab.com/opensavvy/automation/dokka-material-mkdocs)
- Licensed under **Apache 2.0**
- Supports **all Kotlin platforms**

</div>
