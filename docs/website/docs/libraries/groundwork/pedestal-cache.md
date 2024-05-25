# Pedestal Cache

Have you ever visited a search page, clicked on one of the results, then immediately pressed the back button and had to wait for the search page to load its results again?

Caching is a way to store data globally but access it locally, in a way that ensures that data isn't lost just because the user navigated somewhere else, but without risking taking unlimited amount of memory.

Pedestal Cache is a collection of lightweight cache algorithms to store data anywhere, including in-memory and in platform-specific features (e.g. `LocalStorage`), as well as strategies for automatic data expiration.

Pedestal Cache also offers reactive implementations to ensure your application combines multiple requests to the same resource, and automatically notifies the application when a subscribed value changes.

<div class="grid cards" markdown>

- [Repository](https://gitlab.com/opensavvy/groundwork/pedestal)
- Licensed under **Apache 2.0**
- Supports [most Kotlin platforms](../supported-platforms.md)
- Usable with or without coroutines

</div>
