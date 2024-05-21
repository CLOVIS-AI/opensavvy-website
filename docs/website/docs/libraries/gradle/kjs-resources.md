# KJS Resources (Beta)

Usually, when using Gradle, static resources (files in `src/main/resources` or similar) are automatically included by all projects that depend on the one containing the resources. However, the Kotlin plugin doesn't do this for JavaScript targets.

This project contains two Gradle plugins: the `producer` plugin exposes static resources (including through publishing on MavenCentral or other repositories) and the `consumer` plugin extracts and adds them into a project.

<div class="grid cards" markdown>

- [Repository](https://gitlab.com/opensavvy/automation/kotlin-js-resources)
- Licensed under **Apache 2.0**

</div>
