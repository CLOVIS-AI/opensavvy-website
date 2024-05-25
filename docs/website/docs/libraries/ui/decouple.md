# Decouple (Alpha)

[Compose](https://www.jetbrains.com/lp/compose-multiplatform/) is a reactive framework created by Google for use within Android, and ported by JetBrains to other platforms.

However, rendering technologies are quite different on all platforms (e.g. Android vs the DOM), leading to differences in styling that make it impossible to have the exact same component definitions on all platforms.

Decouple aims to allow separation of concerns between the business UI layer, which describes the available screens and their contents, and the styling UI layer, which describes individually components or layouts. This allows keeping the styling UI layer platform-specific, while sharing the business UI layer across implementations.

<div class="grid cards" markdown>

- [Repository](https://gitlab.com/opensavvy/decouple)
- Licensed under **Apache 2.0**
- Supports [most Kotlin platforms](../supported-platforms.md)

</div>
