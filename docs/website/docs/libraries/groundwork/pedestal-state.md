# Pedestal State

Programming languages often focus on representing the happy path: the path followed by the process when everything is going well.
Failure management is often an after-thought.

But, **who wants a “Something went wrong” popup with no further information?** Some failures may automatically be recovered, and even those that cannot would benefit from a precise description for the user, possibly with internationalization. This level of feedback requires the ability to know in advance which failures may happen, and represent related metadata.

Based on the excellent [Arrow Typed Errors](https://arrow-kt.io/learn/typed-errors/working-with-typed-errors/) library, Pedestal State allows representing in-progress and failure states in a typesafe way throughout the codebase.

<div class="grid cards" markdown>

- [Repository](https://gitlab.com/opensavvy/groundwork/pedestal)
- Licensed under **Apache 2.0**
- Supports [most Kotlin platforms](../supported-platforms.md)
- Usable with or without coroutines

</div>
