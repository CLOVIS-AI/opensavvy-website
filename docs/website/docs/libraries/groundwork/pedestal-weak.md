# Pedestal Weak

When writing complex systems and algorithms that must not overwhelm the system, we often need a way to communicate to the runtime that some memory is used for caching data, and may be freed if the system requires it. These mechanisms are platform-specific and can differ quite a bit in behavior.

Pedestal Weak brings a single, unified, multiplatform API to manage weak references, soft references, weak maps, and other similar data structures. The library provides access to platform-specific algorithms, but also provides its own written in pure Kotlin.

<div class="grid cards" markdown>

- [Repository](https://gitlab.com/opensavvy/groundwork/pedestal)
- Licensed under **Apache 2.0**
- Supports [most Kotlin platforms](../supported-platforms.md)
- Usable with or without coroutines

</div>
