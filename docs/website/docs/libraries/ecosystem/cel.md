# CEL (Experimental)

[CEL](https://cel.dev/) is a fast and safe expression language: it can be used to allow users to create custom searches or filters. For example, it could be used in place of Jira's query language, or as a filter criteria for log files.

CEL is designed to be quick to execute and to forbid users from creating any malicious requests.

We have started creating our own CEL parser for Kotlin Multiplatform. Note that this project is one of our toy projects, and probably won't be usable in real projects for a long time.

<div class="grid cards" markdown>

- [Repository](https://gitlab.com/opensavvy/cel-kotlin)
- Licensed under **Apache 2.0**
- Supports [most Kotlin platforms](../supported-platforms.md)
- Usable with or without coroutines

</div>
