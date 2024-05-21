# Overview

Many of our projects are built upon [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html), a technology for the [Kotlin language](https://kotlinlang.org/docs/kotlin-tour-welcome.html) that allows compiling the same code natively on multiple platforms.

**On the JVM**, Kotlin has access to the large [Java](https://kotlinlang.org/docs/server-overview.html) and [Android](https://kotlinlang.org/docs/android-overview.html) ecosystems and profits from great interoperability which makes 2-way access seamless.

**On the browser**, Kotlin can either be transpiled to JavaScript (with optional TypeScript type declaration), or compiled to [WebAssembly](https://kotlinlang.org/docs/wasm-overview.html). [Kotlin can call JavaScript code](https://kotlinlang.org/docs/js-interop.html), but [not all Kotlin constructs are exported from JavaScript](https://kotlinlang.org/docs/js-to-kotlin-interop.html).

**Via native binaries**, Kotlin can be compiled to [native binaries](https://kotlinlang.org/docs/native-overview.html) that can run directly on Windows, Linux, macOS, iOS…, while having access to system libraries. 

Unlike other multiplatform technologies, which are based on a common virtual machine that is ported to each platform, and thus requires changes when new platform-specific features are added, **Kotlin has direct access to each platform's API**.

## OpenSavvy Groundwork

Groundwork is a collection of Kotlin Multiplatform libraries offering various functionalities useful in a large array of projects;

<div class="grid cards" markdown>

- Reporting **progress information** through function calls • [Learn more](groundwork/pedestal-progress.md)
- Representing **failed and in-progress states** • [Learn more](groundwork/pedestal-progress.md)
- **Cache algorithms** that take advantage of platform-specific features, like `LocalStorage` • [Learn more](groundwork/pedestal-cache.md)
- Software architecture for **fullstack multiplatform and heterogeneous apps** • [Learn more](groundwork/backbone.md)
- **Magicless automated testing** • [Learn more](groundwork/prepared.md)
- **Typesafe fullstack Ktor endpoint declaration** and failure management • [Learn more](groundwork/spine.md)
- **Lazy configuration fetching with hot-reloading** • [Learn more](groundwork/indolent.md)

</div>

## Compose UI

Compose UI is a technology created by Google, and ported to some platforms by JetBrains. We create tools to use Compose more easily on more platforms.

<div class="grid cards" markdown>

- Writing multiplatform UIs that **adapt themselves to each platform** • [Learn more](ui/decouple.md)
- **Lazy layouts** for Compose HTML • [Learn more](ui/lazy-layouts.md)
- **Material3** for Compose HTML • [Learn more](ui/material3.md)

</div>

## Gradle plugins

The Kotlin ecosystem at large is built with [Gradle](https://gradle.org/). Through the years, there has been some functionality we have missed, so we created our own plugins.

<div class="grid cards" markdown>

- **Vite support** for Kotlin/JS • [Learn more](gradle/vite.md)
- **Transitive resources support** for Kotlin/JS • [Learn more](gradle/kjs-resources.md)

</div>
